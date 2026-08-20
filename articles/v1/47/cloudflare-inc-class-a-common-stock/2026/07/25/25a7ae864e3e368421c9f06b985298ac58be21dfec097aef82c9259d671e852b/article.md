---
schema_version: "1.0.0"
document_id: "25a7ae864e3e368421c9f06b985298ac58be21dfec097aef82c9259d671e852b"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/introducing-cache-response-rules/"
published_at: "2026-07-23T18:40:38+00:00"
first_seen_at: "2026-07-23T19:18:56.810328+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:4f30bfa6912efd32af2614225e7d231cc68c9a6208b85f380573d57d982be1f5"
---

# Introducing Cache Response Rules

**Today we’re excited to announce Cache Response Rules.** These are a new rule type that runs after an origin server replies but before Cloudflare caches the content.


If you've ever been irked watching something that should easily sail out of cache get dragged back to the origin by a stray` Set-Cookie` or wrong` Cache-Control` , headers that are sometimes hard or impossible to strip or change on the origin itself, then Cache Response Rules is that fix, applied at exactly the right moment.


## When and how caching decisions are made


A CDN cache and an origin server work as a pair. Their goal is to answer from the cache whenever possible and only go back to the origin when the edge can’t respond. Every point of cache hit ratio comes from getting that division of labor right. Check the cache when we shouldn't, and we waste a lookup that was always going to miss. Check it too rarely, the origin serves traffic the edge should have absorbed, and the performance win evaporates.


Importantly, the origin guides the cache. When it returns a cacheable asset, its response headers tell Cloudflare how long it’s OK to serve it, when and how to revalidate, and even whether to cache it at all. The cache is only ever as efficient as the origin allows. If the origin gets it wrong, cache becomes decoration while the origin infrastructure costs skyrocket.


Most cache eligibility problems are not decided at request time. They manifest after the origin replies.


A visitor asks for` /static/app.js` . Cloudflare checks cache, it misses, and forwards the request to the origin. The origin returns the file. Somewhere in those response headers, quietly, is a` Set-Cookie` header. The asset that should have been cached at every Cloudflare data center is now uncacheable. Multiply that by every visitor, on every site, with the same accidental header, and you have a cache hit ratio that is leaking origin bandwidth, ruining performance, and driving infrastructure costs higher.


There is a long list of variations of this same problem. The origin sends` Cache-Control: no-cache` on assets that are perfectly safe to cache on the CDN. The origin sends correct directives, but they're meant for the browser, not for Cloudflare. Or the origin attaches an` ETag` (an identifier for a specific version of a resource) that's overly aggressive and causes revalidation thrash on every conditional request. Often, especially in large teams, those managing the origin’s responses and the group that manages the CDN are different. This makes changing a one-line header a weeks-long negotiation.


None of these problems can be solved at request time. By the time Cloudflare sees the` Set-Cookie` on` /app.js` , the request phase is over. The response is already in flight.


So we put a fix in the right place.


**Cache Response Rules run after the origin’s response arrives at Cloudflare, but before it gets written to cache.** With them, you can rewrite` Cache-Control` directives, manage cache-tags, and strip headers like` Set-Cookie` ,` ETag` , and` Last-Modified` from the origin response before Cloudflare's cache ever sees them. The fix lives entirely on Cloudflare. No origin code changes required.


## The missing piece


