---
schema_version: "1.0.0"
document_id: "5ee8af817d662b69ab2b2cf0d7c4843552c4e46035e37f15c84d31c1ddeb44bd"
company_key: "yc-icepanel"
company: "IcePanel"
source_id: "yc-icepanel-news-import-9cf2a09ec197"
canonical_url: "https://icepanel.io/blog/2026-04-27-impact-of-ai-on-migration-projects"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-21T23:29:04.459524+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:2a843573a71b096f7f43a5a7302a9374938965f2cd155674748a7b3a077213b0"
---

# Impact of AI on migration projects

Software architects play a key role in maintaining architectures through migration projects. Reasons might be driven by security requirements (e.g., integrating with a new 3rd party system or deprecating legacy software) or complying with new product requirements (e.g., scaling to meet customer demand or supporting new features from developer teams).


AI has a visible impact on software development, from coding to testing to documenting features. Now with the rise of AI agents, there are opportunities to integrate AI workflows for architecture-focused projects.


In this post we’ll talk about the impact of using AI tools like[Claude Code](https://claude.com/product/claude-code) or[Codex](https://openai.com/codex/) for architecture projects. We’ll cover what a migration project looks like, the pros and cons of using AI, and tips for managing such projects.


---


# Case study: Database migration project


I once had a 6-month project to migrate our primary database ([Aurora PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.AuroraPostgreSQL.html) ) with multiple microservices connected directly. There was a product discussion to scale our system, and as a result of horizontally scaling the microservices, we started hitting connection limits to Aurora. The services were exhausting the database’s max connections during peak traffic and causing intermittent failures and slow queries.


Wearing the architect’s hat, we started to scope the problem. The project seemed straightforward on paper, but there were several dependencies that required a careful rollout strategy. For example:


1. How many services were connected to our primary database?
2. What were the read/write patterns? Some services were read-heavy and could be routed to read replicas. Others needed write access to the primary.
3. What other migrations or projects are in flight? We had to coordinate timelines with two other teams shipping features that depended on the same database.
4. What would the post-migration deployment process look like? Every team deploying a new service needs to know the new connection path.


Our solution was to introduce[RDS Proxy](https://aws.amazon.com/rds/proxy/) as an intermediary layer between the database and services. This allows services to point to the proxy endpoint instead of the database directly. The proxy handles connection pooling, failover, and routing to read replicas.
To manage a safe migration, we had to roll this out into multiple stages:


- Stage A: Deploy RDS proxy
- Stage B: Migrate services one by one
- Stage C: Scale out the architecture by adding more read replicas


The proxy deployment itself was straightforward. What took time was the coordination: testing each service’s behaviour through the proxy under load, validating failover scenarios, getting sign-off through the organisation’s process, and sequencing the migration across teams without disrupting production traffic.


## AI impact


What worked well was collecting data points with agentic tools like[Claude Code](https://claude.com/product/claude-code) or[Cascade](https://windsurf.com/cascade) and building a prototype to test the RDS proxy connection. For example, we used AI to audit the 12 services connected to the primary database and map their query patterns. We used AI to prototype most of the Terraform configuration for the proxy. It also wrote the migration playbook for other teams. Each service migration required updating infrastructure environment variables, running regression tests, and redeploying.


# How can AI help


There are three main things AI can massively help:


1. Automating repeatable tasks
2. Prototyping and researching solutions
3. Challenging your proposals


## 1. Automate repeatable tasks


Architects or engineers will be asked to follow some process when driving this migration project. Depending on your organisation, you might need to write story-pointed tasks, share a technical spec for review, or give regular updates. Some of these tactics are repeatable steps that architects have to. However, those can be time expensive for an engineering resource that could have been used for , especially at scale


AI excels at these repeatable artifacts. You can prompt it to generate Jira tickets from a migration checklist, draft status updates from commit logs, or convert meeting notes into action items. The key is investing in building templates that align with your organisation’s format so it can produce consistent outputs with minimal editing.


## 2. Prototyping and researching ideas


Architects need to prototype solutions and evaluate alternatives before committing. AI can help accelerate prototype development and generate comparisons with the main architect as the decision maker.
For example, in our RDS proxy migration, we used AI to compare connection pooling alternatives (RDS Proxy vs application-level pooling) and draft Terraform modules for services. This let us evaluate trade-offs in order of hours without investing too much development time.


## 3. Challenging your proposals


Before I publish a migration plan or share a technical spec with stakeholders, I like to prompt AI to critique my ideas. I’ve found AI surprisingly good at surfacing edge cases and suggesting changes. If I’m brainstorming a new solution, I often try to challenge my thinking and see if there’s a better way to solve a problem. With AI, I’m able to broaden the solution space and identify the optimal solution to adopt.


---


# Where AI falls short


## 1. Verbosity (AI-slop)


AI is good at generating content, not context. Whenever you’re prompting an LLM to explain or document, it will often lean towards generating more characters to make the overall output “look” good, but with marginal value.


## 2. Over-simplification


AI is good at solving problems based on the parameters it is aware of. As you work on a migration project, it is a good practice not to accept AI-generated solutions at face value, because there is a good chance it is missing context and oversimplifying the problem you’re trying to solve. For example, if you have two repositories: (1) Application repo and (2) Infrastructure repo, and you gave access to (2) to help come up with a migration plan. It is almost certain that without scanning other sources like (1), it will misjudge the complexity of the problem and give answers without seeing the full picture.


My advice is to try to give access to multiple tools/MCPs, but be critical of the output and identify missing context.


## 3. Delegating critical decisions to AI


Jeff Bezos once introduced the idea of “Decision Types” that later became a core part of Amazon’s culture: Type 1 and Type 2 decisions. He wrote the following in a[shareholder letter](https://www.sec.gov/Archives/edgar/data/1018724/000119312516530910/d168744dex991.htm) :
“Some decisions are consequential and irreversible or nearly irreversible – one-way doors – and these decisions must be made methodically, carefully, slowly, with great deliberation and consultation. If you walk through and don’t like what you see on the other side, you can’t get back to where you were before. We can call these Type 1 decisions. But most decisions aren’t like that – they are changeable, reversible – they’re two-way doors. If you’ve made a suboptimal Type 2 decision, you don’t have to live with the consequences for that long. You can reopen the door and go back through. Type 2 decisions can and should be made quickly by high judgment individuals or small groups.”


Using AI for Type 2 decisions is a great use case. It frees up decision-making space from engineers and allows them to focus on the right problems. However, using AI for Type 1 decisions can be very expensive and risky. Primarily, because you are accountable for the end product of your decisions, and delegating it to AI can damage your reputation and affect the overall project.


Good advice is to explore AI tools and automate most (if not all) Type 2 decisions, and allocate deep thinking for Type 1 decisions.


---


# Conclusion


Architectural thinking has and continues to be a core skill required by businesses from their architects. AI cannot replace that skill, however it can add great leverage for software architects like automating repeatable tasks, prototyping, and research. On the flip side, AI falls short on things like context-heavy decisions and accountability.


If you’re working on a migration project, try to use AI to accelerate the mechanics, but own the strategy. Software architects and engineers are needed for human judgment on many activities like project scoping, coordination, stakeholder alignment, and planning architectural changes.


# 📚 Resources


- [https://openai.com/codex/](https://openai.com/codex/)
- [https://claude.com/product/claude-code](https://claude.com/product/claude-code)
- [https://www.ibm.com/think/topics/agentic-engineering](https://www.ibm.com/think/topics/agentic-engineering)
- [https://addyosmani.com/blog/agentic-engineering/](https://addyosmani.com/blog/agentic-engineering/)
- [https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/rds-proxy.html](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/rds-proxy.html)
- [https://aws.amazon.com/rds/proxy/](https://aws.amazon.com/rds/proxy/)
