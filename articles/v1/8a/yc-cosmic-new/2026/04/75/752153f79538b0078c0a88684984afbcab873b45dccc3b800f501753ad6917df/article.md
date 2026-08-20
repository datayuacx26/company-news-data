---
schema_version: "1.0.0"
document_id: "752153f79538b0078c0a88684984afbcab873b45dccc3b800f501753ad6917df"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/migrate-from-contentful-to-cosmic"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-08-10T06:05:12.278252+00:00"
fetched_at: "2026-08-10T06:05:15.916664+00:00"
content_hash: "sha256:23966023e09a8b3eabb4b1b45ebd384ef80aaede02760f8f2d30c6c241b396e1"
---

# Migrate from Contentful to Cosmic: A Complete Developer Guide

If you've been running on Contentful and started questioning whether the cost, complexity, or lack of AI-native tooling is worth it, you're not alone. The jump from Contentful's free tier to their Lite plan at $300/month is steep, and the platform still doesn't ship native AI agents for content automation. This guide walks you through migrating to[Cosmic](https://www.cosmicjs.com/) , an AI-powered headless CMS built for modern development teams.


Before diving into the steps, take a look at our[Contentful alternative overview](https://www.cosmicjs.com/contentful-alternative) and[detailed feature comparison](https://www.cosmicjs.com/compare/contentful) to understand what you're gaining in the switch.


---


## Why Teams Migrate from Contentful to Cosmic


Most migration decisions come down to three recurring pain points:


### 1. The Pricing Cliff


Contentful's free plan is functional enough for prototypes, but the moment you need a real team or higher API throughput, the jump is jarring: $0 to $300/month for their Lite plan. Cosmic's first paid tier, Builder, is $49/month. The Team plan (the most popular) is $299/month and includes 5 team members, 20,000 objects, and full access to AI agents with scheduling. For pricing details see our[pricing comparison](https://www.cosmicjs.com/compare/contentful) .


**Cosmic pricing:**


- Free: $0/month — 1 Bucket, 2 Team members, 1,000 Objects
- Builder: $49/month — 2 Buckets, 3 Team members, 5,000 Objects
- Team: $299/month — 3 Buckets, 5 Team members, 20,000 Objects
- Business: $499/month — 5 Buckets, 10 Team members, 50,000 Objects
- Enterprise: Custom pricing
- Additional users: $29/user/month


### 2. No Native AI Agents


Contentful has added AI-assisted writing to its editor, but there are no autonomous AI agents. Cosmic ships with four agent types built in:


- **Content Agents:** Research topics, write articles, generate images, and publish drafts on a schedule.
- **Code Agents:** Connect to your GitHub repo, create branches, commit changes, open pull requests.
- **Computer Use Agents:** Browse the web like a human for research, QA, and monitoring.
- **Team Agents:** Join Slack, WhatsApp, or Telegram so your team can manage content from the tools they already use.


Chain these together in multi-agent Workflows for fully automated content pipelines, no glue code required.


### 3. Rich Text and Query Complexity


Contentful's rich text format is a deeply nested JSON structure with its own renderer library. If you've wrestled with` @contentful/rich-text-react-renderer` just to output a basic blog post, you know the friction. Cosmic uses Markdown or HTML for rich content fields. Clean, portable, and renderable by any standard library.


Contentful also offers a REST API, but teams that want more filtering flexibility often reach for their SDK which adds its own abstraction layer. Cosmic's REST API and JavaScript SDK are designed to be simple: fetch what you need with standard HTTP requests, no custom query language to learn.


---


## Contentful Content Model vs. Cosmic Content Model


Before you migrate any data, it helps to understand how the two platforms map conceptually.


Contentful Concept Cosmic Equivalent


Space Bucket


Environment Bucket (separate Buckets per env)


Content Type Object Type


Entry Object


Field Metafield


Asset Media file (with imgix CDN)


Rich Text field Markdown or HTML-textarea metafield


Reference field (single)` object` metafield


Reference field (array)` objects` metafield


Locale Localization add-on


Tag Tag Object Type


The key mindset shift: in Contentful, your content lives in a **Space** with **Environments** . In Cosmic, each environment is its own **Bucket** , with its own API keys and content. This is actually cleaner for teams running staging and production separately.


---


## Step-by-Step Migration Guide


### Step 1: Export Your Content from Contentful


Contentful provides a CLI export tool that exports all your content types and entries as JSON.


```text
npm     install   -g contentful-cli
contentful login
contentful space   export   --space-id YOUR_SPACE_ID --export-dir ./contentful-export
```


This generates:


- ` contentful-export.json` — all entries and content types
- An` assets/` directory with all media files


Keep both. You'll use the JSON to map your schema and the assets to re-upload to Cosmic.


### Step 2: Map Your Content Types to Cosmic Object Types


Open your export JSON and look at the` contentTypes` array. For each Contentful content type, you'll create a corresponding Object Type in Cosmic with matching metafields.


**Contentful field types → Cosmic metafield types:**


Contentful Field Type Cosmic Metafield Type


Short text` text`


Long text` textarea` or` markdown`


Rich Text` markdown` or` html-textarea`


Number` number`


Date and time` date`


Boolean` switch`


Media (single)` file`


Media (multiple)` files`


Reference (single entry)` object`


Reference (multiple entries)` objects`


JSON` json`


**Handling Rich Text (Important):** Contentful's rich text is stored as a complex JSON tree with node types like` paragraph` ,` hyperlink` ,` embedded-entry-inline` , etc. When migrating, you have two options:


1. **Convert to Markdown:** Use a library like` @contentful/rich-text-plain-text-renderer` combined with manual reformatting. Best for content that's mostly prose.
2. **Convert to HTML:** Use` @contentful/rich-text-html-renderer` to generate HTML strings. Store them in a` html-textarea` metafield in Cosmic. This is the fastest path and preserves all formatting exactly.


```text
// Convert Contentful rich text to HTML for Cosmic
import     {   documentToHtmlString   }     from     '@contentful/rich-text-html-renderer'


const   htmlContent   =     documentToHtmlString  (  entry  .  fields  .  body  )
// Store htmlContent in a Cosmic html-textarea metafield
```


Once in Cosmic, you can render the HTML directly in your frontend without any CMS-specific renderer dependency.


### Step 3: Create Your Object Types in Cosmic


Log into your Cosmic dashboard and create Object Types to match your Contentful content types. Or use the Cosmic REST API to create them programmatically:


```text
// Create a Blog Post object type via the Cosmic API
const   response   =     await     fetch  (
`  https://api.cosmicjs.com/v3/buckets/  ${  BUCKET_SLUG  }  /object-types  `  ,
{
method  :     'POST'  ,
headers  :     {
'Content-Type'  :     'application/json'  ,
Authorization  :     `  Bearer   ${  COSMIC_WRITE_KEY  }  `  ,
}  ,
body  :     JSON  .  stringify  (  {
title  :     'Blog Posts'  ,
slug  :     'blog-posts'  ,
singular  :     'Blog Post'  ,
metafields  :     [
{     key  :     'title'  ,     title  :     'Title'  ,     type  :     'text'     }  ,
{     key  :     'body'  ,     title  :     'Body'  ,     type  :     'markdown'     }  ,
{     key  :     'published_date'  ,     title  :     'Published Date'  ,     type  :     'date'     }  ,
{     key  :     'featured_image'  ,     title  :     'Featured Image'  ,     type  :     'file'     }  ,
{     key  :     'author'  ,     title  :     'Author'  ,     type  :     'object'  ,     object_type  :     'authors'     }  ,
]  ,
}  )  ,
}
)
```


### Step 4: Migrate Media Assets


Contentful assets are exported with their CDN URLs. Upload them to Cosmic's media library (which automatically delivers through imgix CDN).


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY  ,
}  )


async     function     migrateAssets  (  contentfulAssets  )     {
const   mediaMap   =     {  }     // Map Contentful asset IDs to Cosmic imgix URLs


for     (  const   asset   of   contentfulAssets  )     {
const   url   =     `  https:  ${  asset  .  fields  .  file  .  url  }  `
const   name   =   asset  .  fields  .  file  .  fileName


// Upload from URL to Cosmic media library
const     {   media   }     =     await   cosmic  .  media  .  insertOne  (  {
media  :     {
url  ,
name  ,
}  ,
}  )


mediaMap  [  asset  .  sys  .  id  ]     =   media  .  imgix_url
console  .  log  (  `  Migrated:   ${  name  }   →   ${  media  .  imgix_url  }  `  )
}


return   mediaMap
}
```


Every uploaded asset gets a Cosmic imgix URL with on-the-fly resizing, format conversion, and optimization built in. No third-party accounts needed.


### Step 5: Migrate Content Entries


With your Object Types created and media migrated, write a migration script to create Cosmic Objects from your Contentful entries.


```text
async     function     migrateEntries  (  contentfulEntries  ,   mediaMap  )     {
for     (  const   entry   of   contentfulEntries  )     {
const   contentType   =   entry  .  sys  .  contentType  .  sys  .  id


if     (  contentType   ===     'blogPost'  )     {
const     {   fields   }     =   entry


// Convert rich text body to HTML
const   bodyHtml   =     documentToHtmlString  (  fields  .  body  )


await   cosmic  .  objects  .  insertOne  (  {
type  :     'blog-posts'  ,
title  :   fields  .  title  ,
status  :     'published'  ,
metadata  :     {
body  :   bodyHtml  ,
published_date  :   fields  .  publishedDate  ,
featured_image  :   mediaMap  [  fields  .  featuredImage  ?.  sys  ?.  id  ]  ,
// Map other fields...
}  ,
}  )


console  .  log  (  `  Migrated entry:   ${  fields  .  title  }  `  )
}
}
}
```


### Step 6: Update Your API Calls


This is where the day-to-day code changes happen. Contentful and Cosmic both use REST APIs, so the transition is straightforward.


**Contentful SDK call:**


```text
import     contentful     from     'contentful'


const   client   =   contentful  .  createClient  (  {
space  :   process  .  env  .  CONTENTFUL_SPACE_ID  ,
accessToken  :   process  .  env  .  CONTENTFUL_ACCESS_TOKEN  ,
}  )


const   entries   =     await   client  .  getEntries  (  {
content_type  :     'blogPost'  ,
order  :     '-fields.publishedDate'  ,
limit  :     10  ,
}  )


const   posts   =   entries  .  items  .  map  (  (  item  )     =>     (  {
title  :   item  .  fields  .  title  ,
slug  :   item  .  fields  .  slug  ,
body  :     documentToHtmlString  (  item  .  fields  .  body  )  ,     // extra step required
image  :   item  .  fields  .  featuredImage  .  fields  .  file  .  url  ,
}  )  )
```


**Cosmic SDK equivalent:**


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  ,
}  )


