---
schema_version: "1.0.0"
document_id: "2a88a8b8f507c26d0e304100c57b393cd41618401b2e594da91173c7ebd28fbe"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/boring-stack-v-0-2-0/"
published_at: "2024-02-29T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:f4210792fc8cb58dffdd291676cb28d172b121eccb376bc2bf9a7f659f474d5a"
---

# Boring Stack 0.2.0

In this Boring Stack release, Inertia responses and redirects are now handled by custom reponses and actions2 exits.


By deprecating` sails.inertia.render` and` sails.inertia.location` in favor of having the inertia response and redirect in` api/responses` , this relealse also solves[this issue](https://github.com/sailscastshq/boring-stack/issues/78) .


## Custom responses


I have wanted for a long time to be able to send back Inertia responses like this:


```text
module  .  exports   =   {
friendlyName:   'Home'  ,


description:   'Home index.'  ,


inputs: {},


exits: {
success: {
responseType:   'inertia'
}
},


fn  :   async   function   () {
return   { page:   'index'   }
}
}
```


But my first attempt at it wasn’t successful, so the obvious choice was the non-Sails way of` sails.inertia.render` .


Since this method was implemented in the Inertia middleware of` sails-inertia` , it wasn’t elegant and it also caused an edge case bug which crashes the Sails server when two or more clients are making unathenticated requests at the same time 😱.


### ❌ sails.inertia.render has been deprecated


The logic of serving an Inertia response which was handled by` sails.inertia.render` previously has now been replaced by a custom response in` api/responses/inertia.js`


### ❌ sails.inertia.location has been deprecated


The logic of handling an Inertia redirect which was handled by` sails.inertia.location` previously has now been replaced by a custom response in` api/responses/inertiaRedirect.js`


## inertia-sails 0.2.0


` inertia-sails` 0.2.0 deprecated and removed` sails.inertia.location` and` sails.inertia.render` And as stated above, those two methods are replaced by the following custom responses:


- ` inertia` - Handles Inertia requests
- ` inertiaRedirect` - Handles Inertia redirects


The middleware is now lightweight as it just set the shared props, validation errors, and flash messages.


See the[full changelog](https://github.com/sailscastshq/boring-stack/releases/tag/v0.2.0) on GitHub.


## ✅ Upgrading


To upgrade to 0.2.0, do the following


### Install the latest version of inertia-sails


```text
npm   i   inertia-sails@latest
```


### Copy inertia and inertiaRedirect responses


Head over to mellow-vue template and copy the[inertia](https://github.com/sailscastshq/boring-stack/blob/main/templates/mellow-vue/api/responses/inertia.js) and[inertiaRedirect](https://github.com/sailscastshq/boring-stack/blob/main/templates/mellow-vue/api/responses/inertiaRedirect.js) custom responses into your` api/responses/` folder.


### Replace calls to render() with custom responses


So if you have an action like this:


```text
module  .  exports   =   {
friendlyName:   'Home'  ,


description:   'Home index.'  ,


inputs: {},


exits: {
success: {}
},


fn  :   async   function   () {
return   sails.inertia.  render  (  'index'  , { bestFramework:   'Sails'  })
}
}
```


You will update it to this:


```text
module  .  exports   =   {
friendlyName:   'Home'  ,


description:   'Home index.'  ,


inputs: {},


exits: {
success: {
responseType:   'inertia'
}
},


fn  :   async   function   () {
return   { page:   'index'  , props: { bestFramework:   'Sails'  } }
}
}
```


> Do note the` responseType: 'inertia'`


And that’s it! I am confident that with this release and the earlier announced 0.1.3 release, The Boring JavaScript is closer to checking all the boxes of being the best way to build amazing web software with JavaScript across the stack.


P.S: If you haven’t[starred the project](https://sailscasts.com/boring) yet, please I’d appreciate if you give it a star ⭐️ on[GitHub](https://sailscasts.com/boring) .
