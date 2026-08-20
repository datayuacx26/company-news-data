---
schema_version: "1.0.0"
document_id: "f7857eb8e511683ca38e8172ab373d730ce23a04020d480ef2f7fa041c9a61a4"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/launch-with-confidence-introducing-the-apollo-studio-launches-dashboard"
published_at: "2021-10-20T10:09:22+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:45ce241e9e0d1bfce4b8fd6f13a4e6e5c7f3975c9652e53329367a8daf23f515"
---

# Launch with confidence: introducing the Apollo Studio Launches dashboard

We’re excited to introduce the Launches dashboard – a real-time view into Apollo Studio’s schema delivery process to help you observe, monitor, and troubleshoot changes to your federated graph with confidence.


## **The need for greater observability into your launch process**


A few years ago, we released[Apollo Federation](https://www.apollographql.com/docs/federation/) – a distributed GraphQL architecture that allows teams to own and update subgraph schemas independently and compose those schemas into a single supergraph to power a graph router like the Apollo Gateway.


As more teams adopted Federation, we realized they also needed a way to automate deploying changes to a federated graph and to observe what was actually happening as part of that process. To do that, we introduced Managed Federation.


### Automating launches with Managed Federation


With[Managed Federation](https://www.apollographql.com/docs/federation/managed-federation/overview/) , whenever you publish a change to a subgraph schema, Apollo will start a workflow to build your supergraph schema, check it against your historical operations to make sure you aren’t breaking anything, and dispatch it to your gateway instances and any configured webhooks. This automated process — called a *launch* — makes it much easier to deliver changes to federated graphs with confidence.


But for large engineering organizations with many teams that frequently make schema changes, observing the progress of a particular launch and tracking down errors can be a challenge. The new Launches dashboard is designed to solve that problem.


## Introducing the Launches dashboard


Building a federated schema is a multi-step workflow, and we need to observe what is actually happening at each step of the process and be able to debug if something goes wrong. The Launches dashboard provides a record of your past schema launches and gives you a detailed live view of launches that are in progress. With this information, you can respond faster to an issue and launch more confidently.


## **Reviewing past launches**


With the Launches dashboard, you’re able to review and search through all of your past launches and be able to look for any patterns to improve your launch velocity or reliability.


The dashboard includes metadata and details about your schema that make it easier than ever to navigate the history of your graph. If you detect that errors have increased or your clients report a problem, you’ll be able to dive into your launch history to analyze the changes to your graph and determine the root cause.


## **Monitoring the launch of schema changes**


As a graph operator, the Launches dashboard makes it easier to answer the following questions about the state of any given launch, we’ll use the image below for example:


### What is the status of the Build?


In the Launches dashboard, you can dive deeper into the Build process and see details for any errors, the status of individual Build steps, the resulting supergraph schema, and a diff between the newly-built schema and the last published version. If the Build failed your clients won’t see it, but you’ll be able to dig in and find the problem to keep changes flowing without disrupting development on the graph.


### Will this change break any clients?


As part of a launch, you can also see details of the final run of Checks so that you know if the schema going live will introduce any breaking changes that cause errors for clients. This can help pre-empt any problems and enable you to alert the relevant people if further changes need to be made to keep your graph healthy.


### Is the change available?


When you are launching a change to an existing schema, you’ll now be able to see in real-time when the schema is built, when the operation checks pass or fail, and when the schema is made available to running gateways via Uplink.


### Launch IDs


Launches have unique IDs, making each one easily shareable and traceable in your external observability tools.


### Subgraph & Supergraph Changes


You can also see details at the subgraph and supergraph level about the schema changes made as part of each launch. At a glance, the dashboard shows which subgraphs were changed, whether they were added, modified, or removed from your supergraph.


And, for the first time in Apollo Studio, you can also view the entire supergraph schema for a particular launch and see a supergraph diff to dig into subgraph URL changes, directive definitions, and more.


Supergraph schema


Supergraph diff view


## Get Started


The Launches dashboard is part of the Apollo Studio Enterprise plan and is can be accessed in Studio today by clicking on the rocket icon on the left nav. Please[reach out to us](https://www.apollographql.com/contact-sales/) if you are interested in this feature and want to talk about an enterprise plan.


Written by


Joe McCarron


[Read more by Joe McCarron](https://www.apollographql.com/blog/author/joemccarron)
