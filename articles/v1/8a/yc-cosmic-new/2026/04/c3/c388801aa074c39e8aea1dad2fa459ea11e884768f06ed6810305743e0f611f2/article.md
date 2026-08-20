---
schema_version: "1.0.0"
document_id: "c388801aa074c39e8aea1dad2fa459ea11e884768f06ed6810305743e0f611f2"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/headless-cms-tanstack-start"
published_at: "2026-04-17T00:00:00+00:00"
first_seen_at: "2026-08-10T06:05:12.278252+00:00"
fetched_at: "2026-08-10T06:05:15.916664+00:00"
content_hash: "sha256:b8486f53f570f31f6930f4c4e7393ecea05e2ca4ac747e0e358c0baa2246939b"
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
npx create-tsrouter-app@latest my-cosmic-app --template start-basic
cd   my-cosmic-app
npm     install
```


This gives you a working TanStack Start app with file-based routing, SSR enabled, and Vite as the bundler.


## 2. Install the Cosmic SDK


```text
npm     install   @cosmicjs/sdk
```


## 3. Configure Your Environment Variables


Create a` .env` file at the root of your project:


```text
COSMIC_BUCKET_SLUG  =  your-bucket-slug
COSMIC_READ_KEY  =  your-read-key
```


You can find both values in your Cosmic dashboard under *Bucket > Settings > API Keys* .


> TanStack Start uses Vite under the hood. Server-side environment variables are accessed via` process.env` inside server functions. For client-side access, prefix with` VITE_` — but keep your read key on the server only.


## 4. Create a Cosmic Client


Add a shared client file at` src/lib/cosmic.ts` :


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


export     const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  !  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
}  )
```


## 5. Fetch Posts with a Server Function


TanStack Start's server functions run exclusively on the server, making them the right place to call external APIs and keep keys out of the client bundle.


Create` src/server/posts.ts` :


```text
import     {   createServerFn   }     from     '@tanstack/start'
import     {   cosmic   }     from     '../lib/cosmic'


export     type     Post     =     {
id  :     string
title  :     string
slug  :     string
metadata  :     {
teaser  :     string
published_date  :     string
image  ?  :     {   imgix_url  :     string     }
}
}


export     const   fetchPosts   =     createServerFn  (  {   method  :     'GET'     }  )  .  handler  (
async     (  )     =>     {
const     {   objects   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'     }  )
.  props  (  [  'id'  ,     'title'  ,     'slug'  ,     'metadata.teaser'  ,     'metadata.published_date'  ,     'metadata.image'  ]  )
.  limit  (  10  )


return   objects   as     Post  [  ]
}
)


export     const   fetchPost   =     createServerFn  (  {   method  :     'GET'     }  )
.  validator  (  (  slug  :     string  )     =>   slug  )
.  handler  (  async     (  {   data  :   slug   }  )     =>     {
const     {   object   }     =     await   cosmic  .  objects
.  findOne  (  {   type  :     'blog-posts'  ,   slug   }  )
.  props  (  [  'id'  ,     'title'  ,     'slug'  ,     'metadata'  ]  )
.  depth  (  1  )


return   object
}  )
```


## 6. Create the Blog Index Route


TanStack Start uses file-based routing. Create` src/routes/blog/index.tsx` :


```text
import     {   createFileRoute  ,     Link     }     from     '@tanstack/react-router'
import     {   fetchPosts   }     from     '../../server/posts'


export     const     Route     =     createFileRoute  (  '/blog/'  )  (  (  {
loader  :     (  )     =>     fetchPosts  (  )  ,
component  :     BlogIndex  ,
}  )  )


function     BlogIndex  (  )     {
const   posts   =     Route  .  useLoaderData  (  )


return     (
<  main className  =  "max-w-2xl mx-auto py-12 px-4"  >
<  h1 className  =  "text-3xl font-bold mb-8"  >  Blog  <  /  h1  >
<  ul className  =  "space-y-6"  >
{  posts  .  map  (  (  post  )     =>     (
<  li key  =  {  post  .  id  }  >
<  Link
to  =  "/blog/$slug"
params  =  {  {   slug  :   post  .  slug     }  }
className  =  "group"
>
<  h2 className  =  "text-xl font-semibold group-hover:underline"  >
{  post  .  title  }
<  /  h2  >
{  post  .  metadata  .  teaser     &&     (
<  p className  =  "text-gray-600 mt-1"  >  {  post  .  metadata  .  teaser  }  <  /  p  >
)  }
{  post  .  metadata  .  published_date     &&     (
<  time className  =  "text-sm text-gray-400"  >
{  new     Date  (  post  .  metadata  .  published_date  )  .  toLocaleDateString  (  )  }
<  /  time  >
)  }
<  /  Link  >
<  /  li  >
)  )  }
<  /  ul  >
<  /  main  >
)
}
```


## 7. Create the Post Detail Route


