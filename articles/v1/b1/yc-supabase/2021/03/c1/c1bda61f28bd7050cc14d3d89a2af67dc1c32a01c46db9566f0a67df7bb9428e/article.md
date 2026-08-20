---
schema_version: "1.0.0"
document_id: "c1bda61f28bd7050cc14d3d89a2af67dc1c32a01c46db9566f0a67df7bb9428e"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/launch-week"
published_at: "2021-03-25T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:7e073d1bfc6df407206be532034c6be0d7d5fe99ba548d7aef302f2a96b80627"
---

# Launch week

When we joined Y Combinator last summer, we spent the entire 3-month program building one (major) feature: Auth.


Demo Day gave our team something to rally around. The looming deadline forced us to deliver a lot of complex functionality in a short amount of time.


After we finished YC, we recreated Demo Day conditions internally: we gave ourselves a 3-month timeline to move from Alpha to Beta.


It worked wonders. We made[giant leaps forward](https://supabase.com/beta) in the performance, security, and stability of the platform.


The day after our Beta announcement (still high on the adrenaline of the launch) we asked ourselves: what's the most ambitious thing we can hope to launch 3 months from now?


We decided not to settle for just one major feature. We wanted to fill an entire week with launches and announcements.


Looking at the list of features we planned, and with the relatively small team we have, this felt like a hugely ambitious idea. But after another 3 months of building, we're happy to announce that...


Starting on Monday 29th March, we'll be commencing our Launch Week!


## What to expect#


We don't want to spoil the surprise, so for now we'll give you a teaser of what to expect. Come back here each day to see what we launch, or follow us on twitter to keep up to date:[@supabase](https://twitter.com/supabase) and follow the hashtag[#supalaunchweek](https://twitter.com/hashtag/supalaunchweek) .


## Monday: Pricing#


- 29 March 2021


We're announcing pricing today. If you don’t care for the details,[here is the pricing](https://supabase.com/pricing) .


The[blog post here](https://supabase.com/blog/pricing) goes into depth around our decision-making process.


The key takeaway is that we will stick with (what we feel is) a generous Free Plan. This was critical for many of our users who are still in the “build” phase of their project/business.


Another key takeaway is predictability. We’ve talked to too many devs who have been caught out by Firebase’s usage billing.


Usually this is due to rogue API requests, which have a habit of spiking without any good debugging tools. To circumvent this, our usage pricing is centered on more “predictable” mechanisms (like storage), avoiding usage-billing on items like API requests.


Even on our Free Plan you’re able to make millions of requests per day (we have developers doing this already!). In the future we will introduce “usage caps” on our top tier, which will allow you to specify maximum spends. Finally, we’re starting with soft-tiers, so nobody will be “unexpectedly” upgraded.


We’re a startup ourselves, so we want to build a sustainable business. Even though we are offering a Free Plan, we’re confident in our ability to support it long-term with the enterprise features we are building (SSO, multi-cloud).


If you aren’t confident in our business model though, we’re open source. You can[host it yourself](https://github.com/supabase/supabase/tree/master/docker) . Even better, everything we build is centered around Postgres, so you can pretty much` pgdump` your database and take it to your favorite Postgres platform.


## Tuesday: Storage#


- 30 March 2021


Checkout the launch post here:[Supabase Storage](https://supabase.com/blog/supabase-storage)


As with anything that we build in Supabase, Storage is fast, secure and scalable. It's integrated well with the rest of the Supabase ecosystem.


**Security** 🔒


The authentication is handled by Kong and the authorization policies are implemented via Row Level Security policies in Postgres. This way we didn't have to implement a new language for implementing authorization policies. Check out our blog post for more details on how this works.


**Performance** ⚡️


Our storage API is a thin layer built with Fastify, a[blazing fast](https://www.fastify.io/benchmarks/) Node.js framework. The security policies are directly evaluated in Postgres. The object explorer in dashboard makes navigation easy even when you have deeply nested folders or millions of objects in your buckets.


**Scalability** 📈


You can use storage for your terabytes of ML training data, e-commerce product gallery, or just your growing collection of JPEGS featuring cat's playing synths in space. You can truly build in a weekend and scale to millions.


## Wednesday: CLI#


- 31 March 2021


Today is Day 3 of[Launch Week](https://supabase.com/blog/launch-week) , and as promised - we're releasing our CLI.


Today we launched the Supabase CLI which includes:


- Running Supabase locally
- Managing database migrations
- Self hosting with Docker!


Check out the announcement post here:[/blog/supabase-cli](https://supabase.com/blog/supabase-cli)


## Thursday: UI (not an April Fool's joke, we promise)#


- 1 April 2021


We've spent a lot of time over the last 12 months making sure we have a lightning fast and delightful user dashboard that keeps pace with everything we do on the backend.


We are constantly iterating on our own UI, and having multiple devs working on different areas simultaniously has led to us requiring a standardized UI component library.


And now we're doing what we do best at Supabase and making it open source, so that the community can benefit from the UI work done at Supabase, and vice versa. The open source community is an invaluable asset to the work we do every day, and it's genuinely the main driver which enables our relatively small team to work fast and ship often.


Check out the demo and documentation website directly here:[ui.supabase.com](https://ui.supabase.com/)


## Friday: One more thing#


- 2 April 2021


It's the season finale of Supabase Launchweek! And we have THREE more things to share:


1.


[Connection Pooling with PgBouncer](https://supabase.com/blog/supabase-pgbouncer) - now you can connect directly to your Postgres instance without needing to worry that you'll cap out on connections. Perfect for use with Prisma.


2.


[Workflow Engine](https://supabase.com/blog/supabase-workflows) - Our WIP states language will eventually enable a drag and drop workflow builder like Zapier inside the dashboard. Built with Elixir for scale!


3.


supabase.io is now supabase.com - check out our origin story here:[/blog/supabase-dot-com](https://supabase.com/blog/supabase-dot-com)