If you've used Cloudflare for any length of time, you've watched the cache control surface area evolve. A few years ago, most of this lived inside[Page Rules](https://developers.cloudflare.com/rules/page-rules/) , which operated as a single, albeit overloaded, primitive that mixed caching with redirects, security, and a dozen other behaviors, all evaluated on requests. We later[split these rules apart](https://blog.cloudflare.com/future-of-page-rules/) so that only relevant behavior changes would be evaluated against requests, reducing unnecessary latency and allowing for complex rule stacking between behaviors.[Cache Rules](https://developers.cloudflare.com/cache/how-to/cache-rules/) became a dedicated, expressive rule type for caching decisions, joined by[CDN-Cache-Control](https://developers.cloudflare.com/cache/concepts/cdn-cache-control/) ,[Origin Cache Control](https://developers.cloudflare.com/cache/concepts/cache-control/) ,[custom cache keys](https://developers.cloudflare.com/cache/how-to/cache-rules/examples/custom-cache-key/) , and other controls that give you precise ways to tell Cloudflare what's safe to cache, for how long, and under what conditions.


Many of these controls share a common trait: they operate on the *request* .


While maybe unintuitive, this makes sense. The most important caching decisions Cloudflare has to make are *should we look this up in cache, and under what key?* This question has to be answered *before* Cloudflare talks to the origin. If the answer is "no" when it should have been "yes," the request has already paid for the origin round trip and there's nothing the response can do to give it back that latency. Request-time rules answer that question using the only information available at that moment: the URL, the requested file’s extension, the request headers, geography, device type, and so on. Using these request parameters and the rules set to change these request parameters, we determine if something is likely in cache and look there before talking with the origin.


But some cache decisions cannot be made on the request. The origin's` Cache-Control` directives (cache-control: max-age=3600) are part of the response Cloudflare receives from the origin. The status code, the ETag, the Last-Modified timestamp, Set-Cookie, and the cache-tag format the origin chooses are all things in the response that the origin passes to the cache. None of it is available when request-time rules run.


Responses from the origin are the source of truth, so if something needed to be changed on Cloudflare that wasn’t available at request time, that previously left you with three workarounds:


1. Change the origin.
2. Write a Worker that re-fetches and rewrites the response.
3. Live with a worse hit ratio.


Each of those costs engineering time, adds latency, or burns money. **Cache Response Rules give you a fourth option: a ruleset where you can modify the origin's response before it hits Cloudflare's cache.**


## Two phases, two questions


The cleanest way to think about the difference between[Cache Rules](https://developers.cloudflare.com/cache/how-to/cache-rules/) and the new[Cache Response Rules](https://developers.cloudflare.com/cache/how-to/cache-response-rules/) is as two phases, each answering its own question.


- **Cache Rules** **run in the request phase,** before Cloudflare talks to the origin server. They answer: *given this request, should Cloudflare cache the response, and under what cache key?* That’s three decisions that can be made at request time with cache rules: **whether** to cache (eligible vs. bypass), **what** the object is (the cache key and how to identify the stored object in the future), and **how** to cache (edge TTL, browser TTL, serve-stale, etc). All of these questions must be settled before the origin fetch, using only what’s in the request.


- **Cache Response Rules run in the response phase,** after the origin replies but before the response is written to Cloudflare's cache. Cache Response Rules answer: *now that the origin has responded, should we adjust how we cache it?* Cache Response Rules can rewrite the **how** from the request by ** stripping headers that would make a response ineligible for cache, ** changing origin` cache-control` directives that tell Cloudflare whether and how to cache *, or* setting cache tags for purging content. When a cache rule and cache response rule are in conflict with each other, the cache response rule wins.


The response phase can't do everything the request phase can. It can't change the **what,** as the key is already fixed. Though it can change **whether** and **how** Cloudflare caches, for example, a response rule can set` no-store` to make a cacheable object non-cacheable or strip a` Set-Cookie` to make non-cacheable content eligible for cache. However, Cache Response Rules can't make a previously ineligible request cacheable by the response phase, because it’s too late.


So Cache Response Rules don't replace Cache Rules. Cache Rules decide **whether** , **what** , and **how** we cache on the request. Cache Response Rules get the final word on the **how** and **whether** , once the origin's response is seen by Cloudflare in a phase that didn't exist before.


## What you can do


Cache Response Rules support three actions today.


1. **Strip headers that break caching**


The` set_cache_settings` action removes things like` Set-Cookie, ETag` , or` Last-Modified` from the origin response before Cloudflare evaluates it for caching:


```text
"action_parameters": {
"strip_etags": true,
"strip_set_cookie": true,
"strip_last_modified": true
}
```


This is the fix for the` Set-Cookie-on-a-static-asset` problem. Origin frameworks frequently attach session cookies to every response (for load balancing or other purposes), including responses for assets that users don't want to be associated with a session. Stripping` Set-Cookie` in the response phase makes those assets cacheable again without asking the origin, load balancer, or anything else upstream to change anything.


Cache Response Rules also run on responses that aren't eligible for caching at all. So when you strip Set-Cookie from a dynamic response, for instance, the rule fires whether or not the object is ever stored. This allows you to control what the client sees even when the response is not stored in cache. You can also strip ETag and` Last-Modified` , which is useful when those headers are misconfigured at the origin and causing thrash. This comes with a tradeoff: stripping both ETag and Last-Modified enables[Smart Edge Revalidation](https://blog.cloudflare.com/introducing-smart-edge-revalidation/) for that response. If Cache Response Rules then *add* new validators, Cloudflare will not enable a Smart Edge Revalidation for browser conditional requests.


So stripping and modifying headers, even on dynamic requests, are a powerful new pattern that did not exist before on Cloudflare, but there can be additional configs to be aware of if you make these changes.


1. **Manage cache tags**


` set_cache_tags` lets you` add` ,` remove` , or` set` cache tags used for[purge by tag](https://developers.cloudflare.com/cache/how-to/purge-cache/purge-by-tags/) on the response. Tags can be static:


```text
"action_parameters": {
"operation": "set",
"values": ["product-catalog", "storefront"]
}
```


Or computed from a response header expression:


```text
"action_parameters": {
"operation": "add",
"expression": "split(http.response.headers[\"Surrogate-Keys\"][0], \",\", 64)"
}
```


That second form is the one that earns its keep during a CDN migration. If your previous CDN attached surrogate keys to responses using a header like` Surrogate-Keys` with a comma delimiter, you can translate them into Cloudflare's` Cache-Tag` format directly in the response phase.


The third argument to split() is the limit: the maximum number of elements in the resulting array can be between 1 and 128. Use a value comfortably larger than the realistic tag count per response. The value 1 would return the entire header as a single tag. Once tags exist on the response,[purge-by-tag](https://blog.cloudflare.com/instant-purge/) just works (and by “works” we mean usually in[under 150 ms, globally](https://blog.cloudflare.com/instant-purge/) ).


1. **Modify Cache-Control directives**


` set_cache_control` is the action that does the most heavy lifting. You can set or remove individual directives:


- Duration directives:` max-age` ,` s-maxage` ,` stale-if-error` ,` stale-while-revalidate`
- Qualified directives:` private` ,` no-cache` (with optional header-name qualifiers)
- Boolean directives:` no-store` ,` no-transform` ,` must-revalidate` ,` proxy-revalidate` ,` must-understand` ,` public` ,` immutable`


For each directive you can also set` cloudflare_only` : true, which is the part that often surprises people:


```text
"action_parameters": {
"s-maxage": {
"operation": "set",
"value": 86400,
"cloudflare_only": true
}
}
```


When` cloudflare_only` is true, the directive affects how Cloudflare caches the response, but the downstream Cache-Control value sent to the browser is left alone. This is the response-phase version of what` CDN-Cache-Control` gives you at the origin: cache the asset for 24 hours at Cloudflare, but tell the browser something else. The difference is that you're now doing it from a Rules surface in the Cloudflare dashboard, rather than asking the origin to set a separate header.


## Examples worth stealing


Below are some examples we thought could help show you the power of Cache Response Rules. Try them out, remix them, and share with others in the community so you can see how to achieve powerful cache customizations using Cache Rules with Cache Response Rules. More examples can be found in the[docs](https://developers.cloudflare.com/cache/how-to/cache-response-rules/create-api/#example-requests) .


### **Strip Set-Cookie from static asset extensions**


```text
Expression:  http.request.uri.path.extension in {"js" "css" "woff2" "woff" "ttf" "png" "jpg" "svg"}
Action:      set_cache_settings
strip_set_cookie: true
```


*Why it works* : The most common cause of "this should be cacheable but isn't" is a session middleware on the origin that attaches a` Set-Cookie` to *every* response. Stripping it for known-static extensions converts those responses to cacheable without any origin change.


*Caveat* : Only do this for asset types where the cookie is not semantically required. If your origin uses cookies to drive variant behavior on those URLs (very unusual but possible), strip selectively or scope stripping these headers by path.


### **Long-cache static assets in Cloudflare, shorter-cache them in the browser**


```text
Expression: http.request.uri.path.extension in {"js" "css" "woff2"}
Action:     set_cache_control
s-maxage: set 2592000 (30 days), cloudflare_only=true
immutable: set
max-age: set 86400  (1 day), cloudflare_only=false
```


*Why it works* : Cloudflare holds the asset for a month and serves it from cache. Browsers see` max-age=86400` and re-validate after a day. You decouple the two cache lifetimes without touching the origin.


*Caveat* :` immutable` tells browsers not to revalidate even on explicit refresh. Pair it only with versioned/hashed filenames.


### **Override no-cache on a known-static path**


```text
Expression: starts_with(http.request.uri.path, "/static/") and http.response.code eq 200
Action:     set_cache_control
no-cache:  remove
s-maxage:  set 3600, cloudflare_only=true
```


*Why it works* : Framework defaults sometimes attach` no-cache` to every response. If you know /static/* is safe to cache, you can strip the directive and impose your own TTL (at Cloudflare only), without changing what the origin or any downstream cache sees.


*Caveat* : Be honest about what's actually static. If` /static/` is sometimes used to serve user-specific content, narrow the match (extension, response header signal, content type).


### **Translate cache tags during a CDN migration**


```text
Expression:  any(http.response.headers.names[*] == "Surrogate-Keys")
Action:     set_cache_tags
operation:  add
expression: split(http.response.headers["Surrogate-Keys"][0], ",", 64)
```


*Why it works* : A lot of CDN migration pain comes from origin tooling that emits cache-tag headers in another vendor's format. Instead of asking the origin team to ship a release that adds Cloudflare's` Cache-Tag` , translate the existing header in the response phase. Purge-by-tag on Cloudflare starts working immediately.


*Caveat* : The third argument to split() is the limit (1–128) on the resulting array size, not anything to do with the separator.


## How to use Cache Response Rules


**Dashboard**


1. Go to **Cache** > **Cache Rules** in the Cloudflare dashboard.
2. Select Create rule and then **Cache Response Rule** .
3. Choose a name and an expression. The expression builder exposes both request fields and response fields.
4. Choose an action: **Modify cache-control directives** , **Modify cache tags** , or **Strip headers** .
5. For directives, toggle **Cloudflare only** when you want the change to apply only to Cloudflare's view of the asset.
6. Save as a draft to iterate, or deploy directly.


### **API**


Rules in this phase live at:


```text
/zones/{zone_id}/rulesets/phases/http_response_cache_settings/entrypoint
```


For further information about how to use Cache Response Rules including API and terraform examples, please see the[documentation](https://developers.cloudflare.com/cache/how-to/cache-response-rules/create-api/) .


## Use Cache Response Rules today


Cache Rules saw the request. Cache Response Rules see the response. Both give you even more control to build the perfect cache on Cloudflare. They are available on all plans today — go[try them](http://dash.cloudflare.com/?to=/:account/:zone/caching/rules/response-rules/new) out!
