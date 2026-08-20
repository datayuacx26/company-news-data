---
schema_version: "1.0.0"
document_id: "4529deee8330edd35c46abe3f45ef77ee0183e7ca6fc86fb9505ab443bf27f7f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/the-5-most-useful-features-of-apollo-studio-explorer-that-you-didnt-know-existed"
published_at: "2021-08-12T11:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:ec894df39f75519fcb265322dbebdcd73e40b4356ac2acfd44cc7b5799049424"
---

# The 5 most useful features of Apollo Studio Explorer that you didn’t know existed

Apollo Studio Explorer is the most powerful web IDE for creating, running, and managing GraphQL operations. Since launching it over a year ago, we’ve added many new features to Explorer to help you navigate your schema and build queries more intelligently than ever before. To help you get the maximum value out of Explorer, we polled a number of developers from the GraphQL community and compiled a list of the top 5 most useful features you may have missed.


Let’s take a look at each.


## 1. Multiple operations


Did you know that the operations editor can manage multiple operations individually? As you work, the editor shifts focus to whichever operation you click into and updates the run button accordingly. Each operation also has its own context menu (“ **…** “) offering various tools specifically for that operation. This makes it easier than ever to manage and test all of your operations independently in a single tab!


## 2. Table layout for response data


You can view an operation’s response as JSON or as a table. Table layout is especially useful when your response includes an array, or when you want to share a query’s results with someone who isn’t familiar with JSON. When looking at arrays of data in table mode, you can click the header of any column of data to sort your array by that column’s values.


## 3. Editor hints


Under Explorer settings, you can toggle on/off Editor hints to view metadata for each of your operation’s fields, right aligned in the editor. You can toggle any of the following three hints to be displayed:


- **Response hints:** When toggled on while building your query, the Explorer runs partial queries under the hood and shows their results in-line. This is helpful when you want to get a sense of the data you’ll get back in your full operation response. It can also help you retrieve a quick answer to a query without needing to click the Run button.
- **Subgraph source:** For a federated graph, you can see which subgraph each field in the operation is sourced from.
- **Field latency approximations:** The Explorer shows you the 95th-percentile (default) response times for the fields in your query to help you get a sense of how “expensive” your query is and what the bottlenecks in response time will be. You can change which percentile you want to see hints from at any time in the Explorer settings.


## 4. Copy link to operation


One of our favorite features! This enables you to share with each other the link to the exact operation. Click on the context menu (“…”) at the top of the operation to copy the link that can be used for sharing. Paste the link on a web browser and voila! Explorer with the exact operation is ready to be executed.


## 5. Inline/Extract variables


While editing your operations, you can toggle between inline or extracted notation for variables. This is useful when you want to switch notations to copy and paste something, or when you’re drafting a query in the editor and want to move it to your code.


## Get started with Explorer


We can’t wait to see what you’ll build with all of the amazing features that Explorer has to offer! If you’re already an Apollo user, you can access all of these features right now by opening Explorer in[Apollo Studio](https://studio.apollographql.com/login) . Or, if you’re new to Apollo and not quite ready to create a Studio account, you can start using much of what Explorer has to offer in[Apollo Sandbox](https://studio.apollographql.com/sandbox/explorer) .


Written by


Parul Schroff


[Read more by Parul Schroff](https://www.apollographql.com/blog/author/parul)
