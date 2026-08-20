---
schema_version: "1.0.0"
document_id: "587d254908cbdf6b05886b0a952724c6612cb140fb1c3b43709e1fab56aa88a5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/what-i-learned-at-graphql-summit-f61d6fc6680a"
published_at: "2019-11-14T18:39:44+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:bdb3257473ce1273c36af3bf3cd99c0f48e5da1aeb062dea8a03d689cbd6bd6f"
---

# What I Learned At GraphQL Summit

This year, I flew out to San Francisco to attend (and help host!) my very first[GraphQL Summit](https://summit.graphql.com/) .


As a Canadian flying to San Francisco for the first time, I was delighted to escape the Toronto cold to explore a new city, meet my awesome new Apollo colleagues, hang out with puppies (oh yeah—we had a puppy booth 🐶), chat with devs from around the world, and of course learn everything I could about the present and future of GraphQL.


From the two days of talks, roundtables, and hallway conversations, some exciting themes emerged at Summit:


- **GraphQL is mature and growing.** It’s here to stay, and it’s poised to become the preferred API convention as client-side UI demands grow in complexity.
- **The data** **graph provides numerous architectural advantages for companies with rich client-side experiences.** It simplifies client-side data fetching, ensures a much more pleasant developer experience, and improves developer velocity ⚡.
- **The React Hooks (useQuery) + Apollo stack is solid.** And just *fun* to code with.
- **There’s no free lunch in software.** Large-scale GraphQL deployments encounter challenges similar to any other service-based architecture. But teams can mitigate those challenges with tools like 🏘️ **federation** (to prevent error-prone monoliths and enable the development of separate graphs by cross-cutting teams) and 🔒 **schema change validation** (to ensure that schema changes don’t break live queries in production).


Let’s dive deeper into each of these!


Puppies + GraphQL = Win


## GraphQL is mature and adoption is growing


### Adoption


GraphQL champions from companies of all sizes came together to share their experience in adopting, scaling, and simplifying their product development using GraphQL.


[Brie Bunge](https://twitter.com/briebunge) showed us the tactical migration strategies that Airbnb used to incrementally migrate to React, Apollo, and GraphQL from a REST and Redux architecture 🔥. Definitely check out Brie’s talk, “[Migrating to Apollo + GraphQL at Airbnb](https://www.youtube.com/watch?v=pywcFELoU8Ehttps://www.youtube.com/watch?v=pywcFELoU8E) ” if you missed it.


We heard both from Brie and throughout Summit that the best approach to start using GraphQL is to **adopt it incrementally** . A GraphQL endpoint can live alongside REST ones, and you can convert REST calls into GraphQL operations one at a time. And using React Hooks like Apollo Client’s` useQuery` instead of writing Redux boilerplate might give you butterflies in your stomach (the happy kind).


### Caching


Caching plays a huge role in every GraphQL architecture—on the client, on the server, and everywhere in between.


On the client-side, Apollo released a beta of[Apollo Client 3](https://blog.apollographql.com/previewing-the-apollo-client-3-cache-565fadd6a01e) , which features several cache performance updates and introduces fine-grained controls for cache eviction and garbage collection. To get caught up to speed, definitely check out[Ben Newman](https://twitter.com/benjamn) ’s talk on[“Fine Tuning Apollo Client Caching.”](https://youtu.be/n_j8QckQN5I)


And on every other side, architects[Sandosh Vasudevan](https://twitter.com/sandosh) and[Aditi Garg](https://twitter.com/adigarg94) of Zillow Group gave an eye-opening talk called “[Cache All the Things](https://www.youtube.com/watch?v=czzanixJG2I) ” that covered the **Connector-Model-Resolver-Service pattern** and how the team implemented caching at the GraphQL data layer (with Data Loader), the HTTP layer (with Varnish), and the DNS layer (with CoreDNS).


Data Source Pattern used by Zillow Group on Trulia.com


### Understanding tradeoffs


[Marc-Andrè Giroux](https://twitter.com/__xuorig__) from GitHub gave a talk titled “[Caching & GraphQL: Setting the Story Straight](https://www.youtube.com/watch?v=CV3puKM_G14) ”.To me, the theme of this talk can be summarized in one word: *pragmatism.*


Marc reminded us to refrain from speaking in absolutes.Context matters when we’re considering technical tradeoffs, andit’s important that we consider that context when deciding whether GraphQL (or any technology, really) is right for the task at hand. Although GraphQL is performant, its greatest benefit (alongside developer experience) is its *customizability.*


As Marc said:


API styles are a spectrum based on tradeoffs. From customization to optimization, we have GraphQL (most customizable), complex field filters, response partials, REST, and RPC (most optimized).


This was an *aha* moment for me *.* It’s important to know the tradeoffs of the tools that we use. GraphQL is fantastic for the things it’s fantastic for, and it doesn’t have to be fantastic for every conceivable use case.


## The data graph is central to the modern web application stack


The **data graph** is a new layer in the tech stack that sits right in between the client and your services, APIs, and databases.


The need for this new layer comes from the fact that user experiences have become increasingly more complex, and the technical details involved in communicating with services can be cumbersome and can degrade developer experience.


In[Matt DeBergalis](https://twitter.com/debergalis) ’ opening keynote, he clarified that “using the data graph is a **better way to do apps** ” because:


- All your data is in one place.
- It decouples your apps from your data.
- It enables us to build better and faster, with powerful tools and less code.


I’m all for it. As someone who’s spent countless hours writing Redux reducers, actions, and action creators to solve the same problemsin every app (data fetching logic, success states, failure states, and so on), I’m ready for change.


### BFF: a step in the right direction


The dependencies between client-side applications and back-end services also made it clear to me that we’re ready for a better way to do things. It can be messy to stitch together several back-end services managed by different teams in order to build a new client-facing product on top of them.


This is exactly the problem Expedia Group faced in their architecture:


Expedia Group’s architecture before adopting a data graph: client and service complexity


One way they tried to solve this problem was with the **Backend-for-Frontend** (BFF) pattern.


BFF attempts to streamline a client-side application’s dependence on several backend services by defining a *new* back-end service specifically for the client-side application, like so:


Expedia Group’s architecture with a BFF


However, from what we learned during[Michelle Garrett](https://twitter.com/msmichellegar) ’s talk,[Scaling GraphQL Beyond a BFF](https://youtu.be/vfYcsgQBTU0) , at Summit, “duplication is inherent” in BFF.


BFF creates a 1-to-1 relationship between the back-end service and the client-side app that it relies on, and results in a lot of duplicated code between BFFs.


### The data graph: everybody’s BFF


To resolve their technical debt, Expedia Group made the decision to build a GraphQL-based data graph that could be used by all of their clients:


How Expedia Group replaced their BFF with a data graph and eliminated technical debt


It’s in my nature to consider pushing back on adding another layer to the stack, but I really think this is justified, especially for companies of this size with many client-side experiences.


Now clients only ever know about that single GraphQL endpoint in order to fetch and write data to the system.


## Tooling for big problems


### Federation


Because I’m virtually obsessed with software design, architecture, and domain-driven design, federation was the topic that I was *personally* most interested in. How do we break a monolithic enterprise application into smaller pieces so that we can assign teams to them, and then how do we connect those pieces together?


In his talk “[The Architecture of Federation](https://www.youtube.com/watch?v=LKQKn1oFXJU) ”, Apollo’s[Jeff Hampton](https://twitter.com/jhampton) walked us through the need for federation, how teams previously solved the “separation of concerns” problem before federation, and how we can do it now.


What’s awesome about federation is that we can compose multiple services together in a declarative and straightforward way by specifying directives to indicate where services connect. Best of all, the federated schema is just GraphQL, so you can query it normally from the client!


Composing schemas declaratively with federation


I think this is super cool, and I’m excited to spend a lot more time exploring what federation can do.


### Best practices for GraphQL in production


As more and more companies roll out GraphQL in their production environment, the larger community continues to lay the critical foundation for best practices around frontend workflows, iterative schema development, and more.


There was no shortage of talks that provided actionable next steps for these real-world challenges:


For TypeScript adoption, check out:


- “[The GraphQL developer experience](https://youtu.be/q-eJvp9CMY8) ” by[Danielle Man](https://twitter.com/danimman) from Apollo
- “[Adding Typescript to your GraphQL server](https://www.youtube.com/watch?v=kyXGMorwKgU&list=PLpi1lPB6opQyraZSmwFre_FpL00_3nTzV&index=11) ” by[Zhifan Li](https://twitter.com/iszfan) from Intuit
- “[Game Of Types: A Song Of GraphQL And TypeScript](https://www.youtube.com/watch?v=AgJ1n75ibCo&list=PLpi1lPB6opQyraZSmwFre_FpL00_3nTzV&index=24) ” by[Steven Musumeche](https://twitter.com/smusumeche) from Formidable Labs


For state management best practices, check out:


- “[State Management in GraphQL using React Hooks & Apollo](https://youtu.be/Y3jXNMhAy0U) ” by[Shruti Kapoor](https://twitter.com/shrutikapoor08) from PayPal
- “[Client-side GraphQL at scale](https://www.youtube.com/watch?v=lo8BpB17jf0&list=PLpi1lPB6opQyraZSmwFre_FpL00_3nTzV&index=29) ” by[Chris Sauvé](https://twitter.com/_lemonmade) from Shopify
- “[A Treatise on State](https://youtu.be/tBz3UmZG_bk) ” by[Jed Watson](https://twitter.com/JedWatson) from Thinkmill


For schema design and evolution, check out:


- “[The Do’s and Don’ts for your schema and GraphQL operations](https://www.youtube.com/watch?v=fG8zy1OROp4&list=PLpi1lPB6opQyraZSmwFre_FpL00_3nTzV&index=26) ” by[Michael Watson](https://twitter.com/ThreeBrewMates) from Apollo
- “[Agile schema evolution and graph management](https://www.youtube.com/watch?v=XAL8MiDN-O0) ” by[Evans Hauser](https://twitter.com/evanshauser) from Apollo


## Final thoughts on GraphQL Summit


Two inspirational days left me excited to run home to unpack the mountain of invaluable information I gathered at Summit. I wish I somehow could have split into three different people and attended all of the talks, but luckily,[they’re all available on YouTube 🎉](https://www.youtube.com/watch?v=EDqw-sGVq3k&list=PLpi1lPB6opQyraZSmwFre_FpL00_3nTzV) .


There were so many gems that I missed out on in person, like “[How to talk to your boss about GraphQL](https://www.youtube.com/watch?v=7783Wo9OgQI&list=PLpi1lPB6opQyraZSmwFre_FpL00_3nTzV&index=17) ” by[Brandon Paquette](https://twitter.com/brandonmp44) , “` useSubscription` : A GraphQL Game Show” by[Alex Banks](https://twitter.com/MoonTahoe) , and “[Building offline-first apps with GraphQL & Apollo](https://www.youtube.com/watch?v=6icuEm8c8a8&list=PLpi1lPB6opQyraZSmwFre_FpL00_3nTzV&index=46) ” by[Kiran Kumar Abburi](https://twitter.com/kiran_abburi) . I’m excited to binge watch these talks next!


Enjoy, and see you next year!


---


*All of these incredible learnings from GraphQL Summit wouldn’t have been possible without the help of our Platinum sponsors,* ***Hasura*** *and* ***DGraph*** *.*


***Hasura*** *is an open-source GraphQL engine that helps you instantly setup a scalable and realtime GraphQL backend. Hasura makes your team super productive by dynamically composing a schema backed by databases and services that you can securely query from frontend clients. Get started at*[hasura.io](http://hasura.io/) *to try it out in 30 seconds!*


***Dgraph*** *is the GraphQL database: an open-source graph database that scales to your needs, with incredible reliability and performance thanks to being highly optimized and horizontally scalable. And what’s best, it also comes with a native GraphQL API so you don’t need to learn any new language to unlock the power of the most popular open-source graph database. Get started today and integrate Dgraph with your favorite GraphQL clients and tools at*[graphql.dgraph.io](http://graphql.dgraph.io/) *.*


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
