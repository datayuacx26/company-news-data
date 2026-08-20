---
schema_version: "1.0.0"
document_id: "4e66134fb04382efa82f7ef888fceaa44bddcbf1b666c773935bbc876f1a252f"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/migrate-from-sanity-to-cosmic"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-08-10T06:05:12.278252+00:00"
fetched_at: "2026-08-10T06:05:15.916664+00:00"
content_hash: "sha256:5a75de23b96aec1b547a2d458977534c1600e9726eaa97bf0a26dd72940c00f6"
---

# Migrate from Sanity to Cosmic: A Complete Developer Guide

If your team has been building on Sanity and the friction is starting to outweigh the flexibility, you are not alone. GROQ is a powerful query language, but it is also one more proprietary system your team has to learn and maintain. Sanity Studio is a flexible editing environment, but it is also a React app you have to configure and deploy separately. And at scale, per-seat pricing, add-on SSO costs, and multi-dataset fees add up fast.


This guide walks you through a complete migration from Sanity to[Cosmic](https://www.cosmicjs.com/) , an AI-powered headless CMS built around a clean REST API, a hosted editing interface, and AI agents built into the platform.


Before you start, take a look at the[Sanity alternative overview](https://www.cosmicjs.com/sanity-alternative) and the[Sanity vs. Cosmic comparison](https://www.cosmicjs.com/compare/sanity) to understand what you are gaining in the switch. If you have already migrated from another platform, the[Contentful migration guide](https://www.cosmicjs.com/blog/migrate-from-contentful-to-cosmic) and[Ghost migration guide](https://www.cosmicjs.com/blog/migrate-from-ghost-to-cosmic) follow the same structure.


---


## Why Teams Migrate from Sanity to Cosmic


### 1. GROQ Query Complexity


GROQ (Graph-Relational Object Queries) is Sanity's custom query language. It is expressive and capable. It is also something only Sanity uses. Every developer who joins your team has to learn it from scratch, every query has to be tested in a GROQ-specific sandbox, and when you leave Sanity, that investment disappears.


Cosmic uses a clean REST API with URL query parameters and a TypeScript SDK. Standard HTTP. Concepts your team already knows. Switching developers or onboarding contractors does not require a GROQ bootcamp.


### 2. Per-Seat Pricing at Scale


Sanity's Growth plan is $15/seat/month. That sounds manageable for a small team, but add SSO ($1,399/month), dedicated support ($799/month), and extra datasets ($999/month each), and the cost for a mid-sized team can reach several thousand dollars a month before you hit enterprise tier.


Cosmic pricing is flat per plan:


Plan Price Team Members Objects


**Free** $0/month 2 1,000


**Builder** $49/month 3 5,000


**Team** $299/month 5 20,000


**Business** $499/month 10 50,000


**Enterprise** Custom Custom Custom


Additional users are $29/user/month. SSO is included on the Enterprise plan.


### 3. Steep Learning Curve for Content Editors


Sanity Studio is powerful for developers. For non-technical content editors, it can be confusing: GROQ previews, schema-driven UI, the concept of "document references" and "portable text blocks" are all developer abstractions leaking into the editorial experience.


Cosmic's dashboard is purpose-built for both audiences. Developers define the schema. Editors fill in structured fields. The interface does not expose the underlying data model to people who should not need to know about it.


### 4. Cold API Performance on Sanity's Free Tier


Sanity's free tier serves content from cold API instances that can have noticeable latency on the first request after a period of inactivity. For production sites or teams testing performance, this creates misleading benchmarks.


Cosmic's API is globally distributed and warmed on all plans, including free.


---


## Sanity-to-Cosmic Concept Mapping


Before writing any migration code, it helps to understand how Sanity's mental model maps onto Cosmic's.


Sanity Concept Cosmic Equivalent Notes


**Schema** Object Type Defined in the dashboard, not in code


**Document** Object The content record


**Field** Metafield Rich type system: text, markdown, file, object, etc.


**Portable Text**` markdown` or` html-textarea` metafield Clean, portable, no renderer dependency


**Dataset** Bucket Each Bucket has its own API keys and content


**Sanity Studio** Cosmic Dashboard Fully hosted, no deployment required


**GROQ query** REST API query with` ?query=` params Standard HTTP, no new language to learn


**Image asset** Media file (imgix CDN) Automatic CDN delivery, on-the-fly transforms


**Reference field**` object` metafield Single relationship


**Array of references**` objects` metafield Multi-relationship


**Slug field** Object` slug` property Top-level on every Cosmic Object


The biggest conceptual shift: in Sanity, your schema is code (TypeScript files in your Studio). In Cosmic, your schema is configuration defined in the dashboard. This means no schema deployments, no code reviews for content model changes, and no divergence between what editors see and what is in the codebase.


---


## Step-by-Step Migration Guide


### Step 1: Export Sanity Content via GROQ or Dataset Export


Sanity provides two ways to export your content.


**Option A: Dataset Export (recommended for full migration)**


```text
npm     install   -g @sanity/cli
sanity login
sanity dataset   export   production ./sanity-export.tar.gz
```


This exports everything: all documents, all drafts, all asset references. Extract the archive and you will find an` ndjson` file with one JSON document per line.


**Option B: GROQ export via the API (selective export)**


For partial migrations or when you only need specific document types:


```text
// export-sanity.js
const   projectId   =   process  .  env  .  SANITY_PROJECT_ID
const   dataset   =     'production'
const   token   =   process  .  env  .  SANITY_TOKEN


// Export all blog posts via GROQ
const   query   =     encodeURIComponent  (  `  *[_type == "blogPost"]{
_id,
_type,
title,
slug,
body,
mainImage,
author->{ name, slug },
publishedAt,
categories[]->{ title, slug }
}  `  )


const   url   =     `  https://  ${  projectId  }  .api.sanity.io/v2021-10-21/data/query/  ${  dataset  }  ?query=  ${  query  }  `


const   response   =     await     fetch  (  url  ,     {
headers  :     {     Authorization  :     `  Bearer   ${  token  }  `     }  ,
}  )


const     {   result   }     =     await   response  .  json  (  )
const   fs   =     await     import  (  'fs'  )
fs  .  writeFileSync  (  './sanity-export.json'  ,     JSON  .  stringify  (  result  ,     null  ,     2  )  )
console  .  log  (  `  Exported   ${  result  .  length  }   documents  `  )
```


---


### Step 2: Map Sanity Schemas to Cosmic Object Types


Look at your Sanity schema files (typically in` schemas/` ) and create corresponding Object Types in the Cosmic dashboard.


**Common Sanity field types and their Cosmic equivalents:**


Sanity Type Cosmic Metafield Type Notes


` string`` text` Single line


` text`` textarea` Multi-line plain text


` array` of` block` (Portable Text)` markdown` or` html-textarea` See Portable Text section below


` number`` number`


` boolean`` switch`


` datetime`` date`


` image`` file` Cosmic delivers via imgix CDN


` file`` file`


` array` of` image`` files`


` reference`` object` Pass related Object slug


` array` of` reference`` objects` Pass array of Object slugs


` object` (nested) Parent metafield or` json`


` array` of` string`` multi-select` or` objects`


` slug` Object` slug` property Top-level, not a metafield


**You can also create Object Types via the Cosmic REST API:**


```text
// create-object-types.js
const     COSMIC_BUCKET_SLUG     =   process  .  env  .  COSMIC_BUCKET_SLUG
const     COSMIC_WRITE_KEY     =   process  .  env  .  COSMIC_WRITE_KEY


async     function     createObjectType  (  schema  )     {
const   response   =     await     fetch  (
`  https://api.cosmicjs.com/v3/buckets/  ${  COSMIC_BUCKET_SLUG  }  /object-types  `  ,
{
method  :     'POST'  ,
headers  :     {
'Content-Type'  :     'application/json'  ,
Authorization  :     `  Bearer   ${  COSMIC_WRITE_KEY  }  `  ,
}  ,
body  :     JSON  .  stringify  (  schema  )  ,
}
)
return   response  .  json  (  )
}


// Create a Blog Posts type
await     createObjectType  (  {
title  :     'Blog Posts'  ,
slug  :     'blog-posts'  ,
singular  :     'Blog Post'  ,
metafields  :     [
{     key  :     'body'  ,     title  :     'Body'  ,     type  :     'markdown'     }  ,
{     key  :     'published_date'  ,     title  :     'Published Date'  ,     type  :     'date'     }  ,
{     key  :     'main_image'  ,     title  :     'Main Image'  ,     type  :     'file'     }  ,
{     key  :     'author'  ,     title  :     'Author'  ,     type  :     'object'  ,     object_type  :     'authors'     }  ,
{     key  :     'categories'  ,     title  :     'Categories'  ,     type  :     'objects'  ,     object_type  :     'categories'     }  ,
{     key  :     'excerpt'  ,     title  :     'Excerpt'  ,     type  :     'textarea'     }  ,
]  ,
}  )
```


---


### Step 3: Migrate Content Using Node.js Script with Cosmic REST API


With your Object Types created and your Sanity export ready, write a migration script.


**Handling Portable Text (Sanity's Rich Text Format)**


Portable Text is Sanity's structured rich text format. It stores content as a JSON array of block nodes. Before you can store it in Cosmic, you need to convert it.


```text
npm     install   @portabletext/to-html
# or for Markdown conversion:
npm     install   @sanity/block-content-to-markdown
```


```text
// migrate-content.js
import     {   toHTML   }     from     '@portabletext/to-html'
import     {   createBucketClient   }     from     '@cosmicjs/sdk'
import     exportedDocs     from     './sanity-export.json'     assert     {     type  :     'json'     }


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY  ,
}  )


