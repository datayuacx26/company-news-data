---
schema_version: "1.0.0"
document_id: "2ebc3d3ab70805697838535f16d7826800c0bdaaa11486a96e31ed601d4946cd"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/dedicated-poolers"
published_at: "2025-03-07T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:76bf758c9efd5832fe8a26a346381bed31b4fb2be3c50308f3ebf8361566e89c"
---

# Dedicated Poolers

Today we're announcing **Dedicated Poolers** - a Postgres connection pooler that's co-located with your database for maximum performance and reliability.


Don't know what a Pooler is? Check out our[docs](https://supabase.com/docs/guides/database/connecting-to-postgres#serverside-poolers) .


This is available today for select customers, and will be generally available by 20th March, 2025. If you want to be notified when it's ready,[sign up here](https://forms.supabase.com/dedicated-pooler) .


## Supabase Dedicated Pooler#


The Dedicated Pooler is a[PgBouncer](https://www.pgbouncer.org/) instance that's co-located with your Postgres database. This will require you to connect with IPv6 or, if that's not an option, you can use the[IPv4 add-on](https://supabase.com/docs/guides/platform/ipv4-address) .


The dedicated pooler is isolated to your own project and grants you fine-grained control over the configuration.


## Connecting to your database#


This gives you now 3 options for connecting to your database:


1. **Direct connections (free):** recommended for when you are connecting using servers.
2. **Shared Pooler (free):** recommended for when you are connecting using serverless functions (like Next.js or AWS Lambda)
3. **Dedicated Pooler (Pro Plan and above):** recommended for when you are connecting using serverless functions and you start to scale up. It is available on the Pro Plan and above.


## Choosing between Supavisor and PgBouncer#


A year ago AWS[started charging for IPv4 addresses](https://supabase.com/blog/ipv6) . We introduced[Supavisor](https://supabase.com/blog/supavisor-postgres-connection-pooler) to ensure that you didn't need to pay to connect to your database if your network didn't support IPv6. Only ~45% of the world have adopted IPv6, so this was a great solution for many.


In the recent months, our platform has seen unprecedented growth. Tens of thousands of new developers are pouring into Supabase every week, doing weird and wonderful things with their databases:


Introducing Dedicated Poolers gives you the flexibility to choose the right connection type for your use case. If you need dedicated hardware, you can now opt for a Dedicated Pooler on the Pro Plan and above for lower latency, better performance, and higher reliability.


## Getting started#


Dedicated Poolers are available today for our Enterprise customers, and will be generally available by 20th March, 2025. If you want to be notified when it's ready,[sign up here](https://forms.supabase.com/dedicated-pooler) .
