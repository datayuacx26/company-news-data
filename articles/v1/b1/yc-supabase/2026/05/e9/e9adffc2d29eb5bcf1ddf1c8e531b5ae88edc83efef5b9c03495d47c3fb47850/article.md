---
schema_version: "1.0.0"
document_id: "e9adffc2d29eb5bcf1ddf1c8e531b5ae88edc83efef5b9c03495d47c3fb47850"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/introducing-supabase-server"
published_at: "2026-05-06T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:12:46.079122+00:00"
content_hash: "sha256:d591c6e7a2b8771110c3bd748a286f774d523df123fd1fc65359b5ab0318d808"
---

# Introducing @supabase/server

Today we're releasing` @supabase/server` in public beta.


This is a new package that handles auth verification, client setup, request context, and common server-side boilerplate for you. It works across Edge Functions, Vercel Functions, Cloudflare Workers, Hono and Bun.


We anonymously analyzed 25,000 deployed Edge Functions and saw the same pattern everywhere: developers were rebuilding the same setup code over and over just to get to their actual business logic.


Most functions needed to:


- Create a Supabase client with` SUPABASE_ANON_KEY`
- Create another admin client with` SUPABASE_SERVICE_ROLE_KEY` that can bypass Row Level Security
- Verify the JWT
- Parse claims
- Handle CORS
- Wire up auth context
- Copy/paste the same` _shared/*.ts` files between functions


With` @supabase/server` you just declare who can call your endpoint and get a fully initialized context back:


- User-scoped Supabase client
- Admin client with service role access
- Verified user identity
- JWT claims
- Built-in request/auth helpers


`
_17


import { withSupabase } from 'npm:@supabase/server'


_17


_17


// Typical Deno.serve usage


_17


Deno.serve(


_17


withSupabase({ auth: 'user' }, async (req, ctx) => {


_17


const { data } = await ctx.supabase.from('todos').select()


_17


return Response.json(data)


_17


})


_17


)


_17


_17


// New fetch style handler usage


_17


export default {


_17


fetch: withSupabase({ auth: 'user' }, async (req, ctx) => {


_17


const { data } = await ctx.supabase.from('todos').select()


_17


return Response.json(data)


_17


}),


_17


}


`


Note that` export default { fetch }` is equivalent to` Deno.serve(...)` . Both define a request handler. We use` export default` throughout this post because it works across Edge Functions, Workers, and Bun. If you prefer` Deno.serve` , you can keep using it — it's[still supported](https://docs.deno.com/runtime/fundamentals/http_server/#default-fetch-export) on Edge Functions.


## How it works#


At the core of` @supabase/server` is the` SupabaseContext` : a request context that includes everything most Edge Functions need, already configured for you.


That includes:


- A user-scoped Supabase client
- An admin client with service role access
- Verified user identity
- JWT claims
- Auth metadata


` @supabase/server` gives you multiple ways to get a` SupabaseContext` . The most common is` withSupabase` , a wrapper that handles auth, client creation, and CORS before your handler runs:


`
_10


import { withSupabase } from 'npm:@supabase/server'


_10


_10


export default {


_10


fetch: withSupabase({ auth: 'user' }, async (req, ctx) => {


_10


const { data } = await ctx.supabase.from('todos').select()


_10


return Response.json(data)


_10


}),


_10


}


`


If you need more control over error handling and responses, you can also call` createSupabaseContext` directly:


`
_11


import { createSupabaseContext } from 'npm:@supabase/server'


_11


_11


export default {


_11


fetch: async (req) => {


_11


const { data: ctx, error } = await createSupabaseContext(req, { auth: 'user' })


_11


if (error) return Response.json({ error: error.message }, { status: error.status })


_11


_11


const { data } = await ctx.supabase.from('todos').select()


_11


return Response.json(data)


_11


},


_11


}


`


Both approaches give you the same` SupabaseContext` . No shared utility files. No environment variable management. No manual JWT verification.


## What's in the context#


Every` withSupabase` handler receives a` ctx` object with two pre-configured clients:


` ctx.supabase` — a user-scoped client that automatically respects RLS policies` ctx.supabaseAdmin` — an admin client using the service role for privileged operations


No manual client setup, JWT verification, or environment variable wiring required.


The full context looks like this:


