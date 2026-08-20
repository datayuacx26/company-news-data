---
schema_version: "1.0.0"
document_id: "6866f0e4cffe78440b7000a6115d18014376185125e85cba0919a22c24ee7c22"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/using-supabase-with-vercel"
published_at: "2023-08-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:c54cba2f063dd5d084c95a438539d465c8a2d3af840c920c6aeaac6f8e202197"
---

# Vercel Integration and Next.js App Router Support

Vercel's open source framework,[Next.js](https://nextjs.org/) , is the most popular frontend framework for Supabase developers.


At Supabase, we feel it's important to provide developers with the tools they need to build on the platforms they love. So for the past few months, we've doubled-down on the Vercel x Supabase experience. Here are a few of the newest improvements.


## The New Supabase x Vercel integration#


Our new[Vercel Integration](https://vercel.com/integrations/supabase) streamlines the process of creating, deploying, and maintaining web applications.


### Monorepo support#


You can now[link multiple](https://app.supabase.com/org/_/integrations) Vercel projects to a single Supabase project:


Previously we mapped each Vercel project to a single Supabase project. With this release, we're introducing the concept of project 'Connections'. Supabase projects can have an unlimited number of Vercel Connections. This is especially useful for monorepos using[Turborepo](https://turbo.build/) .


### Automatically managed Supabase config#


We've improved the way we manage your Supabase environment variables.


Supabase keeps each of your Vercel Projects updated with Environment Variables, managing your secrets (like` service-role-key` ) and public variables (like` supabase-url` ).


Importantly, Supabase now updates the Auth Redirect URIs to match your main Vercel project domain and any preview deployment URLs. We listen to your Vercel deployment webhooks and adjust your redirects accordingly.


### Starter kits#


Don't have a project to work on yet? Not a problem, Supabase has a range of Starter kits. With the click of a button you can you have an full-stack application running in less than a minute.


You can find a Vercel Starter kit by looking for Vercel's blue "Deploy Button". Clicking on any one of these buttons will:


1. Take you to Vercel to clone the repository to your own GitHub account/organization
2. Auto install the Supabase Integration (if not already done so).
3. Then we make things really easy: Supabase checks for any migrations in the Starter kit, and if so, we'll run them in your new Supabase project.


After Vercel has deployed the app, it works without any additional configuration. Starter kits include everything from table schemas, authentication, and sample data from the` seed.sql` file.


### Database Branching#


We announced Database Branching on[Tuesday](https://supabase.com/blog/supabase-local-dev#branching-and-preview-environments) . We've designed this to work perfectly with Vercel's Preview Deployments.


For now, Branching in Supabase is limited to development partners. We'll be rolling it out to everyone over the coming months. If you're interested in testing the new features,[sign up for early access](https://forms.supabase.com/branching-request) .


## Next.js 13 App Router Support#


The new App Router in Next.js adds a lot of exciting features, like React Suspense and Streaming. Supabase now fully-supports the App Router in Next.js:


The Next.js App Router shifts a significant amount of your app development from the client to the server, using Server Components, Route Handlers and Server Actions. This means Supabase Auth needs to be configured to store session data in cookies - available in the browser and on the server - rather than Local Storage. We've simplified this process for you with our new[Next.js Auth Helpers package](https://supabase.com/docs/guides/auth/auth-helpers/nextjs) .


This configures cookie-based Auth, making the user's session available throughout the entire Next.js App Router stack:


- [Client Components](https://supabase.com/docs/guides/auth/auth-helpers/nextjs#client-components) —` createClientComponentClient`
- [Server Components](https://supabase.com/docs/guides/auth/auth-helpers/nextjs#server-components) —` createServerComponentClient`
- [Server Actions](https://supabase.com/docs/guides/auth/auth-helpers/nextjs#server-actions) —` createServerActionClient`
- [Route Handlers](https://supabase.com/docs/guides/auth/auth-helpers/nextjs#route-handlers) —` createRouteHandlerClient`
- [Middleware](https://supabase.com/docs/guides/auth/auth-helpers/nextjs#middleware) —` createMiddlewareClient`


## Scaffolding a new Next.js app with Supabase#


[create-next-app](https://nextjs.org/docs/pages/api-reference/create-next-app) is one of the easiest way to get started with Next.js.


We've created a new template for` create-next-app` scaffolding Supabase projects:


`
_10


npx create-next-app -e with-supabase


`


This creates a new Next.js app configured with:


- Server-side cookie-based Auth
- TypeScript
- Tailwind CSS


This is the perfect starting point for any application built with Next.js and Supabase! 🚀


Check out the[/app/_examples](https://github.com/vercel/next.js/blob/canary/examples/with-supabase/app/_examples) folder for an example of creating a Supabase client in:


- [Client Components](https://github.com/vercel/next.js/blob/canary/examples/with-supabase/app/_examples/client-component/page.tsx)
- [Server Components](https://github.com/vercel/next.js/blob/canary/examples/with-supabase/app/_examples/server-component/page.tsx)
- [Route Handlers](https://github.com/vercel/next.js/blob/canary/examples/with-supabase/app/_examples/route-handler/route.ts)
- [Server Actions](https://github.com/vercel/next.js/blob/canary/examples/with-supabase/app/_examples/server-action/page.tsx)


## More integrations#


We've got plenty more in store for Next.js developers which we will be rolling out over the next few months. If you're looking for more integrations, or you want to build your own, we're also launching a new Supabase[Integrations Marketplace](https://supabase.com/blog/supabase-integrations-marketplace) .


## More Launch Week 8#


- [Supabase Local Dev: migrations, branching, and observability](https://supabase.com/blog/supabase-local-dev)
- [Hugging Face is now supported in Supabase](https://supabase.com/blog/hugging-face-supabase)
- [Launch Week 8](https://supabase.com/launch-week)
- [Coding the stars - an interactive constellation with Three.js and React Three Fiber](https://supabase.com/blog/interactive-constellation-threejs-react-three-fiber)
- [Why we'll stay remote](https://supabase.com/blog/why-supabase-remote)
- [Postgres Language Server](https://github.com/supabase/postgres_lsp)
