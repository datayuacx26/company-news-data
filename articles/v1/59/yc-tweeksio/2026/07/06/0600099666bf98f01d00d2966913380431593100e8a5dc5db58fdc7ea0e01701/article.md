---
schema_version: "1.0.0"
document_id: "0600099666bf98f01d00d2966913380431593100e8a5dc5db58fdc7ea0e01701"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-rss-bbf1d949eada"
canonical_url: "https://www.tweeks.io/blog/best-tampermonkey-alternatives"
published_at: "2026-07-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.378405+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:31a301bc9edc815aee1f05ae36f7b51d46ed55e17cb77f26151e18fa7ee3657b"
---

# Best Tampermonkey Alternatives in 2026

Search for a Tampermonkey alternative and most of what comes up was written before mid-2025, when Chrome finished disabling Manifest V2 extensions and broke several of the managers those pages still recommend. Everything below runs today, on the browsers listed.


One disclosure before the list: we build[Tweeks](https://www.tweeks.io/about) , which shows up near the end, so you know where we stand. We might be biased, but it's pretty darn good, and[our users on the Chrome Web Store](https://chromewebstore.google.com/detail/tweeks-customize-any-webs/fmkancpjcacjodknfjcpmgkccbhedkhc/reviews) seem to think so too.


## TL;DR


[Tweeks](https://www.tweeks.io/about) is the best Tampermonkey alternative for most people in 2026. It's a full Manifest V3 userscript manager: your existing` .user.js` files import unchanged and both` GM_*` API styles work. It's the only manager on this list that runs on Chrome, Firefox, and Safari and ships updates daily. And when you want a new script, you can describe the change that you want instead of writing the code yourself. Installed tweeks run locally in your browser.


Tweeks handles the traditional find-or-write-the-script workflow too, but if you'd rather have a manager that only does that, the usual names by browser: ScriptCat on Chrome, Violentmonkey or Greasemonkey on Firefox, Userscripts on Safari. The rest of this post is the detail behind those calls.


## What Chrome broke in 2025


You can't pick a userscript manager in 2026 without knowing what happened in 2025.


Chrome finished its long-promised Manifest V2 phase-out around July 2025 and force-disabled the extensions that hadn't migrated to Manifest V3. Userscript managers were hit harder than most, because running user-supplied code is the one thing MV3 was designed to restrict. The one route left is the` chrome.userScripts` API, and it only works if the user switches it on themselves. Originally that meant enabling Developer Mode; since Chrome 138 each extension's details page has its own "Allow user scripts" toggle as well. Chrome is making this harder and harder.


Every manager on Chrome now needs that toggle, so don't count the setup step against any one of them. Where the purge does matter is what it killed: a manager that never shipped an MV3 version stopped working on Chrome, however many lists still recommend it. Firefox kept Manifest V2 support and came through untouched. Safari is a separate and trickier world of its own, though Tweeks has that covered too.


## Violentmonkey


For years the standard advice was: want an open source Tampermonkey? Install[Violentmonkey](https://violentmonkey.github.io/) . It's a barebones, simple manager, MIT-licensed, and it should still work on Firefox. It runs the standard userscript format with the usual` GM_*` APIs, and most of what's on Greasy Fork works with it unchanged.


The stable release never made it past Manifest V2, so Chrome's purge disabled it along with the rest of the holdouts. The project[has since shipped a Manifest V3 beta](https://violentmonkey.github.io/) , which the maintainers say will be published as stable after a test period. For now it's unstable: bugs, breaking changes, and data loss are all on the table, so export a backup before trusting it with your scripts. If a roundup recommends Violentmonkey for Chrome, check whether it means the disabled stable release or the beta. And if the purge stranded your script collection on Chrome, Tweeks runs on today's Chrome and imports your` .user.js` files unchanged.


If all you need on Firefox is something minimal with code you can read, it does the job.


## ScriptCat


[ScriptCat](https://scriptcat.org/en/) is the closest thing to Violentmonkey that still runs on Chrome. It's open source, built on Manifest V3 from the start, and it handles standard userscripts and the` GM_*` APIs. There are extras like background scripts, scheduled runs, and cloud sync, but underneath it's still a plain userscript manager: the scripts come from Greasy Fork or from you. Tweeks is the exception to that on this list; you describe the change and it writes the script.


It's also a riskier bet than its open license suggests. The project is small, most of its community and documentation live in Chinese (the[English docs](https://docs.scriptcat.org/en/) still have untranslated corners), and open code only protects you if people are actually reading it, which a small community makes less likely. Same "Allow user scripts" toggle as everything else on Chrome.


If open source on a Chromium browser is a hard requirement, this is where you land; just know you're betting on a smaller project than the established names.


## Userscripts, the Safari app


Safari users have been the ecosystem's second-class citizens for a decade, and[Userscripts](https://github.com/quoid/userscripts) is the app that fixed it. It's free and open source, and it runs as a proper Safari extension on macOS, iOS, iPadOS, and even visionOS, which means you can run userscripts on an iPhone. It handles JavaScript and CSS injection and supports a solid subset of the` GM.*` APIs; if a script you need falls outside that subset, Tweeks supports the full GM API on Safari. The last stable release landed in January 2026, with betas still coming since.


It's simpler than the desktop managers by design: no dashboard, your scripts just live in a folder you point it at. For a personal collection on Apple platforms, that simplicity is most of the appeal.


If you want free and open source on Apple devices, this is the one.


## Greasemonkey


[Greasemonkey](https://addons.mozilla.org/en-US/firefox/addon/greasemonkey/) started all of this in 2005; people still sometimes call userscripts Greasemonkey scripts. It remains Firefox-only and still gets maintained, though updates arrive at a leisurely pace.


The thing to know before choosing it is API drift. Greasemonkey 4 moved to asynchronous` GM.*` APIs years ago, and plenty of scripts on Greasy Fork still target the older synchronous` GM_*` style, so now and then you'll find a script that works in Tampermonkey and Violentmonkey but not here. Tweeks supports both styles, for what it's worth. For scripts you write yourself, none of that matters, and Greasemonkey is a fine, minimal home with two decades of history behind it.


If you're on Firefox and write your own scripts, the original still earns its keep.


## OrangeMonkey


[OrangeMonkey](https://chromewebstore.google.com/detail/orangemonkey/ekmeppjgajofkpiofbebgcbohbmfldaf) ranks high in Chrome Web Store searches, and it does work on current Chrome. The listing presents it as a Violentmonkey fork with a refreshed design and says it collects no data.


We couldn't find a public source repository to check any of that against, which makes it the one manager on this list that nobody outside the project can audit. A userscript manager can read and rewrite every page you visit, so that gap matters more here than it would for most extensions. Weigh it accordingly.


If you want a Violentmonkey-style UI on Chrome and you've made peace with the trade, it works.


## Tweeks


Every other manager on this list assumes the script already exists: you find it on Greasy Fork or you write it yourself.[Tweeks](https://www.tweeks.io/about) drops that assumption. You open the site, describe the change you want ("[hide Shorts](https://www.tweeks.io/t/bcd8bc32b8034b79a78a8564) ", "add a download CSV button to this table"), and Tweeks writes the userscript and installs it. The script stays editable afterward, in code or in plain English, so when the site redesigns and something breaks, the fix is another sentence instead of an evening in DevTools.


Underneath, Tweeks is a complete userscript manager in the same class as everything above, and the only actively developed one that runs on Chrome, Firefox, and Safari. It imports existing` .user.js` files, supports the full GM API in both the classic` GM_*` and modern async` GM.*` styles, and adds its own for things like[model calls and email from a script](https://www.tweeks.io/docs) . There's also a public library where every tweek shows its source before install, like[removing Google's sponsored results](https://www.tweeks.io/t/b3bb5a6573f949899f779e86) or[hiding YouTube Shorts](https://www.tweeks.io/t/bcd8bc32b8034b79a78a8564) . We wrote a longer[comparison with Tampermonkey](https://www.tweeks.io/blog/tweeks-vs-tampermonkey) if you want the detailed version. On Chrome, it needs the same "Allow user scripts" toggle as every other MV3 manager.


On privacy: installed tweeks run entirely in your browser, like any other manager on this list. Page content only goes to the generation service when you ask Tweeks to create or update a script on that page. If you import scripts or install from the library, no page content is sent anywhere.


Pick it if you know what you want changed and would rather describe it than build it, or if you want one manager across all three browsers.


## Side by side


Manager Chrome today Firefox Safari / iOS New modifications Activity


Tampermonkey Yes


Yes


Yes


Find or write scripts Maintenance mode (May 2026)


Violentmonkey Beta only Yes


No


Find or write scripts Active but slow (May 2026)


ScriptCat Yes


Yes


No


Find or write scripts Active (June 2026)


Userscripts No


No


Yes


Find or write scripts Active but slow (January 2026)


Greasemonkey No


Yes


No


Find or write scripts Maintenance mode (June 2026)


OrangeMonkey Yes


No


No


Find or write scripts Unknown (store updated March 2026)


Tweeks Yes


Yes


Yes


Describe the change; Tweeks writes the script Active (daily updates)


Everything that runs on Chrome needs the per-extension "Allow user scripts" toggle. Dates in the activity column are the latest stable release or store update we could verify as of July 12, 2026.


## Which one should you pick?


Start with Tweeks. Yes, it's ours, but the case is easy to make: it's the most modern manager here, it ships updates daily, and it's the only one that writes the script for you. It also runs on Chrome, Firefox, and Safari, so it's one decision instead of three.


If you'd rather stick with a traditional manager, the runners-up by browser: on Chrome, keep Tampermonkey if it's working for you and closed source doesn't bother you, or take ScriptCat for open source with the small-project caveats above. On Firefox, Violentmonkey still covers the basics and Greasemonkey rewards people who write their own scripts. On Safari, Userscripts is the free and open source pick.


Whichever you land on, the scripts themselves are portable. The userscript format has outlived every manager on this list so far, and your` .user.js` files will move with you when the next reshuffle comes.


## Common questions


### Is Tweeks a real userscript manager or an AI tool with scripts bolted on?


A real one. Tweeks parses the standard metadata block (` @match` ,` @include` ,` @grant` ,` @require` ) and implements the classic synchronous` GM_*` APIs along with the modern async` GM.*` style, so a Tampermonkey or Greasemonkey` .user.js` file imports unchanged. Installed scripts run locally in the browser like they would anywhere else. The AI writes and updates scripts, and scripts can use it too, something no traditional manager offers: the[TW_inference](https://www.tweeks.io/docs) grant lets a tweek call a model at runtime, to summarize a thread or filter a feed by meaning. Like every grant, it's declared in the metadata and shown before install.


### Which Tampermonkey alternatives still work on Chrome in 2026?


Tampermonkey itself, ScriptCat, OrangeMonkey, and Tweeks all run on current Chrome, and all four need the per-extension "Allow user scripts" toggle. Greasemonkey does not run on Chrome, and Violentmonkey's stable release doesn't either, though it now has a Manifest V3 beta.


### Is there a userscript manager that works on Chrome, Firefox, and Safari?


Two on this list cover all three: Tampermonkey, whose last stable release was in May 2026, and Tweeks, which ships updates daily. Everything else covers one or two browsers.


### Is it safe to use an AI-assisted manager on sensitive pages?


Generation and execution are different questions. Installed scripts run locally in every manager here, Tweeks included. Tweeks sends page context to its generation service only when you ask it to create or update a script on that page, so don't invoke generation on a page you wouldn't share, same as you wouldn't paste that page into a chatbot. Installing from the library or importing your own scripts involves no generation at all. The exception is deliberate, and it's a capability traditional managers don't have: a tweek that declares the` TW_inference` grant can call a model while it runs. The script sends what it passes and nothing else, and the grant shows before install like any other userscript permission, so you know up front whether a script can reach the model service at all. On the service side, Tweeks is SOC 2 Type 2 compliant and has been through independent security audits.


## Sources


- [Chrome userScripts API reference](https://developer.chrome.com/docs/extensions/reference/api/userScripts)
- [Chrome: Resuming the transition to Manifest V3](https://developer.chrome.com/blog/resuming-the-transition-to-mv3)
- [Violentmonkey releases](https://github.com/violentmonkey/violentmonkey/releases)
- [ScriptCat documentation](https://docs.scriptcat.org/en/)
- [Userscripts for Safari on GitHub](https://github.com/quoid/userscripts)
- [Greasemonkey on Firefox Add-ons](https://addons.mozilla.org/en-US/firefox/addon/greasemonkey/)
- [Tampermonkey](https://www.tampermonkey.net/)
- [Tweeks vs. Tampermonkey](https://www.tweeks.io/blog/tweeks-vs-tampermonkey)
