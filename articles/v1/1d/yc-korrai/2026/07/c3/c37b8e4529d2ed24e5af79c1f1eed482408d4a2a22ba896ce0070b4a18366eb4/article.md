---
schema_version: "1.0.0"
document_id: "c37b8e4529d2ed24e5af79c1f1eed482408d4a2a22ba896ce0070b4a18366eb4"
company_key: "yc-korrai"
company: "KorrAI"
source_id: "yc-korrai-news-import-a686cf049d8d"
canonical_url: "https://www.korrai.com/blog/ai-agents-for-desktop-risk-studies-insights-from-imia-hosted-webinar"
published_at: null
first_seen_at: "2026-07-22T01:41:37.374471+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:79640caa31958cbf611302f62a9e24e5825788916ecd3270ef6f2b630cf6a27b"
---

# AI Agents for Desktop Risk Studies (Insights from IMIA Hosted Webinar)

> *Insights from our webinar on transforming construction insurance underwriting, February 11, 2026*


***Speakers*** *: Rahul Anand and Jean Poirier, KorrAI*


**Moderator** : Patrick Bravery, An insurance industry practitioner


**Host** :[IMIA](https://www.imia.com/events/?_webinar_categories=webinars#:~:text=Webinar%20%E2%80%93%20Engineering%20Judgment,used%20in%20KorrA) (The International Association of Engineering Insurers)


When we registered 300+ attendees from 85 companies across 50 countries for a webinar on desktop risk assessments and AI, it told us something straightforward: this problem isn't regional. It's felt by risk engineering and underwriting teams everywhere.


[Desktop risk assessments](https://www.korrai.com/use-cases/infrastructure-insurance) are a critical step in risk engineering across insurance, construction, and maintenance phases of the infrastructure lifecycle. The current workflow of desktop studies is bottlenecked by fragmented data channels, multiple tools for multi-source data, and the exponential demand as the new-age infrastructure market grows. In the webinar, Rahul Anand (CEO & Co-founder, KorrAI), and Jean Poirier (Senior Product Manager, KorrAI) explain how multi-agent AI systems can help expedite the time-consuming segments of a desktop study while not sacrificing the quality of risk reports. Towards the end, they demonstrate how engineering judgement in risk assessment can be scaled with experts in control in collaboration with AI co-workers.


## Why Underwriting Errors in Construction Insurance are So Costly


The very nature of construction insurance makes early errors expensive: the consequences don't show up immediately.


These policies are **multi-line,** covering physical damage, business interruption, and project-specific exposures that interact with each other. They're **multi-year;** once a policy is bound, insurers are locked in for 24 to 36 months, often longer through maintenance periods. There are **multiple insureds -** owners, contractors, suppliers, and designers, each with different failure modes at different points in the project lifecycle.


A misread at the underwriting stage doesn't correct cleanly. It shows up later as disputes, delays, and claims that are hard to unwind. This is why engineering judgment matters so much here, and why scaling it without losing quality is genuinely hard.


Complex attributes of construction insurance that makes any misjudgment trigger years of repercussions.


## The Perfect Storm for Risk Engineering & Insurance


The scale of infrastructure being built right now is changing the math for risk engineers.


[Data center construction alone was a $241B market in 2024 and is projected to reach $456B by 2030 at an 11.8% CAGR](https://www.grandviewresearch.com/industry-analysis/data-center-construction-market#:~:text=Data%20Center%20Construction%20Market%20Summary,America%3A%20Largest%20market%20in%202025) .[The power demand will grow 5x by 2035.](https://www.notion.so/IMIA-Webinar-2fcec0c9bb6c809687f2e7bbbc7c13f2?pvs=21)[Commercial construction planning is up 30% year-over-year.](https://www.notion.so/IMIA-Webinar-2fcec0c9bb6c809687f2e7bbbc7c13f2?pvs=21)


Data centers and energy infrastructure aren't simple sites. They're large, complex, often in challenging site conditions, and they're being built fast. What used to take four to five years is now compressed into two.


That compression feeds directly into[DSU exposure](https://www.korrai.com/blog/how-insurers-underwrite-delay-from-site-conditions-in-hyperscale-construction-projects-with-korrai-ai-co-worker) : when a project is tightly sequenced and highly capitalized, a small misread of on-site conditions early on can produce a large delay claim later.


All of this is happening while the workforce is shrinking.


Infrastructure demand is surging, while the risk engineering capacity continues to shrink.


## How Much Time Do Desktop Risk Assessments Actually Take?


A desktop risk assessment follows a familiar sequence: submission received, document gathering, manual data hunt, cross-referencing, resolving gaps, applying engineering judgment, writing the report.


The survey we ran during the webinar confirmed what we already see in practice: reviewing submission documents and gathering external data are the two largest time sinks. Most attendees said they spend five to ten hours on typical projects, but the risk analysis may stretch up to 2 weeks when the project is complex in nature, and most of that time goes to data hunting and resolving gaps.


Estimated time dedicated for data hunting, triage, and continuous engineering judgment at present


## 3 Challenges That Clearly Show the Need for Better Systems in Risk Assessment


**More Data, Compressed Timelines:** LiDAR, IoT sensors, satellite imagery, geological databases: the[volume of available data](https://www.korrai.com/platform/evidence-layers) has grown. But more data requires dedicated attention, and interlinking to actually bring value.


**Constant context switching:** Insurance teams are juggling multiple submissions at once. That means bouncing between documents, portals, and spreadsheets while trying to hold the full risk picture in mind.


**Risk that can't be templated:** Two sites a kilometer apart can have completely different risk profiles. Geological conditions, foundation type, construction sequence, DSU exposure, and contractor capability - most importantly, they are all interconnected. Traditional tools treat each factor independently. The risk engineer still has to assemble the full picture manually.


## Why Traceability Matters as much as Accuracy


During the webinar, Jean walked through a specific example that's worth pointing out here:


## Enter AI Agents for Infrastructure Desktop Studies


Point solutions already exist. NLP tools extract data from submissions. Computer vision monitors site conditions. Machine learning generates NatCat curves. LLMs help draft reports.


What they don't do is maintain context across an entire assessment. They don't carry the scope, the assumptions, and the evidence together from submission intake to the final report.


AI agents are different because they plan a course of action, follow standardized practices baked into master instructions, pull from multiple data sources while keeping context intact, and reason through contradictions rather than ignoring them. When the geotechnical report suggests low concern but regional geologic data tells a different story, the agent surfaces that tension, works through it, and records why a particular conclusion was reached.


## How Can a Suitable AI Platform Make This Happen?


In the webinar, KorrAI presented their platform[TRAIL](https://www.korrai.com/old-home) as a solution that uses a multi-agent architecture: several specialized agents orchestrated by a master agent, each built for a specific task. One parses long technical reports. Another interprets site drawings and borehole logs. Another handles structured data like P6 schedules.


For the[site conditions assessment](https://www.korrai.com/blog/asset-owners-perspective-understanding-site-condition-risks-in-hyperscale-construction) , TRAIL agents work against a set of evidence layers (location-anchored multi-modal datasets): ground motion and subsidence risk, soil stratigraphy, subsurface hazards (karst, sinkholes, pipelines), NatCat exposure, groundwater risk, surrounding infrastructure, satellite-based change detection, and regional development context.


The output is a traceable risk report, with every conclusion linked back to its source. Engineers review it, verify with sources, charts, and maps. Then apply their judgment and nuance.


As a consequence, every risk engineer and underwriter gets a co-worker on the TRAIL workspace.


## Does the System Eventually Replace Risk Engineers?


Here is a parallel drawn from the past:


In 2016, Geoffrey Hinton predicted AI would replace radiologists within five years. Medical schools and students got worried.


By 2026, Mayo Clinic employs 55% more radiologists than it did in 2016. AI now touches nearly every scan. The outcome was more specialists, not fewer


The reason: Freeing radiologists from repetitive scan analysis gave them time to work with patients and consult with clinicians. Demand expanded to meet the new capacity.


We expect the same pattern in risk engineering. The volume of infrastructure being built creates real demand for assessment capacity. AI agents handle the tedious work. Engineers spend more time on valuable judgment.


## The Replacement is the Workspace, Not the Expert.


Adopting AI agents in the workflow is a collaboration exercise, not a replacement. Many different overwhelming platforms or channels are converging to become one AI-native workspace for all-around risk assessment. But the core users will still be present.


A room with just one expert, lots of data, and multiple overwhelming tabs is now going to be replaced by:


The same expert, an AI workspace, and lots of AI agents that help orchestrate the overwhelming data and get the most out of it.


The goal is to meet the rising demand by scaling the risk engineering process. Organizations that embrace AI agents as co-workers will have a significant advantage as the workforce crisis deepens in the near future. They will quote faster without sacrificing the quality, handle more volume with the same team, and attract top talent by offering an opportunity to focus on high-value work instead of repetitive work.


If you got curious about AI agents purposely built for risk engineering and insurance, watch the webinar from the below timestamp where Rahul (CEO, KorrAI) goes deeper into how AI agents work for this use case.


For more information or a tailored demo of the TRAIL platform, feel free to[contact us](https://www.korrai.com/contact-us) *or*[book some time with our founders](https://www.korrai.com/book-meeting) *.*