const     {     objects  :   posts   }     =     await   cosmic  .  objects
.  find  (  {     type  :     'blog-posts'     }  )
.  props  (  'title,slug,metadata'  )
.  sort  (  '-metadata.published_date'  )
.  limit  (  10  )


// posts[0].metadata.body is already HTML or Markdown — no extra renderer needed
// posts[0].metadata.featured_image is a direct imgix URL
```


No rich text renderer dependency. No nested` .fields` access. Clean metadata keys you defined yourself.


---


## Framework-Specific Code Examples


### Next.js (App Router)


```text
// app/blog/page.tsx
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  !  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
}  )


export     default     async     function     BlogPage  (  )     {
const     {     objects  :   posts   }     =     await   cosmic  .  objects
.  find  (  {     type  :     'blog-posts'     }  )
.  props  (  'id,title,slug,metadata.teaser,metadata.published_date,metadata.featured_image'  )
.  sort  (  '-metadata.published_date'  )
.  limit  (  10  )


return     (
<  main  >
{  posts  .  map  (  (  post  )     =>     (
<  article key  =  {  post  .  id  }  >
<  img
src  =  {  `  ${  post  .  metadata  .  featured_image  }  ?w=800&auto=format  `  }
alt  =  {  post  .  title  }
/  >
<  h2  >  {  post  .  title  }  <  /  h2  >
<  p  >  {  post  .  metadata  .  teaser  }  <  /  p  >
<  a href  =  {  `  /blog/  ${  post  .  slug  }  `  }  >  Read   more  <  /  a  >
<  /  article  >
)  )  }
<  /  main  >
)
}


