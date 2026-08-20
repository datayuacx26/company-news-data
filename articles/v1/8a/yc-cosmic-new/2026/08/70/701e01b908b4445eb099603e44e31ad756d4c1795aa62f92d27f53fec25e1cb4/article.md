---
schema_version: "1.0.0"
document_id: "701e01b908b4445eb099603e44e31ad756d4c1795aa62f92d27f53fec25e1cb4"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/migrate-payload-cms-to-cosmic"
published_at: "2026-08-15T00:00:00+00:00"
first_seen_at: "2026-08-15T18:28:46.217573+00:00"
fetched_at: "2026-08-15T18:28:50.030483+00:00"
content_hash: "sha256:1df53fb573a0857f397442231b99c3d7c2d130ce36d0745c26cedf25a73e63ce"
---

# How to Migrate from Payload CMS to Cosmic

> **Last verified: August 14, 2026.** Every claim about Payload on this page was checked against Payload's own live pages on that date:[Payload Cloud pricing and status](https://payloadcms.com/cloud-pricing) ,[What is Payload](https://payloadcms.com/docs/getting-started/what-is-payload) , and the[Local API docs](https://payloadcms.com/docs/local-api/overview) . Cosmic pricing was checked against[cosmicjs.com/pricing](https://cosmicjs.com/pricing) . Note that` payloadcms.com/pricing` returned a 404 on this date; Cloud plan details now live on the` /cloud-pricing` page.


## What actually changed at Payload


Payload has joined Figma. The announcement is[on the Figma blog](https://www.figma.com/blog/payload-joins-figma/) , and it is linked from the top of every page on payloadcms.com.


Two things follow from that, both stated by Payload directly on its Cloud page:


1. **New Payload Cloud deployments are paused.** In Payload's words: "Although deployment of new projects is currently paused, existing Cloud projects will continue running as normal."
2. **Existing Cloud projects will eventually move.** From the same page's FAQ, answering "Will I need to migrate my project?": "Yes, eventually. There is no rush, but we are planning to build something better that you will be able to migrate to once it's available."


Here is what has *not* changed. Payload the framework is still open source, still actively developed, and still self-hostable. Payload's own docs state that Payload remains a self-hosted solution, and that anywhere you can run a Next.js app, you can run Payload. If you self-host Payload today, nothing above forces your hand.


The teams with a real decision to make are the ones who picked Payload Cloud because they wanted someone else to run the database, the file storage, and the deploys.


## Should you migrate at all?


Three honest options. Pick based on your team, not on vendor news.


**Stay on Payload and self-host it.** The right call if your content model leans on Payload's code-first strengths: custom access control, hooks, field-level permissions, or an admin panel you have meaningfully customized in React. You will need to own a database, object storage, and a deploy target. Payload is a Next.js application, so this is familiar work for a Next.js team.


**Wait for whatever Figma ships.** Payload says there is no rush and that a migration path to something new is planned. If your project is stable and you have infrastructure people, waiting costs you little.


**Move to a managed API-first CMS.** The right call if the reason you chose Payload Cloud was that you did not want to run infrastructure, and you would rather not go back to running it. That is the path this guide covers.


## What you give up moving to Cosmic


Stating this plainly, because a migration guide that pretends there are no tradeoffs is not worth reading.


- **The Local API goes away.** Payload's biggest technical advantage is that it runs in the same Node process as your app, so you can query the database directly from a React Server Component with no network hop. Cosmic is an HTTP API. For most content sites the difference disappears behind caching and static generation, but it is a genuine architectural change and you should know it going in.
- **No GraphQL.** Payload exposes REST and GraphQL. Cosmic offers a REST API and a TypeScript SDK, and does not offer GraphQL. If your frontend is built on Payload's GraphQL endpoint, that query layer gets rewritten.
- **Code-first config becomes dashboard-first modeling.** In Payload your collections live in version-controlled TypeScript. In Cosmic you define Object Types in the dashboard or over the API. Some teams consider that a downgrade in reviewability, and others consider it the point.
- **Custom admin components.** React components you injected into the Payload admin panel do not carry over.
- **Auth and access control.** Payload ships user auth and granular access control as first-class features. If you used Payload as your application's auth layer and not only as a CMS, that responsibility moves elsewhere.


If several of those matter a lot to you, self-hosting Payload is probably the better answer, and you should stop reading here. No hard feelings.


## Mapping Payload concepts to Cosmic


Payload Cosmic


Collection Object Type


Document Object


Global Single Object (a type with one Object)


Field Metafield


` slug` Object slug


Upload collection Media library


Relationship field Object metafield (` object` /` objects` )


Array field Repeater metafield


Blocks field Repeater, or rich text with Content Blocks


Group field Parent metafield group


Draft / published Object status (` draft` /` published` )


Localization Locale variants on an Object Type


The shapes line up closely enough that most migrations are a data-transform problem rather than a redesign.


## Step 1: inventory what you actually have


Before writing any script, list every collection and global, the document count in each, and which fields are genuinely used. Migrations balloon because teams port fields nobody has filled in for two years.


Payload's Local API gives you counts quickly:


```text
import     {   getPayload   }     from     'payload'
import     config     from     '@payload-config'


const   payload   =     await     getPayload  (  {   config   }  )


for     (  const   collection   of   payload  .  config  .  collections  )     {
const     {   totalDocs   }     =     await   payload  .  count  (  {   collection  :   collection  .  slug     }  )
console  .  log  (  `  ${  collection  .  slug  }  :   ${  totalDocs  }  `  )
}
```


Write the output down. It is your migration checklist and your verification target at cutover.


## Step 2: export from Payload


Run this inside your Payload project so` @payload-config` resolves. Two details matter:` pagination: false` returns every document instead of the first page, and` depth: 0` keeps relationships as raw IDs instead of expanding them into nested objects. You want raw IDs, because you are going to remap them yourself.


```text
import     {   getPayload   }     from     'payload'
import     config     from     '@payload-config'
import     fs     from     'node:fs/promises'
import     path     from     'node:path'


const   payload   =     await     getPayload  (  {   config   }  )
const   outDir   =   path  .  resolve  (  './payload-export'  )
await   fs  .  mkdir  (  outDir  ,     {   recursive  :     true     }  )


// Collections
for     (  const   collection   of   payload  .  config  .  collections  )     {
const     {   docs   }     =     await   payload  .  find  (  {
collection  :   collection  .  slug  ,
pagination  :     false  ,
depth  :     0  ,
overrideAccess  :     true  ,
}  )


await   fs  .  writeFile  (
path  .  join  (  outDir  ,     `  ${  collection  .  slug  }  .json  `  )  ,
JSON  .  stringify  (  docs  ,     null  ,     2  )  ,
)


console  .  log  (  `  exported   ${  docs  .  length  }   from   ${  collection  .  slug  }  `  )
}


// Globals
for     (  const   global   of   payload  .  config  .  globals  )     {
const   doc   =     await   payload  .  findGlobal  (  {   slug  :   global  .  slug  ,   depth  :     0     }  )


await   fs  .  writeFile  (
path  .  join  (  outDir  ,     `  global-  ${  global  .  slug  }  .json  `  )  ,
JSON  .  stringify  (  doc  ,     null  ,     2  )  ,
)
}
```


Commit that folder somewhere safe. Everything after this point is reversible as long as the export exists.


## Step 3: model your content in Cosmic


Create one Object Type per Payload collection. You can do it in the dashboard, or script it so the whole migration is repeatable:


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  !  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY  !  ,
}  )


