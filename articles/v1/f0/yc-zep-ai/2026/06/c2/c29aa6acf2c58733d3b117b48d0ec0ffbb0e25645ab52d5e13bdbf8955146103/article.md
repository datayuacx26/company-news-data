---
schema_version: "1.0.0"
document_id: "c29aa6acf2c58733d3b117b48d0ec0ffbb0e25645ab52d5e13bdbf8955146103"
company_key: "yc-zep-ai"
company: "Zep AI"
source_id: "yc-zep-ai-rss-0db2565bfdf1"
canonical_url: "https://blog.getzep.com/agentic-development-in-go/"
published_at: "2026-06-18T14:58:36+00:00"
first_seen_at: "2026-07-24T08:02:00.467869+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:a4b4a2cb01a941abc6c1770ee4c09dbd962174ed53b6f0e78fbcc49a86f7b035"
---

# Building Agents in Go Without a Framework

## Key takeaways


- A production agent is a long-running, concurrent, I/O-bound process. That shape fits Go's runtime: goroutines start at about 2KB each, channels carry coordination and streaming, and` context.Context` cancels a run across every library at once.
- Go ships as one static binary, which removes interpreter pinning and virtual-environment reconstruction from deployment. The difference shows most at the edge and in customer-managed environments.
- The Go framework field is real but young. ADK Go reached 1.0 in November 2025, alongside Genkit Go (Firebase) and Eino (ByteDance). Most teams skip frameworks, because the agent harness (the loop that drives the model) is about forty lines of Go.
- The major model vendors support Go. OpenAI, Anthropic, and Google ship native Go SDKs, and the official` openai-go` client also talks to any OpenAI-compatible endpoint (vLLM, Ollama, OpenRouter) through a base URL.
- Components attach to the loop one at a time: the official MCP Go SDK (v1.6.0) for tools, Zep's Go SDK for[agent memory](https://www.getzep.com/ai-agents/what-is-agent-memory/?ref=blog.getzep.com) with sub-200ms retrieval, and Hatchet or Temporal for durable execution.


**Go fits the agent runtime: the loop that calls models, runs tools, and stays alive for minutes or hours gets concurrency, cancellation, and single-binary deployment without a framework.**


Most writing about AI agents assumes Python or TypeScript: the frameworks, the tutorials, and the example repositories. In production, a growing number of teams write the agent itself in Go and put it behind a React / TypeScript front end.


