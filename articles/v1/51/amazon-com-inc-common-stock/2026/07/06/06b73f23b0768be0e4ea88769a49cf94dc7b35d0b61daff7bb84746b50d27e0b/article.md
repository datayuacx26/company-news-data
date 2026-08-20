---
schema_version: "1.0.0"
document_id: "06b73f23b0768be0e4ea88769a49cf94dc7b35d0b61daff7bb84746b50d27e0b"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/lambdadf-dotnet/"
published_at: "2026-07-23T18:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:b659e2000e5b05fc42fdaaed3a38f2b3983ed9825c496ab13bf46e219e650649"
---

# AWS Lambda durable execution SDK for .NET is now generally available

Today, AWS announces the general availability of the AWS Lambda Durable Execution SDK for .NET, empowering C# developers to build resilient, long-running workflows using Lambda durable functions. With this SDK, developers can create multi-step applications like payment processing pipelines, AI agent orchestration, and human-in-the-loop approvals directly in their applications without implementing custom progress tracking or integrating external orchestration services.


Lambda durable functions extend Lambda's event-driven programming model with operations that checkpoint progress automatically and pause execution for up to a year when waiting on external events. The AWS Lambda Durable Execution SDK for .NET provides an idiomatic C# experience for building with Lambda durable functions. It includes steps for progress tracking, callback integration for human and agent-in-the-loop workflows, durable invocation for reliable function chaining, and waits for efficient suspension. The SDK installs from NuGet into the .NET toolchain you use today. The[local testing emulator](https://www.nuget.org/packages/Amazon.Lambda.DurableExecution.Testing/) in the SDK enables developers to build and debug locally before deploying to production.


To get started, see the[Lambda durable functions developer guide](https://docs.aws.amazon.com/lambda/latest/dg/durable-functions.html) and the[AWS Lambda Durable Execution SDK](https://www.nuget.org/packages/Amazon.Lambda.DurableExecution) for .NET on NuGet. For Regional availability and pricing details, see the[AWS Regional Services List](https://builder.aws.com/build/capabilities) and[AWS Lambda Pricing.](https://aws.amazon.com/lambda/pricing/)
