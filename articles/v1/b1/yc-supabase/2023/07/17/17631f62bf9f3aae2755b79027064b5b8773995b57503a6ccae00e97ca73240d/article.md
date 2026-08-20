---
schema_version: "1.0.0"
document_id: "17631f62bf9f3aae2755b79027064b5b8773995b57503a6ccae00e97ca73240d"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgrest-11-1-release"
published_at: "2023-07-12T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:0c3eb758f98dcd4c3f1367c62253ff61747c4e6c40e87c5095d40490f8b562a6"
---

# What is new in PostgREST v11.1?

PostgREST 11.1 is now available on the Supabase platform. Besides the[pre-release](https://supabase.com/blog/postgrest-11-prerelease) features, we’ve added configuration and querying improvements. Here is what's new:


## Impersonated Role Settings#


Every role that passes[PostgREST JWT Authentication](https://postgrest.org/en/stable/references/auth.html#jwt-based-user-impersonation) is an *impersonated role* . On the Supabase platform, these are the` anon` ,` authenticated` and` service_role` roles.


These roles can now have settings applied with a regular` ALTER ROLE .. SET` . This is useful, for example, to prevent web users from running expensive queries.


Let’s try it by setting a statement timeout and cost limit.


### Statement timeout#


` statement_timeout` aborts any statement that takes more than the specified amount of time. Let’s set it for the` anon` ,` authenticated` and` service_role` roles:


`
_17


-- anonymous users can run queries that take 100 milliseconds max


_17


alter


_17


role anon


_17


set


_17


statement_timeout = '100ms';


_17


_17


-- authenticated users can run queries that take 5 seconds max


_17


alter


_17


role authenticated


_17


set


_17


statement_timeout = '5s';


_17


_17


-- backend-only users can run queries that take 15 seconds max


_17


alter


_17


role service_role


_17


set


_17


statement_timeout = '15s';


`


You need to reload PostgREST config cache to apply these changes.


`
_10


NOTIFY pgrst,


_10


'reload config';


`


Now, suppose you do an expensive query with the` anon` role. Like filtering on a big table's unindexed column (this will cause a full table scan):


`
_10


const { data, error } = await supabase.from('big_table').select().eq('unindexed_column', 'value')


`


Then, after 5 seconds, the request will be aborted with the response:


`
_10


{


_10


"hint": null,


_10


"details": null,


_10


"code": "57014",


_10


"message": "canceling statement due to statement timeout"


_10


}


`


Which is what we wanted. Note that there's already a global` statement_timeout` set but you can be more fine-grained with this feature. See[timeouts](https://supabase.com/docs/guides/database/timeouts) for more details.


### Statement Cost Limit#


With a statement timeout, expensive queries will still get executed for a length of time. They'll consume resources until they’re terminated.


The[pg_plan_filter](https://github.com/pgexperts/pg_plan_filter) extension (available on the Supabase platform), brings a statement cost limit. This abort queries at the planning phase, before they get executed.


You can use it like:


`
_22


-- anonymous users can only run cheap queries


_22


ALTER


_22


USER anon


_22


SET


_22


plan_filter.statement_cost_limit = 10000;


_22


_22


-- authenticated users can run more expensive queries


_22


ALTER


_22


USER authenticated


_22


SET


_22


plan_filter.statement_cost_limit = 1e6;


_22


_22


-- backend-only users can run any query


_22


ALTER


_22


USER service_role


_22


SET


_22


plan_filter.statement_cost_limit = 0;


_22


_22


NOTIFY pgrst,


_22


'reload config';


_22


_22


-- reload postgREST config cache to apply changes


`


Let’s repeat the previous expensive query with the` anon` role.


`
_10


`


Then, immediately, the request will be aborted and the response will be:


`
_10


{


_10


"hint": null,


_10


"details": null,


_10


"code": "54001",


_10


"message": "plan cost limit exceeded"


_10


}


`


Note that tuning is required to get the cost limit right. You should use the` plan_filter.statement_cost_limit` with care as it can invalidate legitimate queries.


## Configurable Transaction Isolation Level#


By default, all queries run in a transaction with the default *read committed* isolation level.


You can now modify this with the` default_transaction_isolation` setting.


If you want a function to run with *repeatable read* isolation level:


`
_10


create function hello()


_10


returns text as $$


_10


select 'hello';


_10


$$ language sql


_10


set default_transaction_isolation = 'repeatable read';


`


Or if you want an impersonated role to run its queries with a *serializable* isolation level:


`
_10


alter


_10


role service_role


_10


set


_10


default_transaction_isolation = 'serializable';


_10


_10


NOTIFY pgrst,


_10


'reload config';


_10


_10


-- reload postgREST config cache


`


Note that the default *read committed* is good enough for almost all use cases. Higher isolation levels incur in overhead as they use more sophisticated locking. They're only needed in special cases.


## Bulk insert JSON with default values#


A long wanted feature was bulk inserting JSON while considering columns' default values.


Having the following sample table.


`
_10


create table


_10


foo (


_10


id bigint generated by default as identity primary key,


_10


bar text,


_10


baz int default 100


_10


);


`


You can now do it like this:


`
_10


const { error } = await supabase


_10


.from('foo')


_10


.insert(\[


_10


{ "bar": "val1"


_10


}


_10


, { "bar": "val2"


_10


, "baz": 15


_10


}


_10


\], defaultToNull: false)


_10


.select()


`


And the response will be:


`
_10


\[


_10


{ "id": 1, "bar": "val1", "baz": 100 },


_10


{ "id": 2, "bar": "val2", "baz": 15 }


_10


\]


`


As you can see,` id` and` baz` took their default values.


## ANY/ALL filter modifiers#


As a shortcut to` OR` filters, you can now use` any` modifiers on various filters. Take the` like` filter as an example:


`
_10


const res = await postgrest


_10


.from('users')


_10


.select()


_10


.likeAnyOf('username', \['%supa%', '%kiwi%'\])


`


This is equivalent to the following in SQL.


`
_10


select *


_10


from users


_10


where username like ANY('{%supa%,%kiwi%}');


`


` any` modifiers are available for the` eq,like,ilike,gt,gte,lt,lte,match,imatch` filters.


For completeness, the` all` modifier is also included.


## Minimal Breaking Changes from v10#


If you only use PostgREST through Supabase client libraries (like[supabase-js](https://supabase.com/docs/reference/javascript/introduction) ) then it's safe to upgrade to v11. If you use PostgREST with other HTTP clients (like` curl` ), consider the breaking changes for this version:


- The` Range` header is now only considered on GET requests and is ignored for any other method. Previously PostgREST responded with an error but[RFC 9110](https://www.rfc-editor.org/rfc/rfc9110.html#name-range) dictates that we should ignore the` Range` header instead.
- RPC requests no longer consider the` Prefer: params=multiple-objects` header. This header was already deprecated on[v10.1.0](https://github.com/PostgREST/postgrest/releases/tag/v10.1.0) .


By making use of[Logflare](https://logflare.app/) , we detected that out of 20 thousands of projects:


- Only 7 projects used` Range` for HTTP methods other than GET. In these cases all responses were errors so in fact this breaking change is a fix for those requests.
- None were using` Prefer: params=multiple-objects` .


So overall the breaking changes are minimal.


## Closing up#


There you have it, now you can make your API more secure with role settings and use higher isolation levels without resorting to direct PostgreSQL connections.


PostgREST v11.1 is available for all Supabase projects created after 5 July 2023. Existing projects can upgrade by doing a pause/unpause.


## More Postgres resources#


- [Storing OpenAI embeddings in Postgres with pgvector](https://supabase.com/blog/openai-embeddings-postgres-vector)
- [Choosing a Postgres Primary Key](https://supabase.com/blog/choosing-a-postgres-primary-key)
- [SQL or NoSQL? Why not use both (with PostgreSQL)?](https://supabase.com/blog/sql-or-nosql-both-with-postgresql)
- [pg_jsonschema: JSON Schema support for Postgres](https://supabase.com/blog/pg-jsonschema-a-postgres-extension-for-json-validation)
- [Implementing "seen by" functionality with Postgres](https://supabase.com/blog/seen-by-in-postgresql)
