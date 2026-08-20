---
schema_version: "1.0.0"
document_id: "40dc3b437efe687cca4fa804dd59eff7e735fff2c15c3e0da6429731a67f3680"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/5-bookmarklets-developers/"
published_at: "2026-08-08T00:00:00+00:00"
first_seen_at: "2026-08-08T05:17:41.693999+00:00"
fetched_at: "2026-08-08T05:17:43.878103+00:00"
content_hash: "sha256:5ae0f572c9976295a1ed1c4c55160417606ee49f54952dda69611414ed39cdde"
---

# 5 Useful Bookmarklets for Developers

A bookmarklet is a bookmark whose URL is a single line of JavaScript prefixed with` javascript:` , so clicking it runs that code against the current page instead of navigating anywhere. That makes bookmarklets the fastest zero-install way to live-edit a page, debug layout, reveal a masked input, or grab a color — no extension, no DevTools panel, no build step. This article gives you five genuinely useful, copy-clean bookmarklets that work in current Chromium browsers, explains when to reach for each, and covers the gotchas that trip people up (single-line parsing, Content-Security-Policy blocking, same-origin limits) before showing you how to write your own.


Install any of them one of two ways: create a new bookmark and paste the` javascript:` code as its URL, or drag a link whose` href` is the code onto your bookmarks bar. Each snippet below is already a single line — paste it as-is.


## Key Takeaways


- A bookmarklet must be a single line prefixed with` javascript:` ; never use` //` line comments, which would comment out everything after them — use` /* */` if you need a comment.
- End DOM-mutating bookmarklets with` void 0` or wrap them in` void(...)` , because if a` javascript:` URL’s last expression returns a string, the browser replaces the page with that string.
- The native EyeDropper color picker works only in Chromium browsers (Chrome and Edge) over HTTPS; Firefox and Safari do not support it as of 2026.
- Bookmarklets that inject an external script (like the eruda console) are blocked on any site with a strict` Content-Security-Policy` , and network calls are still subject to same-origin and CSP` connect-src` rules.


## 1. Live-edit any page with design mode


Turn the whole page into an editable document to test copy, spacing, and layout on a live site without opening DevTools:


```text
javascript  :  document  .  designMode  =  "  on  "  ;  void   0  ;
```


