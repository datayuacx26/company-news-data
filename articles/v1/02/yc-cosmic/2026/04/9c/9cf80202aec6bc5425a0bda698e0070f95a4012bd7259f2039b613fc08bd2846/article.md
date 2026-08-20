---
schema_version: "1.0.0"
document_id: "9cf80202aec6bc5425a0bda698e0070f95a4012bd7259f2039b613fc08bd2846"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/nuxt-3-headless-cms-tutorial"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T22:15:53.113233+00:00"
content_hash: "sha256:f5df4a1d573bd397d1602f0f102300addfa58ae60615fd09010c992b6cc828f6"
---

# How to Build a Nuxt 3 App with a Headless CMS

# How to Build a Nuxt 3 App with a Headless CMS


Nuxt 3 is one of the best full-stack frameworks available today. It combines Vue 3's Composition API with powerful server-side rendering, static site generation, and a first-class developer experience. Pair it with a headless CMS, and you get all the flexibility of a modern framework with structured, API-delivered content your whole team can manage.


In this tutorial, we'll walk through connecting a Nuxt 3 app to[Cosmic](https://cosmicjs.com/) , an AI-powered headless CMS with a REST API, TypeScript SDK, and built-in AI Agents. By the end, you'll have a working blog that fetches content from Cosmic and renders it in Nuxt 3.


**What we'll cover:**


- Why Nuxt 3 and a headless CMS work so well together
- Installing and configuring Nuxt 3
- Installing the Cosmic SDK
- Modeling content in Cosmic
- Fetching content with and
- Rendering content in Nuxt pages
- Deploying to Vercel or Netlify


---


## Why Nuxt 3 + Headless CMS Is a Great Combo


Nuxt 3 gives you flexibility in how you render content: SSR, SSG, hybrid, or ISR. A headless CMS like Cosmic gives you structured content delivered over a REST API, completely decoupled from your frontend.


This combination means:


- **Developers** control the rendering layer and choose the right strategy per route
- **Content editors** update content in the CMS without touching the codebase
- **Performance** stays high because content is fetched at the right time (build-time for static, server-side for dynamic)
- **Flexibility** is preserved because the same Cosmic API can feed your Nuxt site, a mobile app, a dashboard, or any other channel


Cosmic's sub-100ms API responses pair especially well with Nuxt's server-side data fetching patterns.


---


## Step 1: Set Up a Nuxt 3 Project


Create a new Nuxt 3 app:


```text


```


Verify it runs:


```text


```


You should see the Nuxt 3 welcome page at .


---


## Step 2: Install the Cosmic SDK


Install the official Cosmic JavaScript SDK:


```text


```


The SDK is TypeScript-first and works in any Node.js environment, including Nuxt's server routes and composables.


---


## Step 3: Create a Cosmic Account and Model Your Content


If you don't have a Cosmic account,[sign up for free](https://app.cosmicjs.com/signup) — no credit card required.


Once you're in:


1.


**Create a Bucket** — a Bucket is your project's content container. Give it a name like "My Nuxt Blog."


2.


**Create an Object Type** called with these metafields:


- (file) — featured image
- (markdown) — post body
- (textarea) — short teaser for listings
- (text) — author name


3.


**Add a few test posts** so you have content to fetch.


4.


**Grab your API keys** from Settings > API Keys:


-
-


Cosmic's content modeling is done entirely in the dashboard. No config files, no schema migrations, no code deploys required. Add a field and it's immediately available in the API.


---


## Step 4: Configure Environment Variables


Create a file in your project root:


```text


```


Update to expose these via :


```text


```


---


## Step 5: Create a Cosmic Composable


Create a reusable composable for your Cosmic client:


```text


```


---


## Step 6: Fetch Content with


Create the blog index page:


```text


```


Create the server API route that fetches from Cosmic:


```text


```


This pattern keeps your API keys server-side and gives you a clean separation between your Nuxt frontend and your data fetching layer.


---


## Step 7: Fetch a Single Post


For individual post pages, use Nuxt's dynamic routes:


```text


```


Create the server route for a single post:


```text


```


---


## Step 8: Using as an Alternative


If you prefer to call the Cosmic REST API directly without the SDK, works just as cleanly:


```text


```


Note: When using with public runtime config, your read key will be visible in the client. This is acceptable for read-only content on public sites, but for write operations or sensitive data always use server routes.


---


## Step 9: Render and Style Your Content


For markdown content stored in Cosmic, render it server-side using a package like :


```text


```


```text


```


For images, use Cosmic's imgix integration to serve optimized images. Append query parameters to any imgix URL:


```text


```


---


## Step 10: Deploy to Vercel or Netlify


### Deploying to Vercel


```text


```


In the Vercel dashboard, add your environment variables:


-
-


Vercel auto-detects Nuxt 3 and deploys with the correct preset.


### Deploying to Netlify


For Netlify, set the build command and output directory:


```text


```


Add your environment variables in the Netlify dashboard under Site Settings > Environment Variables.


Both platforms support Nuxt 3's hybrid rendering, so you can mix static and server-rendered routes in the same project.


---


## What to Build Next


You now have a fully functional Nuxt 3 blog powered by Cosmic. Here's what to explore next:


- **Add Cosmic AI Agents** to auto-generate blog drafts on a schedule.[Explore Cosmic AI](https://www.cosmicjs.com/ai)
- **Set up webhooks** to trigger Nuxt cache revalidation when content is updated in Cosmic
- **Add localization** with Cosmic's Localization add-on for multi-language content
- **Explore Cosmic's image API** — every imgix URL supports resize, crop, format conversion, and compression via URL params


---


## Summary


Nuxt 3's server-side rendering and Cosmic's headless CMS are a natural fit. The Cosmic REST API and TypeScript SDK plug cleanly into , , and Nuxt server routes. You get structured content, fast API responses, and a content model your whole team can manage, without any infrastructure overhead.


**Ready to start?**[Create your free Cosmic account](https://app.cosmicjs.com/signup) and have content flowing into your Nuxt 3 app in minutes.


Want a guided walkthrough?[Book a demo with Tony](https://calendly.com/tonyspiro/cosmic-intro) .


---


## Related Resources


- [Nuxt Headless CMS Landing Page](https://www.cosmicjs.com/nuxt-cms)
- [Headless CMS for Vue](https://www.cosmicjs.com/headless-cms-for-vue)
- [Headless CMS for React](https://www.cosmicjs.com/react-cms)
- [Cosmic AI Features](https://www.cosmicjs.com/ai)
- [Best Headless CMS in 2026](https://www.cosmicjs.com/blog/best-headless-cms-2026)
- [Cosmic Docs](https://cosmicjs.com/docs)