async     function     migrateBlogPosts  (  docs  ,   mediaMap  )     {
const   blogPosts   =   docs  .  filter  (  (  doc  )     =>   doc  .  _type     ===     'blogPost'  )
console  .  log  (  `  Migrating   ${  blogPosts  .  length  }   blog posts...  `  )


for     (  const   post   of   blogPosts  )     {
// Convert Portable Text body to HTML
const   bodyHtml   =   post  .  body     ?     toHTML  (  post  .  body  )     :     ''


// Resolve main image to migrated Cosmic URL
const   mainImage   =   post  .  mainImage  ?.  asset  ?.  _ref
?   mediaMap  [  post  .  mainImage  .  asset  .  _ref  ]
:     null


try     {
await   cosmic  .  objects  .  insertOne  (  {
type  :     'blog-posts'  ,
title  :   post  .  title  ,
slug  :   post  .  slug  ?.  current  ,
status  :     'published'  ,
metadata  :     {
body  :   bodyHtml  ,
excerpt  :   post  .  excerpt     ||     ''  ,
published_date  :   post  .  publishedAt  ?.  split  (  'T'  )  [  0  ]     ||     null  ,
main_image  :   mainImage  ,
// Author and category relationships resolved by slug
author  :   post  .  author  ?.  slug  ?.  current   ||     null  ,
categories  :   post  .  categories  ?.  map  (  (  c  )     =>   c  .  slug  ?.  current  )     ||     [  ]  ,
}  ,
}  )
console  .  log  (  `  ✓ Migrated:   ${  post  .  title  }  `  )
}     catch     (  err  )     {
console  .  error  (  `  ✗ Failed:   ${  post  .  title  }  `  ,   err  .  message  )
}
}
}