// app/blog/[slug]/page.tsx
export     default     async     function     BlogPost  (  {   params   }  :     {     params  :     {     slug  :   string   }     }  )     {
const     {     object  :   post   }     =     await   cosmic  .  objects
.  findOne  (  {     type  :     'blog-posts'  ,     slug  :   params  .  slug     }  )
.  props  (  'title,metadata'  )


return     (
<  article  >
<  h1  >  {  post  .  title  }  <  /  h1  >
{  /* If using html-textarea metafield: */  }
<  div dangerouslySetInnerHTML  =  {  {     __html  :   post  .  metadata  .  body     }  }     /  >
<  /  article  >
)
}
```


### Astro


```text
---
// src/pages/blog/index.astro
import { createBucketClient } from '@cosmicjs/sdk'


const cosmic = createBucketClient({
bucketSlug: import.meta.env.COSMIC_BUCKET_SLUG,
readKey: import.meta.env.COSMIC_READ_KEY,
})


const { objects: posts } = await cosmic.objects
.find({ type: 'blog-posts' })
.props('id,title,slug,metadata.teaser,metadata.featured_image,metadata.published_date')
.sort('-metadata.published_date')
.limit(10)
---


<html lang="en">
<body>
<main>
{posts.map((post) => (
<article>
<img
src={`${post.metadata.featured_image}?w=800&auto=format`}
alt={post.title}
/>
<h2><a href={`/blog/${post.slug}`}>{post.title}</a></h2>
<p>{post.metadata.teaser}</p>
</article>
))}
</main>
</body>
</html>
```


```text
---
// src/pages/blog/[slug].astro
export async function getStaticPaths() {
const { objects: posts } = await cosmic.objects
.find({ type: 'blog-posts' })
.props('slug')


return posts.map((post) => ({ params: { slug: post.slug } }))
}


