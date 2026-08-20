---
schema_version: "1.0.0"
document_id: "37cbf2c2682d94468a9ded9ebdfb83b0e33c1378af6ee70af7fd4948c28a5e75"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/artificial-intelligence/ai-value-paradox-measuring-what-matters-not-just-whats-easy"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:46a2be5528c56c1b97e5f76205eb24b5fd4186ef542b0db1032179702f57851b"
---

# The AI value paradox: Measuring what matters (not just what’s easy)

The rapid evolution of AI has a remarkable ability to split a room, and the tension between the two sides illustrates the challenges in scaling AI on an enterprise level.


On one side are the “new tech” enthusiasts, eager to experiment with the latest tools and explore what AI can do. These early adopters help demonstrate AI’s potential on an individual level and encourage others to do the same. However, their efforts are rarely scalable across the enterprise, where value comes from processes and teams working together, and not necessarily from maximizing individual productivity.


On the other side are the skeptics and risk-averse, concerned about data leakage, poor advice, job displacement, or worse. Unfounded or not, these fears hold back adoption and scalability within organizations. While change management, as well as regulations, playbooks, and responsible AI governance frameworks, can help address these fears, they remain a major barrier to adoption and scalability.


Between these extremes lies the real opportunity, which is understanding and demonstrating the actual value AI can create. This removes the hype, fear, and vague promises that “AI will fix it.” If organizations can measure the value, they can scale AI successfully.


But, as anyone who has ever worked on an AI project knows, measuring AI value is harder than it looks.


### Why AI value measurement matters


Before discussing stakeholders, KPIs, or timelines, one question comes first: **Why measure AI value at all** ?


AI projects often begin with high expectations, but enthusiasm is not a sustainable investment strategy. Our experience shows that without a structured approach to defining and measuring value, organizations face three predictable risks:


> 1. **Unclear problem statements leading to vague success criteria**
> Teams build impressive solutions without clearly defining what success looks like, or they define success in a way that’s incomplete or disconnected to business value. This leads to exploratory pilots centered on tools and capabilities, instead of solving actual problems.
> 2. **Misaligned stakeholder expectations**
> We all know the story; everyone appears to be on the same page. However, users expect time savings. Process owners expect throughput. Business leaders expect cost reductions. Technology leaders expect architectural progress. And, compliance just wants to sleep at night. Without measurable value, priorities remain unspoken and then collide.
> 3. **Difficulty securing ongoing investment**
> When value can’t be demonstrated, funding, scaling, and trust quickly erode.


Value measurement is sometimes viewed as bureaucracy (“We need to measure something.”) or a barrier to moving quickly, but it’s strategic. It strengthens decision-making, enables responsible scaling, and ensures that teams work toward outcomes that genuinely matter for organizations and their clients.


### Measuring value must be repeatable and scalable


Success measurements in early AI pilots are rarely representative of enterprise-wide adoption and outcomes. For example, averaging observed improvements in time, cost, and workflows for early adopters often overestimates the benefits for wider organizational users. This makes the reliability of value measurement critical.


