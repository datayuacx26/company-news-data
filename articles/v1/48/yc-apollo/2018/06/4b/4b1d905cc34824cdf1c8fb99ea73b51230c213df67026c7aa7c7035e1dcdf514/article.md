---
schema_version: "1.0.0"
document_id: "4b1d905cc34824cdf1c8fb99ea73b51230c213df67026c7aa7c7035e1dcdf514"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/easy-and-performant-graphql-over-rest-e02796993b2b"
published_at: "2018-06-19T22:44:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:911003569dbe214ce5f63f1175140f0e671e2cde3bea63ffb41a61f8ef17c6db"
---

# Easy and performant GraphQL over REST

Last Friday, we published a first release candidate of[Apollo Server 2.0](https://www.apollographql.com/docs/apollo-server/v2/whats-new.html) , a 100% open source JavaScript GraphQL server that bakes in the best practices we’ve learned working with hundreds of teams running GraphQL in production. This post introduces data sources, one of its new features.


In our experience, the right way to deploy GraphQL is as a layer between your apps and your existing APIs and services. This approach lets teams leverage and complement the investments they’ve made in microservices and infrastructure, and provides product engineers a straightforward, low-friction path to rolling out GraphQL.


That makes patterns for loading data from REST APIs especially important, so we’ve made a built-in answer to that a key part of Apollo Server 2.0. That’s data sources — a new pattern for fetching and caching data from REST endpoints that replaces DataLoader.


Fetching & Caching Data from REST endpoints


## Data sources — a best pattern for fetching from REST APIs


Data sources are classes that encapsulate fetching data from a particular service, with built-in support for caching, deduplication, and error handling. You write the code that is specific to interacting with your backend, and Apollo Server takes care of the rest.


It’s easy to get started. All you need to do to define a data source is to extend the` RESTDataSource` class and implement the data fetching methods that your resolvers require. Your implementation of these methods can call on convenience methods built into the` RESTDataSource` class to perform HTTP requests, while making it easy to build up query parameters, parse JSON results, and handle errors.


```text
class MoviesAPI extends RESTDataSource {
baseURL = 'https://movies-api.example.com';


async getMovie(id: string) {
return this.get(`movies/${id}`);
}


async getMostViewedMovies(limit: number = 10) {
const data = await this.get('movies', {
per_page: limit,
order_by: 'most_viewed',
});
return data.results;
}
}
```


Data sources allow you to intercept fetches to set headers or make other changes to the outgoing request. This is most often used for authorization. Data sources also get access to the GraphQL execution context, which is a great place to store a user token or other information you need to have available.


```text
class PersonalizationAPI extends RESTDataSource {
baseURL = 'https://personalization-api.example.com';


willSendRequest(request: Request) {
request.headers.set('Authorization', this.context.token);
}


async getFavorites() {
return this.get('favorites');
}


async getProgressFor(movieId: string) {
return this.get('progress', {
id: movieId,
});
}
}
```


You pass the data sources to use as an option to the` ApolloServer` constructor:


```text
const server = new ApolloServer({
typeDefs,
resolvers,
dataSources: () => {
return {
moviesAPI: new MoviesAPI(),
personalizationAPI: new PersonalizationAPI(),
};
},
context: () => {
return {
token:
'foo',
};
}
});
```


Apollo Server will then put the data sources on the context for every request, so you can access them from your resolvers. It will also give your data sources access to the context. (The reason for not having users put data sources on the context directly is because that would lead to a circular dependency.)


```text
Query: {
movie: async (_source, { id }, { dataSources }) => {
return dataSources.moviesAPI.getMovie(id);
},
mostViewedMovies: async (_source, _args, { dataSources }) => {
return dataSources.moviesAPI.getMostViewedMovies();
},
favorites: async (_source, _args, { dataSources }) => {
return dataSources.personalizationAPI.getFavorites();
},
},
```


## What about DataLoader?


[DataLoader](https://github.com/facebook/dataloader) was designed by Facebook with a specific use case in mind: deduplicating and batching object loads from a data store. It provides a memoization cache, which avoids loading the same object multiple times during a single GraphQL request, and it coalesces loads that occur during a single tick of the event loop into a batched request that fetches multiple objects at once.


Although DataLoader is great for that use case, it’s less helpful when loading data from REST APIs because its primary feature is *batching* , not *caching* . What we’ve found to be far more important when layering GraphQL over REST APIs is having a resource cache that saves data across multiple GraphQL requests, can be shared across multiple GraphQL servers, and has cache management features like expiry and invalidation that leverage standard HTTP cache control headers.


In fact, we’ve worked with many teams that have implemented exactly this pattern in their GraphQL server, using something like Redis or Memcached as a backing store for the resource cache. Time and again these teams have told us how critical this cache is, but also that their homegrown implementations are incomplete and hard to maintain. Data sources is our answer for that.


## Partial query caching


Resource caching is especially valuable in a GraphQL server because it unlocks *partial query caching* — a design pattern that lets the GraphQL layer cache responses from underlying APIs and then assemble them into arbitrary query results without refetching from the server.


While a a REST endpoint is meant to represent a single resource, GraphQL allows clients to ask for the exact data they need in one request, combining data from a variety of sources. This makes whole GraphQL responses difficult to cache efficiently because they often contain data with very different lifetimes, or a blend of publicly cacheable and user-specific data.


Take this example from an actual customer API. The query asks for information about a TV series, as well as a season of episodes. All this is publicly cacheable (the same for all users), and also relatively static (cacheable with a long TTL). In addition, we ask for the progress for every episode however, which is specific to the current user and also likely to change often.


```text
{
series(id: "98794") {
title
description
season(number: 1) {
episodes {
id
episodeNumber
assetTitle
description
progress {
percent
position
}
}
}
}
}
```


If we were fetching data from the underlying REST APIs directly, we would fetch and cache season and episode data separately from the progress data. But because we’re asking for all of this in a single request, the response we receive from the GraphQL layer isn’t cacheable.


Data sources gives you the best of both worlds. Requests made through REST data sources are automatically cached based on the caching headers returned in the response, which many REST APIs already set. This means partial results are cached, but we retain the flexibility that comes with GraphQL to select and combine data from different sources.


The first time the query is executed, we start out by fetching the data for the series, then the season, followed by progress data for every episode.


As long as the cached content hasn’t expired, subsequent requests can load the series and season data from the cache, and only have to fetch progress data afresh.


By default, resource caching will use an in memory LRU cache. Apollo Server also includes support for using Memcached or Redis as shared cache backends. And soon, if you’re running on edge environments like[Cloudflare Workers](https://www.apollographql.com/edge) or[fly.io](https://fly.io/) , we’ll automatically use the cache built into those environments as well.


## Try it out


To get started, refer to the[data sources section](https://www.apollographql.com/docs/apollo-server/v2/features/data-sources.html) in the Apollo Server 2 docs. We’re eager to hear your input and learn from your experiences. As always, please get in touch on[Twitter](https://twitter.com/apollographql) or[Apollo Slack](https://www.apollographql.com/slack) if you’d like to help.


GraphQL Summit 2018 (Nov 7–8, San Francisco)


I hope you’ll also[join us at the 3rd annual GraphQL Summit on Nov 7–8 in San Francisco](https://summit.graphql.com/) . With over 800 attendees expected, it’s the largest GraphQL developer event in the world. Super early bird tickets are selling fast, so[register today](https://www.eventbrite.com/e/graphql-summit-2018-tickets-46601841362) to reserve your spot!


Written by


Martijn Walraven


[Read more by Martijn Walraven](https://www.apollographql.com/blog/author/martijn)
