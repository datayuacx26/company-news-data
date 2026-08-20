---
schema_version: "1.0.0"
document_id: "a707ae1d7a3668dcf62d92f4152ba5360baade6e7f02342f8b3642c9e8c84230"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/migrate-from-prismic-to-cosmic"
published_at: "2026-04-10T00:00:00+00:00"
first_seen_at: "2026-08-10T06:05:12.278252+00:00"
fetched_at: "2026-08-10T06:05:15.916664+00:00"
content_hash: "sha256:6afec11d565f455d75bd4e06270340dcd67ceb7f20602033091e0ac75bb7377e"
---

# How to Migrate from Prismic to Cosmic

Prismic has been a reliable headless CMS for thousands of development teams. But the combination of recent pricing changes, Slice Machine's tight coupling between content models and frontend code, and the absence of native AI agents has many teams looking for a more flexible, future-proof platform.


This guide covers everything you need to move from Prismic to Cosmic: understanding the conceptual model mapping, migrating your content structure, porting your content and media, and updating your frontend API calls. Real code examples are included throughout.


---


## Why Teams Are Leaving Prismic


Before the technical steps, it is worth understanding what is driving the migration in the first place.


### 1. Pricing Changes


Prismic restructured their pricing model, and many teams saw their bills increase significantly. For teams on annual plans or with multiple repositories, the new pricing created real budget pressure. Prismic's per-repository model also means costs compound for teams managing multiple sites or clients.


### 2. Slice Machine Lock-In


Slice Machine is Prismic's developer tool for defining reusable page sections. It works by storing slice definitions as JSON files inside your repository, alongside your React or Vue components. This has a real downside: your content model is coupled to your code.


To add a new field to a slice:


1. Update the slice definition in Slice Machine (locally)
2. Push the updated type to your Prismic repository via the CLI
3. Deploy your frontend to register the new component


Editors cannot change the content structure without developer involvement and a deployment cycle. For teams that need content flexibility, this is a bottleneck.


### 3. Limited AI Capabilities


Prismic lists AI features as beta and available upon request. In 2026, your CMS should have AI built in as a first-class capability, not as a future roadmap item.


---


## Understanding the Content Model Mapping


Before writing a single line of migration code, you need to understand how Prismic's concepts map to Cosmic's.


### Prismic Concepts Mapped to Cosmic


Prismic Cosmic Notes


**Repository** **Bucket** Your top-level project container


**Custom Type** **Object Type** Defines your content schema


**Document** **Object** A single piece of content


**Slice** **Metafield (repeater or parent)** Reusable structured sections


**Slice Zone** **Repeater metafield** Variable-length list of structured items


**Field (Rich Text)** **Metafield (html-textarea or markdown)** Rich content fields


**Field (Image)** **Metafield (file, media_validation_type: image)** Image fields with imgix CDN


**Field (Link)** **Metafield (text or object)** URL strings or related Objects


**Field (Select)** **Metafield (select or radio-buttons)** Single-choice options


**Field (Boolean)** **Metafield (switch)** Boolean toggle


**Field (Date)** **Metafield (date)** Date fields


**Field (Number)** **Metafield (number)** Numeric fields


**Field (Key Text)** **Metafield (text)** Single-line text


**Field (Color)** **Metafield (color)** Color picker


**Integration Field** **Metafield (object or objects)** Relationships to other content


**Tags** **Tags (object relationship)** Tagging system


**UID** **Slug** URL-friendly unique identifier


### The Key Difference: Where the Schema Lives


In Prismic, your Custom Type definitions are JSON files that live in` .slicemachine/` and` customtypes/` directories in your repository. In Cosmic, your Object Type schema lives in the CMS dashboard and is reflected immediately in the API.


This means you create your Cosmic Object Types through the dashboard (or the REST API), not by editing JSON files in your codebase. Schema changes take effect instantly with no code deployment.


---


## Step 1: Audit Your Prismic Content Types


Start by exporting and documenting your Prismic content model.


### Export Your Custom Types


Prismic stores your Custom Type definitions as JSON. You can find them in your repository under **Custom types** in the dashboard, or in your codebase under the` customtypes/` directory if you use Slice Machine.


Here is an example Prismic Custom Type for a blog post:


