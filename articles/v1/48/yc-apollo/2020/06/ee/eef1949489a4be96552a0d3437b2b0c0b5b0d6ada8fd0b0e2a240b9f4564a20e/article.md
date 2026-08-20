---
schema_version: "1.0.0"
document_id: "eef1949489a4be96552a0d3437b2b0c0b5b0d6ada8fd0b0e2a240b9f4564a20e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/track-schema-changes-with-apollo-schema-reporting"
published_at: "2020-06-17T13:45:41+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:bf9952aa9bb4c186afbfbafe65d0918b9c5dbeab26c14a72f5d1bb116e88f07d"
---

# Register Schema Changes Automatically with Schema Reporting

Apollo Graph Manager’s schema registry powers a ton of features that help you explore, maintain, and monitor your data graph. These features are at their *most* powerful when the schema you register with Apollo stays up to date with the schema you run in production. To help teams keep their schemas synced, we’re excited to announce a new Graph Manager feature called **schema reporting** .


Schema reporting enables Apollo Server (or any other GraphQL server!) to automatically register its schema with[Graph Manager](https://www.apollographql.com/docs/graph-manager/) on startup. Previously, updating your registered schema required running an Apollo CLI command, which is easy to forget if it isn’t part of your continuous delivery pipeline.


By registering your schema automatically, you can:


- Track your *complete* schema history over time
- Share and iterate on development schemas with your team before they land in production
- Set up continuous integration to[check schema changes against past traffic](https://www.apollographql.com/docs/graph-manager/schema-validation/)
- Get up-to-date autocomplete and timing information in your IDE with Apollo’s[VS Code extension](https://www.apollographql.com/docs/devtools/editor-plugins/#gatsby-focus-wrapper)


**Note:** Schema reporting doesn’t currently support graphs that use Apollo Federation. If you’re running a federated graph, check out[Setting up managed federation](https://www.apollographql.com/docs/graph-manager/managed-federation/setup/) .


## Get Started with Apollo Server


**The complete instructions for turning on schema reporting in Apollo Server are in the[Apollo docs](https://www.apollographql.com/docs/graph-manager/schema-registry/#automatic-registration-recommended) .** Here are the high-level steps:


1. [Sign up for Graph Manager](https://engine.apollographql.com/) and create your graph if you haven’t yet.
2. Obtain an API key from Graph Manager.
3. In your server’s environment, set your API key as the value of the` APOLLO_KEY` environment variable.
4. Set the` engine.reportSchema` option to` true` in your` ApolloServer` constructor, like this:


```text
const server = new ApolloServer({
// ...other options...
engine: {
reportSchema: true,
}
});
```


That’s it! Now every time your server starts up, it automatically registers its current schema to your graph. And if you perform a rolling deployment of a new schema version, Graph Manager intelligently waits to register the new schema until every server instance stops reporting the ** previous ** one.


To learn about all of the configuration options for schema reporting, check out[the docs](https://www.apollographql.com/docs/graph-manager/schema-registry/#automatic-registration-recommended) .


**Note:** By default, schema reporting registers to your graph’s default variant, which is named` current` . To register to a different variant (such as` feature-123` or` staging` ), set the` APOLLO_GRAPH_VARIANT` environment variable to the name of the variant you want to register to.


## Other GraphQL Servers


Not using Apollo Server? No problem! Any GraphQL server can implement the schema reporting protocol, which is[publicly documented](https://deploy-preview-892--graph-manager-docs.netlify.app/docs/graph-manager/schema-reporting-protocol/) (as is Apollo Server’s[reference implementation](https://github.com/apollographql/apollo-server/blob/master/packages/apollo-engine-reporting/src/schemaReporter.ts) ). We want it to be as straightforward as possible to add schema reporting to whichever GraphQL server you use, and we’d love tosupport you in any way we can.


The Apollo team is committed to building tools that make it easy for you and your team to develop quickly and safely with GraphQL. If you have feedback on schema reporting or suggestions on how we can improve your team’s workflows,let us know !


Written by


Ran Magen


[Read more by Ran Magen](https://www.apollographql.com/blog/author/ranmagen)
