---
schema_version: "1.0.0"
document_id: "f3bee9a2bdfa90ec2d0dd005e48de648d6948407075d7fa7b13e24b66d6e5226"
company_key: "yc-signoz"
company: "SigNoz"
source_id: "yc-signoz-rss-564a62b873f8"
canonical_url: "https://signoz.io/blog/opentelemetry-architecture"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.602972+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:4781c7cefdea1d5ecfb290a0cc9621cf7baaeef8a0535d6499f86d3c9433016e"
---

# OpenTelemetry Architecture - Understanding the design concepts

# OpenTelemetry Architecture - Understanding the design concepts


Published on: February 23, 2023


Last Updated: June 16, 2026


8 min read


OpenTelemetry consists of several main components. But to understand the architecture, we will start by understanding some of the key design principles behind the design and architecture of OpenTelemetry.


But before that, let us have a brief overview of OpenTelemetry.


## What is OpenTelemetry?


[OpenTelemetry](https://opentelemetry.io/) is an open-source observability framework that aims to standardize the generation, collection, and management of telemetry data (logs, metrics, and traces). It is incubated under the Cloud Native Computing Foundation ([CNCF](https://www.cncf.io/) ), the same foundation that incubated Kubernetes.


OpenTelemetry follows a[specification-driven](https://opentelemetry.io/docs/specs/otel/) development and provides client libraries to instrument applications in most programming languages. Once you have instrumented with OpenTelemetry, you should be able to collect various telemetry signals like logs, metrics, and traces from it.


## What are OpenTelemetry Signals?


OpenTelemetry specification is organized into distinct types of telemetry, which we call **signals** . Currently, OpenTelemetry supports three prominent telemetry signals: traces, metrics, and logs, which are often called the[three pillars of observability](https://signoz.io/blog/three-pillars-of-observability/) .
Signals are the most fundamental unit of design in OpenTelemetry.


OpenTelemetry provides a way to **merge the three signals into a single cohesive data stream** so that debugging issues becomes very easy. For example, if you look at metrics on a chart, you can find the corresponding logs or find the trace associated with that metric instance if you have instrumented your application with OpenTelemetry client libraries.


*Telemetry signals are correlated to give maximum contextual insights for troubleshooting issues.*


## Design Concepts


OpenTelemetry is a cross-cutting concern that follows the execution of a transaction as it passes through each library and service. To achieve this feat, it uses a separation of concerns design principle to separate signals from the underlying system, which stores context data and helps propagate data across network calls.


*Context and Propagators in Action*


All OpenTelemetry signals are built on top of a shared[context propagation](https://signoz.io/blog/opentelemetry-context-propagation/) system. Other non-observability cross-cutting concerns may also use the context propagation mechanism to transport their data through a distributed system. Let's understand the key objects which form the Data transport system of OpenTelemetry.


### Context:


The context object is a key-value store associated with an execution context, such as a thread or coroutine. Implementation of context is language-dependent, but OpenTelemetry provides a context object in every language.


### Propagators:


In order for[distributed tracing](https://signoz.io/distributed-tracing/) to work, the[trace context](https://signoz.io/blog/context-propagation-in-distributed-tracing/) must be shared by every service that participates in the transaction. Propagators accomplish this by serializing and deserializing the context object, allowing the signals to follow their transactions across network requests.


Hopefully, you must be comfortable with the basic concepts now. Let's try to understand the design of OpenTelemetry from a client perspective.


## Client Side Architecture


An application is instrumented with OpenTelemetry by installing a collection of software libraries: the API, the SDK (software development kit), SDK plug-ins, and library instrumentation. This set of libraries is referred to as the OpenTelemetry client.


*OpenTelemetry client-side architecture*


As you can see from the diagram above, there are two key concepts to understand -


the API & the SDK.


### The OpenTelemetry API


**The API** is the bare-bones interface for instrumentation and does not provide any implementation. Third-party libraries or vendors can instrument their code using the API. This is the only part of OpenTelemetry that shared libraries and application logic should take a direct dependency on.


### The OpenTelemetry SDK


**The SDK** is a complete language library that provides implementations of the API. The SDK implements the OpenTelemetry API by providing a plug-in framework. When an application starts, an implementation can be loaded by registering a provider for each signal.


The providers become the receivers of all API calls. When no providers are loaded, the API defaults to a no-op provider. This makes OpenTelemetry instrumentation safe to include in shared libraries. If the application does not use OpenTelemetry, the API calls simply become no-ops and do not incur any overhead.


OpenTelemetry provides SDK implementations for major languages, as shown below.


*OpenTelemetry Standard and Implementation Libraries support*


## Server-Side Architecture


The server-side architecture is mostly concerned with what to do with the telemetry data which is generated in your application by using the API and SDK. The telemetry data needs to be received, processed, and then sent to the target visualization or storage tools like[SigNoz](https://signoz.io/) .


*How OpenTelemetry fits in the entire application stack - OTel agent sends data to OTel collector from where you can export data to a backend of your choice*


As per the figure, you see three key components:


**On the client side:**


The applications uses an agent ( Auto instrumentation ) to produce telemetry data using zero code instrumentation.


**On the server side:**


-


All the signal data is sent to a **Collector** component which is considered to be the heart of the system. It is optional, but any matured and complex implementation will need an[OpenTelemetry collector](https://signoz.io/guides/opentelemetry-collector-vs-exporter/) component in the architecture.


-


All the data gets processed via the Collector and then sent to different observability backends. More on Collectors later, but the collector is responsible for **receiving, processing, and sending** the signal data to the target visualization tool.


-


Application telemetry data can now be exported to **multiple backends** , depending on the requirements. Also, note that you can plug in various out-of-box **exporters** for any target backend.


For example - *Jaeger backend accepts both jaeger format as well as OTLP format.*


*But Zipkin needs data in Zipkin format, so Zipkin exporter translates the trace data from OTLP to Zipkin native format. In the diagram , you can also see that we can configure the exporters directly from the agent library without routing the traces via collector, but such implementations are for simplified requirements only.*


## Understanding OpenTelemetry Collectors


OpenTelemetry Collector is a **vendor-independent agent** that can receive, process, and export telemetry data. It can receive telemetry data in various formats, including OTLP (the native OpenTelemetry protocol), Jaeger, Zipkin, Prometheus, and other formats, including proprietary ones.


The OpenTelemetry Collector then forwards the telemetry it receives to one or more observability backends. It also supports processing and filtering of telemetry data before export, for example, batching data to increase the efficiency of compression or converting between formats, etc.


*Architecture of OpenTelemetry Collector with receivers, processors and exporters.*


For the Collector internals referenced here, our[OpenTelemetry Collector complete guide](https://signoz.io/blog/opentelemetry-collector-complete-guide/) goes deeper on each component.


## Components in OpenTelemetry Collector


The Collector consists of three components that access telemetry data:


-


[Receivers](https://opentelemetry.io/docs/collector/configuration/#receivers) — A receiver, which can be push or pull-based, is how data gets into the Collector. Receivers may support one or more[data sources](https://opentelemetry.io/docs/concepts/signals) ).


-


[Processors](https://opentelemetry.io/docs/collector/configuration/#processors) — Processors are run on data between being received and being exported. Processors are optional though[some are recommended](https://github.com/open-telemetry/opentelemetry-collector/tree/main/processor#recommended-processors) . Processors are very useful when you want to do sampling, enriching, or any additional processing with the telemetry data.


-


[Exporters](https://opentelemetry.io/docs/collector/configuration/#exporters) — An exporter, which can be push or pull-based, is how you send data to one or more backends/destinations. Exporters may support one or more[data sources](https://opentelemetry.io/docs/concepts/signals) . Once the processing is completed, the data can be exported by the Collector to the required destinations.


OpenTelemetry Collector also provides a feature known as pipelines . For a given use case, we can configure a pipeline as below to handle a particular kind of signal.


*OpenTelemetry Collector Pipeline*


Pipeline defines a path the data follows in the Collector, starting from reception, then further processing or modification, and finally exiting the Collector via exporters. You can chain the processors, and they will get executed in the order of their definition. Everything is configurable.


In the example above, we can configure multiple receivers to feed the data to a set of processors and then export the data to multiple exporters.


With this, we have come to conclude our topic on the Architecture of OpenTelemetry.


## Conclusion


OpenTelemetry is quietly becoming the world standard for instrumenting cloud-native applications. It is the right choice for instrumentation as it future-proofs your observability set up from any vendor lock-ins. It is also beneficial for companies as onboarding becomes consistent, and with time a good knowledge base can be built.


It is easy to get started with OpenTelemetry. You can check out[instrumentation instructions](https://signoz.io/docs/instrumentation/) . As OpenTelemetry does not provide a backend, you need to choose one which supports OpenTelemetry. An[OpenTelemetry-native APM](https://signoz.io/blog/opentelemetry-apm/) can be a good choice for your observability backend.


SigNoz is a full-stack[open source APM](https://signoz.io/application-performance-monitoring/) that you can use as your[OpenTelemetry backend](https://signoz.io/blog/opentelemetry-backend/) . It provides logs, metrics, and traces under a single pane of glass with intelligent correlation between the telemetry signals.


## Getting started with SigNoz


SigNoz Cloud is the easiest way to run SigNoz.[Sign up](https://signoz.io/teams/) for a free account and get 30 days of unlimited access to all features.


You can also install and self-host SigNoz yourself since it is open-source. With 24,000+ GitHub stars,[open-source SigNoz](https://github.com/signoz/signoz) is loved by developers. Find the[instructions](https://signoz.io/docs/install/) to self-host SigNoz.
