---
schema_version: "1.0.0"
document_id: "d4d54d762b328ed26c74dcbf931cebaad3da81e069b073aa586be4a7b595830a"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/migrate-from-ghost-to-cosmic"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-08-10T06:05:12.278252+00:00"
fetched_at: "2026-08-10T06:05:15.916664+00:00"
content_hash: "sha256:31b97b1a043704e15bc7186bcf98ca40657a958ecf0e643c24776a29a1bb75c0"
---

# How to Migrate from Ghost to Cosmic: A Complete Developer Guide

Ghost is a solid blogging platform. If you're self-hosting it on a VPS, running it in Docker, or using Ghost Pro, it probably got you pretty far. But at some point, many teams hit the same set of walls: content modeling is too rigid for anything beyond posts and pages, the API doesn't flex the way modern front-ends need, and managing more than one brand or property is painful.


This guide is for developers who've already decided to make the move. We'll cover every step: exporting your Ghost content, mapping it to Cosmic Object Types, migrating posts, pages, tags, and authors, updating your front-end to use the Cosmic JavaScript SDK, setting up webhooks, and handling multi-brand setups.


---


## Why Teams Migrate Away from Ghost


Ghost was built for publishing. It does that job well. But modern content teams often need more than a publishing tool, and that's where Ghost starts to show its limits.


### Content modeling is an afterthought


Ghost gives you posts, pages, tags, and authors. That's it. If you need custom content types (product listings, FAQs, event pages, team bios, course modules), you're either shoehorning everything into post metadata, building a custom Ghost theme with handlebars logic, or reaching for a separate data source. None of these scale cleanly.


Cosmic is built around flexible Object Types. You define exactly what fields each content type needs, enforce validation, and query it all through one clean API. You can model anything from a simple blog to a multi-product content operation without touching your front-end code.


### The API has gaps


Ghost's Content API works for basic queries. But it has rate limits, no native relation querying across custom types, and it's tightly coupled to Ghost's data model. If your front-end needs something the API doesn't expose, you're writing custom endpoints against the Ghost Admin API or hitting the database directly.


Cosmic's JavaScript SDK is purpose-built for headless delivery. Every Object Type you define is immediately queryable. You can filter by any metadata field, sort, paginate, and compose multi-type queries without any additional server configuration.


### Scaling and multi-brand management is hard


Running Ghost at scale means managing Node.js processes, MySQL databases, reverse proxies, and SSL certificates per site. For teams running multiple brands or properties, that operational overhead compounds quickly.


