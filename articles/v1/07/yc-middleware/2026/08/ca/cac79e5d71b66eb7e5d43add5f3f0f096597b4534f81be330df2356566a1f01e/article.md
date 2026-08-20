---
schema_version: "1.0.0"
document_id: "cac79e5d71b66eb7e5d43add5f3f0f096597b4534f81be330df2356566a1f01e"
company_key: "yc-middleware"
company: "Middleware"
source_id: "yc-middleware-news-import-fe54c9028845"
canonical_url: "https://middleware.io/blog/opentracing-vs-opentelemetry/"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T20:24:48.337916+00:00"
fetched_at: "2026-08-04T20:28:10.240209+00:00"
content_hash: "sha256:441a5203763d18e61a914b2a7d4fa1a32b34cf5594a31067be79845893fc892a"
---

# OpenTracing vs. OpenTelemetry: Key Differences and Migration Guide

If you searched for opentracing vs opentelemetry because your instrumentation still imports an OpenTracing library, you are looking at a dependency that CNCF archived years ago.


This guide lays out what actually changed between the two projects, why the switch matters for your traces today, and a concrete path to move your services onto OpenTelemetry without a rewrite-everything-at-once project.


> OpenTracing is deprecated and archived by the CNCF. OpenTelemetry is its actively maintained successor, unifying traces, metrics, and logs under one standard.


### Already know where you’re headed?


Start sending OTLP traces to Middleware today, no OpenTracing migration required first, with a 14-day free trial and unlimited ingestion.


#### Key takeaways


- OpenTracing was a vendor-neutral tracing API; it was archived by the CNCF Technical Oversight Committee in 2021, with the formal announcement following in early 2022.
- OpenTelemetry is the merger of OpenTracing and OpenCensus, formed in 2019, and it now covers traces, metrics, and logs under one specification instead of tracing alone.
- OpenCensus was archived in July 2023, which means all three original standards have converged into OpenTelemetry as the single CNCF-backed option.
- Most languages ship an OpenTracing Shim, so you can run OpenTracing-instrumented code against an OpenTelemetry backend while you migrate call sites incrementally.
- A migration is typically a per-service, per-language effort: swap the tracer initialization and span API calls, then point the exporter at an OTLP-compatible collector or backend.
- Modern observability platforms built around OpenTelemetry, including Middleware, let you correlate traces with logs and metrics instead of running separate tools for each signal.


## What is OpenTracing?


