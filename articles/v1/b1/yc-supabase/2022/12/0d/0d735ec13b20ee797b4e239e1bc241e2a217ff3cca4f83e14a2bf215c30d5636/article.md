---
schema_version: "1.0.0"
document_id: "0d735ec13b20ee797b4e239e1bc241e2a217ff3cca4f83e14a2bf215c30d5636"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgrest-11-prerelease"
published_at: "2022-12-16T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:70eec3a0bac87a76a5e825132ee601bbd1665d734ec0a1e19918e95854799dcb"
---

# PostgREST 11 pre-release

PostgREST 11 is not wrapped up yet, however a pre-release with the **[latest features and fixes](https://github.com/PostgREST/postgrest/releases/tag/v10.1.1.20221212)** is available on the Supabase CLI.


In this blog post we'll cover some of the improved querying capabilities: spreading related tables, related orders and anti-joins.


## Spreading related tables#


Very often the way we structure a database is not the way we want to present it to the frontend application. For example, let's assume we have a` films` and` technical_specs` tables and they form a one-to-one relationship.


Using PostgREST resource embedding, we can query them in one request like so


From HTTP:`
_10


GET /films?select=title,technical_specs(camera,laboratory,sound_mix)


`


or JavaScript:`
_10


const { data, error } = await supabase.from('films').select(\`


_10


title,


_10


technical_specs (


_10


camera, laboratory, duration


_10


)


_10


\`)


`


Response:`
_11


\[


_11


{


_11


"title": "Pulp Fiction",


_11


"technical_specs": {


_11


"camera": "Arriflex 35-III",


_11


"laboratory": "DeLuxe, Hollywood (CA), USA (color)",


_11


"duration": "02:34:00"


_11


}


_11


},


_11


"..."


_11


\]


`


But we'd like to present a “flattened” result to the frontend, without the` technical_specs` object. For this we could create a new database view or function that shapes the json the way we want, but creating extra database objects is not always convenient.


Using the new “spread” operator(syntax borrowed from[JS](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) ), we can expand a related table columns and remove the nested object.


From HTTP:`
_10


GET /films?select=title,...technical_specs(camera,laboratory,duration)


`


or JavaScript:`
_10


const { data, error } = await supabase.from('films').select(\`


_10


title,


_10


...technical_specs (


_10


camera, laboratory, duration


_10


)


_10


\`)


`


Response:`
_10


\[


_10


{


_10


"title": "Pulp Fiction",


_10


"camera": "Arriflex 35-III",


_10


"laboratory": "DeLuxe, Hollywood (CA), USA (color)",


_10


"duration": "02:34:00"


_10


},


_10


"..."


_10


\]


`


This only works for one-to-one and many-to-one relationships for now but we're looking at ways to remove this restriction.


## Order by related tables#


It's also a common use case to order a table by a related table column. For example, suppose you'd like to order` films` based on the` technical_specs.duration` column.


You can now do it like so:


From HTTP:`
_10


GET /films?select=title,...technical_specs(duration)&order=technical_specs(duration).desc


`


or JavaScript:`
_10


const { data, error } = await supabase


_10


.from('films')


_10


.select(\`


_10


title,


_10


...technical_specs (


_10


duration


_10


)


_10


\`)


_10


.order('technical_specs(duration)', { descending: true }))


`


Response:`
_11


\[


_11


{


_11


"title": "Amra Ekta Cinema Banabo",


_11


"duration": "21:05:00"


_11


},


_11


{


_11


"title": "Resan",


_11


"duration": "14:33:00"


_11


},


_11


"..."


_11


\]


`


Similarly to spreading related tables, this only works for one-to-one and many-to-one relationships.


## Anti-Joins#


To do the equivalent of a left anti-join, you can now filter the rows where the related table is` null` .


From HTTP:`
_10


GET /films?select=title,nominations()&nominations=is.null


`


or JavaScript:`
_10


const { data, error } = await supabase


_10


.from('films')


_10


.select(\`


_10


title,


_10


nominations()


_10


\`)


_10


.is('nominations', null))


`


Response:`
_12


\[


_12


{


_12


"title": "Memories of Murder"


_12


},


_12


{


_12


"title": "Rush"


_12


},


_12


{


_12


"title": "Groundhog Day"


_12


},


_12


"..."


_12


\]


`


Note that` nominations` doesn't select any columns so they don't show on the resulting response.


The equivalent of an inner join can be done by filtering the rows where the related table is` not null` .


`
_10


GET /films?select=title,nominations(rank,...competitions(name))&nominations=not.is.null


`


`
_10


const { data, error } = await supabase


_10


.from('films')


_10


.select(


_10


\`


_10


title,


_10


nominations(rank,...competitions(name))


_10


\`


_10


)


_10


.not('nominations', 'is', null)


`


Response:`
_11


\[


_11


{


_11


"title": "Pulp Fiction"


_11


"nominations": \[


_11


{"rank": 1, "name": "Palme d'Or"},


_11


{"rank": 1, "name": "BAFTA Film Award"},


_11


{"..."}


_11


\]


_11


},


_11


"..."


_11


\]


`


This was already possible with the` !inner` modifier([introduced on PostgREST 9](https://supabase.com/blog/postgrest-9#resource-embedding-with-inner-joins) ) but the` not null` filter is more flexible and can be used with an[or filter](https://supabase.com/docs/reference/javascript/or) to combine related tables' conditions.


## Try it out#


This pre-release is not deployed to Supabase cloud but you can try it out locally with the[Supabase CLI](https://supabase.com/docs/reference/cli/introduction) .


`
_10


$ supabase start


`


Please try it and report any bugs, suggestions or ideas!


## More Launch Week 6#


- [Day 1: New Supabase Docs, built with Next.js](https://supabase.com/blog/new-supabase-docs-built-with-nextjs)
- [Day 2: Supabase Storage v2: Image resizing and Smart CDN](https://supabase.com/blog/storage-image-resizing-smart-cdn)
- [Day 3: Multi-factor Authentication via Row Level Security Enforcement](https://supabase.com/blog/mfa-auth-via-rls)
- [Day 4: Supabase Wrappers, a Postgres FDW framework written in Rust](https://supabase.com/blog/postgres-foreign-data-wrappers-rust)
- [Day 5: Supabase Vault is now in Beta](https://supabase.com/blog/vault-now-in-beta)
- [Community Day](https://supabase.com/blog/launch-week-6-community-day)
- [Point in Time Recovery is now available](https://supabase.com/blog/postgres-point-in-time-recovery)
- [Custom Domain Names are now available](https://supabase.com/blog/custom-domain-names)
- [Wrap Up: everything we shipped](https://supabase.com/blog/launch-week-6-wrap-up)