`
_10


interface SupabaseContext {


_10


supabase: SupabaseClient


_10


supabaseAdmin: SupabaseClient


_10


userClaims: UserIdentity | null


_10


jwtClaims: JWTClaims | null


_10


authMode: AuthMode


_10


}


`


## Declarative access control#


With` @supabase/server` , authentication happens before your handler runs.


You declare who is allowed to call the endpoint, and the package handles verification automatically.


For example, this endpoint allows unauthenticated requests:


`
_10


export default {


_10


fetch: withSupabase({ auth: 'none' }, async (_req, _ctx) => {


_10


return Response.json({ status: 'ok' })


_10


}),


_10


}


`


This endpoint requires a valid user JWT:


`
_10


export default {


_10


fetch: withSupabase({ auth: 'user' }, async (req, ctx) => {


_10


const { data } = await ctx.supabase.from('todos').select()


_10


return Response.json(data)


_10


}),


_10


}


`


If the request does not include a valid user token, the request is rejected before your handler executes.


Here's all of the auth modes included in the package:


`
_14


// authenticated users only (default)


_14


withSupabase({ auth: 'user' }, handler)


_14


_14


// no auth required, good for webhooks and health checks


_14


withSupabase({ auth: 'none' }, handler)


_14


_14


// server-to-server with secret key


_14


withSupabase({ auth: 'secret' }, handler)


_14


_14


// with publishable key


_14


withSupabase({ auth: 'publishable' }, handler)


_14


_14


// accept either a user JWT or a secret key


_14


withSupabase({ auth: \['user', 'secret'\] }, handler)


`


Your function's security model is visible in one line.


## Adopting new auth keys without the boilerplate#


