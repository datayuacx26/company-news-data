---
schema_version: "1.0.0"
document_id: "f976d9e1a40d058d0f612d575ff186df7488319989c4daa3f6c3ee52588ae638"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-rss-bbf1d949eada"
canonical_url: "https://www.tweeks.io/blog/tweeks-vs-tampermonkey"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.378405+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:551ab1a3c7af4134e420f8b93813b8511a3b4138123a9aaffa509710fe1d6b4f"
---

# Tweeks vs. Tampermonkey: Which Userscript Manager Fits You?

Many people find Tweeks without having heard of Tampermonkey, Greasemonkey, or userscript managers at all. Others arrive with the immediate question: "How is this different from Tampermonkey?"


We want to support both users well. If userscript managers are new to you, this article can give you some historical context. If you already know Tampermonkey, it will explain where Tweeks overlaps, where it differs, and why those differences matter.


[Tampermonkey](https://www.tampermonkey.net/) is one of the best-known userscript managers. It helps people install, write, manage, and run JavaScript snippets that change websites. If you already know JavaScript, CSS selectors, userscript metadata, and the Greasemonkey-style` GM_*` APIs, Tampermonkey gives you a direct way to control the pages you use.


Tweeks starts from the same idea - websites should be adjustable by the people using them - but the entry point is different. Instead of asking you to write the script first, Tweeks lets you describe the change in plain English, generates the userscript, installs it, and lets you keep editing it with follow-up requests. It also includes a built-in community where you can share your own tweeks and install modifications other people have published.


Tweeks is also a full userscript manager in its own right. Tampermonkey and Greasemonkey` .user.js` files import unchanged, and scripts from Greasy Fork run with the same` GM_*` APIs they expect, on Chrome, Firefox, and Safari. Everything the generator produces is a normal userscript you can read and edit.


## TL;DR


Use Tweeks if you want the flexibility of userscripts without starting from code. It is built for people who want to remove distractions, add missing buttons, change layouts, automate workflows, or install community modifications with less setup. It still handles a` .user.js` collection like any traditional manager.


Use Tampermonkey if you want a script-first tool and you are comfortable finding, reading, writing, or debugging userscripts yourself.


For technical users, the shorter version is:


Need Tampermonkey Tweeks


Install existing` .user.js` scripts Yes


Yes


Write custom JavaScript by hand Yes


Yes, source code is viewable and editable


Create changes from your description No


Yes


Revise a script after it is installed Edit the code manually Edit the code manually or describe the change you want


Share a modification with a public page Not built in, manually share with external site or gist Built in


Browse community modifications Usually via sites like Greasy Fork Built into Tweeks, also supports popular sites like Greasy Fork


Use classic` GM_*` APIs Yes


Yes


Use Tweeks-specific AI, email, toast, alert, avatar, and context-menu APIs No


Yes. See the[Tweeks API docs](https://www.tweeks.io/docs)


## What Tampermonkey Is


Tampermonkey comes from the userscript tradition: small programs that run on matching websites and change the page. A userscript might hide a sidebar, add a download button, restyle a feed, autofill a form, or add a shortcut the site never shipped.


The broader userscript ecosystem usually has three parts:


1. A userscript manager such as Tampermonkey, Greasemonkey, or Violentmonkey.
2. A script source such as[Greasy Fork](https://greasyfork.org/en) , a GitHub repo, a gist, or your own editor.
3. A browser extension runtime that decides which scripts run on which pages.


Tampermonkey is built for people who want to manage those scripts directly. Its documentation covers headers such as` @match` ,` @grant` ,` @run-at` ,` @require` , and` @connect` , plus` GM_*` APIs for storage, network requests, notifications, tabs, downloads, resources, and more. Its dashboard, editor, update checks, and configuration modes all assume you are comfortable working with scripts.


That model works well when you know the code you want to run, or when you are installing a script from a community you trust.


The challenge is when the desired change is clear but the implementation is not. "Hide Shorts from YouTube" is easy to say. Turning that into a stable script can require DOM inspection, selectors, page lifecycle handling, storage, permissions, and ongoing maintenance. Even for someone technical, it is time consuming and difficult to build it, and there are no guarantees that it will still work next week.


## What Tweeks Changes


Tweeks is a browser extension for creating, installing, sharing, and managing website modifications, which we call tweeks. Under the hood, most tweeks are userscripts.


The core difference is that Tweeks treats the script as the implementation, not the starting point. You start with the change you want. You can open a site and write:


> hide sponsored results and AI Overview on Google


or:


> add a download CSV button to this table


or:


> make LinkedIn filter to only posts within the last 3 days


When you ask Tweeks to create or update a tweek, it uses the page context and your request to generate a userscript, install it locally, and keep the source code available for inspection or manual editing.


The first of those prompts maps to two of the most-installed tweeks in the library,[Remove Google Search Sponsored Results](https://www.tweeks.io/t/b3bb5a6573f949899f779e86) and[Remove AI Overview & AI Mode on Google Search](https://www.tweeks.io/t/b181f606d977491aad41e0f3) . The "Hide Shorts from YouTube" example from earlier is there too, as[YouTube - Remove Shorts](https://www.tweeks.io/t/bcd8bc32b8034b79a78a8564) . Each installs in one click from its share page.


You get the flexibility of userscripts without starting from code. You also get a built-in community layer: install tweeks other people have published, share your own, or keep them private.


### Creating


In Tampermonkey, creation usually starts with an editor and a metadata block:


js


```text
// ==UserScript==
// @name         My site fix
// @match        https://example.com/*
// @grant        GM_addStyle
// ==/UserScript==
```


Tweeks includes a full-fledged editor with version history, syntax highlighting, and diffs, but most creation starts from the page and the request rather than manual coding. The generated result is still a script, but you don't need to know the metadata format before you can begin.


### Updating


When a site changes, a Tampermonkey user often debugs selectors, adjusts code, and tests the script again. In Tweeks, you can still edit the code directly, but you can also ask for an update in natural language: "it works on the homepage, but not on search result pages" or "keep the button, but move it next to the export menu."


That's essential for non-technical users, but it's also useful for technical users who don't want every small browser fix to become a maintenance task.


### Sharing and Discovery


Tampermonkey scripts are usually shared through external sites such as Greasy Fork, GitHub, forums, or direct` .user.js` links.


Tweeks brings more of that workflow into the product. You can keep a tweek private, publish it to the public library, add it to your profile, or install public tweeks from other users. Every public tweek gets a share page anyone can open and install from, like the[Universal AI Slop Finder](https://www.tweeks.io/t/PUjj1S2F) or the[X/Twitter article export button](https://www.tweeks.io/t/bc2926d27d5a4c6885dfd49c) . There are also per-site pages that collect everything published for a single site; see[YouTube](https://www.tweeks.io/sites/youtube.com) ,[X](https://www.tweeks.io/sites/x.com) , or[Google](https://www.tweeks.io/sites/google.com) .


The Tweeks extension can also recommend relevant public tweeks when you visit supported sites. Those recommendations are optional and can be turned off.


## Under the Hood


For technical readers, Tweeks is a Manifest V3 userscript manager with AI-assisted creation layered on top.


The extension includes:


- a script repository for installed scripts and source code
- a metadata parser for userscript headers
- a permission system for` @grant` directives
- a compiler that builds runtime stubs and exposes allowed APIs
- URL matching for` @match` ,` @include` , excludes, and globs
- runtime adapters for Chromium, Firefox, and Safari
- service-worker handlers for privileged operations such as tabs, downloads, notifications, network requests, and generation


On Chromium, this sits on top of the browser` userScripts` API where available. Manifest V3 made userscript execution more browser-specific, so Tweeks uses runtime adapters for Chromium, Firefox, and Safari and degrades gracefully if the browser has not granted the permissions needed to run userscripts.


Tweeks also implements familiar Greasemonkey-style APIs such as:


- ` GM_getValue` ,` GM_setValue` ,` GM_deleteValue` , and value listeners
- ` GM_xmlhttpRequest`
- ` GM_addStyle` and` GM_addElement`
- ` GM_notification`
- ` GM_openInTab`
- ` GM_setClipboard`
- ` GM_download`
- ` GM_registerMenuCommand`
- ` GM_getResourceText` and` GM_getResourceURL`


It also adds Tweeks-specific APIs:


- ` TW_inference` for model calls from a tweek
- ` TW_email` for sending or scheduling email to the signed-in user's account
- ` TW_contextMenu` for context-menu actions
- ` TW_toast` and` TW_alert` for local UI notifications


Those APIs are gated by userscript grants. A script does not get every capability just because it exists. The[Tweeks Engine API Reference](https://www.tweeks.io/docs) has signatures, grants, examples, and notes for each` TW_*` API.


## Privacy And Control


Any userscript manager deserves scrutiny because scripts can read and change pages where they run. Tweeks keeps the control points visible:


- You choose which tweeks to install.
- You can enable, disable, update, edit, or remove them.
- Public tweeks show source code before install.
- Scripts remain private unless you share them.
- Tweeks uses page context for generation only when you actively ask it to create or update a tweek.
- Installed tweeks run locally in the browser on the sites you choose.


## Which One Should You Use?


Choose Tampermonkey when:


- you already have a userscript you trust
- you are comfortable reading and debugging JavaScript
- you use an existing userscript community as your source of scripts
- you prefer local manual editing


Choose Tweeks when:


- you know the website change you want, but not necessarily the code
- you want a web editor with syntax highlighting, version control, and diff views
- you want to install community modifications with a simpler workflow
- you want built-in sharing and public profiles
- you want userscripts plus AI, email, notification, and context-menu APIs
- you want to import existing userscripts but manage them inside Tweeks


If you already have a userscript workflow and want a no-frills manager, Tampermonkey remains a good fit. If you want userscript compatibility plus help creating, updating, sharing, and discovering modifications, Tweeks adds that workflow on top of the same basic model. A good way to compare is to browse what people have already published for a site you use, like[Reddit](https://www.tweeks.io/sites/reddit.com) or[Hacker News](https://www.tweeks.io/sites/news.ycombinator.com) , and try one.


## Sources


- [Tampermonkey home](https://www.tampermonkey.net/)
- [Tampermonkey documentation](https://www.tampermonkey.net/documentation.php?locale=en)
- [Tampermonkey Content Script API notes](https://www.tampermonkey.net/documentation.php?locale=en&q=content_script_api)
- [Chrome userScripts API](https://developer.chrome.com/docs/extensions/reference/api/userScripts)
- [MDN userScripts API](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/userScripts)
- [Greasy Fork userscript overview](https://greasyfork.org/en)
- [About Tweeks](https://www.tweeks.io/about)
- [Tweeks Engine API Reference](https://www.tweeks.io/docs)
- [Tweeks extension listing](https://chromewebstore.google.com/detail/tweeks-customize-any-webs/fmkancpjcacjodknfjcpmgkccbhedkhc)
