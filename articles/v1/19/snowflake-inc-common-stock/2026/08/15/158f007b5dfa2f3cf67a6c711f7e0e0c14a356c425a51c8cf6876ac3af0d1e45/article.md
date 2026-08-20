---
schema_version: "1.0.0"
document_id: "158f007b5dfa2f3cf67a6c711f7e0e0c14a356c425a51c8cf6876ac3af0d1e45"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/cto-circle-ai-native-engineering"
published_at: "2026-08-05T16:39:00+00:00"
first_seen_at: "2026-08-05T18:25:20.391643+00:00"
fetched_at: "2026-08-05T18:25:22.128211+00:00"
content_hash: "sha256:13873c6c5eeee89ab66d6d3c0b95cff867f037204c149ffa5f8a8fc54877e208"
---

# CTO Circle: Lessons on Building AI-Native Engineering Teams

Every engineering leader is trying to answer the same questions as AI is changing the rules: How should engineering teams be organized? Where should organizations invest as foundation models continue to improve? How do you increase engineering velocity without introducing unacceptable operational risk? What is the difference between AI-augmented and AI-native organizations?


There is no established playbook for building an AI-native engineering organization and few opportunities for engineering leaders to openly compare what they're learning. These conversations often remain inside individual companies and are shaped by competitive pressures and rapidly evolving technology.


As the platform where thousands of organizations build their data and AI strategies, Snowflake is uniquely positioned to convene these conversations. CTO Circle was created to give engineering leaders a trusted forum to learn from peers navigating the same transformation and to challenge assumptions. Held during Snowflake Summit 2026 in San Francisco, the inaugural event brought together more than 350 CTOs from various industries including financial services, telecommunications, retail and technology to exchange practical lessons.


The discussion moved well beyond coding assistants and model selection. Instead, it focused on how engineering organizations are being redesigned, what is working in production, and where leaders are investing to create long-term competitive advantage. The discussion converged around the following three themes that define what it means to build an AI-native engineering organization: using AI in production, balancing velocity and risk, and designing engineering teams for AI.


## AI in production: What breaks, what scales


Many organizations begin their AI journey by introducing coding assistants into existing engineering workflows. Developers write code a little faster and documentation becomes easier to produce. These improvements are meaningful, but they leave the underlying engineering system largely unchanged.


