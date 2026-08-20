---
schema_version: "1.0.0"
document_id: "ce7cd65893843e275df087b5eaaf079e9f05a1bd76dc875cfa10b166a46f76fc"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/medusajs-nextjs-headless-cms"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-07T19:31:51.633599+00:00"
fetched_at: "2026-08-07T19:31:54.584119+00:00"
content_hash: "sha256:314e6154368b5c98a20de48dfa15e40d9821da63225abdad123444936347d313"
---

# Medusa.js + Next.js: How to Add a Content Layer to Your Storefront

[Medusa](https://medusajs.com/) is a strong commerce engine. It owns products, variants, pricing, inventory, carts, orders, and fulfillment, and it exposes all of it through a clean Store API. Once you have the Next.js Starter Storefront running, the commerce half of your site is basically solved.


Then marketing asks for a buying guide on every category page, a founder story block on the product detail page, a seasonal landing page that goes live Friday at 9am, and an FAQ section that changes weekly. None of that belongs in your commerce database, and none of it should require a deploy.


This post walks through the pattern we see working best: keep Medusa as the source of truth for commerce, add a headless CMS as the source of truth for editorial content, and join them in the Next.js layer on a shared key.


## The problem with putting content in Medusa


Medusa lets you attach arbitrary` metadata` to products. It is tempting to stuff your marketing copy in there and call it done. That falls apart quickly for three reasons.


1. **No editing experience.**` metadata` is a key-value bag. Your content team gets a JSON blob, no rich text, no image handling, no preview, no revision history.
2. **No content modeling.** A buying guide has a title, hero image, intro, sections, related products, and an author. Flattening that into string keys means every consumer has to re-parse it.
3. **Content that is not product-shaped has nowhere to live.** A holiday gift guide, a shipping policy page, a comparison table, a homepage hero: none of these map to a product record at all.


The fix is a second system with its own model and its own editor, joined at read time.


## Draw the ownership line first


Before you write any code, write down who owns what. This one decision prevents most of the sync bugs teams hit later.


Data Owner Why


Product handle, title, variants Medusa Commerce truth, drives cart and checkout


Price, inventory, availability Medusa Must be real time, never cached in a CMS


Cart, orders, customers, fulfillment Medusa Transactional


PDP long-form story, lifestyle imagery CMS Editorial, changes without a deploy


Buying guides, blog posts, landing pages CMS No product record exists


FAQs, size guides, care instructions CMS Reusable across many products


Navigation, promo banners, homepage blocks CMS Merchandising, changes weekly


The rule to enforce in code review: **price and inventory never come from the CMS.** Duplicating those into a content system creates a window where your site shows a price that checkout will reject. Read them from Medusa on every request.


## Step 1: Get the Medusa storefront running


Start from the official Next.js Starter Storefront so you inherit the cart, checkout, and account flows.


```text
npx create-medusa-app@latest my-store
```


That scaffolds a Medusa server and a Next.js storefront. Your storefront needs two environment values to talk to the backend:


```text
# .env.local
NEXT_PUBLIC_MEDUSA_BACKEND_URL  =  http://localhost:9000
NEXT_PUBLIC_MEDUSA_PUBLISHABLE_KEY  =  pk_  ..  .
```


The publishable key is created in the Medusa Admin under Settings, and it scopes requests to a sales channel. Requests to the Store API without it will be rejected.


Set up the Medusa JS SDK once and export a single client:


```text
// src/lib/medusa.ts
import     Medusa     from     '@medusajs/js-sdk'  ;


export     const   medusa   =     new     Medusa  (  {
baseUrl  :   process  .  env  .  NEXT_PUBLIC_MEDUSA_BACKEND_URL  !  ,
publishableKey  :   process  .  env  .  NEXT_PUBLIC_MEDUSA_PUBLISHABLE_KEY  !  ,
}  )  ;
```


Fetching a product by its handle looks like this:


```text
const     {   products   }     =     await   medusa  .  store  .  product  .  list  (  {
handle  :     'classic-tee'  ,
fields  :     '*variants.calculated_price'  ,
}  )  ;


const   product   =   products  [  0  ]  ;
```


## Step 2: Model the content side


Now add the content layer. In Cosmic, create an Object Type called **Product Content** with a metafield for every editorial element your PDP needs.


A model that has held up well in production:


Metafield key Type Purpose


` handle` Text (unique) The join key. Must match the Medusa product handle exactly.


` story` Markdown Long-form product narrative below the buy box


` lifestyle_images` Files Editorial photography separate from catalog shots


` care_guide` Markdown Reusable care or sizing content


` faqs` Repeater Question and answer pairs rendered as an accordion


` related_reading` Objects Links to blog posts or buying guides


The` handle` field is the whole design. Make it unique so two content entries can never claim the same product, and tell your editors it has to match Medusa character for character. Everything else in the model is free to change without touching commerce.


Install the Cosmic SDK and create the client:


```text
npm     install   @cosmicjs/sdk
```


```text
// src/lib/cosmic.ts
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;


export     const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  !  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
}  )  ;
```


Then a small helper that fetches content for a handle and returns` null` instead of throwing when nothing exists yet:


```text
// src/lib/product-content.ts
import     {   cosmic   }     from     './cosmic'  ;


export     async     function     getProductContent  (  handle  :     string  )     {
try     {
const     {   object   }     =     await   cosmic  .  objects
.  findOne  (  {   type  :     'product-content'  ,     'metadata.handle'  :   handle   }  )
.  props  (  'title,slug,metadata'  )
.  depth  (  1  )  ;


return   object  ;
}     catch     (  error  :     any  )     {
if     (  error  .  status     ===     404  )     return     null  ;
throw   error  ;
}
}
```


That` null` return matters. It is what lets you launch a product before marketing has written anything for it.


## Step 3: Join them in a server component


With both clients in place, the product page fetches from each system in parallel and renders one page.


```text
// src/app/products/[handle]/page.tsx
import     {   medusa   }     from     '@/lib/medusa'  ;
import     {   getProductContent   }     from     '@/lib/product-content'  ;
import     {   notFound   }     from     'next/navigation'  ;


export     default     async     function     ProductPage  (  {
params  ,
}  :     {
params  :     Promise  <  {   handle  :     string     }  >  ;
}  )     {
const     {   handle   }     =     await   params  ;


const     [  {   products   }  ,   content  ]     =     await     Promise  .  all  (  [
medusa  .  store  .  product  .  list  (  {
handle  ,
fields  :     '*variants.calculated_price'  ,
}  )  ,
getProductContent  (  handle  )  ,
]  )  ;


const   product   =   products  [  0  ]  ;
if     (  !  product  )     notFound  (  )  ;


return     (
<  main  >
{  /* Commerce: always from Medusa */  }
<  h1  >  {  product  .  title  }  </  h1  >
<  BuyBox     product  =  {  product  }     />


{  /* Editorial: from the CMS, optional by design */  }
{  content  ?.  metadata  .  story     &&     (
<  section     className  =  "  product-story  "  >
<  Markdown  >  {  content  .  metadata  .  story  }  </  Markdown  >
</  section  >
)  }


{  content  ?.  metadata  .  faqs  ?.  length   >     0     &&     (
<  FaqAccordion     items  =  {  content  .  metadata  .  faqs  }     />
)  }
</  main  >
)  ;
}
```


Three things worth calling out.


**Medusa decides whether the page exists.** If there is no product, you 404. A content entry with no matching product should never render a page, because there would be nothing to add to a cart.


**Every content block is conditional.** The page has to render correctly with zero CMS content. Test that path deliberately by pointing a local build at a handle that has no entry.


**Fetch in parallel.**` Promise.all` keeps the two round trips from stacking. On a PDP that difference is visible in your LCP.


## Step 4: Keep pages fresh without redeploying


Static rendering is what makes this pattern fast. Cache invalidation is what makes it usable for a content team.


Tag your content fetches, then invalidate the tag from a webhook.


```text
// src/app/api/revalidate/route.ts
import     {   revalidateTag   }     from     'next/cache'  ;
import     {     NextRequest  ,     NextResponse     }     from     'next/server'  ;


export     async     function     POST  (  request  :     NextRequest  )     {
const   secret   =   request  .  headers  .  get  (  'x-webhook-secret'  )  ;


if     (  secret   !==   process  .  env  .  REVALIDATE_SECRET  )     {
return     NextResponse  .  json  (  {   error  :     'Unauthorized'     }  ,     {   status  :     401     }  )  ;
}


const   payload   =     await   request  .  json  (  )  ;
const   handle   =   payload  ?.  data  ?.  metadata  ?.  handle  ;


if     (  handle  )     {
revalidateTag  (  `  product-content-  ${  handle  }  `  )  ;
}


return     NextResponse  .  json  (  {   revalidated  :     true     }  )  ;
}
```


Point a Cosmic webhook at that route for the Object Edited and Object Published events. An editor publishes a change, the webhook fires, that single product page rebuilds. No deploy, no full site rebuild, no Slack message to a developer.


Do the same in reverse for Medusa: subscribe to product update events and revalidate the matching path when a title or description changes.


One exception to keep in mind. Price and inventory should not rely on webhook timing. Render those parts dynamically or fetch them client side so a customer never sees a cached price that checkout will reject.


## Step 5: Everything that is not a product page


The PDP join is the interesting technical part. The larger traffic win is usually the pages that have no product record at all.


With a content layer already wired in, these become straightforward:


- **Buying guides and comparison pages.** These are the pages that rank for research-stage queries. Model them as a` guides` type with a` related_products` field holding a list of handles, then hydrate live prices from Medusa at render time.
- **Category landing pages.** Medusa gives you the collection and its products. The CMS gives you the hero, the intro copy, and the merchandising blocks above the grid.
- **Campaign pages.** Marketing builds and schedules them without a developer. Cosmic supports scheduled publishing, so a Friday 9am launch is a field, not a deploy window.
- **Policy and support pages.** Shipping, returns, sizing. Low glamour, high support-ticket deflection.


Same pattern each time: content from the CMS, live commerce data from Medusa, joined on the handle.


## Four mistakes to avoid


**Copying prices into the CMS.** It will drift. When it drifts, customers see one number and get charged another. Always read price from Medusa.


**Using the product ID as the join key.** IDs change when you reseed a database or migrate environments. Handles are stable, human readable, and already unique in Medusa.


**Making content required.** If a missing CMS entry breaks the page, you have coupled your product launches to your content calendar. Optional by default.


**Two sources of truth for the product title.** Pick one, and it should be Medusa, since that is what appears on the order. If marketing needs a different display headline, add a separate` display_headline` field so the intent is explicit.


## Why Cosmic fits this stack


A few specifics that matter when you are pairing a CMS with Medusa:


- **REST API and a TypeScript SDK.**` @cosmicjs/sdk` is typed and works in Next.js server components without adapters or codegen steps.
- **Fully managed.** You are already running a Medusa server and a database. Adding a second self-hosted service to patch and scale is real operational cost. Cosmic is hosted, so the content layer is one API call, not another deployment.
- **AI-assisted authoring.** Editors can draft product stories, FAQs, and guide content directly in the dashboard, which is where the volume problem usually lives for catalogs with thousands of SKUs.
- **Scheduled publishing and webhooks.** Both are built in, which is what makes the revalidation flow above work end to end.


As Maximilian Wuhr, Co-Founder at FINN, put it: "Cosmic is: us never having to ask a developer to change anything on the backend of our website."


## Get started


The whole integration is about 60 lines of glue code: one Medusa client, one Cosmic client, one join key, one webhook route.


[Start free with Cosmic](https://app.cosmicjs.com/signup) and model your first Product Content type in a few minutes. The[Cosmic docs](https://www.cosmicjs.com/docs) cover the SDK and webhook setup, and the[Medusa docs](https://docs.medusajs.com/) cover the Store API side.


If you are planning a larger migration or running a catalog with thousands of SKUs,[book time with Tony](https://calendly.com/tonyspiro/cosmic-intro) , our CEO, and he will walk through the content model with you.


## FAQ


**Can I use Cosmic to manage products instead of Medusa?**
You can model product content in Cosmic, but you should not replace Medusa's product records. Carts, pricing rules, inventory, and orders depend on them. Use Cosmic for the editorial layer around the catalog.


**Does this work with the Medusa Next.js Starter Storefront?**
Yes. The pattern above drops into the starter's existing product route. You are adding a second data fetch alongside the Medusa one, and leaving the cart and checkout flows untouched.


**Does Cosmic support GraphQL?**
No. Cosmic offers a REST API and the JavaScript/TypeScript SDK. Both work well with React Server Components, and the SDK handles query building for you.


**How do I handle products with no content entry yet?**
Return` null` from your content helper on a 404 and render every content block conditionally, as shown in Step 2 and Step 3. The product page stays fully functional.


**What about multi-region or multi-language storefronts?**
Medusa handles regional pricing and currency. Cosmic supports locales on objects, so you can request content in the visitor's locale using the same handle-based lookup.
