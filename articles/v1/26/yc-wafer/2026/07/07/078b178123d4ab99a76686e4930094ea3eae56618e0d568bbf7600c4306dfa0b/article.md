---
schema_version: "1.0.0"
document_id: "078b178123d4ab99a76686e4930094ea3eae56618e0d568bbf7600c4306dfa0b"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/wafer-integration-with-truefoundry-ai-gateway"
published_at: "2026-07-17T12:00:00+00:00"
first_seen_at: "2026-07-24T07:20:17.844382+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:f4df22318cde93e4872e100fc0b95725968d7c6697d232a81adc736652101120"
---

# Wafer integration with TrueFoundry AI Gateway

> *This article is cross-posted from TrueFoundry’s official blog. Read the original:[Wafer integration with TrueFoundry AI Gateway](https://www.truefoundry.com/blog/wafer-integration-with-truefoundry-ai-gateway) .*


New inference providers for open-weight models seem to launch every few weeks, each promising faster tokens per second on the same handful of models everyone already runs. The problem for engineering teams is rarely whether a provider is fast enough. It is what happens after: a new base URL, a new API key, a new observability wire-up, and a set of application code paths that now need testing against yet another schema. Wafer’s arrival inside the TrueFoundry AI Gateway removes that second problem, leaving only the first question: is the model fast enough for the workload.


## TrueFoundry AI Gateway: One Execution Layer for Every Model


The TrueFoundry AI Gateway sits between applications and every model they call, exposing a single OpenAI-compatible interface regardless of which provider is actually serving the request underneath. Instead of an application holding a different client, a different key, and a different retry policy for each provider, it sends every request to one endpoint and lets the Gateway resolve where that request should actually go.


That resolution happens through what TrueFoundry calls Virtual Models: logical model identifiers that map to one or more physical providers, with routing decided by priority, weight, or latency. New providers do not require new integration code on the application side. They are added as a provider account and a set of registered models, and every application already pointed at the Gateway can start using them the moment they are turned on.


## Wafer: Serverless Speed for Open-Weight Models


Wafer is a hosted inference provider built specifically around fast serving of open-weight models. Its pitch is not a broader model catalog but faster execution of the models teams already want to run, achieved by optimizing the full inference stack for specific hardware rather than serving models on a generic stack. Through TrueFoundry, that speed is available for GLM 5.2, served as a serverless chat completion with streaming and tool calling.


Wafer also treats data handling as a first-class option rather than an enterprise add-on. Requests can be marked for zero data retention on a per-request basis, and Wafer isolates traffic and offers SLA-backed uptime for workloads that need those guarantees, without requiring a separate contract tier just to turn the option on.


## One Gateway, Every Model: Wafer Inside TrueFoundry


Connecting the two is a matter of registering Wafer as a provider account inside the Gateway’s Models section. The account form asks for a Base URL, defaulting to Wafer’s own` pass.wafer.ai/v1` endpoint, an API key, and a toggle for zero data retention, which is enabled by default. Turning it on tells the Gateway to attach a` Wafer-ZDR: required` header on every request sent to Wafer, so the retention behavior is enforced at the request level rather than left to account-wide configuration.


Once the account exists, individual chat models are registered by their Display Name and their exact Wafer Model ID, such as **glm-5.2** . What makes this integration unusually light is that Wafer’s API already speaks the OpenAI chat completions schema natively. The Gateway’s provider adapter, which normally translates requests into a partner’s own format, has nothing to translate here. It passes the request through largely unchanged, which is the same reason existing OpenAI-compatible clients can point at Wafer directly by swapping only the base URL and key.


## How Smart Routing Works


A request that targets a Wafer-backed model moves through the same request path as any other model on the Gateway:


1.


An application calls the Gateway’s chat completions endpoint with a model identifier in the form` your-wafer-account/glm-5.2` , using the standard OpenAI SDK.


2.


The Gateway validates the request’s JWT against cached public keys and checks the caller’s access against an in-memory map of users to models, both without an external call.


3.


The Virtual Model identifier resolves to the registered Wafer provider account, including its Base URL, API key, and zero data retention setting.


4.


The request is forwarded to` pass.wafer.ai/v1/chat/completions` over HTTPS. Because Wafer already uses the OpenAI schema, the Gateway’s provider adapter makes no format changes, only attaching the` Wafer-ZDR` header when retention is set to required.


5.


Wafer runs inference on the requested GLM 5.2 model and streams tokens back as they are generated.


6.


The Gateway streams the response back to the application and asynchronously publishes token counts, latency, and cost to its telemetry pipeline, the same as it would for any other provider.


None of these steps are unique to Wafer. That is the point: a new, fast inference provider becomes a routing target rather than a new integration surface, which is what lets a Gateway account absorb new providers as they prove themselves without every application needing a code change.


## Get Started with Faster Open-Model Inference


From the TrueFoundry dashboard, open AI Gateway, then Models, and select Wafer to add an account and register your chat models. Call them through the Playground or directly via the OpenAI-compatible client, with the model set to your account name and model ID. Full setup steps and code examples are in the[TrueFoundry AI Gateway documentation for Wafer](https://www.truefoundry.com/docs/ai-gateway/wafer#wafer) , alongside[Wafer’s own router setup guide](https://docs.wafer.ai/serverless/router-setup#truefoundry) .
