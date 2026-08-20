---
schema_version: "1.0.0"
document_id: "efce4959e03a8803e1b69bfe60a062678ab765afc9079dcbce0221e407fbc0fb"
company_key: "yc-signoz"
company: "SigNoz"
source_id: "yc-signoz-rss-564a62b873f8"
canonical_url: "https://signoz.io/blog/dd-trace"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.602972+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:d2c6780631948376cb898e8b211405d8b5bea56f168380aeb57a664774d47709"
---

# What is dd-trace? And Why Teams Move to SigNoz for OpenTelemetry Tracing

# What is dd-trace? And Why Teams Move to SigNoz for OpenTelemetry Tracing


Last Updated: July 13, 2026


11 min read


**TL;DR**


- **SigNoz** : OpenTelemetry-native APM with a Datadog-style UI, a Datadog dashboard migrator, and Datadog Receiver support for staged migrations off` dd-trace` .
- **Datadog (` dd-trace` )** : Per-host APM charges and a proprietary wire protocol that ties your tracing code to the Datadog Agent, so migrating vendors later means rewriting the entire instrumentation.


---


` dd-trace` is the family of language-specific tracing libraries Datadog ships as part of Datadog APM. Each library runs in-process, records spans for incoming requests, database queries, and downstream calls, and ships the data to the Datadog Agent, which forwards it to Datadog's backend. The result is what you see in the Datadog UI as traces, service maps, and APM metrics.


Datadog ships a separate dd-trace package for each supported language (Python, Node.js, Go, Java, Ruby, .NET, and PHP), and the exact package names differ slightly across ecosystems. The library is fast to install and produces good traces, but it also pins your application to Datadog through a proprietary wire protocol,` DD_*` environment variables (` DD_SERVICE` ,` DD_ENV` ,` DD_TRACE_AGENT_URL` , and friends), and the Datadog Agent itself. Moving to any other observability backend later means uninstalling the library, installing a different SDK, and rewriting the instrumentation, not just changing where the data ships.


