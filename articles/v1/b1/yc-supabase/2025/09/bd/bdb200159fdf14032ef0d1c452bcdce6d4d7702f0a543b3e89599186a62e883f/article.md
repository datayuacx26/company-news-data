---
schema_version: "1.0.0"
document_id: "bdb200159fdf14032ef0d1c452bcdce6d4d7702f0a543b3e89599186a62e883f"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgrest-13-release"
published_at: "2025-09-30T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:18d0f936c7717e6e40a24a9a5f34c575fb456bd1a8167ef9e195d4ff02bc6cce"
---

# PostgREST 13

PostgREST 13 is out! It comes with API and Observabilty improvements. In this post, we'll see what's new.


## Spread To-Many relationships#


This new feature allows you to represent one-to-many and many-to-many relationships as flat JSON arrays.


For example, if you have database similar to IMDB and you’d like to represent it as a hierarchical JSON structure for your frontend, like so:


`
_17


\[


_17


{


_17


"title": "The Shawshank Redemption",


_17


"actors": \["Tim Robbins", "Morgan Freeman"\],


_17


"genres": \["Drama"\]


_17


},


_17


{


_17


"title": "The Godfather",


_17


"actors": \["Marlon Brando", "Al Pacino"\],


_17


"genres": \["Drama", "Crime"\]


_17


},


_17


{


_17


"title": "The Dark Knight",


_17


"actors": \["Christian Bale", "Heath Ledger"\],


_17


"genres": \["Drama", "Crime", "Action"\]


_17


}


_17


\]


`


You can now do it this way:


The above` ...people` is “spreading” the many-to-many relationship between` titles` and` people` , forming a flat array only consisting of the` primary_name` column. This flat array is then renamed to` actors` . We do a similar process for` genres` , which also forms a many-to-many relationship with` people` .


You can see the data model used for this example on this[gist](https://gist.github.com/steve-chavez/93f7ae04b4323e1952710af7129b32cf) . There are more details about this feature on the[official docs](https://docs.postgrest.org/en/v13/references/api/resource_embedding.html#spread-to-many-relationships) .


## Automatic tsvector convertion#


Previously you could only use the full text search operator on` tsvector` columns, now you can do it on` text` and` json/jsonb` columns too:


This works because` text` and` json/jsonb` columns will be automatically converted with` to_tsvector` .


To ensure this operation is fast, add an index:


`
_10


create index idx_titles on people


_10


using gin (to_tsvector('english', primary_name));


`


## Max Affected#


You can now limit to the amount of rows affected by an` update` or` delete` operation with` maxAffected` :


If the rows affected by the operation surpass the limit in` maxAffected` , an error will be thrown.


This also works with` rpc()` , given that it modifies rows and returns the affected rows. More on details on the[official docs](https://docs.postgrest.org/en/v13/references/api/preferences.html#max-affected) .


## Content-Length header#


For observability, you can now verify the response body size in bytes in the` Content-Length` header.


`
_10


HTTP/1.1 200 OK


_10


Content-Length: 104


_10


Content-Location: /items


`


This helps in cases where you want to know which requests consume the most traffic to avoid exceeding egress limits.


## Proxy-Status header#


The PostgREST error code is now present in the` Proxy-Status` header.


`
_10


HTTP/1.1 406 Not Acceptable


_10


Proxy-Status: PostgREST; error=PGRST116


`


You can check the` Proxy-Status` and` Content-Length` headers in the Supabase Logs Explorer.


## Breaking Changes#


### JWT` kid` validation#


PostgREST now validates the JWT` kid` claim. If your JWT contains a Key ID (` kid` ), it will try to match this with one of the` kid` 's in the configured JSON Web Key Set. Check the[official docs](https://docs.postgrest.org/en/v13/references/auth.html#jwk-kid-validation) for more details.


If you use Supabase Auth or the CLI to create JSON Web Keys, you shouldn’t worry about this change as both systems will ensure` kid` 's are present in the JSON Web Key Set.


For users that integrate with other Auth systems, make sure that both your JWT and JWKS follow the above rules.


### Schema validation in PostgREST search path#


The schemas inside` db-schemas` and` db-extra-search-path` are now validated. This means you cannot put a nonexistent schema there, if you do PostgREST will fail with an error message.


If you drop a schema during a migration, you should make sure this is synced with the PostgREST search path, which is possible thanks to postgres transactional DDL:


`
_10


begin;


_10


drop schema old_schema;


_10


alter role authenticator set pgrst.db_schemas = 'public, pg_graphql, others'; -- make sure old_schema is not present here


_10


commit;


`


## Try it out#


PostgREST v13 is now available for all new projects on the Supabase platform, old projects can upgrade to get this new version.


You can look at the full changelog on the[release notes](https://github.com/PostgREST/postgrest/releases/tag/v13.0.0) .
