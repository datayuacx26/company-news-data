---
schema_version: "1.0.0"
document_id: "1ffe44324932a2c0df2bf22a135d86605b420726e507cff1257b350c4bc23bbe"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/wiremock-vs-mockserver-vs-proxymock/"
published_at: "2026-03-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:53:30.609843+00:00"
content_hash: "sha256:7602981746a283258e98003df0acd3b3cfc749f5dc9ee855df985c40d054ec66"
---

# WireMock vs MockServer vs Proxymock: Java Mocking in 2026

Your WireMock stubs are lying to you. They were accurate when someone wrote them six months ago, but the payment API added a` metadata` field in January, the inventory service switched from REST to gRPC in February, and nobody updated the stubs because the tests still pass. Meanwhile, production is breaking in ways your mocks will never catch.


This is not a WireMock problem. It is a hand-written mock problem. Any tool that requires a developer to manually craft JSON responses will drift from reality the moment the real API changes.


This post compares three tools head-to-head for enterprise Java API mocking:[WireMock](https://wiremock.org/) ,[MockServer](https://www.mock-server.com/) , and[proxymock](https://docs.speedscale.com/proxymock/getting-started/quickstart/) . Two of them are hand-written mock tools (very good ones). One takes a fundamentally different approach. We will cover how each works, where each breaks down, and which one fits your situation.


## The Core Problem: Hand-Written Mocks Do Not Scale


Every enterprise Java service starts the same way. A developer writes a Spring Boot controller. They add a WireMock stub for the external API. The test passes. The service ships.


Six months later, the service calls 12 external APIs, two Postgres databases, and a gRPC inventory service. The WireMock stubs file is 2,000 lines of hand-crafted JSON. Nobody remembers whether the stub for the payment API reflects the real response format or what someone guessed it looked like in July.


This is the[mock drift problem](https://speedscale.com/blog/api-mocking-tools/) . Hand-written mocks diverge from reality the moment the external API changes. The test still passes, but production breaks.


WireMock, MockServer, and proxymock each approach this problem differently.


## WireMock: The Industry Standard


WireMock is a Java-native HTTP stubbing library. You define request matchers and response templates, and WireMock intercepts HTTP calls and returns your predefined responses.


### How it works


```text
@  RegisterExtension
static   WireMockExtension wm   =   WireMockExtension.  newInstance  ()
.  options  (  wireMockConfig  ().  dynamicPort  ())
.  build  ();


@  Test
void   shouldGetPaymentStatus  () {
wm.  stubFor  (  get  (  urlPathEqualTo  (  "/payments/12345"  ))
.  willReturn  (  okJson  (  """
{
"id": "12345",
"status": "completed",
"amount": 99.99,
"currency": "USD"
}
"""  )));


PaymentStatus result   =   paymentClient.  getStatus  (  "12345"  );
assertEquals  (  "completed"  , result.  getStatus  ());
}
```


This is clean and familiar to any Java developer. WireMock has first-class JUnit 5 integration, supports response templating with Handlebars, and can simulate faults (delays, dropped connections, HTTP errors).


### Where WireMock fits


- **Contract testing.** When you own the API contract and want deterministic tests against a known response shape.
- **Fault injection.** Simulating timeouts, 500 errors, and slow responses. WireMock’s fault simulation is the best of the three tools.
- **JUnit integration.** If your tests are JUnit-based and you want mocks to live in the test class, WireMock is the most ergonomic option.


### Where WireMock breaks down


- **HTTP only.** WireMock does not mock Postgres, Redis, gRPC, or AWS services. If your service calls a database and three HTTP APIs, you need WireMock plus Testcontainers plus a gRPC mock. That is three separate mocking frameworks in one test.
- **Hand-written stubs drift.** The JSON stubs are written by a developer who looked at the API docs once. If the real API adds a field, changes a type from number to string, or starts returning paginated responses, the stub does not update itself.
- **Scale.** A service with 15 external dependencies needs 15 sets of hand-written stubs. Maintaining those stubs across API version changes is a full-time job that nobody wants.


### WireMock Cloud


WireMock also offers a commercial cloud product with team collaboration, SSO, stateful mocking, and Git sync. The free tier gives you 1,000 calls/month and 3 mock APIs. Enterprise pricing is custom.


## MockServer: The Open-Source Alternative


MockServer is a Netty-based HTTP mock and proxy server. Like WireMock, you define expectations (request matchers + response actions) and MockServer returns the matching response.


### How it works


```text
@  RegisterExtension
static   MockServerExtension mockServer   =   new   MockServerExtension  ();


@  Test
void   shouldGetPaymentStatus  (MockServerClient client) {
client.  when  (
request  ()
.  withMethod  (  "GET"  )
.  withPath  (  "/payments/12345"  )
).  respond  (
response  ()
.  withStatusCode  (  200  )
.  withBody  (  json  (  """
{
"id": "12345",
"status": "completed",
"amount": 99.99,
"currency": "USD"
}
"""  ))
);


PaymentStatus result   =   paymentClient.  getStatus  (  "12345"  );
assertEquals  (  "completed"  , result.  getStatus  ());
}
```


If you have used WireMock, this looks almost identical. The API surface is similar. The core difference is in deployment and ecosystem, not in the mocking model.


### Where MockServer fits


- **Kubernetes-native deployment.** MockServer has an official Helm chart (` helm repo add mockserver https://www.mock-server.com` ). If you need a shared mock server running in a cluster for integration tests, MockServer is easier to deploy than WireMock OSS.
- **Request verification.** MockServer can verify that specific requests were made in order, which is useful for testing that your service calls dependencies in the expected sequence.
- **100% free.** No commercial tier, no usage limits. Apache 2.0 licensed.


### Where MockServer breaks down


- **Same hand-written stub problem as WireMock.** You are still writing JSON expectations by hand. Mock drift applies equally.
- **HTTP only.** Same limitation as WireMock. No database, gRPC, or message queue mocking.
- **No commercial support.** There is no company behind MockServer. If you hit a bug in production, you file a GitHub issue and wait. For enterprise teams that need SLAs, this is a risk.
- **Smaller ecosystem.** Fewer tutorials, fewer plugins, fewer StackOverflow answers. When something goes wrong, you are more on your own.


## Proxymock: Record-First Mocking


` proxymock` takes a fundamentally different approach. Instead of writing mock responses by hand, you record real traffic from your running application and replay it as mocks.


### How it works


```text
# Install
brew   install   speedscale/tap/proxymock


# Record: proxymock wraps your app and captures all traffic
proxymock   record   --   java   -jar   myapp.jar


# Exercise the app (manually, with curl, or via test suite)
curl   http://localhost:4143/payments/12345


# Stop recording (Ctrl+C), then replay as mocks
proxymock   mock   --   java   -jar   myapp.jar    # app talks to proxymock instead of real APIs
```


There is no JSON to write.` proxymock` captures the actual HTTP requests, responses, gRPC calls, and database queries your application makes, then serves those recorded responses back when you replay. The mock data is stored as human-readable Markdown files you can inspect and edit.


### Where proxymock fits


- **Multi-protocol.**` proxymock` mocks HTTP, gRPC, PostgreSQL, MySQL, Kafka, AMQP (RabbitMQ), Google Pub/Sub, and AWS services (DynamoDB, S3, SQS, SNS, Kinesis) from a single recording. If your Java service calls a REST API, queries Postgres, publishes to Kafka, and reads from DynamoDB, one proxymock recording covers all four. For non-HTTP protocols, it uses a SOCKS5 proxy or reverse proxy mode — no SDK-specific integrations needed.
- **No mock drift.** The mocks are recorded from the real dependency, so they reflect the actual response format, field types, and edge cases. When the dependency changes, you re-record.
- **AI-generated code.** When an AI agent writes a new controller, it has never seen the real API responses.` proxymock` recordings give the AI (and your tests) actual production context. The built-in MCP server (` proxymock mcp install` ) exposes recording, mocking, and replay tools directly to coding agents like[Cursor](https://www.cursor.com/) ,[Claude Code](https://docs.anthropic.com/en/docs/claude-code) ,[VS Code Copilot](https://code.visualstudio.com/docs/copilot/overview) , and[GitHub Copilot](https://github.com/features/copilot) . An AI agent can start a recording, exercise your app, and create mocks without you leaving the IDE. Speedscale’s Smart Mock agent goes further — it analyzes recorded traffic for dynamic patterns (rotating tokens, timestamps, session IDs) and iteratively builds transform chains to keep mocks accurate across runs.
- **Built-in load testing.** The same recorded traffic doubles as a load test. Run` proxymock replay --vus 50 --for 5m` to hit your service with 50 virtual users for 5 minutes, then assert on results with` --fail-if "latency.p99 > 500"` or` --fail-if "requests.failed != 0"` . Neither WireMock nor MockServer does this — you would need a separate tool like Gatling or k6.
- **Fast onboarding.** For a service with 10+ external dependencies, recording takes minutes. Hand-writing WireMock stubs for the same service takes days.


### Where proxymock breaks down


- **Mocks reflect reality, not imagination.** Recorded responses depend on what actually happened during the recording session. If the external API returned paginated data or an unexpected field, the mock will too. ProxyMock uses signature-based matching (hashing request method, path, and key parameters) to serve the right response, and cycles through multiple responses when the same endpoint was called repeatedly. You can edit the Markdown recordings to pin specific values, but it requires a different mindset than WireMock’s “I control every byte of the response” approach. For requests that were never recorded, ProxyMock passes through to the real endpoint rather than failing — useful during development, but something to be aware of in CI.
- **Different approach to fault injection.** WireMock gives you granular control over synthetic faults: specific timeouts, partial responses, and connection drops per endpoint. Speedscale takes an API-level chaos engineering approach — injecting random slowdowns on individual transactions, occasional errors, and status code manipulation across your traffic. This is useful for resilience testing at scale, but less precise than WireMock when you need to test how your code handles one specific 503 from one specific endpoint.
- **Newer tool.** Smaller community than WireMock. Fewer blog posts, fewer StackOverflow answers. The docs are good but the ecosystem is still growing.


### Pricing


The proxymock CLI is free with unlimited local use. The Speedscale platform (cloud replay, CI/CD integration, data redaction, SSO) requires a paid tier.[Contact sales](https://speedscale.com/company/demo/) for pricing.


## Head-to-Head Comparison


Dimension WireMock MockServer Proxymock


**Mock creation** Hand-written JSON/Java DSL Hand-written JSON/Java client Recorded from live traffic


**Protocols** HTTP/HTTPS only HTTP/HTTPS only HTTP, gRPC, Postgres, MySQL, Kafka, AMQP, AWS, GraphQL


**JUnit integration** First-class (` @RegisterExtension` ) First-class (` @RegisterExtension` ) CLI-based (wrap test execution)


**Fault injection** Excellent (delays, drops, errors) Good (delays, errors) API-level chaos (random latency, errors, status codes)


**Kubernetes** Docker image, Cloud Runner Official Helm chart CLI for local, Speedscale operator for K8s


**Load testing** No No Built-in (VUs, duration, percentile assertions)


**Mock drift risk** High (stubs are static) High (expectations are static) Low (re-record to update)


**AI/MCP integration** WireMock AI (Cloud only) None Native MCP server for AI agents


**Price (OSS)** Free (Apache 2.0) Free (Apache 2.0) Free (CLI, unlimited local)


**Price (Enterprise)** WireMock Cloud (custom) N/A (no commercial tier) Speedscale platform (custom)


**Community size** Large Medium Growing


## When to Use Each Tool


**Use WireMock when:**


- You need deterministic, hand-crafted responses for contract tests
- Your mocking needs are HTTP-only
- You want fault injection (timeouts, connection drops, partial responses)
- Your team already knows the WireMock API


**Use MockServer when:**


- You need a free, Kubernetes-native mock server
- Request verification (asserting call order) is important
- You do not need commercial support or SLAs
- Your mocking needs are HTTP-only


**Use proxymock when:**


- Your service calls multiple protocols (HTTP + Postgres + gRPC + Kafka)
- You are tired of maintaining hand-written stubs that drift from reality
- AI coding agents are generating code that needs to be[tested against real API responses](https://speedscale.com/blog/5-tips-for-agent-to-model-mocking/)
- You want to stand up mocks for 10+ dependencies in minutes, not days
- You need load testing with the same traffic data you use for mocking (no separate Gatling or k6 setup)


## Combining the Tools


These are not mutually exclusive. A practical setup for an enterprise Java team might look like:


- **WireMock** for contract tests where you need exact control over specific edge cases (malformed responses, timeouts, rate limiting)
- **Proxymock** for integration and regression tests where you need realistic mocks of all your dependencies at once


You do not need to pick one and standardize on it. Use WireMock where precision matters and proxymock where realism and coverage matter.


## Getting Started


**WireMock:** Add` com.github.tomakehurst:wiremock-jre8-standalone` to your` pom.xml` and follow the[WireMock docs](https://wiremock.org/docs/getting-started/) .


**MockServer:** Add` org.mock-server:mockserver-netty` to your` pom.xml` and follow the[MockServer docs](https://www.mock-server.com/mock_server/getting_started.html) .


**Proxymock:** Install with` brew install speedscale/tap/proxymock` and follow the[quickstart](https://docs.speedscale.com/proxymock/getting-started/quickstart/) .


For enterprise teams evaluating all three,[book a demo](https://speedscale.com/company/demo/) to see how Speedscale’s platform extends proxymock with cloud replay,[CI/CD integration](https://speedscale.com/blog/automating-api-mocks-in-your-ci-pipeline-with-proxymock/) , and data redaction for production traffic.


## Frequently Asked Questions


### Can I migrate from WireMock to proxymock?


You do not need to migrate all at once. Keep WireMock for your existing contract tests and add proxymock for integration tests, regression tests, and AI-generated code validation. If you do want to bring existing stubs over, Speedscale can import WireMock mappings directly with` speedctl import wiremock` . You can also generate proxymock mocks from an OpenAPI spec with` proxymock generate api-spec.yaml` — useful when you have a spec but no recorded traffic yet. Over time, most teams find they need fewer hand-written WireMock stubs because the recorded mocks cover more ground.


### Does proxymock work with Spring Boot?


Yes.` proxymock` wraps your JVM process and captures all outbound traffic regardless of framework. It works with Spring Boot, Quarkus, Micronaut, Dropwizard, and plain Java. See our[Spring Boot API testing guide](https://speedscale.com/blog/spring-boot-api-testing/) for a full walkthrough.


### How do I handle sensitive data in recorded traffic?


All recorded traffic stays local by default — nothing is sent to Speedscale unless you explicitly push it. For teams that need enterprise-grade data protection, Speedscale’s DLP engine automatically discovers 30+ sensitive data patterns (SSNs, credit cards with Luhn validation, JWTs, IP addresses, email addresses, and more) and replaces them with` REDACTED-` tokens before data leaves your infrastructure. The system can then generate realistic replacement data matching the original formats for test use. You can configure redaction patterns through the CLI or the Speedscale dashboard, and the approach is designed for GDPR, HIPAA, and PCI DSS compliance.


### What about Testcontainers?


Testcontainers solves a different problem: running real databases and services in Docker during tests. It pairs well with all three mocking tools. Use Testcontainers for services you can run locally (Postgres, Redis, Kafka) and use WireMock/MockServer/proxymock for services you cannot run locally (third-party APIs, production databases with real data).
