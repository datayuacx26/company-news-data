---
schema_version: "1.0.0"
document_id: "13e264c25a585c55ccd90fdb88b7b416aee1f858cda09f2c484a3ba268e124b3"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgrest-12"
published_at: "2023-12-13T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:26:20.635269+00:00"
content_hash: "sha256:d570417121239ac644c5ace205ee498541e0acf55634f8dc45475bd5fded9431"
---

# PostgREST 12

PostgREST 12 is out. In this post, we'll focus on a few of the major features. For the complete list, check out the[release on GitHub](https://github.com/PostgREST/postgrest/releases/tag/v12.0.0) .


## Performance: JWT Caching#


Until now, PostgREST has validated JWTs on every request. As of PostgREST 12, the JWT is cached on the first request using the` exp` claim to set the cache entry's lifetime.


Why is that a big deal? Well, it turns out decoding JWTs is expensive. Very expensive.


`
_10


## before


_10


$ curl 'localhost:3000/authors_only' -H "Authorization: Bearer $JWT" -D -


_10


HTTP/1.1 200 OK


_10


Server-Timing: jwt;dur=147.7


_10


_10


## after, with JWT caching


_10


$ curl 'localhost:3000/authors_only' -H "Authorization: Bearer $JWT" -D -


_10


HTTP/1.1 200 OK


_10


Server-Timing: jwt;dur=14.1


`


The JWT cache shaves over 130ms off the server side timing. For projects with a high volume of API calls, upgrading to PostgREST 12 gives you faster responses, higher throughput, and lower resource consumption.


## Server Timing Header#


Did you notice the` Server-Timing` header in the last example?[That's new too](https://postgrest.org/en/v12.0/references/admin.html#server-timing-header) and it does more than measure JWT decoding duration.


Here's a complete reference to what you can extract from your responses:


`
_10


Server-Timing:


_10


jwt;dur=14.9,


_10


parse;dur=71.1,


_10


plan;dur=109.0,


_10


transaction;dur=353.2,


_10


response;dur=4.4


`


Where the information from each phase is internally timed by PostgREST for better visibility into server side performance.


## Aggregate Functions#


Support for aggregate functions has been[much requested feature](https://github.com/supabase/postgrest-js/issues/206) that went through multiple iterations of design and review.


Currently, PostgREST supports` avg` ,` count` ,` max` ,` min` ,` sum` . Here's a minimal example using` count` :


`
_10


$ curl 'http://postgrest/blog_post?select=id.count()'


_10


_10


\[


_10


{


_10


"count": 51,


_10


}


_10


\]


`


We can also add a “group by” simply by adding another element to the select clause.


`
_12


$ curl 'http://postgrest/blog_post?select=title,id.count()'


_12


_12


\[


_12


{


_12


"title": "Supabase Blog",


_12


"count": 40


_12


},


_12


{


_12


"title": "Contributors Blog",


_12


"count": 11


_12


},


_12


...


`


This example only scratches the surface. Aggregates are fully-compatible with[resource embedding](https://postgrest.org/en/stable/references/api/resource_embedding.html) which yields an extremely versatile interface. We'll explore this feature more in a deep-dive coming soon.


## Media Type Handlers#


PostgREST now gives you the flexibility to[handle your custom media types and override the built-in ones](https://postgrest.org/en/v12.0/references/api/media_type_handlers.html) . Among other things, that enables[serving HTML, javascript, or whatever you can think of, straight from your database](https://postgrest.org/en/latest/how-tos/providing-html-content-using-htmx.html) .


`
_26


create domain "text/html" as text;


_26


_26


create or replace function api.index()


_26


returns "text/html"


_26


language sql


_26


as $$


_26


select $html$


_26


<!DOCTYPE html>


_26


<html>


_26


<head>


_26


<meta charset="utf-8">


_26


<meta name="viewport" content="width=device-width, initial-scale=1">


_26


<title>PostgREST + HTMX To-Do List</title>


_26


<!-- Tailwind for CSS styling -->


_26


<link href="https://unpkg.com/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">


_26


</head>


_26


<body class="bg-gray-900">


_26


<div class="flex justify-center">


_26


<div class="max-w-lg mt-5 p-6 bg-gray-800 border border-gray-800 rounded-lg shadow-xl">


_26


<h5 class="mb-3 text-2xl font-bold tracking-tight text-white">PostgREST + HTMX To-Do List</h5>


_26


</div>


_26


</div>


_26


</body>


_26


</html>


_26


$html$;


_26


$$;


`


With PostgREST running locally we can then navigate to[localhost:3000/rpc/index](http://localhost:3000/rpc/index) to see


We're still working through the full implications of this feature, but we're very excited internally about the possibilities it unlocks! Similar to aggregate functions, there's a dedicated post for this feature on the way.


## Availability#


For self-hosting, check out the PostgREST[release on GitHub](https://github.com/PostgREST/postgrest/releases/tag/v12.0.0) .


The latest version will be rolled out across all projects on the managed platform soon. Keep an eye out for notifications inside[Supabase Studio](https://supabase.com/dashboard) .
