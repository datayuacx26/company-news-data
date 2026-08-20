---
schema_version: "1.0.0"
document_id: "c8f8301ea2567eb5c8610f869fff9b7bbed8bbc5e8be679e63bff4997e5f83a3"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-block-scaled-graph-adoption-with-apollo-and-ai"
published_at: "2025-11-24T12:30:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:55:10.614706+00:00"
content_hash: "sha256:36bb4abf37236d02d2e81b2451029ea29dfb96c345fe39155f70cf0a3b48e85a"
---

# How Block Scaled Graph Adoption with Apollo and AI

## A recap from Block’s GraphQL Summit 2025 session on scaling graph adoption with Apollo Connectors, Apollo MCP, and AI


When Block’s GraphQL platform team discovered[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors?utm_term=apollo%20connector&utm_campaign=Google_Search_Brand_ApolloGraphQL&utm_source=google&utm_medium=cpc&hsa_acc=6083416509&hsa_cam=20595484542&hsa_grp=173217050114&hsa_ad=720509297031&hsa_src=g&hsa_tgt=kwd-2372618437622&hsa_kw=apollo%20connector&hsa_mt=e&hsa_net=adwords&hsa_ver=3&gad_source=1&gad_campaignid=20595484542&gbraid=0AAAAABiDybZ05Cofg_KP-qsYRLQcuFIHd&gclid=Cj0KCQjw9obIBhCAARIsAGHm1mRuBffxfq57JSMDMxQq4XpR6UD2C3T-QFfNKfRNGbKzMXMGobte74oaAmhSEALw_wcB) , they saw a path to bring thousands of existing APIs onto the graph without asking hundreds of engineering teams to run new services or learn new tooling. While Apollo Connectors are typically used for REST APIs, Block found a creative way to connect their gRPC services by leveraging their existing JSON translation layer. Paired with[Goose](https://block.xyz/open-source) , Block’s open-source AI agent, and[Apollo MCP Server,](https://www.apollographql.com/docs/apollo-mcp-server) that idea became a repeatable system any large organization can use to give developers the speed and flexibility of building on a unified graph.


“We started asking the most ambitious question that we could: What if we could use Connectors to bring all of the APIs at Block to our graph?” – Jonathan Wondrusch, Staff Software Engineer at Block


## **700 Teams, 3,000 Services, One Graph**


Block runs more than 3,000 internal services across 700 teams, all using gRPC and protobuf. Getting a single service onto the federated graph meant learning new tools, deploying a subgraph, and taking on an on-call burden, a cost many teams weren’t willing to take.


The business impact was clear. API fragmentation slowed cross-product features, delayed partner integrations, and created barriers to innovation. For a company spanning Square merchants and Cash App customers, every siloed API was a missed opportunity.


“Bringing a service into the graph meant learning a new way of working and setting up brand new infrastructure,” said Jonathan Wondrusch, Staff Software Engineer at Block “That’s a learning curve and an engineering cost many teams weren’t willing to take on.”


**Four Days That Changed Everything**


Apollo Connectors is designed to connect REST APIs directly to a federated graph, but Block’s services weren’t REST. They used gRPC with protobuf. The breakthrough came when someone noticed their frameworks were already translating protobufs to JSON behind the scenes. That meant they could treat their gRPC services as REST endpoints and use Apollo Connectors without changing backend infrastructure.


In a four-day hack week, a dozen teams brought their APIs into the graph with Connectors. No new services. No new infra. The results were so promising that the platform team pivoted its roadmap to make Connectors the default on-ramp to the graph.


As adoption grew, the team needed to scale beyond manual schema creation. At first, Goose generated TypeScript subgraphs instead of REST endpoints. After grounding it with Apollo MCP Server and up-to-date documentation, Goose began producing accurate Connector schemas with the right directives, types, and tests.


## **Making AI Reliable at Scale**


Rudra Srivastava, Software Engineer at Block, focused on reliability. Raw prompts gave inconsistent results, so the team built Goose Recipes, structured prompt flows that standardize connector generation across teams.


The improvement was clear:


- **Before** : merchant lists had no pagination and snake_case fields.
- **After** : Connector schemas used relay-spec pagination, correctly mapped field names, and established entity relationships with inaccessible ID fields.


Each recipe could be shared across the company, giving every team a clear path to production-ready Connectors.


“Goose recipes make AI behavior consistent and repeatable” Rudra Srivastava, Software Engineer at Block.


## **From Single Connectors to Entire API Surfaces**


To go from one Connector to many, the team built Flock, a multi-agent orchestration system that runs multiple Goose agents in parallel. It processes dozens of endpoints, creating fully functional connector subgraphs. The system runs on familiar patterns: queuing, polling, and workers managing four stages through separate queues.


A simple sequencing rule prevents type conflicts: if two endpoints modify the same type, agents pick them up in order. This raised success rates and reduced human fixes, while keeping human review where it matters.


Block maps authorization at the field level, aligning fields to group-based access and letting[Apollo Router](https://www.apollographql.com/docs/graphos/routing) policy prune fields the caller should not see. For migrating existing subgraphs to Connectors, the team uses the[@override directive](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/reference/directives) and its traffic-shift label to cut risk while they move ownership.


“Connectors are great for subgraphs that don’t really have business logic on top of their data.” – Rudra Srivastava, Software Engineer at Block


## **What Changed in a Year**


Block’s journey shows how far “vibe coding” has come, from manual prompts to structured workflows, and from REST APIs to generated user experiences around the graph.


They started with one question and ended with a repeatable system:


- Manual prompts became recipes
- Recipes evolved into sub-recipes
- Sub-recipes scaled through multi-agent orchestration with Flock


In just a year, what began as an experiment has become a production-ready foundation for how Block builds its platform.


“A year ago, we were all wondering if agents, models, and protocols would even work,” the team reflected. “Today, we are heavily invested in building with them, and maybe a year from now, we will wonder how we ever worked without them.”


To see how they did it, watch Block’s full GraphQL Summit 2025 session featuring Goose, Flock, and Apollo in action.


Written by


Valeria Gomez


[Read more by Valeria Gomez](https://www.apollographql.com/blog/author/vgomez)
