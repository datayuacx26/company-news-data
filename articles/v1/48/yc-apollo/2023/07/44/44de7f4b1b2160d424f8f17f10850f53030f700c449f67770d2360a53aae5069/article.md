---
schema_version: "1.0.0"
document_id: "44de7f4b1b2160d424f8f17f10850f53030f700c449f67770d2360a53aae5069"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/enable-omnichannel-experiences"
published_at: "2023-07-14T10:43:48+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:378f7cade468fd922b57691bd28abdc889f152f12db33ec72cb6d132167cd5ad"
---

# Enable omnichannel experiences

This post is a part of our “[How to power modern media apps with Apollo GraphOS](https://www.apollographql.com/blog/platform/media/how-to-power-modern-media-apps-with-apollo-graphos/) ” series. Also in this series:


- [Render frontends faster](https://www.apollographql.com/blog/platform/media/render-frontends-faster)
- [Manage versions of your API for third-parties](https://www.apollographql.com/blog/platform/media/manage-versions-of-your-api-for-third-parties)
- [Providing real-time data for consumers](https://www.apollographql.com/blog/platform/media/provide-real-time-data-for-consumers)


---


“Omnichannel” has become the go-to term to describe how data is shared across a company so that customers will have a unified experience interacting with all parts of a business.


Because Apollo GraphOS is the go-to platform for providing a unified API experience to clients apps by connecting all of your data sources to a supergraph, it’s the perfect fit for creating omnichannel experiences. Let’s dive into how you can accomplish that with GraphQL.


## Federating a supergraph schema


A[federated GraphQL architecture](https://www.apollographql.com/docs/federation/) splits services into subgraphs so that each team can own a smaller, more focused domain, while still providing a single, unified supergraph schema for all of your clients to consume. This means that your client teams—whether they work on the website, mobile apps, TVs or tablets — can all use the same API to accomplish their specific goals.


Omnichannel therefore requires the ability to map data across all channels via a common data model. In the case of a media organization, this could be represented by a common Media type, which would mean you have an\`id\` field that can be used across all channels, or a way of converting it in each channel.


We could define that Media type in a subgraph schema as follows:


```text
# Media subgraph -----------------------------------------
type Media @key(fields: "id") {
id: ID!
title: String
description: String
mediaUrl: String
releaseDate: String
}
```


For example, in the separate subgraph for a reviews service, we could add a review field to the Media type by extending it like so:


By setting the id as the key field using the @key directive, we have turned the Media object type into an[entity](https://www.apollographql.com/docs/federation/entities/) so that any other subgraph can now add fields to the Media type or return the Media type from any of their fields, and all they need to know about a specific piece of media is it’s ID.


```text
# Review subgraph ----------------------------------------
type Media @key(fields: "id") {
id: ID!
"The reviews attached to this Media item"
reviews: [Review]
}
```


As more and more subgraphs extend additional fields on the Media type, client developers will be able to see the full representation of the Media type which will give them the ability to create intricate queries that span across many subgraphs, such as fetching reviews or even comments for the given piece of media as long as the backing services are aware of its unique ID value.


## Consistency across channels


An effective omnichannel strategy involves tracking a user’s movements across all your channels, such as web, iOS, Android, and TVs, along with the actions they take on each one. By creating a federated supergraph, each channel can operate independently while reporting to and consuming data from a single, reliable source.


This means that if a user starts streaming a show on one device and then switches to another one, they can pick up their interaction seamlessly from where they left off. This is a powerful concept and a key benefit to media organizations when working with a supergraph.


Another common strategy involves personalisation across all channels. For instance, if a user favorites a specific media item on one device, they might want that favorite item to be accessible wherever they go. Moreover, a collection of favorite items could activate the personalisation engine, which consistently makes recommendations across any channel your business operates. This approach results in a reliable and enjoyable experience for your customer.


To model that data in a GraphQL schema, we can see below that a User type (that was defined in a separate user subgraph that is owned by another team) can be extended by a recommendations subgraph to include a field that will return that user’s customized recommendations:


```text
# The Recommendation subgraph ----------------------------
type User @key(fields: "id") {
id: ID!
recommendations: Recommendation
}


type Recommendation {
media: [Media]
favorites: [Favorites]
}


# The User subgraph --------------------------------------
type User @key(fields: "id") {
id: ID!
email: String
username: String
firstName: String
surname: String
address: Address
}
```


Using the preceding schema we can leverage Apollo Federation to fetch recommendations for a specific user across all possible channels in a consistent manner. What’s more, the recommendation service doesn’t need to know anything about a specific user expect for the user’s unique ID.


In turn, the users service doesn’t need to know anything about the recommendations that are returned in the extended recommendations field because Apollo Router will take care of querying all of the data required by the client from the different subgraphs and assembling it the shape that the client is expecting. And because all front-end clients can query data from this unified API, media consumers can expect a consistent experience across all of their devices.


## Summary


Apollo Federation provides a huge boost in the way you can simplify your data fetching, by breaking up the larger problem into smaller more manageable bits and letting specialized teams own them. Here we demonstrated how you can use it to work with a media focused use case, we have shown you how you can have a centralized entity like Media and have multiple fields contributed by other teams without the Media domain team being the implementational bottleneck.


In this example we demonstrated how multiple domains in a media company use case can be easily brought together in a cohesive and maintainable fashion,. allowing the business to organically grow its business and respond to customer feature requests while maintaining experience parity across all channels.


## Get started with a media supergraph today


The best way to see the possibilities of a supergraph is to try one out.[You can explore a media supergraph and run real queries against it here.](https://studio.apollographql.com/graph/Media-Demo-Graph/variant/current/explorer)


We also have a series of case studies and blogs that dive into different elements of a typical media organization’s schema to illustrate how Apollo GraphOS help power essential features of modern media applications:


- [Provide real-time data for consumers](https://www.apollographql.com/blog/platform/media/provide-real-time-data-for-consumers)


If you’d like to talk to an Apollo expert about how a supergraph can power your media services,[please reach out to us.](https://www.apollographql.com/contact-sales/?referrer=media-solutions-page)


Written by


Daljit Summan


[Read more by Daljit Summan](https://www.apollographql.com/blog/author/daljit)
