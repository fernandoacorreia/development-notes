# Go Programming

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
