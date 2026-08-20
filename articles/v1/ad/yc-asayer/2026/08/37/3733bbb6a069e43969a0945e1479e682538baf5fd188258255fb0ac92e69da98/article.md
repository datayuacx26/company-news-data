---
schema_version: "1.0.0"
document_id: "3733bbb6a069e43969a0945e1479e682538baf5fd188258255fb0ac92e69da98"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/fetch-data-react-server-components/"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T18:30:11.746057+00:00"
fetched_at: "2026-08-04T18:43:13.169222+00:00"
content_hash: "sha256:3a3d8e463a34444651f07410d9e6d5214b53136b2244629eb63f0ad3669064ac"
---

# Fetching Data Inside React Server Components

In a React Server Component, you fetch data by making the component an` async` function and` await` -ing the request directly in the function body — no` useEffect` , no loading state, and no client-side API round-trip. This is the default model in the Next.js App Router, and it inverts two habits carried over from client React: data fetching moves into render, and the biggest footgun is caching, which changed direction in Next.js 15. This article covers the current pattern, the caching reality in Next.js 16, why` "use server"` has nothing to do with Server Components, and how to avoid request waterfalls.


## Key Takeaways


- In a Server Component you fetch by making the component` async` and awaiting the request; since Server Components are rendered on the server, credentials and query logic are not included in the client bundle, so you can[query a database directly with an ORM](https://nextjs.org/docs/app/getting-started/fetching-data) .
- By default,[fetch requests are not cached](https://nextjs.org/docs/app/guides/caching-without-cache-components) in current Next.js — the reverse of the Next.js 13/14 behavior many tutorials still describe.
- To cache, you opt in explicitly:` { cache: 'force-cache' }` or` { next: { revalidate } }` in the previous model, or the` use cache` directive once` cacheComponents: true` is set.
- There is no directive for Server Components; the[“use server” directive is used for Server Functions](https://react.dev/reference/rsc/server-components) (Server Actions), a separate feature.
- Kick off independent fetches without` await` , then resolve them with` Promise.all` to avoid a sequential waterfall.


## How do you fetch data in a Server Component?


The core pattern is a single move: turn the component into an async function and await the request. Server Components are the default for layouts and pages in the App Router, so no directive is needed to get this behavior — async components are a feature of Server Components that[allow you to await in render](https://react.dev/reference/rsc/server-components) .


```text
// app/blog/page.tsx
export default async   function   Page  ()   {
const   res   =   await   fetch  (  '  https://api.vercel.app/blog  '  )
const   posts   =   await   res  .  json  ()
return   (
<  ul  >
{posts  .  map  ((  post  )   =>   (
<  li   key  =  {post  .  id}>{post  .  title}</  li  >
))  }
</  ul  >
)
}
```


Because the code runs only on the server, you can skip the API layer and query your data source directly. You can safely make database queries using an ORM or database client, though you should still[ensure requests are properly authenticated and authorized](https://nextjs.org/docs/app/getting-started/fetching-data) .


```text
// app/blog/page.tsx
import   {   db  ,   posts   }   from   '  @/lib/db  '


export default async   function   Page  ()   {
const   allPosts   =   await   db  .  select  ().  from  (  posts  )
return   (
<  ul  >
{allPosts  .  map  ((  post  )   =>   (
<  li   key  =  {post  .  id}>{post  .  title}</  li  >
))  }
</  ul  >
)
}
```


Compare this to the client pattern you’re replacing: fetching in a` useEffect` , storing the result in` useState` , and shipping the fetch logic (plus any secrets) to the browser. That approach causes an extra client-server round trip after the page loads; awaiting on the server removes it.


## fetch is not cached by default — the Next.js 15/16 change


In current Next.js,` fetch` is not cached by default — worth stating plainly, because most older content says the opposite. Per the[current Next.js fetch reference](https://nextjs.org/docs/app/api-reference/functions/fetch) , the default is` auto no cache` : Next.js fetches the resource from the remote server on every request. The previous caching model behaved the same way — by default,[fetch requests are not cached](https://nextjs.org/docs/app/guides/caching-without-cache-components) , and you cache an individual request by setting the` cache` option to` 'force-cache'` . In Next.js 13/14, an unqualified` fetch` was cached (` force-cache` ) by default, so any tutorial relying on that is out of date.


To cache under the previous model, opt in per request:


```text
// Cache until manually revalidated
await   fetch  (  '  https://api.example.com/posts  '  , {   cache  :   '  force-cache  '   })


// Serve cached data, revalidate at most every 3600s
await   fetch  (  '  https://api.example.com/posts  '  , {   next  :   {   revalidate  :   3600   }   })
```


Next.js 16 also ships a second model, Cache Components, whose opt-in is the` use cache` directive. The catch that trips people up: use cache is a Cache Components feature;[to enable it, add the cacheComponents option to your next.config.ts file](https://nextjs.org/docs/app/api-reference/directives/use-cache) . Without that flag,` use cache` does nothing and you fall back to the` fetch` options above.


```text
// next.config.ts
import   type   {   NextConfig   }   from   '  next  '
const   nextConfig  :   NextConfig   =   {   cacheComponents  :   true   }
export default   nextConfig
```


```text
import   {   cacheLife   }   from   '  next/cache  '


export   async   function   getPosts  () {
'  use cache  '
cacheLife  (  '  hours  '  )
const   res   =   await   fetch  (  '  https://api.example.com/posts  '  )
return   res  .  json  ()
}
```


Here the[use cache directive caches the return value of async functions and components](https://nextjs.org/docs/app/getting-started/caching) , and` cacheLife()` sets how long. Note that[unstable_cache is replaced by the use cache directive](https://nextjs.org/docs/app/guides/migrating-to-cache-components) in Next.js 16 — don’t reach for it in new code.


Cache Components OFF (previous model) Cache Components ON (` cacheComponents: true` )


` fetch` default Uncached Uncached


Cache a fetch` { cache: 'force-cache' }`` use cache` directive


Time-based revalidation` { next: { revalidate: n } }`` cacheLife()` profile


Cache non-fetch data` React.cache` / route config` use cache` on the function


Two behaviors are independent of caching. First, fetch requests using GET with the same URL and options are automatically memoized during a server render pass, so if you call the same fetch in multiple components, Next.js executes it once and shares the result. Second, for non-` fetch` sources, wrap the call in React’s` cache()` ;[React.cache is scoped to the current request only](https://nextjs.org/docs/app/getting-started/fetching-data) — each request gets its own memoization scope with no sharing between requests.


## ”use server” does not make a Server Component


If one thing derails RSC newcomers, it’s this directive. A common misunderstanding is that Server Components are denoted by “use server”, but[there is no directive for Server Components; the “use server” directive is used for Server Functions](https://react.dev/reference/rsc/server-components) . Server Components are simply the default in the App Router — a file with no directive is one.` "use server"` marks Server Functions (Server Actions), and per[React’s reference](https://react.dev/reference/rsc/server-functions) , Server Functions are designed for mutations that update server-side state; they are[not recommended for data fetching](https://react.dev/reference/rsc/use-server) . Use plain async Server Components to read data; use` "use server"` to write it.


## Avoid waterfalls and stream slow data


Awaiting requests one after another creates a sequential waterfall. Within any component, multiple async/await requests can still be sequential if placed after the other;[start multiple requests by calling fetch, then await them with Promise.all](https://nextjs.org/docs/app/getting-started/fetching-data) . Calling the functions without` await` kicks them off immediately:


```text
export default async   function   Page  ({   params   }  :   {   params  :   Promise  <{   username  :   string   }>   })   {
const   {   username   }   =   await   params
const   artistData   =   getArtist  (  username  )     // starts now
const   albumsData   =   getAlbums  (  username  )     // starts now
const   [  artist  ,   albums  ]   =   await   Promise  .  all  ([  artistData  ,   albumsData  ])
return   <  h1  >{artist  .  name}</  h1  >
}
```


For genuinely slow data, don’t block the route. If you have slow data requests, the whole route is blocked from rendering until all data is fetched; to improve load time, break the page into chunks and progressively send them to the client. Wrap the slow subtree in` <Suspense>` (or add a` loading.js` ), and for lower-priority data, start the promise on the server and read it on the client with the` use` API:


```text
// Server Component — note: commentsPromise is NOT awaited
import   {   Suspense   }   from   '  react  '
import   Comments   from   '  ./comments  '


export default async   function   Page  ({   id   }  :   {   id  :   string   })   {
const   commentsPromise   =   getComments  (  id  )
return   (
<  Suspense   fallback  =  {<  p  >Loading comments…</  p  >}>
<  Comments   commentsPromise  =  {commentsPromise} />
</  Suspense  >
)
}
```


```text
// Client Component
'  use client  '
import   {   use   }   from   '  react  '


export default   function   Comments  ({   commentsPromise   }  :   {   commentsPromise  :   Promise  <  Comment  []>   })   {
const   comments   =   use  (  commentsPromise  )
return   comments  .  map  ((  c  )   =>   <  p   key  =  {c  .  id}>{c  .  text}</  p  >  )
}
```


You start the promise on the server and wait for it on the client with the use API;[since async components are not supported on the client, you await the promise with use](https://react.dev/reference/rsc/server-components) . When this goes wrong, the failure is only visible where the user is — a Suspense fallback that never resolves into content, or a streamed chunk that lands after hydration — and a session replay is what lets you reconstruct exactly what the browser rendered at that moment.


## Keep secrets on the server side of the boundary


Data fetched on the server crosses to Client Components as props, and those props must be serializable — pass plain data, not functions or class instances. Secrets stay put by default because Server Component code never ships, but shared modules can leak. In Next.js, only environment variables prefixed with NEXT_PUBLIC_ are included in the client bundle;[if variables are not prefixed, Next.js replaces them with an empty string](https://nextjs.org/docs/app/getting-started/server-and-client-components) . To harden a secret-bearing module, import the` server-only` package at its top so an accidental client import fails at build time. React Context also can’t live in a Server Component — wrap it in a` 'use client'` provider and render that provider inside your server layout.


The mental shift is small but complete: move the fetch into an async component, await it, and treat caching as something you opt into rather than something that happens to you. Start by porting one` useEffect` data fetch to an async Server Component, confirm the request runs server-side, then decide per route whether that data should be cached with` { next: { revalidate } }` or the` use cache` directive.


## FAQs


Is fetch cached by default in Next.js 16?


No. As of Next.js 15 and continuing in 16, fetch is not cached by default and hits the origin on every request. This is the reverse of Next.js 13/14, where an unqualified fetch was cached with force-cache by default. To cache under the previous model you opt in per request with cache set to force-cache or with a next revalidate value; older tutorials asserting cached-by-default behavior are out of date.


What is the difference between use cache and next revalidate for caching a fetch?


They belong to two different models. The next revalidate option and force-cache work in the previous, default caching model with no extra config. The use cache directive is a Cache Components feature that only takes effect when cacheComponents is set to true in next.config.ts, and its duration is controlled with cacheLife. Without that flag, use cache does nothing, so a fetch you meant to cache silently hits the origin every request.


Can I use useEffect to fetch data in a Server Component?


No. useEffect only runs in the browser, so it cannot execute inside a Server Component, which renders only on the server. In a Server Component you make the component async and await the request directly in render, with no loading state and no client round trip. If you need client-side fetching, add the use client directive to make it a Client Component, or start the promise on the server and read it on the client with React's use API.


Does the use server directive turn a component into a Server Component?


No. There is no directive for Server Components; they are simply the default in the App Router, so a file with no directive is already one. The use server directive marks Server Functions, also called Server Actions, which are designed for mutations that update server-side state and are not recommended for data fetching. Use a plain async Server Component to read data and use server to write it.


What happens if one fetch fails when using Promise.all for parallel requests?


Promise.all rejects as soon as any single request rejects, discarding the results of the others and failing the whole render. If one request should be allowed to fail without taking down the rest, use Promise.allSettled instead, which resolves with the status of every request and lets you handle failures individually. Start the fetches without await to run them in parallel, then choose Promise.all or Promise.allSettled based on how you want failures handled.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
