---
schema_version: "1.0.0"
document_id: "99f373b64cb40d4bfb4c89d508bf21ea5fd567826723345883b69bd5a1dda965"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/redefining-api-strategy-why-netflix-platform-engineering-chose-federated-graphql"
published_at: "2024-03-13T12:37:32+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:e264f65a4d0fc5bd351ac654cd67aca9b7613530c3b3e65f6a08737f28ac0cd2"
---

# Redefining API Strategy: Why Netflix Platform Engineering Chose Federated GraphQL

The Netflix API evolution is a saga worth exploring, spanning 15 years and five generations of APIs, culminating in GraphQL adoption to meet the growing demands of its industry-leading streaming platform. Bruce Wang, Director of Product Platform Systems at Netflix, sat down with Matt DeBergalis, CTO at Apollo GraphQL, to discuss Netflix’s extensive API journey and the adoption of[GraphQL](https://www.apollographql.com/docs/resources/faq/#what-is-graphql) in an insightful fireside discussion during the[GraphQL Summit 2023](https://www.youtube.com/watch?v=b78Q7qY4IpI&t=121s) .


## Netflix’s API Evolution: The Unexpected Journey


Their conversation built on Bruce’s earlier webinar, “[Netflix APIs: An Unexpected Journey](https://www.apollographql.com/events/champions-corner/thank-you/netflixs-unexpected-journey-to-the-supergraph/) ” where he shared how their multi-year, multi-generation API evolution culminated in a new “Consumer Edge” powered by federated GraphQL. Along the way the Netflix API team adopted and then outgrew multiple API architectures. From their initial catalog of distributed REST endpoints coined “OpenAPI” to its replacement API platform “API.next” and then to “DNA” a GraphQL-like API. In each case, they were encased in a complex monolith, so their goal was threefold: break the monolithic architecture, unify the APIs across diverse platforms and empower domain owners to manage their APIs independently.


“In all our previous stacks, each individual UI built their own APIs. iOS had one API, Android had another API, TV had a different API, etc. we just couldn’t scale with the business anymore. Unifying those in a single federated GraphQL API was super valuable, and something we couldn’t do before.”


– *Bruce Wang, Director of Product Platform Systems team, Netflix*


As described in their 2020 blog[How Netflix Scales its API with GraphQL Federation](https://netflixtechblog.com/how-netflix-scales-its-api-with-graphql-federation-part-1-ae3557c187e2) , the Netflix API team found[the Apollo Federation specification](https://www.apollographql.com/docs/federation/subgraph-spec/) the perfect way to scale their GraphQL architecture. In the federated model, individual GraphQL schemas become subgraphs, which are composed into a unified supergraph. In this way, they retained the integrated “Consumer Edge” API they sought while decoupling their many domain teams. The result was faster delivery without sacrificing a cohesive customer experience.


The graph’s power to democratize information also helped foster data-centric discussions, empowering every Netflix team member to meaningfully engage using concrete information.


*“There have been numerous instances where people ask, ‘What does this subgraph do?’ And now we can simply go in and see it all, what it does, who calls it. That sounds basic, but we lacked this information with previous APIs; it was like dealing with a black box. Only the API team knew how to manage that black box.”*


– *Bruce Wang, Director of Product Platform Systems team, Netflix*


For a full recounting of Netflix’s API transformation journey, be sure to watch Bruce’s “[Netflix APIs: An Unexpected Journey](https://www.apollographql.com/events/champions-corner/thank-you/netflixs-unexpected-journey-to-the-supergraph/) ” webinar.


## Strategic Lessons from Netflix’s API Evolution


Bruce expanded on Netflix’s API journey at a fireside chat with Matt at GraphQL Summit 2023. In this talk, Bruce highlighted some helpful tips for rolling out a graph across a broader organization, emphasizing the strategic approach of tackling complex challenges first, balancing the needs of API clients and service owners, leaning into developer experience and API governance, and discussing the human element for successful technological innovation. Read on to learn more.


### Lesson 1: Tackle Complex Challenges First


When undertaking major architectural transformations, a conventional approach often suggests starting simple and working up to complex use cases. However, when Matt asked about the key lessons from the API transformative journey, Bruce highlighted the strategy of addressing the most complex customer experiences first, which for Netflix would have been its homepage. This approach helps in de-risking the project and provides immediate value, testing the robustness of the new API strategy under demanding conditions.


In discussing this, Bruce emphasized that technology choices must deliver tangible user benefits, emphasizing the essence of Netflix’s strategic technological shifts anchored in delivering meaningful outcomes.


*“For me, I really believe in providing value. I don’t believe in adopting technology just because it’s cool. It’s cool, but it has to provide value, right?”*


– *Bruce Wang, Director of Product Platform Systems team, Netflix*


### Lesson 2: Balance the Needs of API Clients and Service Owners


While GraphQL is often associated with frontend development due to its client-centric approach, its true value extends far beyond the frontend.


During the conversation, Bruce highlighted that although his team initially centered on the API client’s value proposition, the focus shifted towards federating and democratizing GraphQL development over time. This evolution stemmed from the necessity to break down a complex monolith across major domains such as the growth graph, discovery graph, and identity graph. The API team aimed to distribute ownership, allowing service teams to manage their APIs independently.


An unintended consequence of Netflix’s journey into microservices was the API team inadvertently becoming the central clearing house for an intricate web of models and dependencies, crucial for diverse functionalities. The organic development of graphs within backend and frontend teams marked a significant shift, reducing dependency on the API team. When Matt inquired about the value of this approach, Bruce emphasized that it fostered independent evolution, creating an environment where new graph components emerged organically without the API team’s direct involvement.


*“ \[…\] the thing that we really wanted is for the UI team to talk to the backend team without us being in the middle. \[…\] The design of the graphs are happening organically; the back-end teams and the UI teams are working together. We’re using tooling to make the sharing easier, the testing easier. New graphs are coming up without us even knowing.”*


– *Bruce Wang, Director of Product Platform Systems team, Netflix*


While this heralds a new era of dynamic API development, Bruce cautioned that the challenge remains in balancing autonomy without federating complexity at Netflix. He noted that the API team held significant expertise in managing a large-scale graph, and transferring this responsibility abruptly to service teams could create complications. Bruce emphasized that maintaining the consolidated experience of the API team is crucial to prevent potential issues arising from distributing this responsibility among multiple backend teams.


### Lesson 3: Lean into Developer Experience & API Governance


The discussion also touched on governance aspects in API management. Bruce highlighted a specific instance where the challenge arose while migrating intricate iOS and Android-specific APIs into a more universally applicable graph. This process revealed significant logic embedded in the UI code and Backend for Frontend (BFFs), necessitating strategic decisions on representation and ownership within the graph. The API team intentionally took the lead in the initial translation to simplify these complexities, paving the way for future domain ownership. Bruce elaborates on their collaborative approach with sister teams, such as the Consumer Identity and Access (CIA) team, indicating an ongoing phase where the API team currently manages while actively facilitating a transition of ownership.


The Netflix journey portrays deliberate steps in crafting a schema framework, navigating complexities in transitioning logic to the graph, and fostering a decentralized governance model for effective ownership and evolution of APIs.


### Lesson 4: Bridge Silos by Fostering Inter-Team Conversations


In the realm of Netflix’s API evolution, the conversation steered into the intricacies of the[supergraph](https://www.apollographql.com/docs/federation/#next-steps) and its profound impact on organizational dynamics and developer experiences. Bruce explained that while they employ advanced tooling to facilitate graph management, they also invest significantly in fostering intentional communication patterns among teams.


This emphasis on meaningful dialogue arises from Bruce’s acknowledgment that within organizational frameworks, teams can easily retreat into isolated spheres. Bruce recognized this issue, pointing out that many teams might opt to function within their silos and avoid conversations between teams. To overcome this, the company actively encourages these discussions, leveraging the graph as a focal point. Teams are prompted to deliberate on essential matters like whether a growth API should access a discovery API or develop their own, fostering conversations that might otherwise not take place.


In essence, this dialogue underscores Netflix’s ongoing pursuit to blend technological innovation with human interaction—leveraging tools while acknowledging the importance of human communication in steering effective developer experiences.


*“\[..\] the developer experience to me is a multi-pronged approach \[…\] sometimes you’re using \[…\] tooling but sometimes you’re using just good old fashioned people and trying to nudge the conversation to happen.”*


– *Bruce Wang, Director of Product Platform Systems team, Netflix*


## The Journey Ahead


In the concluding segment of the fireside chat, Matt and Bruce explored the future prospects and challenges for the Netflix API evolution. Bruce reflected on his personal journey, having transitioned from leading the API team to tackling broader, more complex projects at Netflix, such as managing tech debt and orchestrating multiple massive migrations beyond GraphQL. While the transition to GraphQL stands as a pivotal point, Bruce’s attention now gravitates toward rearchitecting the client telemetry framework, underscoring the commitment to refining and enhancing Netflix’s technical landscape. Embracing these lessons from the API journey, Bruce emphasized the importance of applying these insights across the organization, aiming to support other teams while navigating their respective challenges.


While sharing final thoughts and reflections, Matt commended the environment fostered at Netflix, describing it as an ideal crucible for technological ideas to thrive. He highlighted the unique blend of culture, product excellence, and scale that has propelled the company forward. He hoped sharing Netflix’s approach would be valuable for others on a similar path.


Bruce acknowledged that while Netflix has a strong brand, it still faces challenges and constraints like any company. Despite its size, Netflix doesn’t have infinite resources. Bruce wanted to “pierce the aura” around Netflix and convey that they grapple with similar people and design problems that others face when adopting new paradigms.


In closing, Matt noted that many of Netflix’s innovations arose from working within their constraints, such as not being able to rapidly grow their team as the number of services expanded. This forced them to find scalable, efficient solutions. The key takeaway is that while challenging, it is possible to achieve significant efficiency gains through thoughtful cultural, organizational and technical approaches – even with limited resources.


Watch the[GraphQL Summit 2023 panel discussion with Netflix](https://www.youtube.com/watch?v=b78Q7qY4IpI&t=121s) to learn more about the remarkable narrative of growth and adaptability within Netflix’s evolving API landscape.


*Note: Dan Boerner also contributed to this blog post.*


Written by


Ishwari Lokare


[Read more by Ishwari Lokare](https://www.apollographql.com/blog/author/ishwari)