```text
{
"id"  :     "blog_post"  ,
"label"  :     "Blog Post"  ,
"json"  :     {
"Main"  :     {
"uid"  :     {
"type"  :     "UID"  ,
"config"  :     {     "label"  :     "UID"     }
}  ,
"title"  :     {
"type"  :     "StructuredText"  ,
"config"  :     {     "label"  :     "Title"  ,     "single"  :     "heading1"     }
}  ,
"excerpt"  :     {
"type"  :     "StructuredText"  ,
"config"  :     {     "label"  :     "Excerpt"  ,     "single"  :     "paragraph"     }
}  ,
"cover_image"  :     {
"type"  :     "Image"  ,
"config"  :     {     "label"  :     "Cover Image"     }
}  ,
"author"  :     {
"type"  :     "Link"  ,
"config"  :     {     "label"  :     "Author"  ,     "select"  :     "document"  ,     "customtypes"  :     [  "author"  ]     }
}  ,
"body"  :     {
"type"  :     "Slices"  ,
"fieldset"  :     "Slice zone"  ,
"config"  :     {     "label"  :     "Body"     }  ,
"choices"  :     {
"text"  :     {     "type"  :     "Slice"  ,     "fieldset"  :     "Text Section"     }  ,
"image_with_caption"  :     {     "type"  :     "Slice"  ,     "fieldset"  :     "Image with Caption"     }
}
}
}
}
}
```


Make a list of all your Custom Types, their fields, and whether they are repeatable (many documents) or single (one document per type).


---


## Step 2: Create Your Cosmic Object Types


Now map each Prismic Custom Type to a Cosmic Object Type. You can do this through the Cosmic dashboard or via the REST API.


### Mapping the Blog Post Example


Here is how the Prismic blog post Custom Type maps to a Cosmic Object Type:


**Prismic field → Cosmic metafield:**


- ` uid` → becomes the Object's` slug` (auto-generated or set explicitly on create)
- ` title` (StructuredText/heading1) →` title` is the Object's top-level title field
- ` excerpt` (StructuredText/paragraph) → metafield type` textarea`
- ` cover_image` (Image) → metafield type` file` with` media_validation_type: "image"`
- ` author` (Link/document) → metafield type` object` with` object_type: "authors"`
- ` body` (Slice Zone) → metafield type` repeater` (or individual` parent` metafields per slice)


### Creating the Object Type via the REST API


```text
curl   -X POST https://api.cosmicjs.com/v3/buckets/your-bucket-slug/object-types   \
-H   "Content-Type: application/json"     \
-H   "Authorization: Bearer your-write-key"     \
-d   '{
"title": "Blog Posts",
"slug": "blog-posts",
"singular": "Blog Post",
"metafields": [
{
"key": "excerpt",
"title": "Excerpt",
"type": "textarea"
},
{
"key": "cover_image",
"title": "Cover Image",
"type": "file",
"media_validation_type": "image"
},
{
"key": "author",
"title": "Author",
"type": "object",
"object_type": "authors"
},
{
"key": "body_content",
"title": "Body Content",
"type": "html-textarea"
},
{
"key": "published_date",
"title": "Published Date",
"type": "date"
}
]
}'
```


### Handling Slices: Three Patterns


Prismic Slices map to Cosmic in different ways depending on their complexity:


**Pattern 1: Simple slice with a few fields →` parent` metafield**


A Prismic "Quote" slice with` quote_text` and` attribution` fields becomes a` parent` metafield in Cosmic:


```text
{
"key"  :     "quote_section"  ,
"title"  :     "Quote Section"  ,
"type"  :     "parent"  ,
"children"  :     [
{     "key"  :     "quote_text"  ,     "title"  :     "Quote Text"  ,     "type"  :     "textarea"     }  ,
{     "key"  :     "attribution"  ,     "title"  :     "Attribution"  ,     "type"  :     "text"     }
]
}
```


**Pattern 2: Repeatable slice (e.g., FAQ list) →` repeater` metafield**


A Prismic slice with repeatable items becomes a` repeater` :


```text
{
"key"  :     "faqs"  ,
"title"  :     "FAQs"  ,
"type"  :     "repeater"  ,
"children"  :     [
{     "key"  :     "question"  ,     "title"  :     "Question"  ,     "type"  :     "text"     }  ,
{     "key"  :     "answer"  ,     "title"  :     "Answer"  ,     "type"  :     "html-textarea"     }
]
}
```


**Pattern 3: Slice Zone with multiple slice types → Rich text body**


For content-heavy pages where the body is primarily long-form text, convert the Slice Zone into a single` html-textarea` or` markdown` metafield:


```text
{
"key"  :     "body"  ,
"title"  :     "Body"  ,
"type"  :     "markdown"
}
```