const { slug } = Astro.params
const { object: post } = await cosmic.objects
.findOne({ type: 'blog-posts', slug })
.props('title,metadata')
---


<article>
<h1>{post.title}</h1>
<Fragment set:html={post.metadata.body} />
</article>
```


### Nuxt 3


```text
// composables/useCosmic.ts
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


export     const     useCosmic     =     (  )     =>     createBucketClient  (  {
bucketSlug  :     useRuntimeConfig  (  )  .  public  .  cosmicBucketSlug  ,
readKey  :     useRuntimeConfig  (  )  .  public  .  cosmicReadKey  ,
}  )
```


```text
<!-- pages/blog/index.vue -->
<script setup lang="ts">
const cosmic = useCosmic()


const { data: posts } = await useAsyncData('blog-posts', () =>
cosmic.objects
.find({ type: 'blog-posts' })
.sort('-metadata.published_date')
.limit(10)
.then((res) => res.objects)
)
</script>


<template>
<main>
<article v-for="post in posts" :key="post.id">
<img
:src="`${post.metadata.featured_image}?w=800&auto=format`"
:alt="post.title"
/>
<h2>
<NuxtLink :to="`/blog/${post.slug}`">{{ post.title }}</NuxtLink>
</h2>
<p>{{ post.metadata.teaser }}</p>
</article>
</main>
</template>
```


```text
<!-- pages/blog/[slug].vue -->
<script setup lang="ts">
const route = useRoute()
const cosmic = useCosmic()


