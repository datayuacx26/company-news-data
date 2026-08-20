---
schema_version: "1.0.0"
document_id: "78e27f41daa28348b2732e34cc4c74f9c149f052998e18df57eea88cd489fb1c"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/local-tracing/"
published_at: "2026-08-04T13:00:00+00:00"
first_seen_at: "2026-08-04T14:03:34.623865+00:00"
fetched_at: "2026-08-04T14:03:42.322861+00:00"
content_hash: "sha256:72c3f470a7d3df375b37bf46822463975bce79a7be91154614592df81afe7fa2"
---

# Your agent can now debug Workers with local tracing

Starting today,` wrangler dev` and` vite dev` automatically capture[OpenTelemetry traces](https://developers.cloudflare.com/workers/observability/traces/) for local Worker invocations. When Cloudflare's tooling detects an agent session, it points the agent to the[Local Explorer API](https://developers.cloudflare.com/workers/local-development/local-explorer/#use-with-ai-coding-agents) , a local debugging API where it can query those traces. You do not need to install an SDK, enable tracing, configure your agent, or even mention observability in the prompt.


A prompt can be as simple as:


```text
POST /api/orders is returning 500. Find the cause, fix it, and verify the fix locally.
```


This builds on years of investment in local development, from introducing[Miniflare](https://blog.cloudflare.com/miniflare/) to making local mode the default in[Wrangler 3](https://blog.cloudflare.com/wrangler3/) . Local traces give coding agents structured feedback from that development environment before code is deployed.


## Agents discover the Local Explorer API automatically


As part of its normal workflow, an agent starts` wrangler dev` or` vite dev` to run and test the Worker. When the development server recognizes a supported coding-agent session, it automatically prints a hint that looks like this:


```text
This dev session is running in an AI agent.


The Local Explorer API is available at
http://localhost:8787/cdn-cgi/explorer/api


...


Debug with traces:
POST /cdn-cgi/explorer/api/local/observability/query -- query traces and logs with SQL
```


The[Local Explorer](http://developers.cloudflare.com/workers/local-development/local-explorer) is a browser-based interface and REST API for viewing and editing local resource data and querying observability data during development. The API root serves an OpenAPI schema, so agents can discover available endpoints at runtime without hardcoded instructions.


The automatically captured traces are available through a read-only observability endpoint in that API, together with their correlated console logs. The agent can query this telemetry, then use the API's other operations to inspect local Workers and bindings or examine state in D1, KV, R2, Durable Objects, and Workflows.


## Find the failure and verify the fix


Consider` POST`` /api/orders` , which retrieves an active cart from KV, saves the checkout details into D1, and sends a message to a Queue for order processing. After a schema change, the endpoint suddenly starts returning a 500 status.


### Without local traces


The 500 does not identify which operation failed. The agent adds logs around KV, D1, and the Queue, reruns the request, inspects the output, and repeats. Each cycle takes time and burns tokens while the agent reconstructs the request from text.


### With local traces


The agent reproduces the error and queries the read-only observability endpoint. The trace shows that the KV read succeeded, the D1 insert failed with` no such column: delivery_window` , and the Queue was never called. Your agent uses the Local Explorer API to access the same trace data you would see here:


The agent uses the API to inspect the D1 schema. It finds that the migration adding delivery_window exists in the repository but has not been applied locally, applies it, sends the request again, and queries the new trace. Issue resolved.


In one local loop, the agent identifies the failed operation, fixes the local environment, and verifies the result without deploying or adding temporary logs.


## Explore traces and logs in Local Explorer


Agents query local telemetry through the API, but you as a human can visualize the same data in the[Local Explorer](https://developers.cloudflare.com/workers/local-development/local-explorer/) , the browser-based interface built into the local development server. Alongside browsing local binding state, you can select a request to inspect its spans, timing, attributes, errors, and correlated console logs.


Local Explorer runs on the same localhost origin as your Worker, not in the Cloudflare dashboard. Press e in Wrangler or visit` /cdn-cgi/explorer` on the local server to open it.


## How it works


When we launched[Workers Tracing](https://blog.cloudflare.com/workers-tracing-now-in-open-beta/) , we built instrumentation directly into[workerd](https://github.com/cloudflare/workerd) , the open-source runtime that powers Workers. Without requiring an SDK or any code changes, the runtime captures spans for:


- **Fetch calls:** All outbound HTTP requests, including timing, status codes, and request metadata.
- **Binding calls:** Every interaction with KV, R2, D1, Durable Objects, Queues, and other bindings.
- **Handler calls:** The full lifecycle of each invocation, from` fetch` to` scheduled` to queue handlers.


Any[custom spans](https://developers.cloudflare.com/workers/observability/traces/custom-spans/) emitted by **** your application will also appear alongside these automatic spans.


[Wrangler](https://developers.cloudflare.com/workers/local-development/#start-a-local-development-server) and the[Cloudflare Vite plugin](https://developers.cloudflare.com/workers/vite-plugin/) use Miniflare to run your Worker locally in the same runtime, making this instrumentation available during local development.


Miniflare collects runtime events and console output, assembles them into OpenTelemetry traces and correlated logs, then writes the telemetry to an internal SQLite-backed Durable Object that serves as the local trace store. The Local Explorer API exposes that data through the local development server where agents can easily query traces and logs and inspect local state.


## Get started


Update Wrangler or the Cloudflare Vite plugin, whichever your project uses:


```text
# Wrangler
npm   install   --save-dev   wrangler@latest


# Cloudflare Vite plugin
npm   install   --save-dev   @cloudflare/vite-plugin@latest
```


Then ask your agent to debug locally as you normally would. Your agent can already write and run your Worker locally — now it can see what happened, fix what failed, and verify the result before you deploy. Check out the[docs](http://developers.cloudflare.com/workers/local-development/local-explorer/#api) to learn more!
