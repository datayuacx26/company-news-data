---
schema_version: "1.0.0"
document_id: "7e2e7ede6f5ee66b7c82b67147de3f36c9228dd9333d8dfdc582b18dba26b51e"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/how-ai-reduced-customer-bug-triage-from-nearly-a-year-to-less-than-a-week/"
published_at: "2026-07-21T23:16:44+00:00"
first_seen_at: "2026-07-21T23:54:12.643366+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:e3222b757bade505e1c0d2c5bccb7f2d8e3e70c97534b06b624cd50e75d136bc"
---

# How AI Reduced Customer Bug Triage from Nearly a Year to Less Than a Week

*By Priya Sethuraman, Abhishek Ghose, Lovish Agarwal, and Aditya Pandey.*


In our Engineering Energizers Q&A series, we highlight the engineering minds driving innovation across Salesforce. Today, we spotlight Priya Sethuraman, Director of Software Engineering. Priya leads the[Sales Cloud](https://engineering.salesforce.com/sales-clouds-ai-transformation-welcome-to-the-autonomous-selling-era/) BugWiser initiative, an AI-powered system that standardizes customer bug classification and transforms customer bug signals into faster engineering action. By combining a custom machine learning model with a large language model (LLM), BugWiser reduced customer bug triage time from **more than 300 person-days of manual effort to less than a week** while generating trusted release insights at scale.


Explore how Priya’s team transformed years of engineering expertise into a trusted AI system that standardizes bug classification, accelerates root cause analysis, strengthens engineering productivity, and helps Salesforce close the loop with customers faster.


##### **As Salesforce shipped new capabilities at an increasingly rapid pace, what challenge drove your team to rethink how customer bugs were analyzed and acted upon?**


As Salesforce accelerated feature delivery, customer feedback became more valuable and more difficult to process consistently. Every customer bug became a signal that could reveal regressions, missed use cases, accessibility issues, and broader product quality trends. The challenge wasn’t simply fixing individual bugs, rather, it was transforming those customer signals into timely engineering action while continuing to ship innovation.


The mission became shortening the time required to understand customer-reported bugs while enriching every issue with the engineering context needed to accelerate root cause analysis. Historically, engineers manually reviewed every customer bug, analyzed associated investigations, determined the likely root cause, classified each issue, and documented their findings before meaningful work could begin. That process produced valuable insights but required significant manual effort and depended heavily on engineering expertise. Leadership also needed broader release insights, including which product areas generated the most customer issues, which classifications became dominant across a release, and where engineering investments would have the greatest impact. Producing those insights often required days of manual analysis.


BugWiser changes that operating model. By standardizing customer bug classification and enriching every investigation with trusted engineering context, engineering teams can identify trends faster, improve engineering productivity, prioritize investments, and quickly close the loop with customers.


##### **When customer bug investigations were still handled manually, what challenges made AI-powered bug triage and standardized bug classification so difficult?**


The hardest part wasn’t reducing manual effort; it was encoding years of engineering judgment into a system that could apply it consistently across Salesforce. Every customer bug required engineers to review investigations, determine the root cause, classify the issue, and document their reasoning. Because those decisions relied on individual expertise, similar customer issues could be classified differently across engineers and clouds.


Instead of asking AI to replace that expertise, the team focused on teaching a system to think the way subject matter experts already did. The model was trained using years of historical engineering decisions, so instead of relying entirely on generalized AI reasoning, it learned how Salesforce engineers had historically evaluated customer bugs. That preserves institutional engineering knowledge while applying it consistently across thousands of investigations.


Standardization became just as valuable as automation. Engineering teams now share a common framework for bug classification, giving leadership clearer visibility into dominant problem areas, recurring customer issues, and the engineering actions with the greatest customer impact.


##### **When you set out to automate customer bug classification, what made it much harder than simply applying AI to bug reports?**


Applying AI to bug reports wasn’t the difficult part, rather, building a system engineers would trust was. General-purpose AI could generate classifications, but the goal was not to replace subject matter experts. It was to teach the system how those experts already approached customer bug investigations.


That led to building a custom machine learning model trained specifically on Salesforce’s historical bug classifications. The model learns from years of engineering decisions, producing recommendations that reflect established engineering practice rather than simply generating plausible answers. Every prediction includes a confidence score so engineers understand how certain the system is before acting, and it explains the evidence behind each recommendation so reviewers have greater confidence in the outcome.


When engineers disagree with a prediction, they do not simply reject it. Instead, they provide the correct classification, which continuously retrains the model and ensures it evolves alongside engineering expertise. Rather than asking engineers to trust AI, the team built an AI system that continuously learns from engineers. That philosophy, combining machine learning with human expertise to deliver trust by design, became one of the defining principles behind BugWiser.


*Bridging custom model training with enterprise LLMs.*


##### **What engineering trade-offs led your team to combine a custom machine learning model with an LLM instead of relying on a single AI model?**


One of the biggest engineering decisions was recognizing that no single AI model excelled at every part of the workflow. The custom machine learning model performs customer bug classification using Salesforce’s historical engineering decisions to deliver fast, deterministic predictions with confidence scores and explainability. LLMs like[Anthropic’s Opus](https://www.anthropic.com/claude/opus) complements that by synthesizing grounded engineering context and generating richer summaries for engineers.


The decision was not about avoiding large language models. Instead, it was about choosing the right AI model for each task. While LLMs excel at reasoning across unstructured information, the smaller, specialized machine learning model proved better suited for deterministic bug classification, confidence scoring, and explainability. By assigning each AI model to the work it performs best, the team achieved the right balance of accuracy, speed, flexibility, and trust across engineering workflows.


##### **After deciding to build your own machine learning model, what proved most difficult about training, validating, and deploying it in production?**


The biggest challenge was learning an entirely new discipline. Like many engineering teams, the team began with large language models before realizing they couldn’t provide the deterministic behavior, confidence scoring, and explainability the bug classification workflow required.


That realization meant learning how to identify the right model for the use case, train it using years of historical engineering decisions, evaluate its performance against general-purpose LLMs, and continually improve it as new engineering knowledge emerged. Building that expertise meant moving beyond simply consuming AI tools to developing an AI system tailored to a specific engineering problem. Operationalizing the solution introduced another challenge. Unlike LLMs, smaller custom machine learning models required defining how they would be hosted, retrained, monitored, and integrated into Salesforce’s engineering workflows.


The investment proved worthwhile. Beyond delivering a production-ready AI system, the team gained a deeper understanding of one of AI engineering’s most important lessons: the best AI model is not always the largest model. It is the one best suited for the problem you’re trying to solve.


##### **As engineers began relying on BugWiser, what challenges did you face building trust through confidence scoring, explainability, and human review?**


Trust by design became a core engineering principle. Every recommendation had to provide enough evidence for engineers to confidently act on it or intentionally override it. Every prediction includes a confidence score and explains the evidence behind each bug classification, giving engineers the transparency needed to trust or question the recommendation.


Human review remains central to the workflow. High-confidence predictions can be accepted automatically, while lower-confidence recommendations receive manual review. When engineers disagree, their corrections become new training data that continuously improves future predictions. Across the initial Sales Cloud and Service Cloud deployments, **approximately 90% of predictions were accepted without modification, while only a small percentage required human correction.**


Success isn’t measured simply by how quickly customer bugs are classified. It’s measured by how effectively customer signals are transformed into trusted engineering action. By combining a specialized machine learning model, an LLM, and continuous human expertise, BugWiser helps engineering teams standardize bug classification, accelerate root cause analysis, improve engineering productivity, generate richer release insights, and close the loop with customers faster.


##### **Learn more**


- Stay connected by joining our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) .
- Explore our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to see how you can get involved.
