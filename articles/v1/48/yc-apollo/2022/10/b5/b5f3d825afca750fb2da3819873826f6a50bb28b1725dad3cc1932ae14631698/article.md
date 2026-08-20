---
schema_version: "1.0.0"
document_id: "b5f3d825afca750fb2da3819873826f6a50bb28b1725dad3cc1932ae14631698"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-apollo-graphos"
published_at: "2022-10-05T09:29:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:809f269d6384dad6b138a7ba3d97979f4b17eee738a9d85fe0d690a70645cfb3"
---

# Introducing Apollo GraphOS

When we built Apollo in 2016, we had a vision. We set out to build one distributed and scalable data layer that could connect to anything we need, no matter the client, the architecture, the source, the language. It was a lofty goal, but work worth doing, because solving this gets to the heart of how to make app development more approachable and productive.


We realized this vision with[the supergraph](https://www.apollographql.com/blog/announcement/backend/the-supergraph-a-new-way-to-think-about-graphql/) , a new layer of the stack that unifies an organization’s data, services, and capabilities, built around three underlying principles: 1) One unified composition layer; 2) Built in modules; 3) That evolves over time.


Today, we’re announcing Apollo GraphOS, a complete cloud platform that lets anyone build, connect, and scale any supergraph. GraphOS is the execution fabric for the supergraph – a powerful runtime that connects backend and frontend systems in a modular way. It also has an integrated control plane to help developers manage, evolve, and collaborate on the supergraph.


GraphOS lets you connect all your data and services seamlessly with federation. It gives you advanced capabilities like` @defer` without having to write any code or sign up for any ops burden. And it’s secure – we’ve achieved SOC 2 Type 2 compliance, and passed a full audit by Doyensec with flying colors.


## **Why GraphOS?**


We hear a lot from developers that they want to do more with the graph. They want to scale, but they really don’t want to have to think about it. They just want to build products.


We wanted to solve this. Not everyone has a big ops team or the GraphQL expertise or the desire to run infrastructure for the supergraph themselves. They need something that’s easy to start with and that can grow along with them – a supergraph operating system that can scale to any number of subgraphs, across every continent, at lightning speed.


So we built one.


## **What’s in it?**


Apollo Router is the heart of GraphOS – built in Rust, designed for performance. We partnered with Fly.io to build our own cloud-based supergraph runtime on top of Apollo Router, designed for exactly what the supergraph needs. It gives us the global reach of an edge network with the core computing capability and performance to support new and upcoming features like query planning, @defer, live queries, and so much more to come.


We paired this global supergraph runtime with our complete set of schema delivery, collaboration, and observability tools so developers can publish changes to the supergraph and seamlessly deliver them across all their apps. And, last but not least, we included a supergraph development kit that can connect any existing graph to the supergraph.


## **Our supergraph development kit**


We hear from thousands of GraphQL developers that there is a disconnect between how they start building graphs, and how they scale them. That disconnect leads to breaking changes, bloated schemas, and uncertainty about the path forward.


If we have learned one thing about graphs, it is that they MUST mature, change, and adapt. The supergraph doesn’t work if it requires a rewrite of the entire existing system. It’s not the v2 of a schema. It’s about modularizing what already exists – transforming an existing graph into a module and elevating it into a full-fledged supergraph.


What if you could take what you’ve already built, and say: ok this isn’t the end. This is just my first module. Where can I go from here? What else can I build from here?


Let’s add a new service; a new function.


And another.


And another.


All individual and unique, each written in whatever language I choose, each independent and flexible. But all connected.


The supergraph development kit is here to make this all possible with three components that create a seamless developer experience:


- **` rover dev` :** Spins up a supergraph and runtime locally, making the graph you have your first module.
- **` rover template use` :** Gives anyone access to a cookbook of templates for adding new modules in all the most popular languages and frameworks.
- **Sandbox:** A GraphQL IDE designed for local supergraph development and testing.


## **Priced to use**


Alongside GraphOS we are launching a new[usage-based pricing](https://apollographql.com/pricing) model that matches how developers build and scale the graph.


It starts free with a generous free quota to get you on the supergraph – 10M queries every month.


Beyond that, it’s priced as it should be – by the query on a pay-as-you-go basis.


GraphOS is an essential part of the toolkit for any GraphQL developer who is looking to build better apps fast while avoiding technical debt, mitigating risks, and staying competitive in a demand-driven world.


It’s free and takes just a few minutes to get started. We can’t wait for you to[get on the supergraph](https://studio.apollographql.com/signup?referrer=blog) .


Written by


Matt DeBergalis


[Read more by Matt DeBergalis](https://www.apollographql.com/blog/author/matt)
