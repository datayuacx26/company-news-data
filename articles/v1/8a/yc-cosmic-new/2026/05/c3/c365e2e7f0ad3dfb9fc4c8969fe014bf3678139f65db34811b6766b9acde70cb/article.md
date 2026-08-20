---
schema_version: "1.0.0"
document_id: "c365e2e7f0ad3dfb9fc4c8969fe014bf3678139f65db34811b6766b9acde70cb"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/build-a-blog-with-nextjs-16-and-cosmic"
published_at: "2026-05-04T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T22:15:27.754583+00:00"
content_hash: "sha256:1d865f4411d77af8bf7df4351a906480a573118feb25cdab476717500e0e7b94"
---

# Build a Blog with Next.js 16 and Cosmic CMS

Next.js 16 shipped in October 2025 with Turbopack stable, React Compiler support, Cache Components, and a redesigned caching model built around . If you are starting a new blog or content site today, this is the stack to use.


This tutorial walks you through building a production-ready blog from scratch using Next.js 16 and[Cosmic](https://www.cosmicjs.com/) as your headless CMS. You will set up a content model in Cosmic, fetch posts with the TypeScript SDK, render them in Next.js 16 App Router server components, and deploy the whole thing to Vercel. The estimated time is 30 minutes.


**What you will build:** A blog with a homepage listing posts, individual post pages, and tag filtering — all powered by Cosmic's REST API and the .


---


## Prerequisites


- Node.js 20+
- A free[Cosmic account](https://app.cosmicjs.com/signup) (no credit card required)
- A Vercel account for deployment (free tier is fine)
- Basic familiarity with React and TypeScript


---


## Step 1: Create Your Cosmic Bucket


Log into[Cosmic](https://app.cosmicjs.com/signup) and create a new bucket. Give it a name like .


Once inside your bucket, you will see the dashboard. Cosmic organizes content into **Object Types** (your content models) and **Objects** (individual content entries). For this tutorial, you need one Object Type: .


### Set Up the Blog Post Content Model


In your bucket, go to **Object Types** and create a new type called . Add the following metafields:


Field Type Notes


Text Built-in, always present


Text Built-in, auto-generated


Markdown Long-form post body


Textarea Short summary, 160 chars max


File (image) Featured image


Date Post date


Text Comma-separated tags


Save the Object Type. Now create 2–3 sample blog posts so you have content to fetch. Publish them.


### Get Your API Keys


In your Cosmic dashboard, go to **Bucket Settings > API Keys** . You need:


- **Bucket Slug** — e.g.,
- **Read Key** — used for fetching published content on the frontend


Keep these handy. You will add them to your in a moment.


---


## Step 2: Scaffold a Next.js 16 App


Open your terminal and run:


```text


```


When prompted:


- Use the **App Router** (say yes)
- TypeScript: yes
- Tailwind CSS: yes (optional, but the examples below use it)


Next.js 16 uses the App Router by default. The directory is your routing root. All components in this directory are **React Server Components** by default, which means they can fetch data directly on the server with no client-side waterfall.


### Install the Cosmic TypeScript SDK


```text


```


This is the official SDK. Do not use the older package.


### Configure Environment Variables


Create a file in the project root:


```text


```


Add to your if it is not already there.


---


## Step 3: Create the Cosmic Client


Create a file at :


```text


```


That is your entire data layer. The function returns a typed client scoped to your bucket. All fetch methods are available on it.


---


## Step 4: Define Your TypeScript Types


Create :


```text


```


This mirrors the shape Cosmic returns for your blog post objects.


---


## Step 5: Fetch Posts from Cosmic


Create :


```text


```


A few things to note:


- limits the fields returned, keeping API responses lean.
- sorts newest first. The prefix means descending.
- ensures you only fetch published posts (not drafts).
- The SDK is fully typed. Your editor will autocomplete method chains.


---


## Step 6: Build the Homepage — Post Listing


Replace the contents of :


```text


```


**Key Next.js 16 patterns here:**


- enables Incremental Static Regeneration (ISR). The page is statically generated at build time and refreshed every 60 seconds in the background. No need for or .
- The component is and s data directly. No , no loading states on the server.
- from handles optimization automatically. The query appended to Cosmic's imgix URL tells imgix to resize and optimize on the fly.


---


## Step 7: Build the Individual Post Page


Create :


```text


```


Install for rendering your Markdown content:


```text


```


**Important Next.js 16 note:** In Next.js 16, in dynamic route pages is a . You must before accessing its properties. This is a breaking change from Next.js 14. The tool can migrate your existing code automatically.


tells Next.js which slugs to pre-render at build time. Any slug not in the list will be rendered on-demand (ISR fallback).


---


## Step 8: On-Demand Revalidation (Webhook)


When an editor publishes or updates a post in Cosmic, you want your site to reflect those changes quickly. Next.js 16 supports on-demand cache revalidation via its Cache API.


Create :


```text


```


Add to your .


Then, in Cosmic, go to **Bucket Settings > Webhooks** and add a webhook pointing to . Send the secret in a header: . Trigger it on events.


Now when you publish a post in Cosmic, your Next.js site will revalidate within seconds.


---


## Step 9: Deploy to Vercel


Vercel is the easiest way to deploy Next.js apps. If you haven't already:


```text


```


Or push your repo to GitHub and import it at[vercel.com](https://vercel.com/) .


**Add your environment variables in Vercel:**


1. Go to your project in the Vercel dashboard.
2. Open **Settings > Environment Variables** .
3. Add:


-
-
-


Trigger a new deployment. Vercel will build your app, run , and pre-render all your blog posts as static HTML. Your blog is now live.


---


## Step 10: Preview Mode for Drafts (Bonus)


Cosmic supports previewing draft content before publishing. To enable this, set on the Cosmic client:


```text


```


Wire this up to a Next.js Draft Mode route handler to give your editors a live preview experience.


---


## What You Built


Here is what you have at this point:


- A Next.js 16 blog with App Router and server components
- Content managed in Cosmic (no database required)
- Static generation with ISR for fast page loads
- On-demand revalidation via webhooks
- Automatic image optimization via Cosmic's imgix CDN
- Deployed to Vercel in minutes


The full source for this tutorial is structured to extend easily. Add a tag filter, an author page, or a search endpoint using Cosmic's REST API — the SDK supports filtering, sorting, and full-text search out of the box.


---


## Why Cosmic for Next.js 16


Cosmic pairs well with Next.js 16 for a few concrete reasons:


**Sub-100ms API responses.** Cosmic's CDN caches your content globally. Your server components fetch fast, which keeps your Time to First Byte (TTFB) low even at high traffic.


**Structured content model.** You define the schema. No rigid blog templates. Your Object Types map cleanly to TypeScript interfaces, which makes type-safe data fetching straightforward.


**No backend to maintain.** Cosmic handles hosting, scaling, and backups. Your team writes content; your Next.js app delivers it.


**Free tier to start.** The[free plan](https://www.cosmicjs.com/pricing) includes 1 bucket, 1,000 objects, and 100K cached API requests per month. No credit card required. You can go from this tutorial to a live blog without spending anything.


---


## Next Steps


- [Sign up for Cosmic free](https://app.cosmicjs.com/signup) and scaffold your bucket
- Check the[Cosmic documentation](https://www.cosmicjs.com/docs) for advanced SDK usage
- Browse[Cosmic's community projects](https://www.cosmicjs.com/community/projects) for a head start
- Want a guided walkthrough?[Book a demo with Tony](https://calendly.com/tonyspiro/cosmic-intro)


---


*Ready to build?[Start for free — no credit card required.](https://app.cosmicjs.com/signup)*
