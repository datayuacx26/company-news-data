---
schema_version: "1.0.0"
document_id: "e2c532b01b7e4cb73a0aefcd2ad579dc2c0393cd756b3a4ee2136489257db86a"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/boring-stack-v-0-1-3/"
published_at: "2024-02-22T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:857e9f6b7d89eb792cda3c715d8badf0bb7372ec243333c728e0ff1a0e7e2b2b"
---

# Boring Stack 0.1.3

In this Boring Stack release, handling server-side validation errors and support for flash messages has been introduced 🚀.


## inertia-sails 0.1.8


I have really been letting this one fallow for a while as the obvious solution would have been a degradation from the benefit Inertia gave. It turned out that what was needed was a key ingredient - good old flash messages.


The implementation leverages[sessions](https://sailsjs.com/documentation/concepts/sessions) and reintroduced flash messages in your Sails applications via[Sails Flash](https://docs.sailscasts.com/sails-flash)


You can[read the validation docs](https://docs.sailscasts.com/boring-stack/validation) for more on handling server side validation errors in The Boring JavaScript Stack.


A good side effect of using flash messages for validation errors is that you can now pass messages between requests via a shared flash object. Check out[the docs on flash messages](https://docs.sailscasts.com/boring-stack/flash-messages) for more info.


## sails-flash 0.0.1


Sails Flash as mentioned earlier implements support for flash messages in Sails applications and it’s now being leveraged by` inertia-sails` to provide validation errors handling support and flash messages as well.


See the[full changelog](https://github.com/sailscastshq/boring-stack/releases/tag/v0.1.3) on GitHub.


You can also check the[login action](https://github.com/sailscastshq/boring-stack/blob/main/templates/mellow-vue/api/controllers/auth/login.js) and[login page](https://github.com/sailscastshq/boring-stack/blob/main/templates/mellow-vue/assets/js/pages/login.vue) on the[mellow-vue](https://github.com/sailscastshq/boring-stack/tree/main/templates/mellow-vue) template to see these changes in action.


## ✅ Upgrading


To start enjoying the DX of flash messages for validation error handling and for sharing messages across requests, upgrade` inertia-sails` and install` sails-flash` in your project:


```text
npm   i   inertia-sails@latest   sails-flash
```
