---
schema_version: "1.0.0"
document_id: "f3f22e34e194de6e0d9ff67a416c804628ff2fb9985d0a46f41efdc23ac2f5e1"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-b653f977f25a"
canonical_url: "https://building.nubank.com/from-feature-to-platform-how-nubank-reinvented-its-conversation-architecture/"
published_at: "2026-06-24T15:30:00+00:00"
first_seen_at: "2026-07-20T04:36:16.841262+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:4dbd41c2a8223f6a0cb5df3a9d24965f26726ce85cdba034371b635bb47353f8"
---

# From feature to platform: how Nubank reinvented its conversation architecture

*Written by: Nubank Editorial*


For a long time, Nubank’s chat was treated as a product inside the app. But as the company grew, what had once been just a support channel began to take on too many responsibilities: automations, human routing, chatbots, operational flows, multiple products, new countries and, more recently, artificial intelligence.


At some point, the challenge became rethinking the very idea of conversation as infrastructure.


This was the journey presented by Natally Mauerverck, Engineering Manager at Nubank, during the Platform Days held at Nu’s office in São Paulo. In the talk, she shared how the team transformed a highly coupled monolith into a conversational platform designed for autonomy across teams, multiple agents and AI at scale.


The change began when the team realized the problem went beyond architecture. The old system could no longer sustain, on its own, the pace at which products evolved, the expansion into new channels and the growing need for smarter conversational experiences.


The result of this transition was a profound paradigm shift: to stop seeing chat as an isolated feature and start treating it as a *capability platform* able to enable different journeys, products and agents within Nubank’s ecosystem.


## **The chat that grew alongside Nubank**


Chat was born together with Nubank, initially focused on the credit card, and evolved to keep up with the company’s expansion. That is why it became the heart of the support operation.


In practice, this meant that virtually everything went through the same central service: the *chat-client* .


It was responsible not only for sending and receiving messages, but also for automations, auto-replies, integration with chatbots, operational flows, app notifications and managing the state of conversations. All based on a state-machine architecture built years earlier, when the company’s context was completely different.


As new needs arose, new functionalities were added to that same core. The result was a gigantic, critical system that was extremely hard to evolve. That is why it became clear that the problem also had an organizational dimension, beyond the technical one.


Each new functionality depended on the same *codebase* and teams had to do *inner source* within the main service. Seemingly small changes could affect critical human-support flows, and deploys began to cause anxiety among the people involved.


Knowledge also became concentrated: there were specific people who “understood the *chat-client* ”, because the complexity accumulated over the years made it hard to navigate that system without deep historical context.


But, at the same time, Nu kept growing and the monolith remained at the center of everything.


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## **When the problem is no longer “refactoring the system”**


In many scenarios, the natural reaction would be to propose a refactor, but the team’s turning point came when the question changed completely.


Instead of asking “how do we refactor the chat?”, the team began discussing what the role of conversation should be in Nubank’s strategy for the coming years.


That change seems simple in hindsight, but it completely shifted the direction of the project. This new perspective turned the discussion into one about enabling capabilities, and no longer about technical maintenance.


It became clear that the path to allowing multiple channels, supporting specialized agents and creating a foundation ready for AI and more sophisticated conversational workflows was to invest in building a conversational platform.


## **The birth of the Conversation Platform**


The main architectural change consisted of separating conversational infrastructure from business logic.


Before, everything happened inside the same central system and, now, the platform took care only of what is common, shared responsibility:


- session management;
- conversation history;
- conversational runtime;
- message delivery;
- multi-channel support;
- multi-geo support;
- observability;
- contracts between services.


Meanwhile, product teams began building independent agents responsible for the specific business rules and, in practice, conversation came to be treated as infrastructure.


This made it easier to democratize the understanding of this tool within Nu.


## **Agent-oriented architecture**


With the new approach, the platform receives conversation events and routes each interaction to the appropriate agent. To enable that decision, we created the *agent selector* component.


It determines which agent is responsible for each task. Some of the agents, in this context, are deterministic: they have predictable, auditable flows structured around well-defined rules. Human support is one example of this, as are the more rigid and controlled journeys.


Other agents, in turn, use AI, language models and context retrieval to run more flexible interactions. The important point, however, is that all agents follow standardized contracts, regardless of their type.


This allowed product teams to build their own agents without depending directly on the team responsible for the platform. It also made deploys independent and, as a result, coupling decreased drastically. The evolution of the platform stopped blocking the evolution of the products, and vice versa.


## **The real challenge**


Building the architecture was hard, but reorganizing how the team worked was even harder. During the talk, one of the most emphasized points was that internal platforms are also products, but ones that carry a different kind of responsibility.


The team stopped being measured only by feature deliveries and began being measured by:


- adoption;
- stability;
- scalability;
- onboarding experience;
- quality of the contracts;
- observability;
- autonomy of the consuming teams.


This required a profound mindset shift, because the platform needed to enable other teams, not centralize decisions.


At the same time, the consuming teams became responsible for the metrics and journeys of their own products and the platform team stopped owning the business logic.


In practice, this separation of responsibilities required alignment, the redefinition of ownerships and new governance models.


## **Conversational data also needed to change**


In addition, the old architecture made it difficult to build reliable metrics and consistent observability. Part of that came from the very way sessions were managed (in memory), which increased the complexity of audits.


The new platform began treating conversational data as first-class entities: persistent sessions, standardized events and agnostic datasets, all with an auditable history.


But, beyond that, it was necessary to separate platform metrics from product metrics. Before, the chat team also ended up responsible for metrics related to the business journeys. With the new structure, the consuming teams began building their own metrics using the data made available by the platform.


This change was important not only for observability, but to make the new strategy more sustainable.


## **Building the new was easier than shutting down the old**


There is a recurring phrase in modernization projects: “legacy always stays longer than planned”. In the case of the Conversation Platform, this happened too.


The migration needs to happen progressively because, in reality, the new system and the old one coexist. Even with the main agents already validated in production, the platform runtime placed in a gradual rollout, and new teams migrating their flows independently, the old *chat-client* still exists, because not everything worked perfectly from the very first moment.


The time needed to migrate the legacy was underestimated and some contracts evolved too late, generating rework. At some moments, data was treated as a consequence of the architecture, and not as a central part of it.


Challenges like these are common in platforms of this size, but Nu is one of the few companies that share them so openly.


## **Five learnings from the journey**


At the end of the talk, we summarized some of the main learnings from the transformation.


The first was to change the question: modernization only gained traction when it stopped being presented as a “refactor” and started being connected to the product strategy and the future of the company.


The second learning was to start with the most valuable domain. In Nubank’s case, conversation is critical infrastructure.


The third was to treat contracts as a product. After all, well-defined interfaces reduce coupling and make autonomy easier.


The fourth was to understand that data cannot be treated as an *afterthought* , because AI, metrics and observability depend directly on it.


And the fifth is perhaps the most important: platforms require governance, and architecture alone does not solve organizational problems.


## **The future of conversational platforms**


The transformation of Nubank’s chat into a conversational platform shows how scaling challenges are rarely just infrastructure problems. In complex environments, architecture, governance, autonomy across teams and data need to evolve together.


By separating the conversational runtime from the business logic, Nubank built a foundation better prepared for multiple channels, specialized agents and experiences powered by artificial intelligence. More than modernizing a legacy system, the initiative paved the way for different teams to build conversational journeys with more independence.


The original monolith continues to coexist with the new architecture while teams move forward with the gradual migration of flows and agents, but the direction is clear: to treat conversations not only as a support interface, but as the infrastructure needed to build digital products at scale.