const { data: post } = await useAsyncData(`post-${route.params.slug}`, () =>
cosmic.objects
.findOne({ type: 'blog-posts', slug: route.params.slug as string })
.props('title,metadata')
.then((res) => res.object)
)
</script>


<template>
<article>
<h1>{{ post?.title }}</h1>
<!-- HTML content from html-textarea metafield -->
<div v-html="post?.metadata.body" />
</article>
</template>
```


---


## Handling Contentful's Rich Text Renderer


This is the migration pain point teams consistently flag, so let's address it directly.


Contentful's rich text is a JSON document tree. To render it in React, you need` @contentful/rich-text-react-renderer` . For plain HTML you need` @contentful/rich-text-html-renderer` . The dependencies follow you everywhere your content goes.


When you migrate to Cosmic, you have a clean break. Here's the recommended approach:


**Option A: Migrate to HTML (fastest, most complete)**


Run` @contentful/rich-text-html-renderer` on every rich text field during migration and store the resulting HTML in a Cosmic` html-textarea` metafield. Your frontend renders it with` dangerouslySetInnerHTML` (React),` v-html` (Vue), or` set:html` (Astro). No CMS-specific package on the frontend. Done.


**Option B: Migrate to Markdown (cleaner long-term)**


Use a rich-text-to-Markdown converter, review the output for each entry, and store the result in a Cosmic` markdown` metafield. This takes more time upfront but gives you portable, human-readable content that's easy to edit directly in the Cosmic dashboard or via any Markdown editor.


**Handling embedded entries in rich text:**


Contentful's rich text supports embedded entries (e.g. a callout box, a code block component, a video embed). These are the trickiest to migrate because they're model-specific. Audit these before migration and decide:


- Convert to Markdown block elements (e.g. blockquote, fenced code)
- Replace with standalone metafields in the Cosmic Object Type
- Use Cosmic's` json` metafield type to preserve structured data


---


## Multi-Space to Multi-Bucket Migration


Many Contentful customers run multiple Spaces, one per brand, region, or product line. In Cosmic, each Space maps to a **Bucket** . Each Bucket has its own API credentials, content types, objects, and media library.


To migrate multiple Spaces, run the migration script once per Space/Bucket pair:


```text
const     MIGRATIONS     =     [
{
contentfulSpaceId  :     'space-id-brand-a'  ,
contentfulToken  :   process  .  env  .  CONTENTFUL_TOKEN_BRAND_A  ,
cosmicBucketSlug  :     'brand-a-production'  ,
cosmicWriteKey  :   process  .  env  .  COSMIC_WRITE_KEY_BRAND_A  ,
}  ,
{
contentfulSpaceId  :     'space-id-brand-b'  ,
contentfulToken  :   process  .  env  .  CONTENTFUL_TOKEN_BRAND_B  ,
cosmicBucketSlug  :     'brand-b-production'  ,
cosmicWriteKey  :   process  .  env  .  COSMIC_WRITE_KEY_BRAND_B  ,
}  ,
]


