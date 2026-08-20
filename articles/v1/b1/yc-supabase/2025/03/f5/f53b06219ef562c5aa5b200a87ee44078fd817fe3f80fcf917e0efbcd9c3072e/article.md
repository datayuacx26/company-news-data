---
schema_version: "1.0.0"
document_id: "f53b06219ef562c5aa5b200a87ee44078fd817fe3f80fcf917e0efbcd9c3072e"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/clerk-tpa-pricing"
published_at: "2025-03-31T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:5fab26cf6c4a496a22ce2272fc23989f4bc463bc725fb5ab2b6b9190728c5f1e"
---

# Supabase Auth: Bring Your Own Clerk

Today we're expanding our official Third-party Auth integrations to include[Clerk](https://clerk.com/) .


[Third-party Auth](https://supabase.com/docs/guides/auth/third-party/overview) allows you to use external Auth providers with the Supabase as a drop-in replacement for Supabase Auth. This modular design is[intentional](https://supabase.com/docs/guides/getting-started/architecture#product-principles) , allowing you to pick and choose features of Supabase. Our platform makes it easy to get started with Postgres and *any* of your favorite tools.


It was[already possible](https://supabase.com/partners/integrations/clerk) to use Clerk with Supabase, however the previous method was a bit of a hack that required sharing your project's secret and JWT templates from Clerk. We've worked with the Clerk team on the new implementation. Now you can enjoy better security and the same developer experience you've come to expect from Supabase.


To get started with Clerk and Supabase, visit Clerk's[Connect with Supabase](https://dashboard.clerk.com/setup/supabase) page.


Register your Clerk domain[in the Supabase Dashboard](https://supabase.com/dashboard/project/_/auth/third-party) or in the CLI:


`
_10


\[auth.third_party.clerk\]


_10


enabled = true


_10


domain = "example.clerk.accounts.dev"


`


In your JavaScript app all you need to do is write the following code:


`
_10


import { createClient } from '@supabase/supabase-js'


_10


_10


const SUPABASE_URL = 'https://<supabase-project>.supabase.co'


_10


const SUPABASE_ANON_KEY = '<SUPABASE_ANON_KEY>'


_10


_10


const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY, {


_10


accessToken: () => {


_10


return Clerk.session?.getToken()


_10


},


_10


})


`


[Read the docs](https://supabase.com/docs/guides/auth/third-party/clerk) to set up Flutter and Swift (iOS) applications, and to learn how to use Postgres Row-level Security (RLS) Policies.


## Third-Party Auth is now a lot cheaper#


One more thing: today we're making Third-party Auth cheaper so that it has pricing parity with Supabase Auth.


You can have up to 50,000 MAU on the Free plan, or 100,000 MAU on the Pro plan and $0.00325 per MAU above that number.


Free Plan Pro Plan


Previously 50 MAUs included 50,000 MAUs included


Now 50,000 MAUs included 100,000 MAUs included


## Get started today#


Supabase Auth makes it easy to implement authentication and authorization in your app. We provide client SDKs and API endpoints to help you create and manage users.


- [Read the documentation](https://supabase.com/docs/guides/auth/third-party/overview) for Third-party Auth
- [Learn how to use Clerk with Supabase](https://supabase.com/docs/guides/auth/third-party/clerk)
- [Sign up for Supabase](https://supabase.com/dashboard/sign-up) and get started today