CGI’s applied research teams leverage a[best practice approach](https://arxiv.org/pdf/2302.06590) for productivity benefit measurement. This scientific and statistical rigor ensures productivity benefits are repeatable and defensible, even with small sample sizes (as is often the case with AI pilots), including the following two statistical tests:


- **Paired t-tests** to measure the statistical significance of differences between manual effort versus AI-assisted work
- **One-tailed t-tests** to validate whether specific productivity gains are observed


Through this statistical approach, organizations can ensure improvements are attributable to AI, not randomness, and generate meaningful conclusions without massive data volumes.


The next challenge is collecting the right data across the AI life cycle. This is done in two steps, as described below.


Step 1: Map stakeholders and timelines—together


It’s important to understand that value is stakeholder dependent. So, the first question is not, “What does AI do?” but rather, “Why are we using it, and for whom does it matter?”


Stakeholders typically include:


- Users (who will either love the AI tool or try to avoid it)
- Technology teams (the implementers and integrators)
- Customers (internal or external)
- Process owners (whose KPIs you may unintentionally affect)
- Compliance and risk teams (who will have opinions, most definitely)
- Senior management or sponsors (who want to see a positive impact on their strategy)
- Society or regulatory bodies (depending on your domain)


AI tools will impact these stakeholders in different ways—from the experiences of users and customers, to the KPIs of process owners, to the risks faced by compliance teams, to the ROI expected by senior sponsors.


**Case example: Applying AI to manufacturing operations**


A manufacturer deployed AI to improve efficiency, reduce costs, and increase reliability across research and development, production, supply chain, and logistics operations. Early results looked promising with better predictions, richer dashboards, and local efficiency gains. However, lead times remained unchanged, costs were still volatile, and people running daily operations felt there was little overall improvement.


Leadership realized the challenge is not choosing the right AI tools but accurately measuring their success. Judging AI solely by technical performance or isolated improvements, for example, did not demonstrate overall operational outcomes.


They decided that AI should be evaluated by its impact on three outcome-oriented KPIs:


- **System efficiency** : end-to-end throughput time, overall equipment efficiency (OEE), meantime between failures (MTBF), schedule adherence, energy consumed per finished unit
- **Costs** : cost per good unit, unplanned downtime cost, energy cost per order, engineering change cost
- **User experience (predictability and effort in daily work):** operator intervention frequency, maintenance effort per incident, planning stability, engineering feedback cycle time


With these measures, production stabilized, “firefighting” decreased, and feedback loops between engineering and operations shortened. The AI models didn’t fundamentally change. However, evaluating their success using more meaningful measures enabled their value to become more visible and scalable.


***Connect value to the AI life cycle***


Value evolves over time, and KPIs must evolve with it. For example, expecting enterprise-scale value in a pilot guarantees disappointment.


KPIs should be aligned with each project phase:


- **Envisioning pilot** : business and technical feasibility and proof of value (accuracy, precision, automation rate)
- **Early adoption** : operational improvement potential (time saved, reduced rework)
- **Engineering and scaling** : business impact, interoperability across enterprise, strategic alignment
- **Continuous improvement** : long-term outcomes, optimization, drift management


***Create a value matrix***


Value is not only stakeholder dependent, but also time dependent. Understanding **who** benefits and **when** they benefit forms the foundation for everything that follows.


To keep this organized, we use a **value matrix** , which enables us to systematically map intended outcomes and stakeholder expectations.


Use a value matrix in which:


- Columns = stakeholders
- Rows = life cycle phases
- Cells = expected benefits


This matrix becomes the backbone for prioritization, expectation management, and measurement. If a benefit cannot be placed, the use case may be too vague, or not valuable.


**Stakeholder 1**


**Stakeholder 2**


**Stakeholder 3**


**…**


**Envisioning (pilot)**


benefit 1


benefit 2


…


**Exploratory (early adoption)**


**Engineering (and scaling)**


…


**Expansion (for continuous improvement)**


Step 2: Choose metrics that will survive a tough review


Metrics should monitor and measure promised benefits and not simply serve as vanity metrics for dashboards.


Good metrics meet three conditions:


- **Causal plausibility** : AI can reasonably influence the metric.
- **Data availability and quality** : The metric can be measured reliably. This doesn’t need to be quantitative (i.e., time, cost, effort); it can be qualitative (i.e., user experience via interviews or surveys).
- **Robustness** : External factors can be explained or controlled.


Examples include:


- **Efficiency KPIs** : Measure time and effort before and after AI assistance; the baseline manual effort needs to be measured.
- **Cost-benefit KPIs** : End-to-end costs of current technology, licenses, manual effort, wait times, errors, and manual processes help to identify true cost savings; automation also has costs beyond licenses, including consumption, ongoing oversight, and maintenance.
- **User experience** : Ease of use, accuracy, adaptability, and satisfaction not only of the end users but also of the teams who implement and manage the solutions.


These metrics also help illustrate that not everything should be automated. AI must justify its own economics.


### Measuring value makes AI real


AI becomes a strategic asset only when its value is visible, defensible, and connected to real objectives. A structured methodology, value matrix, and life cycle KPIs make progress measurable. Further, transparent communication builds trust.


Here are a few practical recommendations for moving forward:


> 1. Define the problem, stakeholders, timeline, and metrics—in that order.
> 2. Measure value across financial, operational, strategic, and human dimensions.
> 3. Use statistical methods to ensure repeatability.
> 4. Choose KPIs AI can genuinely influence.
> 5. Assign clear business ownership for every KPI; AI doesn’t create value unless someone is accountable for acting on it.
> 6. Align expectations to each project phase; pilots are for learning, not for delivering enterprise value.
> 7. Make results visible because value uncommunicated is value unrealized.


When value is measured properly, AI moves from hype to impact, delivering results leaders can trust and act on. For further discussion, reach out to one of us below.


We want to thank our colleague[Helena Jochberger](https://www.cgi.com/en/blog/manufacturing/scaling-augmented-industrial-data-for-real-world-results) for her contribution to this blog. Also, learn more about CGI’s[artificial intelligence](https://www.cgi.com/en/artificial-intelligence) capabilities and work.
