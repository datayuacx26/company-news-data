---
schema_version: "1.0.0"
document_id: "6da2f065583eefe9a7cb88bc7dd225000b61202f1902b527947f15c52bf1b9b7"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/what-is-apollo-federation"
published_at: "2022-02-09T16:00:24+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:1c77dccf83fdc46ef1931229e0aca80c65d396a15d5815688744e58dd167d258"
---

# What Is Apollo Federation?

You may have heard about Apollo Federation before, especially if you’re on a team that uses GraphQL and Apollo tools. But what is it exactly?


Apollo Federation is an approach to application architecture created by Apollo. It’s open-sourced so anyone has access. It’s not a product that we offer but instead, it’s a specification or a standard for building a distributed graph. It can be implemented in any language that is compatible with GraphQL.


But what does that entail? Why was Apollo Federation created to begin with? These questions will be addressed in this blog post and we’ll also cover the benefits of using the Apollo Federation specification and how you can get started.


### **Before you read…**


Before you dive into this article, here are a few things you should know:


- You know what GraphQL is. If you need help with this, check out this[What is GraphQL](https://www.apollographql.com/blog/graphql/basics/what-is-graphql-introduction/) article
- You have built a graph before. If you haven’t done this before or if you need a refresher, take a look at our[Odyssey course](https://odyssey.apollographql.com/) . You can also follow our[Full-stack GraphQL Tutorial](https://www.apollographql.com/docs/tutorial/introduction/) .


If you have an understanding of these things, you’re ready to continue ahead with this article!


## **Graphs Before Apollo Federation**


There is no doubt that GraphQL is great for building out complex data sets and APIs. But as a company, product, or project grows, so does your graph. And as your graph grows, you can develop a monolithic architecture and by that we mean you’ll end up with one huge graph. Monolithic architectures are great when we’re just starting out or when a project is fairly small in scope. But, if not organized correctly, a monolithic graph can present various problems that ultimately lead to slower development and unhappy developers.


With a monolithic graph, things can get messy and confusing quickly. With so much code in one place, monolithic graphs can be hard to navigate, especially for new developers who are onboarding or joining your team. It’s also difficult to keep monolithic graphs up to date without introducing any breaking changes. This becomes more and more problematic as your project or company needs grow and as you gain traffic.


These are problems that needed fixing. There were some solutions that were implemented. (One of them is[schema stitching](https://www.apollographql.com/blog/backend/graphql-schema-stitching/) , which we won’t dive into too much here). But none of them truly solved these issues in a practical way. So we at Apollo took some time to create the Apollo Federation specification.


## **The Benefits of Implementing Apollo Federation**


As mentioned earlier, Apollo Federation is an open-source architecture created by Apollo. It was created to help you and/or your team organize and implement your graph as it grows and evolves along with your application. So how exactly does it do this?


Apollo Federation is built around a design principle called[separation of concerns](https://www.apollographql.com/docs/federation/#separation-of-concerns) . Separation of concerns means that an application, codebase, or program is broken up into distinct sections, each section addressing a separate concern. How is this done with Apollo Federation?


With the Apollo Federation architecture, instead of having one large graph, you create a federated graph. This means your graph will be divided into smaller subgraphs. These subgraphs are usually broken up based on the function or purpose they fulfill in your application, team, or company. For example, if you or your team are building a restaurant review app, there would be a subgraph for reviews and a subgraph for locations.


This is what separation of concerns looks like and it’s super-useful. Apollo Federation makes it easier to modularize your code, keep things organized, and makes it easier for everyone to navigate your codebase.


If you’re working on a team, this also means that each team can have its own dedicated subgraph. So, returning to our previous example, the team that works on locations only has to deal with the locations subgraph. The team that works on reviews only has to deal with the reviews subgraph. This cuts down on breaking changes and communication while still maintaining necessary collaboration. Ultimately, this means that development will be smoother and quicker.


### Apollo Federation and Client-side Teams


With all these subgraphs, does that mean that folks on the client-side will have to hit several endpoints to change or retrieve the data they need? Thankfully, this isn’t the case! Apollo federation makes sure that while improving the experience for developers working directly with the graph, client-side developers don’t need to change anything about the way they work.


## Federated architecture


We know what the Apollo Federation standards are and how those can benefit individual developers and teams. But how is this architecture implemented?


An Apollo Federation architecture consists of the following components:


- A **collection of subgraphs** (usually represented by different back-end services) that each define a distinct GraphQL schema
- A **gateway** that uses a **supergraph** schema to execute queries across multiple subgraphs. The gateway creates the supergraph by combining the subgraph schemas together and serves only the supergraph schema to clients.


## To sum it up, here’s what Apollo Federation can do for you


The Apollo Federation architecture is great for everyone.


- For individual/solo developers: modularizing your graph means your code will be more organized, easier to navigate, and easier to scale.
- For teams: with the separation of concerns principle, each team can deal with their own subgraph which ultimately increases team productivity and collaboration.
- For client-side developers: their work is basically unchanged so they still get the benefits of using GraphQL aka only accessing one endpoint and only dealing with the schema.


## **Getting Started with Apollo Federation**


After reading this, you might be thinking that Apollo Federation is something you want to try for yourself or your team. There are a few places that will help you learn more about Apollo Federation and get started with implementing it. Check out the[Apollo Federation specification](https://www.apollographql.com/docs/federation/federation-spec/) for more info on the principles behind the architecture. We also have a[Federation quickstart](https://www.apollographql.com/docs/federation/quickstart/) that can get you up and running with a step-by-step guide.


Written by


Ceora Ford


[Read more by Ceora Ford](https://www.apollographql.com/blog/author/ceora)