Setting[document.designMode to "on"](https://developer.mozilla.org/en-US/docs/Web/API/Document/designMode) makes the entire document editable — the same effect as` contenteditable="true"` , but applied to the whole page rather than one element. Click into any heading or paragraph and type. Reach for this when a stakeholder asks “what if this button said X” or you want to check whether a longer product name breaks a nav bar, without touching the codebase.


The trailing` void 0` matters. A` javascript:` URL that ends in a string[gets treated as an HTML document the browser navigates to](https://developer.mozilla.org/en-US/docs/Web/URI/Reference/Schemes/javascript) ;` void 0` forces a non-string completion value so the page stays put. Prefer to edit one region only? Swap in` javascript:void(document.body.contentEditable=true)` .


## 2. Tint every element to debug layout


See the box bounds of every element at once — invaluable for spotting overflow, misalignment, and stray margins:


```text
javascript  :  document  .  querySelectorAll  (  "  *  "  ).  forEach  (  e  =>  e  .  style  .  background  =  "  rgb(0 0 0 / 10%)  "  );  void   0  ;
```


This applies a 10%-opacity black background to every element on the page. Because the semi-transparent layers stack, nested elements render progressively darker, so you can read the DOM’s depth by eye and immediately see which container is wider than its parent. If you prefer hard edges to shaded fills, swap the background line for an outline:` e.style.outline="1px solid rgb(255 0 0 / 40%)"` . Outlines don’t affect layout — unlike borders, they don’t take up box space — so the page geometry you’re debugging stays intact.


## 3. Reveal masked password fields


Flip every password field to plain text:


```text
javascript  :(()  =>  {  document  .  querySelectorAll  (  '  input[type=password]  '  ).  forEach  (  el  =>  el  .  type  =  '  text  '  )})();
```


This walks every` input\[type=password\]` and switches its` type` to` text` , exposing the current value. Reach for it when debugging an auth form, verifying what a password manager or browser autofill actually entered, or confirming that a paste didn’t pick up a trailing space. It reads the live DOM the same way an in-page debugger would — session replays of broken login flows frequently reveal that autofill wrote a subtly wrong value, and this is the fastest manual way to confirm that class of bug.


## 4. Grab any on-screen color with the native eyedropper


Pick any pixel on screen and get its hex value:


```text
javascript  :  void  (  async  ()  =>  {  try  {  prompt  (  '  HEX:  '  ,(  await   new   EyeDropper  ().  open  ()).  sRGBHex  )}  catch  (  e  ){  alert  (  e  )}})()
```


This calls the[EyeDropper API](https://developer.mozilla.org/en-US/docs/Web/API/EyeDropper_API) , which turns the cursor into a magnifier-and-picker; the selected color comes back as` sRGBHex` and lands in a` prompt` you can copy. One caveat to know: the EyeDropper API is **Chromium-only (Chrome and Edge), available only in secure contexts (HTTPS)** , and Firefox and Safari do not support it as of 2026. It’s also user-gated —[open() throws unless it’s triggered by a transient user activation](https://developer.mozilla.org/en-US/docs/Web/API/EyeDropper/open) — but a bookmarklet click *is* a user gesture, so it fires cleanly. The` try/catch` swallows the rejection you get if you hit` Esc` instead of picking.


## 5. Inject mobile DevTools with eruda


Get a full console and DOM inspector on any page — including on a phone with no DevTools:


```text
javascript  :(  function  (){  var   s  =  document  .  createElement  (  '  script  '  );  s  .  src  =  '  https://cdn.jsdelivr.net/npm/eruda  '  ;  s  .  onload  =function  (){  eruda  .  init  ()};  document  .  head  .  appendChild  (  s  )})();
```


This appends a` <script>` that loads[eruda](https://www.npmjs.com/package/eruda) — a console-for-mobile-browsers whose current stable release is 3.4.3 — and calls` eruda.init()` once it loads, floating a DevTools panel over the page. It’s the standout for debugging on real devices where you can’t attach a desktop inspector. Two things to know: the unpinned` cdn.jsdelivr.net/npm/eruda` URL always serves the latest published build, and eruda’s release cadence has stalled (no new npm version in over a year), so treat it as stable-but-not-actively-maintained. Because it loads an external script, this one is subject to the CSP limit described next.


## Why isn’t my bookmarklet running?


Four failure modes account for nearly every “it just doesn’t work”:


Problem Cause Fix


Half the code is ignored A` //` comment silences everything after it on the single line Use` /* */` ; a bookmarklet is one line


Page navigates away or shows raw text Final expression returned a string, rendered as HTML End with` void 0` or wrap in` void(...)`


eruda/external-script bookmarklet silently fails Strict CSP` script-src`[blocks the injected script](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy/script-src) Nothing client-side; the site’s policy wins


A` fetch()` bookmarklet is blocked Same-origin policy and CSP[connect-src](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy/connect-src) restrict cross-origin requests Call only same-origin or CORS-enabled endpoints


The CSP case is the one to internalize: on a site with a strict` Content-Security-Policy` — try the eruda bookmarklet on GitHub versus a permissive page and watch the difference — the console simply won’t appear, and a CSP violation is logged to the (real) console instead.` javascript:` navigation itself can also be blocked by` script-src` .


## How do you write your own bookmarklet?


To write your own bookmarklet, wrap your code in an IIFE inside a` javascript:` URL and end DOM-mutating expressions with` void 0` so the page doesn’t navigate to a stringified return value:


```text
javascript  :(  function  (){  /* your code here */  })();
```


The IIFE keeps your variables out of the page’s global scope, and` javascript:foo()` is conventionally prefixed with` void` to prevent accidental navigation if the call returns a string. Write and debug the logic in the DevTools console first, then collapse it to one line — stripping` //` comments — and paste it as the bookmark’s URL. Anything you can type into the console, you can carry with you as a one-click bookmarklet.


## FAQs


Why does my bookmarklet stop working halfway through the code?


A bookmarklet is a single line, so a // line comment silences every character after it, including your closing braces and function calls. Replace any // comments with /* block */ comments, or remove them entirely. When collapsing multi-line console code into a bookmarklet, strip all // comments first, because the newline that would normally end them no longer exists on a single line.


What's the difference between designMode and contentEditable in a bookmarklet?


Setting document.designMode to 'on' makes the entire document editable in one statement, while contentEditable is set per element, so document.body.contentEditable = true edits only the body region. Use designMode when you want to click into any heading, paragraph, or button across the whole page; use contentEditable when you want to scope editing to a single container and leave the rest of the page fixed.


Does the EyeDropper bookmarklet work in Firefox or Safari?


No. The EyeDropper API is Chromium-only, working in Chrome and Edge and only over HTTPS in a secure context; Firefox and Safari do not support it as of 2026. The API also requires a transient user activation, but a bookmarklet click counts as a user gesture, so open() fires cleanly when triggered that way. In unsupported browsers the bookmarklet throws because the EyeDropper constructor is undefined.


Can a bookmarklet make cross-origin fetch requests?


Not freely. A fetch() call inside a bookmarklet runs in the current page's context, so it is bound by the same-origin policy and the site's CSP connect-src directive. Requests to a different origin are blocked unless that endpoint sends the correct CORS headers. Restrict bookmarklet network calls to the same origin as the page or to CORS-enabled APIs; there is no client-side workaround for a site's policy.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
