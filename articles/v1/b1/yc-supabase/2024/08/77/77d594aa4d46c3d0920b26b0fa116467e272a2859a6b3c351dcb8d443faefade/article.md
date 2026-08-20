---
schema_version: "1.0.0"
document_id: "77d594aa4d46c3d0920b26b0fa116467e272a2859a6b3c351dcb8d443faefade"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/pg-graphql-1-5-7"
published_at: "2024-08-15T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:9f21ce1bfaf36e3c4bc19b595a09baa452ffa0a800bbb8a0b7f29b7b1c0e950e"
---

# pg_graphql 1.5.7: pagination and multi-tenancy support

# What's new in pg_graphql 1.5.7#


Since our[last check-in on pg_graphql](https://supabase.com/blog/whats-new-in-pg-graphql-v1-2) there have been a few quality of life improvements worth calling out. A quick roundup of the key differences includes:


- Pagination via First/Offset
- Schema based multi-tenancy
- Filtering on array typed columns with` contains` ,` containedBy` and` overlaps`


## First/Offset pagination#


Since the earliest days of pg_graphql,[keyset pagination](https://supabase.github.io/pg_graphql/api/#keyset-pagination) has been supported. Keyset pagination allows for paging forwards and backwards through a collection by specifying a number of records and the unique id of a record within the collection. For example:


`
_10


{


_10


blogCollection(


_10


first: 2,


_10


after: "Y3Vyc29yMQ=="


_10


) {


_10


...


_10


}


`


to retrieve the first 2 records after the record with unique id` Y3Vyc29yMQ==` .


Starting in version` 1.5.0` there is support for` offset` based pagination, which is based on skipping` offset` number of records before returning the results.


`
_10


{


_10


blogCollection(


_10


first: 2,


_10


offset: 5


_10


) {


_10


...


_10


}


`


That is roughly equivalent to the SQL


`
_10


select


_10


*


_10


from


_10


blog


_10


limit


_10


2


_10


offset


_10


5


`


In general as offset values increase, the performance of the query will decrease. For that reason its important to use keyset pagination where possible.


## Performance schema based multi-tennancy#


pg_graphql caches the database schema on first query and rebuilds that cache any time the schema changes. The cache key is a combination of the postgres role and the database schema's version number. Initially, the structure of all schemas was loaded for all roles, and table/column visibility was filtered down within` pg_graphql` .


In multi-tenant environments with 1 schema per tenant, that meant every time a tenant updated their schema, all tenants had to rebuild the cache. When the number of tenants gets large, that burdens the database if its under heavy load.


Following version` 1.5.2` each tenant's cache only loads the schemas that they have` usage` permission for, which greatly reduces the query time in multi-tenant environments and the size of the schema cache. At time of writing this solution powers a project with >2200 tenants.


## Filtering array column types#


From` 1.5.6` pg_graphql has added` contains` ,` containedBy` ,` overlaps` filter operators for scalar array fields like` text\[\]` or` int\[\]` .


For example, given a table


`
_10


create table blog (


_10


id int primary key,


_10


name text not null,


_10


tags text\[\] not null,


_10


created_at timestamp not null


_10


);


`


the` tags` column with type` text\[\]` can be filtered on.


`
_12


{


_12


blogCollection(filter: { tags: { contains: \["tech", "innovation"\] } }) {


_12


edges {


_12


cursor


_12


node {


_12


name


_12


tags


_12


createdAt


_12


}


_12


}


_12


}


_12


}


`


In this case, the result set is filtered to records where the` tags` column contains both` tech` and` innovation` .


## Roadmap#


The headline features we aim to launch in coming releases of pg_graphql include support for:


- Insert on conflict / Upsert
- Nested inserts


If you want to get started with GraphQL today, check out the[Docs](https://supabase.com/docs/guides/graphql) or the[source code](https://github.com/supabase/pg_graphql/) .
