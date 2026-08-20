---
schema_version: "1.0.0"
document_id: "548044eacdd6fa5021f1d042ad4d265e836b4f395fb23f8bbe7b7d8d24d630c9"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/migrate-react-app-axios-fetch/"
published_at: "2026-07-12T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:70719bad998efe6872d55a2af7cd26823ac07eb157f2b40f0a172121145ab748"
---

# Migrating a React App from Axios to Fetch

Migrating a React app from Axios to` fetch` is a low-risk change *if you replace the library behind a wrapper instead of rewriting call sites* — and a regression minefield if you don’t. The two clients solve the same problem differently:` fetch` is the native, dependency-free browser standard that hands you raw ingredients, while Axios bundles ergonomics — base URL, automatic JSON, automatic rejection on error responses,` response.data` , interceptors, and timeouts — that you must re-create yourself. This guide resolves the should-I-switch question first, then gives you a sequenced, regression-safe migration path for a real React app, including one complete copy-pasteable client.


## Key Takeaways


- The single most dangerous difference:` fetch()` does not reject on HTTP 4xx or 5xx — it resolves successfully and hands you a` Response` with` ok: false` , so a 500 sails through your` try` block as if it were a 200 unless you check` response.ok` yourself.
- Migrate the wrapper, not the call sites: if your new` fetch` client exposes the same` get/post/patch/delete` signatures and returns the same promises, React Query, your mutations, and your error handling never know Axios is gone.
- Use` AbortSignal.timeout(8000)` to give` fetch` the request timeout Axios had built in — it has been Baseline across browsers since April 2024 and aborts with a` TimeoutError` .
- ` fetch` is no longer browser-only: it is a stable global in Node.js since v21, removing the last reason the old advice “keep Axios for Node” was ever true.
- Order matters: swap every import to the new client first, then remove` axios` and refresh the lockfile — reverse that and your build breaks mid-migration.


## Axios vs Fetch: the verdict


**Choose` fetch` when you use only a thin slice of Axios and want to drop a runtime dependency; keep Axios when you lean on its higher-level features.** Axios gives you base URL, automatic JSON, automatic rejection on failed responses,` response.data` , and interceptors for free; with` fetch` you re-create exactly the subset your app actually uses — and nothing more. For most React SPAs that already centralize requests in one API module, that subset is small.


The motivations to migrate are concrete:


