---
schema_version: "1.0.0"
document_id: "9c1291c9b7cdbe69faa6906e37010c9dad4933966991fcd8fa2b7a782c365b66"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/daily-slack-reports-of-your-graphql-query-performance-and-errors-1749311f33e1"
published_at: "2018-03-29T21:07:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:af3934cd490a916b4a4c1cc47af6b2a5555d36ce73797c740d043035d280a702"
---

# Daily Slack reports of your GraphQL query performance and errors

The best thing about GraphQL as an API technology is that it’s focused on the needs of product developers. You simply describe the data you want for a particular part of your UI with a query, and the GraphQL server takes care of assembling it for you. And because all modern GraphQL clients follow the practice of sending[static, predictable queries](https://dev-blog.apollodata.com/5-benefits-of-static-graphql-queries-b7fa90b0b69a) , you can easily associate the queries that hit your API with the parts of the UI that are using those results.


For example, if you see a query called` ServicesListQuery` hit your API, you can tell that this data is going to be used for the` ServicesList` component in the frontend. This is a huge benefit of GraphQL over a more “generic” API, where a` /services` endpoint might be used by dozens of components in different ways.


To take advantage of this, you need a monitoring and reporting tool that natively understands GraphQL and can show you details on the level of individual queries and resolvers. That’s where[Apollo Engine](https://www.apollographql.com/engine/) comes in.


### Announcing: Daily Slack reports and API overview


Starting today, Engine can send you a bird’s-eye view of your GraphQL API, with a breakdown of your most important queries, both in the dashboard and in a compact daily Slack report.


Here’s what it looks like:


### Getting started with Engine


If you haven’t set up Engine in your GraphQL server yet, here’s how:


1. [Add Engine to your Node.js API in about 5 minutes](https://www.apollographql.com/docs/engine/setup-node.html)
2. [Use Engine with any GraphQL API with Docker](https://www.apollographql.com/docs/engine/setup-standalone.html)


Now, let’s dive in to how these new features work.


## New: Service overviews and reports in Engine


Engine has always enabled you to see how your GraphQL API is working, down to the level of individual queries, mutations, and resolvers. Starting today, Engine will also provide you reports that show you what parts of your API need your attention.


When you log into your Engine dashboard today, you’ll immediately see an overview of your most interesting GraphQL operations:


Visit your[Engine dashboard](https://engine.apollographql.com/) to see this new view!


And, so that you don’t need to log into Engine every day to keep an eye on what’s going on, you can get the same information delivered in a daily report to the Slack channel of your choice.


### Using the charts


We’ve been testing this feature with some early-access customers to make sure that the data presented is the most relevant information you would want to know about your API. Here’s how you can use it to identify issues:


1. **Request rate:** The number of operations per minute, and a list of the most popular operations. A dip in this metric might mean some frontends aren’t able to reach your server!
2. **p95 service time:** The time under which 95% of operations are executing, and a great representation of the majority user experience. Consistent slowness could indicate an overloaded API, and the operation details will show the worst offenders.
3. **Error percentage:** Spikes in error percentages may indicate malfunctions in your backend, and knowing which operations are most error-prone will help find precisely where to look.


### Drilling down into operations


Once you identify something that needs extra attention, you can click on any of the listed operations to get a more detailed view. This will help you find extra information that you can use to address the issue, taking you directly to a detailed trace of the operation timing or a specific error message:


The detailed trace view that tells you timing for each field in your operation.


## Setting up Slack reports


Turning on reports for your GraphQL API is really simple. First, click on the “Daily Reports” button in the top right of the Engine UI:


You’ll see a form that will allow you to set up a new Slack channel:


Follow the directions in the popup to add a Webhook URL to one of your channels, and then paste that URL into Engine. You’ll get a confirmation message in the channel you picked, like so:


Now you’re good to go! In a short while, you’ll see daily reports from Engine show up in your channel.


## GraphQL: Optimized for developer experience


There are all kinds of API monitoring and reporting tools out there, but this type of report demonstrates why having something built around GraphQL makes a big difference. When you see GraphQL queries from your UI show up in the daily report, you not only get a sense of how your API is operating, but you can directly connect that to the GraphQL queries you’ve attached to your components with Apollo Client.


It’s that kind of full-stack integration where GraphQL really shines, and I can’t wait to see what else will be possible!


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
