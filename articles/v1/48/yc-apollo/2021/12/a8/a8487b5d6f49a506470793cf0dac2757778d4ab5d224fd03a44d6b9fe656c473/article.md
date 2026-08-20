---
schema_version: "1.0.0"
document_id: "a8487b5d6f49a506470793cf0dac2757778d4ab5d224fd03a44d6b9fe656c473"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/why-apollo-client-is-an-important-tool-in-your-graphql-stack"
published_at: "2021-12-14T10:06:40+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:1612b0b8a14039bf17efa294606fce2cc846fbabf9f6f4fb90b8fbaff14d3e0e"
---

# Why Apollo Client is an important tool in your GraphQL stack

**This is a guest post by Rares Matei, senior engineer at Nrwl.io. He created a course on Apollo Client called[Manage State in React Apps with Apollo Client and GraphQL.](https://app.egghead.io/playlists/manage-state-in-react-apps-with-apollo-client-and-graphql-a45b3b89)** *The course is free to take up until December 17th, 2021.*


## Intro


Apollo Client has a unique position in our front-end apps: like a proxy sitting right at the edge, all the outgoing and incoming requests go through it, it knows what data each component needs and what the server responds with. All this knowledge puts it in a uniquely powerful position. And it turns out Apollo Client handles all this responsibility beautifully: with its powerful cache, baked-in defaults for keeping interactions fast, APIs for handling the most common frontend problems, and tools for plugging in your local state and even handling REST requests, you’ll notice it guides you towards better user experience and writing more maintainable and readable components.


## Synchronise server and client state


Synchronizing data between a client app and a server is hard. You make the request, wait for it to resolve, you then need to parse the response and update your local model, and finally, you have to ensure all your components are properly connected to your different data points so that they re-render when a change occurs. If your request actually mutates data on the server, then it gets even trickier since you need to make sure the mutation happens in local memory as well.


Apollo abstracts all that away with a powerful in-memory cache: your components declare what data they want in the shape that they want it and Apollo will start building up this local sub-graph representation of your server graph, and ensure no unnecessary network requests are ever made. If two components end up requesting the same slice of your sub-graph, regardless of how they define their queries, Apollo will point them to the same location in the cache.


[Just by using Apollo Client to make your queries, without any special configuration, your app will become faster.](https://app.egghead.io/lessons/apollo-why-have-a-client-side-graphql-cache) (I created a series of videos around this, and if you click some of the links in this article, I will go more in-depth around some of the topics)


Do you need to make a mutation?[Apollo will automatically update its normalised representation of your updated object, and will re-render all components listening to that part of your graph](https://app.egghead.io/lessons/apollo-update-the-local-cache-automatically-when-making-mutations) , regardless of the shape of their query.


And you are not locked into its default behaviours. If you have special cases where you need to reach into the cache to manually modify,[Apollo fully opens that up with powerful APIs.](https://app.egghead.io/lessons/apollo-manually-modify-the-cache-to-remove-deleted-items)


As you let Apollo worry about state management, and data synchronization your components will start becoming simpler, more maintainable. They’ll start describing the shape of the data they need in a single place. And Apollo[even allows us to sprinkle local client-only state in those queries, so that we don’t have to maintain a separate mechanism for handling it](https://app.egghead.io/lessons/apollo-use-read-field-policies-to-query-local-client-state) :


## Solve common frontend challenges


Working on front-end apps is challenging. You are operating on this small island where the only way to communicate with the outside world is via network requests. And those network requests can be fast, or occasionally very slow. They might throw an error. The server might change its data and you might be left displaying stale views to the user. And how would you even build a potentially infinitely scrolling list, like on Twitter?


While the cache sits at the core of Apollo, because of its unique position as a proxy of all incoming and outgoing network requests, it does offer lots of APIs on top of it to help with some of the most common frontend problems in existence:


- simple, boolean “[loading](https://app.egghead.io/lessons/apollo-handle-loading-states-for-your-graphql-queries) ” and “[error](https://app.egghead.io/lessons/apollo-handle-graphql-query-error-states-using-apollo) ” states for each query
- [retries for your failed queries, with optional exponential back-off and jittering](https://app.egghead.io/lessons/apollo-use-the-retrylink-in-apollo-to-retry-queries-after-an-error)
- [simple polling for keeping queries in sync with your backend](https://app.egghead.io/lessons/apollo-keep-queries-in-sync-with-the-backend-by-polling-for-new-data)
- after you’ve made a query,[you can subscribe for further real-time updates](https://app.egghead.io/lessons/apollo-append-graphql-subscription-updates-to-existing-queries)
- [build infinitely scrolling lists or paginate data](https://app.egghead.io/lessons/apollo-use-fetchmore-and-merge-field-policies-to-dynamically-load-more-items-into-a-list)
- [enable optimistic updates](https://app.egghead.io/lessons/apollo-use-optimistic-updates-to-build-uis-that-react-immediately-to-user-actions) so that your UI updates instantly when you make a mutation, even if the server still hasn’t confirmed it. If there’s an error later on, Apollo automatically rolls back the optimistic UI update.
- and many more!


## Extend as needed


With its sensible defaults, but open APIs, Apollo lets you extend its capabilities as and when you need them. You are not locked into a specific workflow, and we briefly saw examples of this above when talking about handling local state and manually modifying the cache under advanced scenarios.


And with its extensible linker API, Apollo’s capabilities become ready to handle even the most advanced requirements.


Are you still using REST but are planning to migrate to GraphQL?[You can start writing your components as if you are making GraphQL queries, and Apollo will transform them to REST calls behind the scenes](https://app.egghead.io/lessons/apollo-incrementally-migrate-from-rest-to-graphql-using-the-restlink) .


Another great example of this is[syncing your in-memory cache to your browser’s localstorage](https://app.egghead.io/lessons/apollo-provide-quick-app-start-up-times-by-syncing-the-apollo-cache-with-local-storage) , to provide almost instant app start-up times.


## Conclusion


Apollo does a lot out of the box. The more you trust and understand its cache, the less you’ll fight with hard-to-understand bugs. The more you read its docs, the higher the chance you’ll find an API that can elegantly handle a feature you’re building. And the more you get familiar with it, the more linkers you’ll find yourself using, that unlock new advanced capabilities.


Since using Apollo in my frontend projects, I’ve started enjoying the process of coding a lot more as well, because I don’t have to worry about whether I’m updating all my local data correctly and whether the right components know about the update. I get nudged towards creating better user experiences and writing simpler, more declarative code. Looking at how Apollo handles all the complex challenges of synchronizing data between the client and the server has taught me to become a better web developer myself, and I’ll continue to apply these learnings in all future projects I’ll work on, regardless of the technologies it uses.


And if you’d like to fully understand what I mean, regardless of whether you’ve never used Apollo before or are an advanced user,[I created a video course here where we’ll cover in-depth most concepts discussed in this article (and many more!), all while building practical features in a real-world app.](https://app.egghead.io/playlists/manage-state-in-react-apps-with-apollo-client-and-graphql-a45b3b89)


Written by


Rares Matei


[Read more by Rares Matei](https://www.apollographql.com/blog/author/raresmatei)