migrateBlogPosts  (  exportedDocs  ,   mediaMap  )
```


**Batch operations for large content sets:**


For content libraries with hundreds of documents, use Cosmic's batch operations to migrate up to 25 objects at a time:


```text
async     function     migrateBatch  (  posts  ,   mediaMap  )     {
const     BATCH_SIZE     =     25


for     (  let   i   =     0  ;   i   <   posts  .  length  ;   i   +=     BATCH_SIZE  )     {
const   batch   =   posts  .  slice  (  i  ,   i   +     BATCH_SIZE  )


const   operations   =   batch  .  map  (  (  post  )     =>     (  {
method  :     'create'  ,
type  :     'blog-posts'  ,
title  :   post  .  title  ,
slug  :   post  .  slug  ?.  current  ,
status  :     'published'  ,
metadata  :     {
body  :   post  .  body     ?     toHTML  (  post  .  body  )     :     ''  ,
main_image  :   mediaMap  [  post  .  mainImage  ?.  asset  ?.  _ref  ]     ||     null  ,
}  ,
}  )  )


const   result   =     await     fetch  (
`  https://api.cosmicjs.com/v3/buckets/  ${  process  .  env  .  COSMIC_BUCKET_SLUG  }  /objects/batch  `  ,
{
method  :     'POST'  ,
headers  :     {
'Content-Type'  :     'application/json'  ,
Authorization  :     `  Bearer   ${  process  .  env  .  COSMIC_WRITE_KEY  }  `  ,
}  ,
body  :     JSON  .  stringify  (  {   operations   }  )  ,
}
)


const   data   =     await   result  .  json  (  )
console  .  log  (  `  Batch   ${  Math  .  floor  (  i   /     BATCH_SIZE  )     +     1  }  :   ${  data  .  objects  ?.  length  }   migrated  `  )
}
}
```


---


### Step 4: Migrate Media Assets


Sanity stores assets with` _ref` IDs in the format` image-{hash}-{dimensions}-{format}` . Your dataset export includes an` assets` object mapping those IDs to CDN URLs.


```text
// migrate-media.js
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY  ,
}  )


