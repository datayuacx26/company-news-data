---
schema_version: "1.0.0"
document_id: "89514b72bf229ae6e2af16d14f3037b553a2666e4da21760b8b3fa822a60400b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-workbench-a-better-way-to-design-federated-graphs"
published_at: "2021-07-21T10:30:04+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:e976e985a3764eb86a32c0cf3400af9a7d88498745345f3fdc99c0b623b24226"
---

# Apollo Workbench: A Better Way to Design Federated Graphs

I’ve been helping companies develop their graph using Apollo Federation for over two years now, and it’s amazing to see all the different applications they’ve enabled.


In developing one graph, companies often struggle to define a process for effectively governing changes to the federated schema. Some companies use a Google doc, while others use a blank piece of paper to plan out a new schema change. Regardless, there is always *some* medium used as a “template” for the desired governance process. In my experience, these primitive mediums always come with unnecessary challenges, whether it’s an electronic document that doesn’t have the context to the subgraph schemas or a monorepo that requires starting up all other services to plan a schema change.


We need a better way to design schema changes in a federated GraphQL architecture — one where we can **understand our graphs composition at design time instead of the implementation phase** . You shouldn’t have to write any code when planning a schema design; you should change be able to change the schema directly.


For this reason, we built Apollo Workbench: a better way to design your federated graph — whether you have an existing graph (monolith, BFF, schema stitched), you’re migrating to Apollo Federation, or you’re just getting started.


Apollo Workbench is a tool built from experience working with our customers as they needed help designing their graphs. It dramatically reduces the feedback loop time when planning our changes to your graph. Specifically, Workbench helps you build faster with:


- Composition Errors – For checking your design as you go
- Query Explain – Understanding how a query will execute based on its generated query plan
- Mocking your designs – This considerably improves the DX for client developers by enabling them to build directly against a locally mocked federated graph in development instead of having to spin up all the backing services or point to a production endpoint


Let’s take a closer look at each.


## Composition Errors – Checking your design


While working with companies on building their graph, the most common problem I saw was teams designing a schema change that led to some composition errors. After planning the design change in some electronic document, everyone would agree on the changes so the work would begin. Teams would implement the changes, but when the developer starts up the` ApolloGateway` instance, it wouldn’t be uncommon to find that it doesn’t start and that it has composition errors:


Other teams plan their design changes around a monorepo structure. The benefit here is that they could merely start everything and understand composition errors upfront, thereby improving the feedback loop. The challenge here arises when creating a new subgraph with a lot of *copypasta* throughout the project.


Today, Apollo Workbench enables you to work with your subgraph schemas directly, with any composition errors displayed prominently in the VS Code Problems Panel:


This is great because now we can understand our design’s overall composition without starting anything up or writing any code. It also enables more people to be involved in the process since you’re just working with the schema directly. There is also IntelliSense for a composed graph where available types to extend are displayed (along with the owning service).


Suppose you’re using Apollo Studio and have integrated with the schema registry using Rover. In that case, you can log in to Apollo Workbench with[your user API key from your Personal Settings in Apollo Studio](https://studio.apollographql.com/user-settings) . Then you can create designs from your graphs in Apollo Studio:


## Query` EXPLAIN`


One of Apollo Federation’s significant benefits is understanding precisely how any given GraphQL operation will execute across your graph. This understanding comes in the form of a query plan generated for each unique operation. I like to think of the query plan as` EXPLAIN` for GraphQL queries. As teams design changes to their graph, they may need to contextualize a change to a GraphQL operation; this could be from a new set of functionality or a schema change that impacts operations currently running in Production.


With Apollo Workbench, you can add an operation to any design and view the query plan for any composing design:


Query` EXPLAIN` provides us with instant feedback on how a design might impact some existing operations. We all want performant queries, right? When you’re tweaking a design, it’s a great idea to start by looking at the query plan first and understanding how it changes with the tweaks you make.


If you’re using Apollo Studio and logged into Apollo Workbench, you can also click on a graph to load the operations from that graph variant. Then you can pull that operation into your design and see how it would execute:


## Mocking your design


Once you’ve created a design and you’re ready for the next steps, what do you do? A lot of companies explore some strategies around mocking to enable teams to develop independently. Client teams typically have some tooling to generate types, and they’ll want to do that for the new schema without waiting for the server team to spin something up. Apollo Workbench provides a simple mocking engine that will generate a mocked graph for any design you create.


Press the play button, and a mocked server will run locally for each subgraph and the graph router (` ApolloGateway` instance). The router will default to port` 4000` , and the subgraphs will start by default on port` 4001` and increment from there:


*Hint: there are settings available in the extension to change what port you want the mocks to run on:*


```text
"apollo-workbench.gatewayPort": 5000,<br>"apollo-workbench.startingServerPort": 5001
```


With Apollo Workbench, anyone can run a design with mocked data locally. The extension also has` faker` integrated into it, and you can write your own custom mocks:


*Note: a workbench file contains everything in the design, including the code snippet for your custom mocks.` eval` is used for the` JavaScript` that you write and a project is managed in the extensions global storage which provides you with feedback if you have errors.* Now you have all the tools you need to plan your graph designs with a complete understanding of what will execute without any code. Apollo Workbench provides insight into the graph and enables you to work only with the subgraph schemas to understand extension points, composition errors, and query execution plans. Once you’re complete with a design, you can mock it locally and use[Apollo Sandbox](https://studio.apollographql.com/sandbox/explorer) to explore your graph.


## Getting started


To get started with Apollo Workbench for your next graph design,[check out the official docs for installation instructions.](https://apollographql.github.io/apollo-workbench-vscode/)


Workbench works best when you can design all your changes from one centralized schema registry. If you haven’t already, I recommend you[sign up for an account with Apollo Studio](https://studio.apollographql.com/signup) and[push your subgraph schemas into the registry](https://www.apollographql.com/docs/studio/schema/cli-registration/) with Rover. Oh yeah and don’t forget, the schema registry is free on our community tier forever.


Happy coding!


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