Zep is one of them. Much of Zep is written in Go. The choice is pragmatic rather than dogmatic: our custom inference servers are written in Rust, and[Graphiti](https://github.com/getzep/graphiti?ref=blog.getzep.com) , our open source graph framework, is written in Python. Each component runs in the language that fits its workload. For the agent runtime, that language is Go.


## The shape of a production agent run


Once an agent has real users, its runtime has a consistent profile. Runs are long, lasting seconds to hours rather than the milliseconds of a web request. They are expensive: the agent drives work that would otherwise need a human operator, such as development environments, browser sessions, and document processing, so an abandoned run is wasted spend. They spend most of their wall-clock time waiting on a model, a tool, or a human. They run concurrently, many at once, each in a different state.


This is a different workload from the request-response service most backend tooling targets: many concurrent, long-lived, I/O-bound processes.


## How Go's model maps to that workload


Go was built for concurrent network services, and the agent workload is a concurrent network service with longer-lived units of work.


Goroutines are cheap. Each starts with about 2KB of stack, and the scheduler runs them across all CPU cores. Runs are I/O-bound, so one goroutine per run costs little and your own runtime is rarely the bottleneck. The ceiling is usually upstream provider rate limits and the memory each run holds (conversation history, open connections), not goroutine count. A CPU-bound moment, such as deserializing a large tool result, does not stall the whole process the way it can in a single-threaded runtime such as Node or Python.


Inside one process: each run is a goroutine costing about 2KB, mostly parked on I/O. The scheduler multiplexes the few runnable ones onto a handful of cores — so your runtime is rarely the bottleneck.


Channels carry coordination. An agent often needs to stream partial output to a user while waiting on the next model call, or pass control between sub-agents. Channels model this directly, and a run can be written as a stateless step that takes messages in and returns messages out, so any worker can pick up the next step.


` context.Context` cancels work across the whole call tree. When a user stops a run that has already cost ten dollars, you cancel one context and the in-flight request and every downstream tool stop. You still pay for the tokens already generated, but the expensive downstream work halts. The cancellation convention is more uniform in Go than in Python or Node, and` goleak` catches the libraries that ignore it.


One static binary is the deployment artifact. There is no interpreter version to pin and no virtual environment to reconstruct in the container. This matters most at the edge and in customer-managed deployments, where a single binary is far easier to ship than a Python environment.


The standard library covers most of what an agent does.` net/http` ,` encoding/json` , and` crypto/tls` are in the box and are high quality. Profiling is in the box too:` runtime/pprof` finds the goroutine and memory leaks that long-running, stateful processes tend to accumulate.


Go's small surface area and large standard library also mean less framework-specific context for a coding assistant to track. A coding agent does not need to know which framework version you are on.


## The framework landscape


The Go agent ecosystem is younger than Python's, and it has real options as of mid-2026.


[ADK Go](https://github.com/google/adk-go?ref=blog.getzep.com) , Google's Agent Development Kit, reached 1.0 in November 2025. It offers sequential, parallel, and loop agent primitives and native OpenTelemetry tracing. It is young relative to the Python ecosystem it follows, and shaped around Google's stack.


[Genkit Go](https://github.com/firebase/genkit/tree/main/go?ref=blog.getzep.com) , from Firebase, is a production-oriented framework with streaming, evaluation, and tracing built in.


[Eino](https://github.com/cloudwego/eino?ref=blog.getzep.com) , from ByteDance's CloudWeGo project, is a Go-native framework with composable building blocks (a` ChatModel` interface, tools, retrievers) and an agent layer for tool use and multi-agent coordination.


[LangChainGo](https://github.com/tmc/langchaingo?ref=blog.getzep.com) is a port of the Python library and trails the original in coverage and pace.


Most Go teams reach for none of these. Two reasons. The first is cultural: Go engineers tend to avoid frameworks and prefer the standard library plus small dependencies. The second is empirical. Anthropic's own finding, after reviewing many production deployments, is that the strongest agents use simple composable patterns rather than a framework. The agent loop is small enough to own.


The harness in full: call the model, run the requested tools in parallel, append the results, and loop until the model stops asking. Cancelling one


` context.Context


` halts every in-flight tool at once.


## The agent harness


The loop that drives the model is the agent's harness: the code that calls the model, dispatches the tools it requests, formats the results back, tracks the conversation state, and decides when to stop. The model chooses the next step; the harness runs it. A framework hands you a harness with defaults attached. Written by hand, it is about forty lines.


Going without a framework does not mean writing an HTTP client by hand. The official SDKs handle the API surface.` github.com/anthropics/anthropic-sdk-go` and` github.com/openai/openai-go` are both officially maintained and give you typed messages, streaming, and tool definitions. What they do not do is run the harness for you. You call the model, inspect the response for tool-use blocks, execute the tools, append the results, and call again. The control flow, the retries, and the durable boundary between steps are decisions you make explicitly.


Define a small interface for the model so the harness does not depend on any one vendor:


```text
type ToolCall struct {
ID    string
Name  string
Input json.RawMessage
}


type Completion struct {
Text      string
ToolCalls []ToolCall
}


type Model interface {
Complete(ctx context.Context, msgs []Message, tools []ToolSpec) (Completion, error)
}


type Tool interface {
Spec() ToolSpec
Run(ctx context.Context, input json.RawMessage) (string, error)
}


```


The loop calls the model, runs any requested tools, and stops when the model stops asking for them:


```text
func Run(ctx context.Context, m Model, tools map[string]Tool, msgs []Message) ([]Message, error) {
for {
out, err := m.Complete(ctx, msgs, specs(tools))
if err != nil {
return msgs, err
}
msgs = append(msgs, assistantMessage(out))


if len(out.ToolCalls) == 0 {
return msgs, nil // exit condition: the model is done
}


results := make([]Message, len(out.ToolCalls))
var wg sync.WaitGroup
for i, call := range out.ToolCalls {
wg.Add(1)
go func() {
defer wg.Done()
t, ok := tools[call.Name]
if !ok {
results[i] = toolError(call, "unknown tool")
return
}
res, err := t.Run(ctx, call.Input)
if err != nil {
results[i] = toolError(call, err.Error())
return
}
results[i] = toolResult(call, res)
}()
}
wg.Wait()
msgs = append(msgs, results...)
}
}


```


A few properties fall out of this shape. The function holds no state between calls; the conversation lives in the` msgs` slice that goes in and comes out, so a run can be persisted and resumed by a different worker. Tool calls in a single turn run in parallel, and a cancelled` ctx` propagates to every tool at once. Tool errors are returned to the model as results rather than killing the run, so the model can react to a failure.` Complete` returns the finished message; token streaming uses the same interface with a callback or a channel, left out here to keep the loop readable. This is the harness a framework wraps.


*Illustrative. Helpers like` specs` and` assistantMessage` , and the per-provider plumbing behind` Complete` , are elided; on Go 1.22+ the loop variable is captured per iteration.*


### Native SDKs from the model vendors


The major model vendors support Go. OpenAI, Anthropic, and Google all ship and maintain native Go SDKs —[openai-go](https://github.com/openai/openai-go?ref=blog.getzep.com) ,[anthropic-sdk-go](https://github.com/anthropics/anthropic-sdk-go?ref=blog.getzep.com) , and Google's[google.golang.org/genai](https://pkg.go.dev/google.golang.org/genai?ref=blog.getzep.com) — and smaller providers ship their own. The official` openai-go` client also takes a base URL through` option.WithBaseURL` , so it speaks to any OpenAI-compatible endpoint: a self-hosted[vLLM](https://github.com/vllm-project/vllm?ref=blog.getzep.com) or[Ollama](https://ollama.com/?ref=blog.getzep.com) server, a vendor endpoint, or a gateway such as[OpenRouter](https://openrouter.ai/?ref=blog.getzep.com) that fronts many providers behind one endpoint. Most models you would reach for already have a maintained Go client.


Back the` Model` interface from the harness with whichever SDK you use. Keeping each one behind the interface lets you swap models or route different steps to different providers without touching the loop. Multi-provider libraries such as Mozilla's[any-llm-go](https://github.com/mozilla-ai/any-llm-go?ref=blog.getzep.com) and Eino's` ChatModel` abstraction do the same job if you would rather not define the interface yourself.


## Workflow patterns


The loop above is the foundation. Most production systems are a small set of patterns built on top of it, and Anthropic's five workflow patterns translate cleanly to Go.


Prompt chaining is sequential model calls where each output feeds the next, with a check between steps. In Go this is ordinary straight-line code.


Routing classifies an input and sends it to a specialized handler. A first model call returns a category; a` switch` dispatches to the right prompt or tool set.


Parallelization runs independent subtasks at once and combines the results. This is the goroutine fan-out already in the loop, applied at the task level: fan out, wait, merge.` errgroup` adds bounded concurrency and first-error cancellation when you need them.


Orchestrator-workers has a coordinating model break a task into subtasks, dispatch them to workers, and synthesize the output. Goroutines and channels model the dispatch and collection directly.


Evaluator-optimizer runs a generate-then-critique loop: one model produces a result, another scores it, and the first revises until the score passes. This is the inner loop Anthropic's diagram labels "until tests pass," and it is a` for` loop with an exit condition.


Streaming is the other common need. Surfacing partial output to a user is a channel feeding an HTTP handler over server-sent events or a WebSocket, the kind of concurrent I/O the standard library handles directly.


Anthropic's five workflow patterns are arrangements of the same loop — each one a plain Go construct: straight-line code, a


` switch


` ,


` errgroup


` , goroutines and channels, a


` for


` loop.


## Components around the loop


A no-framework agent is the loop plus a set of components, each chosen on its own merits.


Model access comes from the native vendor SDKs (` anthropic-sdk-go` ,` openai-go` ,` google.golang.org/genai` ) behind the` Model` interface, or a multi-provider layer over them.


Tool and protocol support comes from the Model Context Protocol. The official[Go MCP SDK](https://github.com/modelcontextprotocol/go-sdk?ref=blog.getzep.com) , maintained in collaboration with Google, reached v1.6.0 in May 2026. It builds typed MCP servers and clients in a few lines and tracks the current spec. An agent can consume third-party tools without writing bespoke integration code for each one.


Agent memory is its own component, and the part most teams underestimate. Passing the full transcript back to the model on every turn stops working as soon as runs get long or span sessions.[Agent memory](https://www.getzep.com/ai-agents/what-is-agent-memory/?ref=blog.getzep.com) is the discipline of assembling the right context for each turn from past conversations and business data.


Here the Go ecosystem is thinner than the model-SDK story above. Most memory tooling is Python- or TypeScript-first: mem0 and the memory bundled into frameworks like Mastra have no Go SDK, and Graphiti, our own open-source library, is Python. Zep is one of the few memory vendors with a native[Go SDK](https://github.com/getzep/zep-go?ref=blog.getzep.com) , shipped because the pull came from customers operating at scale. Zep manages, governs, and serves agent memory via a[temporal knowledge graph](https://www.getzep.com/ai-agents/temporal-knowledge-graph/?ref=blog.getzep.com) that tracks how facts change over time and returns context in under 200ms. The alternative is to build and operate ingestion, retrieval, and fact invalidation yourself.


Durable execution keeps a run alive across crashes and deploys.[Hatchet](https://hatchet.run/?ref=blog.getzep.com) is a good fit here: a Go SDK over a Postgres-backed task queue that checkpoints each step and replays from the last checkpoint on retry. Long runs get exactly-once semantics, and the only infrastructure it needs is Postgres.[Temporal](https://temporal.io/?ref=blog.getzep.com) is the established alternative. It models each run as a workflow with full replay and a mature ecosystem, and the community[agent-sdk-go](https://github.com/agenticenv/agent-sdk-go?ref=blog.getzep.com) builds agents directly on it. Either way, a worker restart does not lose an hour of expensive work. Fitting an agent into a replay engine has constraints: model calls must run as non-deterministic activities rather than workflow code, and long conversation histories can hit payload-size limits. The stateless-step shape above makes the fit easier.


Observability rests on OpenTelemetry, which ADK Go emits natively and which the hand-written loop can emit with a span per model call and per tool. Tracing is how you debug an agent whose path differs on every run.


Testing is straightforward. The loop and the tools are ordinary Go, so they take ordinary table tests, and models write them well.


No framework: the loop is your code, and each component — model access, tools, memory, durable execution, observability — plugs in and swaps out on its own merits.


## Where Go is the wrong choice


Go is the right tool for the agent runtime, not for everything around it.


Third-party support trails Python and TypeScript. A new technique often appears in those ecosystems first, and you may port it yourself. Anything involving model training or classical machine learning belongs in Python; the libraries are there and not in Go. If you need the last increment of raw throughput, Rust or C++ will beat Go. The error handling is verbose.


The split in Zep's own stack reflects this. Go runs the services and the parts that need high concurrency and simple deployment. Rust runs the inference servers, where the performance ceiling matters. Python runs Graphiti, where the surrounding library ecosystem matters most. The agent runtime sits in the first category.


## Closing


At production scale, the hard parts of running an agent are concurrency, cancellation, and deployment. Go handles those in the runtime. The loop is about forty lines, the model vendors ship Go SDKs, and you add components one at a time. For the agent runtime, you usually do not need a framework.


## Sources


Go for agents:


- Hatchet — *Why Go is a good fit for agents* (June 2025):[hatchet.run/blog/go-agents](https://hatchet.run/blog/go-agents?ref=blog.getzep.com)
- humanlayer — *12-factor agents* :[github.com/humanlayer/12-factor-agents](https://github.com/humanlayer/12-factor-agents?ref=blog.getzep.com)


Patterns:


- Anthropic — *Building Effective Agents* :[anthropic.com/research/building-effective-agents](https://www.anthropic.com/research/building-effective-agents?ref=blog.getzep.com)


Frameworks and SDKs:


- ADK Go 1.0 — Google Developers Blog:[developers.googleblog.com/adk-go-10-arrives](https://developers.googleblog.com/adk-go-10-arrives/?ref=blog.getzep.com)
- Eino (CloudWeGo):[github.com/cloudwego/eino](https://github.com/cloudwego/eino?ref=blog.getzep.com)
- anthropic-sdk-go:[github.com/anthropics/anthropic-sdk-go](https://github.com/anthropics/anthropic-sdk-go?ref=blog.getzep.com)
- openai-go:[github.com/openai/openai-go](https://github.com/openai/openai-go?ref=blog.getzep.com)
- Model Context Protocol Go SDK (v1.6.0, May 2026):[github.com/modelcontextprotocol/go-sdk](https://github.com/modelcontextprotocol/go-sdk?ref=blog.getzep.com)


Components:


- Zep Go SDK:[github.com/getzep/zep-go](https://github.com/getzep/zep-go?ref=blog.getzep.com)
- Hatchet — durable tasks:[docs.hatchet.run/v1/durable-tasks](https://docs.hatchet.run/v1/durable-tasks?ref=blog.getzep.com)
- agent-sdk-go (durable agents on Temporal):[github.com/agenticenv/agent-sdk-go](https://github.com/agenticenv/agent-sdk-go?ref=blog.getzep.com)
