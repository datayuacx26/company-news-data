---
schema_version: "1.0.0"
document_id: "4f439d6ebf6b1cf9bb40afdd4a285004794a20c17ecf918405c5977b6416217f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/an-unexpected-journey-how-netflix-transitioned-to-a-federated-supergraph"
published_at: "2024-07-10T09:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:820911baf9f885ebf9c9df56e7ebd513626334fa184854b94ff81f36d8c3e5b2"
---

# An Unexpected Journey: How Netflix Transitioned to a Federated Supergraph

Netflix is the world’s[most popular streaming service](https://www.digitaltrends.com/home-theater/most-popular-streaming-services-by-subscribers) . While everyone knows how it paved the way for a whole new industry when it moved from its physical DVD business to the cloud, you may not know how Netflix’s API platform powered that transition. As Bruce Wang, Director of Engineering of Games Platform at Netflix shared at a[Champions Corner Webinar,](https://www.apollographql.com/events/champions-corner/netflixs-unexpected-journey-to-the-supergraph/) they went through four generations of APIs before transitioning to[GraphQL Federation](https://www.apollographql.com/docs/federation/) . Read on to learn the many twists and turns of their “unexpected journey” as they broke up their API monolith, used the graph to promote collaboration, manage their tech debt and break down organizational barriers.


## Netflix’s API evolution


### Act I: The beginnings of an API-first strategy


The year was 2007 — Netflix had amassed 8 million subscribers and 1.25 billion in revenue just ten years after it launched its DVD shipping business. With the rise of bandwidth speeds, the company saw an opportunity to upsend the competition and pivot to online streaming.


It was at this point that Netflix moved onto the cloud and began its API journey. In 2008, development teams built OpenAPI to share Netflix’s growing catalog. “Early on, we were looking at the golden age of APIs where everyone shared their public APIs, and you could then build your experience on top. But by 2011, 99% of the OpenAPI was used for internal consumption.” Bruce shared.


Netflix’s many streaming devices, such as PS3, Xbox, Windows Phone, LG, Roku and others, relied on the REST-based OpenAPI, and Netflix faced challenges supporting user interface agility at a single layer with distributed REST endpoints. To adapt, Netflix created a new API platform, dubbed “API.NEXT” that allowed any UI team to build APIs on top of a single Java-based service layer.


### Act II: Transitioning to the Graph


By 2015, Netflix was going to global markets and thousands of devices and form factors, they hit a wall with an oversaturation of lightweight APIs and scripts. The unintended consequence of giving engineers a very flexible API layer was that they created many paths to get data from the backend teams to the frontend applications.


To solve this problem, the company developed a graph language called Falcor, which allowed them to create a single JSON API to pull data. However, this effort at simplification quickly became complex again as the new Falcor API existed side by side with the previous “OpenAPI” and “API.next.” The obvious next step was to combine all these APIs into a single graph monolith where all clients could access everything they needed from the service layer below.


### Act III: Netflix Studio’s Rise spurs need for federated architecture


By 2019, Netflix Studio had become one of the biggest production studios in the world. With so much growth, the Netflix Studio team quickly began to feel the pain of their monolithic architecture. They started to explore ways to break apart the API monolith, which included prototyping both a federated graph gateway and a[GraphQL aggregation layer](https://www.infoq.com/presentations/netflix-api-graphql-federation/) that would tie all of Netflix’s services together.


Concurrently, Apollo released[Apollo Federation](https://www.apollographql.com/docs/apollo-server/federation/federation-spec/) , which offered distributed ownership and subgraph API implementation. Netflix Studio was already using GraphQL, so Federation was the natural next step to support Netflix’s next-generation architecture, Studio Edge.


Studio Edge offered easy discovery and code generation, backend control of schemas, and a single source of truth. It also allowed the API team to focus on key areas of leverage, and platform partners could invest in developer experience, subgraphs, a console UI, and observability.


### Netflix API: The sequel


By 2021, Netflix had 200 million subscribers, 25 billion in revenue, 25x growth and pivoting toward original, global content. To remain competitive in a market with dozens of new competitors, Netflix launched games and Tudum (a fansite). With these new offerings, it wanted to use lessons learnt from Studio Edge and apply them to the core consumer experience.


**“And so then we had a choice,” Bruce said. “We could build on top of our existing system — Falcor — or we could adapt the newest technology that Studio Edge was pioneering. And so this is when we started cultivating a federated experience with different teams that owned individual subgraphs.”**


## Lessons learned in the transition to the supergraph


One of the main things the company learned during its federation journey is that developer experience is a critical component for both user interfaces and API production. So, how do you invest in the developer experience to set your company up for success?


### Lesson #1 – Learn how to detangle the monolith


Netflix quickly learned that its teams had to build an extremely powerful API layer by being able to leverage GraphQL to stitch older systems and new systems together. “One of the things that we did early on when we started adapting GraphQL, is figure out how do we detangle the monolith,” Bruce highlighted.


Their answer was to create a bridge layer – pulling out the powerful service layer in the monolith, and then having the new GraphQL layer access the service calls cleanly. Rather than attempting a big bang migration away from its monolith, Apollo Federation allowed them to take a modular approach by combining separate subgraphs for service teams with the agility of the monolith on a supergraph.


### Lesson #2 – Invest in the schema developer experience (DX)


The second lesson Netflix learned was to invest heavily in schema DX. There are many services involved in the subgraph development model, but the construction of the schema and API product itself is crucial to support developers. Netflix’s ultimate goal was to make collaboration simple, and they learned from previous mistakes that improving schema developer experience makes this possible.


Studio Edge is Netflix’s largest and oldest graph, with 150 subgraphs, 3000 plus types, and 2800 queries and mutations. It’s a massive graph powering 50-60 internal apps at one time. With the success of Studio Edge and queries from internal partners, Netflix continues to merge APIs using the power of the supergraph.


### Lession #3 – Manage your tech debt and organizational barriers


The truth is that tech debt is inevitable. This means being able to work with your monolith — or whatever system you have — is an asset.


And don’t underestimate organizational barriers, either, Bruce emphasized. GraphQL enables different methods of collaboration between UI teams and backend teams, but it can’t solve fundamental organizational barriers. “Thinking about your product rather than the individual services is a huge, huge win,” Bruce emphasized.


## Looking toward the future


Netflix is operating in a vastly different environment than it was even ten years ago. Streaming has exploded, rights competition is fierce, and hefty new players appear on the market constantly.


So to remain agile, Netflix focuses on the following questions when looking toward the future of its APIs:


- What is the true value of our APIs?
- How do we create consistency and resiliency?
- How do we make the developer experience excellent?
- How do we manage our tech debt?
- How do we prepare for challenges with the graph?
- How can we use the monolith to our advantage?
- What are our own organizational barriers?


But above all, Netflix focuses on building an open culture. Bruce said, “You really want to build a learning culture where the main focuses are trusting teams, seeking out excellence, and striving for customer satisfaction. APIs are a journey, and if you can build a learning culture that changes and adapts, you’ll have a winning formula.”


## Conclusion


GraphQL federation allows companies to maintain flexible control of their APIs through subgraphs while scaling their existing systems — resulting in better customer experiences. By learning how to detangle your monolith, invest in the schema developer experience, and manage debt and organizational barriers, Netflix unlocked API success with[Apollo Federation](https://www.apollographql.com/docs/federation/) .


You can read more about Netflix’s journey with GraphQL federation[here](https://www.apollographql.com/blog/redefining-api-strategy-why-netflix-platform-engineering-chose-federated-graphql) . If you’re ready to go all-in and federate your graphs with Apollo,[contact](https://www.apollographql.com/contact-sales?referrer=nav-contact-sales) one of our representatives to get started.


Written by


Ishwari Lokare


[Read more by Ishwari Lokare](https://www.apollographql.com/blog/author/ishwari)