Last year we improved project security with[asymmetric JWT Signing Keys](https://supabase.com/docs/guides/auth/signing-keys) and[new API keys](https://github.com/orgs/supabase/discussions/29260) . Better security for every project, but migrating existing functions was hard.


You had to install` jose` , configure a JWKS endpoint, build your own auth middleware, expose new secrets, and update every function individually.


We fixed it.` @supabase/server` handles new key validation and JWT verification internally. You adopt the package and the new security model comes with it. No` jose` . No JWKS configuration. No manual secret setup.


`
_10


export default {


_10


// auth: 'user' will handle incoming user JWT validation for you


_10


fetch: withSupabase({ auth: 'user' }, async (req, { supabase }) => {


_10


const { data } = await supabase.from('subscriptions').select('*')


_10


return Response.json(data)


_10


}),


_10


}


`


Now you get support for the new auth keys without manual JWT verification. Delete your shared utility files and focus on business logic.


## Same code, every runtime#


` withSupabase` returns a standard` (Request) => Promise<Response>` handler. It works with any runtime that supports the Web API pattern.


Edge Functions, Vercel Functions, and Cloudflare Workers:


`
_10


import { withSupabase } from '@supabase/server'


_10


_10


export default {


_10


fetch: withSupabase({ auth: 'user' }, handler),


_10


}


`


> On Edge Functions, declare the dependency in` deno.json` to import` @supabase/server` from` npm:@supabase/server` .


Hono (with the included adapter):


`
_12


import { withSupabase } from '@supabase/server/adapters/hono'


_12


import { Hono } from 'hono'


_12


_12


const app = new Hono()


_12


_12


app.get('/todos', withSupabase({ auth: 'user' }), async (c) => {


_12


const { supabase } = c.var.supabaseContext


_12


const { data } = await supabase.from('todos').select()


_12


return c.json(data)


_12


})


_12


_12


export default { fetch: app.fetch }


`


## Composable primitives#


Most developers don't need anything beyond` withSupabase` or` createSupabaseContext` . But you can use the underlying primitives directly.


`
_10


import {


_10


createAdminClient,


_10


createContextClient,


_10


resolveEnv,


_10


verifyAuth,


_10


} from '@supabase/server/core'


`


These are useful when you need more control: multiple routes with different auth, custom response headers, or domain-specific wrappers like MCP servers.


Here's an Edge Function with per-route auth:


`
_22


import { createContextClient, verifyAuth } from '@supabase/server/core'


_22


_22


export default {


_22


fetch: async (req) => {


_22


const url = new URL(req.url)


_22


_22


if (url.pathname === '/health') {


_22


return Response.json({ status: 'ok' })


_22


}


_22


_22


if (url.pathname === '/todos') {


_22


const { data: auth, error } = await verifyAuth(req, { auth: 'user' })


_22


_22


_22


const supabase = createContextClient(auth.token)


_22


const { data } = await supabase.from('todos').select()


_22


return Response.json(data)


_22


}


_22


_22


return new Response('Not found', { status: 404 })


_22


},


_22


}


`


These are the same primitives that power` withSupabase` . Teams building MCP servers, custom middleware, or framework adapters can compose them into their own patterns.


## One pattern for humans and AI agents#


We designed` @supabase/server` with agentic development in mind. Every function follows the same structure: declare access, receive context, write logic.


During internal testing, Claude Code migrated an entire project's Edge Functions to` @supabase/server` in a single prompt. That included adopting new API keys, removing shared utility files, and switching every function to` withSupabase` . All functions worked on the first run.


When every function looks the same, agents produce correct code from a single example.


## FAQ#


**Does this replace` @supabase/ssr` ?**


No.` @supabase/ssr` handles cookie-based session management for frameworks like Next.js and SvelteKit.` @supabase/server` handles stateless, header-based auth for Edge Functions, Workers, and other backend runtimes. The two packages coexist and are not replacements for each other. Deeper integration with` @supabase/ssr` is on the roadmap.


If you would like to adopt the DX that this package provides, check our[SSR frameworks](https://github.com/supabase/server/blob/main/docs/ssr-frameworks.md) documentation for implementation references.


**Which runtimes does this support?**


Any runtime or platform that supports the standard` Request` /` Response` Web API.` withSupabase` returns a standard` (Request) => Promise<Response>` handler, so it works on Supabase Edge Functions, Vercel Functions, Cloudflare Workers, Bun, Deno and more.


**Is Hono the only supported framework?**


No. Hono was the first framework adapter we shipped, and we have already merged a community PR for the H3 adapter. We expect to accept more community-contributed adapters.


See more in our adapters[documentation](https://github.com/supabase/server/tree/main/src/adapters) .


**Where is the documentation?**


The package ships with full documentation in the[GitHub repo](https://github.com/supabase/server) . We're also working on adding guides to the[Supabase docs](https://supabase.com/docs) .


**What about environment variables?**


On the Supabase platform and Local Development (CLI), your Edge Functions will receive the required environment variables to work out of the box (` SUPABASE_PUBLISHABLE_KEYS` ,` SUPABASE_SECRET_KEYS` ,` SUPABASE_JWKS` ).


In local development or self-hosted environments, use the same plural form:` SUPABASE_PUBLISHABLE_KEYS` instead of` SUPABASE_ANON_KEY` ,` SUPABASE_SECRET_KEYS` instead of` SUPABASE_SERVICE_ROLE_KEY` .


Check out the[environment variables documentation](https://github.com/supabase/server/blob/main/docs/environment-variables.md) for more details.


**How can I leave feedback?**


Open an issue on the[GitHub repo](https://github.com/supabase/server) or join the conversation in[Discord](https://discord.supabase.com/) .


## Get started#


Install the package and the AI skill:


`
_10


npm install @supabase/server@latest


_10


npx skills add supabase/server


`


The skill gives Claude Code, Codex, Cursor and any agentic coding tool full context about the API surface, patterns, and migration paths. From there, you can prompt your way through most tasks.


`
_10


Analyze all Edge Functions, and plan a full migration to use


_10


the new API keys with @supabase/server


`


Scaffold a new REST API with Hono:


`
_10


Create a Hono API with @supabase/server that has CRUD


_10


endpoints for a todos table, using per-route auth


`


Add a protected Edge Function with admin operations:


`
_10


Create an Edge Function that accepts user or secret key auth,


_10


reads from a user's profile with RLS, and writes audit logs


_10


with the admin client


`


Or write it by hand:


`
_10


import { withSupabase } from 'npm:@supabase/server'


_10


_10


export default {


_10


fetch: withSupabase({ auth: 'user' }, async (req, ctx) => {


_10


const { data } = await ctx.supabase.from('todos').select()


_10


return Response.json(data)


_10


}),


_10


}


`


` @supabase/server` is in public beta. We're looking for feedback on the API surface, the adapter patterns, and edge cases we haven't hit yet.


Check out the[GitHub repo](https://github.com/supabase/server) and the[docs](https://supabase.github.io/server/) and let us know what you build.
