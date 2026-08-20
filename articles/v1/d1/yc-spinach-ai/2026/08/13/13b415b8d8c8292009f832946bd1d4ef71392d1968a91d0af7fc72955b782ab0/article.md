---
schema_version: "1.0.0"
document_id: "13b415b8d8c8292009f832946bd1d4ef71392d1968a91d0af7fc72955b782ab0"
company_key: "yc-spinach-ai"
company: "Spinach AI"
source_id: "yc-spinach-ai-rss-876a127397a2"
canonical_url: "https://www.spinach.ai/blog/ai-meeting-tool-zero-data-retention-policy"
published_at: "2026-08-08T12:54:08+00:00"
first_seen_at: "2026-08-11T16:08:21.519453+00:00"
fetched_at: "2026-08-11T16:08:22.287714+00:00"
content_hash: "sha256:2e1577d1db5a1d4bfa56d03dbdaaebb55e7b19e60be5b602112bc53be48208f4"
---

# Zero Data Retention in AI Meeting Tools (August 2026)

Procurement teams are getting sharper about AI data handling, and zero data retention is showing up on security questionnaires more than ever. But the term gets used loosely, and for AI meeting tools where sensitive conversations run through LLMs daily, loose is a liability. The specifics of how ZDR actually works, and where it stops working, are worth understanding before your next vendor review.


**TLDR:**


- ZDR means an AI provider discards your prompt and response after processing: no logs, no storage, no training data.
- Training opt-out and ZDR are separate controls; opting out of model training does not stop a provider from storing your inputs.
- As of 2026, Anthropic’s extended thinking models (including Claude 3.7 Sonnet) are excluded from its standard ZDR agreement, with data retained up to 30 days.
- ZDR covers the LLM provider’s infrastructure only; your meeting tool’s own storage layer is a separate question you must ask separately.
- Spinach AI holds ZDR agreements with OpenAI, Anthropic, and Google; Spinach does not route data through Anthropic’s extended thinking endpoints, which are excluded from standard ZDR, and no customer data is stored after processing or used to train models.


## What Zero Data Retention (ZDR) Means


Zero data retention (ZDR) means an AI provider processes your prompt and returns a response without writing either to persistent storage. Once the inference call completes, the input and output are discarded. The provider keeps no log of what was sent or returned, and cannot retrieve, audit, or use that data in model training afterward.


That distinction matters because standard API traffic typically carries an abuse-monitoring retention window of up to 30 days (exact duration varies by provider), during which inputs and outputs are stored and may be reviewed for safety purposes. ZDR is a contractual agreement that eliminates that window entirely, dropping retention at the API layer to zero.


Many organizations assume opting out of model training provides the same control. It does not, and that gap has real consequences for enterprise data governance.


## ZDR vs. Training Opt-Out: Two Separate Controls


Zero data retention and training opt-out are related but distinct controls, and confusing them is one of the more common procurement mistakes enterprise buyers make.


Training opt-out means the provider agrees not to use your inputs to improve its models. ZDR goes further: it means the provider does not persist your data on its infrastructure at all after returning a response. You can have one without the other. An API call under a training opt-out agreement may still be logged, cached, or stored temporarily for abuse monitoring. Under a true ZDR agreement, that intermediate storage is eliminated.