Cosmic handles infrastructure for you. Each brand or property gets its own Bucket (Cosmic's isolated content workspace), and you manage everything through one dashboard. No more per-site deployments just to launch a new publication.


### AI integration is limited


Ghost has a basic Koenig editor. It doesn't have native AI content generation, AI-assisted workflows, or an agent system that can act on your content autonomously. Cosmic has AI agents built directly into the platform: you can schedule content generation, automate updates, and build content workflows without any third-party integrations.


---


## What's Different About Cosmic


Before you migrate, it helps to understand how Cosmic's mental model differs from Ghost's.


- Ghost **Post** maps to a Cosmic **Object** (in a custom Object Type, e.g. "Blog Posts")
- Ghost **Page** maps to a Cosmic **Object** (in a "Pages" Object Type)
- Ghost **Tag** maps to a Cosmic **Object** (in a "Tags" Object Type, or a metadata field)
- Ghost **Author** maps to a Cosmic **Object** (in an "Authors" Object Type)
- Ghost **Custom fields** map to Cosmic **Metafields** on any Object Type
- Ghost **Site** maps to a Cosmic **Bucket**
- Ghost **Content API** maps to the **Cosmic JavaScript SDK**
- Ghost **Theme / Handlebars** is replaced by your own front-end (Next.js, Nuxt, Astro, etc.)


The key shift: Cosmic is not a theming engine. It's a content API. Your front-end is completely decoupled, which gives you full control over rendering, caching, and deployment.


---


## Option A: Automate the Migration with a Cosmic Team Agent


Before diving into the manual migration steps, it's worth knowing there's a faster path: use a Cosmic Team Agent to call the Ghost Admin API directly and migrate your content automatically.


Cosmic's Team Agents are AI agents that run inside your Cosmic dashboard and can make HTTP requests to external APIs. That means you can instruct an agent to:


1. Fetch all posts, pages, tags, and authors from your Ghost Admin API
2. Transform them into Cosmic Object schema
3. Create the Objects in your Cosmic Bucket via the JavaScript SDK


Here's how to set this up:


### Configure your Ghost Admin API credentials


In your Ghost Admin dashboard, go to **Settings > Integrations > Add custom integration** . Copy the **Admin API Key** and your Ghost API URL (e.g.` https://yoursite.ghost.io` ).


### Create a Team Agent in Cosmic


In your Cosmic dashboard, go to **Agents** and create a new Team Agent. Give it a prompt like:


```text
You   are a content migration agent  .     Your   job is to fetch content   from   the   Ghost     Admin     API
and create matching   Objects     in     this     Cosmic     Bucket  .


Ghost     API     URL  :   https  :  /  /  yoursite  .  ghost  .  io
Ghost     Admin     API     Key  :     [  your  -  key  ]


Steps  :
1.     Fetch   all posts   from     GET     /  ghost  /  api  /  admin  /  posts  /  ?  limit  =  all  &  include  =  tags  ,  authors
2.     For   each post  ,   create a blog  -  posts   Object     in     Cosmic     with   title  ,   slug  ,     HTML   content  ,
published date  ,   and tag  /  author associations
3.     Fetch   all pages   from     GET     /  ghost  /  api  /  admin  /  pages  /  ?  limit  =  all
4.     Create   matching pages   Objects     in     Cosmic
5.     Report   how many objects were created and flag any errors
```


The agent will use its` api_request` capability to call your Ghost endpoint, then use the Cosmic SDK to write the Objects directly into your Bucket. No local scripts, no environment setup.


This approach is especially useful for large migrations (hundreds of posts) or teams that want a repeatable, auditable process without writing custom Node.js scripts.


> **Note:** Keep your Ghost Admin API key secure. Store it as a secret in the Cosmic agent configuration rather than pasting it into the prompt directly.


---


## Option B: Manual Migration with the Cosmic JavaScript SDK


If you prefer a script-based migration you can run locally, the Cosmic JavaScript SDK gives you a clean, typed interface to your Bucket.


### Install the SDK


```text
npm     install   @cosmicjs/sdk
```


### Initialize the client


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY
}  )  ;
```


---


## Step 1: Export Your Content from Ghost


Ghost makes data export straightforward. From your Ghost Admin dashboard:


1. Go to **Settings > Labs**
2. Click **Export your content**
3. Ghost will download a` .json` file containing all your posts, pages, tags, authors, and settings


This JSON export is the source of truth for your migration. Keep it safe.


Here's a simplified example of what a Ghost export looks like:


```text
{
"db"  :     [  {
"meta"  :     {     "version"  :     "5.x.x"     }  ,
"data"  :     {
"posts"  :     [
{
"id"  :     "abc123"  ,
"title"  :     "My First Post"  ,
"slug"  :     "my-first-post"  ,
"html"  :     "<p>Post content here</p>"  ,
"plaintext"  :     "Post content here"  ,
"feature_image"  :     "https://yoursite.com/content/images/..."  ,
"published_at"  :     "2024-03-01T00:00:00.000Z"  ,
"status"  :     "published"  ,
"tags"  :     [  {  "name"  :     "Engineering"  }  ]  ,
"authors"  :     [  {  "name"  :     "Jane Smith"  ,     "email"  :     "jane@example.com"  }  ]
}
]  ,
"tags"  :     [  ...  ]  ,
"users"  :     [  ...  ]
}
}  ]
}
```


**Note on images:** Ghost stores images in its` content/images` directory. You'll need to re-upload these to Cosmic's media library (or another CDN) during migration. For large image libraries, use the Cosmic SDK's media upload method to automate this.


---


## Step 2: Map Ghost Content Types to Cosmic Object Types


Before writing any migration code, plan your schema. Here's a recommended mapping:


### Blog Posts Object Type


Create an Object Type in Cosmic called` blog-posts` with these metafields:


- ` title` (text)
- ` slug` (text, unique)
- ` content` (markdown or html-textarea)
- ` excerpt` (textarea)
- ` featured_image` (file)
- ` published_date` (date)
- ` author` (object, relates to` authors` )
- ` tags` (objects, relates to` tags` )
- ` status` (select: published, draft)


### Authors Object Type


Create` authors` with:


- ` name` (text)
- ` bio` (textarea)
- ` email` (text)
- ` avatar` (file)


### Tags Object Type


Create` tags` with:


- ` name` (text)
- ` description` (textarea)


### Pages Object Type


Create` pages` with:


- ` title` (text)
- ` slug` (text, unique)
- ` content` (html-textarea)
- ` meta_description` (textarea)


You can create these Object Types directly in the Cosmic dashboard under **Object Types** , or via the SDK if you're scripting the full migration.


---


## Step 3: Migrate Authors and Tags First


Author and tag objects need to exist before you can associate them with posts. Migrate these first.


### Migrate Authors


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;
const   ghostExport   =     require  (  './ghost-export.json'  )  ;


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY
}  )  ;


async     function     migrateAuthors  (  )     {
const   authors   =   ghostExport  .  db  [  0  ]  .  data  .  users  ;


for     (  const   author   of   authors  )     {
await   cosmic  .  objects  .  insertOne  (  {
type  :     'authors'  ,
title  :   author  .  name  ,
slug  :   author  .  slug  ,
metadata  :     {
email  :   author  .  email     ||     ''  ,
bio  :   author  .  bio     ||     ''
// Upload avatar separately via cosmic.media.insertOne()
}
}  )  ;
console  .  log  (  `  Migrated author:   ${  author  .  name  }  `  )  ;
}
}


migrateAuthors  (  )  ;
```


