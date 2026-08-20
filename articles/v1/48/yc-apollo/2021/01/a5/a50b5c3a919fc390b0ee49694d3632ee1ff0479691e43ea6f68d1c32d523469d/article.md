---
schema_version: "1.0.0"
document_id: "a50b5c3a919fc390b0ee49694d3632ee1ff0479691e43ea6f68d1c32d523469d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/webhooks"
published_at: "2021-01-27T08:27:08+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:30b8191f2d792ce410923915c5721a179d728b95d6072ae07513e715cb325aba"
---

# Apollo Enterprise: Schema Change Webhooks

tl;dr Apollo Studio Enterprise announces support for Schema Change Webhooks


As GraphQL developers, all of our data is communicated through the GraphQL schema. With the release of schema change Slack notifications in 2020, we made it easier than ever to keep everyone working around your graph up-to-date with schema changes through Slack. Expanding on the functionality to connect your graph’s schema to your workspace, we are excited to announce the addition of **schema change webhooks** to Apollo Studio Enterprise. With webhooks, you and your team can build whatever automation is relevant to your workflow, whether that be automating pull requests to client & server codebases, running linting tests, generating email notifications, or creating a personalized change-log. The possibilities are yours to enumerate. Setting up schema change webhooks will require:


- An Apollo Enterprise subscription
- Configuration in Apollo Studio
- An exposed endpoint to handle the webhook payload


Every time your graph variant’s published schema changes, your configured webhook listener will receive a JSON payload like so:


```text
{
"eventType": "SCHEMA_PUBLISH",
"eventID": "6190b556-457e-4669-b933-8b66aa9b8764",
"changes": [
{
"description": "field `Amazon.referrer`: type `String!` changed to `String`"
}
],
"graphID": "acephei",
"schemaURL": "https://graphql.api.apollographql.com/schema-link?token=6fb35434a55b966f81c0e07a8f93327029d",
"schemaURLExpiresAt": "2021-01-26T20:05:28.856Z",
"timestamp": "2021-01-25T20:05:28.081Z",
"variantID": "acephei@prod"
}
```


Configuring a new webhook notification is a few clicks in Apollo Studio. Navigate to the notifications tab in your graph’s Settings page and click` Add Notification` to get started. For a walk-through, take a look at the dedicated[Studio Notifications docs](https://www.apollographql.com/docs/studio/notification-setup/) .


To handle a webhook notification, you’ll expose an endpoint that can consume the JSON payload. We have an example Node.JS server for you to run to handle this event here:[https://github.com/jsegaran/apollo-schema-change-webhook](https://github.com/jsegaran/apollo-schema-change-webhook) . Alternatively, it’s simple to get up and running with a custom Zapier integration or whatever your favorite workflow automation tool may be. To learn more about schema change webhooks and the other functionality that comes with Apollo Enterprise,[reach out to Apollo](https://www.apollographql.com/contact-sales/) !


Written by


Josh Segaran


[Read more by Josh Segaran](https://www.apollographql.com/blog/author/josh-segaran)
