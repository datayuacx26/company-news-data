---
schema_version: "1.0.0"
document_id: "fa05b02ff3ad035a44b851de9ed37e89f892bacb766f37fa7ad505b8fbcc2145"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgrest-9"
published_at: "2021-11-27T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:5ebb138d4fa7a1c12c2e3efa9378c1288e834bbc96d4325313254b63191181be"
---

# PostgREST 9

PostgREST turns your PostgreSQL database automatically into a RESTful API. Today, PostgREST 9 was[released](https://postgrest.org/en/v9.0/releases/v9.0.0.html) . Let's take a look at some of the new features.


## Resource embedding with Inner Joins#


PostgREST 9 brings a[much-requested feature](https://github.com/supabase/postgrest-js/issues/197) : the ability to do` inner joins` when embedding a table.


Here's an example with` supabase-js` :


hideCopy


`
_10


const { data, error } = await supabase


_10


.from('messages')


_10


.select('*, users!inner(*)')


_10


.eq('users.username', 'Jane')


`


With the new` !inner` keyword, you can now filter rows of the top-level table (` messages` ) based on a filter (` eq` ) of the embedded table (` users` ). This works across all Supabase client libraries and you can use it with any of the available operators (` gt` ,` in` , etc.)


[Read more](https://postgrest.org/en/v9.0/releases/v9.0.0.html#resource-embedding-with-top-level-filtering) .


## Functions with unnamed parameters#


You can now make` POST` requests to functions with a single unnamed parameter. This is particularly useful for webhooks that send JSON payloads.


For example, imagine you were using Postmark as an email provider and you wanted to save email bounces using their[bounce webhook](https://postmarkapp.com/developer/webhooks/bounce-webhook) . Previously this wouldn't be possible with PostgREST, as every function required a named parameter.


As of PostgREST 9, this is possible. Simply create a function inside your PostgreSQL database to receive the raw JSON:


hideCopy


`
_12


create function store_bounces(json)


_12


returns json


_12


language sql


_12


as $$


_12


insert into bounces (webhook_id, email)


_12


values (


_12


($1->>'ID')::bigint,


_12


($1->>'Email')::text


_12


);


_12


_12


select '{ "status": 200 }'::json;


_12


$$;


`


And the webhook can send data directly to your database via an` rpc` call:


hideCopy


`
_15


POST https://<PROJECT_REF>.supabase.co/rest/v1/rpc/store_bounces HTTP/1.1


_15


Content-Type: application/json


_15


_15


{


_15


"RecordType": "Bounce",


_15


"MessageStream": "outbound",


_15


"ID": 4323372036854775807,


_15


"Type": "HardBounce",


_15


"MessageID": "883953f4-6105-42a2-a16a-77a8eac79483",


_15


"Description": "The server was unable to deliver your message (ex: unknown user, mailbox not found).",


_15


"Details": "Test bounce details",


_15


"Email": "john@example.com",


_15


"From": "sender@example.com",


_15


"BouncedAt": "2019-11-05T16:33:54.9070259Z"


_15


}


`


[Read more](https://postgrest.org/en/v9.0/api.html#s-proc-single-unnamed) .


## PostgreSQL 14 compatibility#


If you've ever done your own custom` auth` functions using PostgREST[HTTP Context](https://postgrest.org/en/v8.0/api.html#accessing-request-headers-cookies-and-jwt-claims) , note that a breaking change was necessary for PostgreSQL 14 Compatibility. You'll need to update them:


` From`` To`


` current_setting('request.jwt.claim.custom-claim', true)`` current_setting('request.jwt.claims', true)::json->>'custom-claim'`


` current_setting('request.header.custom-header', true)`` current_setting('request.headers', true)::json->>'custom-header'`


If you only use Supabase default` auth` functions(` auth.email()` ,` auth.uid()` ,` auth.role()` ), then no action is required because we have updated the functions to handle these changes transparently.


[Read more](https://postgrest.org/en/v9.0/releases/v9.0.0.html#postgresql-14-compatibility) .


## Release notes#


There are a lot more improvements released in PostgREST 9, including[support for Partitioned Tables](https://postgrest.org/en/v9.0/releases/v9.0.0.html#partitioned-tables) ,[improved doc](https://postgrest.org/en/v9.0/releases/v9.0.0.html#documentation-improvements) , and[bug fixes](https://postgrest.org/en/v9.0/releases/v9.0.0.html#bug-fixes) .


You can see the full updates on the[PostgREST 9 release notes](https://postgrest.org/en/v9.0/releases/v9.0.0.html) .


## More Postgres resources#


- [Implementing "seen by" functionality with Postgres](https://supabase.com/blog/seen-by-in-postgresql)
- [Partial data dumps using Postgres Row Level Security](https://supabase.com/blog/partial-postgresql-data-dumps-with-rls)
- [Postgres Views](https://supabase.com/blog/postgresql-views)
- [Postgres Auditing in 150 lines of SQL](https://supabase.com/blog/audit)
- [Cracking PostgreSQL Interview Questions](https://supabase.com/blog/cracking-postgres-interview)
- [What are PostgreSQL Templates?](https://supabase.com/blog/postgresql-templates)
- [Realtime Postgres RLS on Supabase](https://supabase.com/blog/realtime-row-level-security-in-postgresql)
