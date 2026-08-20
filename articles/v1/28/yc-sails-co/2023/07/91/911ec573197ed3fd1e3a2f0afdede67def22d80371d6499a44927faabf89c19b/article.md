---
schema_version: "1.0.0"
document_id: "911ec573197ed3fd1e3a2f0afdede67def22d80371d6499a44927faabf89c19b"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/sails-v1-5-5/"
published_at: "2023-07-07T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:1fd6dd464961154dfde383853db04ad8c4212b978d42ac7e3582e092055857c8"
---

# Sails 1.5.5

This release of Sails merged a[PR](https://github.com/balderdashy/sails/pull/7282) that fixed the broken custom inspect implementation on` sails.helpers` .


Prior to this release, the custom inspect which makes the output of` sails.helpers` pretty on the console, was broken due to the breaking change on how Node let’s you implement[custom inspect on objects](https://nodejs.org/api/util.html#utilinspectcustom) .


[Here](https://nodejs.org/dist/latest-v6.x/docs/api/util.html#util_custom_inspection_functions_on_objects) is how Node previously let you do custom inspect on objects.


With **Sails 1.5.5** , this has been fixed.


## 🤖 machine 15.2.3


The` 1.5.5` release of Sails also includes the[fix](https://github.com/node-machine/machine/releases/tag/v15.2.3) on[machine](https://www.npmjs.com/package/machine)` 15.2.3` which is a core dependency of Sails and also had this custom inspect broken for example when inspecting helpers` sails.helpers.fs`


You can check out the[What’s new in Sails 1.5.5](https://youtu.be/JRvFV46J7t8) screencast on YouTube.


Also see the[full changelog](https://github.com/balderdashy/sails/releases/tag/v1.5.5) on GitHub.


## ✅ Upgrading


To upgrade to **Sails 1.5.5** , run:


```text
npm   i   sails@latest
```