Most enterprise AI providers offer these as separate contract terms, sometimes on different pricing tiers. If you’re reviewing[AI meeting notes tools](https://www.spinach.ai/blog/ai-meeting-notes) , these distinctions matter before signing any vendor agreement.


Control


What it covers


What it does not cover


Training opt-out


Excludes your data from model training runs


Does not prevent temporary storage or logging


Zero data retention


No persistence after the API response is returned


Does not govern how your own systems store the output


Both combined


Strongest posture for sensitive data


Still requires your own retention and access controls


When reviewing a ZDR agreement from any provider, confirm whether the ZDR commitment covers abuse-monitoring logs, intermediary inference infrastructure, and any subprocessors the provider routes requests through. A ZDR policy that excludes subprocessors leaves a meaningful gap for enterprise buyers in compliance-sensitive industries.


## Why Enterprise Buyers Now Require ZDR


Regulatory pressure and vendor risk have pushed zero data retention from a “nice to have” into a hard procurement requirement. Incidents where AI providers retained and reviewed customer inputs to improve their models exposed enterprises to data breach liability, IP leakage, and audit failures, all without the customer’s knowledge.


Three drivers now appear on nearly every enterprise security questionnaire:


- [AI transcription tools](https://www.spinach.ai/blog/ai-transcription-tools) that handle sensitive audio must satisfy compliance mandates like HIPAA, SOC 2 Type II, and GDPR, which require that sensitive data not persist beyond the transaction that requires it, and auditors increasingly ask for ZDR agreements as documented proof.
- IP and confidentiality risk is highest in meeting contexts, where engineers, lawyers, and executives routinely discuss unreleased products, deal terms, and litigation strategy. These are inputs that should never sit in an LLM provider’s logs.
- Cyber insurance underwriters and procurement teams treat the absence of a ZDR agreement as an unmitigated third-party data exposure, which can block deals or trigger policy exclusions.


For AI meeting tools, the exposure surface is wide. Every transcript processed by an[AI meeting tool](https://www.spinach.ai/blog/best-ai-note-taker) for summarization or action-item extraction is a potential retention event unless the vendor has a signed ZDR agreement with that provider.


## What ZDR Covers and What It Does Not


Zero data retention agreements with LLM providers cover one specific thing: whether the provider stores your prompt data on their servers after processing your request.


Under a ZDR agreement, the provider processes the input and returns a response without logging, storing, or using that data for model training. What ZDR does not cover is everything else in the pipeline: your internal data warehouse, your meeting tool’s own storage layer, your CRM, or any intermediary service sitting between your application and the LLM API.


Here is where enterprise buyers often get tripped up:


- ZDR applies to the LLM provider’s infrastructure, not to the tool calling the API. A meeting assistant can hold a ZDR agreement with OpenAI or Anthropic and still retain your transcript data indefinitely on its own servers.
- ZDR does not equal HIPAA compliance. For[AI tools for remote teams](https://www.spinach.ai/blog/ai-tools-for-remote-teams) , covered entities still need a Business Associate Agreement with every vendor in the data chain, and ZDR alone does not satisfy that requirement.
- ZDR policies vary by endpoint. OpenAI’s zero data retention is available via API but not through ChatGPT’s consumer interface. Azure OpenAI offers ZDR through specific contractual arrangements, not by default.
- Model training opt-outs and ZDR are separate controls. Some providers stop training on your data without a full ZDR agreement; others bundle both. Check which applies before assuming coverage.


The practical implication: ask every vendor in your stack, including your LLM provider and every layer above it, what data they retain, for how long, and under what conditions.


## How Major AI Providers Implement ZDR


Each major AI provider structures zero data retention differently, so knowing the specifics matters when you’re vetting vendors for enterprise deployment.


### OpenAI


OpenAI offers a zero data retention agreement through its API, meaning inputs and outputs are not stored beyond the immediate request. This applies to the API tier, not to ChatGPT consumer products by default. Enterprises can[request ZDR for eligible endpoints](https://openai.com/enterprise-privacy/) through OpenAI’s enterprise privacy program.


### Anthropic / Claude


Anthropic’s zero data retention agreement covers Claude API usage. Prompts and completions are not logged or used for training when ZDR is active.


### Google Gemini


Gemini’s zero data retention applies to API access through Google Cloud. Consumer Gemini products operate under separate, less restrictive data terms.


### Azure OpenAI


Microsoft’s Azure OpenAI service offers zero data retention as a configurable option, often the preferred path for enterprises already in the Microsoft ecosystem with existing compliance frameworks.


### AWS Bedrock


AWS Bedrock offers ZDR-eligible inference for many models; confirm ZDR status per model provider before assuming default coverage, as the posture varies depending on which model you are calling.


Provider


ZDR Available


Scope


OpenAI API


Yes


API tier only


Anthropic Claude API


Yes


API tier only


Google Gemini API


Yes


Google Cloud API only


Azure OpenAI


Yes


Configurable per deployment


AWS Bedrock


Yes


Default on inference API


Consumer-facing products from each of these providers operate under different terms. ZDR protections apply at the API layer, which is where enterprise AI meeting tools connect.


## The 2026 Anthropic Model Exception ZDR Organizations Must Understand


In early 2026, Anthropic introduced a model-specific carve-out that catches enterprise buyers off guard: Claude’s extended thinking models, including Claude 3.7 Sonnet with extended thinking active, are explicitly excluded from Anthropic’s standard zero data retention agreement. When your organization signs an anthropic zero data retention agreement expecting blanket ZDR coverage, these models fall outside that protection by default.


The practical consequence is straightforward. If a developer on your team routes meeting summaries or conversation data through Claude’s extended thinking endpoint, that data may be retained by Anthropic for up to 30 days ([Anthropic’s covered models data retention policy](https://support.claude.com/en/articles/15425996-data-retention-practices-for-covered-models) ), regardless of your ZDR agreement status. The exception is easy to miss during procurement.


Enterprise buyers reviewing AI meeting tools that run on Anthropic’s API should ask vendors two specific questions: which Claude models does your product call, and are any of those models excluded from your ZDR coverage? This distinction matters in any[Spinach AI vs Fireflies comparison](https://www.spinach.ai/blog/spinach-ai-vs-fireflies) where both tools rely on LLM APIs. A vendor running Claude 3.7 Sonnet with extended thinking for summarization is not covered under a standard anthropic zero data retention agreement, and the contractual gap is yours to own if it’s not surfaced before deployment.


## ZDR and Industry Compliance Requirements


Zero data retention matters most where the cost of a breach is highest. In healthcare, finance, and legal services, the question goes beyond whether your AI meeting tool transcribes accurately. The deeper issue is whether the LLM processing that transcript ever stores it.


HIPAA requires covered entities to sign a BAA with any vendor handling PHI. This applies equally to[Zoom AI meeting notes](https://www.spinach.ai/blog/zoom-ai-meeting-notes-reviews-alternatives) and any other meeting tool that routes audio or text through an LLM without a ZDR agreement in place. That LLM provider may become an unintended business associate. Most enterprise buyers don’t catch this gap until a security review flags it.


ZDR closes it by confirming no PHI persists at the inference layer.


## Why AI Meeting Tools Raise the Stakes for ZDR


Enterprise conversation intelligence platforms occupy a uniquely sensitive position in enterprise data flows. Every conversation captured by a platform like Spinach AI, or any of the leading[Otter.ai alternatives](https://www.spinach.ai/blog/otter-ai-alternatives) , passes through an LLM to produce structured outputs: decisions, action items with named owners, summaries, and records routed into downstream systems. That means confidential strategy sessions, personnel discussions, M&A conversations, and customer calls all touch an AI provider’s infrastructure.


Most enterprises have existing controls around email and document storage. Conversation data is newer as a governed asset, and the audit trails are thinner. ZDR agreements with LLM providers close that gap by confirming that conversation data used for inference is never logged or retained on the provider’s side.


## What Enterprise Buyers Should Demand from AI Meeting Tools on ZDR


When vetting any AI meeting tool during a security review, ask for specific written answers to each of these before signing:


- Does the vendor hold ZDR agreements with every LLM provider it routes data to, or only selected ones?
- Are those agreements documented and accessible on the vendor’s trust center?
- How does the vendor handle model-specific ZDR exceptions, such as Anthropic’s extended thinking carve-out for covered models?
- Does ZDR coverage apply to the transcription step and the summarization step, or only one of them? The answer varies widely across vendors, as a[Spinach AI vs Fathom](https://www.spinach.ai/blog/spinach-ai-vs-fathom) comparison shows.
- What data does the meeting tool itself retain at the application layer, separate from the LLM provider question?
- Can transcript, summary, and video retention each be configured separately, and at what plan tier?
- Is a BAA available for healthcare deployments, and which plan tier requires it?


A vendor that cannot answer all seven in writing has a gap in its security posture, and that gap extends well beyond its documentation.


## Zero Data Retention with LLM Providers: How Spinach AI Is Covered


Spinach AI is an enterprise conversation intelligence platform, the system of record for conversation data across the organization. It holds zero data retention agreements with OpenAI, Anthropic, and Google. Spinach does not route data through Anthropic’s extended thinking endpoints, which are excluded from standard ZDR coverage. When conversation data is processed by those providers to generate structured outputs, such as decisions, action items with named owners, summaries, and downstream CRM or project management records, neither inputs nor outputs are stored after processing. No customer data trains AI models.


At the application layer, retention is configurable per data type on Enterprise. Transcript, summary, and video can each be set independently, from one week to indefinite, so organizations can apply aggressive deletion for EU employees without forcing that same policy onto compliance records requiring multi-year retention.


A BAA is available on Enterprise and HIPAA engagements. Spinach is SOC 2 Type II, GDPR, and HIPAA compliant. Full trust documentation, including the subprocessor list and DPA, is at[trust.spinach.ai](https://trust.spinach.ai/) .


## Final Thoughts on Zero Data Retention in AI Meeting Tools


ZDR does not protect your data everywhere, but it does close the specific gap at the LLM inference layer, which is exactly where meeting content is most exposed. The work on your end is asking the right questions of every vendor in your stack, starting with the one you signed a contract with and extending to every subprocessor beneath it. Get those answers in writing before deployment, not during a security review. Spinach documents ZDR coverage across all three LLM providers it routes data through.


**What is zero data retention, and how does it differ from a training opt-out agreement?**


Zero data retention (ZDR) means an AI provider processes your input and discards it immediately after returning a response — no logging, no temporary storage, no abuse-monitoring window. A training opt-out only prevents your data from being used in model training runs; it does not prevent the provider from storing that data temporarily. Under a true ZDR policy, both the prompt and the response are gone the moment inference completes.


**Which major AI providers offer zero data retention, and does it apply to all their products?**


OpenAI, Anthropic, Google Gemini, Azure OpenAI, and AWS Bedrock all offer zero data retention at the API level — but ZDR protections do not extend to consumer-facing products like ChatGPT’s standard interface or consumer Gemini. OpenAI zero data retention and Anthropic zero data retention each apply only to API traffic, which is where enterprise AI meeting tools connect. Always verify with your vendor which specific endpoints and models fall under their ZDR agreement, since Anthropic’s 2026 carve-out excludes Claude extended thinking models from standard ZDR coverage by default.


**Should I ask an AI meeting tool vendor about its own retention policies, or is a ZDR agreement with OpenAI or Anthropic enough?**


Ask both questions separately, because they cover different parts of the data chain. A vendor can hold a ZDR agreement with OpenAI or Anthropic — meaning the LLM provider discards your data after inference — and still retain your transcript, summary, or video indefinitely on its own servers. The ZDR agreement with the model provider closes the LLM retention gap; your vendor’s application-layer retention policy governs everything else. Spinach AI holds zero data retention agreements with OpenAI, Anthropic, and Google, and on Enterprise, transcript, summary, and video retention can each be configured independently, from one week to indefinite.


**Does zero data retention satisfy HIPAA compliance for AI meeting tools?**


No — ZDR closes the LLM inference retention gap, but it does not satisfy HIPAA on its own. Covered entities still need a Business Associate Agreement with every vendor handling protected health information, and ZDR alone does not constitute a BAA. The two controls address different requirements: ZDR governs whether the model provider stores your data after processing; a BAA governs the vendor’s legal obligations around PHI across the full data chain. For healthcare deployments, confirm both a ZDR agreement and a signed BAA are in place before processing any meeting content through an AI tool.


**What seven questions should enterprise buyers ask any AI meeting tool about its zero data retention policy?**


Ask: (1) Does the vendor hold ZDR agreements with every LLM provider it routes data to, or only selected ones? (2) Are those agreements documented on the vendor’s trust center? (3) How does the vendor handle model-specific ZDR exceptions, such as the Anthropic zero data retention carve-out for Claude extended thinking models? (4) Does ZDR coverage apply to both transcription and summarization steps? (5) What data does the meeting tool retain at its own application layer, separate from the LLM provider question? (6) Can transcript, summary, and video retention each be configured separately, and at what plan tier? (7) Is a BAA available for healthcare deployments, and which tier requires it? A vendor that cannot answer all seven in writing has a gap in its security posture, not just its documentation.


**What does zero data retention actually mean in plain terms?**


Zero data retention (ZDR) means an AI provider processes your input and immediately discards it — no logs, no stored prompts, no abuse-monitoring window. Once the inference call returns a response, both the input and output are gone from the provider’s infrastructure. This is a contractual agreement, not a default setting, and it applies at the API layer where enterprise tools connect.


**What is the Anthropic extended thinking ZDR carve-out, and why does it matter for AI meeting tools?**


Anthropic’s zero data retention agreement excludes Claude extended thinking models, including Claude 3.7 Sonnet with extended thinking enabled, as of 2026 — meaning data processed through those endpoints may be retained for up to 30 days even when a standard anthropic zero data retention agreement is in place. For AI meeting tools that route summaries or transcripts through Claude’s extended thinking endpoint, that gap belongs to your organization if it goes undetected during procurement. Ask any vendor using Anthropic’s API which specific Claude models they call and whether those models fall outside ZDR coverage.


**Does OpenAI zero data retention apply to ChatGPT, or only the API?**


OpenAI zero data retention applies to the API tier only — not to ChatGPT’s consumer interface, which operates under separate data terms. Enterprise AI meeting tools connect at the API layer, which is where ZDR protections apply. If a vendor references ChatGPT zero data retention without specifying the API context, ask them to confirm which endpoint and interface the agreement actually covers.


**How does Azure OpenAI zero data retention differ from the standard OpenAI API agreement?**


Azure OpenAI zero data retention is available as a configurable option per deployment, often through Microsoft’s enterprise data protection commitments rather than a separate standalone ZDR contract. This makes it the preferred path for organizations already operating inside the Microsoft compliance ecosystem with existing frameworks in place. The coverage is real, but it requires configuration — it is not on by default across all Azure OpenAI deployments.


**What is ZDR, and where does it stop protecting your data?**


ZDR (zero data retention) is a contractual commitment from an AI provider that your prompt and response will not be written to persistent storage after the inference call completes. It stops at the boundary of the LLM provider’s infrastructure — your meeting tool’s own storage layer, your CRM, your data warehouse, and any intermediary services between your application and the model API are entirely separate questions. A vendor can hold a valid ZDR agreement with OpenAI or Anthropic and still retain your transcript indefinitely on its own servers.


**Can I use Google Gemini zero data retention for enterprise AI meeting tools?**


Yes — Google Gemini zero data retention applies to API access through Google Cloud, where enterprise AI meeting tools connect. Consumer Gemini products operate under separate, less restrictive data terms and are not covered. When evaluating a vendor that routes data through Google’s models, confirm that their agreement covers the Google Cloud API specifically, not the consumer product.


**How does AWS Bedrock handle zero data retention compared to other providers?**


AWS Bedrock provides zero data retention by default on its model inference API for most model providers available on the service, meaning prompts and responses are not stored after processing without requiring a separate contractual addendum. This default-on posture differs from providers like OpenAI and Anthropic, where ZDR requires a specific agreement. Organizations already running workloads on AWS often use Bedrock as their ZDR-compliant inference layer for this reason.


**What is the difference between a zero data retention policy and a zero data retention agreement?**


A zero data retention policy is a vendor’s stated internal practice — a published commitment to how they handle data. A zero data retention agreement is a signed contract between your organization and the provider that makes those commitments legally binding and enforceable. For regulated industries, a policy document alone does not satisfy audit requirements; you need a signed agreement you can produce during a security review or compliance audit.


**Is zero data retention enough to meet GDPR requirements for AI tools processing meeting content?**


ZDR helps satisfy GDPR’s data minimization and storage limitation principles by preventing LLM providers from retaining personal data after processing, but it does not cover GDPR compliance on its own. Your meeting tool’s application-layer retention, data processing agreements with all subprocessors, lawful basis for recording and processing, and data subject rights workflows all sit outside ZDR’s scope. GDPR compliance for AI meeting tools requires ZDR as one layer inside a broader governance structure.


**Should I check a vendor’s trust center before signing a ZDR agreement, and what should I look for?**


Yes — a vendor’s trust center should document their ZDR agreements with each LLM provider they route data to, their subprocessor list, and their data processing agreement. Gaps to flag include ZDR coverage that excludes specific models or endpoints, subprocessors that fall outside the ZDR commitment, and policies that describe training opt-outs without confirming full ZDR. Spinach documents ZDR coverage across OpenAI, Anthropic, and Google at trust.spinach.ai, alongside the subprocessor list and DPA.


## What should you do now


Next, here are some things you can do now that you've read this article:


1. Our library of[meeting agenda templates](https://www.spinach.ai/agenda-templates/) is designed to help you run more effective meetings.
2. Check out[Spinach](https://www.spinach.ai/?noredirect) to see how it can help you run a high performing org.
3. If you found this article helpful, please share it with others on[Linkedin](https://www.linkedin.com/shareArticle?mini=true&url=https://www.spinach.ai/blog/ai-meeting-tool-zero-data-retention-policy) or[X (Twitter)](https://twitter.com/intent/tweet?text=Zero%20Data%20Retention%20in%20AI%20Meeting%20Tools%20(August%202026)&url=https://www.spinach.ai/blog/ai-meeting-tool-zero-data-retention-policy)