OpenTracing is a vendor-neutral specification for distributed tracing that defines a common API for creating spans, propagating context, and recording timing data across services. It let application code call a standard tracing API while swapping in different backend tracers, such as[Jaeger](https://middleware.io/blog/jaeger-tracing/) or Zipkin, without changing instrumentation code.


OpenTracing was accepted as a CNCF project in October 2016 and became one of the most widely adopted tracing standards during the early rise of microservices and distributed systems. Its scope was narrow by design: it addressed tracing only, not metrics or logs, which meant teams still needed separate tooling and separate instrumentation for the rest of their telemetry.


That narrow scope, combined with a parallel and overlapping project called OpenCensus, created exactly the kind of fragmentation CNCF wanted to resolve. In August 2021, OpenTracing moved to archived status at the CNCF, with OpenCensus following the same path in July 2023. Both projects were folded into OpenTelemetry, which the CNCF now treats as the direct successor for any team still running either one.


## What is OpenTelemetry?


OpenTelemetry is a unified observability framework that provides vendor-neutral APIs, SDKs, and tools for generating, collecting, and exporting traces, metrics, and logs from applications and infrastructure. It was created in May 2019 through the merger of OpenTracing and OpenCensus, and it is now a graduated CNCF observability project, sitting alongside Kubernetes and Prometheus in that tier.


Unlike OpenTracing, OpenTelemetry does not stop at tracing. It defines a common protocol, OTLP (OpenTelemetry Protocol), for shipping all three signal types to any compatible backend. It ships SDKs across more than a dozen languages, auto-instrumentation for common frameworks, and a Collector component that can receive, process, and route telemetry without touching application code. The opentelemetry vs opentracing conversation has one answer: OpenTelemetry absorbed OpenTracing, it is not a competing option.


For the full architecture, see our guide on[how OpenTelemetry works](https://middleware.io/blog/what-is-opentelemetry/) .


## OpenTracing vs OpenTelemetry: key differences


The opentracing vs opentelemetry difference is not a matter of picking a side. One project is archived and frozen; the other is the CNCF’s current, actively developed standard. Here is how they compare directly.


Feature OpenTracing OpenTelemetry


CNCF status Archived (August 2021) Graduated, actively maintained


Signal coverage Tracing only Traces, metrics, and logs


Data protocol No standard wire protocol OTLP (OpenTelemetry Protocol)


Auto-instrumentation Limited, mostly manual Broad, framework-level auto-instrumentation


Backend compatibility Backend-specific tracer implementations Any OTLP-compatible backend


Active development None, frozen API Ongoing, with SIGs per language


Migration support N/A Official Shim for gradual migration


Vendor lock-in risk Moderate, tied to chosen tracer Low, protocol and backend are decoupled


A few points from that table are worth pulling out on their own:


- OpenTelemetry replaces a tracing-only API with a single specification for traces, metrics, and logs, which removes the need to run three separate instrumentation libraries.
- Because OpenTracing is frozen, it will not gain support for new language versions, new context propagation formats, or new backend protocols going forward.
- OTLP decouples instrumentation from backend choice more thoroughly than OpenTracing’s tracer-specific implementations did, which reduces switching costs if you change observability vendors later.
- The OpenTracing Shim exists specifically so this comparison does not have to be all-or-nothing on day one of a migration.


## Why OpenTracing is deprecated and why it matters


OpenTracing is deprecated because the CNCF, along with the maintainers of both OpenTracing and OpenCensus, agreed that running two overlapping standards for the same problem space was holding the ecosystem back. The two projects merged into OpenTelemetry in 2019, and CNCF formally archived OpenTracing in 2021 once OpenTelemetry reached incubating status.


For teams still running OpenTracing instrumentation, this matters for a few concrete reasons:


- No further updates: security patches, new language support, and bug fixes stopped when the project archived.
- Shrinking ecosystem support: newer tracer backends and observability platforms are building against OTLP first, and OpenTracing compatibility is often an afterthought or absent entirely.
- Growing integration gaps: as libraries and frameworks move to native OpenTelemetry instrumentation, OpenTracing-based code increasingly needs manual shims to keep working.
- Compliance and audit friction: relying on an archived standard is a harder position to defend in architecture reviews than adopting the CNCF’s current graduated project.


### See the gap before it costs you


Every dashboard built on a frozen API accumulates blind spots as your stack changes underneath it. See what full trace, log, and metric correlation looks like in Middleware before you scope the migration.


## Benefits of moving to OpenTelemetry


Moving off OpenTracing closes a real gap: teams lose time chasing a metric that lives in a disconnected tool because tracing and metrics were never unified in the first place. The core opentelemetry benefits for a migrating team come down to this:


- **Unified observability** : one instrumentation layer produces traces, metrics, and logs instead of three separate toolchains, which is what most teams mean when they talk about opentelemetry unified observability.
- **Broader language and framework coverage** : SDKs and auto-instrumentation exist for far more languages and frameworks than OpenTracing ever supported.
- **Protocol-level portability** : OTLP means you can change observability backends without re-instrumenting your services, since the export format stays the same.
- **Active governance** : as a graduated CNCF project, OpenTelemetry has ongoing investment from major cloud providers and observability vendors, so gaps get filled rather than left open.
- **Native context propagation** : OpenTelemetry standardizes context propagation across HTTP, gRPC, and messaging systems, reducing the custom glue code many OpenTracing setups needed.
- **Collector-based flexibility** : the OpenTelemetry Collector can batch, filter, sample, and route telemetry centrally, moving that logic out of application code entirely.


## Migration path: from OpenTracing to OpenTelemetry


An opentelemetry migration guide for teams coming from OpenTracing usually follows the same six steps, whether you are migrating one service or an entire platform:


1. **Assess your current instrumentation.** Inventory every service using OpenTracing APIs, note which tracer backend each one talks to, and flag any custom span tags or baggage your teams depend on.
2. **Choose your OpenTelemetry SDKs.** Pick the SDK for each language in your stack and confirm which auto-instrumentation packages cover your existing frameworks.
3. **Decide: Shim or direct API.** Use the OpenTracing Shim if you need OpenTracing-instrumented code to keep working against an OpenTelemetry backend during a phased rollout. Use the direct OpenTelemetry API if you are rewriting instrumentation for a service outright.
4. **Update instrumentation code.** Replace OpenTracing tracer initialization and span creation calls with their OpenTelemetry equivalents, service by service.
5. **Configure exporters and the Collector.** Point each service at an OTLP exporter, and route data through the[OpenTelemetry Collector](https://middleware.io/blog/otel-collector-vs-agent/) if you need central sampling, filtering, or multi-backend fan-out.
6. **Validate before cutover.** Run both paths in parallel for a service or two, compare trace completeness and latency data against your existing dashboards, then retire the OpenTracing path once the new data matches expectations.


### Common migration pitfalls to avoid


- **Dropping baggage items silently.** OpenTracing’s baggage and OpenTelemetry’s baggage API are not always a direct match; verify custom baggage keys survive the switch before removing the old code path.
- **Running the Shim indefinitely.** The Shim is a bridge, not a destination; teams that never schedule the follow-up work to move to the native API end up maintaining two mental models permanently.
- **Skipping sampling parity checks.** If your OpenTracing setup used custom sampling logic, confirm the OpenTelemetry sampler configuration produces a comparable trace volume before comparing dashboards.
- **Assuming auto-instrumentation covers everything.** Auto-instrumentation handles common frameworks well, but custom spans your team added by hand under OpenTracing still need to be ported explicitly.


Here is what that change looks like in practice for a single service, using Node.js as an example:


```text
// Before: OpenTracing setup
const opentracing = require('opentracing');
const tracer = new SomeVendorTracer();
opentracing.initGlobalTracer(tracer);


const span = tracer.startSpan('process_order');
span.setTag('order.id', orderId);
span.finish();
```


```text
// After: OpenTelemetry setup
const { NodeSDK } = require('@opentelemetry/sdk-node');
const { OTLPTraceExporter } = require('@opentelemetry/exporter-trace-otlp-http');
const { getNodeAutoInstrumentations } = require('@opentelemetry/auto-instrumentations-node');


const sdk = new NodeSDK({
traceExporter: new OTLPTraceExporter({ url: 'https://your-otlp-endpoint/v1/traces' }),
instrumentations: [getNodeAutoInstrumentations()],
});
sdk.start();


const tracer = require('@opentelemetry/api').trace.getTracer('order-service');
const span = tracer.startSpan('process_order');
span.setAttribute('order.id', orderId);
span.end();
```


The shape of the code barely changes. What changes is the underlying protocol, the exporter target, and access to auto-instrumentation that removes the need to hand-write most of these spans in the first place. See[Middleware’s OpenTelemetry getting started guide](https://docs.middleware.io/open-telemetry/otel-getting-started) for the equivalent setup in Python and Java.


## Common migration patterns by language


### Java


Java teams typically migrate through the` opentelemetry-opentracing-shim` artifact, which wraps an OpenTelemetry` Tracer` behind the OpenTracing` Tracer` interface.


This lets existing` io.opentracing.Tracer` calls keep running unmodified while the underlying implementation reports through OTLP. Teams then peel off individual services to use the native OpenTelemetry Java agent, which auto-instruments common frameworks like Spring and Jakarta EE without code changes.[Middleware’s Java APM setup](https://docs.middleware.io/apm-configuration/java) covers the exact agent flags and OTLP configuration for this step.


### Node.js


Node.js services usually move directly to` @opentelemetry/sdk-node` combined with` @opentelemetry/auto-instrumentations-node` , since most Express, Fastify, and gRPC instrumentation is available out of the framework’s auto-instrumentation packages.


Where OpenTracing tags mapped one-to-one to custom business context, those become` span.setAttribute()` calls on the OpenTelemetry API.[Middleware’s Node.js APM setup](https://docs.middleware.io/apm-configuration/node-js) walks through the same tracker configuration shown above.


### Go


Go services tend to skip the shim entirely and move straight to` go.opentelemetry.io/otel` , since Go’s OpenTracing usage was often limited to a handful of manually instrumented call sites rather than broad auto-instrumentation.


Context propagation through` context.Context` maps closely enough between the two models that this is usually the fastest of the three migrations to complete.[Middleware’s Go APM setup](https://docs.middleware.io/apm-configuration/go) has the tracker initialization pattern for teams moving straight to the native API.


## When might you still see OpenTracing?


OpenTracing instrumentation still shows up in a few situations:


- Legacy services that have not been touched since before 2022 and still run against an OpenTracing-compatible tracer.
- Third-party libraries or internal packages that were instrumented years ago and have not been revisited.
- Teams running the OpenTracing Shim intentionally, as an interim step while they migrate other parts of the codebase.
- Vendor integrations that predate their provider’s native OpenTelemetry support.


None of these are a reason to standardize new work on OpenTracing. They are simply where the migration queue still has items in it.


## How modern observability platforms fit in


Instrumentation is only half the problem. Once traces, metrics, and logs are flowing through OpenTelemetry, you still need a backend to store that data, let engineers query it, and surface what actually matters during an incident.


A modern observability platform built around OpenTelemetry typically offers:


- Native OTLP ingestion, so you are not writing custom exporters for your specific backend.
- Unified storage and querying across logs, metrics, and traces, instead of three separate products with three separate query languages.
- Correlation across signals, so a slow trace can be linked directly to the log lines and metric spikes that explain it.
- [AI-assisted analysis](https://middleware.io/blog/identify-root-cause-analysis/) that helps surface root cause candidates from the correlated data, rather than requiring an engineer to manually cross-reference three dashboards.


Middleware is one example of an opentelemetry[observability](https://middleware.io/blog/observability/) platform built on this model: it ingests OpenTelemetry data natively, correlates logs, metrics, and traces in a single view, and supports on-premises and BYOC deployment for teams with data residency requirements.


It is not the only option, but it illustrates what “OpenTelemetry-native” looks like in practice for a team evaluating an open telemetry middleware and backend combination rather than building one from scratch. If you’re evaluating more than one option, this[comparison of OpenTelemetry-native platforms](https://middleware.io/blog/opentelemetry-tools/) breaks down the tradeoffs.


## Conclusion


OpenTracing is deprecated, OpenCensus is archived, and OpenTelemetry is the CNCF’s graduated, actively developed standard for[distributed tracing](https://middleware.io/blog/what-is-distributed-tracing/) standards across the industry. For teams still carrying OpenTracing instrumentation, the question is no longer whether to move, it is how to sequence the migration without disrupting production tracing along the way.


Start by migrating one service in one language using the Shim, confirm your trace data holds up against your current dashboards, then expand from there.


### Stop planning around a frozen standard


Start a pilot migration of one service to OpenTelemetry and send its traces straight into Middleware’s 14-day free trial with unlimited ingestion, no cost to see if OTLP-native correlation holds up against your current dashboards.


## FAQs


### What is the main difference between OpenTracing and OpenTelemetry?


OpenTracing was a tracing-only API that is now archived by the CNCF. OpenTelemetry is its successor, covering traces, metrics, and logs under one specification, and it is the actively maintained, graduated CNCF project that both OpenTracing and OpenCensus merged into.


### Is OpenTracing deprecated?


Yes. The CNCF Technical Oversight Committee archived OpenTracing in August 2021, with the formal transition announced in early 2022. Its API is frozen, and CNCF directs users toward OpenTelemetry as the replacement.


### Can I use OpenTracing and OpenTelemetry together?


Yes, temporarily. The OpenTracing Shim lets OpenTracing-instrumented code run against an OpenTelemetry backend, which is how most teams handle a phased, service-by-service migration instead of a single cutover.


### How do I migrate from OpenTracing to OpenTelemetry?


Assess your current instrumentation, choose OpenTelemetry SDKs per language, decide between the Shim and a direct rewrite for each service, update span and tracer calls, configure OTLP exporters, and validate trace data before retiring the OpenTracing path.


### Is OpenTelemetry better than OpenTracing?


OpenTelemetry is not just “better,” it is the only one of the two still under active development. OpenTracing’s API is frozen, while OpenTelemetry adds metrics and logs, broader language support, and ongoing CNCF governance.


### What happened to OpenCensus?


OpenCensus followed the same path as OpenTracing: it merged into OpenTelemetry in 2019 and was formally archived by the CNCF in July 2023. In an opentelemetry vs opencensus vs opentracing comparison, OpenTelemetry is the only surviving standard, since it absorbed both of the others.


### Which should I choose for a new project, OpenTracing or OpenTelemetry?


OpenTelemetry. OpenTracing is archived and receives no further development, so any new project should instrument directly against OpenTelemetry’s API and SDKs rather than adopting a frozen standard.


### How long does a typical OpenTracing to OpenTelemetry migration take?


For a single service with straightforward instrumentation, teams often complete the switch in a day or two using the direct API. Larger platforms with dozens of services usually run a phased migration over several weeks, using the OpenTracing Shim to keep older services working while newer ones move to native OpenTelemetry.


### Do I need to rewrite all my instrumentation at once?


No. The OpenTracing Shim exists specifically to avoid an all-at-once rewrite. Most teams migrate one service or one language at a time, validating trace data at each step, and only retire OpenTracing once every service has moved over.