This approach trades some structural granularity for simplicity. For most blog posts and article-style content, it is the right choice.


---


## Step 3: Export Your Prismic Content


Prismic provides a Migration API for exporting documents. Here is how to use it:


```text
// export-prismic.ts
import     *     as   prismic     from     '@prismicio/client'


const   client   =   prismic  .  createClient  (  'your-repository-name'  ,     {
accessToken  :   process  .  env  .  PRISMIC_ACCESS_TOKEN  ,
}  )


async     function     exportAllDocuments  (  )     {
const   allDocuments   =     [  ]
let   page   =     1


while     (  true  )     {
const   response   =     await   client  .  get  (  {
pageSize  :     100  ,
page  ,
}  )


allDocuments  .  push  (  ...  response  .  results  )


if     (  page   >=   response  .  total_pages  )     break
page  ++
}


// Save to a local JSON file for processing
const   fs   =     await     import  (  'fs'  )
fs  .  writeFileSync  (  'prismic-export.json'  ,     JSON  .  stringify  (  allDocuments  ,     null  ,     2  )  )
console  .  log  (  `  Exported   ${  allDocuments  .  length  }   documents.  `  )
}


exportAllDocuments  (  )
```


Run this script to get a complete export of your Prismic content:


```text
npx ts-node export-prismic.ts
```


---


## Step 4: Migrate Media Files


Before importing content objects, migrate your media files so you have Cosmic` imgix_url` values to reference in your content.


```text
// migrate-media.ts
import     *     as   prismic     from     '@prismicio/client'
import     {   createBucketClient   }     from     '@cosmicjs/sdk'
import     fs     from     'fs'


const   prismicClient   =   prismic  .  createClient  (  'your-repository-name'  ,     {
accessToken  :   process  .  env  .  PRISMIC_ACCESS_TOKEN  ,
}  )


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG     as     string  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY     as     string  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY     as     string  ,
}  )


const   imageMap  :     Record  <  string  ,     string  >     =     {  }


async     function     migrateMedia  (  )     {
// Load your exported documents
const   documents   =     JSON  .  parse  (  fs  .  readFileSync  (  'prismic-export.json'  ,     'utf8'  )  )


for     (  const   doc   of   documents  )     {
// Find all image fields recursively
const   images   =     extractImages  (  doc  .  data  )


for     (  const   image   of   images  )     {
if     (  !  image  .  url     ||   imageMap  [  image  .  url  ]  )     continue


try     {
// Upload the image to Cosmic from its Prismic URL
const   response   =     await   cosmic  .  media  .  insertOne  (  {
url  :   image  .  url  ,
name  :   image  .  alt     ||     `  prismic-media-  ${  Date  .  now  (  )  }  `  ,
}  )


// Map old Prismic URL to new Cosmic imgix_url
imageMap  [  image  .  url  ]     =   response  .  media  .  imgix_url
console  .  log  (  `  Migrated:   ${  image  .  url  }  `  )
}     catch     (  err  )     {
console  .  error  (  `  Failed to migrate   ${  image  .  url  }  :  `  ,   err  )
}
}
}


// Save the image map for use during content migration
fs  .  writeFileSync  (  'image-map.json'  ,     JSON  .  stringify  (  imageMap  ,     null  ,     2  )  )
console  .  log  (  `  Migrated   ${  Object  .  keys  (  imageMap  )  .  length  }   images.  `  )
}


function     extractImages  (  data  :     any  )  :     any  [  ]     {
const   images  :     any  [  ]     =     [  ]
if     (  !  data  )     return   images


if     (  data  .  url     &&   data  .  dimensions  )     {
// This looks like a Prismic image field
images  .  push  (  data  )
}


if     (  typeof   data   ===     'object'  )     {
for     (  const   value   of     Object  .  values  (  data  )  )     {
images  .  push  (  ...  extractImages  (  value  )  )
}
}


if     (  Array  .  isArray  (  data  )  )     {
for     (  const   item   of   data  )     {
images  .  push  (  ...  extractImages  (  item  )  )
}
}


return   images
}


migrateMedia  (  )
```


---


## Step 5: Import Content into Cosmic


With your Object Types created and media migrated, you can now import your content documents.


