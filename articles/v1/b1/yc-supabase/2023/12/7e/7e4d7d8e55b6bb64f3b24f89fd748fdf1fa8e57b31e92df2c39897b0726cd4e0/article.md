---
schema_version: "1.0.0"
document_id: "7e4d7d8e55b6bb64f3b24f89fd748fdf1fa8e57b31e92df2c39897b0726cd4e0"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/pg-graphql-postgres-functions"
published_at: "2023-12-12T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:4f6918d7548b673d031d7575f525cbd0ca25f6c67bc9d6dfcdeca24c37a3cefc"
---

# pg_graphql: Postgres functions now supported

Supabase GraphQL (pg_graphql) 1.4+ supports the most requested feature: Postgres functions a.k.a. User Defined Functions (UDFs). This addition marks a significant improvement in GraphQL flexibility at Supabase, both as a novel approach to defining entry points into the Graph and as an escape hatch for users to implement custom/complex operations.


As with all entities in Supabase GraphQL, UDFs support is based on automatically reflecting parts of the SQL schema. The feature allow for the execution of custom SQL logic within GraphQL queries to help support complex, user defined, server-side operations with a simple GraphQL interface.


## Minimal Example#


Consider a function **` addNums`** for a basic arithmetic operation:


`
_10


create function "addNums"(a int, b int default 1)


_10


returns int


_10


immutable


_10


language sql


_10


as $$


_10


select a + b;


_10


$$;


`


when reflected in the GraphQL schema, the function is exposed as:


`
_10


type Query {


_10


addNums(a: Int!, b: Int): Int


_10


}


`


To use this entry point, you could run:


`
_10


query {


_10


addNums(a: 2, b: 3)


_10


}


`


which returns the JSON payload:


`
_10


{


_10


"data": {


_10


"addNums": 5


_10


}


_10


}


`


Supabase GraphQL does its best to reflect a coherent GraphQL API from all the information known to the SQL layer. For example, the argument` a` is non-null because it doesn't have a default value while` b` can be omitted since it does have a default. We also detected that this UDF can be displayed in the` Query` type rather than the` Mutation` type because the function was declared as` immutable` , which means it can not edit the database. Of the other[function volatility categories](https://www.postgresql.org/docs/current/xfunc-volatility.html) ,` stable` similarly translates into a` Query` field while` volatile` (the default) becomes a` Mutation` field.


### Returning Records#


In a more realistic example, we might want to return a set of an existing object type like` Account` . For example, lets say we want to search for accounts based on their email address domains matching a string:


`
_23


create table "Account"(


_23


id serial primary key,


_23


email varchar(255) not null


_23


);


_23


_23


insert into "Account"(email)


_23


values


_23


('a@foo.com'),


_23


('b@bar.com'),


_23


('c@foo.com');


_23


_23


create function "accountsByEmailDomain"("domainToSearch" text)


_23


returns setof "Account"


_23


stable


_23


language sql


_23


as $$


_23


select


_23


id, email


_23


from


_23


"Account"


_23


where


_23


email ilike ('%@' || "domainToSearch");


_23


$$;


`


Since our function is` stable` , it continues to be a field on the` Query` type. Notice that since we're returning a collection of` Account` we automatically get support for[Relay style pagination](https://relay.dev/graphql/connections.htm) on the response including` first` ,` last` ,` before` ,` after` as well as filtering and sorting.


`
_35


type Query {


_35


accountsByEmailDomain(


_35


domainToSearch: String!


_35


_35


"""


_35


Query the first \`n\` records in the collection


_35


"""


_35


first: Int


_35


_35


"""


_35


Query the last \`n\` records in the collection


_35


"""


_35


last: Int


_35


_35


"""


_35


Query values in the collection before the provided cursor


_35


"""


_35


before: Cursor


_35


_35


"""


_35


Query values in the collection after the provided cursor


_35


"""


_35


after: Cursor


_35


_35


"""


_35


Filters to apply to the results set when querying from the collection


_35


"""


_35


filter: AccountFilter


_35


_35


"""


_35


Sort order to apply to the collection


_35


"""


_35


orderBy: \[AccountOrderBy!\]


_35


): AccountConnection


_35


}


`


To complete the example, here's a call to our user defined function:


`
_10


query {


_10


accountsByEmailDomain(domainToSearch: "foo.com", first: 2) {


_10


edges {


_10


node {


_10


id


_10


email


_10


}


_10


}


_10


}


_10


}


`


and the response:


`
_18


{


_18


"data": {


_18


"accountsByEmail": {


_18


"edges": \[


_18


{


_18


"node": {


_18


"id": 1,


_18


"email": "a@foo.com"


_18


}


_18


},


_18


"node": {


_18


"id": 3,


_18


"email": "c@foo.com"


_18


}


_18


\]


_18


}


_18


}


_18


}


`


While not shown here, any relationships defined by foreign keys on the response type` Account` are fully functional so our UDF result is completely connected to the existing Graph.


It’s worth mentioning that we could have supported this query using the default` accountCollection` field that` pg_graphql` exposes on the` Query` type using an` ilike` filter so the example is only for illustrative purposes.


i.e.:


`
_10


query {


_10


accountCollection(filter: { email: { ilike: "%foo.com" } }, first: 2) {


_10


edges {


_10


node {


_10


id


_10


email


_10


}


_10


}


_10


}


_10


}


`


would give the same result as our UDF.


### Limitations#


The API surface area of SQL functions is surprisingly large. In an effort to bring this feature out sooner, some lesser-used parts have not been implemented yet. Currently functions using the following features are excluded from the GraphQL API:


- Overloaded functions
- Functions with a nameless argument
- Functions returning void
- Variadic functions
- Functions that accept a table/views's tuple type as an argument
- Functions that accept an array type


We look forward to implementing support for many of these features in coming releases.


## Takeaways#


If you're an existing Supabase user, but new to GraphQL, head over to[GraphiQL built right into Supabase Studio](https://supabase.com/dashboard/project/_/api/graphiql) for your project to interactively explore your projects through the GraphQL API. User defined function support is new in pg_graphql 1.4+. You can check your project's GraphQL version with:


`
_10


select *


_10


from pg_available_extensions


_10


where name = 'pg_graphql';


`


To upgrade, check out[our upgrade guide](https://supabase.com/docs/guides/platform/migrating-and-upgrading-projects) .


For new Supabase users,[creating a new project](http://database.new/) will get you the latest version of Supabase GraphQL with UDF support.


If you're not ready to start a new project but want to learn more about` pg_graphql` /Supabase GraphQL, our[API docs](https://supabase.github.io/pg_graphql/api/) are a great place to learn about how your SQL schema is transformed into a GraphQL API.
