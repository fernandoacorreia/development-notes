# Go Programming

## Service architecture

- [How I write HTTP services in Go after 13 years](https://grafana.com/blog/2024/02/09/how-i-write-http-services-in-go-after-13-years/)

## gow

https://github.com/mitranim/gow

Watch mode for Go commands. Watch Go files and execute a command like "go run" or "go test".

## Skaffold

https://skaffold.dev/

Skaffold handles the workflow for building, pushing and deploying your application, allowing you to focus on what matters most: writing code.

With skaffold + gow you can make a change on your local code and on save it automatically updates the files on your remote kubernetes cluster and reruns go without doing a docker build or anything like that.

## sqlc

https://docs.sqlc.dev/en/stable/tutorials/getting-started-postgresql.html

https://docs.sqlc.dev/en/stable/guides/using-go-and-pgx.html

## Huma

https://huma.rocks/

REST API framework. A modern, simple, fast & flexible micro framework for building HTTP REST/RPC APIs in Golang backed by OpenAPI 3 and JSON Schema.

## testify

https://github.com/stretchr/testify

Assertions library. A toolkit with common assertions and mocks that plays nicely with the standard library.

## mockery

https://github.com/vektra/mockery

A mock code autogenerator for Go.

## Background jobs and task queues

### River

Fast and reliable background jobs in Go.
Atomic, transaction-safe, robust job queueing for Go applications. Backed by PostgreSQL and built to scale.

https://riverqueue.com/

## Concurrency

### WaitGroup

Examples:
- https://github.com/dacamp/take-home-challenge/blob/c27cabc5e05cf45ebb23a8112adbb4916d0de92d/peer/client.go#L108

### rheos

[rheos](https://github.com/dmksnnk/rheos) is a small lightweight library with helpers to build asynchronous pipelines.
It takes inspiration from [lo](https://github.com/samber/lo), but allows to do things concurrently.

### rill

https://github.com/destel/rill

Go concurrency with channel transformations: a toolkit for streaming, batching, pipelines, and error handling.

### Channels

- https://www.dolthub.com/blog/2024-06-21-channel-three-ways/
- https://go.dev/blog/pipelines

### orDone pattern

[video](https://www.youtube.com/watch?v=bnbEULxcX3o)

[code](https://docs.google.com/document/d/1x1D1JZ7AXsA6tjZPMZPWIWKyeuob-xo32mYQBe8Qs8w/edit?usp=sharing)

### Caching and thundering herd prevention

- [pocache](https://github.com/naughtygopher/pocache) - cache with preemptive optimistic caching strategy.
- [groupcache](https://github.com/golang/groupcache?tab=readme-ov-file#loading-process) - memcached replacement.
- [singleflight](https://pkg.go.dev/golang.org/x/sync/singleflight) - duplicate function call suppression mechanism.

### Caching libraries and frameworks
- https://github.com/eko/gocache
- https://github.com/go-redis/cache
- https://github.com/mgtv-tech/jetcache-go
- https://github.com/maypok86/otter

## Logging

Structured logging libraries, roughly in order of "default choice" to "reach for when you need it":

### log/slog

https://pkg.go.dev/log/slog

Standard library structured logging (Go 1.21+). No dependency needed, pluggable handlers (JSON/text), and most third-party libraries now offer slog adapters or implement `slog.Handler` directly. The Go team's own position ([blog post](https://go.dev/blog/slog)) is that slog isn't meant to beat zap/zerolog on speed — it's meant to be the common interoperable foundation. In practice it has become the default recommendation for new code, since the ecosystem gap (colorized dev output, context propagation, routing) has been filled by adapters:

- [tint](https://github.com/lmittmann/tint) - colorized handler for readable dev console output.
- [slog-multi](https://github.com/samber/slog-multi) - pipeline/fanout/routing middleware for handlers, part of samber's broader family of `slog-*` adapters (slog-zap, slog-echo, etc.)
- [slog-context](https://github.com/veqryn/slog-context) - context-based logger/attribute propagation (e.g. OTel trace IDs), a common gap vs zap/zerolog's context helpers.
- [devslog](https://github.com/golang-cz/devslog) - another dev-mode pretty handler.

### zerolog

https://github.com/rs/zerolog

Zero-allocation JSON logger with a fluent/chainable API. Actively maintained, benchmarks as the fastest of the mainstream options. Used by Adobe, Cisco, Cloudflare, MongoDB, Netflix, Red Hat, and others ([who uses zerolog](https://github.com/rs/zerolog/wiki/Who-uses-zerolog)).

### Uber zap

https://github.com/uber-go/zap

Near-zero-allocation structured/leveled logging, widely used in high-throughput services. API is more verbose (`zap.String(...)` field builders) but ships a friendlier `SugaredLogger`. API is stable/finalized (1.x), still actively releasing.

### phuslu/log

https://github.com/phuslu/log

Newer entrant marketed as the fastest structured logger, with benchmarks against slog/zap/zerolog in its README. Worth watching but far smaller adoption than zap/zerolog so far.

### charmbracelet/log

https://github.com/charmbracelet/log

Minimal, colorful logger from the Charm team (Bubble Tea/Lip Gloss), aimed at CLI/dev-friendly output rather than high-throughput backend services. Ships a slog handler and stdlib `log` adapter.

### logrus

https://github.com/sirupsen/logrus

One of the oldest and most widely adopted Go loggers (structured + pluggable hooks/formatters), but its README explicitly states it's in maintenance mode: "The project focuses on security, bug fixes, and performance improvements. New features are not planned" — and points users to zerolog/zap/apex for new projects.

### go-kit/log

https://github.com/go-kit/log

Minimal structured logging interface, common in go-kit-style microservices. More of a logging interface than a full-featured logger; effectively dormant (no meaningful activity since mid-2024) outside the go-kit ecosystem.

## Deployment

### Deploying Go programs in containers

If you run Go within containers, you should set GOMAXPROCS appropriately to avoid CPU throttling.

- [CPU Throttling for containerized Go applications explained](https://kanishk.io/posts/cpu-throttling-in-containerized-go-apps/)
- [automaxprocs](https://github.com/uber-go/automaxprocs): Automatically set GOMAXPROCS to match Linux container CPU quota.

### Embedding files in a Go program

[embed](https://pkg.go.dev/embed): provides access to files embedded in the running Go program.

## Performance

### Flame graphs

- https://tech.popdata.org/Flame-Graphs-Performance-Tuning-Made-Easy/
- https://www.benburwell.com/posts/flame-graphs-for-go-with-pprof/
- https://www.goodwith.tech/blog/go-pprof

The `perf` tool has inbuilt flamegraph generation code these days (well leaning on D3.js).
So `perf script report flamegraph` will convert a perf.data file into a flamegraph.html.
Similarly there is `perf script report gecko` to write out the firefox profiler's json format.

### GC memory limit

`GOMEMLIMIT`: see https://jvns.ca/blog/2024/09/27/some-go-web-dev-notes/

[automemlimit](https://github.com/KimMachineGun/automemlimit): Automatically set GOMEMLIMIT to match Linux cgroups(7) memory limit.
