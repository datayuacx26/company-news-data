---
schema_version: "1.0.0"
document_id: "7b8f38b126ac41e029e36fee4ad27637cab354df4a58b2fb8b5e678e3bc18b42"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/how-informatica-reduced-data-integration-pipeline-development-from-days-to-minutes/"
published_at: "2026-07-09T18:59:53+00:00"
first_seen_at: "2026-07-20T03:32:03.151430+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:7a4e37f21568a53b7809822efb7b5e8183505d9b13da92b8dae292787bbd2529"
---

# How Informatica Reduced Data Integration Pipeline Development from Days to Minutes

In our Engineering Energizers Q&A series, we highlight the engineering minds driving innovation across Salesforce. Today, we spotlight Nancy Chen, Vice President of Engineering at[Informatica](https://engineering.salesforce.com/how-informatica-built-a-multi-agent-ai-system-to-reduce-data-workflows-from-months-to-days/) . Nancy leads the development of Copilot, an AI-powered experience that enables users to generate data integration pipelines using natural language. Since launching, customers have generated approximately **10,000 pipelines and submitted more than 25,000 expression-generation requests.**


Explore how the team reduced data integration pipeline development from days to minutes, adapted to rapidly evolving foundation models, and improved accuracy through prompt engineering, grounding, and validation techniques that help deliver trustworthy AI-assisted data integration.


##### What is your team’s mission in building Copilot, and why did traditional data integration workflows become a bottleneck?


The mission is to dramatically simplify[data integration](https://engineering.salesforce.com/inside-informaticas-spark-based-data-integration-platform-running-250k-enterprise-pipelines-daily/) pipeline development and reduce the time required to transform, move, and prepare enterprise data across systems.


Traditional workflows required engineers to inspect metadata, configure transformations, and manually connect sources and targets. Even straightforward pipelines could take days or weeks because users had to understand numerous transformations and configuration options. The complexity itself had become the bottleneck. Customers weren’t struggling with integration capabilities. They were struggling with complexity.


That realization led the team to rethink the experience around[natural language](https://engineering.salesforce.com/bridging-natural-language-and-sql-with-generative-ai/) . Instead of forcing users to manually construct pipelines step by step, the experience was redesigned to let customers describe what they want conversationally and generate pipelines in minutes. By removing that complexity, **customers spend less time building integrations and more time using data.**


##### When customers were manually building mappings across systems like Snowflake and Salesforce, what made transformation design so difficult?


Traditional mapping design is highly manual. Even relatively straightforward pipelines could require engineers to inspect schemas, manually define field mappings, configure transformations, and repeatedly validate outputs. As complexity increased, pipelines that should have taken hours could stretch into days or even weeks.


Moving data from Snowflake into Salesforce is a good example. Customers have to understand schemas,[metadata](https://engineering.salesforce.com/scaling-data-cloud-for-agentforce-and-ai-rethinking-metadata-activation-and-governance-at-hyperscale/) , field relationships, and transformation logic, and every change introduces additional validation and troubleshooting cycles.


The experience changes that interaction model entirely. Instead of focusing on implementation details, users can describe their intent conversationally while the system generates the pipeline automatically. This shifts the focus away from how to build the pipeline and toward the business outcome the customer is actually trying to achieve.


##### Once Copilot reached production, what convinced you that customers were becoming more productive?


Customer behavior quickly showed that the experience was becoming part of daily workflows rather than something users simply wanted to experiment with.


Since mapping generation launched in May 2025, customers have generated approximately 10,000 pipelines using conversational prompts. Expression generation became one of the clearest early signals. Instead of manually writing syntax, users simply described the behavior they wanted, and that capability quickly became one of the most heavily used features on the platform. Customers submitted more than 25,000 expression generation requests, accepting roughly **60%** without any modification.


The team also introduced mapping augmentation capabilities, allowing users to insert transformations into existing pipelines. Within less than two months of launch, customers had already used the feature on m **ore than 900 existing pipelines, with approximately 80% of generated transformations accepted.**


Perhaps the strongest signal came from return usage. Thousands of customers adopted the experience and kept coming back, which made it clear that the platform wasn’t simply attracting curiosity. It was helping customers work more efficiently.


##### What made building reliable AI-assisted data integration so difficult?


The biggest challenge was that AI technology evolved faster than the team’s architecture could keep up with. When the project started,[OpenAI](https://engineering.salesforce.com/detecting-openai-ai-provider-issues-in-under-10-minutes-with-a-new-observability-system/) models weren’t mature enough for the team’s needs. The team initially fine tuned its own models because that approach delivered better results and aligned with what much of the industry was doing at the time. As foundation models improved, OpenAI began producing significantly better outcomes than internally fine-tuned models, making the switch a natural next step.


*Initial design with Fine-Tuned Model*


*New design with OpenAI model.*


Moving to OpenAI improved accuracy, but it also meant giving up direct control over model behavior. It required embracing a faster pace of change and accepting that model behavior would continue evolving underneath the platform. Testing, validation, and development practices all had to be rethought to maintain confidence as foundation models kept improving.


Generative AI also forced engineers to adopt a completely different mindset. Traditional software development is deterministic, where the same inputs produce the same outputs every time. AI systems behave differently. Small prompt changes can produce very different responses, and because outputs are probabilistic, validation required much broader test coverage to ensure improvements in one area didn’t introduce regressions elsewhere.


To address these challenges, the team continuously reevaluated the architecture, invested heavily in testing, and shifted focus from custom models toward prompt engineering, grounding, and leveraging the strengths of rapidly improving large language models. Building the platform required evolving alongside the AI ecosystem itself.


##### What engineering challenges emerged around generating accurate pipelines from conversational prompts?


Generation itself wasn’t the hard part but accuracy proved difficult. Building accurate pipelines required solving challenges around metadata discovery, schema mapping, prompt engineering, data quality, and output validation. Users don’t always know where required data resides, and understanding transformation intent adds another layer of complexity. Pipelines also have to account for varying data quality, formatting differences, missing values, and data cleansing requirements.


Moving to OpenAI introduced a new set of challenges around prompt tuning and grounding. Because the team no longer controls the underlying model, enough context has to be provided to guide generation accurately. Improving results became an iterative process of refining prompts and adding context. Generated outputs also require validation since expressions and object names occasionally need refinement.


To address these challenges, the team is expanding cataloging capabilities to help customers identify metadata automatically, continuously refining prompts, grounding models with additional context, and introducing validation layers to ensure generated outputs are usable. The key insight was that improving generative AI accuracy had less to do with larger models and more to do with context, validation, and guardrails.


##### As AI-assisted data integration continues evolving, what engineering challenges lie ahead?


Customer expectations are evolving just as quickly as the underlying technology. Many customers want broader support for additional transformations and more complex use cases. Others are looking beyond conversational interfaces and asking for code-first experiences similar to AI coding assistants.


Agentic AI workflows are also creating new expectations around headless experiences and automated development. Customers increasingly want[AI agents](https://engineering.salesforce.com/building-enterprise-ai-agents-that-are-both-autonomous-and-reliable/) that generate code, build integrations programmatically, and automate workflows end to end. To meet those demands, the team is expanding support for additional use cases, investing in code-first workflows, and exploring new interaction models that extend beyond conversational experiences.


The challenge is ensuring these capabilities evolve without sacrificing the accuracy, reliability, and production readiness customers expect. Regardless of how interaction models change, trust remains the foundation. Every new capability has to deliver the same reliability customers depend on in production systems.


##### **Learn more**


- Stay connected — join our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) !
- Check out our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to learn how you can get involved.
