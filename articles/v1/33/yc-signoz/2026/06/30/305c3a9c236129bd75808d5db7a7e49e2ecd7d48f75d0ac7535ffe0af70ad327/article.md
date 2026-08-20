---
schema_version: "1.0.0"
document_id: "305c3a9c236129bd75808d5db7a7e49e2ecd7d48f75d0ac7535ffe0af70ad327"
company_key: "yc-signoz"
company: "SigNoz"
source_id: "yc-signoz-rss-564a62b873f8"
canonical_url: "https://signoz.io/blog/opentelemetry-rust"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.602972+00:00"
fetched_at: "2026-07-28T21:15:24.341015+00:00"
content_hash: "sha256:2c5cd86881d3e46cbf4f1fb3dc7c645f969a95abea8977ed54158a8a7a246356"
---

# Implementing OpenTelemetry in Rust Applications

# Implementing OpenTelemetry in Rust Applications


Published on: May 15, 2024


Last Updated: June 16, 2026


15 min read


Over the past several years, Rust has become the go-to language for reliable, high-throughput systems. It also influencing other ecosystems, with teams increasingly adopting Rust for the "heavy-duty" data processing layers.


Case in point: Python developers have been enjoying order-of-magnitude faster performance after switching to developer tools like[Astral’s uv](https://docs.astral.sh/uv/) for project and dependency management, or[ruff](https://docs.astral.sh/ruff/) for their linting and formatting needs.
These tools have helped cut down on CI/CD bills, and have enabled large productivity gains for engineering teams around the globe.


*[uv is essentially entirely written in Rust.](https://github.com/astral-sh/uv)*


## Why do Rust Applications Need Observability?


If you know Rust, you might wonder **why you need observability** in the first place?


While the language itself offers incredible correctness guarantees, a Rust application can still encounter slowdowns and failures as it interacts with other system components, due to human error in business logic or higher-level runtime bugs.


As such, it becomes vital to have insights into how your applications process data, what events occur during these processes, and how they affect its health and performance.


In this write-up, we will explore how to use OpenTelemetry (OTel) to instrument Rust applications through a **practical demo** , delving into key implementation details as we progress. We will capture all three telemetry signals (traces, metrics, and logs) and visualize them using an OpenTelemetry-native backend.


## What is OpenTelemetry?


This section gives a brief overview of OpenTelemetry. Feel free to skip ahead tothe next section that dives into technical decisions.


To start, we must understand what OpenTelemetry is, and why it’s vital for our purpose: understanding application behaviour across its lifespan and development cycle.


[OpenTelemetry](https://signoz.io/opentelemetry/) is a Cloud Native Computing Foundation (CNCF) project aimed at standardizing the way we instrument applications


for generating telemetry data. Before OpenTelemetry arrived, telemetry data lived in silos and often had little or no correlation between signals.


It follows a[specification-driven development](https://github.com/open-telemetry/opentelemetry-specification?tab=readme-ov-file) model that standardizes telemetry generation and collection details, meaning any compatible backend can process and visualize telemetry data emitted via its SDKs.
As there is no need to rewrite the entire instrumentation plumbing each time you change observability backends, there is **no vendor lock-in** .


Before we look at our demo application, let’s understand how logging crates


integrate into the OpenTelemetry ecosystem.


## State of Logging and OpenTelemetry


OpenTelemetry integrations across most major programming languages have a fundamental divide between how trace and metric instrumentation is configured versus how logs are handled.


While traces and metrics are typically introduced when actively implementing observability, *logging is usually defined as a fundamental default* . When scaffolding application code, engineers almost always use the language’s preferred logging libraries. This often leaves little room for utilizing the OpenTelemetry logging SDK as the default logger.


Recognizing this scenario, the OpenTelemetry Rust project[does not expect developers to use a new logging library](https://github.com/open-telemetry/opentelemetry-rust?tab=readme-ov-file#project-status) . Instead it provides *bridge libraries* to generate OpenTelemetry` LogRecords` from log statements emitted by the[log](https://docs.rs/log/latest/log/) or[tracing](https://docs.rs/tracing/latest/tracing/) crates.


Further, the project recommends using` tracing` for new Rust applications. Given this recommendation and` tracing` 's core[Span concept](https://docs.rs/tracing/latest/tracing/span/index.html) , which naturally aligns with[OpenTelemetry spans](https://signoz.io/blog/opentelemetry-spans/) , we have chosen it for logging events in our demo application.


Later, we will see how this integration actually happens within the application code.


The entire application runs on[Tokio](https://tokio.rs/) , which is often regarded as[the one true async runtime](https://corrode.dev/blog/async/#one-true-runtime) , and powers much of Rust’s networking ecosystem. The web server uses Tokio for async execution, while the telemetry and logging layer is implemented via` tracing` — another project[within the Tokio ecosystem](https://github.com/tokio-rs/tracing) .


## Implementing OpenTelemetry in Rust Applications


Now let’s set up the demo application, configure SigNoz as the backend to receive and visualize its telemetry data, and understand how we can use it to delve deep into application behaviour.
We’ll also go over key implementation details that will help you wrap your head around the moving parts involved in the instrumentation process.


Because of Rust’s compiled nature, the OTel implementation isn’t as straightforward as something like Python, which typically relies on[auto-instrumentation agents](https://signoz.io/blog/opentelemetry-agent/) to make things easy. These agents patch objects during start-up to enable applications to emit telemetry.
Here, we have implemented the instrumentation logic manually.


### Prerequisites


Before we begin, ensure you have:


- The Rust toolchain installed ([Download here](https://rust-lang.org/tools/install/) ).
- A[SigNoz Cloud account](https://signoz.io/teams/)
- uv installed ([Download here](https://docs.astral.sh/uv/getting-started/installation/#installation-methods) ).


We built the app on the` 1.93.1` version, and recommend that you use a similar version for maximum compatibility.
You can check the version of your Rust installation via` rustup check` , and if necessary, update via the` rustup update` command:


```text
❯ rustup check
stable-aarch64-apple-darwin - Up to  date    :    1.93  .1  (  01f6ddf75  2026  -02-11 )
rustup - Up to  date    :    1.28  .2


❯ rustup update
info: syncing channel updates  for    'stable-aarch64-apple-darwin'
info: checking  for   self-update


stable-aarch64-apple-darwin unchanged - rustc  1.93  .1  (  01f6ddf75  2026  -02-11 )


info: cleaning up downloads  &   tmp directories


```


` uv` is required to run the Python script to emulate an upstream microservice to showcase[trace context propagation](https://signoz.io/blog/context-propagation-in-distributed-tracing/) .


**Setting up SigNoz**


SigNoz is an OpenTelemetry-native observability platform that provides logs, traces, and metrics in a single pane of glass.


- [Sign up](https://signoz.io/teams/) for a free SigNoz Cloud account.
- [Follow this guide](https://signoz.io/docs/ingestion/signoz-cloud/keys/) to create ingestion keys for your account.
- Ensure the region and ingestion key values are readily accessible for the following steps.


Once done, you’re ready to setup the application and point it to your SigNoz instance.


### Running the Rust Demo Application


Clone the SigNoz Examples repository and navigate to the application folder:


```text
git   clone https://github.com/SigNoz/examples.git
cd   examples/rust/opentelemetry-rust-demo


```


Install the application dependencies and set the OpenTelemetry-specific environment variables while running the application:


```text
OTEL_EXPORTER_OTLP_ENDPOINT  =  "https://ingest.<region>.signoz.cloud:443"    \
SIGNOZ_INGESTION_KEY  =  "<your-ingestion-key>"    \
OTEL_RESOURCE_ATTRIBUTES  =  "service.name=opentelemetry-rust-demo,service.version=0.1.0,deployment.environment=dev"    \
cargo   run


```


Set your SigNoz tenant region and ingestion key in the` OTEL_EXPORTER_OTLP_ENDPOINT` and` SIGNOZ_INGESTION_KEY` environment variables respectively.
` OTEL_RESOURCE_ATTRIBUTES` defines the metadata to attach with each batch of telemetry that goes out of our application. The service name` opentelemetry-rust-demo` ensures the OTel backend can recognize *where the telemetry is coming from* .


Once the application compiles and runs, you’ll see an output like:


*The OpenTelemetry Rust application emits log events via the tracing crate.*


The application is now live on` http://localhost:8085` . Visit the URL and ensure that you get a` Hello, World!` response.


Before we move ahead, let’s go over the key implementation details to understand exactly how we’ve integrated OpenTelemetry into our Rust application.


### Anatomy of the OpenTelemetry Setup


The application uses the` hyper` HTTP crate to expose API endpoints. Calling these endpoints allows users to calculate the Fibonacci sequence of` u8` integers, make an external API call to showcase trace context propagation, or receive 404 responses via a fallback handler — for example, if you call` //` instead of the index` /` endpoint.


**Dependency Choices**


We install dependencies for running the web server, executing the core application logic (e.g.,` serde` &` serde_json` for parsing request bodies), and enabling OpenTelemetry along with its bridge to the` tracing` crate.


The` opentelemetry-otlp` crate, which implements the exporter logic for telemetry signals, requires the` grpc-tonic` and` tls-roots` features to communicate with secure[OTLP backends](https://signoz.io/blog/what-is-otlp/) over gRPC.


**The Core OpenTelemetry Setup**


The core of the OpenTelemetry configuration involves building providers for each telemetry signal.


The` init_<signal>_provider` functions build` OTLP Exporter` s with the necessary configuration and define them as the "provider" for global usage. As all three signals export to the same SigNoz instance, they share the same configuration in their` builder` methods.


```text
fn    init_tracer_provider  (  )    ->    SdkTracerProvider    {
...
// use gRPC exporter with TLS and metadata headers for SigNoz cloud
let   otlp_endpoint  =    OTLP_ENDPOINT  .  clone  (  )  ;
let   otlp_exporter  =    OtlpSpanExporter  ::  builder  (  )
.  with_tonic  (  )
...    // reduced for brevity
.  build  (  )
.  unwrap  (  )  ;


let   provider  =    SdkTracerProvider  ::  builder  (  )
.  with_batch_exporter  (  otlp_exporter )
.  build  (  )  ;


global ::   set_tracer_provider  (  provider .  clone  (  )  )  ;
provider
}


```


If you need to debug issues or understand what the raw telemetry data looks like, enable the exporters from the` opentelemetry_stdout` crate. We have commented them out to avoid cluttering the console.


**Trace Context Propagation**


For distributed traces, we also initialize a` TraceContextPropagator` . We combine this with our custom wrapper structs that implement the OTel` Extractor` and` Injector` traits against` hyper` 's headers.


```text
struct    HeaderExtractor  <  'a  >  (  &  'a    hyper ::  header ::   HeaderMap  )  ;


impl  <  'a  >    Extractor    for    HeaderExtractor  <  'a  >    {
fn    get  (  &  self  ,   key :    &  str  )    ->    Option  <  &  str  >    {
self  .  0  .  get  (  key )  .  and_then  (  |  value |    value .  to_str  (  )  .  ok  (  )  )
}


fn    keys  (  &  self  )    ->    Vec  <  &  str  >    {
self  .  0  .  keys  (  )  .  map  (  |  k |    k .  as_str  (  )  )  .  collect  (  )
}
}


struct    HeaderInjector  <  'a  >  (  &  'a    mut    hyper ::  header ::   HeaderMap  )  ;


impl  <  'a  >    Injector    for    HeaderInjector  <  'a  >    {
fn    set  (  &  mut    self  ,   key :    &  str  ,   value :    String  )    {
let   header_name  =    match    hyper ::  header ::   HeaderName  ::  from_bytes  (  key .  as_bytes  (  )  )    {
Ok  (  name )    =>   name ,
Err  (  _ )    =>    return  ,
}  ;
let   header_value  =    match    hyper ::  header ::   HeaderValue  ::  from_str  (  &  value )    {
Ok  (  value )    =>   value ,
Err  (  _ )    =>    return  ,
}  ;
self  .  0  .  insert  (  header_name ,   header_value )  ;
}
}


```


The` router` function uses the` HeaderExtractor` to extract trace context from incoming requests’ headers, setting it as the parent context for the current span.
Conversely, the` httpbin` function utilizes the` HeaderInjector` to inject the active trace context into the headers of outgoing requests, ensuring that the trace remains active across service boundaries.


**Bridging OpenTelemetry Traces and Logs**


The` init_tracing_subscriber` function registers the following layers as` Subscriber` s for the global` tracing` provider:


- **The` OpenTelemetryTracingBridge` layer** , which uses our OTel logging provider to ensure all application` Event` s are also emitted as OpenTelemetry` LogRecord` statements.
- **The OpenTelemetry Span layer** , created via the` tracing_opentelemetry` crate, ensures that` tracing` spans are reliably translated into equivalent OpenTelemetry Spans.


```text
fn    init_tracing_subscriber  (  tracer :    SdkTracer  )    {
let   filter  =    EnvFilter  ::  new  (
"info"  ,    // removed values for brevity
)  ;
let   logger_provider  =    LOGGER_PROVIDER
.  get  (  )
.  expect  (  "logger provider should be initialized before tracing subscriber"  )  ;
let   otel_log_layer  =    OpenTelemetryTracingBridge  ::  new  (  logger_provider )  ;
let   otel_span_layer  =    tracing_opentelemetry ::   layer  (  )  .  with_tracer  (  tracer )  ;


tracing_subscriber ::   registry  (  )
.  with  (  filter )
.  with  (  tracing_subscriber ::  fmt ::   layer  (  )  )
.  with  (  otel_span_layer )
.  with  (  otel_log_layer )
.  init  (  )  ;
}


```


As discussed above, the OTel Rust project focuses on *bridging* existing logging implementations. The` opentelemetry::global` module therefore lacks a log-equivalent of the` set_tracer_provider` and` set_meter_provider` helper functions.


To work around this limitation and to safely share the logging provider instance across the application, we store it in a` OnceLock` .
` OnceLock` guarantees that the provider initializes only once, and provides lock-free read access across all Tokio tasks.


```text
static    LOGGER_PROVIDER  :    OnceLock  <  SdkLoggerProvider  >    =    OnceLock  ::  new  (  )  ;
...
fn    init_logger_provider  (  )    {
...
let   provider  =    SdkLoggerProvider  ::  builder  (  )
.  with_log_processor  (  BatchLogProcessor  ::  builder  (  otlp_exporter )  .  build  (  )  )
.  build  (  )  ;
let   _  =    LOGGER_PROVIDER  .  set  (  provider )  ;
}


```


In contrast, we acquire the tracer explicitly within the` main` function using the global tracing provider we set up earlier, and pass it directly as a function argument to` init_tracing_subscriber` .


### Generating Telemetry Data for Visualization


To ensure you have enough data to experiment with, and understand how OpenTelemetry works, we have prepared a simple load generator script that calls the endpoints defined in the application. You can run it in a separate terminal instance:


```text
chmod   +x load_gen.sh
./load_gen.sh


```


To induce failures in the fibonacci endpoint, you can try sending a large number manually.


```text
curl    -i   localhost:8085/fibonacci  -d    '{"number": 20111111}'


```


Since our application expects a` u8` integer, it will raise an appropriate error when attempting to parse the input:


```text
{  "error"  :  "invalid value: integer `20111111`, expected u8 at line 1 column 19"  }


```


Such entries will be explicitly marked as failures by our application, and reflected in our observability backend. In SigNoz, you should now see a service named` opentelemetry-rust-demo`


*SigNoz trace detail view showcasing an error trace for sending an invalid payload.*


The trace details view has a` Related Signals` section. By clicking on the` Logs` button, you will see a split view showing all logs related to that trace via trace and span IDs.
You can open the entries in the Logs Explorer view if you wish to inspect them in detail.


*Access all the logs for a trace via telemetry correlation in SigNoz.*


### Visualizing the Flow of Data


In distributed systems, it is common for a single user request to span across multiple services. To demonstrate this flow, let’s call our Rust application from a Python script that is also instrumented to export telemetry to OpenTelemetry.


Before running the command, ensure your region and ingestion key are set in the environment variables:


```text
OTEL_SERVICE_NAME  =  "py-rust-client"    \
OTEL_EXPORTER_OTLP_ENDPOINT  =  "https://ingest.<region>.signoz.cloud:443"    \
OTEL_EXPORTER_OTLP_HEADERS  =  "signoz-ingestion-key=<your-signoz-key>"    \
uv run  \
--with   opentelemetry-distro  \
--with   opentelemetry-exporter-otlp  \
--with   opentelemetry-instrumentation-requests  \
opentelemetry-instrument python scripts/python_client.py


```


You will be prompted to enter the number of requests you’d like to make to the application. For now, you can just enter` 1` .


```text
uv run  ..  .


Installed  25   packages  in   21ms
Enter max API calls to make:  1
{  'httpbin'  :    {  'args'  :    {  }  ,  'data'  :    ''  ,  'files'  :    {  }  ,  'form'  :    {  }  ,  'headers'  :    {  'Accept'  :    '*/*'  ,  'Host'  :    'httpbin.org'  ,  'Traceparent'  :    '00-92bddc16cccfe39df69db2bf6e79a263-4e60e386e4e38e02-01'  ,  'Tracestate'  :    ''  ,  'X-Amzn-Trace-Id'  :    'Root=1-69ac1ead-255755c94cab463b7763a7c4'  }  ,  'json'  :   None,  'method'  :    'GET'  ,  'origin'  :    '106.215.144.235'  ,  'url'  :    'https://httpbin.org/anything'  }  ,  'note'  :    'This endpoint calls https://httpbin.org/anything with propagated trace context.'  ,  'propagated'  :    {  'traceparent'  :    '00-92bddc16cccfe39df69db2bf6e79a263-4e60e386e4e38e02-01'  }  }


```


*Notice the` Traceparent` header in the output above. This is the exact context injected by our application before it made the external call to` httpbin.org` !*


If you open SigNoz trace view and inspect the` GET` request from the` py-rust-client` , you will see a detailed breakdown of the flow of the trace across the service layers.
The Python script called the Rust` /external` endpoint, which in turn called the external` httpbin.org` API, propagating the trace context with each network hop.


*A distributed trace in SigNoz. Notice how each external call creates a child span under the caller service’s span.*


This propagation ensures that all spans created during the request fall under a single trace. Whenever a service utilizes an existing trace context, it attaches its spans **as children** of the upstream API call’s span.


Here, the Python client’s span becomes the parent for the Rust application, which becomes the parent for the further downstream` httpbin` API.


### Monitor System Health with Metrics


[Metrics](https://signoz.io/blog/opentelemetry-metrics-with-examples/) are a foundational pillar of observability, and for good reason. Tracking key metrics helps you understand overall system health at a glance.
In this demo, we are capturing two "standard" metrics — *active requests* via an up-down counter and *request duration* via a histogram.


When ingesting metric data, SigNoz performs aggregations on the raw data to make the data digestible. Let’s see what insights we can derive from these metrics after the load generator has been running for some time.


-


**Active Requests:** Since this metric records the number of in-flight requests, it will primarily capture data points for the (relatively) long-running` /fibonacci` and the` //` fallback endpoints.


While the fallback endpoint returns a 404 response after some delay, the` /` endpoint receives a request and processes it within few milliseconds, meaning its counter increments and decrements almost instantly.
So the periodic metric export snapshot is unlikely to capture its value as non-zero.


*Viewing the Active Requests data per-url, we can see predictable traffic patterns.*


-


**Request Duration** : This metric is an OpenTelemetry[histogram](https://signoz.io/blog/opentelemetry-histogram/) that records the duration of each request and increments a counter corresponding to a specific duration *bucket* for that endpoint. These buckets enable the observability backend to calculate percentiles and infer the performance of that endpoint over time.


By checking the p99 aggregate, we can see that the` /external` API has experienced a spike in its latency, jumping from ~2.5s to 4s, while the other endpoints have remained stable.
In the real world, an engineer would investigate this spike: are we being rate-limited by the downstream external API, are we saturating the server’s network bandwidth, and so on.


*Request duration histograms track system health by monitoring p99 (and other) latencies.*


By now, you must have understood how OpenTelemetry helps eliminate the guesswork behind the occasional latency spike, or by breaking down previously "invisible" failures.
This level of observability becomes absolutely vital as applications continue to grow in complexity.


## What's Next with OpenTelemetry and Rust


With this blog, we hope that now you have the fundamental knowledge required to begin your observability journey with OpenTelemetry and Rust.


We began by exploring why you should instrument your Rust applications, defined OpenTelemetry and its role in the observability landscape, and understood the code implementations and the dependency choices.
Finally, we used SigNoz to visualize events as they happen in Rust applications and observe their impact on performance.


[SigNoz](https://signoz.io/) is an OpenTelemetry-native platform that visualizes traces, metrics, and logs in a single pane. If you’re interested in trying out SigNoz for your applications,[sign up](https://signoz.io/teams/) for a 30-day free trial (no credit card required).
