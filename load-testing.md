# Load testing

## Choosing

- Rate itself is the variable under test, and you want the best reports → **vegeta**
- Scenarios, auth flows, CI thresholds → **k6**
- Single binary, live TUI, recent build → **oha**
- gRPC, or you also need the echo server → **fortio**

For the max-throughput-of-a-box question, none of these are the right instrument — that
calls for a closed-loop generator.

## Why open loop

**Closed loop** tools take a concurrency level — each connection sends its next request
only after the previous response arrives. Offered load drops as the server slows, and the
requests that would have been slowest never get issued, so percentiles look healthy while
the system is failing. That's *coordinated omission*, and the error is not subtle:
[ScyllaDB measured](https://www.scylladb.com/2021/04/22/on-coordinated-omission/) a
reported p99 of 249µs against a corrected 665ms, and
[Tene's original talk](http://highscalability.com/blog/2015/10/5/your-load-generator-is-probably-lying-to-you-take-the-red-pi.html)
shows 10.9ms reported vs. 25s actual — roughly 2300x.

Closed loop answers "what is the max throughput of this box". Open loop answers "at
500 req/s, what does my p99 look like". The second is almost always the question you
have. Both are legitimate; just don't use one tool to answer the other's question.

Among the tools that get the second question right, there are two different mechanisms:

- **Constant applied load** — spawn workers without bound to hold the requested rate.
  Only **vegeta** does this. Load never sags, but the generator can exhaust its own
  memory against a slow target.
- **Fixed concurrency + latency correction** — record latency from the *intended* send
  time, so queueing delay appears in the percentiles, but offered load still drops when
  the target slows. This is **oha --latency-correction** and **fortio**'s catch-up
  pacing. Honest measurement, sagging pressure.

Reference: the [wrk2 README](https://github.com/giltene/wrk2) is still the canonical
explanation of coordinated omission.

## vegeta

https://github.com/tsenart/vegeta

```
brew install vegeta
echo "GET http://localhost:8080/health" | vegeta attack -rate=50/1s -duration=30s > results.bin
vegeta report results.bin
vegeta report -type='hist[0,2ms,10ms,50ms,200ms,1s]' results.bin
vegeta plot results.bin > plot.html
```

Rate is the primary input, and workers are spawned without bound to sustain it. `attack`
writes a binary result stream you re-report over as text, JSON, HDR histogram, or an
interactive HTML latency-over-time plot. Results from several machines merge into one
report: `vegeta report a.bin b.bin`. The HDR histogram support was done with Gil Tene
([#92](https://github.com/tsenart/vegeta/pull/92)).

The author's framing, which is the right way to hold the whole tool
([#520](https://github.com/tsenart/vegeta/issues/520)):

> vegeta isn't so much a benchmarking tool to hammer a system to its limits. It's a load
> test tool to understand your system behavior at different load points.

Targets file for multiple endpoints, with headers and bodies:

```
GET http://host/api/items
X-Api-Key: abc

POST http://host/api/items
Content-Type: application/json
@./body.json
```

Sharp edges:

- Unbounded workers means **memory blowup against a slow target** — one report of 64 GB
  consumed in 30–40s ([#344](https://github.com/tsenart/vegeta/issues/344)). By design.
- `-max-workers` bounds it, but reintroduces coordinated omission, per the author. Treat
  that flag as opting out of the reason you chose vegeta.
- Not perfectly immune: if requests sit in the TCP socket backlog rather than being
  accepted, vegeta is backpressured and behaves as a closed system
  ([#520](https://github.com/tsenart/vegeta/issues/520)).
- No CLI scripting. Dynamic requests mean importing `github.com/tsenart/vegeta/v12/lib`,
  which does get you `ConstantPacer`, `LinearPacer`, `SinePacer`, `StepPacer`.

## k6

https://github.com/grafana/k6

```
brew install k6
k6 run script.js
```

JS/TS scripting. Use the `constant-arrival-rate` executor for an open model — the default
`ramping-vus` is closed:

```js
export const options = {
  scenarios: {
    steady: {
      executor: 'constant-arrival-rate',
      rate: 500, timeUnit: '1s', duration: '5m',
      preAllocatedVUs: 100, maxVUs: 1000,
    },
  },
  thresholds: { http_req_duration: ['p(99)<250'] },
};
```

Failed thresholds exit non-zero, so it doubles as a CI gate. HTTP, gRPC, WebSocket, Kafka.
Choose it over vegeta when you need chained requests, auth token extraction, or multi-step
user journeys.

Caveat on rate fidelity: initializing a VU means standing up a JS runtime, so when k6 has
to allocate unplanned VUs mid-test the actual rate drifts. It warns
`Initializing an unplanned VU, this may affect test results`, and the fix is tuning
`preAllocatedVUs` by hand ([#3052](https://github.com/grafana/k6/issues/3052)). Vegeta's
workers are far cheaper to spawn, which is the concrete reason to prefer it when the rate
itself is the thing under test.

## oha

https://github.com/hatoo/oha

```
brew install oha
oha -z 30s -q 500 -c 50 --latency-correction --no-tui http://localhost:8080/health
```

Rust, with a live TUI while the run is in flight. `-q` sets the rate; without
`--latency-correction` you get plain closed-loop numbers.

Sharp edges:

- `--latency-correction` had a **confirmed bug fixed only in Feb 2026**
  ([#857](https://github.com/hatoo/oha/issues/857)) — use a recent build or the correction
  is not trustworthy.
- The fast internal path applies only when `--no-tui` is set **and `-q` is not**, so
  rate-limited mode is the slower path.
- Open: panic under load ([#883](https://github.com/hatoo/oha/issues/883)), throughput
  degrading over a long run ([#806](https://github.com/hatoo/oha/issues/806)).

## fortio

https://github.com/fortio/fortio

```
docker run -p 8080:8080 -p 8079:8079 fortio/fortio server
fortio load -qps 500 -t 30s -c 50 http://localhost:8080/health
```

Istio's load tool. gRPC support, a web UI, JSON result storage, and it doubles as an
echo/proxy server so you can sanity-check the harness against a known-fast target.

Not truly open loop: N threads paced with catch-up when they fall behind. `-nocatchup` and
`-uniform` exist to tune exactly that, and are worth understanding before quoting numbers.

