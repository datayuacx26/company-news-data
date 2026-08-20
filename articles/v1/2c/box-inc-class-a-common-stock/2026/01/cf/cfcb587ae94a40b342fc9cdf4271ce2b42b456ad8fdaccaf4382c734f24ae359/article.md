---
schema_version: "1.0.0"
document_id: "cfcb587ae94a40b342fc9cdf4271ce2b42b456ad8fdaccaf4382c734f24ae359"
company_key: "box-inc-class-a-common-stock"
company: "Box Inc."
source_id: "box-inc-class-a-common-stock-rss-6b6ba587c738"
canonical_url: "https://medium.com/box-tech-blog/slo-out-of-the-box-311306fd511c"
published_at: "2026-01-23T22:41:58+00:00"
first_seen_at: "2026-07-20T04:36:04.074136+00:00"
fetched_at: "2026-07-28T22:23:12.089184+00:00"
content_hash: "sha256:0f328da55aa5f1ae9f7b4d77d1bb435ce95cc7d3a1f6a054acbd1ac34e43438f"
---

# SLO out of the Box

# SLO out of the Box


[ssuresh](https://medium.com/@suhas-suresh?source=post_page---byline--311306fd511c---------------------------------------)


11 min read


·


Jan 23, 2026


--


Written by Suhas Suresh and Mikey Phun


Press enter or click to view image in full size


Illustrated by Navied Mahdavian / Art directed by Erin Ruvalcaba Grogan


Box started as a scrappy startup with a simple goal: make it easier for businesses to store and share their content. But as our customers’ needs evolved, so did we — transforming from an online storage and collaboration tool into a comprehensive content management platform, and ultimately into the Intelligent Content Management solution we are today. Fast forward to now, and we support more than 100,000 companies worldwide, managing exabytes of critical data — equivalent to billions of hours of HD video — for some of the largest organizations in the world.


Each major shift brought new challenges. The move to public cloud forced us to rethink scaling and security. Microservices improved how we built software but fragmented our visibility. Mobile users demanded consumer-grade experiences with enterprise-level security.


These shifts exposed a critical gap: we lacked a uniform way to accurately measure and assess service reliability. Engineering and Operations tracked different metrics across separate dashboards, making it challenging to efficiently answer questions about availability, performance, and customer impact . Without shared standards, incident response and root cause analysis was delayed. We needed a unified framework to define, measure, and improve customer experience — ultimately leading us to implement **Service Level Objectives (SLOs)** across Engineering.


## What are SLOs and why they matter at Box


Service Level Objectives (SLOs) are measurable goals for reliability — think of them as **internal “user happiness” scores** . Each SLO combines:


- **Service Level Indicators (SLIs)** : Metrics like latency, error rate, or uptime
- **Target** : A specific goal (e.g., 99.9% success rate)
- **Time window** : The measurement period (e.g., 28 days)
- **Error budget** : the difference between 100% and the SLO over a period. It represents the permitted proportion of time the service may be unavailable or outside the SLI threshold without breaching the SLO.


**Example SLO** : “99.9% of API requests complete within 300ms over a 28-day window. Error budget = 100% − 99.9% = 0.1% of the month ≈ 43.2 minutes of allowable downtime”


These internal targets build on each other: Service Level Indicators (SLIs) form the foundation for Service Level Objectives (SLOs), which collectively help us meet our Service Level Agreements (SLAs) with customers.


As Box evolved from on-premises to cloud, each team built custom monitoring for their specific needs. Without a unified approach to measuring reliability, teams fell into a reactive pattern: focus on features until a major incident forced everyone to shift attention to firefighting. Once the crisis passed, teams returned to feature work — only to repeat the cycle when the next issue emerged. This pendulum swing between features and reliability meant that reliability never improved systematically.


SLOs break this cycle by giving service owners a **proactive indicator** of when to shift focus. Instead of waiting for customer complaints or incidents, teams can see degrading reliability trends early and address issues before they escalate — making the shift from reactive firefighting to intentional, data-driven prioritization.


To better protect the customer experience, our newly formed SRE organization embarked on a journey to create an SLO program with a goal to provide a **unified standard for measuring and ensuring the reliability** of Box’s most critical applications. This framework not only gave us consistent reliability metrics across all services but also enabled teams to **make data-driven decisions** about where to invest engineering effort.


## Initial Roadblocks


Despite the clear need for a unified method to objectively quantify scores across all services, implementing SLOs across Box wasn’t straightforward. We encountered three significant challenges that put the initiative at risk.


### Technical Complexity


Creating a unified SLO solution revealed unexpected complexity. Each team had built custom monitoring over the past decade, creating a **maze of incompatible tools and metrics** . Previous SLO attempts had failed by being either too rigid (one-size-fits-all approaches that didn’t fit specific team needs) or too flexible (no standards at all, leading to chaos).


We **needed to find the sweet spot between standardization and customization** — a challenge that proved more difficult than anticipated.


### Organizational Resistance


The **biggest challenge was cultural, not technical** . Teams pushed back with variations of **“Our monitoring works fine — why change?”** They saw SLOs as extra work competing with feature development priorities.


But the real concern ran deeper: SLOs would surface when their service availability suffered due to issues with dependencies they didn’t control — and they’d be held accountable for failures that weren’t their fault. Many teams were hesitant to prioritize implementing SLOs for services due to bias towards existing monitoring, unconvinced that the effort would justify the investment.


### Knowledge Gaps


The SLO project exposed fundamental gaps in how teams understood and measured reliability. It revealed a two-fold problem: teams lacked understanding of what SLOs were and how to implement them properly, and they had never taken a pro-active approach to measuring service reliability, making it difficult to configure the right measurements. We had to educate teams that a low **SLO score was not about blame — but objective measurements** with an emphasis on accuracy, that helped identify problems in both their services and dependencies. We spent considerable effort simplifying how teams get started with SLOs and creating better training materials, but adoption remained slow. This required not only making the setup process easier but also shifting mindsets company-wide.


## Breaking Through and Scaling: Our Journey from Resistance to Adoption


### The Catalyst


The turning point came when several high-visibility teams needed to improve reliability after customer escalations. Their timing aligned perfectly with our newly simplified SLO adoption process. Here’s how momentum built:


### Building Early Momentum


We recognized early on that successful adoption required a dual-pronged approach: engaging directly with early adopter service teams from the bottom up to deliver immediate, value-driven examples they could champion — while simultaneously demonstrating business-level benefits to leadership from the top down, empowering them to drive adoption across their organizations.


**Early wins with critical teams** : We partnered with teams managing customer-critical infrastructure to create their first SLO dashboards. Within a short duration, they could pinpoint performance issues that had been invisible before. They had an objective signal which could be fixed and measured over time to see progress.


**Success Story** : In one of the SRE engagements, we partnered with a dedicated team focused on improving our critical database access infrastructure. By leveraging SLOs, they achieved remarkable improvement from 95% to 99.98% availability over time, which improved overall availability of almost all user-facing applications as a result. The team rigorously monitored their SLO to identify critical gaps in reliability and invested in long-term projects to address them, ultimately achieving drastic improvements over time.


**Executive visibility** : Leadership embraced SLOs once they saw unified reliability metrics across all services. For the first time, they had objective data to prioritize areas of investment to improve performance.


**The snowball effect** : Once the most business-critical services adopted SLOs, it became easier to convince others. Teams saw their dependencies using SLOs and realized they needed the same visibility. Today, new services adopt SLOs without requiring SRE consultation.


### Technical Foundation


**Real-time SLI Bottleneck**


We chose to reuse existing alert mechanisms as SLIs so teams wouldn’t have to rebuild their observability from scratch for this effort. Conceptually, using alerts as SLIs would work well — engineers could reuse existing alerts as SLI definitions. However, using raw alerts proved impractical due to the cost and slowness of real-time scans at scale, especially over long time windows.


**The Scale Challenge** : Assume we have **30,000 requests per minute for the specified api component as given in the expression below** and the system records one sample per minute (60 samples per hour):


**1 hour** : 60 samples × 30,000 users = 1,800,000 samples


**24 hours** : 1,440 samples × 30,000 users = 43,200,000 samples


```text
sum(rate(http_requests_total{job="api"}[5m]))
```


Processing that many samples in real time becomes computationally expensive. Most of our initial attempts to calculate an SLI availability score failed due to the excessive scale of these queries for larger time windows.


### Solution


Instead of computing SLIs on demand, we continuously update compact, indexed aggregates (pre‑computed counters and time‑windowed metrics) from incoming alerts. Dashboards and SLI calculations read these stores, while raw alerts remain available for debugging and audit.


**System workflow (at-a-glance)**


- Metrics/alerts are ingested as before.
- A policy evaluates incoming alerts and updates pre-aggregated counters/time windows in near real time.
- SLI reads use the aggregated stores; links to raw alerts support deep dives.


**Key Benefits**


- Faster reads and predictable scalability
- Preserved alert semantics and traceability back to raw events


**Trade-offs**


- We lose the ability to change past events (recorded scores become immutable)


### Phased Rollout Strategy


Introducing a new process requires clear thinking and planning in an enterprise setup. We realized quickly that mandating SLOs would not lead to adoption. Instead, we took advantage of a fortunate alignment: several high-visibility teams had already prioritized improving their reliability when the SLO initiative launched. We partnered with these teams to demonstrate value, using their feedback to champion SLOs within Box.


We saw incremental adoption as we progressed from one phase to another. Interestingly, newer teams were more eager to adopt SLOs as they had no existing monitoring and were easily sold on the value proposition.


***Phase 1 — Proof of Concept (1–2 teams, 1–2 weeks)***


**Validate SLI Quality and Signal-to-Noise Ratio**
During this phase, the focus is on ensuring that your Service Level Indicators (SLIs) accurately reflect user experience and system health. We assessed whether the selected metrics provide meaningful signals without generating excessive noise from irrelevant or low-impact events. This validation was critical — poorly chosen SLIs would lead to alert fatigue or missed incidents.


**Iterate Weekly Based on Feedback**


The pilot was inherently iterative. We planned to hold weekly retrospectives with participating teams to review what was working and what wasn’t. We used this feedback loop to refine SLI definitions, adjust measurement windows, tweak error budget policies, and improve alerting thresholds. Rapid iteration during the pilot ensured that by the time we scaled to more teams, our SLO framework was well-tested and aligned with real-world operational needs.


***Phase 2 — Hands-On Rollout***


We expanded SLO adoption to critical customer-impacting feature teams by providing dedicated support that included standardized dashboards, detailed runbooks, and hands-on training sessions. We established a continuous improvement loop by systematically collecting usage data and incident feedback from these teams. During post-incident reviews, we verified whether at-fault services actually breached their defined SLOs, and when no breach was detected despite customer impact, we refined the SLO indicators to better reflect real-world reliability expectations.


***Phase 3 — Standard Template***


We published reusable SLO templates with dashboard and alerts using declarative manifests through our internal observability as code framework. This approach enables version-controlled, reproducible monitoring configurations that can be deployed consistently across services and teams and complemented them with best practices that enabled teams to adopt service level objectives more consistently across the organization. By piloting these resources with minimal coaching, we effectively tested the scalability of our approach without requiring extensive hands-on support for every implementation.


***Phase 4 — Self-Service Platform***


As services grew, we automated the creation, validation, and provisioning of SLOs for all applications at Box via self-service platforms. The system provided on-demand documentation and lightweight training to help teams get up to speed quickly. We continuously monitored long-term coverage and reliability metrics to ensure our services met their objectives.


### Making It Usable


***SLOs must be readable and actionable*** *to drive meaningful behavior change.* Our dashboard design focuses on 4 key principles:


1. **Clear Visibility** : We present an explicit SLO score that teams can understand at a glance, showing current health and trend over time.
2. **Composite View** : A composite-SLO view that logically aggregates SLI breaches while avoiding duplicate error-budget counting across related services.
3. **Actionable Drill-downs** : Clear navigation from high-level SLO scores to pre-aggregated metrics and raw alerts for audit or deep-dive analysis.
4. **User Journey Mapping** : We provide clear descriptions of which customer experience is being measured by each SLI — these are called “User Journeys” and help teams understand the business impact of their metrics.


Press enter or click to view image in full size


Fig — All services SLO showing Box wide SLO breach view


A service-wide SLO view gives instant, global reliability status across all services. Benefit: rapid situational awareness — see what’s healthy, at risk, or breaching, and who’s burning error budget fastest. In an incident, use it to gauge blast radius, prioritize the highest-impact services, and drill down to affected dashboards and alerts for root cause.


Press enter or click to view image in full size


Fig- Service SLO dashboard


A service SLO dashboard shows a single service’s SLO score and trend, its error‑budget status, and which SLIs are breaching or at risk, with quick drill‑downs to pre‑aggregated metrics and raw alerts for root‑cause analysis.


Press enter or click to view image in full size


Fig — Composite SLO calculation


A composite SLO calculation combines multiple SLIs into one logical SLO, aggregating breaches while avoiding double‑counting shared failures. It yields a single, clear reliability score for an end‑to‑end user journey, with traceability to the underlying indicators.


### Cultural Transformation


As mentioned earlier, the main challenge we had to tackle was creating a culture of SLO-driven decision making for application-owning teams. This transformation happened over the course of several years, with the SRE organization consistently promoting adoption by demonstrating Engineering value.


Making Reliability Accessible: Our adoption strategy focused on **three key principles:**


- **Approachable** : Simplified onboarding and clear documentation
- **Observable** : Transparent metrics that teams could understand and act upon
- **Blameless** : Focus on system improvement rather than individual accountability


**Tactical Approaches:**


- **Office Hours** : SREs held regular sessions to answer questions and provide hands-on support
- **Reusable Templates** : Standardized SLO configurations that teams could easily adapt
- **Executive Reporting** : Trend reports that tied SLO health directly to business priorities
- **Success Stories** : Early wins from critical teams were showcased as case studies across the organization


**Breaking Down Barriers** : Leadership visibility was crucial in removing political barriers and normalizing SLO discussions in planning sessions and[incident postmortems](https://www.pagerduty.com/resources/digital-operations/learn/incident-postmortem/) . This shifted the conversation from blame assignment to measurable improvement, fundamentally changing how teams approached reliability.


## Conclusion


[Google’s SRE book makes SLOs](https://sre.google/sre-book/service-level-objectives/) sound like an obvious choice — something every organization should adopt to operate efficiently. But the reality is more nuanced. Every company faces its own unique challenges when implementing SLOs successfully.


Our SRE organization spent several years building a culture where SLOs became part of everyday engineering conversations. It wasn’t a quick win. The initiative required significant time, resources, and persistence to gain traction. We went through multiple attempts before we found the right value proposition that resonated with the broader engineering team.


While SLO implementation **at scale presents technical challenges** , the **greater hurdle was cultural** : establishing SLOs as a valuable engineering metric that teams genuinely prioritize.


### Lessons Learned


- **Timing matters:** Introducing SLOs is far easier in a company’s early stages. Once organizational culture is established, driving that kind of change becomes significantly harder.
- **Demonstrate value to gain adoption:** Show concrete benefits and quick wins to build momentum and buy-in across teams.
- **Centralized solutions need consistent foundations:** Deploying a uniform, template-based solution across teams is challenging when the underlying data (like metrics) lacks consistency. There are tradeoffs between simplicity of adoption and accuracy — sometimes standardization means accepting less precision in exchange for broader usability.
- **Cultural change is a marathon:** Bringing meaningful change takes time and persistence. Patience and consistent effort eventually pay off, but expect the journey to take years, not months.


## What’s next?


Even though SLO has seen a broader adoption within Engineering, we are still trying to improve our coverage as not all applications have adopted SLO. We are also working towards automations that regularly validates data quality and correctness of an application’s SLO and thereby creating a feedback mechanism to take action.


SLO is a continuous process and needs a culture of reliability and blamelessness to succeed.


## Acknowledgments


We would also like to acknowledge[Noah Gorka](https://www.linkedin.com/in/noahgorka/) and[Wojciech Krysmann](https://www.linkedin.com/in/wkrysmann/) for their valuable feedback on the content of the blog post.
