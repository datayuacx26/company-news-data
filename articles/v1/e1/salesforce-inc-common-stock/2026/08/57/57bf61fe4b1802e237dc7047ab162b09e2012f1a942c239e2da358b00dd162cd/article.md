---
schema_version: "1.0.0"
document_id: "57bf61fe4b1802e237dc7047ab162b09e2012f1a942c239e2da358b00dd162cd"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/how-standardizing-product-telemetry-reduced-time-to-insight-by-97/"
published_at: "2026-08-11T00:18:28+00:00"
first_seen_at: "2026-08-11T01:34:12.789066+00:00"
fetched_at: "2026-08-11T01:34:14.211400+00:00"
content_hash: "sha256:0ca860c1544564caad9f77cf27250b23db69b4db73aab800b3796f9685bd9372"
---

# How Standardizing Product Telemetry Reduced Time to Insight by 97%

*By Rounak Mehta, Adrian Eng, Sandeep Singh, and Kishore Kanchapalli.*


In our Engineering Energizers Q&A series, we highlight the engineering minds driving innovation across Salesforce. Today, we spotlight Adrian Eng, Director of Product Management, who helped lead efforts to standardize product telemetry across Salesforce so engineering teams can automatically generate trusted product adoption metrics.


Explore how the team replaced fragmented telemetry pipelines with a standardized Product Data Platform (PDP) that reduced time to insight by 97%, automated product adoption metrics across Salesforce, and created a trusted data foundation for AI-powered analytics, MCP integrations, and[developer productivity](https://engineering.salesforce.com/boosting-developer-productivity-with-ai-faster-dashboards-automated-testing-and-70-less-setup-time/) .


##### **What is your team’s mission and why did Salesforce decide to build the Product Data Platform?**


The mission is to make product data the backbone for agents, workflows, applications, and decisions by Salesforce employees and customers. Standardizing product telemetry is the crucial bedrock to enabling this at scale, allowing product teams to instrument once and automatically generate trusted adoption metrics.


Before PDP, highly customized telemetry solutions were built for individual product teams. Engineers, analysts, and data scientists manually transformed telemetry into dashboards and adoption metrics for each product independently. Despite the variation, roughly 80% of teams wanted the same thing: basic product adoption metrics. That approach created silos, duplicated work, and became impossible to scale as Salesforce introduced products like[Agentforce](https://engineering.salesforce.com/how-agentforce-data-and-apps-turned-the-salesforce-stack-into-agentforce-360/) and[Data 360](https://engineering.salesforce.com/how-data-360-segmentation-processes-a-quadrillion-records-across-arbitrary-customer-data-models/) . Enterprise organizations such as Customer Success and applications powering product-led growth also lacked a single place to understand adoption across products.


PDP changes that operating model. Product teams follow a standardized telemetry framework, automated pipelines generate metrics, and enterprise teams gain a trusted marketplace for product adoption data without requiring manual analysis.


##### **When individual product teams defined telemetry in their own ways, what engineering challenges made the previous model impossible to scale?**


The biggest challenge was the amount of manual work required before anyone could generate meaningful metrics. Each product team instrumented telemetry differently, requiring engineers every release to interpret product-specific events, transform raw telemetry, and build custom dashboards. That same effort repeated across product after product, while inconsistent telemetry made trustworthy metrics increasingly difficult to produce.


PDP changed that operating model by introducing a standardized telemetry framework that every product team could follow. Instead of building custom pipelines for each organization, automated pipelines now generate consistent adoption metrics from standardized instrumentation. Engineering teams instrument once, and PDP handles the downstream processing that previously required significant manual effort.


##### **What made standardizing telemetry across hundreds of products the hardest engineering challenge?**


The hardest challenge was identifying what every product truly had in common. Each Salesforce product measures success differently, so creating one telemetry model that worked broadly while still supporting product-specific needs required careful balance. The goal was enough standardization to automatically generate trusted metrics while allowing teams reasonable flexibility for their own analysis. Achieving that required close collaboration across PDP and subject matter experts representing products like[Agentforce](https://engineering.salesforce.com/how-agentforce-prevents-language-drift-in-600k-daily-multilingual-ai-workflows/) and[Tableau](https://engineering.salesforce.com/einstein-copilot-for-tableau-building-the-next-generation-of-ai-driven-analytics/) .


Technically, the team built a standardized custom schema on Monitoring Cloud infrastructure. Certain fields remain mandatory because they’re required for consistent metrics, while carefully governed optional attributes give teams additional flexibility without sacrificing standardization. The schema continues evolving as new cross-product use cases emerge.


This evolution resulted in an ocean of standardized event data that had to be translated into clear, actionable insights. Today, Product Data Platform handles this at a staggering scale, processing **45 billion rows of data every single day** across **19,000 distinct events** and covering more than **2,000 Salesforce product features** .


*How PDP processes billions of daily events for enterprise scale.*


##### **Once the Product Data Platform was built, what challenges emerged getting engineering teams to adopt a common telemetry standard?**


The biggest challenge was changing established engineering workflows. Product teams were focused on shipping features, so introducing another instrumentation standard naturally created resistance. Many teams initially viewed it as additional work rather than a productivity improvement.


That changed once the long-term benefit became clear. Instead of repeatedly creating custom telemetry, dashboards, and metrics every release, teams could instrument once and rely on PDP to generate standardized metrics automatically. The team also partnered on an AI-powered MCP tool that references the PDP standard and recommends the appropriate instrumentation automatically, helping engineers instrument faster while staying compliant without needing to memorize telemetry requirements. The combination of roadmap savings and improved developer productivity became the key to driving adoption.


##### **As PDP scaled across Salesforce, what results did you observe?**


The results showed up in two ways. At scale, automated pipelines now generate metrics regardless of how many new product features are introduced, as long as product teams follow the PDP standard. On speed, teams previously waited about a month between instrumenting telemetry and seeing metrics appear on dashboards. Today, dashboards refresh daily, **reducing time to insight by approximately 97%.**


Developer productivity improved as well. Instead of requiring multiple engineers to manually transform telemetry, product teams typically spend only a few hours implementing standardized instrumentation, and developer surveys consistently report approximately 9/10 CSAT for the onboarding experience. Automation now handles much of the repetitive work previously required throughout the telemetry pipeline.


##### **As engineering teams began building AI tools and MCP integrations, what challenges made a standardized telemetry foundation essential?**


AI systems are only as effective as the data they’re built on. Before PDP, telemetry either wasn’t standardized or was too fragmented to be consumed efficiently by AI applications. Because PDP established a common telemetry schema and standardized instrumentation, the team was able to build an MCP plugin that interacts directly with the platform.


Engineers and product managers can now use AI-powered workflows to query product adoption information through the PDP plugin and retrieve metrics tied to the correct product feature taxonomy. Standardizing telemetry didn’t just automate adoption metrics. It established the trusted data foundation needed for the next generation of AI-powered engineering tools.


##### **Learn more**


- Stay connected by joining our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) .
- Explore our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to see how you can get involved.
