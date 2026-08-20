---
schema_version: "1.0.0"
document_id: "6f66a789a8cc005cd276b7954742a7f14cb78e931a190210213231f2aeb6489a"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/headless-cms-nextjs-developer-guide"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-08-10T06:05:12.278252+00:00"
fetched_at: "2026-08-10T06:05:15.916664+00:00"
content_hash: "sha256:e34224f9b394a5c3fa7ba8cea5c34ba119d206a13d428142728300ec11511093"
---

# Headless CMS for Next.js: The Developer Guide

If you're building a Next.js app and you've started thinking about where your content lives, you're in the right place. This guide cuts through the noise and gives you a practical, code-first look at what a headless CMS is, what to look for when choosing one, and exactly how to connect Cosmic to a Next.js project using the App Router.


---


## Why Next.js Developers Need a Headless CMS


Next.js gives you three powerful rendering strategies: Static Site Generation (SSG), Server-Side Rendering (SSR), and Incremental Static Regeneration (ISR). Each one has different data-fetching requirements, and a headless CMS fits cleanly into all three.


### SSG: Build-time content fetching


With SSG, your pages are pre-rendered at build time. This is ideal for content that doesn't change often: blog posts, marketing pages, documentation. A headless CMS gives non-developers a place to manage that content without touching code, while your build pipeline pulls it in via API at build time.


### SSR: Fresh content on every request


SSR renders pages on the server for every request. This is useful for personalized content, frequently updated data, or anything where stale content is a problem. A headless CMS with a fast REST API means you're not bottlenecked by a slow content layer.


### ISR: The best of both worlds


ISR lets you statically generate pages at build time and revalidate them in the background on a configurable schedule. This is the sweet spot for most content-heavy sites: fast delivery with reasonably fresh data. A headless CMS pairs perfectly with ISR because you get CDN-speed page delivery while editors can push updates that propagate within minutes.


In all three cases, the pattern is the same: your Next.js app fetches structured content from an API, renders it, and ships it to users. The CMS is a backend you never have to maintain.


---


## What to Look for in a CMS for Next.js


Not every CMS is built with modern frontend frameworks in mind. Here's what actually matters when you're evaluating one for a Next.js project.


### API-first architecture


The CMS needs to expose content over a well-documented REST API. You're not using a monolithic PHP CMS that renders HTML server-side. You need clean JSON responses that map directly to your TypeScript types. Look for predictable response shapes, good filtering and sorting options, and solid pagination.


### TypeScript support


If you're writing TypeScript (and you should be), you want a CMS SDK that ships types out of the box. Manually typing API responses is tedious and error-prone. A typed SDK means autocompletion in your editor, compile-time safety, and faster development.


### Media handling and image optimization


Content-heavy apps need image handling that actually performs. Look for a CMS that stores images in a CDN and supports on-the-fly transformations via URL parameters. This integrates directly with Next.js's` <Image>` component, letting you serve correctly sized, format-optimized images without extra build tooling.


### Flexible content modeling


Your content structure will evolve. You'll start with a blog post type and end up with landing pages, product listings, author profiles, and more. A headless CMS should let you define custom content types with arbitrary fields, without requiring a developer to deploy schema changes.


### Webhooks for cache invalidation


With ISR, you'll want to trigger revalidation when content changes. A CMS that fires webhooks on publish lets you build a tight feedback loop: editor saves a post, webhook fires, Next.js revalidates the relevant routes, updated content is live within seconds.


---


## How to Connect Cosmic to a Next.js App


