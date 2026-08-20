---
schema_version: "1.0.0"
document_id: "62220f67acb0e9485c204f3ee2a2b06402cfd1376f3bb260306d75f5e6f91f1c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/schema-validation-with-apollo-engine-4032456425ba"
published_at: "2018-06-21T17:59:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:ed7c6834a686af5c244c59e9d495b4dd05f4cee72a30ad67402e58e394fbd8f9"
---

# Schema Validation with Apollo Engine

GraphQL is a powerful and flexible way to expose data to client applications. It empowers application teams to describe the exact data they need for each feature without having to coordinate with a backend team. That flexiblity dramatically reduces the amount of code teams need to write and accelerates how quickly they can ship applications.


But with that flexibility comes a need for management and control. Teams growing their use GraphQL constantly make changes to their schema by adding data, refactoring types, retiring unused or deprecated fields and more. To be able to do that safely, it’s essential to know which parts of the schema are used by each client, and to understand the impact of a schema change on your application. So how does a fast moving team evolve their schema?


## Introducing the schema validation beta


Starting today teams using Apollo Engine can validate their schema against previous versions and against **real world usage** of their API. This incredible new workflow lets teams of any size manage and validate changes to their API while taking advantage of GraphQL’s flexibility to allow each client to fetch exactly the data it needs.


To learn more about how Engine empowers teams keep reading, but if you want to give it a try right now head over to the[docs](https://www.apollographql.com/docs/engine/schema-history.html) and *publish away* !


## Bug free schema evolution


A lot can change between two versions of a GraphQL schema. Changes to types, fields, directives and more can be dangerous to shipped client applications. So how can a team know what is safe to change and what will trigger a PagerDuty alert at 3 am? Even graphql-js includes a couple undocumented utilities to find[breaking and dangerous changes](https://github.com/graphql/graphql-js/blob/master/src/utilities/findBreakingChanges.js) but these are only a part of the picture needed to evolve your schema.


### Usage powered information


The most important tool to evolving a schema safely is to know how it is being **used** . Changing a return type could have no impact at all if that field was only used in a really old version of the app. Apollo Engine will use historical data on operations used by clients as far back as 90 days to determine if schema changes would break any operations being sent. It does this by replaying all observed operations against a diff of the two schemas and finding usage patterns that would lead to broken apps.


Instead of being held hostage by past decisions, with Apollo Engine teams can have full confidence in how they change their schema.


Replaying fine grained usage data opens the door to a new kind of API governance that doesn’t exist in the REST ecosystem. Using Apollo provides the most accurate and granular information to make smart decisions about changing your services. Static analysis is just not enough to correctly identify critical parts of your schema, but instead the rich data Engine provides, combined with other Apollo tooling, makes GraphQL the perfect way to manage data in your application.


## Track changes over time


Products change over time, and with safely evolving schemas, GraphQL endpoints can seamlessly evolve into the right shape of data the product needs. Much like a git history, having a historical timeline of how these changes can really improve a team’s response rate to issues, fosters communication between backend and frontend teams, and aides in the reviewing of changes. With Apollo Engine, teams can now publish every change of their schema and track the exact commit that caused a change.


## Supercharge your workflow


With all that can change between two versions of a schema, it is really difficult to effectively review pull requests that introduce changes. To add to the complexity, teams that own the GraphQL server aren’t always the ones actually **using** it! What seems like a small backend tweak could break production apps without anyone knowing what happened. That is why Apollo Engine works with the most critical part of teams workflow, the GitHub pull request.


Last month, GitHub introduced their new[checks platform](https://blog.github.com/2018-05-07-introducing-checks-api/) to improve the code review process for teams. I am so excited that we are launching full checks API integration today for schema validation. Here is how it works:


1. Configure the[Apollo Engine GitHub app](https://github.com/apps/apollo-engine)
2. Run` apollo schema:check` on every commit
3. View the difference in GitHub!


Engine will update the status of a pull request to prevent breaking changes from going into production and comment back detailed information about what changed. **Think of it like a CI for GraphQL!**


You can see this in action on our example project[GitHunt](https://github.com/apollographql/GitHunt-API/pull/280/checks?check_run_id=4703076) .


## Get started today


As long as I’ve been working on GraphQL for production applications, I’ve wanted a safe way to change my schema. Schema design can be scary, and at Apollo we are committed to making it easier through[in-depth guides](https://www.apollographql.com/docs/guides/schema-design.html) ,[workshops and support](https://www.apollographql.com/support) , and powerful tools like Engine. To try out this feature today, head over to our[docs](https://www.apollographql.com/docs/engine/schema-history.html) and publish away!


I hope you’ll also[join us at the 3rd annual GraphQL Summit on Nov 7–8 in San Francisco](https://summit.graphql.com/) . With over 800 attendees expected, it’s the largest GraphQL developer event in the world. Super early bird tickets are selling fast, so[register today](https://www.eventbrite.com/e/graphql-summit-2018-tickets-46601841362) to reserve your spot!


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
