---
schema_version: "1.0.0"
document_id: "0a1e0595c191c6643dd873bf7b3716691a2e4bdbef4efda40d04148c6b0a21b0"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/gradient-ai-platform-build-debug-faster"
published_at: "2025-09-22T15:40:41.359+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:7f9b9a149814621a59e5f503867c689ac482fbf9c6066e445818fef68f1d09c7"
---

# Build faster, debug smarter, and make AI safer with new DigitalOcean Gradient™ AI Platform features

The way developers build with AI is changing fast—and so is the[DigitalOcean Gradient™ AI Platform](https://www.digitalocean.com/products/gradient/platform) . Debugging data, reinventing agents from scratch, or connecting AI to workflows manually takes time and slows you down. The DigitalOcean Gradient™ AI Platform is evolving to tackle these challenges, helping you work smarter, faster, and more confidently.


This month, we’re introducing four new updates designed to make AI development more transparent, productive, and secure:


-


**Knowledge base activity logs:**[track and debug indexing jobs →](https://cloud.digitalocean.com/registrations)


-


**Agent templates:**[start projects faster →](https://github.com/digitalocean/gradient-agent-templates)


-


**n8n integration:**[automate workflows →](https://www.npmjs.com/package/@digitalocean/n8n-nodes-digitalocean-gradient-serverless-inference)


-


**Enhanced guardrails:**[make AI safer →](https://cloud.digitalocean.com/registrations)


## Build better knowledge bases with activity logs


When you add data to a knowledge base, it isn’t always clear what’s happening behind the scenes. Did the indexing job finish? Did certain files fail to index? Why isn’t the data showing up in the knowledge base? Until now, debugging knowledge base inputs meant guesswork. Activity logs give you clear visibility into every run, so you always know what worked, what failed, and why.


-


**Real-time banners** for active jobs


-


**History view** to review past runs


-


**CSV downloads** with per-source error details


With this update, you can quickly pinpoint issues, fix them, and be confident about your knowledge base without slogging through trial and error.


[Explore Activity Logs →](https://cloud.digitalocean.com/registrations)


## Build faster with agent templates


Why start from scratch when you can start from a working agent? Agent templates are code-first projects available on GitHub that you can clone, customize, and deploy in minutes.


-


**[llm-Auditor Agent:](https://github.com/digitalocean/gradient-agent-templates/blob/main/llm-auditor)** Adds fact-checking using Tavily, an AI tool that verifies answers for accuracy, with optional knowledge base grounding.


-


**[Product Documentation Agent:](https://github.com/digitalocean/gradient-agent-templates/blob/main/pdocs-agent)** Transforms product documentation into object storage, builds and indexes a knowledge base, deploys a citation-rich answering agent, and optimizes it for high RAG performance as a support chatbot.


-


**[SQL Agent:](https://github.com/digitalocean/gradient-agent-templates/blob/main/sql-agent)** Translates natural language into safe, read-only SQL queries.


-


**[Twilio API Agent:](https://github.com/digitalocean/gradient-agent-templates/blob/main/twilio-api-agent)** Sends marketing texts via Twilio with ready-made logic.


Each Gradient AI Platform template includes dependencies, configurations, and examples so you can focus on building instead of scaffolding.


[Browse agent templates on GitHub →](https://github.com/digitalocean/gradient-agent-templates)


**💡 Community spotlight:** One of our Developer Advocates used the new SQL Agent Template and will show you how to query databases safely with natural language. It’s a great resource if you want to see a real-world implementation from the community.[Check it out ->](https://www.digitalocean.com/community/tutorials/sql-agent-template)


## Automate workflows with n8n integration


With the new n8n community node, you can add DigitalOcean’s Serverless Inference directly into your n8n workflows without having to code.


-


**Chat completions with multiple roles:** Support system, user, and assistant roles to create more complex, context-aware interactions.


-


**Access to multiple models:** Choose the Gradient AI Platform model that best fits your workflow.


-


**Customizable parameters:** Adjust temperature, max tokens, and other settings to fine-tune responses.


-


**Simple credential management:** Authenticate quickly with a Model Access Key from the Gradient AI Platform.


For developers already using n8n, this means you can integrate AI-powered features, like chatbots, content generation, or automated responses, directly into your workflows with a simple drag-and-drop. Install the[DigitalOcean n8n node](https://www.npmjs.com/package/@digitalocean/n8n-nodes-digitalocean-gradient-serverless-inference) , connect it to your serverless inference setup, and start building intelligent, automated workflows in minutes.


.png)


## Make AI safer with enhanced guardrails


AI agents are powerful. But they’re vulnerable to malicious inputs and jailbreak attempts. Our upgraded guardrails make it easy to help ensure your agents aren’t hacked.


-


**Stronger jailbreak detection:** Uses Meta’s latest Llama Guard 4 model to better detect malicious prompts before they reach your agent.


-


**Input protection:** Blocks potentially harmful requests automatically, helping to secure your agent from malicious prompts.


-


**Broader coverage:** Improved detection across categories like fraud, illegal activity, malware, violence, and hate speech.


-


**Low-latency, production-ready:** Works in real-time without slowing down your agent.


Developers can enable the upgraded guardrail directly in the Gradient AI Platform UI. It’s a seamless way to add enhanced security to your chatbots, content generation tools, or any customer-facing AI applications.


[Add Guardrails →](https://cloud.digitalocean.com/registrations)


## Always evolving for developers


From indexing data to building agents to deploying apps and automating workflows, the Gradient AI Platform grows with you. These new updates are just the latest step in making AI development simpler, faster, and more secure.


**Ready to try them out?**


-


[Explore Activity Logs →](https://cloud.digitalocean.com/registrations)


-


[Browse Agent Templates →](https://github.com/digitalocean/gradient-agent-templates)


-


[Try the n8n Integration →](https://www.npmjs.com/package/@digitalocean/n8n-nodes-digitalocean-gradient-serverless-inference)


-


[Add Guardrails ->](https://cloud.digitalocean.com/registrations)
