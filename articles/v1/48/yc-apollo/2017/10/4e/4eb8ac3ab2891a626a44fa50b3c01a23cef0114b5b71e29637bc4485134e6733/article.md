---
schema_version: "1.0.0"
document_id: "4eb8ac3ab2891a626a44fa50b3c01a23cef0114b5b71e29637bc4485134e6733"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/exposing-trace-data-for-your-graphql-server-with-apollo-tracing"
published_at: "2017-10-17T23:40:22+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:6923458a1647a773a5a3bc208e57af6820529eedc3b5202c8c84e34c2c07a4b5"
---

# Exposing trace data for your GraphQL server with Apollo Tracing

Today I’m excited to introduce you to[Apollo Tracing](https://github.com/apollographql/apollo-tracing) , a GraphQL extension for performance monitoring that we’ve been working on for the last few months. Thanks to the community, Apollo Tracing already works with most popular GraphQL server libraries, including[Node](https://github.com/graphql/graphql-js) ,[Ruby](https://github.com/rmosolgo/graphql-ruby) ,[Scala](https://github.com/sangria-graphql/sangria) ,[Java](https://github.com/graphql-java/graphql-java) , and[Elixir](https://github.com/absinthe-graphql/absinthe) , and it enables you to easily get resolver-level performance information as part of a GraphQL response.


Apollo Tracing works by including data in the` extensions` field of the GraphQL response, which[is reserved by the GraphQL spec for extra information that a server wants to return](https://facebook.github.io/graphql/#sec-Response-Format) . That way, you have access to performance traces alongside the data returned by your query. It’s already supported by Apollo Engine, the new version of Optics that[entered preview recently](https://dev-blog.apollodata.com/apollo-engine-and-graphql-error-tracking-e7dd3ce8b99d) , and we’re excited to see what other kinds of integrations people can build on top of this format. For example, you could create a panel in Graph *i* QL that visualizes trace data on a per-response basis.


We’d like to thank[Oleg Ilyenko](https://medium.com/u/93c50e4b5ebe?source=post_page-----97c5dd391385----------------------) for adding support for Apollo Tracing to Sangria,[Reginald Suh](https://github.com/RSuh) from Universe for the Ruby support,[Brad Baker](https://github.com/bbakerman) from Atlassian for Java, and[Sikan He](https://github.com/sikanhe) for Elixir.


## Example response


Here’s an example of a response with trace data included:


```text
{
"data": {
"hero": {
"name": "Luke Skywalker",
"friends": [
{
"name": "Han Solo"
},
{
"name": "Leia Organa"
},
{
"name": "C-3PO"
},
{
"name": "R2-D2"
}
]
}
},
"extensions": {
"tracing": {
"version": 1,
"startTime": "2017-10-16T18:58:06.797Z",
"endTime": "2017-10-16T18:58:06.811Z",
"duration": 13962507,
"execution": {
"resolvers": [
{
"path": [
"hero"
],
"parentType": "Query",
"fieldName": "hero",
"returnType": "Character",
"startOffset": 11089188,
"duration": 181335
},
{
"path": [
"hero",
"name"
],
"parentType": "Human",
"fieldName": "name",
"returnType": "String!",
"startOffset": 12304767,
"duration": 103632
},
{
"path": [
"hero",
"friends"
],
"parentType": "Human",
"fieldName": "friends",
"returnType": "[Character]",
"startOffset": 12588394,
"duration": 1106042
},
{
"path": [
"hero",
"friends",
0,
"name"
],
"parentType": "Human",
"fieldName": "name",
"returnType": "String!",
"startOffset": 13583004,
"duration": 16006
},
...,
{
"path": [
"hero",
"friends",
2,
"name"
],
"parentType": "Droid",
"fieldName": "name",
"returnType": "String!",
"startOffset": 13662093,
"duration": 1928
},
...
]
}
}
}
}
```


As you can see, the trace data includes the start and end time of the request, as well as detailed timings and type information for individual resolvers. Resolver-level data is organized by path, similar to the way error paths are handled in the GraphQL spec. The reason for including the type information is that we didn’t want interpretation of the results to require knowledge of the schema, especially because schemas may change, and because some resolvers depend on runtime type information.


For a more detailed description of the format, see the` <a href="https://github.com/apollographql/apollo-tracing" target="_blank" rel="noreferrer noopener">apollo-tracing</a>`[repo](https://github.com/apollographql/apollo-tracing) .


## Try it today


If you’re using Apollo Server, support for Apollo Tracing is built-in and you can try it out just by setting` tracing` to` true` :


```text
app.use('/graphql', bodyParser.json(), graphqlExpress({
schema,
context: {},
tracing: true,
}));
```


Since options passed to` graphqlExpress` can be a function, you can turn tracing on and off on a per-request basis.


### Supported GraphQL servers


For other servers, you can follow these instructions to enable tracing:


- [Node.js](https://github.com/apollographql/apollo-tracing-js)
- [Ruby](https://github.com/uniiverse/apollo-tracing-ruby)
- [Scala](https://gist.github.com/OlegIlyenko/124b55e58609ad45fcec276f15158d16)
- [Java](http://graphql-java.readthedocs.io/en/v4/instrumentation.html#apollo-tracing-instrumentation)
- [Elixir](https://github.com/sikanhe/apollo-tracing-elixir)


## Supporting more servers and tools


We think this format is broadly useful, and we’d love to work with you to add support for it to your tools of choice. If you’re looking for a first idea, we especially think it would be great to see support for Apollo Tracing in[GraphiQL](https://github.com/graphql/graphiql) and the[Apollo Client developer tools](https://github.com/apollographql/apollo-client-devtools) !


If you’re interested in working on support for other GraphQL servers, or integrations with more tools, please get in touch on the` #apollo-tracing` channel on the[Apollo Slack](http://www.apollodata.com/#slack) .


Written by


Martijn Walraven


[Read more by Martijn Walraven](https://www.apollographql.com/blog/author/martijn)
