---
schema_version: "1.0.0"
document_id: "d7cf6cef4887cc9c4e05ee87e7e1c0ed5d538352319d7691674cfdbcc30bc64b"
company_key: "yc-gaiia"
company: "Gaiia"
source_id: "yc-gaiia-news-import-0ba41f3cf900"
canonical_url: "https://gaiia.com/newsroom/ai-inference-inside-gaiia-workflows"
published_at: "2026-03-31T00:00:00+00:00"
first_seen_at: "2026-07-23T10:16:42.852450+00:00"
fetched_at: "2026-07-28T21:25:41.488077+00:00"
content_hash: "sha256:11bf3369415fbe3ad912f7bb8bafa7ad0cbfd4a281c59fc08b68346c4a093fe7"
---

# AI Inference Inside Your ISP Workflows

AI tools like Claude, ChatGPT and Gemini are already part of many teams’ daily work. Support agents summarize tickets with them. Operations teams analyze logs. Marketing teams draft responses. The challenge is that these insights often stay outside the systems where work actually happens.


With the new AI Inference node in gaiia’s Workflow Builder, CSPs can now bring large language models directly into their operational workflows. Instead of copying information into external AI tools, workflows can now send data to a model, interpret the response, and take action automatically. No custom API integrations required.


### How it works: AI becomes another building block in your workflows


The AI Inference node works just like any other node in the Workflow Builder. It can receive data from existing gaiia nodes and pass results to the next step in the workflow.


This means AI can now sit inside the same automations that already power provisioning, ticketing, notifications, and customer lifecycle events.


For example a workflow could:


1. Trigger when a new customer interaction is logged
2. Pull the customer’s account notes
3. Send those notes to an AI model for summarization
4. Automatically create a ticket with recommended next actions


All of this happens in a single automated flow. Instead of adding more manual review steps, workflows become capable of interpreting information and making smarter decisions.


‍


### Bring your own AI models


The AI Inference node currently supports:


- OpenAI models such as GPT-4o and GPT-4o-mini
- Google Gemini models such as Gemini Flash
- Claude models such as Opus and Sonnet


Operators simply connect their API key in the integrations module and select the model inside the workflow. This approach gives teams full control over:


1. **Provider and model selection:** Operators can choose the AI provider and model that best fits their needs, whether that means prioritizing cost, speed, or output quality. Teams can easily test different models across workflows without changing infrastructure.
2. **Usage costs:** AI requests are billed directly by the provider, giving operators clear visibility into usage and the flexibility to control costs by selecting smaller or faster models where appropriate.
3. **Data policies:** Each provider has its own security and privacy controls. By connecting directly to their chosen provider, CSPs maintain control over where data is processed and which policies apply. For enterprise environments, this includes options like Claude via AWS Bedrock or ChatGPT via Azure Foundry, giving teams deeper visibility, control, and compliance alignment.


If a team already uses Claude, ChatGPT or Gemini internally, they can now automate those same capabilities inside gaiia.


‍


#### Example: AI-powered ticket routing


Many ISPs still route tickets manually or through simple keyword rules. This works for basic scenarios but breaks down when customer descriptions vary. With the AI node, workflows can interpret the intent of a support request and route it automatically.


Example Workflow:


1. Trigger when a new support ticket is created
2. Send the ticket description and customer context to an AI model
3. Classify the issue (billing, Wi-Fi, fiber cut, installation, outage)
4. Route the ticket to the correct team, tag ticket, & set priority level


Instead of relying on rigid rules, AI can understand the meaning behind customer messages and route issues more accurately. This reduces internal handoffs and speeds up resolution times.


‍


#### Example: Churn risk detection


Customer churn is often visible in interaction history long before a cancellation happens. Repeated support issues, billing disputes, or negative sentiment in conversations can all signal risk. With AI inside workflows, operators can automatically analyze customer interactions and flag accounts that may need attention.


A workflow could:


1. Trigger after a customer support interaction
2. Analyze notes or messages using an AI model
3. Detect signals of frustration, repeated issues, or cancellation intent
4. Tag the account as at-risk and notify the retention team


Instead of discovering churn only after a cancellation request, operators can proactively intervene when early signals appear.


‍


#### From a system of record to a system of action


The real power of this feature comes from how it integrates with gaiia’s workflow engine.


Traditional OSS/BSS platforms tend to act as systems of record. They store data but rarely enable teams to build new operational capabilities on top of it. gaiia is designed differently.


By combining automation, integrations, and now AI inside the workflow builder, gaiia becomes a system of action. Operators are not just managing records. They are building automated processes that improve how the business runs.
