---
schema_version: "1.0.0"
document_id: "1382b5a63e5ce58844a5f1ae898009dcb7c8085fdd7effb776a9b780d088fb43"
company_key: "yc-rescale"
company: "Rescale"
source_id: "yc-rescale-rss-a01d3810de6f"
canonical_url: "https://rescale.com/blog/nafems-americas-2026-recap/"
published_at: "2026-06-04T15:42:28+00:00"
first_seen_at: "2026-07-20T23:20:48.219568+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:21f591e0ea2ad0c0cc4577f6f5719bdacfd443a7bf5d752f1e457905f26346e6"
---

# NAFEMS Americas 2026: How Engineering Teams are Integrating Agents and AI Physics into CAE Workflows

[Back to Blog](https://rescale.com/blog)


[Events](https://rescale.com/blog/category/events/) ,


[Industry Trends](https://rescale.com/blog/category/industry-trends/) ,


[Technology Innovation](https://rescale.com/blog/category/technology-innovation/)


## NAFEMS Americas 2026: How Engineering Teams are Integrating Agents and AI Physics into CAE Workflows


Garrett VanLee


June 4, 2026


The[2026 NAFEMS Americas](https://www.nafems.org/events/nafems/2026/nafems-americas-conference/) conference made it clear that the conversation on AI for digital engineering has shifted into high gear. A year ago, sessions on AI in simulation were largely exploratory. This year, a striking share of the program was devoted to AI deployment, what’s working, what’s not, and what comes next. AI applied to various aspects of engineers’ day-to-day workflows has become the dominant theme across NAFEMS tracks globally, combined with nearly every industry and physics discipline on the agenda.


NAFEMS gathered hundreds of engineering and technology professionals from industry-leading companies to share best practices for product development.


## **Agentic Engineering: Real Impact, Human Collaboration Required**


Contents


- 1 Agentic Engineering: Real Impact, Human Collaboration Required
- 2 AI Physics: High Interest, High Validation Bar
- 3 What Practitioners Are Asking and Testing
- 4 What Rescale Brought to the Conversation
- 5 The Signs and Signals Going Forward


The session on agent-driven simulation workflows was one of the most heavily attended of the conference. The core questions driving the conversation weren’t about whether agents belong in engineering, they were about how to deploy them responsibly. Practitioners want narrow, workflow-native use cases: autonomous job orchestration, error detection, post-processing, and structured reporting. Broad “agentic transformation” framing generates skepticism. Specific use cases generate interest.


A consistent theme across Q&A: engineers want to stay in control. Human-in-the-loop checkpoints are a design requirement. The most credible demos showed agents that surface decisions to engineers rather than make them silently. That framing resonated. Autonomous agents that can identify when they’re wrong (and pause for human intervention) were viewed as meaningfully more trustworthy than those that simply execute.


Survey data from attendees who visited the Rescale booth reinforces a key theme of the event: most (55%) said they’re still *manually* passing results between simulation steps — for example, downloading outputs and re-uploading to the next tool or handing off to the next team. Agentic workflow automation is already beginning to address this, but the practitioners made it clear that trust must be earned incrementally in this early phase of adoption.


## **AI Physics: High Interest, High Validation Bar**


Physics-informed AI and surrogate modeling generated significant discussion across multiple sessions and hallway conversations.Engineers distinguished clearly between surrogate models, reduced-order models, and physics-informed neural networks, and they hold each to different proof standards.


Data availability was the most consistently cited barrier to implementation. Training a surrogate model requires structured simulation data at scale, and for many teams, that data either doesn’t exist in usable form or is scattered across disconnected systems.


One practitioner noted that some organizations delete simulation data on a rolling basis simply to manage storage, keeping only the decision data. The implication: agentic workflows that activate dormant or previously unusable data are a critical prerequisite for AI Physics at scale.


Rescale hosted an interactive workshop covering Agentic Workflows, AI Physics, and Compute Optimization for Modern R&D.


## **What Practitioners Are Asking and Testing**


The Q&A sessions across the conference were as informative as the presentations themselves. A few questions came up repeatedly, signaling where the real friction is:


*How do we protect our IP and simulation data in a multi-tenant cloud environment?* Data governance was the most consistent trust barrier not just for AI tools. Teams want clear answers on isolation, access controls, and ownership.


*Does AI physics require deep data science experience to deploy?* The answer organizations want is no because their goal is to make AI methods available to more roles within the organization and product value chain. The deployments that generated the most interest were those built for simulation engineers, not ML engineers — tools that fit existing workflows rather than requiring new skill sets to operate.


*How good is AI-generated reporting, really?* Practitioners are eager to shift repetitive, error-prone reporting to agents but they’re looking for confirmation that agent-generated reports can be relied on to capture key information they care about and summarize findings accurately. A multi-disciplinary agent analysis report weighing design-performance tradeoffs is seen as a strong starting point, one that saves time but still requires expert review before it reaches a decision. That framing is correct, and NAFEMS engineers saw this is exactly where real agentic value is showing up.


The same pattern showed up in booth conversations: when asked what they’d most want an AI agent to handle autonomously, attendees split evenly between two priorities: A) detecting errors and applying fixes, and B) exploring design variations to recommend the best candidates. Both tie directly to where manual effort and time spent is highest today.


## **What Rescale Brought to the Conversation**


Rescale presented two sessions at the conference focusing on agentic workflows and AI Physics infrastructure, and the audience participation reinforced what we’ve been building toward. Engineers engaged most when the conversation moved from platform capabilities to specific workflow outcomes: what does an agent actually do with my solver? How does AI Physics fit into my existing pipeline? What changes, and what stays the same within my current workflows?


At GTC earlier this year,[Rescale and McLaren demonstrated](https://rescale.com/blog/rescale-gtc-2026-agentic-era-begins-nvidia-mclaren-leading-way/) what this kind of AI-first engineering looks like: agents running multi-step aerodynamic simulations, handling job orchestration, and surfacing results in real time. The McLaren collaboration has been a strong proof point for what agentic engineering looks like when it’s deployed by teams deploying AI-driven engineering within their simulation workflows, the kind of deployment Rescale makes accessible for teams starting to explore agentic AI to those looking for advanced capabilities.


Rescale’s[Spring 2026 Platform Launch](https://rescale.com/lp/spring-26-release/) built on that momentum, introducing AI Physics OS as an end-to-end framework for developing, deploying, and governing surrogate models at scale, directly addressing the data, workflow, and compute challenges that practitioners raised throughout the conference.


## **The Signs and Signals Going Forward**


Three patterns emerged from NAFEMS Americas 2026 that will shape how engineering AI develops over the next year.


**Infrastructure determines outcomes.** Engineering teams with cloud-native stacks, accessible simulation data *with context* , and defined governance are deploying agentic workflows at a meaningfully faster rate than those without. The gap between mature adopters and early experimenters is widening, and it’s being driven by foundational choices, not just tool selection.


**Data is the real constraint for AI Physics.** The “data readiness” objection is real for surrogate modeling specifically, but practitioners with mature programs are learning to work around it. Agents that unlock dormant historical data, governing equations that compensate for sparse training sets, and better data capture at the point of simulation are all active areas of exploration.


**Trust is earned at the workflow level, not the platform level.** Engineers aren’t evaluating AI abstractly. They’re evaluating it for a specific job: troubleshooting failed jobs, generating a post-processing report, or recommending the next design variant. Vendors that meet engineers at that level of specificity with credible demos, governance, and honesty about what AI can and can’t do (today), will win.


Rescale’s Head of Product led a session on building an AI-First Architecture for CAE to help engineers take advantage of new agentic AI capabilities within existing workflows.


The simulation industry is moving fast. NAFEMS Americas 2026 confirmed that AI in engineering has shifted from pilot to production — and that the teams pulling ahead are the ones who got their infrastructure, data governance, and workflow foundations right first. Rescale is already deploying this. If you’re ready to move from experimentation to production, Rescale is *the* platform to get you there –[get in touch with our team](https://rescale.com/contact-us/) to get started.


**Learn more about Rescale’s platform for agentic engineering and AI physics at*[Rescale.com](https://rescale.com/) *.**