### Migrate Tags


```text
async     function     migrateTags  (  )     {
const   tags   =   ghostExport  .  db  [  0  ]  .  data  .  tags  ;


for     (  const   tag   of   tags  )     {
await   cosmic  .  objects  .  insertOne  (  {
type  :     'tags'  ,
title  :   tag  .  name  ,
slug  :   tag  .  slug  ,
metadata  :     {
description  :   tag  .  description     ||     ''
}
}  )  ;
console  .  log  (  `  Migrated tag:   ${  tag  .  name  }  `  )  ;
}
}


migrateTags  (  )  ;
```


---


## Step 4: Migrate Posts and Pages


With authors and tags in place, you can now migrate posts and associate them correctly.


```text
async     function     migratePosts  (  )     {
const   posts   =   ghostExport  .  db  [  0  ]  .  data  .  posts  ;
const   blogPosts   =   posts  .  filter  (  p     =>   p  .  type     ===     'post'  )  ;


for     (  const   post   of   blogPosts  )     {
const   authorSlug   =   post  .  authors  ?.  [  0  ]  ?.  slug   ||     null  ;
const   tagSlugs   =     (  post  .  tags     ||     [  ]  )  .  map  (  t     =>   t  .  slug  )  ;


await   cosmic  .  objects  .  insertOne  (  {
type  :     'blog-posts'  ,
title  :   post  .  title  ,
slug  :   post  .  slug  ,
status  :   post  .  status     ===     'published'     ?     'published'     :     'draft'  ,
metadata  :     {
content  :   post  .  html     ||     ''  ,
excerpt  :   post  .  custom_excerpt     ||   post  .  plaintext  ?.  slice  (  0  ,     200  )     ||     ''  ,
published_date  :   post  .  published_at  ?.  split  (  'T'  )  [  0  ]     ||     null  ,
author  :   authorSlug  ,       // Cosmic resolves slug to ObjectId automatically
tags  :   tagSlugs          // Array of slugs for "objects" metafield
}
}  )  ;


console  .  log  (  `  Migrated post:   ${  post  .  title  }  `  )  ;
await     new     Promise  (  r     =>     setTimeout  (  r  ,     100  )  )  ;     // Avoid rate limiting
}
}


migratePosts  (  )  ;
```


**Handling images in post content:** Ghost stores image URLs in post HTML that point to your Ghost instance. After migrating images to Cosmic's CDN, run a find-and-replace on the HTML content to update URLs to your new` imgix.cosmicjs.com` addresses.


### Migrate Pages


```text
async     function     migratePages  (  )     {
const   posts   =   ghostExport  .  db  [  0  ]  .  data  .  posts  ;
const   pages   =   posts  .  filter  (  p     =>   p  .  type     ===     'page'  )  ;


for     (  const   page   of   pages  )     {
await   cosmic  .  objects  .  insertOne  (  {
type  :     'pages'  ,
title  :   page  .  title  ,
slug  :   page  .  slug  ,
status  :   page  .  status     ===     'published'     ?     'published'     :     'draft'  ,
metadata  :     {
content  :   page  .  html     ||     ''  ,
meta_description  :   page  .  meta_description     ||     ''
}
}  )  ;
console  .  log  (  `  Migrated page:   ${  page  .  title  }  `  )  ;
}
}
```


---


## Step 5: Update Your Front-End to Use the Cosmic SDK


This is where things get exciting. Here's how to replace Ghost's Content API with the Cosmic SDK across common frameworks.


### Replacing Ghost's API with Cosmic in Next.js


**Before (Ghost Content API):**


