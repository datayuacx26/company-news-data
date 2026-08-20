---
schema_version: "1.0.0"
document_id: "9d9f7584fe1a651a50500a782becfb33562af6d46a052d8dd8e502fb7fa566e1"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-schema-change-notifications-with-apollo-graph-manager-b0f2ef13ce9d"
published_at: "2020-04-09T18:55:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:deb10115d0d1c941554d5369cf657f1c0a1e3867901c90e8cc6a65c3b98540f7"
---

# Introducing Schema Change Notifications with Apollo Graph Manager

One of the biggest benefits of a GraphQL-powered data layer is the ability to maintain a single schema that continuously evolves to reflect your collection of products and features.


As your schema evolves, it’s essential to keep your teams informed whenever you introduce new capabilities or deprecate existing ones. To help with that, **we’re delighted to announce an exciting new feature in Apollo Graph Manager: schema change notifications!** 🚀


With schema change notifications, you can configure[Graph Manager](https://www.apollographql.com/docs/graph-manager/) to notify you via Slack **any time an update is made to your registered schema** .


## Setting it up


Setting up schema change notifications takes just a couple of quick steps:


### 1. Register your schema


If you aren’t already registering your schema with Graph Manager, check out the[schema registry docs](https://www.apollographql.com/docs/graph-manager/schema-registry/) . And if you’re *brand new* to Graph Manager,[get started here!](https://www.apollographql.com/docs/graph-manager/getting-started/)


### 2. Enable notifications


On your graph’s **Integrations** page in Graph Manager, click **Configure** :


The Integrations page in Apollo Graph Manager


From here, you can specify a Slack webhook URL, along with the name of the channel you want to send notifications to:


Specifying a channel name and webhook URL


Next, select the[variant](https://www.apollographql.com/docs/graph-manager/schema-registry/#managing-environments-with-variants) of your graph you want to receive notifications for and click **Done** :


Selecting a graph variant


That’s it! You’re all set and ready to go. The next time anyone pushes a schema change to the specified graph variant, you’ll get a Slack notification like the one at the top of this post.


## Do even more with Slack


Schema change notifications aren’t the only feature of Graph Manager’s Slack integration! To learn more about daily metrics reports and threshold-based alerting,[check out the docs](https://www.apollographql.com/docs/graph-manager/slack-integration/) .


---


## Thank you!


We appreciate your feedback and would ❤️ to hear from you[on Twitter](https://twitter.com/apollographql) ! Let us know how you’re liking schema change notifications, and don’t hesitate to give us a shout you if there are any other features you’d like to see in[Graph Manager](https://www.apollographql.com/docs/graph-manager/?referrer=blog) .


Written by


Caydie Tran


[Read more by Caydie Tran](https://www.apollographql.com/blog/author/caydie)
