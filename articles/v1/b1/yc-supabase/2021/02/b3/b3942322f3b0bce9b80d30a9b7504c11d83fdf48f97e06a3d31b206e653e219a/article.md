---
schema_version: "1.0.0"
document_id: "b3942322f3b0bce9b80d30a9b7504c11d83fdf48f97e06a3d31b206e653e219a"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-beta-january-2021"
published_at: "2021-02-02T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:58af6660bd5125550479cffdd1f529c659903a4b99c1c3f715b6bba664f54c68"
---

# Supabase Beta January 2021

New year, new features. We've been busy at Supabase during January and our community has been even busier. Here's a few things you'll find interesting.


### Quick demo#


Watch a full demo:


### Count functionality#


Anyone who has worked with Firebase long enough has become frustrated over the[lack](https://stackoverflow.com/questions/49979714/how-to-get-count-of-documents-in-a-collection) of` count` functionality. This isn't a problem with PostgreSQL! Our libraries now have support for PostgREST's[exact](https://postgrest.org/en/v7.0.0/api.html?highlight=count#exact-count) ,[planned](https://postgrest.org/en/v7.0.0/api.html?highlight=count#planned-count) , and[estimated](https://postgrest.org/en/v7.0.0/api.html?highlight=count#estimated-count) counts. A massive thanks to[@dshukertjr](https://github.com/supabase/postgrest-js/issues/94#event-4210564301) for this adding support to our client library.


### New Auth Providers#


We enabled 2 new Auth providers - Facebook and Azure. Thanks to[@Levet](https://github.com/supabase/gotrue/pull/54) for the Azure plugin, and once again to[Netlify's amazing work](https://github.com/netlify/gotrue/issues/107) with GoTrue to implement Facebook.


### Auth Audit Trail#


We have exposed the audit trail directly in the dashboard, as well as the GoTrue logs. Great for security and debugging.


### Auth UI widget#


In case our Auth endpoints aren't easy enough already, we've built a React[Auth Widget](https://ui.supabase.com/?path=/story/auth-auth--default) for you to drop into your app and to get up-and-running in minutes.


### New` auth.email()` function#


We added a helper function for extracting the logged in user's email address.


### New Regions#


Launch your database in London or Sydney!


### Copy rows as Markdown#


You can now copy SQL results as Markdown - super useful for adding to blogs and issues.


### React server components#


If you're excited by React Server components then check out the Supabase + Server Components experimental repo.[https://github.com/supabase/next-server-components](https://github.com/supabase/next-server-components)


### Learn#


We know that Auth can be a bit daunting when you're just starting out, so we have created some intro videos to get you up to speed in no time:


- [Supabase Auth Deep Dive Part 1: JWTs](https://youtu.be/v3Exg5YpJvE)
- [Supabase Auth Deep Dive Part 2: Restrict Table Access](https://youtu.be/qY_iQ10IUhs)
- [Supabase Auth Deep Dive Part 3: User Based Access Policies](https://youtu.be/0LvCOlELs5U)


### Kaizen#


- Performance: We migrated all of our subdomains to Route53, implementing custom Let's Encrypt certs for your APIs. As a result, our read benchmarks are measuring up 12% faster.
- Performance: We upgrade your databases to the new[GP3](https://aws.amazon.com/about-aws/whats-new/2020/12/introducing-new-amazon-ebs-general-purpose-volumes-gp3/) storage for faster and more consistent throughput.


### Community#


- @kiwicopple chats to @bdougie on HeavyBit's Jamstack Radio:[Link](https://www.heavybit.com/library/podcasts/jamstack-radio/ep-71-open-source-firebase-alternative-with-paul-copplestone-of-supabase)
- Watch @leerob from Vercel deploy a full Next.js app with Supabase in just 2 minutes:[Link](https://twitter.com/leeerob/status/1351576575888797696)
- Redwood now supports Supabase:[Link](https://twitter.com/redwoodjs/status/1347311574415863811)
- Deploy a full analytics solution using Umami:[Link](https://twitter.com/mkalvas/status/1353880637506260994)
- Check out this open source Trello Clone:[Link](https://twitter.com/joshnuss/status/1352094804335857664)
- Get started with Expo + Supabase using this starter template from Kiki:[Link](https://twitter.com/kikiding/status/1352086899242856449)
- Use Supabase Auth with NestJS:[Link](https://twitter.com/atsuhio/status/1348516650144780288?s=21)
- The community has made some serious advances on the[Dart](https://github.com/supabase?q=dart&type=&language=) ,[C#](https://github.com/supabase?q=csharp&type=&language=) ,[Python](https://github.com/supabase?q=python&type=&language=) , and[Kotlin](https://github.com/supabase?q=kotlin&type=&language=) libraries.
- We were one of the fastest growing open source startups in Q4 last year:[Link](https://twitter.com/RunaCapital/status/1351122231791910916)


Source:[repository.surf/supabase](https://repository.surf/supabase)


### Coming next#


We're ramping up to "Launch week" at the end of Q1, where we will be giving you some very exciting new features (including Storage!).


### Get started#


- Start using Supabase today:[supabase.com/dashboard](https://supabase.com/dashboard/)
- Make sure to[star us on GitHub](https://github.com/supabase/supabase)
- Follow us[on Twitter](https://twitter.com/supabase)
- Subscribe to our[YouTube channel](https://www.youtube.com/c/supabase)
- Become a[sponsor](https://github.com/sponsors/supabase)
