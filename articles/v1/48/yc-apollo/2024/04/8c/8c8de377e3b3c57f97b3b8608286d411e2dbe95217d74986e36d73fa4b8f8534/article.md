---
schema_version: "1.0.0"
document_id: "8c8de377e3b3c57f97b3b8608286d411e2dbe95217d74986e36d73fa4b8f8534"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-realtor-com-achieved-technological-and-cultural-transformation-with-graphql"
published_at: "2024-04-23T16:14:56+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:8d7f1f94d6df777dfcdfb5b0cfd5029a5fdf168310db255b19e26c90b425345e"
---

# How Realtor.com Achieved Technological and Cultural Transformation with GraphQL

Realtor.com is a trusted real estate listing platform that connects buyers and realtors for a seamless experience when renting, buying, or selling properties. At[GraphQL Summit 2023](https://www.youtube.com/watch?v=w07Z572q3kg) , Suzy Julius and Cabe Waldrop — SVP of Product and Engineering and Principal Engineer at Realtor.com — discussed the company’s GraphQL journey to simplify its tech stack and scale without compromising performance.


## Context and challenges


In 2020, Suzy and her team performed a technology audit and found that many of their decisions were based on short-term gains, incomplete migrations, and siloed platforms. In response, the company adopted a technology strategy called “simplify to scale” to target new businesses. The involved teams narrowed in on four key areas of focus: data platform, identity, CI/CD pipeline, and the API layer.


In order to tackle these challenges, Suzy and her team evaluated many API solutions but ultimately, they decided GraphQL and[Apollo Federation](https://www.apollographql.com/docs/federation/) was the best solution to streamline and unify their APIs. Suzy and Cabe found that only addressing the technological components hindered their progress; they needed to invest just as many resources into storytelling and communication to connect with other teams.


Figure: Realtor.com journey to GraphQL adoption


Read on to learn about Realtor.com’s most prominent requirements to adopt new technologies and solutions throughout the three phases of its journey: the tech stack, the supergraph, and the API paradigm shifts needed to deliver great UI experiences to clients. They discuss each phase through the lens of their four-step process for GraphQL adoption resiliency, which include solving the technical problem, building momentum, overcoming resistance, and communicating at every level. Let’s dive in.


Figure: Four-step process to create adoption resiliency


## The road to GraphQL adoption with Apollo


### Tech stack


The first area that Realtor.com targeted was the API layer. Realtor.com had previously struggled with over-indexed microservices, endpoint and API inconsistencies, and complex data aggregation on the client side. Cabe and his team considered many options — like service mesh, microservices, and shared libraries — but ultimately, they decided GraphQL was the best solution to streamline and unify their APIs.


Suzy and Cabe quickly realized that to implement a unifying API solution, they needed to collaborate with other teams to create understanding and transparency.


Suzy and Cabe created a Strategic Technical Initiative Group (STIG) to approach the transition with multiple individuals and teams, including executive sponsors, tech drivers, working groups, and stakeholders. Each of these groups provided different areas of expertise to help the GraphQL STIG prepare for opposition.


It came as no surprise that the STIG faced resistance from engineering, product, design, leadership, and cross-functional teams. “Don’t assume that everyone understands or cares about your technical challenges,” said Cabe. “People aren’t going to be swayed because you’ve lined the boxes up nicely… but the good news is that you can get through \[to them\] if you persevere.”


Suzy continued, “We really leaned into the idea that your single biggest problem in communication is the illusion that it’s already taken place.”


With that in mind, the two began tailoring their messaging to each individual team, appealing to their specific interests and challenges. This approach helped the STIG better communicate GraphQL advantages like a single endpoint for all clients, standard payload and error handling, and standardized authentication. As those messages were received, teams started to understand how GraphQL would solve issues they faced day-to-day, related to complexity, security, and developer quality of life.


Figure: The GraphQL advantage


### The supergraph


Once the STIG sold Realtor.com teams on a GraphQL migration, they faced another challenge: rebuilding a monolithic schema. The existing centralized ownership model had lengthy review processes, unclear roles, and inadequate tools that were slowing progress. They found themselves in a position where too many actors were editing and handling the code base, contributing to a fractured API sprawl.


The core GraphQL team decided to employ[Apollo Federation](https://www.apollographql.com/docs/federation/) to achieve a federated GraphQL architecture called the[supergraph](https://www.apollographql.com/supergraph/) . Federation maintains the simplicity of the monolith for client teams but offers a modular, decoupled approach for service teams — allowing for individual GraphQL APIs maintained by separate teams on separate servers called[subgraphs](http://r/) . These subgraphs sit behind a single API router and access point and combine into one schema to form a supergraph.


**Under the supergraph, each service team at Realtor.com could maintain and release changes on their own GraphQL APIs and define core entities in one graph. Plus, client developers could consume data from the federated graph at a single endpoint, quickly and painlessly.**


To support this process, Suzy and Cabe recommend finding early adopters and providing them with resources to get the supergraph project off the ground. At Realtor.com, security and growth teams were particularly excited about GraphQL, so the core team built a guild to discuss best practices. When they eventually faced opposition about moving away from a monolith approach, they had already focused on building out the tools and automation that simplify subgraph deployment for better visualization.


Suzy said, “Big migrations are often a combination of focusing on the journey and not just the destination, but also celebrating the wins when you have them. Let’s be honest, it takes a bit of cleverness to get things done right.”


### Paradigm shifts


The major paradigm shift that Realtor.com faced involved finding a way for its APIs to both express the intent of the product and power a high-quality user experience. Realtor.com displays listings for thousands of homes, with specific information about features, timelines, and price drops. Developer teams need to write the same code for every single one of these experiences across Android, iOS, web, and more — and as the number of clients increases, the likelihood that the code stays uniform decreases.


“This is where GraphQL really shines, regardless of whether you’re in tech, product, or design,” Cabe said.


GraphQL allows teams to get features out the door faster and ensure quality across an entire product suite by building experience-driven APIs and moving logic to the backend. Furthermore, with a single backend expression of core business logic, it streamlines how teams understand and process data. These benefits support a focus on the entire product and a stronger sense of customer trust.


“This is more of a cultural shift than a technical problem you’re solving for,” Suzy clarified. “We can’t stress enough just how critical it is that you need to build resiliency into your business because if you do that, and you communicate, you can continue this big paradigm shift when business is up and also when it’s down.”


Suzy and Cabe said this comes down to exposure. Paradigm shifts are about noting what works when showing different teams how GraphQL impacts business processes and results.


## Looking toward the future


By jumping headfirst into GraphQL with Apollo, Realtor.com transitioned from multiple product expressions to a shared product expression, from an architectural point of technical leverage to an organizational point of product leverage, and from many services and data sources to a single platform.


Cabe emphasized, “If you’re an engineer, there’s a level of maturity and growth that comes from investing in the product side and knowing more about what you’re trying to deliver. Think about those APIs and about how they power the experience so you can deliver more in the future.”


Check out Suzy Julius and Cabe Waldrop’s insightful[session at the GraphQL Summit 2023](https://www.youtube.com/watch?v=w07Z572q3kg&list=PLpi1lPB6opQzUOqG3QroLLN06FF-Q_uhX&index=3) to learn more. Ready to start your GraphQL journey with Apollo?[Contact](https://www.apollographql.com/contact-sales?referrer=nav-contact-sales) one of our representatives today!


Written by


Ishwari Lokare


[Read more by Ishwari Lokare](https://www.apollographql.com/blog/author/ishwari)
