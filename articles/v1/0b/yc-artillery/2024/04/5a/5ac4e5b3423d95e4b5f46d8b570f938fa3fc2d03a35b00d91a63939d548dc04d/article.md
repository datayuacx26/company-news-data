---
schema_version: "1.0.0"
document_id: "5ac4e5b3423d95e4b5f46d8b570f938fa3fc2d03a35b00d91a63939d548dc04d"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/announcing-tracetest-integration"
published_at: "2024-04-16T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:de8f55ee356fa704d4e382130773a261b62886806b51fa4e698f3dc0d7258dbc"
---

# Announcing Tracetest integration

April 16th, 2024[Announcement](https://www.artillery.io/blog/tag/announcement)


# Announcing Tracetest integration


Ines Fazlic


Adnan Rahic


Today we’re announcing an integration with[Tracetest](https://docs.tracetest.io/) , a trace-based end-to-end debugging and testing tool. Tracetest is an open-source project that is part of the CNCF landscape, with a[cloud-based managed platform](https://app.tracetest.io/) for enterprise use-cases.


*Join the Artillery and Tracetest teams for a live webinar to learn hands-on how to use Artillery load testing with Playwright and Tracetest!*


## Why Tracetest?


Load testing typically runs from the perspective of your end-users. That’s a good thing, but sometimes it means you miss part of the picture. For example, if your service under test triggers an asynchronous job, you may not be able to understand that that feature is failing under scale from load test results alone.


Tracetest uses your existing[OpenTelemetry](https://opentelemetry.io/) traces to power trace-based testing with assertions against trace data at every point of the request transaction. It makes it possible to:


- Define tests and assertions against every single microservice that a trace goes through.
- Build tests based on your already instrumented system.
- Define multiple transaction triggers, such as a GET against an API endpoint, a GRPC request, a Kafka message queue, etc.
- Define assertions against both the response and trace data, ensuring both your response and the underlying processes worked as intended.
- Save and run the tests manually or via CI build jobs with the Tracetest CLI.


With this integration, load tests run with Artillery can be easily analyzed and checked end-to-end with Tracetest, providing a more comprehensive view of how your system behaves under load.


## Try it out


- [Follow this guide](https://docs.tracetest.io/tools-and-integrations/artillery-plugin#run-this-quckstart-example) to try out a running example that uses Docker Compose.
- An example for Tracetest with Artillery and Playwright can be found[here](https://docs.tracetest.io/examples-tutorials/recipes/running-playwright-performance-tests-with-artillery-and-tracetest) .


## Find out more


- Learn more about Tracetest[here](https://docs.tracetest.io/)
