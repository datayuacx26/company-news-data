---
schema_version: "1.0.0"
document_id: "c02a43c3127a3b776c79bebdf6b254edff242eb0c3bd82d83a482cef157b5a7b"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/whats-new-in-pg-graphql-v1-2"
published_at: "2023-04-21T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:13a1520f6439ee14c8a35933e0314e9c745c0bdbb6c6a8cd9068687ffa474d1a"
---

# What's New in pg_graphql v1.2

It's been 4 months since the[1.0.0 release of pg_graphql](https://supabase.com/blog/pg-graphql-v1) . Since then, we’ve pushed several features to improve the APIs that` pg_graphql` produces.


In this article, we’ll walk through those features and show examples of each.


📢 These features are only available on projects with Postgres version` 15.1.0.63` or higher. For help with upgrading, please review the[migrating and upgrading projects guide](https://supabase.com/docs/guides/platform/migrating-and-upgrading-projects) .


### View Support#


Prior to` v1.1` ,` pg_graphql` would only reflect standard tables. Since then, views, materialized views, and foreign tables are now also reflected in the GraphQL schema.


For example:


`
_10


create view "ProjectOwner" as


_10


select


_10


acc.id,


_10


acc.name


_10


from


_10


account as acc


_10


join role as r on r.id = acc.role_id


_10


where acc.role = 'project_owner';


`


Since all entities exposed by` pg_graphql` require primary keys, we must define that constraint for the view. We do that using a comment directive:


`
_10


comment on view "ProjectOwner"


_10


is '@graphql({"primary_key_columns": \["id"\]})';


`


Which yields the GraphQL type:


`
_10


type ProjectOwner implements Node {


_10


nodeId: ID!


_10


id: UUID!


_10


name: String


_10


}


`


With associated` Edge` and` Connection` types. That enables querying via:


`
_10


{


_10


projectOwnerCollection(first: 2) {


_10


edges {


_10


node {


_10


nodeId


_10


name


_10


}


_10


}


_10


}


_10


}


`


Additionally, simple views automatically support mutation events like inserts and updates. You might use these to migrate underlying tables while maintaining backwards compatibility with previous API versions.


## Filtering#


Filtering in SQL is endlessly flexible. We’ve taken two incremental steps to bring more of that flexibility to the GraphQL interface.


### ` is null` and` is not null`#


Handling` null` values can be tricky in both SQL and GraphQL. However, there are similarities we can take advantage of. In` pg_graphql` , every scalar data type has its own filter type, such as` IntFilter` and` StringFilter` . Each of these filter types now includes an` is` argument, which allows you to filter based on whether a value is null or not null. You can do this by using` {is: NULL}` for` null` values and` {is: NOT_NULL}` for non-` null` values.


`
_10


enum FilterIs {


_10


NULL


_10


NOT_NULL


_10


}


_10


_10


type IntFilter {


_10


...


_10


is: FilterIs


_10


}


`


For example:


`
_10


{


_10


blogCollection(filter: { name: {is: NULL}}) {


_10


...


_10


}


_10


}


`


to return all` blog` s where the` name` is` null` .


### **` like`** , **` ilike`** , and **` startsWith`**#


Text filtering options in` pg_graphql` have historically been restricted to equality checks. The hesitation was due to concerns about exposing a default filter that is difficult to index. The combination of[citext](https://www.postgresql.org/docs/current/citext.html) and[PGroonga available on the platform](https://supabase.com/docs/guides/database/extensions/pgroonga) solves those scalability risks and enabled us to expand the` StringFilter` with options for` like`` ilike` and` startsWith` .


`
_10


input StringFilter {


_10


eq: String


_10


...


_10


startsWith: String


_10


like: String


_10


ilike: String


_10


}


`


Note that` startsWith` filters should be preferred where appropriate because they can leverage simple B-Tree indexes to improve performance.


`
_11


{


_11


generalLedgerCollection(filter: { identifierCode: { startsWith: "BX1:" } }) {


_11


edges {


_11


node {


_11


nodeId


_11


identifierCode


_11


amount


_11


}


_11


}


_11


}


_11


}


`


### GraphQL directives` @skip` and` @include`#


The GraphQL spec has evolved over time. Although the spec is clear, it is common for GraphQL servers to selectively omit some chunks of functionality. For example, some frameworks intentionally do not expose an introspection schema as a form of security through obscurity.


` pg_graphql` aims to be unopinionated and adhere exactly to the spec. The` @skip` and` @include` directives are part of the[GraphQL core specification](https://spec.graphql.org/October2021/#sec--skip) and are now functional.


The **` @skip`** directive in GraphQL is used to conditionally skip a field or fragment during query execution based on a Boolean variable. It can be used to make the query more efficient by reducing the amount of data retrieved from the server.


The **` @include`** directive is the mirror of **` @skip`** where a field or fragment is conditionally included depending on the value of a Boolean variable.


Here's an example of how the **` @skip`** directive can be used in a GraphQL query:


`
_11


query getBooks($includeDetails: Boolean!) {


_11


booksCollection {


_11


edges {


_11


node {


_11


id


_11


title


_11


description @skip(if: $includeDetails)


_11


}


_11


}


_11


}


_11


}


`


### User Defined Descriptions#


Users can now use the[comment directive system to assign descriptions](https://supabase.github.io/pg_graphql/configuration/#description) to tables, views and columns.


`
_10


create table public.book(


_10


id int primary key,


_10


title text not null


_10


);


_10


_10


comment on table public.book


_10


is e'@graphql({"description": "a library book"})';


_10


_10


comment on column public.book.title


_10


is e'@graphql({"description": "the title of the book"})';


`


GraphQL IDEs, such as GraphiQL render those descriptions, allowing developers to provide clearer API documentation.


## Roadmap#


The headline features we aim to launch in coming releases of` pg_graphql` include:


1. Support for user-defined functions:[GitHub issue](https://github.com/supabase/pg_graphql/issues/222)
2. Support for nested inserts:[GitHub issue](https://github.com/supabase/pg_graphql/issues/294)
3. An alternative approach to computed relationships based on SQL functions returning **` SET OF`** rather than comment directives (compatible with PostgREST)


## More pg_graphql#


- [Introducing pg_graphql: A GraphQL extension for PostgreSQL](https://supabase.com/blog/pg-graphql)
- [GraphQL is now available in Supabase](https://supabase.com/blog/graphql-now-available)
- [pg_graphql v1.0](https://supabase.com/blog/pg-graphql-v1)
- [pg_graphql: Postgres functions now supported](https://supabase.com/blog/pg-graphql-postgres-functions)
