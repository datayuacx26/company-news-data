---
schema_version: "1.0.0"
document_id: "6b579cd7a647a45048494b02011f6a8d88a0ea34eb8a2bcda660b98147e202f3"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/whats-coming-in-apollo-client-2-0-bcd8ea64acbd"
published_at: "2017-09-13T00:20:08+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:1096769f1a44ca36a750cfdea89e4f86e4f5c0dce08e570857af187fd8da96ef"
---

# What’s coming in Apollo Client 2.0

### **tldr;**


The 2.0 version of Apollo Client brings a faster and more flexible experience while keeping the API you know and love. It adds features like modifiable request chains and custom data stores while providing production-tested defaults. The 2.0 is more performant, smaller, and just the beginning of what’s next for Apollo! The best news is that the new version is a minimal change to the API, so updating should take less than five minutes.


## New features


Apollo Client 2.0 will include lots of important changes that address the needs of current and future GraphQL developers.


### Flexible network management with Apollo Link


The first large change for 2.0 is switching out the network interface for Apollo Link. Evans Hauser gives an overview of what the project is about[here](https://dev-blog.apollodata.com/apollo-link-the-modular-graphql-network-stack-3b6d5fcf9244) , but in a nutshell links provide a way to compose actions around requesting data and handling results. The design is meant to allow for interoperability between links that you will one day be able to install from the community, and custom links specific to your application. We are excited to see the community start to publish links for common needs, like` apollo-link-sentry` to report errors to Sentry.io,` apollo-link-offline` to support handling request while offline, and even` apollo-link-local` to support managing your local application state using GraphQL!


### Apollo Cache


The second important change was removing Apollo’s dependency on Redux, and allowing for custom cache implementations. The current normalized store is available as` apollo-cache-inmemory` and supports all of the features the 1.0 store supports. However, we’re excited to see new ideas and features come from custom stores![Ian MacLeod](https://medium.com/u/6ca780f916b8?source=post_page-----bcd8ea64acbd----------------------) has already done some incredible work on Hermes, a new caching package which is sooooo fast and handles large data sets especially well. We are currently trying out Hermes in our own products, like Apollo Optics. Custom caches open up a lot of new possibilities, including the use of native persistent storage instead of in-memory stores.


### Performance


As a result of some internal reworking, the 2.0 benchmarks anywhere from 2x to 5x faster depending on the shape of your data. When coupled with different store implementations, some results are showing an over 10x speed improvement for large data sets! We would love to hear if you see these results in your application.


### Bundle size


Execution speed is only one part of having a performant implementation. With 2.0 we took a focused approached on reducing the overall footprint of Apollo Client. The full 1.0 package weight of Apollo comes in around 45kb between Apollo Client, Redux, and graphql-tag with the core being 30kb on its own. The new 2.0 core is only 11kb, and getting smaller. When constructed to have all of the features of the 1.0 client, the new version adds less than 27kb to your app. Overall the new build is around 40% smaller.


### Future features


When combined, links and custom stores can be used for incredible new features. We’ve factored some core functionality into a new package called[apollo-utilities](https://www.npmjs.com/package/apollo-utilities) to make it easy to write custom features to suit your needs. Here are a few examples of possible customizations:


- Sending mutations to traditional REST endpoints instead of a GraphQL server
- Labeling data as user-specific with a[@user](http://twitter.com/user) directive, and cleaning those fields out of the store when a user logs out
- Racing a request between a service worker and a network call
- Setting cache TTLs
- Using[@defer](http://twitter.com/defer) to lazy load data from heavy operations
- Parsing of custom scalars like Dates
- Managing application state using GraphQL (my personal favorite)


And so many more!


To try out the beta, checkout the[upgrade guide](https://github.com/apollographql/apollo-client/blob/master/Upgrade.md) ! We would love your feedback as we work towards the official release!


## Small changes


Now that you are all excited about what can be done, let’s not delay the question you are most likely asking; what does my upgrade look like? I have good news for you: We expect nearly all of current Apollo Client users to only need to change one file! Here is an example showing before and after:


```text
import ApolloClient, { createNetworkInterface } from "apollo-client";


const client = new ApolloClient({
networkInterface: createNetworkInterface({ uri: "/graphql" }),
});
```


```text
import ApolloClient from "apollo-client";
import HttpLink from "apollo-link-http";
import InMemoryCache from "apollo-cache-inmemory";


const client = new ApolloClient({
link: new HttpLink({ uri: "/graphql" }),
cache: new InMemoryCache(),
});
```


It may not look like a lot, but these changes represent a large reworking of the core of Apollo Client. Instead of all of the features you *may* need being built into the core, we have split them out into new packages which can be picked and chosen as needed. Not using batching? No worries! Want to use a different store? No problem!


If you want to use a single package, we are working on an` apollo-client-preset` that will include all the defaults from the 1.0 version 🎉, making upgrading or getting started with all of the defaults even easier.


As you can see, we’ve set up Apollo Client 2.0 to become exactly the client your GraphQL application needs, and nothing more.


## Preview at GraphQL NYC


Earlier this month, I had the pleasure of being one of the speakers at the first ever[GraphQL NYC](https://www.meetup.com/GraphQL-NYC/) meetup, put on by the incredible[kurtiskemple](https://medium.com/u/8be181d9f877?source=post_page-----bcd8ea64acbd----------------------) ! I gave an introduction to Apollo Client 2.0 and shared some of my excitement about what this will mean towards using GraphQL in production for you and your users. Check out the video, and if you find yourself in the NYC area,[try to make it to a meetup](https://www.meetup.com/GraphQL-NYC/) !


## What’s next


The[2.0 pull request](https://github.com/apollographql/apollo-client/pull/1941) is merged and the next version is now the master branch! During the beta, we will be shipping new builds under the` beta` tag (` npm install apollo-client@beta` ) and we would **love** your feedback! Once the beta phase is done, we will formally recommend a release candidate so everyone can update! We will be writing upgrade guides and how-tos for the exciting new features like Apollo Link! After a few weeks in beta / release candidate, the 2.0 will be come the stable latest on npm! 🎉


Special thanks to[Jonas Helfer,](https://medium.com/u/39cbd38c57c8?source=post_page-----bcd8ea64acbd----------------------)[Evans Hauser,](https://medium.com/u/712ce1f56160?source=post_page-----bcd8ea64acbd----------------------)[Shadaj Laddad,](https://medium.com/u/47e634bb4796?source=post_page-----bcd8ea64acbd----------------------)[Sashko Stubailo,](https://medium.com/u/803918030a60?source=post_page-----bcd8ea64acbd----------------------) and[Ian MacLeod](https://medium.com/u/6ca780f916b8?source=post_page-----bcd8ea64acbd----------------------) for their help and guidance on this effort!


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