```text
// lib/ghost.js
import     GhostContentAPI     from     '@tryghost/content-api'  ;


const   api   =     new     GhostContentAPI  (  {
url  :   process  .  env  .  GHOST_URL  ,
key  :   process  .  env  .  GHOST_CONTENT_API_KEY  ,
version  :     'v5.0'
}  )  ;


export     async     function     getPosts  (  )     {
return   api  .  posts  .  browse  (  {     include  :     'authors,tags'  ,     limit  :     'all'     }  )  ;
}
```


**After (Cosmic JavaScript SDK):**


```text
// lib/cosmic.js
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY
}  )  ;


export     async     function     getPosts  (  )     {
const     {   objects   }     =     await   cosmic  .  objects  .  find  (  {
type  :     'blog-posts'
}  )
.  props  (  'title,slug,metadata,status'  )
.  depth  (  1  )     // Resolves related author and tag objects inline
.  limit  (  100  )  ;


return   objects  ;
}


export     async     function     getPostBySlug  (  slug  )     {
const     {   object   }     =     await   cosmic  .  objects  .  findOne  (  {
type  :     'blog-posts'  ,
slug
}  )
.  props  (  'title,slug,metadata,status'  )
.  depth  (  1  )  ;


return   object  ;
}
```


The` .depth(1)` call tells Cosmic to resolve related objects (authors, tags) inline, equivalent to Ghost's` include: 'authors,tags'` option.


### Replacing Ghost's API in Nuxt 3


```text
// composables/useCosmic.js
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;


const   config   =     useRuntimeConfig  (  )  ;


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   config  .  public  .  cosmicBucketSlug  ,
readKey  :   config  .  public  .  cosmicReadKey
}  )  ;


export     const     usePosts     =     async     (  )     =>     {
const     {   objects   }     =     await   cosmic  .  objects  .  find  (  {
type  :     'blog-posts'
}  )
.  props  (  'title,slug,metadata,status'  )
.  depth  (  1  )
.  limit  (  100  )  ;


return   objects   ||     [  ]  ;
}  ;
```


In your` nuxt.config.ts` , replace Ghost environment variables:


```text
export     default     defineNuxtConfig  (  {
runtimeConfig  :     {
public  :     {
cosmicBucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  ,
cosmicReadKey  :   process  .  env  .  COSMIC_READ_KEY
}
}
}  )  ;
```


### Replacing Ghost's API in Astro


```text
// src/lib/cosmic.js
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;


const   cosmic   =     createBucketClient  (  {
bucketSlug  :     import  .  meta  .  env  .  COSMIC_BUCKET_SLUG  ,
readKey  :     import  .  meta  .  env  .  COSMIC_READ_KEY
}  )  ;


export     async     function     getPosts  (  )     {
const     {   objects   }     =     await   cosmic  .  objects  .  find  (  {
type  :     'blog-posts'
}  )
.  props  (  'title,slug,metadata'  )
.  depth  (  1  )
.  limit  (  100  )  ;


return   objects  ;
}
```


In an Astro page component:


```text
---
// src/pages/blog/[slug].astro
import { getPosts } from '../../lib/cosmic.js';


export async function getStaticPaths() {
const posts = await getPosts();
return posts.map(post => ({
params: { slug: post.slug },
props: { post }
}));
}


const { post } = Astro.props;
---


<article>
<h1>{post.title}</h1>
<Fragment set:html={post.metadata.content} />
</article>
```


---


## Step 6: Set Up Webhooks for Content Updates


Ghost uses webhooks to notify external services when content is published. Cosmic has a native webhooks system that works the same way.


In your Cosmic dashboard:


1. Go to **Bucket Settings > Webhooks**
2. Click **Add Webhook**
3. Set the **Event** (e.g.,` object.published` ,` object.edited` ,` object.deleted` )
4. Set the **Endpoint URL** (your revalidation endpoint, deploy hook, or custom handler)


### Example: On-demand ISR revalidation in Next.js


```text
// app/api/revalidate/route.js
import     {   revalidatePath   }     from     'next/cache'  ;
import     {     NextResponse     }     from     'next/server'  ;


export     async     function     POST  (  request  )     {
const   secret   =   request  .  headers  .  get  (  'x-webhook-secret'  )  ;


if     (  secret   !==   process  .  env  .  WEBHOOK_SECRET  )     {
return     NextResponse  .  json  (  {     error  :     'Unauthorized'     }  ,     {     status  :     401     }  )  ;
}


const   body   =     await   request  .  json  (  )  ;
const   slug   =   body  .  data  ?.  slug  ;


if     (  slug  )     {
revalidatePath  (  `  /blog/  ${  slug  }  `  )  ;
revalidatePath  (  '/blog'  )  ;
}


return     NextResponse  .  json  (  {     revalidated  :     true     }  )  ;
}
```


