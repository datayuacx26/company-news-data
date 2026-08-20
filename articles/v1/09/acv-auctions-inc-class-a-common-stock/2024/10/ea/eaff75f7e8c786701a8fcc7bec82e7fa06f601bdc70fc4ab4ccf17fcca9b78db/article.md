---
schema_version: "1.0.0"
document_id: "eaff75f7e8c786701a8fcc7bec82e7fa06f601bdc70fc4ab4ccf17fcca9b78db"
company_key: "acv-auctions-inc-class-a-common-stock"
company: "ACV Auctions Inc."
source_id: "acv-auctions-inc-class-a-common-stock-rss-d00f1f014d5a"
canonical_url: "https://acv.engineering/posts/pdlc/"
published_at: "2024-10-18T05:00:00+00:00"
first_seen_at: "2026-07-20T23:17:32.344046+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:fe50061ed6c30aa3c4fe44ef4fb92943ea67a7f7e8b6df0912f07e99b6599385"
---

# From Failure to Success: How a Strong Product Development Life Cycle Can Save Your Software Projects

A 2024[education report](https://zipdo.co/software-project-failure-statistics/) produced by Zipdo found that 60% of software projects undertaken in 2023 are considered failures based on meeting scope, time, or cost goals. The report concluded that the top reasons for software project failures are poor or unstable requirements (78%), technical complexity (64%), the use of inadequate project management techniques (49%), and unrealistic project goals (35%). These failures can be contributed to misses in the work intake process and the tools provided to project planners. This leads to a big concern for any technical leader. A software project could be destined for failure before a single line of code is written.


These statistics highlight why every engineering organization needs to have a solid Product Development Life Cycle (PDLC). Without one, engineering organization are lacking organization, the efficient and orderly approach to tasks. These groups may have great vision, excellent people, and innovative technology, but they do not have an execution strategy. This article will look at what a PDLC is, how it addresses the most common causes of project failures, and how the engineer’s at ACV have structured our PDLC to ensure software project success.


## What is a PDLC?


The Product Development Life Cycle (PDLC) is a series of stages that a product goes through from initial concept to market launch and beyond. The goal of the PDLC is to provide a structured approach to developing new products. While the cycle can vary in length and complexity depending on industry or product, it typically includes the following phases:
**Idea Generation** : This is where new product ideas are conceived through research, customer feedback, or market analysis.
**Concept Development** : The new product idea is refined into specific concepts, including defining features, target market, and potential benefits.
**Feasibility Analysis** : The new product idea is assessed for technical, financial, and market feasibility.
**Design and Development** : Prototypes and designs are produced over a series of iterations.
**Testing** : The new product idea is evaluated for market, user, and quality assurance to ensure the product meets requirements and expectations.
**Launch** : The new product idea is introduced into its beachhead market, which includes marketing, sales strategies, and distribution planning.
**Post-Launch Evaluation** : The new product idea is monitored for performance, customer feedback is gathered, and adjustments are made as needed.


## The Advantages of the PDLC


By having a strong PDLC, engineering organizations can not only define what needs to happen and when, but also who is responsible for making sure it happens. This level of detail will be critical for tracking and communicating the status of projects. Keep in mind that 49% of projects fail because of inadequate project management techniques. The PDLC can help address this by driving expectations and accountability down into the organization.


In addition, PDLCs leverage iterations to refine requirements. Market analysis, prototyping, and early and often feedback collection all contribute to stable requirements. Product ideas are also assessed for technical, financial, and market feasibility. This level of rigor introduced by a strong PDLC help organizations avoid wasting time on concepts that don’t have any chance of success due to various factors outside the organization’s control. This gives businesses a fighting chance of avoiding both the 78% of projects that fail due to poor or unstable requirements and the 35% of projects that fail due to unrealistic project goals.


When looking at the advantages of the PDLC it becomes clear that it is really a risk management tool. Good PDLCs should allow the business to identify potential risks early in the process allowing for mitigation strategies and reducing the likelihood of failure. In addition, because it is a clearly outlined process, the PDLC can be iterated on and continuously improved with the learnings of every project that leverages it. Over time, the PDLC can become an incredibly powerful engine driving success.


## ACV’s PDLC


We break our PDLC into nine phases, four in upstream planning and five in downstream development. The four upstream phases are: ideation, validation, discovery / planning, and sizing. The five downstream phases are: ready for development, in development, deploy / release, measure, and done.


*ACV's PDLC Phases*


### Ideation


This is the phase where development ideas enter the funnel of work. For each new idea, there is a corresponding high level write up about what the idea is, what problem the idea is trying to solve, how the idea solves the problem, and a FAQ containing potential questions that could be asked by the intended users of the idea or the stakeholders of the solution. These ideas can come from anyone, but normally a Product Manager will screen the ideas and lead the market research effort. Once the Product Manager has completed the high level write up, the idea can move to the next phase, validation.


### Validation


The high level write up may include assumptions about the idea, the problem, or even the intended users and stakeholders. The engineers at ACV designed the validation phase to address these assumptions to ensure that there is a good market fit and business opportunity. This phase should also include a technical assessment to determine feasibility. Product Managers are expected to coordinate the marketplace analysis while Technical Leads conduct the technical assessment. This phase is critical for helping identify potential issues early and start developing mitigation strategies.


### Discovery / Planning


Once the team has determined the business opportunity and the feasibility of the idea, Product Managers can start writing more detailed requirements to turn the idea into a product. In this phase, designs, wire diagrams, and prototypes can be built. Feedback on these artifacts is gathered and the requirements are further refined. Iterating on the designs in this phase help create high quality, stable, requirements. Once the requirements are solid, a roadmap can be created that outlines key milestones, timelines, and deliverables. Product Managers and Technical Leads work together to identify necessary resources including budget, personnel, and tools. Finally, the Product Manager develops the KPIs, based on the market analysis done in the validation phase. These KPIs will be used to track the success of the new idea once it is live in Production.


### Sizing


With the initial requirements in hand, Product Managers can now work directly with engineering teams to get a sizing on each requirement. During this phase, the requirements can be broken down into technical tasks. Engineering teams are expected to groom their backlogs on a regular cadence. Engineering Managers are responsible to make sure that the team is providing sizing estimates back to the Product Managers in a timely fashion. The level of effort (LOE) produced in this phase should then be compared back to the product roadmap created in the discovery / planning phase to make sure the project goals are realistic.


### Ready for Development


Once the initial requirements have been broken down into technical tasks, and those technical tasks are sized, the work can begin. The Product Managers and Technical Lead should work together to slot the technical tasks into sprints based around the key milestone and deliverables outlined in the product roadmap developed in the discovery / planning phase. The Product Manager and Technical Lead should take the team’s bandwidth or velocity into account when slotting technical tasks into sprints. This is the first time the work for the new idea may need to be balanced with the work the team already has in flight, so some negotiation may be necessary between the Product Manager and the Engineering Manager. Any challenges discovered while slotting the work that would prevent the team from achieving a deadline or milestone should be raised and addressed by the Product Manager and Engineering Manager.


### In Development


Once the sprint starts, all technical tasks in that sprint are now considered in development. During the development phase, the engineering team is expected to give frequent updates regarding progress and problems. For teams utilizing Agile software development methodologies this is accomplished during the team’s standup. This phase should include writing the code, having the code reviewed by a peer, and having the code tested either through automation or manual processes. Once the code meets our standards, it can then be released to Production.


### Deploy / Release


The engineers at ACV uses continuous integration and continuous deployment best practices. This allows the Product and Tech organization to release code hundreds of times a week. Every team has an automated build pipeline that will execute a battery of tests including unit, integration, performance, and end-to-end. Once these tests pass, the deployment to production happens.


If a team wants to deploy the code in an “off” state, meaning the code is on production but there is no path to executing that code, the team will implement a feature flag. The feature flag acts as a bypass. When activated, the newly deployed code is skipped and execution paths to that code are blocked. This allows teams to continuously release code independent of when new features go live.


### Measure


In this phase, the new product should be assessed against the KPIs developed in the discovery / planning phase. In addition, the end users that were identified in the ideation phase and validated in the validation phase should be sampled for feedback. Based on the feedback received, iterative improvements can be made to the new product until it hits the KPI targets. Product Managers are expected to lead the post-launch evaluation efforts while the engineering team implements necessary updates and improvements.


### Done


A product is considered done when it has achieved its KPI targets. During this phase, a postmortem is conducted to establish learnings that can be applied to the next project. Feedback is also gathered on the PDLC itself so that it can also be further refined. In addition, comprehensive documentation is created as the new product shifts into maintenance mode. Product Managers and Engineering Managers are expected to facilitate knowledge sharing sessions.


## Vision and Execution


A well thought out PDLC helps marry the vision of the organization with a solid understanding of execution. It derisks projects through iterative refinement that helps build confidence. It provides opportunities for the project teams to discover and address problems with mitigation strategies. It drives expectations and accountability into the organization and helps establish clear communication lines for project progress and status updates. These benefits help create more successful software projects that deliver greater value to consumers and give companies an advantage over their competitors. Whether you’re launching a new product or refining an existing one, understanding and applying the principles of the PDLC can make all the difference.


-
-
-
-
