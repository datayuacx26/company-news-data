---
schema_version: "1.0.0"
document_id: "820355e7bae0d274588bbdaa0b9c9d61a88476bb2037d1dec081640a2d603560"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/build-react-blog-nextjs-cosmic"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T22:13:07.113521+00:00"
content_hash: "sha256:abfc98176d5b0d7708145fb71e9953f52edf2a57bc3be29cfacb4058824d97a9"
---

# How to Build a React Blog with Next.js and Cosmic

Building a blog with React and Next.js is one of the most common use cases for modern web development. But where you store and manage your content matters just as much as the frontend you choose. In this tutorial, you'll learn how to build a fast, production-ready React blog using the[Next.js App Router](https://nextjs.org/docs/app) and[Cosmic](https://www.cosmicjs.com/) as your headless CMS.


Want to skip ahead? Grab the free[Simple React Blog template](https://cosmicjs.com/templates/simple-react-blog) and deploy in minutes.


---


## Why Use a Headless CMS for a React Blog?


Managing blog content directly in your codebase works fine for a handful of posts, but it breaks down fast. Markdown files scattered across a repo, no visual editing interface, and every content update requiring a developer and a deployment are all pain points that accumulate quickly.


A headless CMS solves this by separating your content layer from your presentation layer. You manage posts, authors, and categories in a structured dashboard. Your Next.js app fetches that content via the REST API at build time or on demand. Non-technical editors can publish without touching code, and developers stay focused on the frontend. Cosmic gives you a clean API, a TypeScript SDK, a generous free tier, and zero infrastructure to manage. It's purpose-built for exactly this kind of project.


---


## Prerequisites


Before you start, you'll need:


- Node.js 18 or later
- A free[Cosmic account](https://app.cosmicjs.com/signup)
- Basic familiarity with React and TypeScript


---


## Step 1: Set Up Your Cosmic Bucket and Content Model


After signing up for a free Cosmic account, create a new bucket. You can start from the[Simple React Blog template](https://cosmicjs.com/templates/simple-react-blog) to get a pre-configured bucket with sample content and a ready-to-deploy Next.js app, or follow along manually.


### Creating a Blog Posts Object Type


In your Cosmic dashboard, navigate to **Object Types** and create a new type called (slug: ). Add the following metafields:


Field Type Key


Title Text (built-in)


Slug Text (built-in)


Cover Image File (image)


Excerpt Textarea


Content Markdown


Published Date Date


Author Text


Once saved, go ahead and create a few sample posts so you have content to query during development.


### Get Your API Credentials


Go to **Settings > API Keys** in your bucket. You'll need:


- **Bucket Slug** (e.g. )
- **Read Key** (for public content fetching)


Save these as environment variables. You'll use them in the next step.


---


## Step 2: Create a New Next.js App


Scaffold a new Next.js project with TypeScript:


```text


```


Install the Cosmic SDK:


```text


```


Create a file at the root of your project:


```text


```


---


## Step 3: Initialize the Cosmic Client


Create a shared Cosmic client so you're not re-initializing it in every file. Add a new file at :


```text


```


The package is fully typed, so you get autocomplete and type safety throughout your app.


---


## Step 4: Define Your TypeScript Types


Create a file to keep things clean:


```text


```


---


## Step 5: Fetch Posts from the Cosmic REST API


With the SDK initialized, fetching content is straightforward. The SDK wraps the Cosmic REST API with a clean, promise-based interface.


### Fetch All Posts (Blog Index)


```text


```


### Fetch a Single Post by Slug


```text


```


Note: The method limits the fields returned, which keeps payloads lean and improves performance.


---


## Step 6: Build the Blog with Next.js App Router


Next.js 13+ uses the App Router with React Server Components by default. This is a great fit for a Cosmic-powered blog: data fetching happens on the server, HTML is pre-rendered, and pages are fast.


### Blog Index Page


```text


```


### Dynamic Post Page


```text


```


pre-renders all post pages at build time, which means your blog is fast and SEO-friendly out of the box.


---


## Step 7: Optimize Images with Imgix


Cosmic stores all media through[Imgix](https://imgix.com/) , a powerful image CDN. You can append transformation parameters directly to any media URL:


```text


```


Common params:


- sets width
- serves WebP to supported browsers
- reduces file size automatically
- crops to exact dimensions


No additional image optimization setup needed.


---


## Step 8: Deploy to Vercel


Vercel is the natural home for Next.js apps. Deploying is a one-command operation once you've pushed your project to GitHub.


1. Push your project to a GitHub repository
2. Go to[vercel.com](https://vercel.com/) and import the repo
3. Add your environment variables under **Settings > Environment Variables** :


-
-


4. Click **Deploy**


Vercel will detect Next.js automatically, build your app, and give you a live URL in under a minute.


### Enable On-Demand Revalidation (Optional)


For a production blog, you'll want new Cosmic posts to appear without a full redeploy. Add a revalidation route:


```text


```


Then configure a Cosmic webhook to call this endpoint whenever a post is published. In Cosmic, go to **Settings > Webhooks** and add your Vercel deployment URL with the path.


---


## What You've Built


Here's what your React blog now has:


- A Next.js App Router project with TypeScript
- A Cosmic bucket with a structured content model
- Server-side data fetching via the
- Static pre-rendering with
- Imgix-powered image optimization
- Live on Vercel with optional on-demand revalidation


Your editors can log into the Cosmic dashboard and publish new posts without touching your codebase. Your readers get a fast, statically rendered experience. And your SEO improves because every post is pre-rendered HTML at build time.


---


## Start with the Free Template


Don't want to build from scratch? The[Simple React Blog template](https://cosmicjs.com/templates/simple-react-blog) gives you everything in this tutorial, already wired up and ready to deploy with one click.


**[Get the free React blog template →](https://cosmicjs.com/templates/simple-react-blog)**


Or jump straight in:


**[Start building free →](https://app.cosmicjs.com/signup)**


Have questions or want to talk through your project?[Book a quick intro call](https://calendly.com/tonyspiro/general-meeting) with the Cosmic team.


---


## FAQ


### Can I use this with the Next.js Pages Router instead of App Router?


Yes. The works the same way. Replace server components with and , and the data fetching calls are identical.


### Does Cosmic support Markdown for blog content?


Yes. Cosmic has a native Markdown metafield type. The SDK returns the raw Markdown string, so you can render it with a library like or convert it to HTML server-side with .


### Is the Cosmic free tier enough for a personal blog?


The free plan includes 1 Bucket, 1,000 Objects, and 2 team members, which is plenty for most personal or small team blogs. Paid plans start at $49/month for more objects and additional buckets.


### Does Cosmic offer a REST API directly, without the SDK?


Yes. The SDK wraps the Cosmic REST API, but you can call the REST endpoints directly using if you prefer. The SDK is recommended for TypeScript projects because of the built-in types.
