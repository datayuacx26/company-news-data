---
schema_version: "1.0.0"
document_id: "b7c1b72886f158bbd00839d240a4128a7b9f8e55ba61cb7f17bd99d55c5b0446"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-interns-on-graphql-radio-4af512f5d3b3"
published_at: "2017-08-22T17:58:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:641f3f18a4efe22f8ee556b128146c7445f6be117ca324083c76278b807db28b"
---

# Apollo interns on GraphQL Radio

One of my favorite times of year at the office is when the interns join us for the summer. This summer was the biggest group of interns yet, with 6 interns working on open source and commercial products at MDG. Three of the interns —[Shadaj](https://medium.com/u/47e634bb4796?source=post_page-----4af512f5d3b3----------------------) ,[Ramya](https://medium.com/u/18ee0b5365cd?source=post_page-----4af512f5d3b3----------------------) , and[Evans](https://medium.com/u/712ce1f56160?source=post_page-----4af512f5d3b3----------------------) — were working on Apollo open source technology, and so[Abhi](https://medium.com/u/6d7ee7c0b225?source=post_page-----4af512f5d3b3----------------------) and[Nikolas](https://medium.com/u/72347b4464b3?source=post_page-----4af512f5d3b3----------------------) from[GraphQL Radio](https://graphqlradio.com/) suggested they come on the show to round out the summer. Watch the episode below or[listen on Soundcloud](https://soundcloud.com/graphql-radio/graphqlradio-interns-audio) !


Here are the details and links talked about in this episode, sorted in the order they were sitting from left to right:


## Shadaj Laddad


Shadaj is a senior at Lynbrook High School, and has been programming for a long time. He’s been historically interested in Scala, but was working mostly in JavaScript during his internship. Here’s what he worked on:


- **Subscriptions tutorial:** Shadaj continued the Full-stack GraphQL tutorial with two steps about subscriptions:[Server-side](https://dev-blog.apollodata.com/tutorial-graphql-subscriptions-server-side-e51c32dc2951) and[client-side](https://dev-blog.apollodata.com/tutorial-graphql-subscriptions-server-side-e51c32dc2951) .
- **Apollo Cache API:** He refactored Apollo Client internals to separate out the internal GraphQL cache implementation behind a clean API. In Apollo Client 2.0, the cache will be completely pluggable, so that people can integrate caches based on different technologies like MobX, and build new features like offline support.[Check out his PRs to add this support here.](https://github.com/apollographql/apollo-client/pulls?q=is%3Apr+author%3Ashadaj) There are already people in the community working on alternative cache implementations that will be compatible with Apollo Client 2.0, such as[apollo-cache-hermes](https://github.com/convoyinc/apollo-cache-hermes) !
- **Scala.js and codegen:** In his spare time, Shadaj implemented a typed wrapper and even GraphQL query code generation for Scala for Apollo Client, which can be used with React and Scala.js.[Read the post here.](https://dev-blog.apollodata.com/scala-js-and-apollo-better-together-b066c6b09af4)


Shadaj’s pick at the end of the podcast was[Scala Native](http://www.scala-native.org/en/latest/) , an ahead-of-time compiler for Scala.


## Ramya Nagarajan


Ramya is a sophomore at MIT studying Electrical Engineering and Computer Science. She joined the Apollo team for the summer to work on tools that could help people be more productive in their work, and boy was she successful! Here’s what she worked on:


- **Apollo Client Devtools:** First, Ramya started out by learning about Apollo and writing[a post about different use cases for the devtools](https://dev-blog.apollodata.com/inspecting-your-graphql-client-in-the-browser-2481f8bcdd55) . The[Apollo Client Devtools](https://github.com/apollographql/apollo-client-devtools) allow you to send queries to your server, inspect the queries and mutations on your page, and look at the current state of the cache. Since they were built in a hack week, the original implementation relied on a brittle polling approach that used a lot of resources and even had the potential to lock up your browser. Ramya[rewrote the architecture to use a message-passing approach instead](https://github.com/apollographql/apollo-client-devtools/pulls?q=is%3Apr+author%3Aramyanaga+is%3Aclosed) , which was infinitely more efficient and also made the tools more reliable.
- **New features for Launchpad:**[Apollo Launchpad](https://dev-blog.apollodata.com/introducing-launchpad-the-graphql-server-demo-platform-cc4e7481fcba) is the open source platform for GraphQL server demos. It’s got a ton of[GraphQL server examples](https://github.com/apollographql/launchpad) with complete working code that you can edit and download. Ramya dove into the complicated world of modern UI development to refactor the code to use modern libraries like React Router 4, add meta tags with React Helmet for social sharing, and a button to use Prettier to format the code.[See her work here.](https://github.com/apollographql/launchpad/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Aramyanaga%20)


Ramya’s pick at the end of the podcast was[Go](https://golang.org/) , a programming language she’s started to learn.


## Evans Hauser


Evans is a Master’s degree student at Carnegie Mellon University, studying Electrical and Computer Engineering. He got interested in interning at MDG because of his excitement about the Meteor platform, and got to work on the most futuristic feature of Apollo Client:[Apollo Link.](https://dev-blog.apollodata.com/apollo-link-the-modular-graphql-network-stack-3b6d5fcf9244) Here’s more detail about his projects:


- **Tutorial about input types and custom resolvers:** Evans added[another step](https://dev-blog.apollodata.com/tutorial-graphql-input-types-and-client-caching-f11fa0421cfd) to the full-stack GraphQL and React tutorial.
- **Apollo Fetch:**[Apollo Fetch](https://github.com/apollographql/apollo-fetch) is a simple GraphQL query fetcher that you can use anywhere you need to retrieve a result from a GraphQL server. It sends the result and returns a promise, and has a simple way to set options like headers.
- **Apollo Link:**[Apollo link](https://dev-blog.apollodata.com/apollo-link-the-modular-graphql-network-stack-3b6d5fcf9244) is the first completely modular GraphQL network stack, and is one of the core parts of Apollo Client 2.0. It allows you to create your own “links”, which encapsulate a bit of GraphQL client functionality like polling, retrying, subscriptions, or something else. Read[Evans’ newest blog post](https://dev-blog.apollodata.com/apollo-link-creating-your-custom-graphql-client-c865be0ce059) to learn how to make your own links. With Apollo Link, we hope to make Apollo Client 2.0 smaller and more customizable than ever!


Evans’ pick at the end of the podcast was[apollo-fetch-upload](https://github.com/apollographql/apollo-fetch/tree/master/packages/apollo-fetch-upload) , an extension to Apollo Fetch written by[Jayden Seric](https://medium.com/u/5bd1b4d20a6d?source=post_page-----4af512f5d3b3----------------------) to enable file upload support!


## Join us!


Thanks so much to Shadaj, Ramya, and Evans for joining us this summer, and to[Nikolas](https://medium.com/u/72347b4464b3?source=post_page-----4af512f5d3b3----------------------) and[Abhi](https://medium.com/u/6d7ee7c0b225?source=post_page-----4af512f5d3b3----------------------) for hosting the podcast! If you or someone you know would like to be an intern at Meteor Development Group, (or want to apply for a full-time position) please apply at[meteor.io/jobs](https://www.meteor.io/jobs/) !


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
