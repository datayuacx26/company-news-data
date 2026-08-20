---
schema_version: "1.0.0"
document_id: "8fe25b3090f267eab869e3d98e093894c8ed66e2509858a34daf662f8d37f29b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/render-frontends-faster"
published_at: "2023-07-14T10:58:08+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:6db1f4462534dcb7c98eacd92bcadfcd08a695280adc5f2b3f1424bf8c9bdb26"
---

# Render frontends faster

This post is a part of our “[How to power modern media apps with Apollo GraphOS](https://www.apollographql.com/blog/platform/media/how-to-power-modern-media-apps-with-apollo-graphos/) ” series. Also in this series:


- [Enable omnichannel experiences](https://www.apollographql.com/blog/platform/media/enable-omnichannel-experiences)
- [Manage versions of your API for third-parties](https://www.apollographql.com/blog/platform/media/manage-versions-of-your-api-for-third-parties)
- [Providing real-time data for consumers](https://www.apollographql.com/blog/platform/media/provide-real-time-data-for-consumers)


---


For nearly all media companies, performance is a critical feature. Providing a user experience which comes as close to instant as the holy grail. At the same time, users expect more sophisticated media consumption experiences like recommendations and reviews based on their own individual habits and historical choices, and this data often needs to be retrieved from multiple services.


In practice, this means striking a careful balance of what minimum data requirements must be met for the current user request to allow them to engage with content as quickly as possible, and – then progressive rendering of components not critical to the users initial request.


Fortunately for those who expose a GraphQL API to their front-end clients, GraphOS supports the @defer directive so that client developers can declaratively mark fields in a GraphQL queries to indicate “I want this data immediately” and “I can wait for this data, give it to me when you get it”. This powerful feature of Apollo Router allows client developers to exercise a new level of control over their requests and determine what data is most important to render first while also eliminating the need for service owners to draw awkward boundaries around slower-to-retrieve data so that clients may query them separately.


## Seeing @defer in action


The @defer directive can play a transformative role in enhancing user experience within a media streaming application. A media streaming application typically loads multiple pieces of data when a user opens a movie or a show’s detailed view — for example,. the core details, user ratings, reviews, and recommendations for similar content. The primary data for immediate user engagement could be these core details (like movie/show title, description, and a play button), while the other data (like user ratings, reviews, and recommendations) may be less critical for initial interaction.


Without @defer, the client must wait for all data to load before rendering the page, which will block the rendering of the page until all data is returned. However, with @defer, the application can load and display essential data swiftly, then progressively load and display the non-critical data as it becomes available, without async logic needing to be created by the engineer.


This approach not only reduces the perceived loading time, but also creates a more seamless and responsive user experience by allowing users to interact with the content faster. This also provides the added advantage of no longer needing to create this behavior manually in order to handle slow data boundaries.


Consider the following query.


```text
query GetMovieDetails($id: ID!) {
# I need movie immediately it is critical to the create intial
# rendering requirement for client


movie(id: $id) {
id
title
director {
name
}


# I need this in the future its not critical to intial rendering
# can you deal with this automatically and return to me when ready
... @defer {
reviews {
user {
name
avatar
}
rating
comment
}
}


# I need this in the future its not critical to intial rendering
# can you deal with this automatically and return to me when ready


... @defer {
relatedMovies {
id
title
coverImage
}
}
}
}
```


In the above query, the customer selects a movie to view more details, the server will then initially respond with the basic movie details like title, description, duration, release date, cover image, genres, and director’s name which will allow the application to immediately render these details on the screen.


Meanwhile, the cast information, reviews, and similar movie recommendations, marked with the @defer directive, are not critical for the initial rendering of the page. They will be awaited in the background by Apollo Router, and once ready, they will be sent to the client in a subsequent response and can then be rendered on the page. This optimizes the user’s experience by allowing them to start interacting with the movie’s main details quickly, while other, less critical information loads progressively.


## Get started with a media supergraph today


The best way to see the possibilities of a supergraph is to try one out.[You can explore a media supergraph and run real queries against it here.](https://studio.apollographql.com/graph/Media-Demo-Graph/variant/current/explorer)


We also have a series of case studies and blogs that dive into different elements of a typical media organization’s schema to illustrate how Apollo GraphOS help power essential features of modern media applications:


- [Provide real-time data for consumers](https://www.apollographql.com/blog/platform/media/provide-real-time-data-for-consumers)


If you’d like to talk to an Apollo expert about how a supergraph can power your media services,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=media-solutions-page) .


Written by


Daljit Summan


[Read more by Daljit Summan](https://www.apollographql.com/blog/author/daljit)
