---
schema_version: "1.0.0"
document_id: "46dd86f458e536a26eb1aef3353a930ccea4a20248d09eb9e707fd74ee4bbd61"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/boring-stack-v-0-3-0/"
published_at: "2024-06-06T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:12424a49590b492782b66239e3d76cbb9b2319943b074fed42094423c0753a69"
---

# Boring Stack 0.3.0

In this Boring Stack release, the starter templates have all been upgraded to[Shipwright 0.2.0](https://github.com/sailshq/sails-hook-shipwright/releases/tag/v0.2.0) , which brings some performance gains and better DX to TBJS in terms of asset handling.


## Asset versioning


Before version 0.3.0, you had to manually update the version configuration in` config/inertia.js` every time you updated your app to[burst the browser cache when you deployed](https://inertiajs.com/asset-versioning) .


This was because there was no way to dynamically inject the script and style tags to leverage the Rsbuild asset filename hash in production builds.


That means, you no longer have the concern of versioning your asset manually as this will be done automatically for you going forward.


## Chunk splitting for better performance


Since we weren’t relying on Rsbuild to generate the asset filenames, we also could not rely on the amazing[chunk splitting strategies](https://rsbuild.dev/guide/optimization/split-chunk) Rsbuild provides out of the box.


With this release, we now rely on the['split-by-experience'](https://rsbuild.dev/guide/optimization/split-chunk#split-by-experience) chunk splitting strategy provided by Rsbuild.


See the[full changelog](https://github.com/sailscastshq/boring-stack/releases/tag/v0.3.0) on GitHub.


## ✅ Upgrading


To upgrade to` 0.3.0` , do the following:


### Install the latest version of Shipwright


```text
npm   i   sails-hook-shipwright@latest   -D
```


### Update` app.ejs`


Replace the script and link tags for your` app.js` and` app.css` respectively with the new` shipwright.scripts()` and` shipwright.styles()` methods, which will inject the scripts and style tags respectively so the file looks like this:


```text
<!  DOCTYPE   html  >
<  html   lang  =  "en"  >
<  head  >
<  meta   charset  =  "UTF-8"   />
<  meta   http-equiv  =  "X-UA-Compatible"   content  =  "IE=edge"   />
<  meta   name  =  "viewport"   content  =  "width=device-width, initial-scale=1.0"   />
<  %- shipwright.styles() %>
</  head  >
<  body  >
<  div   id  =  "app"   data-page  =  "  <  %= JSON.stringify(page) %>"  ></  div  >
<  %- shipwright.scripts() %>
</  body  >
</  html  >
```


P.S: If you haven’t[starred the project](https://sailscasts.com/boring) yet, please give it a star ⭐️ on[GitHub](https://sailscasts.com/boring) .