```text
// import-to-cosmic.ts
import     {   createBucketClient   }     from     '@cosmicjs/sdk'
import     *     as   prismic     from     '@prismicio/client'
import     fs     from     'fs'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG     as     string  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY     as     string  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY     as     string  ,
}  )


// Load the image map from the media migration step
const   imageMap  :     Record  <  string  ,     string  >     =     JSON  .  parse  (
fs  .  readFileSync  (  'image-map.json'  ,     'utf8'  )
)


// Load exported Prismic documents


function     prismicRichTextToHtml  (  richText  :   prismic  .  RichTextField  )  :     string     {
// Use Prismic's serializer to convert rich text to HTML
return   prismic  .  asHTML  (  richText  )
}


async     function     importBlogPosts  (  )     {
const   blogPosts   =   documents  .  filter  (  (  doc  :     any  )     =>   doc  .  type     ===     'blog_post'  )
console  .  log  (  `  Importing   ${  blogPosts  .  length  }   blog posts...  `  )


for     (  const   doc   of   blogPosts  )     {
const   coverImageUrl   =   doc  .  data  .  cover_image  ?.  url
const   cosmicImageUrl   =   coverImageUrl   ?   imageMap  [  coverImageUrl  ]     :     undefined


try     {
await   cosmic  .  objects  .  insertOne  (  {
title  :   prismic  .  asText  (  doc  .  data  .  title  )  ,
slug  :   doc  .  uid  ,
type  :     'blog-posts'  ,
status  :     'published'  ,
metadata  :     {
excerpt  :   prismic  .  asText  (  doc  .  data  .  excerpt  )  ,
body  :     prismicRichTextToHtml  (  doc  .  data  .  body  )  ,
cover_image  :   cosmicImageUrl  ,
published_date  :   doc  .  first_publication_date
?   doc  .  first_publication_date  .  split  (  'T'  )  [  0  ]
:     null  ,
}  ,
}  )


console  .  log  (  `  Imported:   ${  prismic  .  asText  (  doc  .  data  .  title  )  }  `  )
}     catch     (  err  )     {
console  .  error  (  `  Failed to import   ${  doc  .  uid  }  :  `  ,   err  )
}
}


console  .  log  (  'Blog post import complete.'  )
}


importBlogPosts  (  )
```


### Handling Slice Zones


If your content uses a Slice Zone (variable list of slices), convert it to structured HTML or markdown before importing:


```text
function     convertSliceZoneToHtml  (  slices  :     any  [  ]  )  :     string     {
return   slices
.  map  (  (  slice  )     =>     {
switch     (  slice  .  slice_type  )     {
case     'text'  :
return   prismic  .  asHTML  (  slice  .  primary  .  text  )
case     'image_with_caption'  :
const   imgUrl   =   imageMap  [  slice  .  primary  .  image  ?.  url  ]     ||   slice  .  primary  .  image  ?.  url
const   caption   =   prismic  .  asText  (  slice  .  primary  .  caption  )
return     `  <figure><img src="  ${  imgUrl  }  " alt="  ${  caption  }  " /><figcaption>  ${  caption  }  </figcaption></figure>  `
case     'quote'  :
return     `  <blockquote>  ${  prismic  .  asText  (  slice  .  primary  .  quote_text  )  }  <cite>  ${  slice  .  primary  .  attribution  }  </cite></blockquote>  `
default  :
return     ''
}
}  )
.  join  (  '\n'  )
}
```


---


## Step 6: Update Your Frontend


With your content in Cosmic, update your frontend to fetch from the Cosmic API instead of Prismic.


### Before: Fetching from Prismic


```text
// lib/prismic.ts (before)
import     *     as   prismic     from     '@prismicio/client'


const   client   =   prismic  .  createClient  (  'your-repository-name'  )


export     async     function     getBlogPosts  (  )     {
const   documents   =     await   client  .  getAllByType  (  'blog_post'  ,     {
orderings  :     {
field  :     'document.first_publication_date'  ,
direction  :     'desc'  ,
}  ,
}  )
return   documents
}


export     async     function     getBlogPost  (  uid  :     string  )     {
return   client  .  getByUID  (  'blog_post'  ,   uid  )
}
```


### After: Fetching from Cosmic


```text
// lib/cosmic.ts (after)
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG     as     string  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY     as     string  ,
}  )


export     async     function     getBlogPosts  (  )     {
const     {   objects   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'     }  )
.  props  (  'id,title,slug,metadata,created_at'  )
.  sort  (  '-created_at'  )
return   objects
}


export     async     function     getBlogPost  (  slug  :     string  )     {
const     {   object   }     =     await   cosmic  .  objects  .  findOne  (  {
type  :     'blog-posts'  ,
slug  ,
}  )
return   object
}
```


