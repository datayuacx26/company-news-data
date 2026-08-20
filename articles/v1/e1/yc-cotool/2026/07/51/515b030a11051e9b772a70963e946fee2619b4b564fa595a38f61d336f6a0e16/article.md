---
schema_version: "1.0.0"
document_id: "515b030a11051e9b772a70963e946fee2619b4b564fa595a38f61d336f6a0e16"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-db39e53ee409"
canonical_url: "https://www.cotool.ai/blog/introducing-cotool-router"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-29T20:17:37.490945+00:00"
fetched_at: "2026-07-29T20:17:38.438130+00:00"
content_hash: "sha256:0c3fd55cade3088f0fe5e81d7e1d91869b746ea80d7410473792a863c63ae8b9"
---

# Introducing Cotool Router

# Introducing Cotool Router


Cotool Router gives security teams automatic cross-provider fallback, access to open-weight models, and evaluation-informed model selection for any security task.


Company


July 29, 2026


Max Pollard


CEO


[Download Router Flow Animation.mp4](https://www.cotool.ai/api/media/file/Router%2520Flow%2520Animation.mp4)


Earlier this month, an OpenAI agent autonomously broke out of its sandbox and gained unauthorized access to Hugging Face's internal datasets and service credentials. When responders tried to investigate, commercial frontier models refused to help.


The refusals stem from an issue security teams know all too well: false positives. Proprietary model providers don't want bad actors abusing their services, so they use guardrails to refuse requests that appear potentially malicious. The problem is that offensive and defensive cyber work can look nearly identical to a model. For Hugging Face, the same safety controls that were meant to stop attackers from weaponizing frontier models stopped them from investigating the breach, putting them at a massive disadvantage.


***Today, we're introducing Cotool Router, a routing layer designed specifically for security agents, so that a refusal never stalls an investigation.***


Router allows organizations to see 100% reliability for their defensive security workloads by making any agent request on the platform resilient to upstream risks such as cyber refusals, provider downtime, model deprecations, and more.


Security operations should not stop because a model provider changes its policies, infra goes offline, or an organization hits their quota. Cotool Router lets teams select the model for each agent, govern defaults centrally, and automatically reroute work across providers (including to open-weight models) when the primary path fails. Customers using Cotool Router today are seeing 100% task completion across routed defensive security workloads.


## Providers are refusing security work


If your AI security operations depend on permission from another company's imperfect classifiers, you have a problem. Hugging Face's incident report shows what this looks like in production.


The team needed to analyze more than 17,000 recorded attacker events, including exploit payloads and command-and-control artifacts. It initially used frontier models through commercial APIs, but provider safety systems blocked the requests.


Hugging Face described the problem in its[incident disclosure](https://huggingface.co/blog/security-incident-july-2026) :


> *When we started the log analysis, we first used frontier models behind commercial APIs. This did not work: the analysis requires submitting large volumes of real attack commands, exploit payloads, and C2 artifacts, and these requests were blocked by the providers' safety guardrails, which cannot distinguish an incident responder from an attacker.*


Cotool has seen cyber refusals block real defensive work firsthand. We started seeing cyber-refusals online across our customers' trajectories months ago. Our data shows refusals becoming more frequent as more capable models are released and guardrails tighten. Even offline when evaluating Claude Fable 5 against 40 tasks from a real[Windows intrusion](https://www.cotool.ai/research/windows-enterprise-intrusion) dataset, Anthropic’s guardrails blocked every request. These observations are what kicked off our work on the Cotool Router; the Hugging Face incident only validated our approach.


There is little reason to expect this dependency to disappear with future frontier releases. As models become capable of more consequential cyber work, providers are likely to gate access even further.[Trusted-access programs](https://openai.com/index/trusted-access-for-cyber/) may reduce refusals for approved customers, but they remain provider-controlled exceptions that can change with the next model, company policy update, or government intervention.


Hugging Face is fortunate that it is an open-source model provider itself, and was able to work around the problem by improvising and using GLM-5.2 in the middle of the incident. Other security teams without model-agnostic infrastructure may not be as lucky.


## Security teams need model choice


Refusals make the risks of depending entirely on frontier model providers obvious. Their policies determine which defensive work can run, while defaulting every alert, investigation, and scheduled hunt to their most expensive models drives up cost without guaranteeing the best result. Security agents span detection engineering, malware analysis, threat hunting, and incident response, and model performance and cost vary across those tasks.


Today, security teams have three imperfect options. They can use traditional AI SOC platforms, where model choice is completely opaque. They can build on a provider-specific harness such as Codex or Claude Code, tying their workflows to one model family, its guardrails, and its availability. Or they can build a multi-provider harness internally, taking on provider integration, model normalization, evaluation, routing, and failover themselves.


Cotool Router gives security teams the flexibility of a multi-provider harness without having to build and operate one.


## How Cotool Router Works


Cotool Router makes model choice and provider resilience part of every agent's configuration, before an incident begins. It combines automatic cross-provider fallback, a broader provider pool, and evaluation-based model selection.


### Automatic cross-provider fallback


Cotool Router automatically switches providers when a model refuses a request or becomes unavailable.


When an eligible call fails, Cotool examines the complete error chain and classifies the failure mode. Availability failures include quota exhaustion, retryable API errors, disabled or missing models, and common authentication, timeout, and endpoint errors.


The following agentic turns are then intelligently routed to the best substitute, which may be the same exact model from a different provider or a model of the same class from a different provider entirely. The policy prevents a refused request from being retried through another model subject to the same provider-specific failures.


*How Cotool Router routes a failed model call to another provider.*


Fallback occurs without any visible interruption to the agent or user’s task. Cotool discards any incomplete streamed output, retries the same turn with the next provider, and attributes usage to the model that completed the work.


### Access to open-weight models


Open-weight models give security teams another option when proprietary models refuse the work. Cotool's Router launch set includes GLM 5.2, DeepSeek V4 Pro and Flash, Kimi K3, Kimi K2.7 Code, Kimi K2.6, MiniMax M3, and Qwen3.7 Plus.


Cotool serves its open-weight models through US-based inference providers without any operational overhead by normalizing differences in tool calling, sampling behavior, prompt formatting, and usage reporting.


Open-weight models are already competitive primary options for some security workflows. GLM 5.2 placed fifth among 22 models on our Windows intrusion evaluation with a 72.5% overall score, less than two-tenths of a point behind GPT-5.5.


Enterprises looking to add open-weight models to their ensemble natively face several challenges.


First, frontier open-weight models have largely come out of China, and security teams likely cannot send data to Chinese datacenters for a variety of reasons.
Second, hosting through existing hyperscaler providers puts you behind the frontier. As of July 26, 2026, 40 days after[Z.ai released GLM-5.2](https://z.ai/blog/glm-5.2) , Amazon[Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/model-cards-zai.html) and Google[Vertex](https://cloud.google.com/vertex-ai/generative-ai/docs/maas/use-open-models) have yet to add support for it.


Lastly, self-hosting inference is costly: at current[Lambda on-demand prices](https://lambda.ai/pricing) , keeping a single eight-GPU H100 node online costs roughly $280,000 per year before redundancy, monitoring, and maintenance.
The only remaining option is serverless model inference APIs, and existing providers vary in capability, as some cut costs by serving lower precision model weights or larger batch sizes at the expense of accuracy and performance.


Cotool Router handles all of this complexity automatically, providing instant access to open-weight models in the same model picker, agent loop, and routing system as their existing OpenAI, Anthropic, and Google options, without operating another inference platform.


### Evaluation-based model selection


Cotool’s security evaluations help teams select the best model for each agent from a broad provider pool. Teams can choose any supported model for each agent or let Cotool select the model automatically. The goal is to provide maximum flexibility and allow teams to adapt as models and tasks change.


That flexibility matters because the same model is not best for every security task. Across[seven published evaluations](https://www.cotool.ai/research) spanning more than 13,000 questions and tasks, we have measured substantial differences across cybersecurity knowledge, detection engineering, malware analysis, threat hunting, and incident response. Those results inform Cotool’s defaults and help teams select models for their own workflows.


The cost of getting model choice wrong for a given task can be high. In our[Windows Enterprise Intrusion](https://www.cotool.ai/research/windows-enterprise-intrusion) evaluation, GPT-5.6 Sol earned the highest overall score at 79.2% for $1.22 per task. GPT-5.5 scored 72.7% at $4.62 per task. On the same workload, the worse-performing route cost 3.8 times more.


*The wrong model can cost nearly four times more and perform worse on the same security workload.*


Cost is only one dimension. Model ranks can vary across different task tracks, for example GPT 5.5 showing stronger performance in incident response but weaker performance in threat hunting.


*Model rank across four investigation tracks on BlueBench-Intrusion-002*


That is why Cotool treats model selection as a property of each agent's task, set by evaluation rather than a global preference.


### Toward Task-aware routing


Today in Cotool, teams choose the model for each agent, informed by our security-specific evaluations. During execution, the router responds to explicit provider failures with a quality-first fallback from another provider.


The next step is to automate more of that configured decision. Cotool already knows an agent's system prompt, tools, objective, and expected output before a run begins. We are building toward routing by task category, complexity, and organizational context. This foundation is then combined with evaluation history, organization policy, provider health, cost, and latency to select the right approved model for the work.


A detection engineering agent, a scheduled threat hunt, and an incident response agent may each benefit from a different model, even when they work from the same telemetry. Task-aware routing will let Cotool make more of those choices automatically, while cross-provider fallback ensures reliability when existing providers fail.


Cotool Router is available today.[Book a demo](https://www.cotool.ai/request-demo) to see it in action.
