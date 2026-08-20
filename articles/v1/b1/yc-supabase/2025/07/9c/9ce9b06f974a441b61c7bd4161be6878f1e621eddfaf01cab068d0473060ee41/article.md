---
schema_version: "1.0.0"
document_id: "9ce9b06f974a441b61c7bd4161be6878f1e621eddfaf01cab068d0473060ee41"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/new-observability-features-in-supabase"
published_at: "2025-07-17T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:a4b4be0924dc4dea25964bd1fabe45fcbb04c2a0c7a981c86b237f980c010afb"
---

# New Observability Features in Supabase

We are starting to add OpenTelemetry support to[all](https://github.com/supabase/storage/pull/494)[our](https://github.com/supabase/auth/pull/679)[core](https://github.com/supabase/edge-runtime/pull/554)[products](https://github.com/supabase/realtime/commit/c9683f3f5f94bd2e37494f02c1f4415551e96e5b) and[our Telemetry server](https://github.com/Logflare/logflare/pulls?q=is%3Apr+otel+sort%3Acreated-asc) . OpenTelemetry (OTel) standardizes logs, metrics, and traces in a vendor-agnostic format, so you can ingest data into tools like Datadog, Honeycomb, or any monitoring solution you already use. While you'll still have the freedom to bring your own observability stack, we're preparing to surface this data natively in the Supabase dashboard.


Today, we are announcing:


- New Logging Interface (coming soon!)
- Advanced Product Reports
- Supabase AI Assistant with debugging capabilities (coming soon!)


If you would like early access to the new Logging Interface and Supabase AI Assistant with debugging capabilities please sign up[here](https://forms.supabase.com/unified-logs-signup) .


These updates mark the first step toward unified, end-to-end observability. You won't get the full OTel visualization just yet, but with these foundations in place, you'll soon be able to trace, analyze errors and performance issues, and troubleshoot your entire stack without leaving Supabase.


## New logging Interface#


Supabase is a collection of seamlessly integrated services. Storage talks to Postgres via the dedicated connection pooler. Edge Functions can talk to Auth and Realtime. If Storage uploads fail, you must determine whether the problem lies with the Storage server, the dedicated connection pooler, or the database. Previously, pinpointing the root cause meant jumping between multiple log streams.


Now, there is one interleaved stream of logs across all services. You can trace a single request across the entire Supabase stack. No more jumping between tabs to diagnose errors.


We have also added contextual log views. You can now jump from a function's invocation log directly into its execution logs. What used to require two disconnected sources is now stitched together in one view.


The new interface also supports filtering logs by the request status code, method, path, log level and the auth user associated with the request. This means you can quickly find all Postgrest 500 errors, or all requests made by a specific user with a few clicks.


The new interface currently supports API Gateway logs and Postgres logs, with logs for the other products coming soon.


Shoutout to[openstatus.dev](http://openstatus.dev/) for providing the inspiration for some of our Log components.


## Advanced Product Reports#


Apart from making our logs better, we also revamped the metrics exposed in our product reports. Previously, you had to host your own[Grafana dashboard](https://github.com/supabase/supabase-grafana) to access some of these advanced metrics. We are bringing some of these metrics directly into the dashboard, so that you can access them without any additional setup or maintaining your own production ready monitoring infrastructure.


Each product has its own dedicated[report](https://supabase.com/dashboard/project/_/observability/api-overview) with a common set of metrics like number of requests, egress, and response time, along with product specific metrics like “Realtime connected clients”.


Additionally, you can drill into a specific time frame and filter by various request and response parameters across all reports.


Free users get a basic set of metrics for all products, while some of the advanced metrics (like p99 response time) is available for all paid customers.


Try out the new reports[here](https://supabase.com/dashboard/project/_/observability/api-overview) .


## Supabase AI Assistant with debugging capabilities#


The Supabase AI Assistant now offers powerful new debugging capabilities, making it easier to identify and resolve issues across your stack.


You can now ask the Assistant to:


- Retrieve logs for any Supabase product
- Analyze log volume over time to identify spikes
- Drill into specific time windows to investigate anomalies


This means you can go from "something looks off" to concrete answers, without leaving the chat.


The Assistant also comes with several quality-of-life upgrades:


- **Automatic chat renaming** based on your queries
- **Branch diff reviews** , perfect for projects using branching environments
- **A more capable model** with additional controls for data privacy and security improvements built in


It's the fastest way to get answers for your project, whether you're debugging a failing function, reviewing changes between branches, or just trying to understand how your app is behaving in production.


This is an example of how the Assistant can analyze logs to identify issues:


It can also provide recommendations for fixes:


## What's next#


- We'll keep adding more metrics across our reports
- We're adding logs from the remaining products to new logging interface
- We plan to make the new logging interface the default experience for all projects soon
- Expose OpenTelemetry trace information in the logging interface
