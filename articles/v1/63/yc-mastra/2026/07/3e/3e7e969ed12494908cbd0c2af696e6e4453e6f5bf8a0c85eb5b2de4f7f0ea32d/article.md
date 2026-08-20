---
schema_version: "1.0.0"
document_id: "3e7e969ed12494908cbd0c2af696e6e4453e6f5bf8a0c85eb5b2de4f7f0ea32d"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/trace-id"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-28T17:31:22.495262+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:84ef61ee0ed1510b70f9b0da0483fd1233d44d8cc0754dc05d2efbd3a5e8930f"
---

# Trace ID explained: what it is, how it works, and when to use it

You have a request that touches six services, and something broke at step four. Your logs show the error, but you can’t tell which user triggered it or what happened in the five steps before. A trace ID solves this by giving you one identifier that ties every operation in that request’s lifecycle into a single, queryable unit.


Distributed tracing has become a standard approach for debugging microservices, and the trace ID is its foundation. The[W3C Trace Context specification](https://www.w3.org/TR/trace-context-2/) formalizes how these identifiers propagate across service boundaries, giving your teams a shared standard for traceability across vendors and languages.


This guide explains what a trace ID is, how it compares with a correlation ID, and how you can implement both in your distributed systems.


## What is a trace ID?


A trace ID is a unique identifier assigned to a request when it first enters your system. It stays with that request as it moves through every service, queue, and database call in its path. Every operation the request triggers gets grouped under this single trace ID, giving you a complete map of what happened and how long each step took.


### Key characteristics of trace IDs


Your trace ID carries more weight than a simple tag. It anchors an entire tree of operations that you can query and visualize.


-


Hierarchical structure: traces contain spans, and spans have parent-child relationships that show you exactly which operation called which.


-


Timing data: each span records start time, duration, and end time, so you can pinpoint bottlenecks down to the millisecond.


-


Rich metadata: spans store parameters, return values, error codes, and custom attributes alongside the trace ID.


-


Standard format: most implementations follow OpenTelemetry conventions, producing a 32-character hex string (128 bits) as the trace ID.


### How trace IDs create a complete picture across services


You can think of a trace as a tree. The root span represents your entry point, and each child span represents a downstream call. A span ID identifies individual operations within the trace, while the trace ID groups them all together. Each span ID is unique within the trace, so you can isolate any single operation and see its parent, its children, and its duration.


A simplified trace for an API request looks like this:


```text
Trace ID: 4bf92f3577b34da6a3ce929d0e0e4736
├── Span: API Gateway (span ID: a1b2c3) - 120ms
│   ├── Span: Auth Service (span ID: d4e5f6) - 15ms
│   ├── Span: Order Service (span ID: g7h8i9) - 80ms
│   │   └── Span: Database Query (span ID: j0k1l2) - 60ms
│   └── Span: Notification Service (span ID: m3n4o5) - 20ms


```


*A trace hierarchy showing how parent and child spans represent the flow of a request across services*


This view tells you the full story. The database query inside the Order Service took 60ms of the total 120ms, and you can confirm that by comparing the span ID for the database call against its parent span ID in the Order Service. Without the trace ID linking these spans, you’d be left correlating timestamps across separate log files.


## What is a correlation ID?


You’ll often hear correlation IDs mentioned alongside trace IDs, and the overlap can be confusing. A correlation ID is a simpler concept: a unique identifier, usually a UUID, that you attach to a request and propagate through every service that handles it. Unlike a trace ID, it doesn’t carry structural or timing information on its own.


### Key characteristics of correlation IDs


Your correlation ID serves one purpose: connecting log lines across services for the same request.


-


Flat identifier: no hierarchy, no parent-child relationships, just one string that travels with the request.


-


Lightweight implementation: generate a UUID at the entry point, pass it in a header like X-Correlation-ID, and log it everywhere.


-


Technology-agnostic: works across any language, framework, or message broker without specialized instrumentation.


-


No formal standard: unlike trace IDs, correlation IDs don’t follow a universal specification.


### Practical example of correlation ID usage


You can implement correlation IDs in minutes. When a user submits a form, your API gateway generates a unique ID and attaches it to the request headers. Every downstream service reads and logs this value.


```text
import   {   randomUUID   }   from   '  crypto  '  ;
import   express   from   '  express  '  ;


const   app   =   express  ();


app.  use  ((req,   res,   next)   =>   {
const   correlationId   =   req.headers[  '  x-correlation-id  '  ]   ??   randomUUID  ();
req.correlationId   =   correlationId;
res.  setHeader  (  '  X-Correlation-ID  '  ,   correlationId);
next  ();
});
```


When something fails downstream, you search your logs for that single ID and see every service’s perspective on the request. The implementation overhead is minimal, and you can roll it out across your entire stack in a day.


## Trace ID vs correlation ID: key differences


You might wonder whether you need both. The short answer is that they solve overlapping but distinct problems.


**Feature** **Correlation ID** **Trace ID**


Structure Flat, single string Hierarchical tree of spans


Timing data None Duration per span


Implementation Header passing, minimal code Tracing SDK and instrumentation


Standard No formal spec W3C TraceContext, OpenTelemetry


Query capability Log search by ID Trace visualization, latency analysis


Overhead Negligible 1-3% request processing overhead


Correlation IDs give you log-level connectivity. Trace IDs give you observability: latency breakdowns, dependency mapping, and structured troubleshooting across distributed systems.


## When to use trace IDs


You should reach for trace IDs when your architecture has enough moving parts that log correlation alone can’t tell you the full story.


-


Complex microservices: if your request touches more than three or four services, the hierarchical span structure becomes essential for understanding flow.


-


Performance tuning: trace IDs let you identify bottlenecks by showing exactly which span consumed the most time, and each span ID lets you drill into the specific operation responsible.


-


Production debugging: when a failure cascades through multiple services, the span tree shows you the root cause and every downstream effect.


-


Service dependency mapping: traces reveal how your services actually communicate, which often differs from your architecture diagrams. This kind of traceability across service boundaries is difficult to achieve through logs alone.


## When to use correlation IDs


You should use correlation IDs when you need request tracking without the overhead of full tracing instrumentation.


-


Legacy systems: older services that can’t run modern tracing SDKs can still pass a header and log it.


-


Simple architectures: if your system is a monolith with a few external calls, full tracing adds complexity you don’t need.


-


Cross-boundary tracking: correlation IDs survive transitions between internal systems, third-party APIs, and async queues where tracing context might break.


-


Quick wins: you can implement correlation IDs across your entire stack in a day, giving you basic request tracking immediately.


## Implementing distributed tracing with trace IDs


Your implementation approach depends on your stack, but the core pattern is consistent: generate at the edge, propagate through headers, collect centrally.


### Generating and propagating trace IDs


When you instrument a service with OpenTelemetry, the SDK creates a trace ID for incoming requests that don’t already carry one and propagates the existing trace ID for requests that do. Each new span gets its own span ID, and the SDK tracks parent-child relationships automatically.


A basic OpenTelemetry setup in Node.js looks like this:


```text
import   {   NodeSDK   }   from   '  @opentelemetry/sdk-node  '  ;
import   {   OTLPTraceExporter   }   from   '  @opentelemetry/exporter-trace-otlp-http  '  ;
import   {   getNodeAutoInstrumentations   }   from   '  @opentelemetry/auto-instrumentations-node  '  ;


const   sdk   =   new   NodeSDK  ({
traceExporter:   new   OTLPTraceExporter  ({
url:   '  http://localhost:4318/v1/traces  '  ,
}),
instrumentations: [  getNodeAutoInstrumentations  ()],
serviceName:   '  order-service  '  ,
});


sdk.  start  ();
```


With auto-instrumentation, your HTTP handlers, database drivers, and outgoing requests all get wrapped in spans automatically. The trace ID propagates through the traceparent header on outgoing HTTP calls.


*A span tree viewed in a terminal, showing the hierarchical relationship between spans in a trace*


### Trace context standards: W3C Trace Context and OpenTelemetry


You should adopt W3C Trace Context as your propagation format. It defines a traceparent header containing the trace ID, parent span ID, and trace flags in a standard format:


```text
traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01


```


OpenTelemetry uses W3C Trace Context by default. Older tools like Jaeger and Zipkin have their own formats, but OpenTelemetry bridges between them. Standardizing on one propagation format prevents broken traces at service boundaries.


### How to find a trace ID in your system


You can extract a trace ID from several places depending on your setup.


-


Check response headers: many instrumented services return the trace ID in a header like traceparent or a custom X-Trace-ID.


-


Search your logs: if you’ve configured your logger to include trace context, grep for the trace ID field.


-


Query your tracing backend: tools like Jaeger and Zipkin let you search traces by service name, time range, tags, or duration.


-


Inspect OpenTelemetry collector output: the collector logs processed traces, including their trace IDs, to stdout in debug mode.


## How to implement correlation IDs in your system


Your correlation ID implementation needs two things: generation at the entry point and consistent propagation.


### Attaching correlation IDs to requests and logs


You want every log line in every service to include the correlation ID without requiring developers to pass it manually. In Node.js, AsyncLocalStorage handles this cleanly.


```text
import   {   AsyncLocalStorage   }   from   '  node:async_hooks  '  ;


const   correlationStore   =   new   AsyncLocalStorage  <  string  >();


// Middleware: set correlation ID for the request lifecycle
app.  use  ((req,   res,   next)   =>   {
const   id   =   req.headers[  '  x-correlation-id  '  ]   ??   randomUUID  ();
correlationStore.  run  (id,   ()   =>   next  ());
});


// Logger: automatically include correlation ID
function   log  (message  :   string  )   {
const   correlationId   =   correlationStore.  getStore  ();
console.  log  (JSON.  stringify  ({   correlationId,   message }));
}
```


This pattern ensures the correlation ID is available anywhere in your call stack without threading it through function arguments.


### Should correlation IDs be exposed to end users?


Your customer support team benefits when correlation IDs appear in response headers or error pages. When a user reports a problem, they can share the ID from their response, and your team can pull up every log entry for that request instantly.


A UUID in a response header is generally safe. The consideration shifts if your trace ID exposes service names or internal topology through your tracing backend, so evaluate what your identifiers reveal before surfacing them.


## Building trace-aware agents with Mastra


Your AI agents introduce a new observability challenge. An agent might call a model, invoke three tools, retry a failed call, and branch into a sub-workflow, all within a single user request. Without tracing, you have no visibility into what the agent decided, why it chose a particular tool, or where it spent tokens.


[Mastra](https://mastra.ai/) is an open-source TypeScript framework with observability for agent runs. Model calls, tool invocations, and workflow steps appear as spans with inputs, outputs, latency, and token usage. It can persist traces for inspection in Studio and export them to supported observability platforms, including OpenTelemetry-compatible backends.


*Tracing view showing application-level spans, token counts, and model operations*


Agents can return 200 OK while silently regressing, which makes structured tracing essential for catching failures that traditional monitoring misses.[Principles of Building AI Agents](https://mastra.ai/books/principles-of-building-ai-agents) presents observability as a practical way to manage both accuracy and token costs in production AI systems.


After you configure an observability storage backend and run Mastra Studio, you can inspect the JSON flowing into and out of model calls, review tool selections, and find spans with high latency or token usage. This gives you a trace-first debugging workflow during local development.


*Studio view for reviewing traced agent runs during development*


[Build your first traced agent with Mastra](https://mastra.ai/ai-agent-observability) .


## Using both together: trace IDs and correlation IDs in practice


You don’t have to choose one or the other. In most production systems, your trace ID can double as your correlation ID for log search, while the full trace provides the structural and timing data.


The practical approach works like this: your OpenTelemetry instrumentation generates and propagates the trace ID automatically. Your structured logger extracts the trace ID from the active span context and includes it in every log line.


```text
import   {   trace   }   from   '  @opentelemetry/api  '  ;


function   getTraceId  ()  :   string   {
return   trace.  getActiveSpan  ()?.  spanContext  ().traceId   ??   '  no-trace  '  ;
}


// Every log line includes the trace ID
logger.  info  (  '  Order processed  '  ,   {   traceId:   getTraceId  (),   orderId:   '  12345  '   });
```


Now your logs and traces are linked by the same identifier. You can filter logs by trace ID, then jump to the trace view for latency and dependency analysis.


## Observability and debugging with trace IDs


Your traces are only valuable if you can query them effectively and manage the data volume they produce.


### Querying traces and correlating logs by trace ID


You should structure your observability pipeline so that logs, traces, and metrics share the same trace ID as a join key. This lets you start from a log alert, pull the trace ID, view the full span tree, and correlate with infrastructure metrics in one flow.


Most tracing backends support queries by trace ID, service name, duration threshold, and custom tags. For troubleshooting, filter by error status and high latency to surface the traces that matter most, then use the shared trace ID to move between trace and log views.


### Performance considerations and sampling strategies


Your full tracing pipeline on every request generates significant data. Sampling lets you control costs without losing visibility.


Your two main options are head-based sampling (you decide at the trace start whether to record it) and tail-based sampling (you decide after the trace completes, keeping only interesting ones like errors or slow requests). Your production setup should sample 1-10% of normal traffic at the head while capturing 100% of error traces via tail-based rules.


**Strategy** **Decision point** **Best for** **Trade-off**


Head-based Trace start Predictable throughput May miss rare errors


Tail-based Trace completion Error and latency capture Higher memory usage


Hybrid (recommended) Both Balanced coverage More complex configuration


This keeps storage costs manageable while ensuring you never miss a failure.


### Handling trace context in serverless and async environments


You need special attention when propagating trace context through serverless functions, message queues, and event-driven architectures. Lambda invocations, SQS messages, and Kafka events don’t automatically carry the traceparent header.


Your solution is to embed trace context in your message payload or metadata. For serverless runtimes, OpenTelemetry-compatible exporters inject trace context into the invocation context object. For message queues, include the traceparent value in message headers or attributes so the consuming service can continue the trace and preserve the original trace ID across async boundaries. This gives you end-to-end traceability even when your request hops between synchronous HTTP calls and asynchronous event processing, and it prevents bottlenecks in your troubleshooting workflow caused by broken trace chains.


## Wrapping up


Trace IDs and correlation IDs solve the same core problem, connecting the dots across distributed systems, but at different levels of depth. Start with correlation IDs for quick log linking, adopt full tracing with OpenTelemetry when you need latency analysis and dependency mapping, and use both together for unified observability across your services. If your stack includes AI agents,[Mastra](https://mastra.ai/ai-agent-observability) extends the same trace-first visibility to model calls, tool runs, and workflow steps.