Let's get into the code. We'll walk through setting up[Cosmic](https://www.cosmicjs.com/) as your headless CMS for a Next.js App Router project, step by step.


If you're evaluating CMS options for React more broadly, the patterns here also apply to the[headless CMS for React](https://www.cosmicjs.com/headless-cms-for-react) ecosystem. This guide focuses on using Cosmic for your[Next.js headless CMS](https://www.cosmicjs.com/blog/headless-cms-nextjs-developer-guide) .


### Prerequisites


- A Next.js 14+ project using the App Router
- A Cosmic account (free tier available at[cosmicjs.com](https://www.cosmicjs.com/) )
- Node.js 18+


### Step 1: Install the Cosmic SDK


The` @cosmicjs/sdk` package is the official TypeScript-first client. Install it:


```text
npm     install   @cosmicjs/sdk
```


Then create a client instance. The best pattern is a shared module so you initialize the client once:


```text
// lib/cosmic.ts
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


export     const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG     as     string  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY     as     string  ,
}  )
```


Add your credentials to` .env.local` :


```text
COSMIC_BUCKET_SLUG  =  your-bucket-slug
COSMIC_READ_KEY  =  your-read-key
```


You can find both values in your Cosmic dashboard under **Settings > API Keys** .


### Step 2: Fetch Content in an App Router Server Component


Next.js App Router components are server components by default. That means you can fetch data directly inside the component without` useEffect` or client-side loading states.


Here's a complete example fetching a list of blog posts:


```text
// app/blog/page.tsx
import     {   cosmic   }     from     '@/lib/cosmic'


type     Post     =     {
id  :     string
title  :     string
slug  :     string
metadata  :     {
teaser  :     string
published_date  :     string
image  :     {
imgix_url  :     string
}
}
}


async     function     getPosts  (  )  :     Promise  <  Post  [  ]  >     {
const     {   objects   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'     }  )
.  props  (  [  'id'  ,     'title'  ,     'slug'  ,     'metadata.teaser'  ,     'metadata.published_date'  ,     'metadata.image'  ]  )
.  sort  (  '-created_at'  )
.  limit  (  10  )


return   objects
}


export     default     async     function     BlogPage  (  )     {
const   posts   =     await     getPosts  (  )


return     (
<  main  >
<  h1  >  Blog  <  /  h1  >
<  ul  >
{  posts  .  map  (  (  post  )     =>     (
<  li key  =  {  post  .  id  }  >
<  a href  =  {  `  /blog/  ${  post  .  slug  }  `  }  >  {  post  .  title  }  <  /  a  >
<  p  >  {  post  .  metadata  .  teaser  }  <  /  p  >
<  /  li  >
)  )  }
<  /  ul  >
<  /  main  >
)
}
```


A few things to note here:


- The` .props()` call limits the response to only the fields you need. This reduces payload size significantly on content-heavy objects.
- ` .sort('-created_at')` sorts newest first. Prefix with` -` for descending order.
- There's no loading state to manage because this runs on the server before the page is sent to the client.


### Step 3: Fetch a Single Post


For individual post pages, fetch by slug:


```text
// app/blog/[slug]/page.tsx
import     {   cosmic   }     from     '@/lib/cosmic'
import     {   notFound   }     from     'next/navigation'


type     PostDetail     =     {
id  :     string
title  :     string
slug  :     string
metadata  :     {
markdown_content  :     string
teaser  :     string
published_date  :     string
image  :     {
imgix_url  :     string
}
author  :     {
title  :     string
metadata  :     {
image  :     {
imgix_url  :     string
}
}
}
}
}


async     function     getPost  (  slug  :     string  )  :     Promise  <  PostDetail     |     null  >     {
try     {
const     {   object   }     =     await   cosmic  .  objects
.  findOne  (  {   type  :     'blog-posts'  ,   slug   }  )
.  props  (  [  'id'  ,     'title'  ,     'slug'  ,     'metadata'  ]  )
.  depth  (  1  )


return   object
}     catch     {
return     null
}
}


type     Props     =     {
params  :     Promise  <  {   slug  :     string     }  >
}


export     default     async     function     BlogPostPage  (  {   params   }  :     Props  )     {
const     {   slug   }     =     await   params
const   post   =     await     getPost  (  slug  )


if     (  !  post  )     notFound  (  )


return     (
<  article  >
<  h1  >  {  post  .  title  }  <  /  h1  >
<  p  >  By     {  post  .  metadata  .  author  ?.  title  }  <  /  p  >
<  div dangerouslySetInnerHTML  =  {  {   __html  :   post  .  metadata  .  markdown_content     }  }     /  >
<  /  article  >
)
}
```


The` .depth(1)` option tells Cosmic to resolve relationship fields one level deep. Without it,` author` would return only the author's ID instead of the full object.


### Step 4: Use` generateStaticParams` for SSG


To pre-render all blog post pages at build time, export a` generateStaticParams` function from your dynamic route segment:


```text
// app/blog/[slug]/page.tsx (add this export)
export     async     function     generateStaticParams  (  )     {
const     {   objects   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'     }  )
.  props  (  [  'slug'  ]  )
.  limit  (  100  )


return   objects  .  map  (  (  post  )     =>     (  {
slug  :   post  .  slug  ,
}  )  )
}
```


Next.js calls this function at build time, collects all the slugs, and pre-renders each corresponding page. The result is a set of fully static HTML pages served from your CDN, with no server compute at request time.


For large content sets, paginate the Cosmic API using` .skip()` and call it in a loop to collect all slugs before returning them.


### Step 5: ISR with` revalidate`


For content that changes regularly, ISR gives you static performance with automatic freshness. Add a` revalidate` export to your route segment:


```text
// app/blog/[slug]/page.tsx
export     const   revalidate   =     300     // revalidate every 5 minutes
```


Next.js will serve the cached static page and revalidate it in the background after 300 seconds. The first request after the cache expires triggers a fresh fetch from Cosmic.


For more granular control, use on-demand revalidation. Set up a Cosmic webhook to call a Next.js Route Handler when content is published:


```text
// app/api/revalidate/route.ts
import     {   revalidatePath  ,   revalidateTag   }     from     'next/cache'
import     {     NextRequest  ,     NextResponse     }     from     'next/server'


export     async     function     POST  (  request  :     NextRequest  )     {
const   secret   =   request  .  headers  .  get  (  'x-webhook-secret'  )


if     (  secret   !==   process  .  env  .  REVALIDATION_SECRET  )     {
return     NextResponse  .  json  (  {   error  :     'Unauthorized'     }  ,     {   status  :     401     }  )
}


const   body   =     await   request  .  json  (  )
const   slug   =   body  ?.  data  ?.  slug


if     (  slug  )     {
revalidatePath  (  `  /blog/  ${  slug  }  `  )
}


revalidatePath  (  '/blog'  )


return     NextResponse  .  json  (  {   revalidated  :     true     }  )
}
```


In Cosmic, configure a webhook under **Settings > Webhooks** pointing to` https://yourdomain.com/api/revalidate` with your chosen secret. Now every time an editor publishes a post, the relevant Next.js routes are revalidated within seconds.


---


## Common Patterns


### Blog with Pagination


For a paginated blog index, use Cosmic's` .limit()` and` .skip()` together:


```text
// app/blog/page.tsx
const     PAGE_SIZE     =     10


type     SearchParams     =     Promise  <  {   page  ?  :     string     }  >


export     default     async     function     BlogPage  (  {
searchParams  ,
}  :     {
searchParams  :     SearchParams
}  )     {
const     {   page   }     =     await   searchParams
const   currentPage   =     Number  (  page   ??     1  )
const   skip   =     (  currentPage   -     1  )     *     PAGE_SIZE


const     {   objects  :   posts  ,   total   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'     }  )
.  props  (  [  'id'  ,     'title'  ,     'slug'  ,     'metadata.teaser'  ,     'metadata.published_date'  ]  )
.  sort  (  '-created_at'  )
.  limit  (  PAGE_SIZE  )
.  skip  (  skip  )


const   totalPages   =     Math  .  ceil  (  total   /     PAGE_SIZE  )


return     (
<  main  >
{  posts  .  map  (  (  post  )     =>     (
<  article key  =  {  post  .  id  }  >
<  /  article  >
)  )  }
<  nav  >
{  currentPage   >     1     &&     <  a href  =  {  `  ?page=  ${  currentPage   -     1  }  `  }  >  Previous  <  /  a  >  }
{  currentPage   <   totalPages   &&     <  a href  =  {  `  ?page=  ${  currentPage   +     1  }  `  }  >  Next  <  /  a  >  }
<  /  nav  >
<  /  main  >
)
}
```


### Landing Pages with Dynamic Content


Headless CMSes shine for marketing landing pages where the content team needs to iterate quickly. Define a` landing-pages` type in Cosmic with fields for headline, subheadline, body copy, and CTA text. Then render it in Next.js:


```text
// app/[landing-page]/page.tsx
async     function     getLandingPage  (  slug  :     string  )     {
try     {
const     {   object   }     =     await   cosmic  .  objects
.  findOne  (  {   type  :     'landing-pages'  ,   slug   }  )
.  props  (  [  'title'  ,     'metadata.headline'  ,     'metadata.subheadline'  ,     'metadata.markdown_content'  ,     'metadata.seo_title'  ,     'metadata.seo_description'  ]  )


return   object
}     catch     {
return     null
}
}
```


Content editors can create, update, and publish landing pages without touching your codebase. Your Next.js app just fetches and renders whatever Cosmic returns.


### Image Optimization with Cosmic's imgix CDN


Cosmic serves all media through imgix, which means you can transform images on the fly using URL parameters. This integrates perfectly with Next.js's` <Image>` component:


```text
import     Image     from     'next/image'


type     Props     =     {
src  :     string     // imgix_url from Cosmic
alt  :     string
width  :     number
height  :     number
}


export     function     CosmicImage  (  {   src  ,   alt  ,   width  ,   height   }  :     Props  )     {
return     (
<  Image
src  =  {  src  }
alt  =  {  alt  }
width  =  {  width  }
height  =  {  height  }
// imgix handles format conversion (WebP, AVIF) automatically
// when you pass quality and format params
/  >
)
}
```


To use Next.js's built-in image optimization with imgix URLs, add the domain to your` next.config.ts` :


```text
// next.config.ts
import     type     {     NextConfig     }     from     'next'


const   nextConfig  :     NextConfig     =     {
images  :     {
remotePatterns  :     [
{
protocol  :     'https'  ,
hostname  :     'imgix.cosmicjs.com'  ,
}  ,
]  ,
}  ,
}


export     default   nextConfig
```


You can also append imgix parameters directly to the` src` URL for server-side resizing before the image even reaches Next.js:


```text
const   optimizedSrc   =     `  ${  post  .  metadata  .  image  .  imgix_url  }  ?w=800&auto=format,compress&q=80  `
```


This reduces bandwidth, improves Core Web Vitals scores, and works without any extra configuration.


---


## Putting It All Together


Here's a quick mental model for a complete Next.js + Cosmic setup:


- **Content types** live in Cosmic (blog posts, landing pages, authors, categories)
- **Content editing** happens in the Cosmic dashboard, accessible to anyone on your team
- **Fetching** uses the typed` @cosmicjs/sdk` in server components or Route Handlers
- **Rendering** uses SSG with` generateStaticParams` for known slugs, ISR with` revalidate` for automatic freshness, or SSR for personalized or always-fresh content
- **Cache invalidation** happens via Cosmic webhooks triggering` revalidatePath` in your Next.js app
- **Images** are served through imgix with on-the-fly optimization


This architecture keeps your Next.js app lean and fast while giving your content team full autonomy. The separation is clean: the CMS is the source of truth for content, and Next.js handles rendering and delivery.


For teams building React apps beyond Next.js, many of these same patterns apply. See our[headless CMS for React](https://www.cosmicjs.com/headless-cms-for-react) guide for framework-agnostic patterns.


---


## Ready to Build?


The fastest way to see this in action is to spin up a Cosmic bucket and connect it to your Next.js project. The free tier gives you plenty of room to prototype.


**[Start free on Cosmic](https://app.cosmicjs.com/signup)** and have content flowing into your Next.js app in under 10 minutes.


Want a walkthrough of your specific use case? **[Book a 30-minute intro with Tony](https://calendly.com/tonyspiro/cosmic-intro)** and we'll map out the right setup for your project.


For the full feature overview and pricing, visit our **[headless CMS for Next.js](https://www.cosmicjs.com/headless-cms-for-nextjs)** page.