async     function     migrateSanityAssets  (  sanityAssets  )     {
const   mediaMap   =     {  }     // Maps Sanity asset _ref to Cosmic imgix_url


for     (  const     [  assetRef  ,   asset  ]     of     Object  .  entries  (  sanityAssets  )  )     {
// Sanity CDN URL format
const   sanityUrl   =     `  https://cdn.sanity.io/images/  ${  process  .  env  .  SANITY_PROJECT_ID  }  /production/  ${  asset  .  path  }  `


try     {
const     {   media   }     =     await   cosmic  .  media  .  insertOne  (  {
media  :     {
url  :   sanityUrl  ,
name  :   asset  .  originalFilename     ||   assetRef  ,
}  ,
}  )


mediaMap  [  assetRef  ]     =   media  .  imgix_url
console  .  log  (  `  ✓ Migrated:   ${  asset  .  originalFilename  }  `  )
}     catch     (  err  )     {
console  .  error  (  `  ✗ Failed:   ${  assetRef  }  `  ,   err  .  message  )
}
}


return   mediaMap
}
```


Every migrated asset gets a permanent Cosmic imgix URL with on-the-fly resizing and format conversion built in. No separate CDN configuration needed.


---


### Step 5: Update Front-End Queries (Remove GROQ, Use Cosmic SDK or REST API)


This is where day-to-day developer experience improves most noticeably. Replace GROQ queries and the Sanity client with Cosmic's REST API and JavaScript SDK.


**Before (Sanity client + GROQ):**


```text
import     {   createClient   }     from     '@sanity/client'


const   client   =     createClient  (  {
projectId  :   process  .  env  .  SANITY_PROJECT_ID  ,
dataset  :     'production'  ,
apiVersion  :     '2024-01-01'  ,
useCdn  :     true  ,
}  )


// GROQ query - proprietary syntax, Sanity-only knowledge
const   posts   =     await   client  .  fetch  (
`  *[_type == "blogPost" && !(_id in path("drafts.**"))] | order(publishedAt desc)[0..9] {
_id,
title,
"slug": slug.current,
excerpt,
publishedAt,
"mainImageUrl": mainImage.asset->url,
"authorName": author->name
}  `
)
```


**After (Cosmic SDK):**


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  ,
}  )


// Standard SDK - works like any other API client
const     {     objects  :   posts   }     =     await   cosmic  .  objects
.  find  (  {     type  :     'blog-posts'     }  )
.  props  (  'id,title,slug,metadata.excerpt,metadata.published_date,metadata.main_image,metadata.author'  )
.  sort  (  '-metadata.published_date'  )
.  limit  (  10  )


// posts[0].metadata.main_image is a direct imgix_url
// No GROQ, no ->dereferences, no projection syntax
```


**Using the REST API directly:**


