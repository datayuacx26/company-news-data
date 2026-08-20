---
schema_version: "1.0.0"
document_id: "cfd8f50d85e5cab4d692145f3ac824bed60c9b4769ed4d2516c463fcdfd40f89"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/delivering-personalized-travel-experiences-with-graphql"
published_at: "2023-07-14T12:47:41+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:80733e051231772e3b63c678fc875b2680a62d1b14d6bd74ce9ff6c978a8fadf"
---

# Delivering personalized travel experiences with GraphQL

This post is a part of our “[How to build connected travel apps with Apollo GraphOS](https://www.apollographql.com/blog/platform/travel/how-to-build-connected-travel-apps-with-apollo-graphos/) ” series. Also in this series:


- [Ship products faster with SDUI](https://www.apollographql.com/blog/platform/travel/ship-products-faster-with-sdui/)
- [Mitigate scraping and bot attacks with GraphOS](https://www.apollographql.com/blog/platform/travel/mitigate-scraping-and-bot-attacks-with-graphos/)
- [Seamlessly integrate with partners to enhance the travel experience](https://www.apollographql.com/blog/platform/travel/seamlessly-integrate-with-partners-to-enhance-the-travel-experience/)


---


In the ever-evolving travel industry, companies are recognizing the value of[delivering personalized experiences](https://medium.com/expedia-group-tech/ai-personalization-and-openness-exploring-the-definitive-tech-trends-of-2023-d5ba45875c80) to their customers. To achieve this, travel companies are investing in a unified data source that offers a consolidated view of a user’s journey. However, this data consolidation must be achieved while still accommodating the existing microservice architectures that enable separation of concerns.


Traditional approaches often fall short in providing a comprehensive and aggregated view of a customer’s data from multiple sources. Data duplication and fragmentation across services hinder the ability to create a holistic and personalized travel experience.


Fortunately, Apollo GraphOS and Apollo Federation come to the forefront, offering a solution that combines the benefits of a federated model with the flexibility required for service decomposition. This powerful combination enables travel companies to maintain a single unified view of their data while still leveraging the advantages of microservice architectures.


In this post, we will delve into the transformative potential of GraphOS and Apollo Federation in revolutionizing the travel industry by enabling highly personalized experiences. We will delve into the concept of a unified data source as the single source of truth, providing a consolidated view of a traveler’s journey, and discuss the advantages of using a federated model that allows service decomposition while maintaining data unity.


## A unified interface


In traditional approaches like REST APIs, constructing a complete view of a customer’s travel journey requires clients to make multiple requests, tracking various data dependencies and associated business logic.


For example, in order to generate recommendations for a traveler’s journey, the service responsible for generating these recommendations needs to be aware of the current state of the traveler’s journey (e.g. whether the traveler is on the trip, or is looking for recommendations prior to booking a trip) as well as specifics on which products the traveler has purchased in the past. Service teams would need to ensure they’re managing these dependencies appropriately, and clients would have to know which fields are required to generate these recommendations and forward them to the appropriate endpoints. A typical REST-workflow for a client may look like this:


```text
GET user/{id}/trip # Fetch a list of trips for the user
GET recommendations/trip/{id} # Fetch recommendations for the trip
# Fetch details for each recommendation, either hotel or flight based on
# the type
GET hotels/{hotel_id} # Fetch hotel details
GET flights/{flight_id} # Fetch flight details
```


REST Based Approach requiring chaining of API calls


One of the key benefits of GraphQL federation is its ability to manage dependencies between services. By using federation-specific directives like` @key` , services can mark up their schemas to indicate the fields they depend on or provide to other services. This allows the graph to intelligently track and resolve these dependencies, ensuring a complete and accurate customer view.


A simplified approach with the Apollo Router and federation using a single GraphQL request


## Leveraging Federation for enhanced traveler experience


Apollo Federation enables service owners to expose the GraphQL fields that they’re responsible for and using GraphOS and composition, ensure that their changes will be composed into a single unified graph.


A service team responsible for providing recommendations then, can contribute a schema like so:


```text
type TripSuggestion @key(fields: "trip { id }") {
trip: Trip
suggestions: [Product]
}
union Product = Hotel | Flight
```


The` TripSuggestion` type utilizes Federation’s` @key` directive to indicate its dependency on a trip, specifically referencing the` id` of the trip. By doing so, the field resolvers for this type can assume that the` id` of the trip will always be available.


Note that the suggestions field of this type is a list of` Product` ‘s, which itself is a union of the` Hotel` and` Flight` types. This schema design significantly simplifies the client logic required to fetch and display a user’s suggestions. Client developers can leverage fragments to specify the particular types they are interested in and handle them accordingly. For example, a client can fetch the recommendations along with the they need with a single request:


```text
query CurrentTrip($userId: ID!) {
currentTrip(userId: $userId) {
products {
... on TripSuggestion {
suggestions {
... on Hotel {
id
name
}
... on Flight {
id
origin {
airportCode
}
}
}
}
}
}
}
```


By using the fragments` ... on Hotel` and` ... on Flight` , client developers can independently handle each case while making a single request to fetch all the necessary fields specific to hotels and flights.


This idea of using fragments to handle each case can be taken a step further, such as[when deploying server-driven UI](https://www.apollographql.com/blog/platform/travel/ship-products-faster-with-sdui) .


## Get started with a travel supergraph today


Beyond delivering highly personalized recommendations, the best way to see the possibilities of a supergraph is to try one out.[You can explore a travel supergraph schema and run real queries against it here](https://studio.apollographql.com/public/apollo-travel-supergraph/variant/prod/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgDMcAZgJwAsATBRALQQCsADB6-T7Va0OANhisAjBmqcA7ACNZMLhUYgANOGjxkaTLgIBRfFgBOAT2JkQMWRDhcMGCawwQYIvhAAcvCPW9PWThaWQ55DHlbRlkQAF8gA&referrer=operation_collections) .


We also have additional posts in this[series of travel best practices](https://www.apollographql.com/blog/platform/travel/how-to-build-connected-travel-apps-with-apollo-graphos/) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern travel applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your financial services experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=travel-solutions-page) .


Written by


Tushar Bhushan


[Read more by Tushar Bhushan](https://www.apollographql.com/blog/author/tushar)