await   cosmic  .  objectTypes  .  insertOne  (  {
title  :     'Posts'  ,
singular  :     'Post'  ,
slug  :     'posts'  ,
metafields  :     [
{   key  :     'excerpt'  ,   title  :     'Excerpt'  ,   type  :     'textarea'     }  ,
{   key  :     'content'  ,   title  :     'Content'  ,   type  :     'markdown'     }  ,
{   key  :     'hero'  ,   title  :     'Hero Image'  ,   type  :     'file'     }  ,
{
key  :     'author'  ,
title  :     'Author'  ,
type  :     'object'  ,
object_type  :     'authors'  ,
}  ,
]  ,
}  )
```


Migrate types with no relationships first (authors, categories, tags), then the types that point at them. That ordering saves you a second reconciliation pass.


## Step 4: move the media


Payload upload documents store a filename and a URL. Pull each file and push it into the Cosmic media library, keeping a map from the old Payload ID to the new Cosmic media name so you can rewrite references later.


```text
const   idMap   =     new     Map  <  string  ,     string  >  (  )


for     (  const   doc   of   uploadDocs  )     {
const   res   =     await     fetch  (  doc  .  url  )
const   buffer   =     Buffer  .  from  (  await   res  .  arrayBuffer  (  )  )


const     {   media   }     =     await   cosmic  .  media  .  insertOne  (  {
media  :     {   originalname  :   doc  .  filename  ,   buffer   }  ,
folder  :     'payload-import'  ,
alt_text  :   doc  .  alt     ??     ''  ,
}  )


idMap  .  set  (  doc  .  id  ,   media  .  name  )
}
```


If you set` alt` text in Payload, carry it across now. Alt text lives on the Cosmic media record itself, so every Object referencing that image inherits it, which is one less accessibility cleanup later.


## Step 5: convert rich text


This is the step that surprises people, so budget real time for it.


Payload stores rich text as a structured JSON tree, not as markdown or HTML. You cannot drop that JSON into a markdown metafield and expect it to render. You need a serializer that walks the tree and emits markdown or HTML.


Payload's rich text documentation covers converting its editor state to other formats, and that is the tool to reach for. Two things to watch:


- **Uploads embedded in rich text** become nodes referencing an upload ID. Rewrite those to the Cosmic media URLs from your` idMap` in Step 4.
- **Blocks embedded in rich text** need a destination. Either flatten them into markdown, or create a reusable Cosmic Content Block and reference it with a` {{block-name /}}` token in a rich text metafield.


Convert one document, eyeball the output, then convert the rest. Do not batch-convert 2,000 documents on faith.


## Step 6: import into Cosmic


With media mapped and rich text serialized, the import itself is short. Keep a Payload-ID to Cosmic-ID map as you go so relationship fields can be resolved on a second pass.


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'
import     posts     from     './payload-export/posts.json'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  !  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY  !  ,
}  )


for     (  const   doc   of   posts  )     {
await   cosmic  .  objects  .  insertOne  (  {
type  :     'posts'  ,
title  :   doc  .  title  ,
slug  :   doc  .  slug  ,
status  :   doc  .  _status     ===     'published'     ?     'published'     :     'draft'  ,
metadata  :     {
excerpt  :   doc  .  excerpt     ??     ''  ,
content  :     await     toMarkdown  (  doc  .  content  )  ,
hero  :   doc  .  hero     ?   idMap  .  get  (  doc  .  hero  )     :     null  ,
author  :   authorIdMap  .  get  (  doc  .  author  )  ,
}  ,
}  )
}
```


