---
schema_version: "1.0.0"
document_id: "4231f6b5ab3dfc0bb25fed0c027ee990df3e91fa492d9624c9f0eaadca8d7646"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/boring-stack-v-0-5-0/"
published_at: "2025-02-12T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:2238e739a84db1987a0c97b815fa934e016e288917414df128f3800be0c455d5"
---

# Boring Stack 0.5.0

In this Boring Stack release, the` inertia-sails` adapter for Sails has been enhanced to fully support the features of Inertia 2.0, including[deferred props](https://docs.sailscasts.com/boring-stack/deferred-props) and[merging props](https://docs.sailscasts.com/boring-stack/merging-props) .


The mellow templates now come with comprehensive support for Inertia 2.0, and both mellow-react and mellow-svelte have been updated to support React 19 and Svelte 5, respectively.


## Simplified Custom Responses with inertia-sails 0.3.0


In inertia-sails 0.2.0, we deprecated` sails.inertia.render()` and` sails.inertia.location()` in favour of using` inertia` and` inertiaRedirect` custom responses.


However, in version 0.3.0, these methods have been reintroduced to simplify the` inertia` and` inertiaRedirect` custom responses.


Previously, the` inertia` and` inertiaRedirect` responses exposed some internal workings of` inertia-sails` . With inertia-sails 0.3.0, these internals have been encapsulated within` sails.inertia.render()` and` sails.inertia.location()` . Now, both custom responses simply call these methods.


> You only need to use these methods within the` inertia` and` inertiaRedirect` custom responses, and nowhere else in your application.


Here is how your` inertia` and` inertiaRedirect` custom responses will look like after upgrading to TBJS 0.5.0:


```text
// api/responses/inertia.js
module  .  exports   =   function   inertia  (  data  ) {
return   this  .req._sails.inertia.  render  (  this  .req,   this  .res, data)
}
```


```text
// api/responses/inertiaRedirect.js
module  .  exports   =   function   inertiaRedirect  (  url  ) {
return   this  .req._sails.inertia.  location  (  this  .req,   this  .res, url)
}


```


While we’re on the topic of simplifying custom responses, we’ve also introduced` sails.inertia.handleBadRequest()` . This method streamlines the implementation of` api/responses/badRequest.js` to the following:


```text
// api/responses/badRequest.js
module  .  exports   =   function   badRequest  (  optionalData  ) {
return   this  .req._sails.inertia.  handleBadRequest  (
this  .req,
this  .res,
optionalData
);
}
```


## Fixed Bug with Inertia Redirects


In this release, we addressed an issue where redirecting to a non-Inertia page would incorrectly open the page in a modal. This fix ensures that redirects to non-Inertia pages now function as expected, providing a smoother user experience.


## ✅ Upgrading to Boring Stack 0.5.0


To upgrade to version 0.5.0, follow these steps:


### 1. Update Dependencies


First, install the latest versions of` inertia-sails` and` create-sails-generator` :


```text
npm   install   inertia-sails@latest
```


```text
npm   install   create-sails-generator@latest   -D
```


Next, update your client-side adapter to the latest version:


#### Vue


```text
npm   install   @inertiajs/vue3@latest
```


#### React


```text
npm   install   @inertiajs/react@latest
```


#### Svelte


```text
npm   install   @inertiajs/svelte@latest
```


### 2. Update Svelte Setup


If you are using Svelte, update your` assets/js/app.js` to the latest setup for Svelte 5:


```text
import   { createInertiaApp }   from   '@inertiajs/svelte'  ;
import   { mount }   from   'svelte'  ;
import   '~/css/main.css'  ;


createInertiaApp  ({
resolve  : (  name  )   =>   require  (  `./pages/${  name  }`  ),
setup  ({   el  ,   App  ,   props   }) {
mount  (App, { target: el, props });
},
progress: {
color:   '#6C25C1'
}
});
```


> Note: We are now using the` mount` method from Svelte.


### 3. Generate Custom Responses


Run the following commands to generate the new versions of the custom responses:


```text
# inertia
npx   sails   generate   inertia   --force


# inertiaRedirect
npx   sails   generate   inertia-redirect   --force


# badRequest
npx   sails   generate   bad-request   --force
```


And that’s it! With this release and improvements, The Boring JavaScript Stack is steadily advancing towards becoming the delightful choice for building exceptional full-stack web applications with JavaScript across the stack.


JavaScript developers must ship!


P.S: If you haven’t already, please consider[starring the project](https://sailscasts.com/boring) on[GitHub](https://sailscasts.com/boring) . Your support is greatly appreciated! ⭐️
