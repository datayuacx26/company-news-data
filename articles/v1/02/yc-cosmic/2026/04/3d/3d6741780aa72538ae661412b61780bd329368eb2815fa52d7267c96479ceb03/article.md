---
schema_version: "1.0.0"
document_id: "3d6741780aa72538ae661412b61780bd329368eb2815fa52d7267c96479ceb03"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-headless-cms-astro"
published_at: "2026-04-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T22:15:48.391228+00:00"
content_hash: "sha256:d9a40d7cb82063b85c4c458ce6bb41b649b9ef52b32f091ecd01e39683564fb5"
---

# Cosmic Headless CMS for Astro: Complete Tutorial with TypeScript SDK

Astro is the fastest-growing web framework for content-driven websites. Its islands architecture delivers near-zero JavaScript by default, making it ideal for blogs, docs sites, marketing pages, and anything where performance directly impacts SEO.


Cosmic is a headless CMS built around the same philosophy: content that works with any frontend, delivered fast from the edge. In this tutorial, you will connect Cosmic to an Astro v6 project from scratch, fetch content with the TypeScript SDK, set up dynamic routing, and understand the REST API patterns that make it all work.


**What you will build:** A blog with Cosmic as the content backend, Astro as the frontend, dynamic routing per post, and TypeScript throughout.


**Prerequisites:**


- Node.js 18+
- A Cosmic account (free at[cosmicjs.com](https://app.cosmicjs.com/signup) )
- Basic familiarity with TypeScript and Astro


---


## Step 1: Create a Cosmic Bucket


Sign up for Cosmic at[app.cosmicjs.com/signup](https://app.cosmicjs.com/signup) . No credit card required. On the free plan you get 1 Bucket, 1,000 Objects, and 3 AI agents.


Once signed in:


1. Click **Create New Bucket**
2. Name it (or anything you like)
3. Go to **Object Types** and create a new type called
4. Add these metafields:


- (Text) — the post headline
- (Markdown) — the post body
- (Textarea) — short summary
- (File, image only) — featured image
- (Date)


5. Create a few test blog posts with real content


Then, from your bucket dashboard:


- Go to **Settings > API Access**
- Copy your **Bucket Slug** and **Read Key**


---


## Step 2: Scaffold an Astro Project


```text


```


Select the following options when prompted:


- Template: **Blog** (or Empty)
- TypeScript: **Strict**
- Install dependencies: **Yes**


Then install the Cosmic TypeScript SDK:


```text


```


---


## Step 3: Configure Environment Variables


Create a file in your project root:


```text


```


For production deployments (Vercel, Netlify, etc.), add these as environment variables in your hosting dashboard.


**Never commit your Read Key to a public repository.**


---


## Step 4: Create a Cosmic Client


Create a utility file at :


```text


```


This creates a typed client you can import anywhere in your Astro project. The client handles authentication, error handling, and TypeScript inference automatically.


---


## Step 5: Define TypeScript Types


Create to type your content:


```text


```


This interface maps directly to the object shape returned by Cosmic's REST API and TypeScript SDK.


---


## Step 6: Fetch Blog Posts on the Index Page


Update :


```text


```


Key points:


- takes a query object. filters by object type.
- limits which fields are returned, reducing response size and improving performance.
- returns newest posts first. Prefix with for descending.
- caps the result set.


---


## Step 7: Dynamic Routing with


Create for individual post pages:


```text


```


**What's happening here:**


is Astro's static generation API. It runs at build time, fetches all posts from Cosmic, and generates a static HTML page for each. Your deployed site will have one pre-rendered HTML file per post. Zero server-side rendering overhead. Maximum performance.


The on Cosmic media objects gives you access to Imgix image transformations: append to serve WebP automatically at the right size.


---


## Step 8: Server-Side Rendering (Optional)


If you want dynamic content (pages that update without rebuilding), enable SSR in your Astro config:


```text


```


With SSR enabled, your page components run on every request. The Cosmic API call happens server-side, and you always get fresh content without a rebuild:


```text


```


fetches a single object by its properties. Combine with Astro's dynamic route and you have on-demand rendering with Cosmic as the backend.


---


## Step 9: Filtering and Querying with MongoDB-Style Operators


Cosmic's REST API supports MongoDB-style query operators for flexible filtering. These work identically in the TypeScript SDK:


```text


```


Supported operators: , , , , , , , . These match MongoDB query syntax, so any developer familiar with MongoDB or Mongoose already knows the pattern.


---


## Step 10: Image Optimization with Imgix


Every file uploaded to Cosmic is served through Imgix. The property on media objects gives you access to Imgix's transformation API:


```text


```


Common Imgix parameters for Astro projects:


- / : width and height in pixels
- : crops to the exact dimensions
- : serves WebP to browsers that support it, JPEG/PNG as fallback
- : applies lossy compression automatically
- : quality from 1-100 (85 is a good default)


---


## Step 11: REST API Direct Usage


For cases where you prefer direct HTTP calls over the SDK (e.g., edge functions, Deno environments), Cosmic's REST API is simple:


```text


```


The REST API base URL is . All responses are standard JSON. No special client required.


---


## Step 12: Deploy to Vercel or Netlify


For static sites (the default Astro behavior):


```text


```


For Vercel with SSR:


```text


```


Add to :


```text


```


Then push to GitHub and connect to Vercel. Set your environment variables (, ) in the Vercel dashboard under Project Settings > Environment Variables.


For Netlify:


```text


```


---


## Performance Tips for Cosmic + Astro


**Use always.** Only request the fields you need. A blog index page doesn't need the full markdown body of every post. Fetching only cuts response size significantly.


**Cache API responses at the edge.** Astro's directive (or ) builds static HTML at deploy time. For dynamic SSR pages, use headers or Astro's for fine-grained caching.


**Use Imgix transforms.** Never serve raw uploaded images. Always append at minimum to your values. This alone can reduce image payload by 40-60%.


**Limit your result set.** Add to every query. Unbounded queries on large buckets increase latency.


---


## Common Patterns


### Blog with Categories


```text


```


### Related Posts


```text


```


### Search by Title


```text


```


---


## What to Build Next


Now that your Astro site is fetching content from Cosmic:


1. **Add a newsletter signup form** that stores leads as Objects in Cosmic via the Write API
2. **Set up a Cosmic Content Agent** to draft blog posts on a schedule and publish them automatically
3. **Use the Cosmic MCP Server** to manage your content model from Cursor or Claude
4. **Add Cosmic's AI image generation** to auto-generate cover images for new posts


---


## Summary


Concept What to Use


Install SDK


Create client


Fetch many


Fetch one


Static routes +


SSR routes in component frontmatter


Images + Imgix params


Filtering MongoDB-style operators (, , )


REST API


Cosmic's REST API and TypeScript SDK are designed to stay out of your way. No proprietary query language. No lock-in. Just fast, typed, edge-delivered content for your Astro site.


**Ready to build?**


- [Create a free Cosmic account](https://app.cosmicjs.com/signup)
- [Read the full REST API docs](https://cosmicjs.com/docs/api)
- [Browse Astro + Cosmic templates](https://cosmicjs.com/marketplace)
- [Book a demo with Tony](https://calendly.com/tonyspiro/cosmic-intro)
