---
schema_version: "1.0.0"
document_id: "3a0ff1a270a5ac3565da7e32a1e806a91694a30711f5ddbc9d1d226d9e9bf309"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgrest-12-2"
published_at: "2024-08-16T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:d46e741ecd477aa0e44e7bde9898c5adef18a9e523224f7f2ee92553abcd8bb7"
---

# PostgREST 12.2: Prometheus metrics

[PostgREST 12.2](https://github.com/PostgREST/postgrest/releases/tag/v12.2.0) is out! It comes with Observability and API improvements. In this post, we'll see what's new.


## Prometheus Metrics#


Version 12.2 ships with Prometheus-compatible metrics for PostgREST's schema cache and connection pool. These are useful for troubleshooting, for example, when PostgREST's pool is starved for connections.


`
_10


curl localhost:3001/metrics


_10


_10


# HELP pgrst_db_pool_timeouts_total The total number of pool connection timeouts


_10


# TYPE pgrst_db_pool_timeouts_total counter


_10


pgrst_db_pool_timeouts_total 7.0


_10


_10


# ....


`


A full list of supported metrics is available in the[PostgREST documentation](https://postgrest.org/en/latest/references/observability.html#metrics) .


## Hoisted Function Settings#


Sometimes it's handy to set a custom timeout per function. You can now do this on 12.2 projects with:


`
_10


create or replace function special_function()


_10


returns void as $$


_10


select pg_sleep(3); -- simulating some long-running process


_10


$$


_10


language sql


_10


set statement_timeout to '4s';


`


And calling the function with the[RPC interface](https://supabase.com/docs/reference/javascript/rpc) .


When doing` set statement_timeout` on the function, the` statement_timeout` will be “hoisted” and applied per transaction.


By default this also works for other settings, namely` plan_filter.statement_cost_limit` and` default_transaction_isolation` . The list of hoisted settings can be extended by modifying the[db-hoisted-tx-settings](https://postgrest.org/en/latest/references/configuration.html#db-hoisted-tx-settings) configuration.


Before 12.2, this could be done by setting a` statement_timeout` on the API roles, but this affected all the SQL statements executed by those roles.


## Max Affected#


In prior versions of PostgREST, users could limit the number of records impacted by mutations (insert/update/delete) to 1 row using vendor media type` application/vnd.pgrst.object+json` . That supports a common use case but is not flexible enough to support user defined values.


12.2 introduces the` max-affected` preference to limit the affected rows up to a custom value.


For example:


`
_10


curl -i "http://localhost:3000/items?id=lt.15" -X DELETE \\


_10


-H "Content-Type: application/json" \\


_10


-H "Prefer: handling=strict, max-affected=10"


`


If the number of affected records exceeds` max-affected` , an error is returned:


`
_10


HTTP/1.1 400 Bad Request


_10


{


_10


"code": "PGRST124",


_10


"message": "Query result exceeds max-affected preference constraint",


_10


"details": "The query affects 14 rows",


_10


"hint": null


_10


}


`


## **Try it out**#


PostgREST v12.2 is already available on the Supabase platform on its latest patch version ([v12.2.3](https://github.com/PostgREST/postgrest/releases/tag/v12.2.3) ) for new projects. Spin up a new project or upgrade your existing project to try it out!
