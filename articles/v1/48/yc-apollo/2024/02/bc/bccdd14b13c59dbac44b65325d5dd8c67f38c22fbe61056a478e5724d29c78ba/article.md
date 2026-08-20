---
schema_version: "1.0.0"
document_id: "bccdd14b13c59dbac44b65325d5dd8c67f38c22fbe61056a478e5724d29c78ba"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphos-schema-proposals-a-principled-development-workflow-for-graphql-api-platforms"
published_at: "2024-02-08T12:09:52+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:039f1ad60c6a6a27e0124224da90c9ff43fb8395a9a29cd7e6706dacaca5fc9a"
---

# GraphOS Schema Proposals: a principled development workflow for GraphQL API platforms

As an engineering organization grows, the quality of its internal developer platforms directly influences its ability to execute. It’s why[80% of large software organizations are expected to establish platform engineering teams by 2026](https://www.gartner.com/en/articles/what-is-platform-engineering) and why many existing platform teams are turning to federated GraphQL APIs, known as[supergraphs](https://www.apollographql.com/docs/resources/glossary/#supergraph) , to build self-service API platforms. Client teams leverage these platforms to quickly build features without orchestrating APIs, and backend teams autonomously contribute services from their respective domains via modules called subgraphs.


However, contributions that affect the *shape* of your supergraph (i.e. the schema) still require cross-team coordination to ensure (1) we are designing the schema in a way that continues to benefit developer velocity, (2) that we don’t break client apps, and (3) that our changes don’t conflict with other backend teams’ subgraph schemas. The modularity of the supergraph can make this coordination tricky since each subgraph schema may live in a different codebase, and not every required contributor or approver may have access to each of those codebases.


Apollo aims to make it easy for any number of teams to better collaborate and contribute to a supergraph. Today, we are excited to announce general availability of[GraphOS Schema Proposals](https://www.apollographql.com/docs/graphos/delivery/schema-proposals) – a new centralized change management workflow in GraphOS Studio that streamlines the process of proposing, reviewing, and approving schema changes to a supergraph.


## Centrally manage changes with integrated checks


Developers contributing to a supergraph are typically distributed across multiple teams, making it difficult to take advantage of traditional change management workflows that application teams use across a particular codebase. Although these workflows provide the right tooling, they are implemented at too granular of a level, failing to provide the visibility into the overall impact to the supergraph that platform teams require.


GraphOS Schema Proposals provide a purpose-built change management workflow for the API platform layer, allowing developers and platform engineers across multiple teams to propose, review, and approve schema changes.


### Propose and test schema changes


Proposing a schema change in GraphOS is the first step to making a change to a supergraph. GraphOS Studio now has an integrated schema editor that subgraph developers can use to make changes to existing subgraph schemas or even create new subgraph schemas from scratch.


The schema editor is integrated with all of the other great features of GraphOS Studio, allowing authors to see GraphQL-aware diffing in real time and run linter checks as they work.


Whenever someone saves a revision on a proposal, GraphOS Studio will automatically run[schema checks](https://www.apollographql.com/docs/graphos/delivery/schema-checks/) to validate that the proposed schema changes will compose with other existing subgraphs and won’t break client operations.


### Review and approve schema changes


Once someone on your team has drafted a schema proposal, the author can mark it as` OPEN FOR FEEDBACK` ** to make it visible to other subgraph, platform, and client developers with the proper permissions. If the proposal needs approval from specific reviewers like a team lead or a potentially affected client developer, the author can request a review from those users.


If your organization has a governance policy that requires certain users to approve every schema change for a graph – a platform team lead, for example – graph admins can add those users as *default* reviewers from the graph settings page. Admins can also set a minimum number of reviewers required for a proposal to be` APPROVED` .


Once the minimum number of approvals has been reached and any mandatory default reviewers have approved the proposal, the proposal is marked as` APPROVED` and is ready for publishing.


## Publish approved changes


In addition to editing and reviews, GraphOS Schema Proposals also provides developers with a streamlined workflow for implementing approved changes locally and safeguards to prevent unapproved changes from being published.


Once a developer has created a proposal, subgraph developers can use Apollo’s Rover CLI to quickly[pull associated subgraph schemas](https://www.apollographql.com/docs/graphos/delivery/schema-proposals/implementation#pull-proposed-schemas-with-rover) with rover subgraph fetch. From this point, subgraph developers can then follow the standard steps for developing and delivering changes to their subgraph:


1. Make local changes to subgraph server code, including updating or writing new resolvers
2. Run GraphOS schema checks locally using the Rover CLI (including a new Proposals task to verify that schema changes have a matching approved proposal in GraphOS)
3. Publish the subgraph schema to GraphOS
4. Deploy changes to your subgraph server


*(Note: the Schema Proposals workflow* ***does not*** *automate the above steps, but*[they can be automated in your CI/CD pipelines using the Rover CLI](https://www.apollographql.com/docs/rover/ci-cd/) *)*


### Preventing publishing of unapproved changes


Although the proposals workflow does not automate the deployment process for schema changes, the new Proposal task in the schema checks workflow does provide a safeguard against publishing unapproved changes. Graph Admins can enable or disable the Proposal task in graph settings, and they can also configure the severity of the check to` Error` or` Warn` .


Setting the Proposal task severity to` Error` will cause schema checks to fail if any schema changes do not have a matching approved proposal. This includes schema checks that are run locally as well as when a schema is published to GraphOS, ensuring that unapproved changes are never published to the supergraph.


## Get started


Schema Proposals are generally available today for all[GraphOS Enterprise](https://apollographql.com/pricing) customers. To create a proposal, navigate to your graph in GraphOS Studio, and open the Proposals page from the left navigation. For a full walkthrough of creating and managing proposals, head over to the[Schema Proposals documentation](https://www.apollographql.com/docs/graphos/delivery/schema-proposals/) .


To learn more about GraphOS Schema Proposals and agile API platform development,[join Apollo’s webinar](https://bit.ly/42E9vd9) on February 22, 2024 with Parul Schroff, product manager for GraphOS Schema Proposals! Here are a few things the talk will cover:


- How federated GraphQL enables platform engineering teams to build a self-serve API platform
- How Apollo GraphOS can help your team operationalize delivering a federated graph
- How to leverage GraphOS Schema proposals to streamline federated schema design, review, and implementation


We hope to see you there!


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