```text
const   bucketSlug   =   process  .  env  .  COSMIC_BUCKET_SLUG
const   readKey   =   process  .  env  .  COSMIC_READ_KEY


const   res   =     await     fetch  (
`  https://api.cosmicjs.com/v3/buckets/  ${  bucketSlug  }  /objects  `     +
`  ?query={"type":"blog-posts"}  `     +
`  &read_key=  ${  readKey  }  `     +
`  &props=id,title,slug,metadata.excerpt,metadata.published_date,metadata.main_image  `     +
`  &sort=-metadata.published_date  `     +
`  &limit=10  `
)
const     {     objects  :   posts   }     =     await   res  .  json  (  )
```


---


### Step 6: Set Up Webhooks for Content Sync


Cosmic supports outgoing webhooks that fire on` object.created` ,` object.updated` , and` object.deleted` events. Use these to trigger ISR revalidation in Next.js, rebuild jobs in Netlify or Vercel, or custom sync pipelines.


**Set up a webhook in the Cosmic dashboard:**


1. Go to **Bucket Settings > Webhooks**
2. Add your endpoint URL (e.g.` https://yoursite.com/api/revalidate` )
3. Select trigger events (` object.published` is the most common)
4. Save. Webhooks are available as a $99/month add-on, or included in the $199/month bundle with Localization, Revision History, and Automatic Backups.


**Next.js revalidation endpoint:**


```text
// app/api/revalidate/route.ts
import     {   revalidatePath   }     from     'next/cache'
import     {     NextRequest  ,     NextResponse     }     from     'next/server'


export     async     function     POST  (  req  :     NextRequest  )     {
const   secret   =   req  .  headers  .  get  (  'x-cosmic-webhook-secret'  )


if     (  secret   !==   process  .  env  .  WEBHOOK_SECRET  )     {
return     NextResponse  .  json  (  {     error  :     'Unauthorized'     }  ,     {     status  :     401     }  )
}


const   body   =     await   req  .  json  (  )
const   slug   =   body  .  data  ?.  slug


if     (  slug  )     {
revalidatePath  (  `  /blog/  ${  slug  }  `  )
revalidatePath  (  '/blog'  )
}


return     NextResponse  .  json  (  {     revalidated  :     true     }  )
}
```


---


## Framework-Specific Code Examples


### Next.js (App Router)


```text
// lib/cosmic.ts
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


export     const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  !  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
}  )


export     interface     BlogPost     {
id  :     string
title  :     string
slug  :     string
metadata  :     {
body  :     string
excerpt  :     string
published_date  :     string
main_image  :     {   imgix_url  :     string     }
author  :     {   title  :     string     }
}
}
```


```text
// app/blog/page.tsx
import     {   cosmic   }     from     '@/lib/cosmic'


export     default     async     function     BlogPage  (  )     {
const     {   objects  :   posts   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'     }  )
.  props  (  'id,title,slug,metadata.excerpt,metadata.published_date,metadata.main_image'  )
.  sort  (  '-metadata.published_date'  )
.  limit  (  10  )


return     (
<  main  >
{  posts  .  map  (  (  post  )     =>     (
<  article key  =  {  post  .  id  }  >
<  img
src  =  {  `  ${  post  .  metadata  .  main_image  ?.  imgix_url  }  ?w=800&auto=format  `  }
alt  =  {  post  .  title  }
/  >
<  h2  >  <  a href  =  {  `  /blog/  ${  post  .  slug  }  `  }  >  {  post  .  title  }  <  /  a  >  <  /  h2  >
<  p  >  {  post  .  metadata  .  excerpt  }  <  /  p  >
<  /  article  >
)  )  }
<  /  main  >
)
}
```


```text
// app/blog/[slug]/page.tsx
import     {   cosmic   }     from     '@/lib/cosmic'


export     async     function     generateStaticParams  (  )     {
const     {   objects  :   posts   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'     }  )
.  props  (  'slug'  )


return   posts  .  map  (  (  post  )     =>     (  {   slug  :   post  .  slug     }  )  )
}


export     default     async     function     BlogPost  (  {   params   }  :     {   params  :     {   slug  :     string     }     }  )     {
const     {   object  :   post   }     =     await   cosmic  .  objects
.  findOne  (  {   type  :     'blog-posts'  ,   slug  :   params  .  slug     }  )
.  props  (  'title,metadata'  )


return     (
<  article  >
<  h1  >  {  post  .  title  }  <  /  h1  >
<  div dangerouslySetInnerHTML  =  {  {   __html  :   post  .  metadata  .  body     }  }     /  >
<  /  article  >
)
}
```


