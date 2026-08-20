---
schema_version: "1.0.0"
document_id: "3eb7c86e10a67e319012a6dd181b74d61789d642a6e4368bcfbfd86fe6c4fa69"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/integrate-with-graphos-using-the-new-platform-api"
published_at: "2022-10-05T15:39:47+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:56018e500e0fc50ce89c141a7e47ffbe5252c8688f89f4ebd3f3dad92fe605f2"
---

# Integrate with GraphOS using the new Platform API

As of today, you can now use our new public API to integrate with the Apollo GraphOS platform.


We built this API to enable those using GraphOS to easily automate operations in their own systems and CI workflows. In fact, this is the same API that the[Rover CLI](https://www.apollographql.com/docs/rover/) uses to run schema checks and publish schemas to Apollo Studio.


Under the hood, the GraphOS platform API uses schema[contracts](https://www.apollographql.com/blog/announcement/platform/contracts-are-now-generally-available/) to make our private platform API publicly available. By creating a contract, we can filter out the necessary subset of our private platform API schema, all the while keeping the public schema in sync with the source.


See the[API’s readme](https://studio.apollographql.com/public/apollo-platform/home?variant=main) to get started, and check out the operation collections in Explorer to see example GraphQL queries and mutations you can use for your own graphs.


## Take the API for a spin


So, let’s try it out! We’ve embedded the API’s Explorer from Studio below. (Yes, you can[embed the Explorer](https://www.apollographql.com/docs/studio/explorer/embed-explorer/) for any of your graphs managed in Studio!)


Here are some examples of what you can do with the GraphOS platform API:


- Retrieve a list of your GraphQL supergraphs
- Fetch a graph for a specific environment (such as staging)
- Publish your supergraph’s schema
- Check your supergraph’s schema for errors


Note: Some of these operations, like publishing a new graph, require that you set your own Studio API key. See the[readme](https://studio.apollographql.com/public/apollo-platform/home?variant=main) for instructions. Check out our operations collections and try running some queries, read the API’s inline documentation, or see the[schema reference](https://studio.apollographql.com/public/apollo-platform/schema/reference?variant=main) to view all the API’s types and fields:


## When to use the API and when to use Rover


In most cases, you’ll want to use the Apollo Rover CLI to manage and publish your GraphQL supergraph schema. As developers iterate on their subgraphs, we recommend regularly running schema checks to ensure that their updated graphs properly compose into a supergraph. If you use continuous integration (CI), we recommend integrating Rover into your pipeline.[Here’s how to get started](https://www.apollographql.com/docs/rover/ci-cd) .


If you need to do more complex operations, then you might want to use the GraphOS platform API. For instance, you might want your CI pipeline to create a new Studio variant for each test run, then delete that variant when the run finishes. To accomplish this, you can do the following using the Platform API:


- Create a test variant by[Publishing your schema(s) to it](https://studio.apollographql.com/public/apollo-platform/home?variant=main#publishing-schemas) .
- Perform any desired tests (such as[schema checks](https://studio.apollographql.com/public/apollo-platform/home?variant=main#running-schema-checks) ) on the test variant.
- Delete the variant with the corresponding[destructive action](https://studio.apollographql.com/public/apollo-platform/home?variant=main#destructive-actions) .


In the near future, we will post about other ways to integrate with the GraphOS platform, such as using the platform API with your governance workflows for your supergraphs. We also look forward to learning how you will use the API!


## Looking ahead


This is just the start for the GraphOS platform API! We are continuing to add new features to both GraphOS and the platform API to make it easier for you to get your schema changes safely to production, to keep your teams up-to-date, and to ensure that clients consuming your graph never experience breaking changes.


To stay up-to-date on what’s coming next with the GraphOS platform API,[star and watch the Apollo Studio Community GitHub project](https://github.com/apollographql/apollo-studio-community) . We will be sharing more about the GraphOS platform API roadmap there and here in the Apollo blog.


You can help us by letting us know what you’d like added to the API by creating a GitHub issue or by giving us feedback in the[Apollo Community](https://community.apollographql.com/) .


## Get started with GraphOS


Still getting started with a single GraphQL server or new to GraphQL? No problem. We’ve made it easier than ever to leverage all of the analytics and automation of supergraphs in Studio.[Create your first graph](https://apollographql.com/tutorials?ref=www) and make your first graph query in under an hour.


Written by


Jenn Allen


[Read more by Jenn Allen](https://www.apollographql.com/blog/author/jenn)
