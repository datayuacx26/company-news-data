---
schema_version: "1.0.0"
document_id: "6cad2064a6759fedd5a816febafda2183750d9b0c931704d59493bcf8f98f6b9"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/building-trusted-agentic-ai-in-financial-services"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T21:26:42.907604+00:00"
fetched_at: "2026-08-06T21:26:44.239800+00:00"
content_hash: "sha256:7f62aaae2d3b686f5f6eabb3b3d0b03af05476cff23b92dfb9da0a658369d496"
---

# Building trusted agentic AI in financial services: From data to autonomous action

# Building trusted agentic AI in financial services: From data to autonomous action


As financial institutions move from AI experimentation to autonomous operations, trusted context, governance, and observability become the foundation for enterprise-scale Agentic AI.


By


[Karen Mcdermott](https://www.elastic.co/blog/author/karen-mcdermott)


August 6, 2026


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


Artificial intelligence in financial services is entering a new era.


Historically, financial services companies have focused on deploying generative AI to improve productivity, enhance customer experiences, accelerate software development, and streamline operations. Now, the conversation is shifting toward


**agentic AI**


— intelligent systems capable of reasoning, orchestrating workflows, and taking action across the enterprise.


McKinsey's


[State of AI trust in 2026: Shifting to the agentic era](https://www.mckinsey.com/capabilities/tech-and-ai/our-insights/tech-forward/state-of-ai-trust-in-2026-shifting-to-the-agentic-era) report suggests organizations are moving quickly toward enterprise-scale AI deployment. Yet, only about


**30% of organizations have reached higher levels of maturity across AI strategy, governance, and agentic AI controls**


. As AI becomes more autonomous, the challenge is no longer simply deploying AI; it's deploying AI that can be trusted.


For financial institutions, trust isn't a nice-to-have. It's the foundation for scaling AI responsibly.


## Agentic AI changes the risk equation


Traditional generative AI answered questions. But agentic AI takes action.


An AI assistant might summarize suspicious transactions or explain a compliance report. But an AI agent can investigate fraud alerts, retrieve customer information, correlate evidence across multiple systems, recommend next steps, and initiate workflows.


The potential business value is enormous, but so are the consequences if an AI system acts on incomplete, inaccurate, or poorly governed information.


Every autonomous decision depends on more than a powerful foundation model. It depends on whether the AI has access to the right context like current, trusted information that reflects what's happening across the organization.


Without that foundation, organizations risk automating mistakes instead of accelerating better decisions.


## Trust begins with context


Financial services has always operated in a trust-first environment.


Whether complying with DORA, the EU AI Act, the NYDFS Cybersecurity Regulation, or internal model risk management standards, institutions must demonstrate where data originated, how decisions were made, and whether outcomes can be reconstructed for regulators and auditors.


McKinsey's research reflects this maturity, identifying financial services among the industries leading in responsible AI because of its strong governance and risk management foundations.


That leadership creates an opportunity.


Organizations investing in responsible AI are also more likely to report higher AI maturity and stronger business outcomes. Governance doesn't slow innovation; it enables it.


For agentic AI, governance starts with context.


Context is the information an AI agent uses to understand a situation before taking action. In financial services, that means connecting operational telemetry, customer information, transaction history, application data, security events, regulatory policies, and institutional knowledge into a unified view.


Without rich context, even the most advanced models can produce incomplete recommendations. With trusted context, AI agents can reason more accurately, explain their conclusions, and act with greater confidence.


### Better context starts with better data


Trusted AI begins with trusted data. For financial institutions, that means breaking down silos, improving data quality, and ensuring AI systems have access to complete, timely, and relevant information. Without that foundation, even the most sophisticated AI models can produce incomplete, inaccurate, or unexplainable outcomes.


This is especially important as organizations move toward Agentic AI, where autonomous systems increasingly reason, make decisions, and initiate actions with limited human intervention. The quality of those decisions depends on the quality of the context they receive.


During a


[discussion at RSAC 2026](https://www.youtube.com/watch?v=z1aLMtYyDq8&t=6s) , Elastic General Manager of Security Mike Nichols joined Sal Picheria, corporate vice president of security engineering at New York Life Insurance, and theCUBE's Dave Vellante to discuss how organizations are preparing their security operations for the era of AI-powered threats. Rather than viewing AI as the starting point, Picheria described New York Life's multiyear effort to build a data-first foundation, curating operational data, avoiding the creation of a "data swamp," and prioritizing real-time visibility before expanding AI-assisted security operations.


The lesson extends well beyond cybersecurity. Whether deploying AI to detect fraud, automate compliance, improve customer service, or accelerate software delivery, financial institutions must first ensure their data is accurate, observable, searchable, and secure. As Nichols concluded, "The first step of being AI ready is getting visibility and control over the data that you have."


Trusted context is what transforms AI from an experimental capability into a dependable enterprise asset.


## Observability becomes the control plane for AI


As AI agents become more autonomous, organizations need visibility into more than applications and infrastructure.


They need visibility into AI itself.


Leaders increasingly need to answer questions, such as:


-


Which systems did the AI agent access?


-


What information influenced its recommendation?


-


Why did it choose one action over another?


-


Can every step of its reasoning be reconstructed?


Answering these questions requires a unified view across


**metrics, traces, and logs**


.


**Metrics**


reveal performance trends.


**Traces**


show how requests move across distributed applications.


**Logs**


provide the detailed operational events behind every transaction. Together, these telemetry signals create the operational context needed to understand not only what happened, but why it happened.


Observability is no longer just about monitoring infrastructure. It becomes the control plane for governing AI.


## Security must evolve alongside agentic AI


McKinsey found that nearly two-thirds of organizations identify security and risk concerns as the biggest obstacle to scaling agentic AI. At the same time,


**74%**


of respondents identified AI inaccuracies, and


**72%**


cited cybersecurity as highly relevant risks.


For financial institutions, these concerns are well founded.


Every AI agent introduces new interactions with enterprise systems, APIs, sensitive customer information, and third-party services. Every autonomous action must be monitored, validated, and governed.


An


**agentic security operations platform**


helps address this challenge by enabling AI to investigate alerts, correlate activity across logs, metrics, traces, threat intelligence, and historical investigations, and present analysts with a comprehensive view of an incident.


Importantly, humans remain in control.


AI accelerates investigation and reduces repetitive work while preserving the governance and oversight required in highly regulated environments.


Morgan Stanley's perspective reflects this evolution. At a recent client event in NYC hosted by Elastic,


[Rachel Wilson, chief data officer and managing director at Morgan Stanley](https://www.elastic.co/resources/article/financial-services-fortune-500-case-studies) , described cybersecurity as fundamentally a data problem, emphasizing the importance of bringing together fraud, cybersecurity, privacy, and information management data. That unified view becomes increasingly important as organizations adopt autonomous AI.


## Enterprise search delivers trusted context


Large language models provide reasoning. Enterprise search provides context.


As AI agents retrieve information across the enterprise, search ensures they access current, permission-aware, and governed knowledge rather than relying solely on what a model learned during training.


An AI agent investigating potential fraud, for example, may combine customer records, transaction histories, operational logs, fraud indicators, regulatory policies, customer communications, and previous investigations before recommending a course of action.


Enterprise search makes those connections possible, grounding AI in authoritative enterprise knowledge regardless of where the information resides. Rich context improves more than accuracy. It also improves explainability, governance, and trust.


## 5 questions every financial institution should ask before deploying agentic AI


Before moving AI agents into production, financial services leaders should ask:


-


Does the AI have the right context?


-


Can every decision be explained and audited?


-


Can we observe AI behavior through unified logs, metrics, and traces?


-


Is our operational data prepared for autonomous AI?


-


Can security continuously validate AI-driven actions while keeping humans in control?


Organizations that can confidently answer "yes" to these questions will be better positioned to scale agentic AI responsibly.


## From AI experimentation to trusted autonomous operations


McKinsey concludes that AI trust is becoming more than a compliance exercise. It is increasingly a business enabler. For financial institutions, that trust is built on context.


It combines trusted enterprise data, AI-assisted data preparation, enterprise search, unified observability, intelligent security operations, and governance that keeps humans accountable for AI-driven outcomes.


Financial services has spent decades earning trust through resilience, transparency, and disciplined risk management. As the industry enters the era of agentic AI, those same principles remain the key to success.


The institutions that lead won't simply deploy more autonomous AI. They'll deploy AI grounded in trusted context where every decision is informed by the right data, every action is observable, every outcome is explainable, and every innovation strengthens trust rather than compromising it. Because in financial services, the future won't belong to the organizations with the most AI. It will belong to the organizations with the most


**trusted AI**


.


[Get in touch](https://www.elastic.co/contact?pg=global&plcmt=nav&cta=205352) to learn more about how Elastic can support your trusted Agentic AI journey.


**Related blogs:**


- ****


[Inside Fortune 500 financial risk strategies](https://www.elastic.co/resources/article/financial-services-fortune-500-case-studies)
- [Transform financial services with AI: Unlock growth, innovation, and insights](https://www.elastic.co/blog/how-banks-use-existing-data-ai-business-challenges)
- [AI-powered fraud detection: Protecting financial services with Elastic](https://www.elastic.co/blog/elastic-ai-fraud-detection-financial-services)
- [Agentic AI in financial services: The rise of autonomous intelligence](https://www.elastic.co/blog/agentic-ai-financial-services)
- [The rise of intelligent banking: Unifying fraud, security, and compliance in the era of AI](https://www.elastic.co/blog/intelligent-banking)


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, and associated marks are trademarks, logos or registered trademarks of elasticsearch B.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
