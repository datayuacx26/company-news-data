---
schema_version: "1.0.0"
document_id: "20aa01fd9a0efbbe55d92f2ad7483037bdebe8724cc7076f027d81a13256c71b"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-b653f977f25a"
canonical_url: "https://building.nubank.com/a-day-in-the-life-of-an-analytics-engineer-at-nubank/"
published_at: "2026-05-07T14:00:00+00:00"
first_seen_at: "2026-07-20T04:36:16.841262+00:00"
fetched_at: "2026-07-28T21:14:13.187831+00:00"
content_hash: "sha256:c6ac0c90992b751b49d674305093ebaab5a8232606a897f447782389eb22aa56"
---

# A day in the life of an Analytics Engineer at Nubank

*By Marlon Ferrari – Senior Analytics Engineer at Nubank*


If you’ve ever been curious about what an Analytics Engineer actually does day to day, including their responsibilities, tools, decisions, and what the pace of work looks like inside the world’s largest digital bank, this article is for you.


At Nubank,[Analytics Engineers](https://building.nubank.com/analytics-engineer-at-nubank/) operate between two worlds: on one side, engineering, where pipelines, architectures, and data systems must scale efficiently; on the other, the business, where those same systems need to answer real questions and support products used by millions of people.


Here, technical depth and organizational impact are complementary forces. The pipelines, schemas, and governance structures built by Analytics Engineers help shape how the entire company understands its data and, ultimately, how it makes decisions.


## How an Analytics Engineer starts the day at Nubank


The day usually begins with the squad meeting: a cross functional daily where Software Engineers, Data Scientists, Business Analysts, and Analytics Engineers align around the team’s priorities.


This is where blockers surface, dependencies become visible, and each person understands how their work contributes to broader goals. After that, it’s time to build, and Nubank’s technology stack reflects the scale of its operation:


**Spark and Scala** play a central role. Most of Nubank’s data processing runs on[Apache Spark, with code written in Scala](https://building.nubank.com/how-nubank-uses-spark-to-process-data/) , a choice directly tied to the scale of its operations.When a single dataset needs to process hundreds of millions of rows across multiple countries, a framework built for massive parallel processing becomes essential. On a daily basis, Analytics Engineers write, debug, and optimize Spark jobs as a core part of their work.


[Databricks](https://building.nubank.com/mastering-databricks-performance-with-spark-ui/) also plays a strategic role in the analytics workflow. Clusters, notebooks, job scheduling, and catalog management are all part of the routine within an environment that integrates with a broader data platform built by Nubank itself. For many Analytics Engineers, a significant portion of the day unfolds within this ecosystem.


Beyond that,[the internal pipeline ecosystem](https://building.nubank.com/scaling-data-analytics-with-software-engineering-best-practices/) supports this entire operation. Batch processing frameworks, incremental ingestion layers, and streaming interfaces form the infrastructure on which teams build data products. A deep understanding of these systems is essential not only for designing scalable solutions, but also for balancing performance with cost efficiency.


[Apache Flink](https://building.nubank.com/avalanche-stack-and-real-time-streaming-applications-at-nu/) , in turn, extends this work into the world of real time data. In pipelines that need to operate within milliseconds, Flink powers everything from feature generation for machine learning models to continuous event processing and low latency products that batch systems alone could not support. Analytics Engineers working closer to these domains expand their scope beyond traditional processing, also operating with Kafka and a broader streaming infrastructure.


And there is an important differentiator: at Nubank, we are ****[AI First](https://building.nubank.com/enhancing-engineering-workflows-with-ai-a-real-world-experience/) **.** Investment in AI driven applied engineering runs deep, and this is already part of the operational routine for Analytics Engineers. AI tools are integrated into nearly every stage of the workflow, from code generation and validation for pipelines to automated quality checks, accelerated code reviews, and navigation across an internal knowledge base with thousands of pages. In practice, this means faster iteration cycles, more robust validations, and data products reaching production with a level of confidence that would be difficult to achieve manually.


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## The core work: Building, governing, and translating data


Once the morning alignment is done, the core engineering work begins. The rest of the day balances collaboration and deep focus: mornings are often more coordination oriented, filled with syncs, discovery sessions, dashboard reviews, and alert monitoring; afternoons are dedicated to building, writing pipelines, investigating failures, optimizing performance, and reviewing pull requests.


Protecting this balance is one of the most important priorities within squads. Each team has autonomy to define its own ceremonies, rhythms, and work blocks, but there is a shared principle across the organization: having protected time for focused work is non-negotiable.


Below the[Data Science & Engineering umbrella](https://building.nubank.com/a-deep-dive-into-data-roles-at-nubank/) , the Analytics Engineer’s role is to build data products that the entire organization can use with confidence and at scale.


**Data modeling and transformation** form the backbone of this work. Across Nubank[more than 60,000 datasets](https://building.nubank.com/scaling-data-analytics-with-software-engineering-best-practices/) receive contributions from over a thousand people every month. Operational dashboards, datasets that power machine learning models, regulatory reports, and nearly every analytical asset depend on pipelines designed, built, optimized, and maintained by Analytics Engineers.


In this context, a poorly structured join can represent tens of thousands of dollars in computational costs over the course of a year. A neglected partitioning strategy can turn a fifteen minute batch pipeline into a four hour bottleneck, impacting entire chains of reporting at scale.


**Data governance** accompanies every stage of this process, because lineage must be traceable back to its source, schemas need to evolve without breaking, ownership must remain clear across teams, and costs require continuous monitoring. In a company operating across multiple countries and regulatory frameworks, governance encompasses privacy, compliance, quality, and operational accountability throughout the entire data lifecycle.


**Performance and cost optimization** are also indispensable here. An Analytics Engineer’s ability to fine tune a pipeline by choosing the right partitioning strategies, optimizing shuffle operations, or reducing unnecessary reads has a direct impact on the financial efficiency of the infrastructure.


However, perhaps the most important skill is something else entirely: **business translation** . Analytics Engineers frequently move between meetings with Product Managers to discuss churn, conversion, or other key metrics, and technical environments where those concepts must be transformed into reliable datasets and robust architectures.


Listening to a business problem and translating it into a trustworthy, scalable, and reusable data structure is at the core of the role. At Nubank, there is no rigid separation between analytics and engineering. Analytics Engineers also operate in the analytical layer, but with the technical depth required to build what the analysis demands.


**Pull request reviews** carry their own strategic importance. When reviewing another Analytics Engineer’s pipeline, the focus goes far beyond code alone. Schema decisions, governance adherence, performance trade offs, and alignment with squad level data contracts are all part of the evaluation.


These reviews often cross borders, time zones, and regulatory contexts, requiring global consistency across data standards. On a daily basis, Analytics Engineers work side by side with:


- Software Engineers, improving data quality at the source
- Data Scientists, structuring features and analytical products
- Business Analysts, defining reporting requirements
- Product Managers, aligning metrics with business goals


In an organization with more than a hundred autonomous squads, Analytics Engineers often serve as the central link connecting these different layers.


The operation itself is supported by Nubank’s[Squad and Chapter matrix](https://building.nubank.com/distributing-the-data-team-to-boost-innovation-reliably/) . Squads have autonomy over their domains and execution pace, while the Analytics Engineering Chapter functions as a horizontal layer that promotes knowledge sharing, technical alignment, and collective growth.


Each Analytics Engineer belongs to a squad’s mission, but is strengthened by the shared expertise of the Chapter. In practice, AEs own their data domains end to end and have the autonomy to make architectural decisions. At the same time, they are responsible for understanding that these choices directly impact products and services used daily by millions of customers.


At Nubank, there is no extensive approval layer separating engineers from production, and ownership remains with those who build.


## Beyond the day: How Analytics Engineers learn and grow


Not all learning happens within the daily operational flow. At Nubank, some of the most valuable moments in an Analytics Engineer’s journey happen precisely outside the traditional delivery routine: in ceremonies, rituals, and spaces where AEs continuously teach, learn, and challenge one another.


Imagine, for example, an Analytics Engineer working on cost optimization within a specific squad who discovers a new pattern for reducing computational consumption in pipelines. Within days, that learning can spread to dozens of other Analytics Engineers through the Chapter’s knowledge sharing mechanisms, being applied across multiple products, business domains, and countries.


This ability to rapidly disseminate knowledge is one of the greatest strengths of Nubank’s Chapter model, and the weekly Knowledge Sharing sessions are the backbone of this ecosystem. Every week, AEs from different squads come together to present technical deep dives, explore new approaches, discuss architectural decisions, and share feedback on ongoing projects.


It is one of the fastest ways to learn how other teams are solving challenges you may not have even encountered yet.


**Internal workshops** deepen this development even further. At Nubank, the Chapter leads structured, hands on training programs that cover everything from[Scala and Spark workshops](https://building.nubank.com/scala-hands-on-a-path-to-build-and-manage-queries-in-spark/) focused on building and managing data pipelines at scale to advanced sessions on Databricks performance tuning, data governance, and pipeline architecture.


These trainings are built by Analytics Engineers for Analytics Engineers, grounded in the real challenges faced daily within Nubank’s own technology stack.


**Hackathons** add a different layer of innovation. Nubank organizes company wide hackathons, including initiatives dedicated to[LLMs and artificial intelligence](https://building.nubank.com/nubank-llm-hackathon-lessons-learned/) , where Analytics Engineers collaborate with engineers, data scientists, and product teams to prototype solutions in accelerated cycles.


These events frequently generate tools, frameworks, and ideas that evolve into real production applications.


**Public meetups** expand this knowledge beyond the company itself. The[Building Nu Analytics Engineering Meetup](https://www.youtube.com/watch?v=wPPKmZILlZ0) series shares with the broader community how the Chapter operates, how the discipline evolves, and which real world challenges are part of the role’s daily routine.


Beyond strengthening Nubank’s technical brand, these events also contribute to the broader data ecosystem. Together, these four channels keep the Chapter connected and ensure that knowledge generated within one squad does not remain isolated. It circulates continuously, regardless of seniority, geographic location, or domain expertise.


This dynamic accelerates not only individual growth, but organizational evolution as well. A key part of this success comes from the diversity of backgrounds within the Chapter itself. Computer scientists, statisticians, economists, and professionals with less traditional data career paths collaborate constantly, bringing complementary perspectives to complex problems.


In the end, this combination of structured knowledge sharing, autonomy, and technical diversity transforms learning into one of the core engines behind the evolution of Analytics Engineering at Nubank.


## Five stages of an Analytics Engineer


The Analytics Engineer career at Nubank follows a structured leveling framework that supports progression from the earliest steps in the field to roles focused on organizational architecture.


Each stage carries its own expectations around autonomy, technical complexity, depth of knowledge, and organizational impact. Understanding this journey is valuable both for those exploring their entry into the data space and for experienced professionals evaluating their next career moves.


### The Apprentice – Associate Analytics Engineer (IC3p)


This is where the journey begins. At this stage, tasks tend to be more straightforward, clearly defined, and supported by close supervision. More experienced engineers have usually already structured the problem and outlined the initial path, leaving the Associate responsible for execution, learning, and building foundational experience.


The primary focus is developing strong technical fundamentals. This includes learning how data warehouses function in practice, how dimensional models are structured, how to write efficient queries that respect partitioning strategies, how to avoid unnecessary full scans, and how governance policies apply to the datasets being handled.


At this point, the world of data often feels like an entirely new map being discovered. It is when professionals begin building technical vocabulary and systems thinking, understanding, for example, what lineage means beyond theory or why an efficient pipeline today may become tomorrow’s bottleneck as scale increases.


Within this context, Associates begin developing a critical mindset. The expectation is not simply to ask “how to do it,” but to become genuinely interested in “why it works this way.”


Curiosity, rapid learning, and the ability to absorb foundational concepts are the key differentiators at this stage. Core skills at this level include:


- Data architecture fundamentals
- Basic query engineering
- Introduction to Spark and Scala
- Object oriented programming
- Foundational data governance


This is the phase of building technical foundations and systemic thinking, the elements that will support all future progression.


### The Builder – Analytics Engineer (IC4)


At the Analytics Engineer level, autonomy begins to solidify. Professionals at this stage can independently handle routine responsibilities such as building datasets, implementing quality checks, investigating pipeline failures, and maintaining analytical operations without constant supervision.


More complex challenges may still require guidance, but day to day operational work increasingly becomes the direct responsibility of the AE. This is also the stage where professionals move beyond simply following established patterns and begin developing their own solutions, advancing from execution into active architectural contribution.


Root cause analysis becomes an essential skill. When a pipeline fails, the objective is not simply to restore operations, but to deeply understand the origin of the issue, whether it involves data skew, upstream schema changes, computational bottlenecks, or infrastructure limitations.


Spark, for example, evolves from being just a tool into a complex distributed system with its own partitioning logic, shuffle operations, execution plans, and resource management. As a result, Analytics Engineers begin making more sophisticated technical decisions directly tied to scalability.


At the same time, governance shifts from being a set of policies to follow into something actively implemented within the solutions being built. Collaboration in architectural decisions also expands, bringing the professional closer to broader system design discussions. Core skills at this stage include:


- Practical data modeling
- Spark pipeline development and maintenance
- Implementation of governance practices
- Technical diagnostics and failure analysis
- Participation in architectural decisions


### The Autonomous – Senior Analytics Engineer (IC5)


At the Senior Analytics Engineer stage, independence is no longer a goal. It becomes a defining characteristic of the role. At this level, professionals already operate with a high degree of autonomy, executing complex projects, making significant architectural decisions, and taking direct responsibility for higher impact deliverables.


While support still exists for particularly complex challenges, the expectation is that senior professionals can solve most problems through their own judgment. This is also the point where leadership begins to expand.


Beyond building, Analytics Engineers start leading initiatives, influencing technical direction, and mentoring less experienced professionals. Cross functional collaboration becomes inevitable. Senior Analytics Engineers frequently work across squad boundaries, product areas, and technical disciplines, connecting business needs to data solutions at multiple levels.


From a technical perspective, this is the stage where professionals design and implement massively parallel processing pipelines for a wide range of scenarios, balancing scalability, performance, cost, governance, and operational reliability.


Governance, in turn, evolves beyond simply following established guidelines and becomes something actively shaped by the professional. Senior AEs begin translating business processes into data policies, overseeing classifications, driving cost reduction, and raising organizational standards.


Another important milestone is the balance between innovation and stability, as Analytics Engineers must be capable of introducing improvements and new approaches without compromising systems that are already critical to operations. This structural shift becomes clearly visible in the environment around them.


At this stage, colleagues begin seeking your perspective, trusting your technical judgment, and relying on your architectural instincts for important decisions. Core skills at this level include:


- Complex pipeline optimization
Technical leadership and mentorship
Cross functional influence
Strategic data governance
Alignment between data architecture and business objectives


### The Strategist – Lead Analytics Engineer (IC6)


At the Lead Analytics Engineer level, the scope shifts significantly. Challenges are no longer concentrated around clearly defined problems and instead expand into broader, more complex, and often ambiguous issues that span multiple squads, business units, and data domains.


At this stage, responsibility is no longer limited to specific pipelines or datasets. Analytics Engineers operate across entire ecosystems. This means coordinating projects that depend on alignment across multiple teams, harmonizing technical and strategic priorities, and building structures capable of sustaining broader operations.


The role increasingly combines technical depth with organizational leadership, and influence becomes a core part of the function. Lead Analytics Engineers help define the standards that other teams follow for data quality, pipeline architecture, governance, operational best practices, and organizational scalability.


Another important milestone is direct involvement in shaping the organization itself. At Nubank, participation in hiring processes becomes an expected part of the role. This means Lead Analytics Engineers actively help define who joins the team, contributing not only to technical execution, but also to talent development and the long term growth of the discipline.


At this stage, impact is no longer measured solely by the systems a professional builds, but by the ecosystems they enable, including platforms, structures, and standards that expand the capabilities of multiple teams simultaneously. Influence extends far beyond the immediate squad. Core skills at this level include:


- Leadership of cross functional projects
- Definition of architectural standards
- Organizational governance
- Hiring and talent development
- Ownership of large scale data systems and platforms


### The Architect – Staff Analytics Engineer (IC7)


At the Staff Analytics Engineer stage, the role reaches its highest strategic level within the individual contributor track. Here, autonomy is at its maximum and supervision is minimal, because Analytics Engineers no longer simply execute or lead projects. Their technical leadership directly helps shape the roadmap of their area and influences the structural direction of the organization.


The focus shifts away from point solutions for specific challenges. Instead, the work revolves around creating scalable frameworks, reusable standards, and architectural structures capable of solving entire classes of problems at the Business Unit level.


These solutions are designed for broad adoption across dozens of squads and multiple domains, expanding organizational capacity in a systemic way. At this stage, Analytics Engineers become some of the primary technical references within their business units. They are the professionals managers turn to for critical architectural decisions, senior ICs consult when facing complex systems, teams rely on for strategic troubleshooting, and leadership depends on for long term structural decisions.


Beyond technical depth, there is also a high expectation of business understanding. Staff Analytics Engineers must understand the organization deeply enough to anticipate technical needs before they become urgent. This means not only responding to problems, but predicting their arrival and proactively structuring solutions in advance.


Staff Analytics Engineers also mentor engineers through IC5, helping develop new technical leaders and strengthening the organization’s capabilities as a whole. At this level, the nature of the work changes fundamentally. It is no longer simply about solving problems, but about defining how problems should be solved.


This distinction represents the full transition into strategic architecture. Core skills at this level include:


- Business Unit level architectural design
- Development of scalable frameworks
- Advanced technical mentorship
- Strategic organizational troubleshooting
- Influence over company wide data strategy


## Advice for future Analytics Engineers at Nubank


For those considering building a career as an Analytics Engineer at Nubank, certain patterns consistently appear among the candidates who stand out the most. And the perspective of those who have already gone through the process reveals three especially important lessons.


### 1. Think about the business, not just the code


At Nubank, technical excellence is essential, but it is not enough on its own. The hiring process includes live data modeling case studies, and the strongest candidates often distinguish themselves not by immediately jumping into implementation, but by first taking the time to understand the problem.


Before writing any query or designing any architecture, it is critical to ask questions such as:


- What business problem are we solving?
- Who will use this data?
- Which decisions will depend on this structure?
- What trade offs exist between performance, completeness, and scalability?


The expectation is not simply to evaluate your implementation skills, but your ability to think like someone who is already part of the team. In other words, approach the case as though you are already working on the project.


### 2. Demonstrate clarity when navigating trade offs


- Analytics Engineering is, to a large extent, the discipline of making strong decisions within constraints. In practice, this means constantly managing tensions such as:
- Data freshness versus operational cost
Schema flexibility versus query performance
Urgent squad specific demands versus organizational platform stability
Delivery speed versus long term governance


During interviews, the quality of your reasoning often matters more than the final answer itself. The goal is to understand how you structure decisions when faced with real ambiguity. At Nubank, we value professionals who can solve complex problems, not just provide quick answers.


### 3. Bring curiosity, regardless of your background


There is no single path into Analytics Engineering. The Chapter brings together professionals from diverse backgrounds, including computer science, statistics, economics, and less conventional career paths. What unites them is not a specific academic discipline, but shared characteristics such as:


- Intellectual curiosity
Comfort with ambiguity
Genuine interest in complex problems
A desire to deeply understand how businesses operate


At a company that operates with an “it’s still day one” mindset, there will always be new systems to understand, new challenges to solve, and new opportunities to build. That is why curiosity is such a central part of the role.


In summary, for future Analytics Engineers at Nubank: think strategically, navigate trade offs with clarity, and cultivate curiosity. The combination of technical depth, business perspective, and continuous learning is what most strongly differentiates professionals who thrive in this environment.


## Build the purple future


Analytics Engineers at Nubank work across more than a hundred squads, maintaining over 60,000 datasets that serve more than 100 million customers across multiple countries. The Chapter continues to grow alongside the scale of our challenges.


If this is the kind of work you want to do, explore open Analytics Engineering roles at Nubank.
