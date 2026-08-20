---
schema_version: "1.0.0"
document_id: "5d94d5f44624013f08440ca632f8df4e74b49d1ee0b620d8601cbf21eceb528a"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/volvo-cars-drives-into-the-future-of-online-car-shopping-with-the-supergraph"
published_at: "2022-12-01T16:48:44+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:b98ee53bfc38b93e913b553dbe411498303e7498790ef12ae30c5b68ef221425"
---

# Volvo drives into the future of online car shopping with the supergraph

Volvo Cars is on a journey to have at least 50% of their sales online by 2025. To achieve this goal, the organization needed to create a consistent and intuitive user experience across its website. However, Volvo’s system landscape was so complex that it was taking developers over a week to figure out how to combine APIs into new experiences. To complicate matters further, there were site delays that caused abandonment and shoppers weren’t seeing accurate pricing information or graphics.


In his[Champions Corner webinar](https://www.apollographql.com/events/champions-corner/how-volvo-is-building-a-better-digital-car-buying-experience/) , Stephan Lonntorp, Senior Engineering Manager at Volvo Cars, shares how implementing a supergraph with Apollo GraphOS is helping Volvo address these challenges to build a better digital car buying experience that increases online sales.


## **Struggling to scale with a GraphQL monolith**


When Stephan started on this journey, he began with a GraphQL monolith. He replaced different API gateways with a schema that brought context to the data, abstracting away the complexity of how it’s pieced together. But, as more teams started using the graph, the monolith strained his team. Changes took a long time to implement and experiences broke daily without warning. (A typical incident being that not every car would show on the website.)


The central graph team was always called to troubleshoot, which meant they couldn’t work on anything else – even though the incidents were due to underlying systems breaking.


## **Unlocking agility with the supergraph**


After realizing the monolithic graph wouldn’t scale, he adopted a supergraph, powered by[Apollo GraphOS](https://www.apollographql.com/graphos) , to allow each team to own and manage their data end to end. A new layer in the tech stack, the supergraph unifies all cloud data and services into a single connected map that developers can query with GraphQL.


*We want our entire business on the supergraph because we think it’s better than those dusty old REST API’s. We wouldn’t have an accurate online shopping experience without the supergraph.*


Stephan Lonntorp


GraphOS also delivers visibility into what APIs are being used, so his teams can safely evolve their subgraphs and know who to talk to about potential breaking changes. Today, unplanned breaking changes are fully eliminated.


And, since the supergraph aggregates data from 40 different REST endpoints into one single source of truth, frontend **developers are finding the data they need 99.5% faster.**


The supergraph is also inspiring developers to experiment with the data they now have at their fingertips. Stephan says, “ *First, you understand what the supergraph is and what it can do for you. Then you realize you can ask for whatever data you want. Just because you’re working on a certain part of the website doesn’t mean you can’t ask for data they’re displaying somewhere else. If you have an idea you can go to the supergraph and start experimenting.* ”


Stephan was promoted to run engineering for the online fleet sales business and build out its online service offering. Since most of the data they needed was already in the supergraph, they only had to add a few pieces to the business domain supergraph to build the new products.


*Without the supergraph, we’d still be trying to figure out how to build a user journey for the fleet business.*


Stephan Lonntorp


One new area they built out is company car policies, which enables a fleet manager to set up rules specific to a company and then the car configurator would just show the options allowed by that policy. **They built the initial prototype for that new feature in two and a half weeks** .


*The supergraph is key to achieving our online sales goals because it makes data readily available for developers to use across our website, allowing us to show consistent information throughout the user journey on volvocars.com. At the end of the day, that is what’s going to help us sell more cars.*


Stephan Lonntorp


[Watch Stephan’s webinar](https://www.apollographql.com/events/champions-corner/how-volvo-is-building-a-better-digital-car-buying-experience/) to hear more about Volvo’s journey to improve their digital car buying experience with the supergraph. And, be sure to check out the rest of our[Champions Corner webinars](https://www.apollographql.com/events#type=champions-corner) *.*


Written by


Melissa Annecchini


[Read more by Melissa Annecchini](https://www.apollographql.com/blog/author/melissa)
