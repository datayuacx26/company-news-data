---
schema_version: "1.0.0"
document_id: "2aeb9a5e2c6ae11ab954c8491dd0bb8a71475550ef53cb706cc4eb21a41a5a8a"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/astro-cosmic-cms-static-site"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:b8fe7fe5fa0263704219c57f1f481abe8c04c773b2dcd55aebc6bc4633dc72c5"
---

# Building a Static Site with Astro and Cosmic CMS

# Building a Static Site with Astro and Cosmic CMS


Astro's HTML-first philosophy is having a moment. A[post on Hacker News](https://news.ycombinator.com/item?id=48475483) this week about doubling user growth by going HTML-first cracked the top 10 with 383 points — and the replies were full of developers reconsidering their stacks.


The short version: ship less JavaScript, get faster sites, keep more visitors. Astro is purpose-built for exactly this. And when you pair it with Cosmic as your content layer, you get a stack that is fast to build, fast to run, and easy for non-developers to manage content without touching code.


This tutorial walks through building a production-ready blog with Astro and Cosmic from scratch.


## Why Astro + Cosmic


Astro generates zero-JavaScript HTML by default. Every page is a static file until you opt into interactivity with[islands](https://docs.astro.build/en/concepts/islands/) . That means:


- Lighthouse scores in the high 90s out of the box
- No hydration overhead for content pages
- Content editors can update copy in Cosmic without a developer involved


Cosmic delivers content via REST API and the . Your Astro build fetches content at build time, bakes it into static HTML, and serves it from a CDN. Fast, structured, and zero runtime CMS overhead.


## Prerequisites


- Node.js 18+
- A free Cosmic account ([sign up here](https://app.cosmicjs.com/signup) )
- Familiarity with TypeScript basics


## Step 1: Create Your Cosmic Bucket


Log in to[Cosmic](https://app.cosmicjs.com/) and create a new bucket. Then create a Object Type with these metafields:


- (text)
- (text, unique)
- (date)
- (textarea)
- (markdown)
- (file, image)


Add a few sample posts so you have content to render.


## Step 2: Scaffold the Astro Project


```text


```


Choose the "Empty" starter when prompted. We'll wire up everything manually so you understand each piece.


## Step 3: Initialize the Cosmic Client


Create :


```text


```


Add your credentials to :


```text


```


You'll find both values in your Cosmic bucket under *Settings > API Keys* .


## Step 4: Fetch Posts at Build Time


Create :


```text


```


## Step 5: Generate Individual Post Pages


Create for dynamic routing:


```text


```


Astro's runs at build time, fetches all post slugs from Cosmic, and generates one HTML file per post. Zero runtime API calls.


## Step 6: Handle Images with imgix


Cosmic stores all media on[imgix](https://imgix.com/) , which means you get URL-based image transformations for free. Append query params to any image URL:


```text


```


No image processing pipeline needed. imgix handles it at the CDN edge.


## Step 7: Deploy to Vercel


```text


```


Add your environment variables in the Vercel dashboard under *Settings > Environment Variables* :


-
-


Set up a deploy webhook in Cosmic ( *Settings > Webhooks* ) so every content publish triggers a new Vercel build. Your editors publish in Cosmic, the site rebuilds automatically — no developer involvement required.


## Step 8: Enable Incremental Builds (Optional)


For larger sites, Astro's[content collections](https://docs.astro.build/en/guides/content-collections/) combined with Vercel's[ISR](https://vercel.com/docs/incremental-static-regeneration) let you rebuild only changed pages. Configure in :


```text


```


Then mark individual pages for server-side rendering when you need fresh data, while keeping everything else static.


## Performance Baseline


A default Astro + Cosmic blog with no optimization effort typically scores:


- Performance: 95-100
- Accessibility: 90+
- Best Practices: 100
- SEO: 90+


The main lever is images. Use imgix transforms and the attribute on below-the-fold images and you'll stay in the green across the board.


## Adding an AI Agent (Optional)


Once your Cosmic bucket has content, you can add a Cosmic AI agent to manage it. From the Cosmic dashboard, connect an agent to your bucket — it can draft new posts, update metadata, generate featured images, and publish on a schedule. Your Astro site rebuilds automatically via webhook every time the agent publishes.


This is what the HTML-first HN crowd is discovering: a static frontend plus an API-first CMS is the right architecture for AI-assisted content workflows. The agent writes to the CMS API. The static site rebuilds. No CMS logic in your frontend, no CMS vendor in your runtime.


## What's Next


- Add a search index with[Pagefind](https://pagefind.app/) (runs at build time, zero runtime JS)
- Add RSS with
- Add a sitemap with
- Add an MCP server so AI assistants like Claude and Cursor can read and write your Cosmic content directly


## Get Started


Free Cosmic plan includes 1 bucket, 1,000 objects, and 2 team members — enough to build and launch a full blog.


<a href="https://app.cosmicjs.com/signup">Start building for free</a> or <a href="https://calendly.com/tonyspiro/cosmic-intro">book a demo with Tony</a> if you want to talk through your architecture.


---


*Questions? Join the[Cosmic Discord](https://discord.gg/MSCwQ7D6Mg) or check the[Astro + Cosmic docs](https://www.cosmicjs.com/docs) .*
