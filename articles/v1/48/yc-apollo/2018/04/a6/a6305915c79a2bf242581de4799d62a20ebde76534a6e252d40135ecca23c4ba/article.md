---
schema_version: "1.0.0"
document_id: "a6305915c79a2bf242581de4799d62a20ebde76534a6e252d40135ecca23c4ba"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/the-next-generation-of-schema-stitching-2716b3b259c0"
published_at: "2018-04-26T19:54:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:00da7ab1656dc7d49f2abbee65651113d30475a0afafd3831efcfb9f18a08140"
---

# The next generation of schema stitching

**Note:** This post discusses Schema Stitching, an approach we explored that eventually led to the development of[Apollo Federation](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/federation) . We recommend migrating to Federation for a more robust and scalable architecture. To get started, follow[this migration guide](https://www.apollographql.com/docs/graphos/schema-design/guides/migrating-from-stitching) and read[this blog post](https://www.apollographql.com/blog/announcement/expedia-improved-performance-by-moving-from-schema-stitching-to-apollo-federation/) for a real-world example of an organization benefiting from this migration.


A few months ago, we[launched](https://dev-blog.apollodata.com/graphql-tools-2-0-with-schema-stitching-8944064904a5) the first version of a new concept called “schema stitching” in graphql-tools 2.0. Schema stitching is a way to combine multiple GraphQL APIs together, or use an existing API in the construction of a new one. Since the launch, we’ve been excited to see this concept gain a ton of traction in the GraphQL community and we’re more confident than ever that this is going to be a promising long-term direction.


Here are some of best use cases we’ve seen:


1. Developing your GraphQL API as a set of microservices, and then stitching them together to create one complete API
2. Using[Prisma](https://www.prisma.io/) , a GraphQL database layer that lets you work with MySQL or Postgres, as part of your GraphQL schema
3. Using a public or internal existing API as part of a new GraphQL schema you’re building


Since the original launch of schema stitching last year, we’ve learned a ton about these different use cases, so we set out to improve the API to better support what we’ve seen.


**Today, we’re excited to release graphql-tools 3.0, a major improvement to how schema stitching works.**


It also comes with some new abstractions to support more extensibility in the future. It’s completely **backwards compatible** , meaning your existing code will still work as-is, but we’ve updated the recommended approach so we felt a major version bump was appropriate.


This new feature and the blog post you’re reading now was developed by[Mikhail Novikov](https://medium.com/u/51db47a6a4ef?source=post_page-----2716b3b259c0----------------------) and[Ben Newman](https://medium.com/u/540bbf1a0d2b?source=post_page-----2716b3b259c0----------------------) . Mikhail is currently available for remote GraphQL contract work, so contact him atfreiksenet@reindex.io if you need some GraphQL help.


### Making it easier to modify schemas and handle conflicts


After gathering a lot of feedback from beta users, we think we have found an improved API that is simpler and more flexible:


1. It’s easier to modify schemas before or after you stitch them together
2. It’s easier to identify and avoid schema conflicts by namespacing


This is enabled by a new concept called **schema transforms.** These are functions you can apply to a schema to do things like rename or filter types or fields.


To get started, read about[schema stitching](https://www.apollographql.com/docs/graphql-tools/schema-stitching.html) and[schema transforms](https://www.apollographql.com/docs/graphql-tools/schema-transforms.html) in the new docs. To get the full details, check out the[change log](https://github.com/apollographql/graphql-tools/blob/master/CHANGELOG.md#v300) .


To learn about the new concepts, read on!


---


## Schema transforms


When you start using GraphQL, you usually have exactly one schema. But as usage progresses at your company, you often end up with multiple. Perhaps that’s because different teams developed a GraphQL API in parallel, or maybe you’re using a GraphQL database layer like Prisma which comes with its own schema, or you’re trying to build a new layer over an existing GraphQL API.


Once you have multiple schemas, you can start thinking of them as the building blocks of data in your organization. If you’re going to use them as building blocks, sometimes you’ll need to make modifications to a schema for it to fit your needs.


Until now, it has been very difficult to make a change to a schema and still be able to run arbitrary queries against it. With the schema transformation functionality in graphql-tools 3.0, that’s now easier than ever before.


### Example: Renaming a type


Let’s look at an example to see why it’s difficult to rename a type in a GraphQL schema you don’t control. In this case, let’s say we’re trying to use the GitHub API schema as part of our app’s schema, but we don’t want the` User` types to conflict. That means we might want to rename GitHub’s` User` type to` GitHubUser` . If we still want to be able to send queries to this part of our schema, we need to handle a lot of different cases:


1. Results that have` __typename` need to replace any occurrence of` User` with` GitHubUser`
2. Queries that have fragments need to replace` on GitHubUser` with` on User` before sending the query to GitHub
3. When the schema is introspected, it needs to return` GitHubUser` for the name of the type everywhere, including when it’s a member of a union or interface


So it’s not as easy as just taking the GitHub schema object and renaming the type — we need to correctly transform the queries and results as well. Transforming the query and result, in addition to the schema itself, is what a schema transform does in graphql-tools 3.0.


Here’s how we could rename a type in a remote GitHub schema using the new` transformSchema` method:


```text
import {
makeRemoteExecutableSchema,
transformSchema,
RenameTypes,
} from 'graphql-tools';// Make a schema that points to the GitHub API
const gitHubSchema = makeRemoteExecutableSchema(...);const transformedSchema = transformSchema(gitHubSchema, [
new RenameTypes((name) => {
if (name === 'User') {
return 'GitHubUser';
}    return name;
}
]);const result = graphql(transformedSchema, `
query {
viewer {
__typename
... on GitHubUser {
username
}
}
}
`);
```


You can see that in the query we use the *new* name for the type,` GitHubUser` , but the query that gets sent to the GitHub API will refer to the original` User` type. Also, while the GitHub API will return their` __typename` , response types named` User` will also be replaced correctly with` GitHubUser` .


Basically, using` transformSchema` here enables us to reshape a GraphQL schema at will, while making sure all queries work as expected. This is critical so that we can do namespacing, avoid schema conflicts when stitching, and limit access to only the fields we want.


## Built-in transforms


The graphql-tools 3.0 release comes with a set of transforms pre-implemented for you, based on the set of feature requests we have received for the previous version of schema stitching. We think these will make it easy to address many cases of schema transformation, filtering, and namespacing you encounter.


### Filtering


These allow you to select just part of a schema.


- ` FilterRootFields` : Filter the entry points into a schema, so clients don’t have access to all of the` Query` /` Mutation` fields of the underlying schema.
- ` FilterTypes` : Allows you to remove some types from the schema by name.


### Renaming


These allow you to rename parts of the schema.


- ` RenameRootFields` : Rename fields on the` Query` and` Mutation` types.
- ` RenameTypes` : Rename types (and any references to those types) throughout the schema.


## Implementing your own transforms


While the above transforms should be adequate to get you started, it’s also pretty easy to implement your own. You just need to implement one or more methods of the following interface:


```text
type Transform = {
transformSchema?: (schema: GraphQLSchema) => GraphQLSchema;
transformRequest?: (request: Request) => Request;
transformResult?: (result: Result) => Result;                       };
```


This describes the three things that a schema transform needs to handle, like we mentioned above:


1. Transforming the` GraphQLSchema` object itself
2. Transforming incoming GraphQL requests — queries and their variables
3. Transforming outgoing result objects


Not all transforms need to handle each method — for example, some transformations might just need to process the schema and requests, but not results. For inspiration, take a look at the existing transforms, for example` <a href="https://github.com/apollographql/graphql-tools/blob/79868c597145a04e896677720769f15ee2b386b9/src/transforms/RenameTypes.ts" target="_blank" rel="noreferrer noopener">RenameTypes</a>` .


## Combining schema transforms and stitching


The main use case for which we designed the current version of schema transforms is to avoid naming collisions or exposing unnecessary fields during stitching. For that use case, you simply remove the fields you don’t want to expose, or rename types before you stitch the schema. But what happens when you don’t want to have a certain root field in the result schema, but you still need it for stitching?


Well, it turns out that you can *delegate* to the original, untransformed schema by keeping both objects around:


```text
import {
transformSchema,
mergeSchemas,
FilterRootFields,
RenameTypes,
RenameRootFields,
} from 'graphql-tools';


// Transform the schema to namespace it and only keep one root field
const transformedChirpSchema = transformSchema(chirpSchema, [
new FilterRootFields((operation: string, rootField: string) =>
['Query.chirpById'].includes(`${operation}.${rootField}`),
),
new RenameTypes((name: string) => `Chirp_${name}`),
new RenameRootFields((name: string) => `Chirp_${name}`),
]);


const schema = mergeSchemas({
schemas: [
// Use the transformed schema for merging, so that we only
// get the fields we want
transformedChirpSchema,
authorSchema,
linkTypeDefs,
],
resolvers: {
User: {
chirps: {
fragment: `fragment UserFragment on User { id }`,
resolve(parent, args, context, info) {
const authorId = parent.id;
return info.mergeInfo.delegateToSchema({
// Use the original unfilitered schema for delegation
// since we need the fields we filtered out for merging
schema: chirpSchema,
operation: 'query',
// This field is not accessible in the transformed schema
fieldName: 'chirpsByAuthorId',
args: {
authorId,
},
context,
info,


// Apply transforms here for type renaming
transforms: transformedChirpSchema.transforms,
});
},
},
},
},
},
```


The key function here is` info.mergeInfo.delegateToSchema` , which makes it easy to implement resolvers in terms of any existing schema.


This new function for delegation now also takes a set of transforms as a parameter, allowing you to pick and choose the transforms you want for a specific delegation. We hope that people can build abstractions around this to take advantage of the ultimate flexibility offered by this new approach.


---


## Next steps


With graphql-tools 3.0, we wanted to bring you this new, simplified way to handle schema conflicts as soon as possible. But we think the potential for schema transforms is going to be huge as more and more people see schemas as the building blocks of their organization’s data. If you’re interested in implementing some new transforms for your use cases, let us know!


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
