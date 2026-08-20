---
schema_version: "1.0.0"
document_id: "bd0c7be8d4f32e3519e4ad44f31a48ab9c3586ce664368e9457ad6ba79a33ff2"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-beta-august-2021"
published_at: "2021-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:8f910056196e556e1362cb01e7a6e8daf15a2e4304515199d409ffa24feb016c"
---

# Supabase Beta August 2021

We've raised $30M and shipped a bunch of features. Let's dive into what's been happening at Supabase during the month of August.


### Quick recap#


Watch a recap of this month's release.


## We raised $30 million#


Hot off the press, we raised our Series A. We'll use the funds to do more of the same - ship features and hire open source developers. We'll release more details soon. Read more on[TechCrunch](https://techcrunch.com/2021/09/09/supabase-raises-30m-for-its-open-source-insta-backend/) .


## Realtime Security, codename: WALRUS#


If you've been waiting for Row Level Security to land in Postgres[subscriptions](https://supabase.com/docs/reference/javascript/subscribe) , then you're going to love our new repo:[Write Ahead Log Realtime Unified Security (WALRUS)](https://github.com/supabase/walrus) . The name might be a bit forced, but the security design is deliberate. It's not in production yet, but we're making the repo public for comments using an[RFC process](https://github.com/supabase/walrus/blob/master/README.md#rfc-process) .


## Custom SMS templates#


If you're using SMS login in[Auth v2](https://supabase.com/blog/2021/07/28/supabase-auth-passwordless-sms-login) , you can now customize the SMS which is sent to your users. Read more in the[docs](https://supabase.com/docs/guides/auth/phone-login/twilio#sms-custom-template) .


## Dart and Flutter Docs#


Thanks entirely to[@dshukertjr](https://twitter.com/dshukertjr) , we now have in-depth[reference Dart documentation](https://supabase.com/docs/reference/dart/installing) for CRUD, Auth, Realtime and more!


## We launched the South Korea region#


We added another region for those wanting to host their data and APIs in Seoul. We now have 12 regions to choose from


## Table creation is even easier#


You can now create columns while creating your table. We've also added improvements for composite primary keys and foreign key creation.


## Unbreakable CSV Imports#


Our previous importer would choke on CSV files which were too large. Not any more!


## Connection strings#


We now provide a handy copy utility for various database connection strings because we were so tired of looking them up on Stack Overflow.


## We released a primer on Row Level Security#


RLS can be a bit foreign for developers getting started with Postgres. This video by[@_dijonmusters](https://twitter.com/_dijonmusters) demystifies it. If you find the video a useful medium for learning, consider[subscribing to our channel](https://www.youtube.com/c/supabase) .


## Community#


### We had a community Hackathon#


We held a one-week async Hackathon. Check out[all the winners](https://supabase.com/blog/hackathon-winners) - it was truly impressive what people were able to build in just 7 days.


### We had a team Hackathon#


The Supabase team didn't want to miss out on the fun so we held our own hackathon. It was a good way to dog food. Some notable projects include


- [Supaflix](https://supaflix.vercel.app/) by[@abc3](https://twitter.com/abc3erl) \[[GitHub](https://github.com/abc3/supaflix) \] - a Netflix clone build with Supabase
- [Personal-Casts](https://github.com/inian/personal-casts) by[@everconfusedguy](https://twitter.com/everconfusedguy) \[[GitHub](https://github.com/inian/personal-casts) \] - converts Youtube videos and articles into a personal podcast feed.
- And it wouldn't be a Supabase hackathon without a high-effort meme entry. Check out[meme.town](http://meme.town/) by[@Joshenlimek](https://twitter.com/joshenlimek) \[[GitHub](https://github.com/joshenlim/meme-maker) \]


### GitHub Trending#


We hit 18,000 stars on GitHub, and got to the[top of GitHub trending](https://twitter.com/supabase/status/1435868863518760964) for Typescript.


Source:[repository.surf/supabase](https://repository.surf/supabase)


If you want to keep up to date, make sure you[subscribe to our YouTube channel](https://www.youtube.com/c/supabase) or[follow us on Twitter](https://twitter.com/supabase) .


## Dependency contributions#


### GoTrue (Auth)#


- HCaptcha - users can add captcha on their passwordless logins to prevent abuse
[https://github.com/supabase/gotrue/pull/192](https://github.com/supabase/gotrue/pull/192)
- Email change endpoint is fixed (sends out 2 emails, one to the old email and one to the new one)
[https://github.com/supabase/gotrue/pull/132](https://github.com/supabase/gotrue/pull/132)


### PostgREST (APIs)#


- Allow a function with single unnamed parameter to be called with POST
[https://github.com/PostgREST/postgrest/issues/1735](https://github.com/PostgREST/postgrest/issues/1735)


### pg_net (Database Webhooks)#


- Better handling of URL errors
[https://github.com/supabase/pg_net/issues/39](https://github.com/supabase/pg_net/issues/39)


## Coming Next#


Last December we[moved from Alpha to Beta](https://supabase.com/beta) , with a focus on Security, Performance, and Reliability. After a couple of Launch Weeks pushing out new and sexy features, we have decided it's time to focus on these again.


By the time we're done, Supabase will be production-ready for all use cases.


## Get started#


- Start using Supabase today:[supabase.com/dashboard](https://supabase.com/dashboard/)
- Make sure to[star us on GitHub](https://github.com/supabase/supabase)
- Follow us[on Twitter](https://twitter.com/supabase)
- Subscribe to our[YouTube channel](https://www.youtube.com/c/supabase)
- Become a[sponsor](https://github.com/sponsors/supabase)
