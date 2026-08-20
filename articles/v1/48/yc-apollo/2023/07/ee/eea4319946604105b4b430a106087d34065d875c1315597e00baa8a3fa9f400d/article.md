---
schema_version: "1.0.0"
document_id: "eea4319946604105b4b430a106087d34065d875c1315597e00baa8a3fa9f400d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/ship-products-faster-with-sdui"
published_at: "2023-07-13T18:35:59+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:6b96467d34e64643b9c6097885c96c5063b3eae7cc208bb331d8d6e6d2b86c23"
---

# Ship products faster with SDUI

This post is a part of our “[How to build connected travel apps with Apollo GraphOS](https://www.apollographql.com/blog/platform/travel/how-to-build-connected-travel-apps-with-apollo-graphos/) ” series. Also in this series:


- [Delivering personalized travel experiences with GraphQL](https://www.apollographql.com/blog/platform/travel/delivering-personalized-travel-experiences-with-graphql/)
- [Mitigate scraping and bot attacks with GraphOS](https://www.apollographql.com/blog/platform/travel/mitigate-scraping-and-bot-attacks-with-graphos/)
- [Seamlessly integrate with partners to enhance the travel experience](https://www.apollographql.com/blog/platform/travel/seamlessly-integrate-with-partners-to-enhance-the-travel-experience/)


---


When booking a vacation there are many different steps that require very different needs. From browsing and getting inspiration of travel destinations, to booking and enjoying the various experiences, and finally reviewing and posting about your trip and getting excited about your next destination, these all contribute to the[Traveler’s Journey](https://maddenmedia.com/travelers-journey/) . A common trend among travel companies today is figuring out how to provide the best experience for all our customers as they move through these phases and how can our technology and data capabilities adapt to serve them. This is where development teams are deploying Server-Driven UI with GraphQL.


## Where should biz logic go?


[Server Driven UI](https://www.apollographql.com/docs/technotes/TN0032-sdui-and-graphql/) (SDUI) is a design pattern that aims to push business logic from the client side to the server. But how much business logic? That depends on your team’s requirements, but a general rule of thumb is “as much as possible.”


What can be tricky for developers is identifying what is considered business logic. If you ask any developer what business logic is, they may tell you it should include things that revolve around the domain data, like pricing or customer info, and that UI or view logic is meant for the front end apps.


However, we all know that is never a full-proof approach, and over time functions that merge and transform the API responses trickle into the clients. We also tend to have more than one app consuming our APIs and so any logic that needs to be updated across them would take a lot of development effort. This leads us to a state where making a seemingly simple update to a line of text requires considerable work and coordination across many different teams and code bases to get everyone synchronized.


## Update in one location


If you find that you and your teams are consistently running into these issues it may be time to start adopting a SDUI API. If there is any logic that your product team may want to change, experiment on, or update frequently, it makes sense that it should live on the server side so that you can make updates immediately and not have to wait for all the apps to update or even longer for an app store release.


Every team will have different product requirements and so we view SDUI not as a binary adoption or library, but as a sliding scale of how much logic is moved to the server and you can be on many different parts of the scale:


As you continue to discuss among your team what can be controlled by the server you might start with things like all the text for localization and formatting. Next may be images and controlling the scale and size given the device info. Then you might consider whether a particular component or view is shown to a given customer which can be controlled by GraphQL nullability.


As you work your way through the app it might make sense to even control what components are used and in what order they are rendered. This is probably most useful in landing pages or pages that have to show many different types of components, like a[trips page for a travel website](https://www.apollographql.com/customers/expediagroup/) :


```text
query TripsPage($userId: ID!) {
currentTrip(userId: $userId) {
id
products {
... on Flight {
...FlightFragment
}
... on Hotel {
...HotelFragment
}
... on TripSuggestion {
suggestions {
... on Flight {
...FlightFragment
}
... on Hotel {
...HotelFragment
}
}
}
}
}
}


fragment HotelFragment on Hotel {
id
name
reviewScore
}


fragment FlightFragment on Flight {
id
origin {
airportCode
}
destination {
airportCode
}
}
```


You will notice that we have a generic` product` query above. We had to discuss with our design and product team what types of travel products we might want to render, the different designs we might want to display, and if there were any changes in the future they wanted to make.


This query might seem overly simple, but that’s by design. What’s not shown here is that now on the server side, we can control which order they come from the server, when we render suggestions, and what products are inside of them. Using[Federation](https://www.apollographql.com/docs/federation) , we can also split up the data resolution so that all the trips API needs to track is the product IDs. If we want to make a change to this logic we just need to update the server and deploy and our customers will start seeing the new data without having to update their app, which they probably won’t be doing while on vacation.


```text
type Query {
currentTrip(userId: ID!): Trip
}


type Trip @key(fields: "id") {
id: ID!
# The logic to decide what products to show lives here and is easily updated but we only need to store the IDs
products: [TripsPageResult]
}


union TripsPageResult = TripSuggestion | Hotel | Flight


type TripSuggestion @key(fields: "trip { id }") { trip: Trip }
type Hotel @key(fields: "id", resolvable: false) { id: ID! }
type Flight @key(fields: "id", resolvable: false) { id: ID! }
type User @key(fields: "id", resolvable: false) { id: ID! }
```


Building an SDUI app requires synchronization between the backend and frontend teams to agree on the components we want to use. Using a design system and a component library makes this process easier, but more importantly, taking a[demand-driven schema design approach](https://www.apollographql.com/events/tech-talk/schema-design-best-practices-with-apollo-federation-3/) will lead to the schema that works best for your clients that use it.


## Get started with a travel supergraph today


Beyond using SDUI as a powerful to design, develop, and secure your GraphQL APIs, the best way to see the possibilities of a supergraph is to try one out.[You can explore a travel supergraph schema and run real queries against it here](https://studio.apollographql.com/public/apollo-travel-supergraph/variant/prod/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgDMcAZgJwAsATBRALQQCsADB6-T7Va0OANhisAjBmqcA7ACNZMLhUYgANOGjxkaTLgIBRfFgBOAT2JkQMWRDhcMGCawwQYIvhAAcvCPW9PWThaWQ55DHlbRlkQAF8gA&referrer=operation_collections) .


We also have additional posts in this[series of travel best practices](https://www.apollographql.com/blog/platform/travel/how-to-build-connected-travel-apps-with-apollo-graphos/) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern travel applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your travel experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=travel-solutions-page) .


Written by


Shane Myrick


[Read more by Shane Myrick](https://www.apollographql.com/blog/author/shane)