This article covers what` dd-trace` does, where it adds engineering friction, and how teams replace it with[OpenTelemetry](https://signoz.io/opentelemetry/) and SigNoz Cloud.


For the full step-by-step replacement path, our[Datadog APM migration guide](https://signoz.io/docs/migration/migrate-from-datadog/traces/) has the per-language commands for swapping` dd-trace` out for OpenTelemetry SDKs and pointing your traces at SigNoz.


## How dd-trace Works


*dd-trace data flow*


` dd-trace` is a per-language SDK that runs inside your application, captures spans, and ships them through the Datadog Agent over a proprietary HTTP API to Datadog's backend. There's a separate package per language:


Language Package


Python` ddtrace`


Node.js` dd-trace`


Go` dd-trace-go`


Java` dd-java-agent`


Ruby` datadog` gem


.NET` Datadog.Trace`


PHP` datadog-setup.php`


Newer support also exists for C++, Rust, Android, and iOS, but the shape is the same in every language and instrumentation runs in-process, talks to the Agent locally, and the Agent talks to Datadog.


The application never talks to Datadog's backend directly, and the destination is hard-coded by design. Whichever language SDK you use, changing where traces go means swapping the SDK out, not redirecting traffic to a different endpoint.


## How` dd-trace` Makes Tracing Harder Than It Should Be


Beyond the bill and the lock-in,` dd-trace` quietly costs engineering time because each language SDK was built around Datadog's pipeline rather than a shared open standard. The same product behaves differently across your stack, and the rules aren't always obvious until something breaks in production.


### Node.js initialization is fragile


The Node.js SDK uses runtime patching to attach itself to imports, which means` require('dd-trace').init()` has to run before any instrumented module is loaded.


Modern Node setups break that assumption in different ways: ESM imports hoist, webpack and esbuild change module resolution order, and Next.js and Nest.js bring their own bootstrapping. Fixes are scattered across separate doc pages and version-specific notes.


```text
// dd-trace: must be the first import, before anything else
const   tracer  =    require  (  'dd-trace'  )  .  init  (  )


// In ESM, before any other import:
// node --import 'data:text/javascript,import("dd-trace").then(d => d.default.init())' app.js


const   express  =    require  (  'express'  )
// ...


```


Compare with OpenTelemetry, which uses auto-instrumentation via` @opentelemetry/auto-instrumentations-node` , configured separately from the application code through a single` --require` flag. Application code stays untouched.


### Python has two entry points and you can only use one


The Python tracer has two valid entry points: the older` ddtrace-run` command that wraps the process, and the newer` import ddtrace.auto` import that bootstraps the tracer at module load.


Using both at the same time causes duplicate spans and confuses every dashboard that groups by trace ID.


### .NET and PHP need installer-level setup, not just packages


` Datadog.Trace` on NuGet is not enough to auto-instrument a .NET app. You also need either the machine-wide Datadog installer or the per-app` Datadog.Trace.Bundle` package, plus a set of environment variables on the target process. PHP needs the` ddtrace` extension installed through Datadog's` datadog-setup.php` installer before any of the Composer packages do anything.


Go now supports compile-time auto-instrumentation through Orchestrion, but teams on the older path still add per-package imports and a` tracer.Start()` call by hand.


### ` DD_*` env vars override code, often silently


Configuration precedence in most` dd-trace` SDKs puts the` DD_*` environment variables last, which means they override anything you set in code. The service name your application thinks it's emitting isn't always what shows up in Datadog. The exact rule varies per SDK, so checking the per-language docs before assuming env vars win is the only reliable answer.


These are small problems on their own. They add up to engineers debugging the tracer instead of the application, which is where OpenTelemetry's shared instrumentation model and CNCF-maintained docs save real time even though OTel is not free of complexity itself.


Already using dd-trace and looking for an OpenTelemetry-native alternative? Try SigNoz Cloud. Same Datadog-style UI, OTLP instead of proprietary protocols.


[Get Started - Free](https://signoz.io/teams/)


## dd-trace vs OpenTelemetry


*dd-trace vs OpenTelemetry*


Every difference below clearly shows how` dd-trace` is built for Datadog's pipeline, while OpenTelemetry is built as an open standard that any backend can implement.


Aspect` dd-trace` OpenTelemetry


**Maintained by** Datadog The CNCF (Cloud Native Computing Foundation)


**Wire protocol** Datadog Agent protocol (proprietary HTTP API)[OTLP (open standard)](https://signoz.io/blog/what-is-otlp/)


**Context propagation** Historically` x-datadog-*` headers, current SDKs also extract and inject W3C Trace Context and Baggage by default W3C Trace Context as the cross-vendor default


**Configuration**` DD_*` environment variables` OTEL_*` environment variables


**Backend** Datadog only Any OTLP-compatible backend


**Auto-instrumentation** Datadog-maintained for each library Community and vendor-maintained instrumentations


**Language support** Python, Node.js, Go, Java, Ruby, .NET, PHP Broader, including languages` dd-trace` doesn't support


**Lock-in** Tracing code is tied to Datadog None. The same SDK works with any OTLP backend


Whether you stay on` dd-trace` or move to OpenTelemetry comes down to portability.` dd-trace` works only with Datadog while OpenTelemetry works with any compatible backend, so teams that expect to stay on Datadog indefinitely can keep` dd-trace` , and teams that want flexibility, lower cost, or independence from a single vendor's roadmap end up moving to OpenTelemetry.


## When to Replace dd-trace with OpenTelemetry


The most common reason teams move off` dd-trace` is cost.[Datadog APM pricing](https://signoz.io/blog/datadog-pricing/) starts at $31 per host per month, with APM Pro at $35 and APM Enterprise at $40, before per-span ingestion or indexing overages. At 100 hosts on the base APM plan, that's $3,100/month for APM alone. Moving to an OpenTelemetry-native backend usually drops that bill noticeably, especially at higher host counts.


At scale, the remaining reasons start to matter as much as the bill itself. Once your tracing code uses OpenTelemetry instead of` dd-trace` , you can point traces at any backend by changing a single endpoint, which` dd-trace` simply doesn't allow. OpenTelemetry is also the open standard the industry has settled on, so most new instrumentation work goes into OTel rather than vendor-specific SDKs. Separately, Datadog's[custom metrics billing](https://signoz.io/blog/datadog-custom-metrics-pricing/) is driven by tag-combination cardinality, so rich application tags like` customer_id` or` request_id` can compound the metrics line on the invoice even when APM costs are held flat.


If any of these apply, the swap itself is mechanical. You uninstall the` dd-trace` package, install the equivalent OpenTelemetry SDK, swap the` DD_*` environment variables for` OTEL_*` , and point the OTLP exporter at your new backend.


```text
# Before: dd-trace
# pip install ddtrace
# DD_SERVICE=checkout DD_ENV=prod ddtrace-run python app.py


# After: OpenTelemetry
# pip install opentelemetry-distro opentelemetry-exporter-otlp
# opentelemetry-bootstrap -a install
# OTEL_SERVICE_NAME=checkout \
# OTEL_EXPORTER_OTLP_ENDPOINT=https://ingest.us.signoz.cloud:443 \
# OTEL_EXPORTER_OTLP_HEADERS=signoz-ingestion-key=<key> \
# opentelemetry-instrument python app.py


```


Application code is unchanged. Only the wrapper command and env vars switch from` DD_*` to` OTEL_*` . The same instrumentation works against any OTLP-compatible backend, including SigNoz Cloud.


## SigNoz Is Where Most dd-trace Migrations Land


The` dd-trace` to OpenTelemetry swap is the easy part. The harder question is which OTel backend the traces actually go to once` dd-trace` is out. Teams coming off Datadog usually pick SigNoz Cloud for the reasons below.


### Your Datadog Muscle Memory Carries Over


*Datadog-style trace waterfall in SigNoz*


SigNoz's UI is built around the same primitives a Datadog user already knows. Trace waterfalls, service maps, the log explorer, and the query builder map closely to their Datadog equivalents, so the team's existing Datadog muscle memory carries over instead of being thrown away. That tends to keep migration risk lower than picking a tool with a completely different mental model.


### Logs, Metrics, and Traces in One Platform


*Correlated logs, metrics, and traces in SigNoz*


SigNoz keeps logs, metrics, and traces in a single platform with a single query language and a single bill. Pivoting from a trace to its logs to the host metrics behind it happens inside one product, so an incident never turns into a tab-switching exercise across separate tools and the team isn't learning four query languages to answer one question.


### Lower Per-GB Pricing, No Per-Host Bills


*SigNoz pricing page*


Datadog APM is billed per host so the bill scales with how many machines you run rather than how much tracing data you actually send.[SigNoz prices traces at $0.30/GB](https://signoz.io/pricing/) with no per-host or per-user charges, so the bill is bounded by trace volume alone. Adding hosts, services, or engineers doesn't change the line item.


### OpenTelemetry-Native, So There's No Second Lock-In


A few backends accept OTLP traffic but push their own agents or SDKs back onto you for full feature parity. Picking those means trading one lock-in for another, which is exactly what you were trying to avoid when leaving` dd-trace` . SigNoz is built on OpenTelemetry from the ground up, so the instrumentation work you do for SigNoz keeps working with any other[OTLP-compatible backend](https://signoz.io/blog/opentelemetry-backend/) if you ever switch again.


### Migration Tooling Actually Built for This Move


Most OTel destinations leave the migration entirely up to you. SigNoz ships a[Datadog dashboard JSON migrator](https://signoz.io/datadog-migration-tool/) that converts your existing Datadog dashboards into SigNoz dashboards in one step, supports the open-source[OpenTelemetry Datadog Receiver](https://signoz.io/docs/migration/migrate-from-datadog/opentelemetry-datadog-receiver/) so you can forward Datadog Agent traffic to SigNoz while you swap SDKs at your own pace, and publishes per-language migration documentation for every` dd-trace` variant.


```text
receivers  :
datadog  :
endpoint  :   0.0.0.0 :  8126


exporters  :
otlp/signoz  :
endpoint  :   $ {  env :  SIGNOZ_OTLP_GRPC_ENDPOINT }
headers  :
signoz-ingestion-key  :   $ {  env :  SIGNOZ_INGESTION_KEY }


service  :
pipelines  :
traces  :
receivers  :    [  datadog ]
exporters  :    [  otlp/signoz ]


```


The OpenTelemetry Collector listens on the Datadog Agent's port, accepts traffic from your existing` dd-trace` -instrumented apps, and forwards it to SigNoz Cloud over OTLP. The staged migration runs without rewriting any SDK code on day one.


Ready to replace dd-trace? SigNoz Cloud is OpenTelemetry-native, ships a Datadog dashboard migrator, and runs the same workflow at a fraction of the per-host cost.


[Get Started - Free](https://signoz.io/teams/)


## FAQs


**What does` dd-trace` do?**


It's Datadog's tracing SDK, hard-wired to the Datadog Agent. SigNoz Cloud uses the OpenTelemetry SDK instead, so the same spans land in an OTel-native backend.


**Is` ddtrace` the same as` dd-trace` ?**


Yes, just spelled differently per ecosystem. Python uses` ddtrace` , Node.js uses` dd-trace` .


**Is` dd-trace` open source?**


Source is on GitHub but the protocol and backend are Datadog's. SigNoz Cloud is OpenTelemetry-native end to end, no translation layer needed.


**Does` dd-trace` work with OpenTelemetry?**


Partially. It accepts OTel spans and supports W3C Trace Context, but full OTel means replacing` dd-trace` with the OTel SDK and pointing it at SigNoz Cloud or any OTLP backend.


**Is there a free Datadog option?**


No free tier for Datadog APM. SigNoz Community Edition (open-source, self-hosted) and SigNoz Cloud's free trial are the two simplest free paths.


**Which package do I install for my language?**


Python:` ddtrace` . Node.js:` dd-trace` . Go:` dd-trace-go` . Java:` dd-java-agent.jar` . Ruby:` datadog` gem. .NET:` Datadog.Trace.Bundle` . PHP:` datadog-setup.php` . Replacing them with the OTel SDK is what unlocks SigNoz Cloud as the destination.


**Can I run` dd-trace` without the Datadog Agent?**


No.` dd-trace` only talks to the Datadog Agent. SigNoz Cloud uses the OpenTelemetry Collector, which has no such pinning.


**How do I move my services off Datadog's` dd-trace` SDK to OpenTelemetry?**


Uninstall` dd-trace` , install the OpenTelemetry SDK, swap` DD_*` env vars for` OTEL_*` , and point the OTLP exporter at SigNoz Cloud. Per-language steps are in the migration guide linked at the top.
