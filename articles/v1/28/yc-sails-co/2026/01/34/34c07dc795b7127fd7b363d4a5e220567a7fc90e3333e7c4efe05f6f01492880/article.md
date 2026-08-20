---
schema_version: "1.0.0"
document_id: "34c07dc795b7127fd7b363d4a5e220567a7fc90e3333e7c4efe05f6f01492880"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/boring-stack-v-1-0-0/"
published_at: "2026-01-15T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T22:23:37.214552+00:00"
content_hash: "sha256:135112ad02eb0bd0cd0bea45376bfab857905d699b32b716a45f6978c7d52c48"
---

# The Boring Stack 1.0.0

After about 3 years of development and real-world testing, I’m thrilled to announce **The Boring JavaScript Stack 1.0** - the most significant release since the project began.


This release represents a fundamental architectural shift in how inertia-sails handles request-scoped state, plus a wealth of new features that bring us to feature parity with inertia-laravel.


## The Big Picture


The Boring Stack 1.0 focuses on three pillars:


1. **Request Isolation** - Bulletproof handling of concurrent requests
2. **Intelligent Caching** - Reduce database queries with` once()` props
3. **Developer Experience** - New APIs that make common patterns trivial


Let’s dive into what’s new.


## AsyncLocalStorage: The Foundation


The most important change in 1.0 is invisible to most developers, but it fixes a critical bug that could cause **user data to leak between concurrent requests** .


### The Problem


In previous versions,` sails.inertia.share()` stored data in a global object:


```text
// OLD (buggy) - Data shared across ALL requests
sails.inertia.sharedProps   =   {}


share  (key, value) {
sails.inertia.sharedProps[key]   =   value   // Race condition!
}
```


Imagine this scenario:


1. User A requests` /dashboard` (slow database query)
2. User B requests` /dashboard` (fast query)
3. User B’s` loggedInUser` gets stored in global state
4. User A’s response completes… **with User B’s data!**


This is a classic race condition, and it’s terrifying.


### The Solution


inertia-sails 1.0 uses Node.js’s` AsyncLocalStorage` to create **isolated request contexts** :


```text
// NEW (safe) - Each request has its own isolated state
const   {   AsyncLocalStorage   }   =   require  (  'node:async_hooks'  )
const   requestContext   =   new   AsyncLocalStorage  ()


// Every request runs in its own context
requestContext.  run  ({ sharedProps: {} }, ()   =>   {
// share() now stores in request-specific context
share  (key, value) {
const   context   =   requestContext.  getStore  ()
context.sharedProps[key]   =   value   // Isolated!
}
})
```


This change affects all request-scoped APIs:


API Now Request-Scoped


` share()` ✅


` viewData()` ✅


` flash()` ✅


` encryptHistory()` ✅


` clearHistory()` ✅


` setRootView()` ✅


` refreshOnce()` ✅


For truly global data (app name, version), use the new` shareGlobally()` :


```text
// In hook initialization (runs once at startup)
sails.inertia.  shareGlobally  (  'appName'  ,   'My App'  )
```


---


## Once Props: Intelligent Caching


One of the most requested features,` once()` props let you cache expensive data across page navigations. The client tracks what it has, and the server skips re-sending it.


### Before: 50 Page Views = 50 Database Queries


```text
// Every navigation hits the database
sails.inertia.  share  (  'loggedInUser'  ,   async   ()   =>   {
return   await   User.  findOne  ({ id: req.session.userId })
})
```


### After: 50 Page Views = 1 Database Query


```text
// Client caches the result, server skips it on subsequent requests
sails.inertia.  share  (  'loggedInUser'  ,
sails.inertia.  once  (  async   ()   =>   {
return   await   User.  findOne  ({ id: req.session.userId })
})
)
```


### Chainable Options


```text
sails.inertia.  once  (()   =>   fetchPermissions  ())
.  as  (  'user-permissions'  )    // Custom cache key
.  until  (  3600  )               // Expire after 1 hour
.  fresh  (needsRefresh)       // Conditionally force refresh
```


### Refreshing Cached Data


When the user updates their profile, you need to refresh the cached data. Use` refreshOnce()` :


```text
// api/controllers/user/update-profile.js
module  .  exports   =   {
fn  :   async   function  ({   fullName   }) {
await   User.  updateOne  ({ id: userId }).  set  ({ fullName })


// Force the client to get fresh data
sails.inertia.  refreshOnce  (  'loggedInUser'  )
sails.inertia.  flash  (  'success'  ,   'Profile updated!'  )


return   '/profile'
}
}
```


---


## Flash Messages Done Right


Flash messages now use proper session storage and **don’t persist in browser history** . No more “ghost” toast notifications when users click back.


