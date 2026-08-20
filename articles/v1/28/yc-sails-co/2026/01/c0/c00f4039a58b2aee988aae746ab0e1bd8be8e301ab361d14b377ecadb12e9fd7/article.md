---
schema_version: "1.0.0"
document_id: "c00f4039a58b2aee988aae746ab0e1bd8be8e301ab361d14b377ecadb12e9fd7"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/introducing-shipwright/"
published_at: "2026-01-09T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:54:51.009577+00:00"
content_hash: "sha256:f4eb08558913d06ac2e81efef62db05abd2d2d5bc5f0b45188667e9c808581bf"
---

# Introducing Shipwright - The Modern Asset Pipeline for Sails.js

Over two years ago, I made a quiet commit that would eventually become[Shipwright](https://github.com/sailshq/sails-hook-shipwright) . No fanfare, no announcements. Just the first line of code for something I knew the Sails community needed.


Today, after patient iteration and relentless real-world testing, I’m excited to finally announce **Shipwright** , the modern asset pipeline for Sails.js.


Every feature has been battle-tested on[The Boring JavaScript Stack](https://sailscasts.com/boring) because if it’s not good enough for our own stack, it’s not good enough for the broader Sails.js ecosystem.


First, I want to thank **Mike McNeil** for trusting me with this evolution of the Sails asset pipeline. The Grunt pipeline has served the community well for over a decade, and taking on the responsibility of building its successor has been both humbling and exciting.


## The Numbers


Shipwright replaces Grunt with[Rsbuild](https://rsbuild.dev/) , a blazing-fast Rust-powered bundler. Here’s what that means in practice:


Metric Grunt (legacy) Shipwright


Build speed ~16s ~1.4s


JS bundle size 3.0MB 229KB


CSS bundle size 733KB 551KB


*Benchmarks from the[fleetdm.com](https://fleetdm.com/) migration ([fleetdm/fleet#38079](https://github.com/fleetdm/fleet/issues/38079) )*


That’s an **11x faster build** and **92% smaller JS bundles** , with zero configuration changes.


## Why Rsbuild?


Back in late 2023 when I started exploring modern bundlers, I evaluated both esbuild and Vite.


**Vite** is an excellent tool, but it’s primarily designed around the SPA (Single-Page Application) paradigm. Sails.js apps are classic server-side rendered applications where each route renders a complete HTML page from the server. Vite’s architecture assumes it controls the HTML generation and expects to inject scripts via its dev server. Adapting this to work seamlessly with Sails’ EJS templates, where the server renders views and we just need bundled assets, felt like swimming against the current. The[MPA support in Vite](https://runebook.dev/en/articles/vite/guide/build/multi-page-app) exists but requires manual configuration and doesn’t align naturally with traditional server-rendered frameworks.


**esbuild** is blazing fast and I got a working prototype running. However, I found myself needing plugins for almost everything: LESS/SASS compilation, asset handling, dev server with HMR. Many plugins either didn’t exist or weren’t mature enough, which meant building and maintaining them myself. For a community tool that needs to “just work” out of the box, this wasn’t sustainable.


Then I discovered **Rsbuild** . It’s built on top of[Rspack](https://rspack.dev/) , which was created by ByteDance to solve real bundling problems at massive scale. Their production builds were taking up to 30 minutes and dev server startup could exceed several minutes. Bundling performance wasn’t just nice-to-have for them; it was a[business necessity that impacted engineering productivity](https://rspack.rs/guide/start/introduction) . Rspack now powers over 1,000 applications at ByteDance, including TikTok and Douyin.


What makes Rsbuild perfect for Shipwright is that it’s designed to be batteries-included while Rspack handles the low-level bundling. The ByteDance team explicitly built Rsbuild to provide an[“out-of-the-box” development experience](https://rspack.rs/blog/announcing-1-0) without the configuration complexity. LESS, SASS, TypeScript, HMR, code splitting… it all works without hunting for plugins or writing custom integrations.


## What You Get


- **Hot Module Replacement:** See changes instantly without full page reloads
- **TypeScript support:** Type-safe frontend code without extra configuration
- **ES Modules:** Use` import` /` export` syntax naturally
- **Tree shaking:** Automatic dead code elimination


All while maintaining backward compatibility with existing Sails apps.


## Zero-Config Getting Started


For most apps, Shipwright works out of the box:


```text
npm   install   sails-hook-shipwright   --save
```


Disable Grunt in` .sailsrc` :


```text
{
"hooks"  : {
"grunt"  :   false
}
}
```


Update your layout to use Shipwright helpers:


```text
<  head  >
<  %- shipwright.styles() %>
</  head  >
<  body  >
<!-- your content -->
<  %- shipwright.scripts() %>
</  body  >
```


That’s it! Shipwright auto-detects your entry points (` assets/js/app.js` ,` assets/styles/importer.less` ,` assets/css/app.css` , etc.) and handles the rest.


## TypeScript Just Works


One of the most requested features: TypeScript support with zero configuration. Rename your files to` .ts` or` .tsx` and Shipwright handles the transpilation automatically:


```text
// assets/js/app.ts
import   { setupCloud }   from   './cloud.setup'
import   type   { User }   from   './types'


const   user  :   User   =   await   Cloud.  getCurrentUser  ()
```


## Drop-in Grunt Replacement


Already have a` tasks/pipeline.js` with glob patterns? Shipwright speaks the same language:


```text
// config/shipwright.js
module  .  exports  .shipwright   =   {
js: {
entry: [
'js/cloud.setup.js'  ,
'js/components/**/*.js'  ,
'js/utilities/**/*.js'  ,
'js/pages/**/*.js'
],
inject: [
'dependencies/sails.io.js'  ,
'dependencies/lodash.js'  ,
'dependencies/**/*.js'
]
}
}
```


Files are concatenated in the specified order, preserving the global scope behavior you’re used to. Translate your` pipeline.js` patterns directly and you’re good to go.


## LESS and SASS


Using LESS or SASS? One plugin away:


```text
npm   install   @rsbuild/plugin-less   --save-dev
```


```text
const   {   pluginLess   }   =   require  (  '@rsbuild/plugin-less'  )


module  .  exports  .shipwright   =   {
build: {
plugins: [  pluginLess  ()]
}
}
```


Shipwright auto-detects` importer.less` ,` main.scss` ,` app.css` , and other common entry points.


## Production Ready


In production (` NODE_ENV=production` ), Shipwright automatically:


- Minifies JS and CSS
- Adds content hashes for cache busting (` app.a1b2c3d4.js` )
- Enables tree shaking to eliminate unused code
- Generates a manifest for asset versioning


## Upgrading sails-hook-content


If you’re using` sails-hook-content` for static content generation, you’ll need to upgrade to the latest version:


```text
npm   install   sails-hook-content@latest   --save
```


This is required because Shipwright now exposes its tag generators via` sails.hooks.shipwright.scripts()` and` sails.hooks.shipwright.styles()` instead of the previous method names.


## Get Started


Check out the[full documentation](https://sailsjs.com/documentation/concepts/assets/shipwright) and the[sails-hook-shipwright repository](https://github.com/sailshq/sails-hook-shipwright) .


If you run into issues or have feature requests, please[open an issue](https://github.com/sailshq/sails-hook-shipwright/issues) . We’d love to hear from you!


Happy shipping!
