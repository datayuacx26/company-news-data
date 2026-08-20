---
schema_version: "1.0.0"
document_id: "dfbb08e4404a214cd7806508a27d4e52f217cb4db309cee873a4c2859e04c9cf"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/sdk-2-zero-dependencies-native-fetch-smarter-media-uploads"
published_at: "2026-04-07T00:00:00+00:00"
first_seen_at: "2026-08-10T04:57:33.751116+00:00"
fetched_at: "2026-08-10T04:57:36.475692+00:00"
content_hash: "sha256:664cb9781e7a7594fc46e067c71a2aa0ac608e4bbdc0da9f460245bcd79b03ee"
---

# SDK 2.0: Zero Dependencies, Native Fetch, and Smarter Media Uploads

The` @cosmicjs/sdk` has been rebuilt from the inside out. Version 2.0 removes all runtime dependencies, replaces axios with the native` fetch` API, and adds automatic MIME type detection for media uploads. The public API is unchanged — upgrade with confidence.


---


## Zero Runtime Dependencies


The SDK no longer ships with` axios` or` form-data` . Every HTTP request now goes through the native` fetch` ,` FormData` , and` Blob` APIs built into Node.js 18+ and all modern browsers.


- **Before:** 2 runtime dependencies (` axios` ,` form-data` ) plus 7 transitive packages
- **After:** 0 runtime dependencies
- **Result:** Smaller` node_modules` , faster installs, reduced supply chain surface area


## Native Fetch


All request handling has been rewritten to use the[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) . This means the SDK works identically in Node.js, Bun, Deno, Cloudflare Workers, and the browser without polyfills or adapters.


```text
// Nothing changes in your application code
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :     'YOUR_BUCKET_SLUG'  ,
readKey  :     'YOUR_READ_KEY'  ,
writeKey  :     'YOUR_WRITE_KEY'  ,
}  )


const     {   objects   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'posts'     }  )
.  props  (  [  'title'  ,     'slug'  ,     'metadata'  ]  )
.  limit  (  10  )
```


## Automatic MIME Type Detection for Media Uploads


Media uploads now automatically detect the correct MIME type from the filename. Previously, uploads without an explicit` contentType` were sent as` application/octet-stream` , which could cause issues with image processing and CDN optimization.


The SDK recognizes 25+ file types out of the box — images, video, audio, documents, fonts, and more.


```text
// MIME type is inferred from the filename
const   buffer   =   fs  .  readFileSync  (  'photo.jpg'  )
await   cosmic  .  media  .  insertOne  (  {
media  :   buffer  ,
filename  :     'photo.jpg'  ,     // → image/jpeg detected automatically
}  )


// Or with the object format
await   cosmic  .  media  .  insertOne  (  {
media  :     {   buffer  ,   originalname  :     'hero.webp'     }  ,     // → image/webp
}  )


// You can still set it explicitly
await   cosmic  .  media  .  insertOne  (  {
media  :   buffer  ,
filename  :     'data.bin'  ,
contentType  :     'application/custom-type'  ,
}  )
```


## Batch Operations


Also new since 1.8: the` objects.batch()` method lets you create, update, and delete multiple objects in a single API call. Each operation succeeds or fails independently.


```text
await   cosmic  .  objects  .  batch  (  [
{   method  :     'add'  ,   object  :     {   title  :     'Post 1'  ,   type  :     'posts'     }     }  ,
{   method  :     'add'  ,   object  :     {   title  :     'Post 2'  ,   type  :     'posts'     }     }  ,
{   method  :     'edit'  ,   object_id  :     'existing-id'  ,   object  :     {   title  :     'Updated'     }     }  ,
{   method  :     'delete'  ,   object_id  :     'old-id'     }  ,
]  )
```


## Breaking Changes


**Node.js 18+ is required.** The native` fetch` API was added in Node.js 18.0. If you're on an older version, stay on` @cosmicjs/sdk@1.x` .


That's the only breaking change. The entire public API — objects, media, object types, object revisions, and AI — is fully backward compatible.


## Upgrade


```text
npm     install   @cosmicjs/sdk@latest
# or
bun   add   @cosmicjs/sdk@latest
```


## Full Changelog


Version Change


**2.0.1** Automatic MIME type detection for media uploads


**2.0.0** Replace` axios` and` form-data` with native` fetch` ,` FormData` , and` Blob` . Zero runtime dependencies. Requires Node.js >= 18.


**1.9.0** Add` objects.batch()` method for bulk create, update, and delete operations


---


Read the docs:[API Reference](https://www.cosmicjs.com/docs/api) ,[SDK on npm](https://www.npmjs.com/package/@cosmicjs/sdk)
