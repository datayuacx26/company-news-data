---
schema_version: "1.0.0"
document_id: "21e5220c858564e2920f7107a6a3ebf10b1b0a5dd48a881c32257fce30e21d3d"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/headless-cms-for-nuxt"
published_at: "2026-04-07T00:00:00+00:00"
first_seen_at: "2026-08-10T06:05:12.278252+00:00"
fetched_at: "2026-08-10T06:05:15.916664+00:00"
content_hash: "sha256:df55b697cf239c3bfaa3f93f0a8b71eff7de00c0cd66b69fc59f0f5eea28feef"
---

# Headless CMS for Nuxt 3: The Complete Guide

> **Updated July 31, 2026.** Pricing, plan limits, and add-on costs in this guide were re-verified against the[live pricing page](https://www.cosmicjs.com/pricing) on this date. Code examples use the current` @cosmicjs/sdk` TypeScript SDK. Comparing platforms first? Read[Best Headless CMS in 2026](https://www.cosmicjs.com/blog/best-headless-cms-2026) or the[headless CMS overview](https://www.cosmicjs.com/best-headless-cms) .


Nuxt 3 is one of the most capable full-stack frameworks available today. Its support for server-side rendering (SSR), static site generation (SSG), and incremental static regeneration (ISR) gives developers real flexibility in how they deliver content. That flexibility creates a challenge, because traditional CMS platforms were not built for it.


WordPress, Drupal, and legacy monolithic CMS tools assume they own the frontend. They couple content storage to template rendering, making it nearly impossible to use Nuxt's rendering modes effectively. When you need to switch a page from SSG to SSR, or use ISR for a high-traffic section, a coupled CMS gets in the way.


A headless CMS solves this by separating the content layer from the presentation layer entirely. Content is stored and managed in one place, then delivered via API to any frontend, including your Nuxt 3 app. You get full control over rendering strategy, no server coupling, and a content model that can evolve without touching your frontend code.


This guide covers why Cosmic is the right headless CMS for Nuxt 3 projects, followed by a complete step-by-step integration tutorial.


---


## Why Cosmic Fits Nuxt 3 Specifically


### REST API That Works Natively with` useAsyncData` and` useFetch`


Nuxt 3's data fetching composables,` useAsyncData` and` useFetch` , are designed to wrap async data sources cleanly, deduplicate requests between server and client, and handle SSR hydration automatically. Cosmic's REST API slots directly into this pattern.


Cosmic API endpoints are served through a global CDN cache layer, so cached reads return from the edge region closest to your build or SSR process. You call the API inside` useAsyncData` , Nuxt handles the rest. Measure your own p95 against your real content model and payload sizes rather than trusting any vendor's published number, including ours.


### TypeScript SDK (Nuxt is TypeScript-First)


Nuxt 3 is built on TypeScript from the ground up, with auto-imported composables, typed route params, and full IDE support. Cosmic's JavaScript/TypeScript SDK matches this philosophy. You get typed responses out of the box, which means your content models surface as proper TypeScript types in your components instead of untyped JSON blobs.


### No Migrations, Flexible Content Modeling


Nuxt projects often evolve quickly. A new section needs a new field, a blog post type needs a tag taxonomy, a product page needs localized variants. With a traditional database-backed CMS, every content model change is a migration, a deployment, and sometimes a production risk.


Cosmic uses a schema-optional approach: you define the content model you need, add fields when requirements change, and nothing breaks. No migrations, no database downtime, no coordination between content and infrastructure teams.


### AI Agents That Live in Slack


Content teams working in Nuxt projects often run into a familiar frustration: a copywriter or editor needs to update a headline or swap out a banner image, but the developer owns the CMS access and the workflow grinds to a halt.


Cosmic AI Agents change this. Non-technical team members can message a Cosmic agent directly in Slack to create content, update existing objects, or trigger content workflows, all without writing a single line of code or filing a developer ticket. Every plan includes agents, from 1 on Free up to 25 on Business.


That is the same class of problem Maximilian Wuhr, Co-Founder at FINN, described when he summed up Cosmic this way: "Cosmic is: us never having to ask a developer to change anything on the backend of our website." Vuetify, a project at the center of the Vue ecosystem, also runs on Cosmic.


---


## Step-by-Step: Connecting Nuxt 3 to Cosmic


### Step 1: Install the Cosmic SDK


Install the Cosmic JavaScript SDK in your Nuxt 3 project:


```text
npm     install   @cosmicjs/sdk
```


Or use the Cosmic CLI to scaffold a project from a template:


```text
npx create-cosmic-app@latest my-nuxt-app
```


### Step 2: Set Up Your Environment Variables


Create a` .env` file in your project root with your Cosmic bucket credentials. You will find these in your Cosmic dashboard under **Bucket > Settings > API Keys** .


```text
# .env
COSMIC_BUCKET_SLUG  =  your-bucket-slug
COSMIC_READ_KEY  =  your-read-key
COSMIC_WRITE_KEY  =  your-write-key
```


In your` nuxt.config.ts` , expose these as runtime config:


```text
// nuxt.config.ts
export     default     defineNuxtConfig  (  {
runtimeConfig  :     {
cosmicBucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  ,
cosmicReadKey  :   process  .  env  .  COSMIC_READ_KEY  ,
}
}  )
```


### Step 3: Create a Content Type in the Cosmic Dashboard


Log into your[Cosmic dashboard](https://app.cosmicjs.com/signup) and create a new Object Type. For a blog, you might create a` Posts` type with these metafields:


- **Title** (text, required)
- **Slug** (auto-generated)
- **Content** (markdown or rich text)
- **Cover Image** (file)
- **Published Date** (date)
- **Author** (object relationship)


Cosmic's visual schema builder means no YAML files or code-based schema definitions. Add a field, save, and it is immediately available via the API.


### Step 4: Fetch Content Using` useAsyncData`


Create a Nuxt plugin to initialize the Cosmic client:


```text
// plugins/cosmic.ts
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


export     default     defineNuxtPlugin  (  (  )     =>     {
const   config   =     useRuntimeConfig  (  )


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   config  .  cosmicBucketSlug  ,
readKey  :   config  .  cosmicReadKey  ,
}  )


return     {
provide  :     {
cosmic
}
}
}  )
```


Then fetch content in your page component using` useAsyncData` :


```text
// pages/blog/index.vue
<  script setup lang  =  "ts"  >
const     {   $cosmic   }     =     useNuxtApp  (  )


const     {   data  :   posts  ,   error   }     =     await     useAsyncData  (  'posts'  ,     (  )     =>
$cosmic  .  objects
.  find  (  {   type  :     'posts'     }  )
.  props  (  'id,title,slug,metadata,thumbnail'  )
.  limit  (  10  )
.  sort  (  '-created_at'  )
)
<  /  script  >
```


` useAsyncData` ensures this runs on the server during SSR, deduplicates the call during hydration, and works correctly with Nuxt's ISR strategy when combined with` getCachedData` .


### Step 5: Render Content with Vue Components


With data in hand, render it using standard Vue 3 component patterns:


```text
// pages/blog/index.vue (template section)
<  template  >
<  div   class  =  "posts-grid"  >
<  article v  -  for  =  "post in posts?.objects"     :  key  =  "post.id"  >
<  NuxtLink     :  to  =  "`/blog/${post.slug}`"  >
<  img
v  -  if  =  "post.thumbnail"
:  src  =  "post.thumbnail + '?w=800&auto=format,compress'"
:  alt  =  "post.title"
loading  =  "lazy"
/  >
<  h2  >  {  {   post  .  title     }  }  <  /  h2  >
<  p  >  {  {   post  .  metadata  ?.  teaser   }  }  <  /  p  >
<  /  NuxtLink  >
<  /  article  >
<  /  div  >
<  /  template  >
```


Note the imgix query parameters appended to the thumbnail URL. Every image stored in Cosmic is served through imgix CDN automatically, so you get on-the-fly resizing, format conversion, and compression without any additional configuration.


### Step 6: Deploy to Vercel or Netlify


Deployment is straightforward. Both Vercel and Netlify support Nuxt 3 natively.


**Vercel:** Add your environment variables in the Vercel project settings, then deploy:


```text
vercel deploy
```


**Netlify:** Add your environment variables in the Netlify site settings, then deploy:


```text
ntl deploy
```


For SSG builds, trigger a new deployment when content changes by setting up a Cosmic webhook that fires a deploy hook on your hosting platform. Webhooks are a feature add-on at $99/month, or $199/month bundled with Localization, Revision History, and Automatic Backups, and every add-on includes a 14-day free trial. Configuration takes a couple of minutes once enabled.


---


## Nuxt-Specific Tips


### Using Cosmic with the Nuxt Content Module


If your project already uses` @nuxt/content` for local Markdown files, Cosmic can complement it. Use Nuxt Content for documentation or long-form static content that lives in your repository, and Cosmic for dynamic content that editors update frequently, such as blog posts, product pages, or landing page copy. The two do not conflict and can be used side by side in the same Nuxt project.


### Image Optimization with imgix CDN


Every file uploaded to Cosmic is automatically served through the imgix CDN. This means you get:


- **On-the-fly resizing** via URL parameters (` ?w=800&h=600&fit=crop` )
- **Format conversion** to WebP or AVIF automatically (` ?auto=format` )
- **Compression** tuned for quality/performance balance (` ?auto=compress` )
- **Lazy loading** support with proper placeholder strategies


In Nuxt 3, combine imgix URL parameters with the native` <NuxtImg>` component (via` @nuxt/image` ) or simply append parameters directly to the imgix URL string Cosmic returns. No additional image optimization infrastructure needed.


### Using Nuxt Server Routes with the Cosmic REST API


For content operations that should stay server-side (such as write operations using your write key, or preview drafts), use Nuxt server routes:


```text
// server/api/posts/[slug].get.ts
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


export     default     defineEventHandler  (  async     (  event  )     =>     {
const   slug   =     getRouterParam  (  event  ,     'slug'  )


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  !  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
}  )


const     {   object  :   post   }     =     await   cosmic  .  objects
.  findOne  (  {   type  :     'posts'  ,   slug   }  )
.  props  (  'id,title,slug,metadata,thumbnail'  )


return   post
}  )
```


This keeps your read key off the client entirely, which is important for buckets where content sensitivity matters.


---


## AI Agent Workflow for Nuxt Projects


One of the most underrated advantages of using Cosmic with Nuxt 3 is the AI agent layer it adds to your content workflow. Two integrations stand out:


### Cursor Agent Skill


If your team uses Cursor as their IDE, the Cosmic Cursor Agent Skill lets your AI coding assistant directly read from and write to your Cosmic bucket. You can describe a content structure in plain English and have Cursor scaffold the Object Type, populate sample data, and generate the Nuxt composable to fetch it, all in one conversation.


### Claude Code MCP Server


The Cosmic MCP (Model Context Protocol) server integrates with Claude Code, giving the AI assistant direct access to your content API. During Nuxt development, this means Claude can introspect your content model, write type-safe fetch functions that match your actual schema, and generate component code that maps directly to your real content fields. No more copy-pasting API responses into a chat window to get useful code suggestions.


Together, these tools mean Nuxt developers spend less time wiring up content infrastructure and more time building features.


---


## What Cosmic Costs for a Nuxt Project


Verified against the[live pricing page](https://www.cosmicjs.com/pricing) on July 31, 2026:


Plan Price Buckets Team members Objects


Free $0/mo 1 2 1,000


Builder $49/mo 2 3 5,000


Team $299/mo 3 5 20,000


Business $499/mo 5 10 50,000


Enterprise Custom Custom Custom Custom


Two numbers worth knowing before you plan a team rollout. Additional team members beyond your plan's included seats are **$29/user/month** , and additional buckets are **$29/bucket/month** . Usage above your plan limits is metered rather than hard-capped, at published overage rates such as $0.23 per 10k non-cached API requests. Every plan also includes Cosmic Insights analytics, starting at 100K events/month on Free.


No credit card is required to start, and the Free plan is enough to build and ship a production Nuxt 3 project.


---


## Get Started with Cosmic and Nuxt 3


**Ready to build?**


- [Start free on Cosmic](https://app.cosmicjs.com/signup) and have your first Nuxt 3 fetch running in under 10 minutes
- [Book a demo with Tony](https://calendly.com/tonyspiro/cosmic-intro) if you want to walk through the platform with someone before committing
- Weighing options first? See[Best Headless CMS in 2026: Top 5 Compared](https://www.cosmicjs.com/blog/best-headless-cms-2026) and the[headless CMS for developer-first teams](https://www.cosmicjs.com/best-headless-cms) overview
- Already using Vue? Read the[Vue.js headless CMS guide](https://www.cosmicjs.com/headless-cms-for-vue) for the foundational concepts, then come back here for the Nuxt-specific layer
- Want the full Vue + Cosmic tutorial? Check out[Headless CMS for Vue.js: The Complete Tutorial](https://www.cosmicjs.com/blog/headless-cms-for-vuejs)


Nuxt 3 gives you a powerful rendering framework. Cosmic gives you the content layer that keeps up with it.
