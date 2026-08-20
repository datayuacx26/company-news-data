---
schema_version: "1.0.0"
document_id: "f7d09b6bffdea4a5fd4d5669007177106acde408356a394870a0079cdb724ca6"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-managed-federation-265c9f0bc88e"
published_at: "2019-07-16T22:45:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:37e2525dbea305ad50f93dad9d6f0e704c6e8abe47a3f948ea34f3adc32a9986"
---

# Announcing managed federation

Just a few weeks ago, we released[Apollo Federation](https://www.apollographql.com/docs/apollo-server/federation/introduction/) — our new open-source technology for composing multiple GraphQL services into a unified data graph. As[James Baxley III](https://medium.com/u/fd3d3dea3cd3?source=post_page-----265c9f0bc88e----------------------) explained in his[blog post](https://blog.apollographql.com/apollo-federation-f260cf525d21) , federation is the essential architecture that lets organizations scale their GraphQL API from a single team to a shared asset serving the whole company. It enables multiple teams to collaborate on a shared graph without any fragile stitching code or heavyweight development processes.


Apollo Federation is implemented as a set of capabilities that lets GraphQL servers reference and extend each other’s individual schemas, plus a gateway that answers incoming queries by executing them in parts to the appropriate GraphQL servers. But like any shared infrastructure, running a gateway and a set of distributed services raise an important need for tools that help teams coordinate their efforts and collaborate productively.


Today, we are happy to announce a major update to Apollo’s commercial platform:[managed federation](https://www.apollographql.com/docs/platform/federation/) . Our update adds support for seamlessly building, managing, and querying federated graphs just like you can with a monolithic schema. Read on to learn more, including how you can[try it today](https://engine.apollographql.com/) .


## A need for data graph infrastructure


We’ve spoken to hundreds of teams who are scaling GraphQL within their organization about their infrastructure needs for federation. These requirements came up time and time again:


- **Easy updates to the graph:** Changes need to be easy for every developer on the team to make. They can’t require a manual update or restart of the gateway, which was necessary with[schema stitching](https://github.com/apollographql/graphql-tools/issues/791) .
- **An even greater need for agility and safety:** Teams with federated graphs tend to have more developers working on the graph and more clients querying it. That puts a higher premium on workflows like[schema change validation](https://www.apollographql.com/docs/platform/schema-validation/) to help teams move quickly without breaking their applications.
- **Clean integration with cloud-native infrastructure:** Teams implementing service meshes and using new tools like Envoy, Istio, and Spinnaker may have multiple versions of each GraphQL service running in parallel with sophisticated deployment and rollback workflows. In larger organizations, different teams may even use completely different approaches. This means we need a tight integration between service deploy and graph configuration, which drives home the need for a[central place to track the schema](https://principledgraphql.com/integrity#3-track-the-schema-in-a-registry) and its underlying GraphQL services.
- **A smooth adoption path:** Teams need an easy path from a monolithic schema to a federated graph without any disruptive changes to their clients, resolvers, and development workflows. Popular tools — like GraphiQL and[Apollo’s VS Code extension](https://marketplace.visualstudio.com/items?itemName=apollographql.vscode-apollo) — should work the same no matter the graph’s underlying implementation.


Our work is driven by the needs of app developers, so we decided to create the turnkey solution that developers have been asking for in order to successfully roll out GraphQL across multiple teams. With managed federation, teams can collaborate seamlessly on one shared graph without the complexity of building specialized tooling for a distributed architecture.


## Managing a federated graph


Teams need federation-aware tooling in order to help them coordinate updating the graph whenever an underlying service changes. Apollo’s **Graph Manager** is a complete solution to this problem that includes federated service checks, managed service deployments, and federation-aware analytics out of the box.


A federated graph is composed from multiple services, so the Graph Manager includes a service list to track a graph’s implementing services and their capabilities. Whenever that service list changes, the Graph Manager recomposes the service definitions into an updated schema and pushes a new gateway configuration.


### Key features


- [Analytics and diagnostics for a federated graph](https://www.apollographql.com/docs/platform/federation/#metrics-and-observability) **:** When developing a distributed system, how do we know what’s going on underneath the hood? The Graph Manager gives teams key insights about how to improve, maintain, and support services in a federated environment. Developers can drill down into detailed information about what services are live in production and how queries interact across them.
- [Federated service checks](https://www.apollographql.com/docs/platform/federation/#validating-changes-to-the-graph) **:** Just like with a monolithic schema, teams building a federated graph want to protect against making breaking changes to their part or another team’s part of the graph. With managed federation, service checks wire into your existing CI/CD pipeline so that every team can independently and safely update their piece of the graph.
- [Managed service deployments](https://www.apollographql.com/docs/platform/federation/#connecting-apollo-server-to-the-graph-manager) **:** When deploying a federated graph, how do we manage rolling out changes to downstream services? With managed deploys, teams can easily coordinate changes to a distributed graph without having to align across multiple teams, build new CI/CD processes, or manually redeploy the GraphQL server to fetch the latest changes.


The diagram below shows managed deployments in action. Once a new Reviews service is deployed, it registers its new capabilities with the Apollo Graph Manager. Then, the manager recomposes the graph and pushes the new configuration to the gateway.


Managed deployments with Apollo


## Introducing the Apollo Graph Manager 🚀


With the new addition of managed federation, we also wanted to take the opportunity to rename our commercial product in order to more accurately describe what it really is to our users. Previously called Apollo Engine, the commercial part of the Apollo platform is now the **Apollo Graph Manager** — a full, turnkey solution for teams operating a data graph.


The Graph Manager contains several key pieces of cloud infrastructure to help teams seamlessly collaborate when building, managing, and querying one unified graph. It has a schema registry for tracking a graph’s history, including all changes made to its underlying services. It contains a client registry and an operation registry, so teams know exactly how their graph is being consumed down to the field level. It also comes with a dashboard for visualizing analytics, viewing history, and configuring features such as alerts.


The Graph Manager is included in all three plans of the Apollo platform:


- **Community Edition** is our free tier, with the essentials you need to build and run a data graph. It includes managed service deployments, Apollo’s schema registry, and basic data graph analytics.
- **Team Edition** is for teams collaborating on a production graph. It adds tracing, query plan diagnostics, performance metrics, configurable alerts, and schema change validation — everything you need to run confidently in production, and all built to work equally well with federated and monolithic graphs.
- **Enterprise Edition** is for companies making a strategic investment in a shared graph that spans the organization. It adds single-sign-on, 24×7 production support, and access to our solution architects and core developers who can help guide your data graph efforts from start to finish.


### Try managed federation today!


If managed federation sounds like it might fit your team’s needs, we encourage you to[sign up for a free trial](https://engine.apollographql.com/) **.** Please[reach out to us](https://www.apollographql.com/contact-sales) if you have questions.


To dive into the technical details, check out the[new docs page on managed federation](https://www.apollographql.com/docs/platform/federation) . Since this page assumes you already have a federated graph running, you should visit the docs page on[implementing a federated graph](https://www.apollographql.com/docs/apollo-server/federation/introduction/) first if you’re new to federation.


We look forward to hearing your feedback on managed federation!


Written by


Matt DeBergalis


[Read more by Matt DeBergalis](https://www.apollographql.com/blog/author/matt)
