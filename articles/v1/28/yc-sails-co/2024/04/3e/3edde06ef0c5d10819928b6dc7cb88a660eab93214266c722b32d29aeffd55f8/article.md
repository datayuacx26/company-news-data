---
schema_version: "1.0.0"
document_id: "3edde06ef0c5d10819928b6dc7cb88a660eab93214266c722b32d29aeffd55f8"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/boring-stack-v-0-2-2/"
published_at: "2024-04-10T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:2eea2c265f58027d3777d8dfb12c17a1a31cab49fc9eab0e8d63684c772e9cca"
---

# Boring Stack 0.2.2

In this Boring Stack release, the` create-sails-generator` simplifies creating pages for your SPAs along with their actions.


Previously, to add a new page to your SPA, you had to manually create the component file in` assets/js/pages/` and then create the corresponding action to serve the page in` api/controllers/` .


Not only that, you’d have to change the` responseType` of your` success` exit signal to` inertia` and then return the page within the return object. And of course, you’d have to specify the route in` config/routes.js` .


Sounds like a lot, right? Oh yeah, that’s because it is!


With` create-sails-generator` , all of the above processes are now streamlined into a single` sails generate page <page-name>` command.


The command will detect the UI framework you are using (Vue, React, Svelte) and generate the corresponding component file accordingly.


It will also generate the accompanying action to serve the page, leaving you with the only task of specifying the route in` config/routes.js` .


## Simplified Custom Responses Update


Since transitioning to[custom responses in Boring Stack v0.2.0](https://blog.sailscasts.com/boring-stack-v-0-2-0) , one thing I’ve been thinking about is how to evolve the responses without requiring users to copy and paste them when updates are made.


To simplify managing evolving custom responses in The Boring Stack,` create-sails-generator` now provides generators for copying custom responses with generator commands. Check them out below:


### inertia custom response


```text
sails   generate   inertia
```


### inertia-redirect custom response


```text
sails   generate   inertia-redirect
```


### bad-request custom response


```text
sails   generate   bad-request
```


See the[full changelog](https://github.com/sailscastshq/boring-stack/releases/tag/v0.2.2) on GitHub.


## ✅ Upgrading


To upgrade to 0.2.2, do the following:


### Install the latest version of create-sails-geneator as a dev dependency


```text
npm   i   create-sails-generator@latest   -D
```


### Update .sailsrc


Update your` .sailsrc` generator config with the below config in` "modules" : {}` :


```text
"modules"  : {
"page"  :   "create-sails-generator/generators/page"  ,
"inertia"  :   "create-sails-generator/generators/inertia"  ,
"inertia-redirect"  :   "create-sails-generator/generators/inertia-redirect"  ,
"bad-request"  :   "create-sails-generator/generators/bad-request"
}
```


You can now create pages by running` sails generate page example/page` .


> Be sure to replace` example/page` with the exact name of the page you want to create


P.S: If you haven’t[starred the project](https://sailscasts.com/boring) yet, please I’d appreciate if you give it a star ⭐️ on[GitHub](https://sailscasts.com/boring) .