In your Cosmic webhook settings, add a custom header` x-webhook-secret` with your secret value. This gives you Ghost-equivalent publish-and-update triggering with full control over cache invalidation.


**Note:** Webhooks are available as an add-on ($99/month) or as part of the Bundle add-on ($199/month, which also includes Localization, Revision History, and Automatic Backups).


---


## Managing Multiple Brands or Properties


This is one of the most compelling reasons teams migrate from Ghost. Running multiple Ghost sites means multiple Node.js processes, multiple databases, and multiple deployments to manage.


In Cosmic, each brand or property gets its own **Bucket** . All Buckets are managed under one account and accessible via the same dashboard.


### Setting up multi-brand in Cosmic


Each Bucket has its own:


- API credentials (bucket slug + read/write keys)
- Object Types and content schema
- Media library
- Team members (you can share members across Buckets or keep them separate)
- Webhooks


For a team running three publications, you'd create three Buckets:` brand-a` ,` brand-b` ,` brand-c` . Each has its own content but you manage everything from one Cosmic account.


### Fetching content across multiple brands


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;


const   brands   =     [
{     slug  :     'brand-a-bucket'  ,     readKey  :   process  .  env  .  BRAND_A_READ_KEY     }  ,
{     slug  :     'brand-b-bucket'  ,     readKey  :   process  .  env  .  BRAND_B_READ_KEY     }  ,
{     slug  :     'brand-c-bucket'  ,     readKey  :   process  .  env  .  BRAND_C_READ_KEY     }  ,
]  ;


async     function     getAllPostsAcrossBrands  (  )     {
const   results   =     await     Promise  .  all  (
brands  .  map  (  async     brand     =>     {
const   cosmic   =     createBucketClient  (  {
bucketSlug  :   brand  .  slug  ,
readKey  :   brand  .  readKey
}  )  ;


const     {   objects   }     =     await   cosmic  .  objects  .  find  (  {     type  :     'blog-posts'     }  )
.  props  (  'title,slug,metadata'  )
.  depth  (  1  )
.  limit  (  50  )  ;


return     (  objects   ||     [  ]  )  .  map  (  post     =>     (  {     ...  post  ,     brand  :   brand  .  slug     }  )  )  ;
}  )
)  ;
return   results  .  flat  (  )  ;
}
```


### Plan considerations for multi-brand


We recommend upgrading to a Cosmic Workspace for managing multiple projects / brands which includes granular roles and permissions for your team.


For individual projects, the number of Buckets you can use depends on your plan:


- **Builder** ($49/month): 2 Buckets
- **Team** ($299/month): 3 Buckets
- **Business** ($499/month): 5 Buckets
- **Enterprise** : Custom


Additional Buckets beyond your plan limit are available for $29/bucket/month. For larger multi-brand operations, Enterprise pricing is worth exploring.


---


## Migration Checklist


Before you cut over DNS and decommission Ghost, work through this checklist:


- Ghost JSON export downloaded and backed up
- Cosmic Bucket created and Object Types defined
- Authors migrated to Cosmic
- Tags migrated to Cosmic
- All posts migrated with correct author/tag associations
- All pages migrated
- Images re-uploaded to Cosmic media library (or external CDN)
- Image URLs updated in post HTML content
- Front-end updated to use Cosmic JavaScript SDK (` @cosmicjs/sdk` )
- Environment variables updated (` COSMIC_BUCKET_SLUG` ,` COSMIC_READ_KEY` ,` COSMIC_WRITE_KEY` )
- Webhooks configured for cache invalidation
- Redirects in place for any changed slugs
- 301 redirects for RSS feed URL if it changed
- SEO: canonical URLs and meta tags verified
- Staging deployment tested end-to-end
- DNS cutover scheduled during low-traffic window


---


## Ready to Make the Move?


Migrating from Ghost to Cosmic is a straightforward process for a developer-comfortable team. You gain flexible content modeling, a clean SDK, AI-native workflows, and scalable multi-brand management without the infrastructure overhead of self-hosting.


If you want to talk through your specific setup before migrating,[book a call with Tony](https://calendly.com/tonyspiro/cosmic-intro) . Whether you're running three Ghost blogs on a single VPS or managing five brands across different Ghost Pro accounts, we can walk through the migration plan together.


**Start free:**[Sign up at cosmicjs.com](https://app.cosmicjs.com/signup) and have your first Bucket running in minutes.


**Explore the docs:**[cosmicjs.com/docs](https://www.cosmicjs.com/docs)
