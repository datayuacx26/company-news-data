---
schema_version: "1.0.0"
document_id: "cb82ed2033606a31e30827b37c5ec60b1566e410a42e55fc96e32bd69d15f7c4"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/headless-cms-for-tanstack-router"
published_at: "2026-04-18T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T20:52:31.063760+00:00"
content_hash: "sha256:c957dd65f4a12fe5f3e3aa4e742d933df39c58ef55eaefc3fe29f9e73a73a0d8"
---

# Headless CMS for TanStack Router: Type-Safe Content with Cosmic

# Headless CMS for TanStack Router: Type-Safe Content with Cosmic


You're building a React app with TanStack Router. You've got file-based routing, fully type-safe components, and route loaders that eliminate waterfall fetches. The routing problem is solved. Now you need content.


You could hard-code it. You could drop markdown files into your repo. Or you could wire up a headless CMS that gives your whole team a clean editing interface while keeping your frontend completely decoupled.


That's exactly what this guide covers: how to use[Cosmic](https://www.cosmicjs.com/) as the content backend for a TanStack Router application. You'll get:


- Content fetched in route functions (no waterfall, no flash of empty content)
- Full TypeScript types from the Cosmic JavaScript SDK
- Nested dynamic routes for blog posts, product pages, or any content with a slug
- A working pattern you can drop into any existing TanStack Router project


---


## Why TanStack Router + Cosmic is a great pairing


TanStack Router is the most widely adopted piece of the TanStack suite. Millions of React developers use it standalone, outside of TanStack Start. Its killer feature isn't just routing — it's **type safety end-to-end** : type-safe params, type-safe search params, type-safe loaders, and type-safe components that catch broken routes at compile time.


Cosmic is a headless CMS with a[JavaScript/TypeScript SDK](https://www.npmjs.com/package/@cosmicjs/sdk) that returns typed content objects. When you combine TanStack Router's typed loaders with Cosmic's typed SDK responses, you get a content pipeline where TypeScript catches schema mismatches before they hit production.


Neither tool locks you in. TanStack Router works with Vite, Webpack, Rspack, or any build tool. Cosmic serves content over a REST API to any frontend, any framework, any deployment target.


---


## Prerequisites


- A Cosmic account ([free tier available](https://app.cosmicjs.com/signup) )
- A TanStack Router project (we'll scaffold one below)
- Node.js 18+


---


## Step 1: Scaffold a TanStack Router project


The fastest way to start is with the official Vite + TanStack Router template:


```text


```


This gives you file-based routing out of the box. Routes are defined by files inside , and TanStack Router generates the route tree automatically.


---


## Step 2: Install the Cosmic SDK


```text


```


Create a file at to initialize the client:


```text


```


Add your credentials to :


```text


```


You can find both values in your Cosmic dashboard under **Bucket > Settings > API Keys** .


---


## Step 3: Create your content model in Cosmic


In your Cosmic dashboard, create an Object Type called **Posts** with the following metafields:


Field Type Key


Cover Image File


Excerpt Textarea


Body Markdown


Published Date Date


Add a few test posts with titles, slugs, and body content. You're ready to fetch.


---


## Step 4: Define TypeScript types for your content


Create :


```text


```


These types will flow through your route loaders, giving you autocomplete and error checking on every field.


---


## Step 5: Fetch content in a route loader


This is where TanStack Router really shines. The function runs before the component renders, so your content is ready the moment the route activates — no loading spinners on initial render.


Create the blog index route at :


```text


```


Notice the component: TanStack Router knows the shape of at compile time. If you mistype the param name, you'll get a TypeScript error before you ever open a browser.


---


## Step 6: Nested dynamic route for individual posts


Now create the dynamic post route at :


```text


```


The value is fully typed — TanStack Router infers it from the filename . No manual type casting, no hacks.


The throw is also typed. TanStack Router catches it and renders the you defined on the route, keeping your 404 handling clean and co-located with the route itself.


---


## Step 7: Add route-level error boundaries


Cosmic API calls can fail. Wrap your routes with an to handle it gracefully:


```text


```


TanStack Router calls if the loader throws (and it isn't a ). Your app stays running; only the affected route shows the error state.


---


## Step 8: Preloading on hover for instant navigation


TanStack Router supports route preloading — it can fire the loader the moment a user hovers over a link, so the data is cached by the time they click.


Enable it globally in :


```text


```


With , hovering over a prefetches the Cosmic content for that post. Navigation feels instant. This is zero additional code in your route files — it's a single router config option.


---


## Step 9: Search params for filtering and pagination


TanStack Router's search params are typed and validated just like route params. Here's a pattern for paginated Cosmic content:


```text


```


The function tells TanStack Router to re-run the loader when or changes. The search params are validated with Zod, so gracefully falls back to instead of crashing.


---


## Putting it all together: the file structure


```text


```


This is a minimal, production-ready structure. Each route file owns its data fetching, its loading state, and its error handling. No global state, no prop drilling, no context providers for content.


---


## Performance considerations


**Images via Imgix:** Cosmic serves all media through Imgix. Append transform params to your image URLs for automatic optimization:


```text


```


**Stale-while-revalidate:** Set to match your content update frequency. For a blog with infrequent updates, 60 seconds is reasonable. For real-time content, set it to .


**Parallel loaders:** TanStack Router runs sibling route loaders in parallel. If your layout route and page route both need Cosmic data, they fetch simultaneously with no code changes needed.


---


## What to build next


This tutorial covers the core pattern: typed loaders, dynamic slugs, nested routes, and preloading. Here's where to take it from here:


- **Add TanStack Start** for SSR and server-side data fetching. The loaders you wrote here translate directly. Read[Headless CMS for TanStack Start: Build a Blog with Cosmic](https://www.cosmicjs.com/blog/headless-cms-tanstack-start) for the full walkthrough.
- **Add TanStack Query** for client-side caching, background revalidation, and optimistic UI. TanStack Router integrates with TanStack Query via the External Data Loading guide.
- **Add Cosmic localization** for multi-language content — the SDK supports locale-scoped queries with one extra parameter.
- **Deploy to Vercel or Netlify** — both support Vite apps with zero configuration.


---


## Start building


Cosmic's free plan includes everything you need to get started: 1 bucket, 2 team members, and 1,000 objects. No credit card required.


[Sign up for Cosmic free](https://app.cosmicjs.com/signup) and have your first route loader fetching real CMS content in under 10 minutes.


Want a personalized walkthrough of how Cosmic fits your stack?[Book a demo with Tony](https://calendly.com/tonyspiro/cosmic-intro) .


---


*Canonical URL:[https://www.cosmicjs.com/blog/headless-cms-for-tanstack-router](https://www.cosmicjs.com/blog/headless-cms-for-tanstack-router)*
