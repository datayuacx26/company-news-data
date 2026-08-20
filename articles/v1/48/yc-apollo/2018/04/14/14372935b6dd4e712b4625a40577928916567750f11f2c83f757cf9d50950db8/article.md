---
schema_version: "1.0.0"
document_id: "14372935b6dd4e712b4625a40577928916567750f11f2c83f757cf9d50950db8"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-schema-delegation-9d832648c543"
published_at: "2018-04-09T21:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:19c0e037bb270395230b6a2c69adc19ae8e7a621316ed2e02a49296327b2bf3b"
---

# GraphQL schema delegation

This is a guest post by Mikhail Novikov, who worked on GraphQL schema delegation and schema stitching in the[graphql-tools library](https://github.com/apollographql/graphql-tools) . He is currently available for remote GraphQL contract work. Contact him atfreiksenet@reindex.io if you need some GraphQL help.


In this article I’m going to talk about schema delegation — a way to automatically forward a GraphQL query (or a part of it) to another schema. Schema delegation allows reusing parts of other schemas without manually querying them. Examples where schema delegation would be particularly appropriate include:


- Building a GraphQL gateway that would forward queries to underlying GraphQL microservices
- Using a third-party GraphQL API as part of your schema
- Using a GraphQL database proxy like[Prisma](https://www.prisma.io/)


## Implementing a GraphQL gateway


Let’s consider a GraphQL service for a basic blog API. The schema has two types —` User` and` Blog` . It also has root fields to retrieve them by their` _id` . On top of the service there is a GraphQL gateway implementing a` Node` interface for the types of an underlying GraphQL service.


` Node` is a common interface in the GraphQL world which indicates any object that can be globally addressed by some` id` . It’s usually coupled with a root field called` node` , which returns any object of a type that implements` Node` , by id. You can read more about` Node` interface in[this article](https://facebook.github.io/relay/graphql/objectidentification.htm) .


The gateway reuses the same` User` and` Blog` types extending them so that they implement the` Node` interface. It also provides` node` root field on top of the two existing ones. Here are the schemas for the service and the gateway.


```text
type User {
_id: ID!
username: String
blogs: [Blog]
}


type Blog {
_id: ID!
user: User
title: String!
text: String
}


type Query {
userById(id: ID!): User
blogById(id: ID!): Blog
}
```


```text
interface Node {
id: ID!
}


extend type User implements Node {
id: ID!
}


extend type Blog implements Node {
id: ID!
}


extend type Query {
node(id: ID!): Node
}
```


Instead of reimplementing` userById` and` blogById` in the gateway, it will execute the query using the service schema. To do it, you could build the query from the information you get as the fourth argument of a resolver —` GraphQLResolveInfo` . It has the subquery starting at the current resolver (` fieldNodes` ), fragments used in the operation (` fragments` ) and variables in` operation` . Out of this information you could create a GraphQL` Document` ,` print` it and send it to the service schema.


For the root fields that match with the service fields you don’t need to do anything else. As for the new fields like` node` you also need to change the name of the root field depending on the global` id` .


### Filtering the query


The above would work for the simpler cases where the query only selects data available on both the gateway and the service. However, it’s not always the case. The underlying service schema doesn’t have` id` fields that` Node` interface has. In addition, one can only spread the fragment with compatible types: when both` User` and` Blog` fragments are spread inside the query, it will fail.


```text
# Source query


{
node(id: "test-global-id") {
id # Does not exist on User or Blog in original schemas
... on User { # Only valid in userById
username
}
... on Blog { # Only valid in blogById
title
}
}
}
```


To fix this, the delegation must filter out the incompatible fields and fragments, as well as unused fragments and variables after filter. Of course, you can do it by going through the document manually, tracking down the types and removing all the fields that don’t exist on the original schema. But you don’t have to because there is a` delegateToSchema` function inside` graphql-tools` that can do all that work for you.


*Schema delegation is adapting the source query so that it matches the subschema automatically.*


## delegateToSchema


` delegateToSchema` creates a GraphQL query that’s a valid query for an underlying schema and executes it. It accepts an object of options:


- ` schema` is the sub-schema that should handle the delegated query
- ` operation` is either` "query"` ,` "mutation"` or` "subscription"` and determines the operation of the GraphQL document
- ` fieldName` is the root field in the schema from which the query will start
- ` args` is an object of *additional* arguments to pass to the field. These arguments supplement and/or override arguments given to the original field in the parent schema
- ` context` is the GraphQL context for the execution
- ` info` is` GraphQLResolveInfo` . It’s used to extract the selection set of the current query, along with variables and fragments.
- ` transforms` is an array of GraphQL Schema Transforms. Transforms will be covered in the next article.


Gateway resolvers using` delegateToSchema` will look like:


```text
import { delegateToSchema } from 'graphql-tools';


const resolvers = {
Query: {
userById(parent, { id }, context, info) {
const { type, localId } = deserializeGlobalId(id);
return delegateToSchema({
schema: subserviceSchema,
operation: 'query',
fieldName: 'userById',
args: { id: localId },
context,
info,
});
},


node(parent, { id }, context, info) {
const { type, localId } = deserializeGlobalId(id);
let fieldName;
if (type === 'Blog') {
fieldName = 'blogById',
} else if (type === 'User') {
fieldName = 'userById',
} else {
throw new Error('Invalid global id');
}
return delegateToSchema({
schema: subserviceSchema,
operation: 'query',
fieldName,
args: { id: localId },
context,
info,
});
},
},
};
```


Here is how a` node` query would delegate to the service:


```text
# Query coming to gateway


query(
$id: ID! # overriden in parameters, so it won't be included
) {
node(id: $id) { # delegate to userById
# This is a selection set from the node resolver
id # This field doesn't exist in User
...BlogFragment # Blog can't be spread here
...UserFragment
}
}


# This fragment is used, so should be included
fragment UserFragment on User {
username
}


# This fragment isn't used, so should be removed
fragment BlogFragment on Blog {
title
}
```


```text
# Query sent to service


query(
$_v0_id: ID! # generated variable name
) {
userById(id: $_v0_id) {
...UserFragment
}
}


fragment UserFragment on User {
username
}
```


Besides forwarding from root fields, you can use` delegateToSchema` to delegate from any other resolvers. For example, if you split` User` and` Blog` types to separate services, you would no longer be able to directly get` User.blogs` from User service and` Blog.user` from Blog service. However, you can connect them at the gateway server and use delegation to get related data:


Get the code[in a Gist](https://gist.github.com/freiksenet/e239464b56203ab79f5e5b5aa50de648#file-resolvers2-js)


## Schema stitching and delegation


Schema delegation is often used together with schema stitching. Schema stitching is a process of combining multiple GraphQL schemas together. It simplifies the creation of a gateway schema — especially when there are multiple services. Schema stitching automatically sets up delegation for root fields that already exist in the stitched-together schemas. New root fields (as well as any new non-root fields) require new resolvers. The above gateway schema can be created as follows:


```text
import { mergeSchemas, delegateToSchema } from 'graphql-tools';


const extensionSchema = `
interface Node {
id: ID!
}
extend type User implements Node {
id: ID!
}
extend type Blog implements Node {
id: ID!
}
extend type Query {
node(id: ID!): Node
}
`;


const schema = mergeSchemas({
schemas: [serviceSchema, extensionSchema],
resolvers: {
Query: {
node(parent, { id }, context, info) {
const { type, localId } = deserializeGlobalId(id);
let fieldName;
if (type === 'Blog') {
fieldName = 'blogById',
} else if (type === 'User') {
fieldName = 'userById',
} else {
throw new Error('Invalid global id');
}
return delegateToSchema({
schema: serviceSchema,
operation: 'query',
fieldName,
args: { id: localId },
context,
info,
});
},
},
// ... more resolvers
},
});
```


You can read about schema stitching in more detail in[the official docs](https://www.apollographql.com/docs/graphql-tools/schema-stitching.html) .


## Further reading


In my next article, I’ll tell how you can customize schema delegation with transforms. Even if your schema has been significantly altered using schema transforms, its ability to delegate to the subschema will be preserved.


Updated schema delegation and schema transforms are are going to be available in version 3 of` graphql-tools` and are already available as` graphql-tools@next` on npm — though please be aware that the exact API could still change slightly before the final release. Check out the PR on GitHub for the code:[Schema transforms, new mergeSchemas and everything by freiksenet · Pull Request #527 ·…This is out of date ATM, will be updatedgithub.com](https://github.com/apollographql/graphql-tools/pull/527)


If you’d like to read more about GraphQL schema delegation, check out these resources:


- [Official docs](https://github.com/apollographql/graphql-tools/blob/next-api/docs/source/schema-delegation.md)
- [GraphQL Schema Stitching Explained](https://blog.graph.cool/graphql-schema-stitching-explained-schema-delegation-4c6caf468405) by Graphcool
- [GraphQL Bindings docs](https://github.com/graphql-binding/graphql-binding) . GraphQL Bindings is a nice abstraction on top of schema delegation, which exposes a programmatic API to root fields of a schema.


Written by


Mikhail Novikov


[Read more by Mikhail Novikov](https://www.apollographql.com/blog/author/freiksenet)
