---
schema_version: "1.0.0"
document_id: "3711447ca6388d306898a95ef8e3f337f16a1669238f7ec1a639de0cd17659ec"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-beta-update-may-2023"
published_at: "2023-06-09T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:0e3a11a7283128a3783d3896660ae3c47d9e1dbd669ee9c2a8e5df8b19dae43a"
---

# Supabase Beta May 2023

We have two big product announcements this month, new integrations with products we love, and exciting highlights from our amazing community.


We're also absolutely stoked to let you know that we've hit a massive milestone:[50k GitHub Stars](https://github.com/supabase/supabase) ! 🌟. Much love to everyone who helped us get there!


## Supabase Vector: the open source Vector Toolkit for Postgres#


Storing vector embeddings in Postgres with` pgvector` is becoming increasingly popular for AI applications, so we're building out a collection of tools to store, index, and query embeddings at scale.


[Supabase Vector](https://supabase.com/modules/vector)


## Vault is now available for all projects#


Vault is a Postgres extension and accompanying Supabase UI that makes it safe and easy to store encrypted secrets and other data in your database.


[Learn how to use Vault.](https://supabase.com/docs/guides/database/vault)


## Auth Helpers now include server-side Auth and full support for the Next.js App Router#


We have updated the[Next.js Auth Helpers package](https://github.com/supabase/auth-helpers) to make it available across the client and server of the App Router. They also now implement server-side auth by default with PKCE - meaning the entire auth flow is now possible server-side.


[Updated docs](https://supabase.com/docs/guides/auth/auth-helpers/nextjs)


[Video course](https://www.youtube.com/playlist?list=PL5S4mPUpp4OtMhpnp93EFSo42iQ40XjbF)


## Improving our dashboard with user feedback#


As we plan the next few months of Dashboard development, we're reaching out to users to see all the different ways people use the Dashboard in their work.


Last month, we opened up a public[RFC for the Dashboard SQL Editor](https://github.com/orgs/supabase/discussions/14206) . It's been amazing to see how people use this tool to build their projects. If you're a heavy user of the SQL Editor, we'd love to get your feedback.


We also started doing user interviews to understand how users use the Dashboard. Our goal is to build the best possible Dashboard for all of our users, and you can help! Reach out to[Terry](https://twitter.com/saltcod) you would like to share your experience.


## Quick product announcements#


-


\[Auth\] You can now use Turnstile as a Captcha provider.[Doc](https://supabase.com/docs/guides/auth/auth-captcha#sign-up-for-captcha)


-


\[Auth\] How to send a password reauthentication nonce.[Doc](https://supabase.com/docs/reference/javascript/auth-resend)


-


\[Dashboard\] Supabase Wrappers UI that supports pulling data in from Firebase, Stripe, S3, and Clickhouse.[Create a Wrapper](https://supabase.com/dashboard/project/_/database/wrappers)


-


\[Edge Functions\] Support for deploying all Edge Functions via CLI.[Doc](https://supabase.com/docs/guides/functions/quickstart#deploy-all-functions)


-


\[Edge Functions\] Custom domains and vanity domains support for Edge Functions.[PR](https://github.com/supabase/supabase-js/pull/778)


-


\[Storage\] Image Transformation is now out of Beta.[Doc](https://supabase.com/docs/guides/storage/serving/image-transformations)


-


\[Postgres Extensions\] pg_cron 1.5.2 (new projects only) now supports sub-minute schedules.[PR](https://github.com/citusdata/pg_cron)


## AI resources & examples#


The whole team has been busy building with ChatGPT, pgvector, and creating valuable resources so the community can unleash their imagination and build great AI apps faster than ever.


-


[Supabase Vecs: a vector client for Postgres](https://supabase.com/blog/vecs)


-


[ChatGPT plugins now support Postgres & Supabase](https://supabase.com/blog/chatgpt-plugins-support-postgres)


-


[Building ChatGPT Plugins with Supabase Edge Runtime](https://supabase.com/blog/building-chatgpt-plugins-template)


-


[Using AI to Generate a PostgreSQL Database for Tony Hawk’s Pro Skater 2 with Zero SQL Experience](https://www.youtube.com/watch?v=ffdUZbYov9g)


## Flutter Hackathon winners#


We hosted our first-ever Flutter Hackathon, in partnership with FlutterFlow and Invertase. Samuel Philip built[Syno](https://github.com/ineffablesam/Syno) , a YouTube summarizer app that uses OpenAI, and won the limited Flutter-themed Supabase keyboard 💙, and there are tons of other great open source projects to check out.


[Read the blog post with all the winners](https://supabase.com/blog/flutter-hackathon-winners)


[Watch the live announcement with Pooja and Majid](https://www.youtube.com/watch?v=AazB9mQetkw)


## Made with Supabase#


[Quivr](https://github.com/stangirard/quivr) , your second brain in the cloud, utilizes the power of GenerativeAI to store and retrieve unstructured information. Built by[Stan Girard](https://twitter.com/_StanGirard) using LangChain, OpenAI, and Supabase Vector.


Bonus track: make sure to also check out[ChartGPT](https://www.chartgpt.dev/) , which just launched V2.0 with a big UI refresh ✨


## Extended Community Highlights#


- We worked with Cloudflare on a new integration that makes it super easy to connect your Supabase database from Cloudflare Workers.[Doc](https://supabase.com/partners/integrations/cloudflare-workers)


- Deno Fresh Starter: ChatGPT-style doc search.[Blog post](https://deno.com/blog/build-chatgpt-doc-search-with-supabase-fresh)


-


Vector Similarity Search in Bubble using Supabase Vector.[Integration](https://medium.com/@ivbran/vector-similarity-search-in-bubble-io-using-supabase-and-pgvector-672a2e69fbc7)


-


Supabase RLS with Typescryp.


-


How Supabase does performance benchmarking using k6, with Egor Romanov.[Webinar](https://www.youtube.com/watch?v=VcrQidYBjEg)


-


Code With Antonio - Spotify Clone with Next.js Auth Helpers.[Video tutorial](https://www.youtube.com/watch?v=2aeMRB8LL4o)


-


Our friends from 1Password announced Passage and wrote an integration guide.[Doc](https://supabase.com/partners/integrations/passageidentity)


-


How to create a chatbot with OpenAI's API: a conceptual cheat sheet.[Tutorial](https://dev.to/farez/how-to-create-a-chatbot-with-openais-api-a-conceptual-cheat-sheet-2o5p)


-


How to Implement Role-Based Access with Supabase.[Tutorial](https://dev.to/akkilah/how-to-implement-role-based-access-with-supabase-3a2)


-


Rodrigo Mansueli, Developer Support Enginere here at Supabase, started a blog that is starting to become go-to resource for leveling up with Supabase and Postgres.[Blog](https://blog.mansueli.com/)


-


Building a mobile authentication flow for your SaaS with Expo and Supabase.[Tutorial](https://blog.spirokit.com/building-a-mobile-authentication-flow-for-your-saas-with-expo-and-supabase)


-


Huntabyte released a new course: Modern SaaS Apps with SvelteKit, Stripe, & Supabase.[Full Course](https://courses.huntabyte.com/modern-saas)


-


Setting Up n8n on Railway with Postgres Database.[Tutorial](https://nesin.io/blog/host-n8n-railway-external-database)


## We're hiring#


Great opportunity to be an early and key member of our Customer Success team. Come join one of the fastest-growing open source projects ever 🤗


- [Customer Solution Architect](https://boards.greenhouse.io/supabase/jobs/4888237004)


## Meme Zone#


As always, one of our favorite memes from last month.[Follow us on Twitter](https://twitter.com/supabase) for more.