Install` react-markdown` for safe, component-based markdown rendering:


```text
npm     install   react-markdown
```


Create` src/routes/blog/$slug.tsx` :


```text
import     {   createFileRoute  ,   notFound   }     from     '@tanstack/react-router'
import     ReactMarkdown     from     'react-markdown'
import     {   fetchPost   }     from     '../../server/posts'


export     const     Route     =     createFileRoute  (  '/blog/$slug'  )  (  {
loader  :     async     (  {   params   }  )     =>     {
const   post   =     await     fetchPost  (  {   data  :   params  .  slug     }  )
if     (  !  post  )     throw     notFound  (  )
return   post
}  ,
component  :     BlogPost  ,
}  )


function     BlogPost  (  )     {
const   post   =     Route  .  useLoaderData  (  )


return     (
<  main className  =  "max-w-2xl mx-auto py-12 px-4"  >
{  post  .  metadata  .  image  ?.  imgix_url   &&     (
<  img
src  =  {  `  ${  post  .  metadata  .  image  .  imgix_url  }  ?w=800&auto=format  `  }
alt  =  {  post  .  title  }
className  =  "w-full rounded-lg mb-8"
/  >
)  }
<  h1 className  =  "text-3xl font-bold mb-4"  >  {  post  .  title  }  <  /  h1  >
{  post  .  metadata  .  published_date     &&     (
<  time className  =  "text-sm text-gray-400 block mb-8"  >
<  /  time  >
)  }
<  div className  =  "prose"  >
<  ReactMarkdown  >  {  post  .  metadata  .  markdown_content     ||     ''  }  <  /  ReactMarkdown  >
<  /  div  >
<  /  main  >
)
}
```


` react-markdown` renders markdown as React components, so there's no raw HTML injection and no XSS risk. It also gives you a clean hook to customize rendering via the` components` prop if you need custom heading styles, code blocks, or link behavior down the line.


## 8. Run the Dev Server


```text
npm   run dev
```


Open` http://localhost:3000/blog` and you should see your Cosmic posts rendered server-side via TanStack Start.


## How It Works


- *Server functions* (` createServerFn` ) run on the server only. Cosmic SDK calls stay server-side, so your read key never leaks to the browser.
- *Route loaders* call server functions and pass data down to components. TanStack Router handles caching, deduplication, and hydration automatically.
- *File-based routing* keeps the project organized. Each route in` src/routes/` maps to a URL segment.
- *Vite* handles bundling and HMR, so development is fast.


## Deploying to Vercel


TanStack Start supports multiple deployment targets. For Vercel:


```text
npm     install   @tanstack/start-vercel
```


Then update` app.config.ts` :


```text
import     {   defineConfig   }     from     '@tanstack/start/config'


export     default     defineConfig  (  {
server  :     {
preset  :     'vercel'  ,
}  ,
}  )
```


Push to GitHub, import the repo in Vercel, and add your` COSMIC_BUCKET_SLUG` and` COSMIC_READ_KEY` environment variables in the Vercel dashboard. That's it.


## What to Build Next


Once the basics are working, here are four high-value extensions that are specific to the TanStack Start and Cosmic combination:


**Add localization with Cosmic's Localization add-on.** Cosmic has built-in localization support at the object level. Enable it in your bucket settings, add locale variants to your blog post objects, and pass a` locale` parameter in your SDK queries. Pair that with TanStack Router's search params to build locale-aware routes without a separate i18n library.


**Trigger Vercel rebuilds with Cosmic Webhooks.** In your Cosmic bucket, go to *Settings > Webhooks* and add a` publish` event webhook pointing at your Vercel Deploy Hook URL. Every time an editor publishes a post in Cosmic, Vercel kicks off a fresh build automatically. No polling, no manual deploys.


**Manage content from Slack with a Cosmic Team Agent.** Cosmic's AI agents let you create, update, and query content objects directly from a Slack channel. Set up a Team Agent connected to your bucket and your editors can say "publish the TanStack post" or "create a draft about X" without ever opening the dashboard. It's particularly useful for teams that live in Slack.


**Add full-text search with the Cosmic REST API.** The Cosmic REST API supports a` ?query=` parameter for full-text object search. Wire up a TanStack Start server function that accepts a search term and returns matching posts, then connect it to a search input on the index route for instant, server-rendered results.


## Get Started


You can get a Cosmic account for free at[cosmicjs.com](https://www.cosmicjs.com/) . The free plan includes 1 bucket, 1,000 objects, and 2 team members. No credit card required.


Ready to see how Cosmic fits your team's workflow?[Book a quick intro with Tony](https://calendly.com/tonyspiro/cosmic-intro) and we'll walk you through it.


Have questions or want to share what you've built? Drop into the[Cosmic community](https://www.cosmicjs.com/community) or open a GitHub issue on the[Cosmic SDK repo](https://github.com/cosmicjs/cosmic-sdk-js) .
