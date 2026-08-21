# Go Agentic Frameworks

Go-native alternatives to LangChain/LangGraph, focused on frameworks that support multiple AI providers.

As of August 2026, the serious contenders are **Eino**, **Google ADK Go** and **Genkit Go**. `langchaingo` is still relevant but comes behind those three for a new production agent system.

## Summary

| Framework | Rough equivalent | Graph/workflow orchestration | State / resume / HITL | Provider support | Observability | Verdict |
| --- | --- | --- | --- | --- | --- | --- |
| Eino | LangChain + LangGraph | Excellent | Excellent | Excellent | Good | Best general Go choice |
| Google ADK Go 2.0 | Mostly LangGraph + agents | Excellent | Excellent | Fair in Go | Good / OTel | Best graph runtime |
| Genkit Go | LangChain + app framework | Good | Very good | Good | Excellent / OTel | Best app/platform tooling |
| LangChainGo | LangChain | Basic/moderate | Limited vs LangGraph | Good | Basic | Familiar, but less compelling now |
| Galdor | Small agent runtime + LangSmith-ish observability | Moderate | Some | Moderate | OTel-native | Interesting but much smaller/newer |

## Eino

https://github.com/cloudwego/eino

Go-native framework from ByteDance's CloudWeGo project. Much more than a LangChain port: it has separate layers for components (`ChatModel`, `Tool`, `Retriever`, embeddings, templates), chains, DAGs/graphs, workflows, ReAct agents, multi-agent coordination, checkpoints, interrupt/resume with human-in-the-loop, and streaming throughout the graph.

The graph layer is conceptually close to LangGraph:

```text
             ┌──> search ───┐
input -> classify           merge -> model -> output
             └──> database ─┘
```

Supports conditional execution, asynchronous nodes, graphs with state, workflow composition, and wrapping a graph itself as an agent tool.

Provider support is one of its biggest advantages — OpenAI, Claude, Gemini, Ollama and others through [eino-ext](https://github.com/cloudwego/eino-ext).

Very active: [v0.9.14 on 2026-08-13](https://pkg.go.dev/github.com/cloudwego/eino), ~11.5k GitHub stars, but still pre-1.0.

Strengths:

- Very idiomatic Go
- Strong graph primitives
- Broad provider ecosystem
- Streaming is a first-class concept
- Good agent and multi-agent abstractions
- Checkpoint + interrupt/resume
- No tie to Google infrastructure

Weakness: maturity/API stability — no 1.0 API declared yet.

Choose Eino if you're using OpenAI/Anthropic or multiple providers and want something structurally similar to the combination of LangChain + LangGraph.

## Google ADK Go

https://github.com/google/adk-go

[ADK Go 2.0 went GA on 2026-06-30](https://adk.dev/2.0/) and introduced a graph-based workflow runtime, making it a genuine LangGraph competitor.

The runtime supports directed graphs, conditional routing, fan-out/fan-in, concurrent workers, per-node retries and timeouts, input/output schema validation, human-in-the-loop pause/resume, and agents, tools and ordinary Go functions as graph nodes.

Higher-level primitives:

```text
SequentialAgent
ParallelAgent
LoopAgent
multi-agent collaboration
```

Explicitly designed around idiomatic Go execution and streaming. See [Go getting started](https://adk.dev/get-started/go/).

The catch for a non-Gemini shop: the Go implementation's provider ecosystem lags Python. ADK exposes a generic `LLM` interface, so it's architecturally model-independent, but the Go SDK still lacks first-class OpenAI/Anthropic providers comparable to Python's — there are open issues requesting [OpenAI-compatible provider parity](https://github.com/google/adk-go/issues/596). Outside Gemini/Vertex you may need to write the adapter yourself.

Strengths:

- Probably the closest Go equivalent to LangGraph itself
- Real graph scheduler rather than merely chaining Go functions
- Strong typing
- Very good multi-agent model
- HITL
- Retry/timeout primitives
- Backed by Google, Apache 2.0, designed for production/cloud-native deployments

Weakness: provider support in Go.

Choose ADK Go if you're primarily on Gemini/Vertex, or happy implementing your own model adapter, and graph orchestration is central to the system.

## Genkit Go

https://genkit.dev/docs/go/overview/

Different in emphasis from ADK. Lower-level primitives:

```go
Generate()
Tool()
Prompt()
Flow()
Retrieve()
```

plus a higher-level [Agents API](https://genkit.dev/docs/go/agents/overview/) covering conversation history, typed state, persistent sessions, snapshots, streaming, interrupts, background execution, abort, resuming from snapshots, and multi-agent delegation.

[State](https://genkit.dev/docs/agents/state/) can be server-managed:

```text
server-managed
     ↓
session store
     ↓
session ID / snapshot ID
```

or client-managed, with the full state passed between requests. That's more control than many agent frameworks offer.

[Observability](https://genkit.dev/docs/go/local-observability/) is excellent — instrumented with OpenTelemetry, and traces/metrics can be exported to arbitrary OTel backends rather than being locked into Google's monitoring stack.

It isn't as graph-centric as LangGraph. In Genkit, [ordinary Go remains the orchestration language](https://genkit.dev/go/docs/flows):

```go
flow := genkit.DefineFlow(... func(ctx context.Context, input Input) (...) {
    a := doSomething(...)
    b := model.Generate(...)
    return doSomethingElse(a, b)
})
```

The newer Agents API is marked Beta and can still introduce minor-version breaking changes.

Choose Genkit if you're building an interactive AI product and value typed state, persistence, frontend integration, developer tooling and OTel more than explicit graph execution.

## LangChainGo

https://github.com/tmc/langchaingo

Community Go implementation inspired directly by LangChain: models, prompts, chains, agents, tools, memory, retrievers, vector stores, multiple providers. Substantial adoption at ~9.3k GitHub stars.

Architecture is traditional LangChain rather than a durable state-machine/graph runtime:

```text
prompt -> LLM -> parser

retriever -> prompt -> LLM

agent -> tools
```

The README states development is moving toward a more community-maintained model, and the latest stable [release](https://github.com/tmc/langchaingo/releases) shown is v0.1.14 from October 2025. Not abandoned, but Eino and ADK now have substantially more interesting orchestration architectures.

## Choosing

Ranking for a Go production system needing LangChain + LangGraph + agents/tools:

1. Eino
2. Google ADK Go 2.0
3. Genkit Go
4. LangChainGo

Decision point:

```text
                     Need Go agent framework
                             │
                  ┌──────────┴───────────┐
                  │                      │
          Provider-neutral?        Mostly Gemini?
                  │                      │
                 yes                    yes
                  │                      │
                Eino              Google ADK Go
                  │
         Need Genkit's app/
         frontend/OTel tooling?
                  │
                 yes
                  │
               Genkit
```

For a backend/service-oriented Go architecture, investigate Eino first. Its lower-level graph runtime can be used without forcing the whole application into an "agent framework" — the application stays normal Go, and Eino handles the specific portions where graph execution, LLM tool loops, checkpointing or streaming are useful.

If OpenTelemetry integration matters most, Genkit deserves more consideration; its native OTel approach is cleaner today than Eino's callback-oriented observability layer.

If you need durability across crashes for multi-hour/day workflows, consider an architecture that isn't purely an agent framework: Eino or a thin LLM/tool layer inside Temporal workflows. That gives stronger execution guarantees than most LangGraph-like runtimes while remaining very Go-native.
