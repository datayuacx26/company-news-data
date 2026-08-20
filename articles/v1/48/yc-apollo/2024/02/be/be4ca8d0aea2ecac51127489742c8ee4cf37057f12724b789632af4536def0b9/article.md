---
schema_version: "1.0.0"
document_id: "be4ca8d0aea2ecac51127489742c8ee4cf37057f12724b789632af4536def0b9"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/mlbs-api-strategy-hitting-a-home-run-transformation-with-graphql-midfield"
published_at: "2024-02-12T11:09:01+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:4b0d22451310e1746bed4bf9b7a4fb5f287e7e65e1034f275ca3c3156c0e106b"
---

# MLB’s API Strategy: Hitting a Home Run Transformation with GraphQL Midfield

*Highlights of MLB Discussion at API World 2023 conference*


In the realm of[Major League Baseball (MLB)](https://www.mlb.com/) , the quest for excellence extends beyond the playing field. MLB’s unwavering dedication to a personalized, real-time, media-rich experience for fans deeply connects them to their beloved teams and players. Since the early days of mobile technology, MLB has been a trailblazer with their award winning apps, constantly modernizing its platform to enhance the fan experience. Rob Engel, Vice President of Software Engineering at MLB, unveils the journey of MLB’s technology transformation in a candid discussion with Dan Boerner, Field CTO, Apollo GraphQL, at the[API World 2023 conference](https://www.youtube.com/watch?v=5DVi99KPALo&t=1184s) .


## MLB’s API Challenges


Over a decade ago, MLB faced significant technological challenges with their siloed, batch-oriented XML delivery system for publishing game data to fans and the fan experience suffered.


To address these challenges, Rob and his team recognized the need to modernize their stack. They aimed to build a dynamic API capable of fetching data dynamically from various sources based on incoming requests, starting with the game schedule and expanding to include ticketing prices, video highlights, player insights, and personalized aspects for users’ favorite teams and players. The resulting REST-based Stats API was a huge success, serving over 3 billion daily requests. Yet, its very success led its clients to request additional data, related to game statistics but sourced by completely different teams and underlying services. The Stats API had become an orchestrating monolith, each new feature adding complexity and payload size to what had started as a streamlined single purpose service.


Rob and his team recognized they had reached a new bottleneck and began to search for a more scalable and more performant API architecture. GraphQL stood out as a remedy to the problems of REST over fetching by offering a way for API clients to precisely access only the necessary data, significantly reducing bandwidth usage and accelerating response times. This approach not only enhanced client performance but also contributed to cost savings, avoiding downloading large payloads during frequent scoreboard refreshes.


Beyond runtime efficiency, they also sought a solution to the challenges of ever growing hand stitched service orchestration. How could they get out of the business of being the all things for all clients monolithic orchestration service?


## GraphQL and REST: Better Together


The answer was nearby — the content and media side of MLB had been using GraphQL for years and had moved to a federated GraphQL architecture, where teams contribute their domain expertise to a growing superset, a supergraph of API capabilities. But would they face a rewrite of their battle tested, performance tuned Stats API into GraphQL? Not at all, instead, they exposed all the features of their StatsAPI via a new[GraphQL](https://www.apollographql.com/docs/resources/faq/#what-is-graphql) subgraph for their growing federated supergraph.


Once the GraphQL resolvers were written, and clients changed their REST API calls to GraphQL queries requesting exactly the data they needed, the[Apollo Router telemetry](https://www.apollographql.com/docs/router/configuration/telemetry/overview) began streaming in. Instantly, the Stats team could see which clients needed which data, providing detailed insights into attribute usage. This level of visibility was previously unavailable with REST APIs, and powered better decision-making regarding API optimization, caching and maintenance.


### Hitting a Home Run with Midfield Graph


The GraphQL-based “midfield” tier, positioned between the consumer-facing applications (infield) and the microservices (outfield), revolutionized MLB’s data orchestration. This approach streamlined the integration process for various client platforms, facilitated faster onboarding and reduced redundancies in logic implementation.


*Figure 1: MLB’s Midfield Graph*


The midfield graph layer enabled uniform representation of game-related queries across multiple consumer-facing products, eliminating the need for individual client teams to implement custom logic.


*“So where \[GraphQL\] midfield is nice is it moves some of that logic up one level so that the stats team can focus on their domain, which is stats. Ticketing can focus on their domain, which is ticketing, so on and so forth. \[…\] So now these teams can really hyper focus on their domain.”*


Rob Engel, VP. of Software Engineering, MLB


Moreover, it addressed intricate scenarios such as “to be determined” game schedules, suspended/resumed games, and unknown opponent scenarios without requiring extensive custom coding in the front end.


*“ \[Front-end clients\] can get it exactly how they want it without having the different teams \[…\] have knowledge of each other or creating a problem downstream where all the clients are reimplementing the exact same logic from device to device, from team to team, etc.”*


Rob Engel, VP. of Software Engineering, MLB


The graph’s ability to handle complex business logic, tailored responses, and standardized representations significantly benefited MLB’s scalability and agility. Notably, the introduction of a personalization aspect through user subgraphs empowered MLB to deliver personalized experiences without overburdening client-side logic. This move towards personalization represents a major business shift enabled by the midfield graph layer. Reflecting on the transition, Rob Engel has stated that the adoption of GraphQL was recognized for enforcing strongly typed data contracts, simplifying documentation, centralizing business logic, and averting sprawls of backend-for-frontend (BFF) solutions. This centralized approach prevented inconsistent data contracts and reduced complexities, offering a streamlined and standardized solution across MLB’s ecosystem.


## Next Steps


Watch the[API World 2023 Keynote with MLB](https://www.apollographql.com/events/tech-talk/graphql-and-rest-true-bffs/) to learn more about MLB’s journey to improve their baseball fan experience with GraphQL, reducing data redundancy, optimizing response times, and enhancing overall data accessibility. Want to learn more about why GraphQL and REST are true BFFs, read the “[Design a Resilient API Strategy with GraphQL](https://www.apollographql.com/resources/design-a-resilient-api-strategy-with-graphql) ” whitepaper today.


Explore other inspiring[customer stories](https://www.apollographql.com/customers) and discover how Apollo GraphQL can help future-proof your API strategy.


*Note: Dan Boerner also contributed to this blog post.*


Written by


Ishwari Lokare


[Read more by Ishwari Lokare](https://www.apollographql.com/blog/author/ishwari)