for     (  const   migration   of     MIGRATIONS  )     {
await     runMigration  (  migration  )
console  .  log  (  `  Completed:   ${  migration  .  cosmicBucketSlug  }  `  )
}
```


**Cosmic plan considerations for multi-bucket setups:**


- Builder ($49/mo): 2 Buckets
- Team ($299/mo): 3 Buckets
- Business ($499/mo): 5 Buckets
- Additional Buckets: $29/bucket/month on any paid plan
- Enterprise: Custom bucket limits


For large organizations migrating many Spaces, the Business or Enterprise plan is typically the right fit.


---


## Using a Cosmic Team Agent to Automate Bulk Migration


For large content libraries (thousands of entries), running a migration script manually can be slow and error-prone. Cosmic's Team Agents can automate bulk migration tasks directly from your Slack channel.


Here's how to set one up:


1. **Create a Team Agent** in your Cosmic dashboard with CMS write access and the API request capability.
2. **Write the migration prompt:** Describe the source (Contentful export JSON), the target Object Types, and the field mappings.
3. **Connect it to Slack:** The agent joins your channel and accepts commands like "migrate the next 100 blog posts" or "retry failed entries."
4. **Monitor progress:** The agent posts status updates as batches complete, flagging any entries that need manual review.


The agent can process entries in batches of up to 25 using Cosmic's batch operations API, making large migrations significantly faster than sequential single-entry scripts.


For enterprise migrations with complex data models,[book a call with Tony](https://calendly.com/tonyspiro/cosmic-intro) and we'll scope a custom migration plan.


---


## Pre-Launch Checklist


Before you cut traffic over to Cosmic, run through this checklist:


**Content**


- All content types mapped and created as Cosmic Object Types
- All entries migrated and spot-checked in the Cosmic dashboard
- Rich text converted to HTML or Markdown and rendering correctly in the frontend
- Author, category, and tag relationships resolved correctly
- Drafts vs. published status verified for all entries


**Media**


- All assets uploaded to Cosmic media library
- imgix URLs replacing Contentful CDN URLs in all content
- Image resizing parameters (e.g.` ?w=800&auto=format` ) applied where needed
- Video and file assets accessible via Cosmic URLs


**API and Code**


- Contentful SDK removed from all dependencies
- Cosmic SDK installed:` npm install @cosmicjs/sdk`
- All API calls updated from Contentful to Cosmic
- ` COSMIC_BUCKET_SLUG` ,` COSMIC_READ_KEY` ,` COSMIC_WRITE_KEY` environment variables set
- Contentful environment variables removed or deprecated
- Rich text renderer dependency (` @contentful/rich-text-react-renderer` ) removed


**Frontend**


- All pages rendering correctly with Cosmic data
- Dynamic routes (` \[slug\]` ) working for all content types
- Fallback and 404 states handling missing slugs gracefully
- Image` alt` text preserved from migration
- Open Graph and meta tags pulling from Cosmic metadata


**SEO**


- Existing URLs preserved (same slugs migrated from Contentful)
- Canonical tags correct
- Sitemap regenerated and submitted
- Redirects set up if any slugs changed during migration


**Testing**


- Full page audit on staging environment with Cosmic data
- API response times acceptable under load
- Webhook integrations (preview deploys, ISR) reconfigured for Cosmic
- Content editor workflow tested by a non-technical team member


---


## Ready to Make the Switch?


Migrating from Contentful to Cosmic is straightforward when you follow the steps above. Most teams complete a standard migration in a few days, not weeks. You get a simpler pricing model, a cleaner API, native AI agents that actually automate content work, and an imgix CDN for every media asset out of the box.


**Start free, no credit card required:**[Sign up at cosmicjs.com](https://app.cosmicjs.com/signup)


**Migrating a larger team or complex data model?**[Book a 30-minute intro call with Tony, our CEO](https://calendly.com/tonyspiro/cosmic-intro) and we'll walk through your specific setup.


**Explore more resources:**


- [Contentful alternative overview](https://www.cosmicjs.com/contentful-alternative)
- [Contentful vs. Cosmic feature comparison](https://www.cosmicjs.com/compare/contentful)
- [Cosmic docs](https://www.cosmicjs.com/docs)
- [Cosmic JavaScript SDK](https://www.npmjs.com/package/@cosmicjs/sdk)