### Updating Component Rendering


Prismic returns rich text as structured arrays that require a serializer. Cosmic returns it as HTML or markdown that you can render directly:


```text
// Before (Prismic rich text)
import     {     PrismicRichText     }     from     '@prismicio/react'


// In your component:
<  PrismicRichText   field  =  {  post  .  data  .  body  }     /  >


// After (Cosmic HTML)
// In your component:
<  div dangerouslySetInnerHTML  =  {  {   __html  :   post  .  metadata  .  body     }  }     /  >


// Or if you stored markdown:
import     ReactMarkdown     from     'react-markdown'
<  ReactMarkdown  >  {  post  .  metadata  .  body  }  <  /  ReactMarkdown  >
```


### Updating Image References


Prismic image URLs differ from Cosmic's imgix-powered URLs. Update your image rendering:


```text
// Before (Prismic)
<  img src  =  {  post  .  data  .  cover_image  .  url  }   alt  =  {  post  .  data  .  cover_image  .  alt  }     /  >


// After (Cosmic with imgix optimization)
<  img
src  =  {  `  ${  post  .  metadata  .  cover_image  .  imgix_url  }  ?w=1200&auto=format,compress  `  }
alt  =  {  post  .  title  }
/  >
```


Every Cosmic image URL is an imgix URL. Append` ?w=800&auto=format,compress` (or any imgix parameters) to resize, convert to WebP, and optimize on the fly. No separate image processing service needed.


---


## Step 7: Set Up Webhooks for Cache Revalidation


If your frontend uses Next.js ISR, Astro's on-demand revalidation, or any other cache invalidation pattern, set up Cosmic webhooks to trigger a rebuild or revalidation when content is published.


In your Cosmic dashboard: **Bucket Settings > Webhooks > Add Webhook**


Set the webhook URL to your deployment provider's trigger URL:


- **Vercel:**` https://api.vercel.com/v1/integrations/deploy/your-token`
- **Netlify:**` https://api.netlify.com/build_hooks/your-token`
- **Custom Next.js ISR:**` https://your-site.com/api/revalidate?secret=your-token`


Select the trigger events:` object.published` ,` object.updated` ,` object.deleted` .


From this point, every time an editor publishes content in Cosmic, your frontend automatically revalidates.


---


## What You Gain After Migrating


**Schema changes without deployments.** Add a new field in the Cosmic dashboard. It is in the API immediately. No CLI, no commit, no deploy.


**Framework freedom.** The same Cosmic content powers your Next.js site, your Vue app, your mobile React Native app, and any other client. No slice library to rebuild.


**Built-in AI Agents.** Create content automatically with Content Agents that research, write, and publish on a schedule. Connect your GitHub repo with Code Agents. Browse the web with Computer Use Agents.


**imgix CDN for every image.** Every uploaded image is instantly served from a global CDN with on-the-fly transforms. No image pipeline to build or maintain.


**Predictable multi-project pricing.** One Cosmic plan covers multiple Buckets. No per-repository billing.


---


## Migration Checklist


- Audit all Prismic Custom Types and document their fields
- Map each Custom Type to a Cosmic Object Type
- Map Slices to Cosmic metafields (parent, repeater, or rich text)
- Create Object Types in Cosmic dashboard or via the REST API
- Export Prismic documents to JSON using the Prismic client
- Migrate media files to Cosmic (captures imgix URLs)
- Import content objects into Cosmic via the REST API or SDK
- Update frontend data-fetching functions from Prismic SDK to Cosmic SDK
- Update image rendering to use Cosmic imgix URLs with optimization params
- Update dynamic route generation (` getStaticPaths` ,` generateStaticParams` ) to use Cosmic
- Set up Cosmic webhooks for cache revalidation
- Smoke test all content types in staging
- Switch DNS / promote to production


---


## Get Help With Your Migration


Ready to make the switch? Start on the free plan with no credit card required.


**[Start free on Cosmic →](https://app.cosmicjs.com/signup?utm_source=blog&utm_medium=cta&utm_campaign=prismic-migration)**


For teams that want hands-on migration support, book a call with Tony:


**[Book a demo with Tony →](https://calendly.com/tonyspiro/cosmic-intro)**


Also migrating from a different platform? See our guides for[Strapi](https://www.cosmicjs.com/strapi-alternative) and[WordPress](https://www.cosmicjs.com/wordpress-alternative) . Or explore the[Prismic alternative landing page](https://www.cosmicjs.com/prismic-alternative) for a full feature comparison.
