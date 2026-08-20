---
schema_version: "1.0.0"
document_id: "fbf79740f60f9333462b563449923928163cfc430846816bd4293adf1bc1272c"
company_key: "criteo-s-a-american-depositary-shares"
company: "Criteo S.A."
source_id: "criteo-s-a-american-depositary-shares-rss-02db2411825d"
canonical_url: "https://medium.com/criteo-engineering/what-the-epm-role-looks-like-in-practice-at-criteo-0944ea18cf0f"
published_at: "2026-05-26T06:31:00+00:00"
first_seen_at: "2026-07-20T23:17:33.645392+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:81c7d3b714a74c4a0d01fda10ad019d84b9ad37d01836600ba519c54be04e7fb"
---

# What the EPM Role Looks Like in Practice at Criteo

Software Development


Software Engineering


Way Of Working


Engineering


Program Manager


# What the EPM Role Looks Like in Practice at Criteo


[Criteo Tech](https://medium.com/@criteotech?source=post_page---byline--0944ea18cf0f---------------------------------------)


9 min read


·


May 26, 2026


--


Authors:[Auriane Bonmarchand](https://www.linkedin.com/in/auriane-barbalat-754a1534/) ,[Carolina Tealdo](https://www.linkedin.com/in/carolina-tealdo/?locale=en) &[Minh-Quyên NGUYEN](https://www.linkedin.com/in/mnguyen10/)


In our previous[post](https://medium.com/criteo-engineering/at-the-heart-of-delivery-what-engineering-program-managers-do-at-criteo-d7fc19896acc) , we explained the Engineering Program Manager role at Criteo at a high level: EPMs sit at the intersection of business ambition, technical reality, and execution, helping complex cross-team initiatives move from intent to delivery. But if you are considering this role, that high-level description is only the beginning. The more interesting question is: what does the role actually feel like in practice?


For us, the answer comes down to one simple idea: **an EPM drives projects to completion in complex environments** .


That means aligning people who do not naturally work together, maintaining momentum over time, and ensuring the work converges toward a coherent outcome. A big part of the role is spotting inconsistencies early, whether they are in priorities, understanding, ownership, or execution, and addressing them before they slow everything down. And underneath all of that, there is one principle that matters especially: **transparency** . Because consistency across teams only happens when people have access to the same information, the same context, and the same reality.


Press enter or click to view image in full size


To illustrate that, we will use two very different examples:


- **Audience Agent** , a product-facing initiative with high ambiguity, fast iteration, and many cross-functional dependencies
- **Dallas datacenter migration** , a large infrastructure transformation with strict operational constraints and a strong need for sequencing, risk visibility, and production safety


On the surface, they have very little in common. One is a product-facing initiative in an emerging space, with fast learning cycles and evolving technical approaches. The other is a large infrastructure transformation, with strict operational constraints and a strong need for sequencing, readiness, and production safety. Through these two examples, we’ll look at how projects with very different dynamics and constraints still rely on the EPM role in similar ways: creating clarity, enabling collaboration, and helping teams move forward together.


## Audience Agent: bringing structure to an emerging space


The **Audience Agent** project explored a new way of interacting with customer audience insights and segmentation data, using emerging agent-like systems to make complex workflows more intuitive and automated. The goal was to empower clients with an AI-driven experience that makes audience building smarter, faster, and more streamlined — enabling marketers to explore insights, create segments, and manage audiences through more natural and efficient interactions. More details are available in this article[Introducing Agentic Audiences](https://www.criteo.com/blog/introducing-agentic-audiences/) , and in the[product demo showcasing how the agent works in practice](https://www.youtube.com/watch?v=tt0RJipZZXs) .


From the start, there were strong product signals. We had clear use cases and a good understanding of user needs. But on the execution side, much more was still open. Some of the features had never been built at Criteo before, there were no established internal guidelines for this kind of agentic experience, and the technical approach was still evolving.


So the project started with a familiar EPM challenge: **clear intent, but high uncertainty** .


Our ambition was not to wait until everything was perfectly defined. It was to deliver fast, learn fast, iterate, and keep moving.


Press enter or click to view image in full size


The Audience Agent is available for all Criteo clients in[CGrowth platform](https://marketing.criteo.com/audience-manager)


### Bringing structure without losing flexibility


One of the most important things an EPM does is turn fuzzy ideas into a delivery shape that teams can actually work with. On this project, which started with a kickoff bringing together all the key stakeholders. This was not just about reviewing requirements. It was about creating shared clarity on how we would work together, what we were trying to achieve first, and where we needed to stay flexible. From there, we structured the project into three milestones. The first two had clear goals and deliverables. The third was intentionally left more open. That was a deliberate choice: we wanted to reach an MVP within the quarter, while also keeping room to adapt based on feedback and learning.


That balance matters a lot in EPM work.


> ***Structure is necessary, but over-structuring too early can slow a project down instead of helping it*** *.*


### Making a cross-functional team work as one team


Another key challenge was that success depended on close collaboration across domains that do not always work tightly together. The project brought together contributors from backend, frontend, testing, UX, AI research, AI platform, agentic core, and product. Each group had its own expertise, constraints, and perspective. None of them alone could make the project successful.


This is exactly where the EPM role becomes very concrete.


The EPM job was not to replace the expertise of those teams. It was to create the conditions for that expertise to come together effectively: making sure the right people were involved at the right time, that dependencies were visible, that the work was framed in a way people could act on, and that decisions did not get lost in the gaps between teams.


### What the role looked like in practice


On a project like this, the day-to-day work of an EPM is rarely one thing. It is a mix of structuring, facilitating, connecting, challenging, and adjusting.


That meant:


- framing the work and the milestones
- collecting needs across teams
- identifying the right contributors
- aligning stakeholders around a shared delivery plan
- putting in place the right artifacts, rituals, and tracking
- keeping the broader stakeholder group informed
- surfacing cross-team impacts early
- spotting inconsistencies across workstreams
- challenging priorities when needed
- unblocking collaboration issues
- protecting momentum over time


For the Audience Agent project, we set up a dedicated cross-functional squad composed of people from different teams. While they worked almost exclusively on this scope, they still belonged to and participated in the dynamics of their respective teams. We also used a cadence that matched the project’s needs: squad stand-ups twice a week, deep dives on specific topics, and a weekly sync with a broader group of stakeholders.


That cadence was chosen to match the project’s level of dependency, urgency, and ambiguity, not to follow a supposedly “correct” process..


This is another important part of the EPM role at Criteo: **we create structure, but we do not force a one-size-fits-all model** . In the broader EPM role, the goal is to support ways of working that serve the teams and the program, not the other way around.


### Launch is not the finish line


We reached the initial goal of delivering an MVP within the quarter. But on this kind of project, launch is not the end. It is the start of the next cycle.


From there, the work continued through iteration: incorporating user feedback, refining priorities, and building on what the first milestones had taught us.


That is part of what makes projects like this so interesting for an EPM. You are operating in an environment with high ambiguity, strong dependencies, and fast learning loops. The role is not just to keep a plan on track. It is to help the project keep making coherent progress as reality evolves.


Press enter or click to view image in full size


### Adapting the level of involvement


One aspect of the role that is often underestimated is how much you need to adapt your level of involvement.


Sometimes you are deeply embedded with the team, helping structure discussions and move decisions forward. Sometimes you step back and simply monitor progress. Sometimes you only intervene when something starts to drift.


The goal is not to be everywhere but to step in where and when it matters most.


That is also why the role is hard to reduce to a checklist. EPM work is contextual. It depends on the maturity of the teams, the shape of the project, the level of ambiguity, and where the biggest delivery risk sits at a given moment.


## Dallas datacenter migration: making a complex infrastructure transformation deliver safely


If Audience Agent shows the EPM role in a fast-moving product and innovation space, the **Dallas datacenter migration** shows the same role in a completely different context: infrastructure transformation at scale.


Criteo operates and manages its own on-premises infrastructure across multiple datacenters in Europe, Asia, and America. In that context, this project is about moving out of the current Dallas datacenter and opening a new Dallas setup aligned with newer Criteo standards for architecture, resiliency, power efficiency, network access, and new server generations, while keeping production stable during the transition. This is not a simple move. It is a real infrastructure evolution: opening the new room, opening the new network hub, interconnecting the environments, migrating workloads, and eventually vacating the legacy site.


Press enter or click to view image in full size


### Why is such a delivery challenge


What makes this project particularly interesting is that complexity is everywhere, but not always in the same form as on a product-focused project.


Here, complexity comes from architecture choices, migration patterns, operational safety, procurement and readiness dependencies, and the sheer number of teams that need to move in the right order.


The migration approach itself is designed to reduce disruption. Instead of doing a large, customer-visible traffic cutover, the setup keeps the carrier hotel as the stable connection point, and allows work to be shifted server by server gradually into the new environment, making it transparent for clients (both internal and external).


At the same time, the project introduces updated network standards and new server generations to improve resilience, reduce power and space usage, and prepare for future infrastructure needs.


And all of this needs to happen without disturbing production in the current datacenter, and it must be completed and stabilized well before the end-of-year peak season.


### What the EPM adds in practice


On this kind of initiative, the EPM does not replace technical ownership. The technical decisions stay in the experts’ hands. The role of the EPM is to create the conditions for successful delivery: ensuring the project progresses against the agreed planning, making dependencies visible as early as possible, identifying and communicating that risks along with mitigation plans, and keeping teams aligned throughout a long and complex execution path.


A lot of that work is almost invisible when things go well.


But behind that apparent continuity, there is a lot of coordination:


- defining what “done” means at each stage
- aligning multiple technical roadmaps
- sequencing readiness and migration activities
- making small but critical dependencies visible early
- clarifying which workloads can move transparently and which require more preparation
- maintaining visibility on the critical path
- communicating risks and mitigation actions clearly
- keeping stakeholders aligned without centralizing every technical conversation


This project also shows something important about EPM work: **shared clarity is already a delivery enabler** .


When many teams, dependencies, and risks have to move together, progress does not happen automatically. It needs structure, visibility, and continuous alignment.


## Two very different projects, one same role


At first glance, these two projects do not look similar at all. One is about experimenting with a new user-facing agent experience in a fast-moving environment. The other is about evolving physical infrastructure and migration architecture without disturbing production. One needs flexibility to learn and iterate quickly. The other needs sequencing, readiness management, and careful operational control. And yet, despite these differences, the core expectations of the EPM role remain remarkably similar.


In both projects, the value of the EPM is to make delivery possible across complexity.


That means:


- creating structure without removing flexibility
- making dependencies visible before they become blockers
- maintaining transparency across teams
- adapting the level of involvement to what the project needs
- creating momentum and making experts makes the best technical decisions


This is also fully aligned with how we describe the EPM role at Criteo more broadly: EPMs help turn fuzzy ideas into scoped programs, surface dependencies and risks early, communicate progress transparently, and lead through influence rather than through formal authority.


> **EPMs do not replace technical ownership. They create the conditions for expert teams to deliver together** .


## What this means if you are considering the role


For us, this is one of the most rewarding things about being an EPM at Criteo. You are not there to be in the spotlight. You are there to help complex things actually happen.


You work close enough to the technical side to understand the real constraints, and close enough to the broader program view to keep the whole system moving coherently. That position is demanding because ambiguity, dependencies, and trade-offs are constant. But it is also what makes the role so engaging.


If you enjoy turning messy situations into actionable plans, helping teams align without flattening their expertise, and driving progress in environments where many things are moving at once, then this role can be a very good fit.


Because in the end, whether the project is innovative, operational, product-facing, or deeply infrastructural, the heart of the job stays the same: **turning complexity into alignment, and alignment into execution.**


If this article inspires you and sounds like the kind of challenge you’d enjoy, keep an eye on EPM opportunities on our[careers page](https://careers.criteo.com/en/jobs/?keyword=&team=Engineering&pagesize=10#results) 👇


[Your next adventure awaits! Explore new opportunities at Criteo. Browse our list of open jobs and see if you can find one matching your skillset… careers.criteo.com](https://careers.criteo.com/en/jobs/?keyword=&team=Engineering&pagesize=10&source=post_page-----0944ea18cf0f---------------------------------------#results)