### Nuxt 3


```text
// composables/useCosmic.ts
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


export     const     useCosmic     =     (  )     =>
createBucketClient  (  {
bucketSlug  :     useRuntimeConfig  (  )  .  public  .  cosmicBucketSlug     as     string  ,
readKey  :     useRuntimeConfig  (  )  .  public  .  cosmicReadKey     as     string  ,
}  )
```


```text
<!-- pages/blog/index.vue -->
<script setup lang="ts">
const cosmic = useCosmic()


const { data: posts } = await useAsyncData('blog-posts', () =>
cosmic.objects
.find({ type: 'blog-posts' })
.props('id,title,slug,metadata.excerpt,metadata.published_date,metadata.main_image')
.sort('-metadata.published_date')
.limit(10)
.then((res) => res.objects)
)
</script>


<template>
<main>
<article v-for="post in posts" :key="post.id">
<img
:src="`${post.metadata.main_image?.imgix_url}?w=800&auto=format`"
:alt="post.title"
/>
<h2>
<NuxtLink :to="`/blog/${post.slug}`">{{ post.title }}</NuxtLink>
</h2>
<p>{{ post.metadata.excerpt }}</p>
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
<div v-html="post?.metadata.body" />
</article>
</template>
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
.sort('-metadata.published_date')
.limit(10)
---


<main>
{posts.map((post) => (
<article>
<img
src={`${post.metadata.main_image?.imgix_url}?w=800&auto=format`}
alt={post.title}
/>
<h2><a href={`/blog/${post.slug}`}>{post.title}</a></h2>
<p>{post.metadata.excerpt}</p>
</article>
))}
</main>
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


---


## Multi-Dataset to Multi-Bucket Setup


Sanity charges $999/month per additional dataset beyond the included ones. Many teams use multiple datasets for staging, production, and regional variants.


In Cosmic, each dataset is a **Bucket** . Each Bucket has its own API keys, content model, media library, and team members. The pricing is much more accessible:


- Builder ($49/mo): 2 Buckets
- Team ($299/mo): 3 Buckets
- Business ($499/mo): 5 Buckets
- Additional Buckets: $29/bucket/month on any paid plan


To migrate multiple Sanity datasets, run your migration script once per dataset/bucket pair:


```text
const     DATASET_MIGRATIONS     =     [
{
sanityDataset  :     'production'  ,
sanityToken  :   process  .  env  .  SANITY_TOKEN_PROD  ,
cosmicBucketSlug  :     'my-project-production'  ,
cosmicWriteKey  :   process  .  env  .  COSMIC_WRITE_KEY_PROD  ,
}  ,
{
sanityDataset  :     'staging'  ,
sanityToken  :   process  .  env  .  SANITY_TOKEN_STAGING  ,
cosmicBucketSlug  :     'my-project-staging'  ,
cosmicWriteKey  :   process  .  env  .  COSMIC_WRITE_KEY_STAGING  ,
}  ,
]