Preserve the original` slug` values. That single decision is what keeps your URLs, your rankings, and your inbound links intact.


## Step 7: rewire the frontend


If your app is Next.js, this is the most mechanical part of the whole project. Payload Local API calls become Cosmic SDK calls.


Before:


```text
const   payload   =     await     getPayload  (  {   config   }  )
const     {   docs   }     =     await   payload  .  find  (  {
collection  :     'posts'  ,
limit  :     10  ,
sort  :     '-publishedDate'  ,
}  )
```


After:


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  !  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
}  )


const     {   objects   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'posts'     }  )
.  props  (  [  'title'  ,     'slug'  ,     'metadata'  ]  )
.  depth  (  1  )
.  limit  (  10  )
.  sort  (  '-created_at'  )
```


Use` .props()` to request only the fields the page renders. Payload's Local API had no network cost, so over-fetching was cheap. Over an HTTP API it is not, and trimming the payload is the single easiest performance win in the port.


## Step 8: verify, then cut over


Work through this before you flip DNS:


1. **Counts match.** Compare every Object Type count against the inventory from Step 1.
2. **Spot-check the ugly documents.** Not the simple ones. The post with nested blocks, four images, and a relationship array.
3. **Every relationship resolves.** Query with` .depth(1)` and confirm nothing comes back null.
4. **Slugs are identical** to production Payload for every public URL.
5. **Media loads** on a real page render, not only in the dashboard.
6. **Redirects staged** for any URL that genuinely had to change.
7. **Keep Payload running** in parallel until the checks pass. There is no prize for deleting it early.


## What this costs on Cosmic


Verified against[cosmicjs.com/pricing](https://cosmicjs.com/pricing) on August 14, 2026:


Plan Price Buckets Team members Objects


Free $0/mo 1 2 1,000


Builder $49/mo 2 3 5,000


Team $299/mo 3 5 20,000


Business $499/mo 5 10 50,000


Enterprise Custom Custom Custom Custom


Additional team members beyond a plan's included seats are $29/user/month.


The practical move is to run the whole migration on the Free plan first. One Bucket and 1,000 Objects is enough to prove the export, the transform, and the import against real content before anyone approves a budget line.


## FAQ


**Is Payload CMS being shut down?**
No, and nothing on this page should be read that way. Payload is open source and self-hostable, and Payload states that existing Cloud projects continue running as normal. What changed is that new Payload Cloud deployments are paused, and Payload says existing Cloud projects will eventually need to migrate to a future replacement.


**Can I keep my URLs?**
Yes, as long as you carry the` slug` field across unchanged. Cosmic Objects have a slug you control, so a one-to-one mapping is normal.


**Does Cosmic support GraphQL?**
No. Cosmic provides a REST API and a TypeScript SDK. Payload does expose GraphQL, so if your frontend depends on it, plan on rewriting that data layer.


**How long does a migration take?**
It depends almost entirely on rich text and blocks. A handful of collections with plain fields is an afternoon. A heavily block-driven site with thousands of documents is a multi-week project, and the serializer is where the time goes.


**Can I run both during the transition?**
Yes, and you should. Import into Cosmic, point a staging branch at it, verify against the Step 8 checklist, and only then cut over.


## Next steps


If you were on Payload Cloud for the managed hosting and you want to stay out of the infrastructure business, the cheapest way to evaluate Cosmic is to migrate one collection and look at the result.


[Create a free Bucket](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=website&utm_campaign=migrate-payload-cms-to-cosmic&utm_content=closing-cta) and run the export script above against your smallest collection. If you would rather walk through your content model with someone first,[book time with our CEO Tony Spiro](https://calendly.com/tonyspiro/cosmic-intro) .


More reading:


- [Payload CMS Alternative](https://www.cosmicjs.com/payload-alternative)
- [Payload CMS vs Cosmic: Which Headless CMS Is Right for You?](https://www.cosmicjs.com/blog/payload-cms-vs-cosmic-which-headless-cms-is-right-for-you)
- [Payload vs Strapi: Which Open-Source Headless CMS Should You Choose in 2026?](https://www.cosmicjs.com/blog/payload-vs-strapi)