```text
// In your action
sails.inertia.  flash  (  'success'  ,   'Invoice sent!'  )
sails.inertia.  flash  (  'error'  ,   'Payment failed'  )


// Multiple at once
sails.inertia.  flash  ({
success:   'Saved!'  ,
highlight:   'billing-section'
})
```


Access in your frontend via` page.props.flash` .


---


## Infinite Scroll with scroll()


Building infinite scroll lists is now trivial with the` scroll()` helper:


```text
const   page   =   this  .req.  param  (  'page'  ,   0  )
const   perPage   =   20
const   invoices   =   await   Invoice.  find  ().  paginate  (page, perPage)
const   total   =   await   Invoice.  count  ()


return   {
page:   'invoices/index'  ,
props: {
invoices: sails.inertia.  scroll  (()   =>   invoices, {
page,
perPage,
total
})
}
}
```


The helper:


- Wraps data with pagination metadata
- Automatically merges with existing client data
- Supports` append()` and` prepend()` strategies
- Works seamlessly with Waterline’s` .paginate()`


---


## Deep Merge for Nested Objects


The existing` merge()` does shallow merging. For nested objects, use` deepMerge()` :


```text
// Shallow merge - replaces nested objects entirely
settings  : sails.inertia.  merge  (()   =>   updatedSettings)


// Deep merge - recursively merges nested objects
settings  : sails.inertia.  deepMerge  (()   =>   updatedSettings)
```


---


## Per-Request Root Views


Different layouts for different pages? Use` setRootView()` :


```text
// In a policy for auth pages
module  .  exports   =   async   function  (  req  ,   res  ,   proceed  ) {
sails.inertia.  setRootView  (  'auth'  )    // Use views/auth.ejs
return   proceed  ()
}
```


## Automatic Asset Versioning


No more manual version bumps! inertia-sails now automatically handles asset versioning:


**With Shipwright:** Reads` .tmp/public/manifest.json` and generates an MD5 hash. When your assets change, the version changes automatically.


**Without Shipwright:** Falls back to the server startup timestamp, ensuring fresh assets on each restart.


```text
// config/inertia.js
module  .  exports  .inertia   =   {
rootView:   'app'
// version is auto-detected - no configuration needed!
}
```


You can still override with a custom version if needed:


```text
module  .  exports  .inertia   =   {
version  : ()   =>   require  (  './package.json'  ).version
}
```


## Error Handling


In development, server errors now display in a **styled modal** instead of breaking your app. In production, errors redirect back with a flash message.


Just add` api/responses/serverError.js` to your app (included in all templates).


## Breaking Changes


### share() is Now Request-Scoped


If you were relying on` share()` persisting across requests, use` shareGlobally()` instead.


### Flash Messages API


Replace` this.req.flash()` with` sails.inertia.flash()` .


### Back Navigation


Replace` return 'back'` with` return sails.inertia.back('/fallback')` .


## Upgrading to 1.0


### 1. Update inertia-sails


```text
npm   install   inertia-sails@latest
```


### 2. Update Custom Hook


Wrap your shared user data with` once()` :


```text
// Before
sails.inertia.  share  (  'loggedInUser'  , user)


// After
sails.inertia.  share  (  'loggedInUser'  ,
sails.inertia.  once  (  async   ()   =>   {
return   await   User.  findOne  ({ id: req.session.userId })
})
)
```


### 3. Add refreshOnce() to Update Actions


```text
// In update-profile.js
sails.inertia.  refreshOnce  (  'loggedInUser'  )
sails.inertia.  flash  (  'success'  ,   'Profile updated!'  )
```


### 4. Copy Updated Responses


Copy the latest` serverError.js` from the templates to your` api/responses/` folder.


## What’s Next?


The Boring Stack is now at feature parity with inertia-laravel for the features that matter most. Here’s what’s on the horizon:


- **Route helpers** - Simple Inertia pages without actions
- **Advanced MergeProp options** -` matchOn()` ,` append()` ,` prepend()`
- **Even more boring** - Because boring means reliable


## Thank You


This release wouldn’t be possible without the community. Special thanks to everyone who reported issues, suggested features, and tested early builds.


If you haven’t[starred the project](https://sailscasts.com/boring) yet, please give it a star on[GitHub](https://github.com/sailscastshq/boring-stack) . It helps more developers discover The Boring Stack.


Here’s to building amazing web software with JavaScript across the stack - the boring way.


**Full Changelog:**[v1.0.0 on GitHub](https://github.com/sailscastshq/boring-stack/releases/tag/v1.0.0)


**Documentation:**[docs.sailscasts.com/boring-stack](https://docs.sailscasts.com/boring-stack)