for     (  const   migration   of     DATASET_MIGRATIONS  )     {
console  .  log  (  `  Starting migration:   ${  migration  .  sanityDataset  }   →   ${  migration  .  cosmicBucketSlug  }  `  )
await     runMigration  (  migration  )
console  .  log  (  `  Completed:   ${  migration  .  cosmicBucketSlug  }  `  )
}
```


---


## Using a Cosmic Team Agent for Bulk Content Migration


For large Sanity content libraries with thousands of documents, running migration scripts manually can be slow and error-prone. Cosmic's AI Team Agents can automate bulk migration tasks directly from your Slack channel.


Here's how to set one up:


1. **Create a Team Agent** in your Cosmic dashboard with` cms_write` and` api_request` capabilities enabled.
2. **Write the migration prompt** describing the source (your Sanity export JSON or API), the target Cosmic Object Types, and the field mappings.
3. **Connect the agent to Slack.** It joins your channel and accepts commands like "migrate the next 50 blog posts from the Sanity export" or "retry the 12 entries that failed."
4. **Monitor progress.** The agent posts status updates as batches complete, flagging entries that need manual review due to missing fields or relationship mismatches.


The agent can process entries in batches of 25 using Cosmic's batch operations API, significantly faster than running sequential single-entry scripts.


For enterprise migrations with complex Sanity schemas and large content libraries,[book a call with Tony](https://calendly.com/tonyspiro/cosmic-intro) and we will scope a custom migration plan.


---


## Pre-Launch Checklist


Before you cut traffic to Cosmic, run through this checklist:


**Content Model**


- All Sanity schemas recreated as Cosmic Object Types
- Metafield types match the original Sanity field types
- Relationship fields (` object` ,` objects` ) reference the correct Object Types
- Slug fields are set and unique across each Object Type


**Content Data**


- All documents migrated and spot-checked in the Cosmic dashboard
- Portable Text converted to HTML or Markdown and rendering correctly in the frontend
- Draft vs. published status set correctly for all Objects
- Author, category, and tag relationships resolving to the correct related Objects
- Published dates preserved from Sanity` publishedAt` fields


**Media**


- All Sanity image assets migrated to Cosmic media library
- ` mediaMap` correctly resolves Sanity asset` _ref` IDs to Cosmic imgix URLs
- Image resizing parameters (e.g.` ?w=800&auto=format` ) applied in frontend templates
- File and video assets accessible via Cosmic URLs
- ` alt` text preserved for all images


**API and Code**


- ` @sanity/client` removed from all dependencies
- Cosmic SDK installed:` npm install @cosmicjs/sdk`
- All GROQ queries replaced with Cosmic REST API or SDK calls
- ` COSMIC_BUCKET_SLUG` and` COSMIC_READ_KEY` environment variables set in all environments
- Sanity environment variables (` SANITY_PROJECT_ID` ,` SANITY_DATASET` , etc.) removed or deprecated
- ` @portabletext/to-html` or similar Portable Text renderer removed from frontend if no longer needed


**Frontend**


- All pages rendering correctly with Cosmic data
- Dynamic routes (` \[slug\]` ) working for all Object Types
- Fallback and 404 states handling missing slugs gracefully
- Open Graph and meta tags pulling from Cosmic metadata fields
- Responsive images using imgix URL parameters


**SEO**


- All existing URLs preserved (same slugs migrated from Sanity)
- Canonical tags correct
- Sitemap regenerated and submitted to Google Search Console
- 301 redirects set up for any slugs that changed during migration


**Webhooks and Automation**


- Sanity webhooks removed from hosting provider
- Cosmic webhooks configured for ISR revalidation or rebuild triggers
- Webhook endpoint authenticated and tested
- Content preview workflow tested by a non-technical team member


**Testing**


- Full page audit on staging environment with Cosmic data
- API response times verified under realistic load
- Content editor workflow tested end-to-end: create, publish, verify on frontend
- Rollback plan documented in case of production issues


---


## Ready to Make the Switch?


Migrating from Sanity to Cosmic removes a proprietary query language from your stack, eliminates a separate Studio deployment from your infrastructure, and gives your editors a purpose-built interface they can actually use without developer help.


Most teams complete a standard migration in a few days. You come out the other side with a simpler codebase, predictable flat-rate pricing, and AI agents that automate content work rather than just assisting with it.


**Explore related resources:**


- [Sanity alternative overview](https://www.cosmicjs.com/sanity-alternative)
- [Sanity vs. Cosmic feature comparison](https://www.cosmicjs.com/compare/sanity)
- [Contentful migration guide](https://www.cosmicjs.com/blog/migrate-from-contentful-to-cosmic)
- [Ghost migration guide](https://www.cosmicjs.com/blog/migrate-from-ghost-to-cosmic)
- [Cosmic JavaScript SDK](https://www.npmjs.com/package/@cosmicjs/sdk)
- [Cosmic docs](https://www.cosmicjs.com/docs)


**Start free, no credit card required:**[Sign up at cosmicjs.com](https://app.cosmicjs.com/signup)


**Migrating a larger team or complex Sanity schema?**[Book a 30-minute intro call with Tony, our CEO](https://calendly.com/tonyspiro/cosmic-intro) and we will walk through your specific setup.