[Vivek Raghunathan](https://www.linkedin.com/in/raghunathanvivek/) , SVP of Engineering at Snowflake, challenged leaders to think much more broadly about AI adoption. Vivek argued that building an AI-native engineering organization starts with a shift in management philosophy. Rather than viewing developer productivity as an engineering or culture problem, Snowflake began treating it as a product.


"What if you treated your developers like customers?" That question became the foundation for how Snowflake approached its own engineering transformation. Instead of assuming that leadership knew what engineers needed, the team applied the same product management principles they use to build customer-facing products.


They interviewed developers to understand where work slowed down and mapped points of friction across the software development lifecycle. The team then established baseline metrics and ran experiments to gauge the impact of every change. The approach combined clear executive sponsorship with bottom-up adoption. This ensured that improvements reflected how engineers actually worked rather than how leaders expected them to work.


Figure 1: An AI-native approach.


The results were measurable. In 18 months, Snowflake increased its internal developer Net Promoter Score by more than 30 points. This resulted in a 4:1 ratio of satisfied to dissatisfied developers within that time frame. More importantly, that improvement translated into an engineering organization capable of delivering software more efficiently and adapting more quickly as AI capabilities continued to evolve.


Figure 2: Customer satisfaction with overall developer velocity at Snowflake.


Vivek also explained that adoption alone is not enough to drive meaningful productivity gains. The high leverage comes from the depth of usage and true mastery of the tools at scale. At Snowflake, that journey has evolved through three stages:


- **Adoption:** Begins when developers learn to use AI tools in their daily work.
- **Mastery:** Develops as engineers discover repeatable workflows that consistently produce better outcomes.
- **Optimization:** Occurs when those workflows become organizational knowledge that every engineer can benefit from.


Figure 3: AI-augmented versus AI-native organizations.


One example is the collection of engineering design patterns developed internally at Snowflake. Early AI adopters experimented with prompting techniques, planning methods and debugging approaches. Over time, the organization documented the patterns that consistently delivered better results and made them available across engineering.


Every developer had access to the same AI tools, but the engineers using proven workflows consistently outperformed those stuck at the early levels of tooling adoption. The competitive advantage came from institutionalizing successful ways of working rather than simply deploying another AI assistant.


This emphasis on workflows also changes how organizations think about software development itself. As AI reduces the effort required to transform ideas into working software, everyone becomes a builder. Product managers can prototype new experiences, designers can validate concepts directly in code, and domain experts can add guardrails for other personas. Instead of debating ideas through presentations or documents, teams can validate them by building working software. Code increasingly becomes the fastest way to test assumptions.


[Jon McNeill](https://www.linkedin.com/in/jonmcneill1/) , author of *The Algorithm* , expanded on this theme by encouraging leaders to reconsider assumptions that have shaped engineering organizations for decades. Too often, companies begin with the technology and search for places to apply it. Jon argued that successful organizations reverse that equation. They begin by identifying the few business constraints that matter most and redesign their engineering systems around solving those problems. As organizations move AI into production, success will be measured less by who generates the most code or consumes the most tokens and more by who builds the simplest, fastest and most effective engineering system. Perhaps the more interesting leaderboard will not consist of engineers generating the most AI code or consuming the most tokens; rather, it will include engineers who have eliminated the most unnecessary steps between an idea and production.


## Maximizing velocity, containing risk


As AI becomes embedded throughout the software development lifecycle, engineering leaders face a second challenge. Every improvement in development velocity also increases the need for reliable systems and operational discipline. The organizations moving fastest are investing in architectures that make both possible.


[Jeremy Burton](https://www.linkedin.com/in/jburton0/) , General Manager of the Observability Business Unit at Snowflake, argued that AI has entered a new phase. Early experimentation is giving way to production deployment, and organizations are increasingly expected to demonstrate measurable business value rather than isolated technical successes. At the same time, AI introduces entirely new operational challenges. AI agents generate more telemetry, interact with more systems and make decisions using information distributed across increasingly complex environments. This changes the role of observability from monitoring infrastructure to providing the context AI systems need to operate reliably.


Figure 4: New AI workloads drive more telemetry and more complexity.


Jeremy challenged one of the most common assumptions in enterprise AI that says that better models simply need better data. In reality, AI is only as effective as the context it can access. That context extends far beyond raw telemetry. It includes the semantics that describe what data means, the relationships captured through ontologies and knowledge graphs, and the business context that connects systems. Just as important is how that context is accessed. AI agents need standardized interfaces, such as APIs, CLIs and Model Context Protocol (MCP), to reliably retrieve and act on information, while engineers need intuitive ways to drill into data and validate AI-generated insights.


Rich context combined with open access transforms disconnected telemetry into an environment where both humans and AI can reason effectively, investigate problems faster and make better decisions. Organizations that continue to store logs, metrics, traces, operational data and business context across disconnected systems make it difficult for AI to reason accurately about production environments. Instead, engineering telemetry should be treated as data that exists within a common foundation where relationships between systems can be understood and queried.


[Aditya Gaur](https://www.linkedin.com/in/adityagaur/) , Engineering Manager at Netflix, demonstrated what this looks like in practice through the company's work on automated root cause analysis. Although the project is often described as an AI initiative, Aditya explained that its success depended far more on data architecture than on AI itself.


Years before introducing AI agents, Netflix invested in connecting fragmented telemetry across systems, modeling operational relationships through an ontology and knowledge graph, and creating a shared context layer. By the time AI entered the picture, the foundation already existed. Rather than searching across disconnected logs and dashboards, AI agents could reason over structured operational knowledge and generate far more meaningful hypotheses during incident investigations.


[Caitlin Colgrove](https://www.linkedin.com/in/colgrove/) , CTO at[Hex](https://hex.tech/) , approached velocity from a different perspective. She urged leaders to rethink what it means to move fast in the AI era. Organizations cannot afford to be partially AI native. They eventually reach a point where they must commit fully by restructuring how teams build products, make decisions and incorporate AI into everyday work. She described this as "burning the boats." AI has to become foundational to how the business operates.


Hex initially responded to the arrival of generative AI by creating a dedicated AI product team. While the approach delivered useful features, it also created an organizational bottleneck. Hex ultimately disbanded the centralized AI organization and distributed responsibility across every product team. Today, Hex delivers product rapidly and embeds AI capabilities throughout the product because ownership lives with the engineers closest to the customer problem.


[Chris Kozlowski](https://www.linkedin.com/in/chris-kozlowski-2ba0395/) , Managing Director at Barclays, brought an enterprise perspective to the discussion and highlighted that speed only creates value when it is matched with governance and trust. In highly regulated environments, engineering teams cannot afford to choose between innovation and control.


[Corey Burke](https://www.linkedin.com/in/coreyb78/) , SVP Engineering at[Dialpad](https://www.dialpad.com/) , and[Arun Rajamanickam](https://www.linkedin.com/in/arunrajamanickam/) , VP of Engineering at[project44](https://www.project44.com/) , described how AI is changing the pace of software development. As AI agents become capable of implementing significant portions of a feature independently, engineers spend less time writing code and more time defining intent, orchestrating multiple agents and validating outcomes.


They emphasized that maximizing velocity requires building engineering platforms that allow teams to experiment quickly without compromising reliability. AI shortens the path from identifying a customer problem to validating a solution, but only if teams have the infrastructure, shared context and operational discipline to iterate confidently.


## Designing engineering teams for AI


If AI changes how software is built, it inevitably changes how engineering organizations are designed. It requires new team structures, new leadership models and a different understanding of where engineers create the most value.


[Qi Jin](https://www.linkedin.com/in/qijin/) , EVP at[Cerebras Systems](https://www.cerebras.ai/) , suggested that organizations optimized for previous generations of software development are often the biggest barrier to AI adoption. Established processes and organizational boundaries were designed to scale proven ways of working, not to continuously adapt to disruptive technologies. Successfully adopting AI therefore requires more than introducing new tools. Leaders need to rethink operating models so teams can quickly incorporate new capabilities as the technology evolves.


Qi described this transformation through the concept of "Code Yellow," a framework for driving organizational change with urgency and a focus on customer problems. His perspective echoes a broader leadership philosophy he has written about in his[essay](https://medium.com/@qijinibm/advice-for-the-wartime-manager-737a940a98a6) on the “wartime manager,” where he argues that periods of rapid disruption demand leaders who simplify decision-making, move decisively, and are willing to challenge established norms rather than optimize existing processes.


[Parvez Naqvi](https://www.linkedin.com/in/parvez/) , Managing Vice President of Cloud Platforms, Resilience Engineering and Enterprise Architecture at Capital One, elaborated on how these organizational changes are reshaping the role of engineers themselves. As AI makes implementation dramatically faster, the value of engineering shifts toward the decisions that AI cannot make independently. The primary constraints on software delivery now include planning, architectural design and engineering judgment.


Traditional review processes that evolved around manually written code begin to break down as AI dramatically increases development velocity. Senior engineers increasingly create leverage by defining technical direction and establishing architectural patterns. They are also becoming increasingly involved with system design and providing the context that allows both engineers and AI agents to make better decisions.


Figure 5: CTO Circle panel discussion on the changing role of data engineers, featuring Jeremy Burton, General Manager of the Observability Business Unit at Snowflake, and Parvez Naqvi, Managing Vice President at Capital One.


This evolution also elevates the importance of developer platforms and engineering infrastructure. Internal tooling and deployment systems become strategic assets because they reduce the operational burden and allow teams to focus on designing resilient systems instead of managing repetitive work. As more product managers, designers and domain experts contribute directly to software creation, engineering organizations increasingly become stewards of quality, architecture and operational excellence rather than the sole producers of code.


[Harjot Gill](https://www.linkedin.com/in/harjotsgill/) , CEO at[CodeRabbit](https://www.coderabbit.ai/) , shared how AI is fundamentally changing the developer experience. As AI takes on more of the implementation work, engineers spend less time translating ideas into code and more time defining intent, evaluating tradeoffs and shaping system design. AI-assisted reviews can help engineering teams keep pace without compromising correctness, security or maintainability.


[Jordan Topoleski](https://www.linkedin.com/in/jtopoleski/) , COO at[Cursor](https://cursor.com/) , focused on what happens after code is generated. As AI dramatically increases development velocity, code review, validation and the maintenance of engineering quality become the new bottlenecks. Jordan’s perspective reinforced the idea that organizations that scale successfully build stronger feedback loops and engineering systems that ensure AI-generated software is ready for production.


Figure 6: CTO Circle panel discussion featuring Shruti Bhat, Head of Marketing at Observe by Snowflake; Jordan Topoleski, COO at Cursor; Harjot Gill, CEO at CodeRabbit; and Jacob Leverich, Head of Technology at Observe by Snowflake.


Viewed together, these discussions point toward a broader transformation. AI-native engineering is defined by how quickly an organization can learn and institutionalize better ways of building software. The companies that move fastest will be those that continuously redesign their engineering organizations to take advantage of every new advance in AI.


## Looking ahead: Conversations that will shape what’s next


The conversations throughout the day reflected an industry that has moved well beyond experimentation. Although every organization is at a different stage of its transformation journey, the challenges are remarkably consistent. The feedback from attendees reinforced our vision for CTO Circle. Engineering leaders consistently highlighted the opportunity to engage in candid conversations with peers facing similar decisions. What made the event valuable was the opportunity for engineering leaders to have candid conversations they rarely get to have elsewhere.


Snowflake's unique vantage point across industries made it possible to bring together organizations facing similar challenges. This helped create a trusted forum for sharing practical lessons rather than polished success stories.


The playbook for AI-native engineering is still being written. As models continue to improve and engineering practices evolve alongside them, these takeaways will become even more relevant. We will continue sharing practical lessons from engineering leaders building AI-native organizations, including a deeper look at Snowflake's own transformation. If these conversations resonate with the challenges your organization is facing, we invite you to check out[our latest podcast](https://www.youtube.com/watch?v=n1QlE46CS1E) as we continue exploring what it takes to build the next generation of engineering organizations.
