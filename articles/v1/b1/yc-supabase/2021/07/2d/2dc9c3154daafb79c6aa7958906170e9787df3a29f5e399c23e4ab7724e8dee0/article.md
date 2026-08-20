---
schema_version: "1.0.0"
document_id: "2dc9c3154daafb79c6aa7958906170e9787df3a29f5e399c23e4ab7724e8dee0"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-community-day"
published_at: "2021-07-26T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:4d6ceeac77453f7a3589d8114da18feef11296f3075f07095085bd6619978625"
---

# Supabase Community Day

Supabase has grown a lot1 since last[Launch Week](https://supabase.com/blog/launch-week) , but it wouldn't be possible without some amazing open source tools. Since we're shipping a few upgrades this week we feel it's only fair to shine a spotlight on some tools and community efforts that make Supabase possible.


## PostgreSQL version 13.3#


PostgreSQL is a big part of Supabase, and it's also a huge inspiration - the speed that they ship, their community organization, and their absolute dedication towards reliability. PostgreSQL 13.3 was released in June and (from today) every new Supabase project will be on[PostgreSQL version 13.3](https://supabase.com/blog/supabase-postgres-13) .


### Giving back#


Supabase makes it easy to get started with Postgres, but we think you should try it even if you don't want to use Supabase. Here's a few ways we make it easy to use PostgreSQL without Supabase:


- Supabase was the first company to put Postgres in the[Digital Ocean Marketplace](https://marketplace.digitalocean.com/apps/supabase-postgres) . Since then it's been installed over 1000 times (not by us!).
- We provide a Postgres image in the[AWS marketplace](https://aws.amazon.com/marketplace/pp/prodview-lk2pfa5nafecg) .
- We're packaging the Marketplace version,[along with "Bundles"](https://supabase.com/blog/supabase-postgres-13#postgresql-bundles)
- We're developing a few extensions:[supautils](https://github.com/supabase/supautils) ,[pg_net](https://github.com/supabase/pg_net) , and a couple more soon to be announced.


### Get involved#


- Follow the official[PostgreSQL Twitter](https://twitter.com/PostgreSQL) account.
- Join the official[mailing lists](https://www.postgresql.org/community/) .


## PostgREST version 8.0#


This new PostgREST stable version comes with the improvements we've made to make it truly enterprise-grade. Due to our commitment with OSS, all of our improvements are upstreamed, so you can fully use them on your own self-hosted projects.


### Highlights#


**Improved Performance** : Due to Supabase's high throughput requirements, PostgREST 8.0 handles up to 50% more throughput on GET requests, according to our benchmarks. We converted all SELECT queries to use prepared statements and reduced logging verbosity to make this possible.


**Reduced downtime** : we need PostgREST to be more "set it and forget it", so reloading schema cache now has zero downtime.


**Dynamic Configuration** : we've made it easier to handle PostgREST configuration at scale (we manage thousands of PostgREST instances). This new version includes the ability to use an in-database configuration that is reloadable through a` NOTIFY` command.


**Less admin burden** : PostgREST previously required the` pg_listen` utility to reload its schema cache. This is no longer needed. The schema cache is reloadable with a simple` NOTIFY` command.


**Better diagnostic information** : in the rare cases where PostgREST fails, we want to find the exact root cause quickly. For this we've improved its logging by adding logging levels and timestamps to all server errors.


**Simpler OpenAPI** : showing a complete OpenAPI output used to require a highly-privileged` anon` role. With the new` openapi-mode` , this is no longer needed and` anon` can be kept with minimal privileges.


The community has made many more enhancements and bug fixes for the new version. See[v8.0 CHANGELOG](https://github.com/PostgREST/postgrest/releases/tag/v8.0.0) for the full list.


### Get Started#


Want to get started with PostgREST? The Supabase community has built a number of client libraries to make it simpler to use:


- Javascript:[postgrest-js](https://github.com/supabase/postgrest-js)
- Rust:[postgrest-rs](https://github.com/supabase/postgrest-rs)
- Go:[postgrest-go](https://github.com/supabase/postgrest-go)
- Python:[postgrest-py](https://github.com/supabase/postgrest-py)
- Dart:[postgrest-dart](https://github.com/supabase/postgrest-dart)
- C#:[postgrest-csharp](https://github.com/supabase/postgrest-csharp)
- Swift:[postgrest-swift](https://github.com/supabase/postgrest-swift)
- Ruby:[postgrest-rb](https://github.com/supabase/postgrest-rb)
- Kotlin:[postgrest-kt](https://github.com/supabase/postgrest-kt)


### Get involved#


- Help with[PostgREST development](https://github.com/PostgREST/postgrest/blob/main/.github/CONTRIBUTING.md) .
- Help with[PostgREST docs translations](https://github.com/PostgREST/postgrest-docs/issues/393) .
- Follow the official[PostgREST Twitter](https://twitter.com/postgrest_org) account.


## Supabase Flutter/Dart (Beta release)#


While the Supabase team have been busy with the Javascript libraries, the community have been beavering away with Dart. They've even[built fully-functional apps](https://www.producthunt.com/posts/spot-2d300f54-7a0a-4dbf-aee2-4a75311217cc) using Supabase Auth and Storage.


Today the Community are releasing both the Flutter and Dart libraries in Beta (with a bit of help from the Supabase team).


### Get started#


- Check out the[Flutter Quickstart Guide](https://supabase.com/docs/guides/with-flutter)
- Check out the code:


- Supabase Flutter:[GitHub](https://github.com/supabase/supabase-flutter/tree/main/packages/supabase_flutter) |[Release](https://pub.dev/packages/supabase_flutter)
- Supabase Dart:[GitHub](https://github.com/supabase/supabase-flutter/tree/main/packages/supabase) |[Release](https://pub.dev/packages/supabase)


### Get involved#


- Help[write](https://supabase.com/supasquad#author) the Flutter Docs.
- Help[maintain](https://supabase.com/supasquad#trusted-host) the[Flutter](https://github.com/supabase/supabase-flutter/tree/main/packages/supabase_flutter) and[Dart](https://github.com/supabase/supabase-flutter/tree/main/packages/supabase) libraries.


## Supabase Discord#


OK, OK, we get it - GitHub[Discussions](https://github.com/supabase/supabase/discussions) aren't enough for y'all. While we've been trying to keep the conversation contained to our GitHub org, the community has been creating[subreddits](https://www.reddit.com/r/Supabase/) , StackOverflow[tags](https://stackoverflow.com/questions/tagged/supabase) , and GitHub[topics](https://github.com/topics/supabase) .


A few weeks ago one of the community created a Community-led Discord, and so the team figured we might as well join the fun.


We'll still be using Discussions for debugging, but if you're looking for a place to hang out with Supabase developers, Discord is where to find it.


### Get involved#


Join the Supabase Community Discord:[discord.supabase.com](http://discord.supabase.com/) , say hi, and start building.


## Footnotes#


1.


Supabase has been one of the fastest growing startups[on GitHub](https://github.com/supabase/supabase) for four consecutive quarters: 207% in[Q3](https://runacap.com/ross-index/q3-2020/) 2020; 1,373% in[Q4](https://runacap.com/ross-index/q4-2020/) 2020; 462% in[Q1](https://runacap.com/ross-index/q1-2021/) 2021; 1,653% in[Q2](https://runacap.com/ross-index/q2-2021/) 2021.↩
