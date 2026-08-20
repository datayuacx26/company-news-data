---
schema_version: "1.0.0"
document_id: "77898681934a2e563e51c839a87f69b1ea39d4c8cb39734b220209306082f361"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/beta-update-september-2023"
published_at: "2023-10-04T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:398371f2b63a33877e1bfc7758aa06fdf9e10945204fb8dc816713b90f0d70dc"
---

# Supabase Beta September 2023

We’ve come a long way since our first update email in April 2020. We’ve gone from 6 hosted databases on our platform to over 650,000 (and that doesn’t even include[database branches](https://supabase.com/blog/supabase-local-dev) . As always we’ve had another hectic month of shipping fast, and community involvement in our[open source repos](https://github.com/supabase/supabase) , meaning we can iterate on Supabase faster than ever. Here’s a small sample of some of the things we shipped during September…


## Broadcast realtime messages via REST API#


You can now broadcast Realtime messages to all your connected users by simply using a REST API call, removing the need to connect to a WebSocket. This will be especially useful with our Edge Functions!


[Broadcast docs](https://supabase.com/docs/guides/realtime/broadcast#send-messages-using-rest-calls)


## Supavisor is now used for connection pooling in all new projects#


Less than two months ago, we announced Supavisor, our own Postgres connection pooler that handles millions of connections. It’s now available in all new projects. You can continue using pgbouncer alongside Supavisor, however, it will be deprecated effective January 15th, 2024.


[Learn more](https://github.com/orgs/supabase/discussions/17817)


### Moving to IPV6 for Database Connection Strings#


With IPv4 addresses becoming increasingly scarce and[cloud providers starting to charge](https://aws.amazon.com/blogs/aws/new-aws-public-ipv4-address-charge-public-ip-insights/) for it, we won’t be assigning IPv4 addresses to Supabase projects from January 15th, 2024.` \[db.projectref.supabase.co\](http://db.projectref.supabase.co)` will start resolving to a IPv6 address instead. If you plan on connecting to your database directly, you must ensure that your network can communicate over IPv6. Supavisor will continue to return IPv4 addresses, so you can update your applications to connect to Supavisor instead.


[Learn more](https://github.com/orgs/supabase/discussions/17817)


## New Foreign Data Wrapper: Airtable#


Introducing the latest addition to our Wrappers lineup: Airtable! You can use it to query data from your Airtable bases and tables directly from Postgres:


`
_10


select * from my_airtable_table;


_10


# data from Airtable


`


[Read the Airtable Wrapper docs](https://supabase.com/docs/guides/database/extensions/wrappers/airtable)[Learn about Wrappers](https://supabase.com/blog/postgres-foreign-data-wrappers-rust)


## Supabase Studio's latest enhancements#


Supabase Studio is under constant improvement, here is what’s new:


- Added UI support for cascade updates
- Improved JSON previewing and editing
- Button for toggling Realtime right from the Table Editor
- See View definition and open it in the SQL Editor to modify
- UI to view and unban IP addresses to manage banned IPs more conveniently
- Import CSV files at the table creation stage and a new button to download the table as CSV


[Try them out](https://supabase.com/dashboard/project/_)


## HNSW Support for Vecs + pgvector#


Added HNSW support inside Vecs, our Python library for` pgvector` . Vecs automatically creates schemas and collections inside your database, making it one of the easiest ways to get started with` pgvector` .


[Read the HNSW docs](https://supabase.com/docs/guides/ai/python/api#create-an-index)


## Quick product announcements#


- \[PostgREST\] JWT caching just landed. API requests are about to get 100ms faster. \[[PR](https://github.com/PostgREST/postgrest/pull/2928) \]
- \[Auth\] Added a default in-memory storage mechanism to allow using` supabase-js` in these environments to fall back to use this default storage mechanism now. Upgrade to supabase-js v2.36.0 or gotrue-js v2.54.0 for the latest changes. \[[PR](https://github.com/supabase/gotrue-js/pull/774) \]
- \[Edge Functions\] Supports much simpler API for creating functions` Deno.serve(req => new Response("ok"))` . No` http` standard library dependency needed. (Thanks eifr for contributing with updated CLI templates. \[[PR](https://github.com/supabase/cli/pull/1504) \]
- \[Edge Functions\] You can manage the secrets for your project's Edge Functions via the dashboard. \[[Try it now](https://supabase.com/dashboard/project/_/functions/secrets) \]


## Supabase at the AI Engineer Summit#


We’re sponsoring a conference for the first time. We'll be participating in the keynote and hosting a workshop on pgvector. Our team of AI experts will be on hand at booth D2 to introduce you to Supabase Vector (and handing out exclusive swag 🛍️).


[Participate both physically and remotely](https://www.ai.engineer/summit)


## Made with Supabase#


Discover the stories of two innovative developers who used Supabase to build AI applications that reached remarkable achievements:


- 🧠[Quivr](https://supabase.com/customers/quivr) : an open source second brain that went viral and became one of the most popular Generative AI projects, launching 5000 databases on Supabase.
- 💬[Chatbase](https://supabase.com/customers/chatbase) : an AI chatbot builder that scaled to $1M in 5 months, making it one of the most successful single-founder AI products in the industry.


## Extended Community Highlights#


- Sign in with Apple on Expo React Native.[Video](https://www.youtube.com/watch?v=-tpcZzTdvN0)
- Infra & remote working AMA with the Fly team.[Twitter Space](https://twitter.com/supabase/status/1704023293295243600)
- Supaweek: build a production-ready API backed by Supabase, OpenAI, and Zuplo.[Day 1](https://zuplo.com/blog/2023/09/25/using-openai-and-supabase-db-to-create-an-api) |[Day 2](https://zuplo.com/blog/2023/09/26/handling-user-requests-dynamically) |[Day 3](https://zuplo.com/blog/2023/09/27/documentation-for-your-supabase-api) |[Day 4](https://zuplo.com/blog/2023/09/28/monetizing-apis) |[Day 5](https://zuplo.com/blog/2023/09/29/announcing-supabase-auth-for-dev-portal)
- Building a real-time WhatsApp Web Clone with Next.js, Supabase, and Stream.[Full series](https://getstream.io/blog/whatsapp-nextjs/)
- Full Stack Calendly Clone using SwiftUI & Supabase.[Video](https://www.youtube.com/watch?v=SKkh2ZFTgdY)
- Using Supabase’s vector database with PostgreSQL.[Article](https://blog.logrocket.com/using-supabases-vector-database-postgresql/)
- Build A Complex Supabase App w/NoCode.[Video](https://www.youtube.com/watch?v=1n4UGyNDAis)
- Configuring Office365 as the SMTP Provider in Supabase Auth: A Comprehensive Guide.[Guide](https://blog.mansueli.com/configuring-office365-as-the-smtp-provider-in-supabase-auth-a-comprehensive-guide)
- The Edgiest Stack of 2023: Next.js, ShadCN UI, OpenAI, and Supabase.[Article](https://programmingfire.com/the-edgiest-stack-of-2023-nextjs-shadcn-ui-openai-and-supabase)
- Live coding: Harnessing the Power of Vector Embedding Search with Supabase.[Live video](https://www.youtube.com/watch?v=NtOi784Evls)
- Deploying Documenso with Vercel, Supabase and Resend.[Article](https://dev.to/documenso/deploying-documenso-with-vercel-supabase-and-resend-foi)
- Next x Nest - Using Supabase & Google OAuth in NestJS.[Tutorial](https://dev.to/abhikbanerjee99/next-x-nest-using-supabase-google-oauth-in-nestjs-1m7j)


## We are hiring#


Come join one of the fastest-growing open source projects ever 🤗


- [Head of Data](https://boards.greenhouse.io/supabase/jobs/4981444004)
- [Support Engineer (Americas)](https://boards.greenhouse.io/supabase/jobs/4973255004)
- [Technical Documentation Lead (Frontend Engineer)](https://boards.greenhouse.io/supabase/jobs/4965064004)


## **⚠️ Baking hot meme zone ⚠️**#
