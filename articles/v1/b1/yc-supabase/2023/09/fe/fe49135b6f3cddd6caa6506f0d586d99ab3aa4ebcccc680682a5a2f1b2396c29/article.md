---
schema_version: "1.0.0"
document_id: "fe49135b6f3cddd6caa6506f0d586d99ab3aa4ebcccc680682a5a2f1b2396c29"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/beta-update-august-2023"
published_at: "2023-09-08T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:2f093fd21c8353faa64fb96018561cbdac1c4e0d5529cf19804ec4644aa1f29d"
---

# Supabase Beta August 2023

Launch Week 8 breezed by, leaving behind a trail of new features to explore. Here is a recap of everything and an update of what else we are working on, like pgvector 0.5.0 with HNSW.


## pgvector v0.5.0: Faster semantic search with HNSW indexes#


Supabase Vector is about to get a lot faster 💨. pgvector v0.5.0 adds Hierarchical Navigable Small World (HNSW), a new type of index that ensures lightning-fast vector searches, especially in high-dimensional spaces and embeddings.


[Blog post](https://supabase.com/blog/increase-performance-pgvector-hnsw)


## Day 1 - Hugging Face is now supported in Supabase#


We are all about open source collaboration, and Hugging Face is one of the open source communities we admire most. That’s why we've added Hugging Face support in our Python Vector Client and Edge Functions (Javascript) 🤗


- [Blog post](https://supabase.com/blog/hugging-face-supabase)
- [Video announcement](https://www.youtube.com/watch?v=RJccSbJ9Go4)


## Day 2 - Supabase Local Dev: migrations, branching, and observability#


The CLI received some serious upgrades including observability tools, streamlined backups, and enhanced migrations. But that's not all – the big game-changer is the introduction of Supabase branching which we’re rolling out to selected customers.


- [Blog post](https://supabase.com/blog/supabase-local-dev)
- [Video announcement](https://www.youtube.com/watch?v=N0Wb85m3YMI)


## Day 3 - Supabase Studio 3.0#


Supabase Studio went to[#1 on Product Hunt](https://www.producthunt.com/products/supabase#ai-sql-editor-by-supabase) with some huge new features, including AI SQL editor, Schema diagrams, Wrappers UI, and a lot more!


- [Blog post](https://supabase.com/blog/supabase-studio-3-0)
- [Video announcement](https://www.youtube.com/watch?v=51tCMQPiitQ)


## Day 4 - Supabase Integrations Marketplace#


With the release of OAuth2 applications, we've made it easier than ever for our partners to extend the Supabase platform with useful tooling 🙌


- [Blog post](https://supabase.com/blog/supabase-integrations-marketplace)
- [Video announcement](https://www.youtube.com/watch?v=gtJo1lTxHfs)


## Day 4 - Vercel Integration 2.0 and Next.js App Router Support#


The New Supabase x Vercel integration streamlines the process of creating, deploying, and maintaining web applications with several enhancements. Plus, it fully supports the App Router in Next.js ▲


[Blog post](https://supabase.com/blog/using-supabase-with-vercel)


## Day 5 - Supavisor: Scaling Postgres to 1 Million Connections#


Supavisor is a scalable, cloud-native Postgres connection pooler written in Elixir. It has been developed with multi-tenancy in mind, handling millions of connections without significant overhead or latency. We’re rolling it out to every database on our platform.


- [Blog post](https://supabase.com/blog/supavisor-1-million)
- [Video announcement](https://www.youtube.com/watch?v=qzxzLSAJDfE)


## Community Highlights from the past 4 months#


Launch Week is an event for our community, so it’s a good time to look back at what happened in the last months (spoiler: a lot).


[Blog post](https://supabase.com/blog/launch-week-8-community-highlights)


## One more thing: HIPAA and SOC2 Type 2#


Supabase is officially SOC2 Type 2 and HIPAA compliant! In this write-up, we offer insights into what you can expect if you’re planning to go through the same process.


[Blog post](https://supabase.com/blog/supabase-soc2-hipaa)


## Launch Week 8 Hackathon Winners#


The decision was not easy, but after assessing a record number of submissions, the panel of judges chose[WITAS](https://github.com/alex-streza/witas) as the winner of the Best Overall project. As the name doesn't suggest, it's an acronym for Wait is that a sticker? In a nutshell, it generates stickers with Midjourney. Huge congrats to[Alex Streza](https://twitter.com/alex_streza) and[Catalina Melnic](https://twitter.com/Catalina_Melnic) .


- [Full list of winners](https://t.co/onYiaDmavb)
- [All the submissions](https://www.madewithsupabase.com/hackathons/launch-week-8)


## More product announcements!#


Shipping doesn’t stop here at Supabase! We are back in full shipping mode and already thinking about the next LW. These are some of the things we’ve been working on:


- Updated and rewrote a bunch of docs:[Row-Level-Security](https://supabase.com/docs/guides/database/postgres/row-level-security) ,[Postgres Roles,](https://supabase.com/docs/guides/database/postgres/roles)[Database configuration](https://supabase.com/docs/guides/database/postgres/configuration) .
- Implemented read only UI for indexes.[PR](https://github.com/supabase/supabase/pull/16582)
- Organization-based Billing, project transfers, Team Plan.[Blog post](https://supabase.com/blog/organization-based-billing)


## Extended Community Highlights#


- Supabase’s Happy Hour made a comeback! Two new episodes of Alaister, Tyler, and Jon chatting about the latest news while live coding.[Episode #27](https://www.youtube.com/watch?v=OWhKVbg1p7Y) |[Episode #28](https://www.youtube.com/watch?v=_Z2f-gGrYu8)
- The State of Postgres 2023 is live. Take the survey and help improve Postgres.[Survey](https://timescale.typeform.com/state-of-pg-23/)
- Jon Meyers stopped by the PodRocket podcast to chat about everything we shipped for LW8.[Podcast](https://podrocket.logrocket.com/supabase-launch-week)
- Supabase Crash Course for iOS Developers: Mikaela shows how to implement a Postgres database in a Swift project.[Video](https://www.youtube.com/watch?v=XBSiXROUoZk)
- The Vite ecosystem conference is back and we are happy to be a Community Partner again.[Get your ticket](https://viteconf.org/23/ecosystem/supabase)
- Building a real app with Tamagui and Supabase.[Video](https://www.youtube.com/watch?v=d32F7crxXsY)
- Build PostgreSQL Databases Faster With Supabase AI SQL Editor.[Video](https://www.youtube.com/watch?v=ueCECQ24STI)
- Creating Customized i18n-Ready Authentication Emails using Supabase Edge Functions, PostgreSQL, and Resend.[Blog post](https://blog.mansueli.com/creating-customized-i18n-ready-authentication-emails-using-supabase-edge-functions-postgresql-and-resend)
- Expo Router Tabs with Supabase Authentication.[Video](https://www.youtube.com/watch?v=6IzrH-1M0uE&list=PL2PY2-9rsgl2DikpQG-GgO7TBgRtdB6NT&index=6)
- Integrating Supabase with Prisma and TRPC: A Comprehensive Guide.[Tutorial](https://tobicode.hashnode.dev/integrating-supabase-with-prisma-and-trpc-a-comprehensive-guide)
- A Supa-Introduction to Supabase.[Blog post](https://medium.com/@alex.streza/a-supa-introduction-to-supabase-e551ea6708e)
- Authentication in Next.js with Supabase Auth and PKCE.[Tutorial](https://dev.to/mryechkin/authentication-in-nextjs-with-supabase-auth-and-pkce-45pk)
- Implementing OAuth in Nuxt with Supabase.[Tutorial](https://dev.to/jacobandrewsky/implementing-oauth-in-nuxt-with-supabase-4p1k)


## ⚠️ Baking hot meme zone ⚠️#


If you made it this far in the email you deserve a devilish treat.


That's it for now, see you next month 👋