- **Bundle size.** Axios is an additional runtime dependency — roughly 13–14 KB minified and gzipped, version-dependent — whereas a` fetch` wrapper is a few hundred lines of code you own and can tree-shake. The exact figure moves with each release, so measure it against your installed version with a tool like[Bundlephobia](https://bundlephobia.com/package/axios) rather than trusting a hardcoded number.
- **One fewer dependency / smaller supply-chain surface.** Reducing dependencies is also a security posture. In March 2026, attackers published two backdoored Axios versions to npm through a compromised maintainer account, and every project on a floating version range pulled them on the next install. The[maintainers’ post-mortem (GitHub issue #10636)](https://github.com/axios/axios/issues/10636) and[Microsoft’s security advisory](https://www.microsoft.com/en-us/security/blog/2026/04/01/mitigating-the-axios-npm-supply-chain-compromise/) both confirm this was a distribution/account compromise — two malicious published versions, not a flaw in Axios’s source — and the bad versions were pulled within hours;[CISA issued an alert](https://www.cisa.gov/news-events/alerts/2026/04/20/supply-chain-compromise-impacts-axios-node-package-manager) as well. The lesson isn’t “Axios is unsafe”; it’s that one fewer dependency is one less account that can be compromised on your behalf.
- **You use only a sliver.** If your code is` axios.get(url).then(r => r.data)` , you’re paying for a library to do what eight lines of` fetch` do.


### When to keep Axios


Keep Axios — migration is not worth it — if any of these describe your app:


Keep Axios if you rely on… Why` fetch` makes it painful


Heavy use of request/response **interceptors**` fetch` has no interceptor system; you’d rebuild it


**Upload progress** UI` fetch` request bodies don’t expose upload progress events; you need` XMLHttpRequest`


**Legacy browser** support (e.g. IE11)` fetch` was never available there


A large surface of Axios-specific config (` paramsSerializer` , transformers, adapters) Re-creating it costs more than the dependency saves


One reason that *no longer* belongs on this list is Node.js.` fetch` is no longer a browser-only story: Node.js 18 (April 2022)[shipped a global fetch enabled by default](https://nodejs.org/en/blog/announcements/v18-release-announce) , and Node.js 21 (October 2023)[promoted fetch and Web Streams to stable](https://nodejs.org/en/blog/announcements/v21-release-announce) . For a deeper feature-by-feature comparison, LogRocket’s[Axios vs Fetch breakdown](https://blog.logrocket.com/axios-vs-fetch-2025/) covers download progress, streaming, and cancellation in detail.


## The behavioral gaps you must close


Axios was doing six things implicitly that` fetch` will not — and each is a silent behavioral change, code that compiles and runs but behaves differently. Internalize these gaps before writing a wrapper.


1. **` fetch` does not reject on 4xx/5xx.** This is the footgun. Per[MDN’s Fetch API reference](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) , the promise resolves as soon as the server responds with headers — *even on an HTTP error status* . A 500 resolves with` response.ok === false` . Axios auto-rejects non-2xx into` .catch()` ;` fetch` does not. You must check` response.ok` and throw yourself.
2. **No automatic JSON.** Axios stringifies request bodies and parses responses for you. With` fetch` , you call` JSON.stringify()` on send and` await response.json()` on receive, and set` Content-Type: application/json` manually.
3. **No` baseURL` .** Axios prepends a configured base URL.` fetch` takes a full URL or one relative to the document; you join paths yourself.
4. **No` response.data` .** The parsed body lives behind` await response.json()` , not on a` .data` property.
5. **No interceptors.** There is no built-in hook for attaching auth tokens or handling 401s globally.
6. **No timeout option.** Axios has a` timeout` config;` fetch` has none. You supply an` AbortSignal` .


### Before and after


The same call, in Axios and in the client you’ll build:


```text
// Axios — auto-reject on error, response.data, auto-JSON
const   {   data   }   =   await   axios  .  get  (  `  /users/  ${  id  }`  );
return   data  ;


// fetch (raw) — must check ok, must parse, no baseURL
const   res   =   await   fetch  (  `  /api/users/  ${  id  }`  );
if (  !  res  .  ok  )   throw   new   Error  (  res  .  statusText  );   // <-- without this, a 500 looks like success
return   res  .  json  ();
```


## The strategy: migrate the wrapper, not the call sites


**Do not touch your components, hooks, or React Query calls. Replace the library behind your existing API surface.** If your app already routes requests through a single module exposing` get/post/patch/delete` , you only need to make the *new* implementation behavior-compatible — not Axios-compatible. Keep identical signatures, return the same promises, and throw the same error type, and everything downstream stays untouched. This is what makes the migration boring instead of risky.


The sequence:


1. **Audit usage.** Confirm calls go through one wrapper. If they don’t, your first PR is to route them through one — that refactor is independent of` fetch` and safe to ship on its own.
2. **Match the error contract.** If your UI branches on` error.code === 'EXPIRED'` or reads` error.status` , your new client must produce errors with those same fields. A mismatch here is a silent break.
3. **Preserve promise shape.** React Query, SWR, and your mutations care that you return a promise that resolves to data or rejects with an error — not which library produced it.


## A complete fetch client you can copy


This complete` api-client.ts` closes every gap from the comparison: base URL join, query params, header merge, JSON body, the` ok` -check that rejects error responses, a custom error class, auth token injection, central 401 handling, and a timeout via` AbortSignal.timeout()` . Drop it in, point your imports at it, and your call sites keep working.


```text
// lib/api-client.ts
const   BASE_URL   =   import  .  meta  .  env  .  VITE_API_URL   ??   "  /api  "  ;
const   DEFAULT_TIMEOUT   =   8000  ;


export   class   ApiClientError   extends   Error   {
status  :   number  ;
code  ?:   string  ;
data  ?:   unknown  ;
constructor  (  message  :   string  ,   status  :   number  ,   code  ?:   string  ,   data  ?:   unknown  ) {
super  (  message  );
this  .  name   =   "  ApiClientError  "  ;
this  .  status   =   status  ;
this  .  code   =   code  ;
this  .  data   =   data  ;
}
}


type   Query   =   Record  <  string  ,   string   |   number   |   boolean   |   undefined  >;


interface   RequestOptions   extends   Omit  <  RequestInit  ,   "  body  "  > {
body  ?:   unknown  ;
params  ?:   Query  ;
timeout  ?:   number  ;
}


function   buildUrl  (  path  :   string  ,   params  ?:   Query  )  :   string   {
const   base   =   BASE_URL  .  endsWith  (  "  /  "  )   ?   BASE_URL   :   `${  BASE_URL  }  /  `  ;
const   url   =   new   URL  (  path  .  replace  (  /^  \/  /  ,   ""  ),   new   URL  (  base  ,   window  .  location  .  origin  ));
if (  params  ) {
for (  const   [  key  ,   value  ]   of   Object  .  entries  (  params  )) {
if (  value   !==   undefined  )   url  .  searchParams  .  set  (  key  ,   String  (  value  ));
}
}
return   url  .  toString  ();
}


async   function   request  <  T  >(  path  :   string  ,   options  :   RequestOptions   =   {})  :   Promise  <  T  > {
const   {   body  ,   params  ,   timeout   =   DEFAULT_TIMEOUT  ,   headers  ,   ...  rest   }   =   options  ;
const   token   =   localStorage  .  getItem  (  "  token  "  );


let   res  :   Response  ;
try   {
res   =   await   fetch  (  buildUrl  (  path  ,   params  ), {
...  rest  ,
headers  :   {
"  Content-Type  "  :   "  application/json  "  ,
...  (  token   ?   {   Authorization  :   `  Bearer   ${  token  }`   }   :   {}),
...  headers  ,
},
body  :   body   !==   undefined   ?   JSON  .  stringify  (  body  )   :   undefined  ,
signal  :   AbortSignal  .  timeout  (  timeout  ),
});
}   catch   (  err  ) {
// Chromium may surface a fetch timeout as AbortError rather than TimeoutError.
if (  err   instanceof   DOMException   &&   (  err  .  name   ===   "  TimeoutError  "   ||   err  .  name   ===   "  AbortError  "  )) {
throw   new   ApiClientError  (  "  Request timed out  "  ,   0  ,   "  TIMEOUT  "  );
}
throw   new   ApiClientError  (  "  Network error  "  ,   0  ,   "  NETWORK  "  );
}


// The footgun: fetch resolves on 4xx/5xx. Reject explicitly.
if (  !  res  .  ok  ) {
let   payload  :   any  ;
try   {   payload   =   await   res  .  json  (); }   catch   {   payload   =   undefined  ; }


if (  res  .  status   ===   401  ) {                // interceptor-equivalent: central 401 handling
localStorage  .  removeItem  (  "  token  "  );
window  .  location  .  assign  (  "  /login  "  );
}
throw   new   ApiClientError  (
payload  ?.  message   ??   res  .  statusText  ,
res  .  status  ,
payload  ?.  code  ,
payload  ,
);
}


if (  res  .  status   ===   204  )   return   undefined   as   T  ;
return   (  await   res  .  json  ())   as   T  ;
}


export   const   api   =   {
get  :      <  T  >(  path  :   string  ,   options  ?:   RequestOptions  )   =>   request  <  T  >(  path  ,   {   ...  options  ,   method  :   "  GET  "   }),
post  :     <  T  >(  path  :   string  ,   body  ?:   unknown  ,   options  ?:   RequestOptions  )   =>   request  <  T  >(  path  ,   {   ...  options  ,   method  :   "  POST  "  ,   body   }),
patch  :    <  T  >(  path  :   string  ,   body  ?:   unknown  ,   options  ?:   RequestOptions  )   =>   request  <  T  >(  path  ,   {   ...  options  ,   method  :   "  PATCH  "  ,   body   }),
put  :      <  T  >(  path  :   string  ,   body  ?:   unknown  ,   options  ?:   RequestOptions  )   =>   request  <  T  >(  path  ,   {   ...  options  ,   method  :   "  PUT  "  ,   body   }),
delete  :   <  T  >(  path  :   string  ,   options  ?:   RequestOptions  )   =>   request  <  T  >(  path  ,   {   ...  options  ,   method  :   "  DELETE  "   }),
};
```


Two specifics worth calling out. The timeout uses` AbortSignal.timeout()` , which[returns an AbortSignal that will automatically abort after a specified time](https://developer.mozilla.org/en-US/docs/Web/API/AbortSignal/timeout_static) , and the signal aborts with a TimeoutError DOMException on timeout; since April 2024, this feature works across the latest devices and browser versions. Catch both` TimeoutError` and` AbortError` : in Chromium-based browsers a` fetch` timeout can surface as` AbortError` rather than` TimeoutError` , so handle both names defensively. The` 204` short-circuit avoids calling` .json()` on an empty body.


## Re-creating interceptors — and what to leave on XHR


**Your wrapper *is* your interceptor.** Everything Axios interceptors did — attach a Bearer token, normalize errors, redirect on 401 — lives in the` request()` function above, applied to every call by construction. You don’t need a plugin system; you need one chokepoint, which you already have. Adding retry-on-429 or request logging means adding a few lines to` request()` , not registering a handler.


One thing to deliberately *not* migrate: **upload progress.** Browser` fetch` request bodies don’t expose upload progress events the way` XMLHttpRequest` does, so keep your existing XHR-based upload flow. There is no shame in a hybrid:` fetch` for the 95% of JSON calls, XHR for the file-upload progress bar.


## The migration sequence


Run these steps in order. The ordering is the part people get wrong.


1. **Swap imports first.** Point every call site at the new client (` import { api } from "@/lib/api-client"` ). Because the signatures match, this is a find-and-replace, not a rewrite.
2. **Remove Axios, then refresh the lockfile.** Delete` axios` from` package.json` and run` npm install` (or your package manager’s equivalent) to update the lockfile. Do this *after* the import swap — reverse the order and your build breaks mid-migration with unresolved imports. As of June 2026 the[npm registry](https://www.npmjs.com/package/axios) lists Axios at version 1.18.1; if you’re pinning a safe version elsewhere rather than removing it, check the registry for the current release.
3. **Validate by building, not by reading.** Run your test suite, a TypeScript typecheck (` tsc --noEmit` ), and a production build. **A migration is finished when the app builds, type-checks, and behaves — not when the diff looks clean.**


That last point is where the silent regressions hide. The most dangerous Axios→` fetch` regression is the unhandled error response: a request that returned a 500 but, because` fetch` doesn’t throw, flowed through as “success” and left the user staring at an empty or broken UI with no error shown. Unit tests mock the happy path and pass; the server log shows the 500, so the backend looks fine. The gap between “server logged an error” and “user saw nothing” is exactly what session replay with network capture surfaces — it ties the captured error response to the actual broken UI the user experienced, catching the regression that your tests and server logs each individually miss. The` !res.ok` check in the client above is what prevents this class of bug in the first place; verifying it fires in production is what confirms the migration is actually done.


A` fetch` -based client gives you a smaller dependency tree, full control over error and auth behavior, and one less third-party account in your supply chain — without rewriting a single component, provided you keep the API surface identical. The next concrete step: route your app’s requests through a single module if they aren’t already, then drop in the client above and let your build, types, and tests tell you when you’re finished.


## FAQs


Does fetch throw an error on a 404 or 500 response?


No. fetch only rejects its promise on a network failure or a request that never completes; an HTTP 404 or 500 resolves successfully with a Response object whose ok property is false. This means a server error flows through your try block as if it were a success unless you explicitly check response.ok and throw yourself. Axios behaves differently and auto-rejects any non-2xx status into the catch path.


Do I still need Axios for making requests in Node.js?


No. fetch is now a stable global in Node.js since version 21, released in October 2023, and was available as an experimental global enabled by default since Node.js 18 in April 2022. The older advice to keep Axios specifically for server-side requests no longer holds. You can use the same fetch-based client in browser and Node code, though older Node versions before 18 still require a polyfill or library.


How do I set a request timeout with fetch?


Pass AbortSignal.timeout(milliseconds) as the signal option, for example signal: AbortSignal.timeout(8000). It returns a signal that automatically aborts after the specified time, rejecting the fetch with a TimeoutError DOMException. It has been Baseline across browsers since April 2024. In Chromium-based browsers the rejection can surface as an AbortError instead of a TimeoutError, so catch both names defensively when distinguishing a timeout from a manual cancellation.


Will switching from Axios to fetch break my React Query or SWR setup?


No, as long as your new fetch client returns the same promises and exposes the same method signatures. React Query, SWR, and your mutations only care that a call resolves to data or rejects with an error, not which library produced it. Keep your get, post, patch, and delete signatures identical and throw the same error type with the same fields your UI branches on, and the data-fetching layer never knows Axios is gone.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
