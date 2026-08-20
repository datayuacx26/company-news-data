---
schema_version: "1.0.0"
document_id: "769c8391822b4ea5a4f423e01d8a747981daf16367e25709bbc1ed4747699faf"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/headless-cms-tanstack-start"
published_at: "2026-04-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T20:52:31.063760+00:00"
content_hash: "sha256:6b328a8290a7aa1972ad2b15b490cb3f8a5568faf1cf22ba18dc10b4844dde00"
---

# Headless CMS for TanStack Start: Build a Blog with Cosmic

You want SSR, fast routing, and a CMS your whole team can edit without touching code. Here's how to build that stack in under an hour.


TanStack Start pairs naturally with Cosmic: Start handles full-document SSR, streaming, and type-safe routing via Vite and TanStack Router, while Cosmic gives you a structured, API-first content layer your editors can use without a developer in the room. The result is a modern content stack that's fast to build, easy to maintain, and genuinely pleasant to work with.


This tutorial walks through building a content-driven TanStack Start blog powered by Cosmic. You'll fetch posts from Cosmic using the JavaScript SDK, render them with server functions, and have a working SSR blog in under 30 minutes.


## Prerequisites


- Node.js 18 or later
- A free[Cosmic account](https://app.cosmicjs.com/signup) with a bucket set up
- Basic familiarity with React and TypeScript


## 1. Create a TanStack Start Project


The fastest way to scaffold a new project is with the TanStack CLI:


```text


```


This gives you a working TanStack Start app with file-based routing, SSR enabled, and Vite as the bundler.


## 2. Install the Cosmic SDK


```text


```


## 3. Configure Your Environment Variables


Create a file at the root of your project:


```text


```


You can find both values in your Cosmic dashboard under *Bucket > Settings > API Keys* .


> TanStack Start uses Vite under the hood. Server-side environment variables are accessed via inside server functions. For client-side access, prefix with — but keep your read key on the server only.


## 4. Create a Cosmic Client


Add a shared client file at :


```text


```


## 5. Fetch Posts with a Server Function


TanStack Start's server functions run exclusively on the server, making them the right place to call external APIs and keep keys out of the client bundle.


Create :


```text


```


## 6. Create the Blog Index Route


TanStack Start uses file-based routing. Create :


```text


```


## 7. Create the Post Detail Route


Install for safe, component-based markdown rendering:


```text


```


Create :


```text


```


renders markdown as React components, so there's no raw HTML injection and no XSS risk. It also gives you a clean hook to customize rendering via the prop if you need custom heading styles, code blocks, or link behavior down the line.


## 8. Run the Dev Server


```text


```


Open and you should see your Cosmic posts rendered server-side via TanStack Start.


## How It Works


- *Server functions* () run on the server only. Cosmic SDK calls stay server-side, so your read key never leaks to the browser.
- *Route loaders* call server functions and pass data down to components. TanStack Router handles caching, deduplication, and hydration automatically.
- *File-based routing* keeps the project organized. Each route in maps to a URL segment.
- *Vite* handles bundling and HMR, so development is fast.


## Deploying to Vercel


TanStack Start supports multiple deployment targets. For Vercel:


```text


```


Then update :


```text


```


Push to GitHub, import the repo in Vercel, and add your and environment variables in the Vercel dashboard. That's it.


## What to Build Next


Once the basics are working, here are four high-value extensions that are specific to the TanStack Start and Cosmic combination:


**Add localization with Cosmic's Localization add-on.** Cosmic has built-in localization support at the object level. Enable it in your bucket settings, add locale variants to your blog post objects, and pass a parameter in your SDK queries. Pair that with TanStack Router's search params to build locale-aware routes without a separate i18n library.


**Trigger Vercel rebuilds with Cosmic Webhooks.** In your Cosmic bucket, go to *Settings > Webhooks* and add a event webhook pointing at your Vercel Deploy Hook URL. Every time an editor publishes a post in Cosmic, Vercel kicks off a fresh build automatically. No polling, no manual deploys.


**Manage content from Slack with a Cosmic Team Agent.** Cosmic's AI agents let you create, update, and query content objects directly from a Slack channel. Set up a Team Agent connected to your bucket and your editors can say "publish the TanStack post" or "create a draft about X" without ever opening the dashboard. It's particularly useful for teams that live in Slack.


**Add full-text search with the Cosmic REST API.** The Cosmic REST API supports a parameter for full-text object search. Wire up a TanStack Start server function that accepts a search term and returns matching posts, then connect it to a search input on the index route for instant, server-rendered results.


## Get Started


You can get a Cosmic account for free at[cosmicjs.com](https://www.cosmicjs.com/) . The free plan includes 1 bucket, 1,000 objects, and 2 team members. No credit card required.


Ready to see how Cosmic fits your team's workflow?[Book a quick intro with Tony](https://calendly.com/tonyspiro/cosmic-intro) and we'll walk you through it.


Have questions or want to share what you've built? Drop into the[Cosmic community](https://www.cosmicjs.com/community) or open a GitHub issue on the[Cosmic SDK repo](https://github.com/cosmicjs/cosmic-sdk-js) .
