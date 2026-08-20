---
schema_version: "1.0.0"
document_id: "99bd2e11c73e4e76f83da41a30920f8106fab2db4c91311687637be53b784a72"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/headless-cms-for-svelte-step-by-step-tutorial"
published_at: "2026-04-10T00:00:00+00:00"
first_seen_at: "2026-08-10T04:57:33.751116+00:00"
fetched_at: "2026-08-10T04:57:36.475692+00:00"
content_hash: "sha256:dfa28c243b3e049e124b56e76e7977f246fa51ef8a5ca077074b200fd906af7b"
---

# Headless CMS for Svelte: Step-by-Step Tutorial

SvelteKit is one of the leanest, fastest web frameworks available. Its compiler-based approach means almost no runtime JavaScript — and pairing it with Cosmic means your content delivery is equally lean. The Cosmic REST API returns structured JSON fast, and SvelteKit's server-side` load` functions are the perfect place to call it.


In this tutorial, you'll build a SvelteKit app connected to Cosmic, fetch content in` load` functions using the TypeScript SDK, render it in Svelte components, and deploy to Vercel.


**Target keyword:** Svelte headless CMS tutorial


---


## Prerequisites


- Node.js 18+
- A free Cosmic account ([sign up here](https://app.cosmicjs.com/signup?utm_source=organic&utm_medium=landing&utm_campaign=svelte-headless-cms) )
- Basic familiarity with Svelte and TypeScript


---


## Step 1: Create a SvelteKit App


Scaffold a new SvelteKit project:


```text
npm   create svelte@latest my-cosmic-svelte-app
cd   my-cosmic-svelte-app
npm     install
```


When prompted, choose:


- **Skeleton project**
- **TypeScript** (yes)
- Any additional linting options you prefer


Start the dev server:


```text
npm   run dev
```


You should see the default SvelteKit welcome page at` http://localhost:5173` .


---


## Step 2: Install the Cosmic TypeScript SDK


Install the official Cosmic SDK:


```text
npm     install   @cosmicjs/sdk
```


This gives you` createBucketClient` — a fully typed client for querying your Cosmic bucket.


---


## Step 3: Set Up Your Cosmic Bucket


1. Log in to[Cosmic](https://app.cosmicjs.com/) and create a new bucket (or use an existing one).
2. Create an Object Type called` blog-posts` with these metafields:


- ` title` (text)
- ` content` (markdown)
- ` published_date` (date)
- ` teaser` (textarea)


3. Add a few test blog post objects.
4. Navigate to **Settings > API Keys** in your bucket and copy your **Bucket Slug** and **Read Key** .


---


## Step 4: Configure Environment Variables


Create a` .env` file in your project root:


```text
PUBLIC_COSMIC_BUCKET_SLUG  =  your-bucket-slug
PUBLIC_COSMIC_READ_KEY  =  your-read-key
```


SvelteKit exposes environment variables prefixed with` PUBLIC_` to both server and client. For server-only secrets, use unprefixed variables and access them via` $env/static/private` .


---


## Step 5: Create a Cosmic Client Module


Create a reusable server-only client module:


```text
// src/lib/server/cosmic.ts
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;
import     {   env   }     from     '$env/static/private'  ;


export     const   cosmic   =     createBucketClient  (  {
bucketSlug  :   env  .  COSMIC_BUCKET_SLUG  ,
readKey  :   env  .  COSMIC_READ_KEY  ,
}  )  ;
```


By placing this in` src/lib/server/` , SvelteKit enforces that it's never imported by client-side code — keeping your keys secure.


---


## Step 6: Fetch Content in a Load Function


Create a blog index route:


```text
// src/routes/blog/+page.server.ts
import     {   cosmic   }     from     '$lib/server/cosmic'  ;
import     type     {     PageServerLoad     }     from     './$types'  ;


export     const   load  :     PageServerLoad     =     async     (  )     =>     {
const     {   objects  :   posts   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'     }  )
.  props  (  'id,title,slug,metadata'  )
.  sort  (  '-created_at'  )
.  limit  (  20  )  ;


return     {   posts   }  ;
}  ;
```


Now render the data in your Svelte component:


```text
<!-- src/routes/blog/+page.svelte -->
<script lang="ts">
import type { PageData } from './$types';


export let data: PageData;
</script>


<main class="max-w-2xl mx-auto py-12 px-4">
<h1 class="text-4xl font-bold mb-8">Blog</h1>
<ul class="space-y-8">
{#each data.posts as post}
<li>
<a
href="/blog/{post.slug}"
class="text-2xl font-semibold hover:underline"
>
{post.title}
</a>
{#if post.metadata?.teaser}
<p class="mt-2 text-gray-600">{post.metadata.teaser}</p>
{/if}
</li>
{/each}
</ul>
</main>
```


---


## Step 7: Fetch a Single Post


Create a dynamic route for individual blog posts:


```text
// src/routes/blog/[slug]/+page.server.ts
import     {   cosmic   }     from     '$lib/server/cosmic'  ;
import     {   error   }     from     '@sveltejs/kit'  ;
import     type     {     PageServerLoad     }     from     './$types'  ;


export     const   load  :     PageServerLoad     =     async     (  {   params   }  )     =>     {
try     {
const     {   object  :   post   }     =     await   cosmic  .  objects
.  findOne  (  {
type  :     'blog-posts'  ,
slug  :   params  .  slug  ,
}  )
.  props  (  'id,title,slug,metadata'  )  ;


return     {   post   }  ;
}     catch     (  err  )     {
throw     error  (  404  ,     'Post not found'  )  ;
}
}  ;
```


And the corresponding Svelte component:


```text
<!-- src/routes/blog/[slug]/+page.svelte -->
<script lang="ts">
import type { PageData } from './$types';


export let data: PageData;
const { post } = data;
</script>


<article class="max-w-2xl mx-auto py-12 px-4">
<h1 class="text-4xl font-bold mb-4">{post.title}</h1>


{#if post.metadata?.published_date}
<time class="text-sm text-gray-500">
{new Date(post.metadata.published_date).toLocaleDateString()}
</time>
{/if}


{#if post.metadata?.content}
<div class="mt-8 prose">
{@html post.metadata.content}
</div>
{/if}
</article>
```


---


## Step 8: Add TypeScript Types for Your Content


Keep things clean with a shared types file:


```text
// src/lib/types.ts
export     type     BlogPost     =     {
id  :     string  ;
title  :     string  ;
slug  :     string  ;
metadata  :     {
teaser  ?  :     string  ;
content  ?  :     string  ;
published_date  ?  :     string  ;
}  ;
}  ;
```


Import and use these types in your load functions and components for full end-to-end type safety.


---


## Step 9: Deploy to Vercel


Install the SvelteKit Vercel adapter:


```text
npm     install   @sveltejs/adapter-vercel
```


Update your` svelte.config.js` :


```text
import     adapter     from     '@sveltejs/adapter-vercel'  ;
import     {   vitePreprocess   }     from     '@sveltejs/vite-plugin-svelte'  ;


/**   @type     {  import  (  '@sveltejs/kit'  )  .  Config  }   */
const   config   =     {
preprocess  :     vitePreprocess  (  )  ,
kit  :     {
adapter  :     adapter  (  )  ,
}  ,
}  ;


export     default   config  ;
```


Install the Vercel CLI and deploy:


```text
npm     install   -g vercel
vercel
```


Add your environment variables in the Vercel dashboard under **Settings > Environment Variables** :


- ` COSMIC_BUCKET_SLUG`
- ` COSMIC_READ_KEY`


Deploy to production:


```text
vercel --prod
```


---


## What's Next


You now have a fully functional SvelteKit app powered by Cosmic. From here you can:


- Add more Object Types (products, team members, case studies)
- Use Cosmic's AI Agents to generate content automatically
- Connect the Cosmic MCP Server to your AI toolchain
- Use the Cosmic CLI to manage content models from the terminal


---


## Pricing


Cosmic's Free plan is forever free — no credit card required.


Plan Price Objects


Free $0/month 1,000


Builder $49/month 5,000


Team $299/month 20,000


Business $499/month 50,000


Additional users are $29/user/month on any plan.


---


## Related Resources


- [Headless CMS for Svelte — Landing Page](https://www.cosmicjs.com/headless-cms-for-svelte)
- [Headless CMS for Vue](https://www.cosmicjs.com/headless-cms-for-vue)
- [Cosmic Docs](https://cosmicjs.com/docs)


---


**Ready to build?**[Sign up free](https://app.cosmicjs.com/signup?utm_source=organic&utm_medium=landing&utm_campaign=svelte-headless-cms) — no credit card required. Or[book a demo with Tony](https://calendly.com/tonyspiro/cosmic-intro) to get a guided walkthrough.
