---
schema_version: "1.0.0"
document_id: "042172688c7a53bc1e7b1c45dad44a0d2a9f49fb22bc42698917cbac2b793e15"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-n-able-certified-1300-growth-with-apollo"
published_at: "2026-01-08T13:17:24+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:58:18.576112+00:00"
content_hash: "sha256:889bf7b1cd2ad87f1e4b3090ceef9120ee27629a1d98537ee0d90617d17d868b"
---

# How N-able Certified 1300% Growth with Apollo

## A recap from GraphQL Summit 2025 on how N-able turned certification, automation, and governance into a system for scale.


When N-able’s platform team forecasted their first year of Apollo GraphQL adoption, the plan was conservative: one subgraph, ten engineers, and a year to get it right. What they didn’t anticipate was the demand – developers loved it. Twelve months later, they were running 14 subgraphs, with 85 contributors and 300 Apollo Studio users. The challenge wasn’t infrastructure, it was staying in control through 1,300 percent growth.


At the center of that transformation is Dhanik Alkegama, Engineering Manager at N-able and leader of the GraphQL platform team. In his[Summit session](https://youtu.be/0n82t41v5ro?si=XxloWvUeHYvCYBAG) he shared how his team survived that growth curve, avoided chaos, and built governance that accelerates instead of blocks.


*“We planned for one subgraph in 12 months. We ended up with fourteen. That wasn’t a scaling problem, it was a survival problem.”* – Dhanik Alkegama, Engineering Manager, N-able


## **From API Sprawl to a Single API with Infinite Possibilities**


N-able builds cybersecurity and IT management software for managed service providers overseeing thousands of devices and endpoints. Years of acquisitions left them with fragmented systems and many REST APIs that behaved almost the same but never quite aligned. Customers were frustrated, and the numbers showed why.


A simple question like “which devices are offline?” could require more than 100 API calls: authentication, customer lookup, device enumeration, and then an API request for each of tens of thousands of devices. Some customers managed fleets of 30,000 devices, turning that workflow into thousands of API calls. Many wrote custom PowerShell scripts to orchestrate retries, handle errors across multiple endpoints, and filter payloads with 2,000 fields just to extract the one they needed.


“Why do I have to make so many API calls just to get one answer?” one customer asked. “Why do I get 2,000 fields when I only need one?”


For partners managing infrastructure at scale, this wasn’t just friction. It was a reason to leave for competitors with cleaner APIs. When customers started asking for more REST endpoints, leadership made a decision that surprised everyone: no more REST. Everything, from product UI to AI integrations, would flow through a single graph.


With Apollo GraphQL, those PowerShell scripts requiring 100+ calls collapsed into a single query returning exactly the needed fields. No loops, no client-side orchestration, no overfetching. The vision was clear: one API with infinite possibilities. The question was how to build it without repeating past mistakes.


## **The Breakthrough: Education as Infrastructure**


Dhanik had seen the challenge before. Six years earlier, he worked on N-able’s first graph, an early effort that left experimental schema hanging around for years and slowed adoption. This time, he wanted a clear plan and a shared understanding from the start.


He went to leadership with an architectural proposal, a business case and an idea that would become the foundation for scale:[Apollo certification](https://www.apollographql.com/tutorials/#certifications) would be mandatory. Every engineer contributing to the supergraph needed Level 2 certification, and every PM or manager needed Level 1.


“You wouldn’t let anyone deploy without a CI/CD pipeline. The same should be true for the graph.”


Leadership agreed. They understood that helping everyone speak the same language from day one would ease and accelerate the development of a federated graph built by many teams. Within a year, 154 people were certified, including 132 engineers at Level 2. Reviews became faster, proposals more consistent, and new contributors productive on day one.


The impact showed up in reviews and proposals. “When you have the same training, same understanding of GraphQL, as reviewers we can trust the proposals,” Alkegama notes. “For the newer contributors, they are productive from day one.”


Education became infrastructure, a built-in layer of trust that scaled with the team. “Instead of hoping people will figure it out along the way, we frontloaded the knowledge,” Alkegama says. “Discipline can be painful, but so is regret.”


## **Governance that Accelerates**


With the team aligned and certified, the next step was refining how they worked. N-able’s engineers were moving fast and giving clear feedback that the standard proposal flow of development, testing, and then production was slowing iteration. Rather than enforce a rigid process, the platform team listened.


They redesigned the workflow around developer feedback. Instead of running proposals through test and staging environments first, contributors proposed schema changes directly against the production variant.A Lambda-based workflow listens for Apollo Studio webhooks, validates each schema change, runs policy checks, and posts results back to Studio as pass, fail, or warning.


“The bottleneck wasn’t the engineers’ knowledge, it was the process,” Alkegama explained. “That realization allowed us to manage fourteen subgraphs with just two or three reviewers.”


The system runs asynchronously, so developers keep working while checks complete. Two or three reviewers can now safely govern fourteen subgraphs.


## **What Growth Looks Like**


In just twelve months, N-able’s numbers tell the story:


- **14** subgraphs deployed (from a plan for one)
- **85** active contributors (from an initial team of 10)
- **300** Apollo Studio users across the organization
- **1.3 million+** operations running through the unified graph


For customers, the change is clear. Workflows that once required hundreds of REST calls now take a single query. The platform delivers what N-able calls a “single API with infinite possibilities,” removing the need for client-side orchestration and manual filtering.


## **Lessons for Every Platform Team**


N-able’s experience offers a simple but powerful blueprint for scaling a graph quickly and safely.


1. **Make education your first dependency.** **** Certification gave every contributor a shared language and framework for building with confidence. Graph literacy is as critical as a CI/CD pipeline.
2. **Automate governance into the workflow.** **** By embedding checks directly into Apollo Studio, governance became self-service and non-blocking, keeping engineers in flow.
3. **Design for both speed and safety.** **** Production-first proposals allowed N-able to move quickly without sacrificing stability, turning process into a catalyst instead of a constraint.
4. **Build for the graph you will have, not the one you have now.** Expect exponential adoption. Systems that can handle ten times your current scale give teams freedom to innovate without fear.


*“Instead of hoping people will figure it out along the way, we frontloaded the knowledge,” Alkegama says. “Discipline can be painful, but so is regret.”*


## **What Comes Next**


N-able is now experimenting with preview contracts and AI-assisted governance to manage experimental features safely. Their next goal is to make the graph the default entry point for all customers, lowering the barrier for teams still using REST APIs.


N-able didn’t just scale their graph. They scaled how their teams work together. Watch their full[GraphQL Summit 2025 session](https://youtu.be/0n82t41v5ro?si=XxloWvUeHYvCYBAG) to see how they certified to grow, then listen to the[Customer Spotlight](https://www.apollographql.com/events/scaling-apis-smarter-faster-how-n-able-uses-apollo-to-optimize-rapid-growth) to hear the story behind it.


Written by


Valeria Gomez


[Read more by Valeria Gomez](https://www.apollographql.com/blog/author/vgomez)
