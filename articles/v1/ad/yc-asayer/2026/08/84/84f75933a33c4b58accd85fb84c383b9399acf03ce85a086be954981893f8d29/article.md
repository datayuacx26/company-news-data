---
schema_version: "1.0.0"
document_id: "84f75933a33c4b58accd85fb84c383b9399acf03ce85a086be954981893f8d29"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/rel-noopener-obsolete-links/"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T18:37:30.227787+00:00"
fetched_at: "2026-08-18T18:37:31.619257+00:00"
content_hash: "sha256:ab08c1f337ce673a9768e600bb155f31aec825b49da360056258bcc22ab5d5a2"
---

# Why rel='noopener' Is Obsolete for Links

For a plain` target="_blank"` link,` rel="noopener"` is now redundant: every current version of Chrome, Edge, Firefox, and Safari applies` noopener` behavior automatically, so a bare` target="_blank"` already sets` window.opener` to` null` .


If you still type it out of muscle memory, or watch the linter flag the one anchor you forgot, you are patching a hole the browser closed years ago. This article explains the vulnerability it was meant to prevent, when browsers made the fix default, and the precise cases where` rel` still does real work:` noreferrer` (not automatic),` rel="opener"` (opt back in), and the` Cross-Origin-Opener-Policy` header for site-wide control.


## Key Takeaways


- On modern browsers, a bare` target="_blank"` already nulls` window.opener` , so hand-adding` rel="noopener"` is defense-in-depth for a hole the browser already closed.
- Implicit` noopener` shipped in stages (Safari in 2018–19, Firefox 79 in mid-2020, Chromium 88 in early 2021) and is now part of the WHATWG HTML standard.
- Implicit` noopener` covers roughly 95% of global browser usage, per caniuse.com.
- ` noreferrer` is *not* implicit: it still strips the` Referer` header and also implies` noopener` , so add it only when you want referrer privacy.
- Use` rel="opener"` to opt back into` window.opener` , and` Cross-Origin-Opener-Policy: same-origin` to sever opener sharing across a whole document in one place.


## The original problem: reverse tabnabbing


Before browsers changed the default, a` target="_blank"` link handed the newly opened page a live reference back to the page that opened it.[Reverse tabnabbing](https://owasp.org/www-community/attacks/Reverse_Tabnabbing) is the attack that exploits this: the destination page reads` window.opener` and redirects the original tab to a phishing clone while the user is focused on the new tab. Mathias Bynens’[canonical explainer on the problem](https://mathiasbynens.github.io/rel-noopener/) sums it up plainly: wherever` window.opener` exists, the opened page can steer the opener somewhere else, whatever origin either page belongs to.


The exploit is a one-liner running in the opened document:


```text
if (  window  .  opener  ) {
window  .  opener  .  location   =   '  https://you-re-hacked.com  '  ;
}
```


The critical detail is that this works across origins. Reading and writing` window.opener.location` is not blocked when the two pages come from different hosts, so neither the same-origin policy nor CORS stands in the way of redirecting the opener. That made it dangerous anywhere you rendered user-generated or third-party links (forums, comments, profile fields) where an attacker controls the` href` .


## What changed: target=“_blank” now implies rel=“noopener”


Browsers fixed the default. On` <a>` ,` <area>` , and` <form>` elements, a` target="_blank"` now carries[the same effect as writing rel="noopener" yourself](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/rel/noopener) : the opened document gets` null` back from` window.opener` , with no attribute required. That behavior is written into the WHATWG HTML specification, whose[rules for following a hyperlink](https://html.spec.whatwg.org/multipage/links.html#following-hyperlinks-2) treat any` _blank` target as noopener unless the link opts out with` rel="opener"` . OWASP now points readers at that same standardized default and treats the attack as largely closed on evergreen browsers.


The change rolled out over roughly three years, so “modern browsers do this” is a timeline, not a single date:


Engine First stable version with implicit` noopener` Approx. ship date


Safari / WebKit Safari 12.1 (preview in Tech Preview 68) Late 2018 – 2019


Firefox / Gecko[Firefox 79](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Releases/79) Mid-2020


Chromium (Chrome, Edge) Chrome/Edge 88 Early 2021


Note these versions describe *implicit* behavior, not when the` rel="noopener"` attribute itself became supported. That landed years earlier and is a different milestone.[Caniuse’s implicit- noopener table](https://caniuse.com/mdn-html_elements_a_implicit_noopener) puts global support at roughly 95%, with evergreen browsers covered since around 2018. The remaining slice is small but real, so check your own analytics before you strip the attribute out. The notable holdout is legacy non-Chromium Edge.


## Obsolete to hand-add is not the same as useless


` rel="noopener"` being redundant to type does not make the whole` rel` attribute pointless. The keyword that’s now automatic is` noopener` specifically. The others still change behavior:


Keyword What it does Still needed to type in 2026?


` noopener` Nulls` window.opener` in the opened page No, implicit on` target="_blank"`


` noreferrer` Strips the` Referer` header *and* implies` noopener` Only when you want referrer privacy


` opener` Restores` window.opener` (opts back in) Yes, when you genuinely need the reference


` noreferrer` is not implicit. It still suppresses the` Referer` header, so add it only when you actually want to withhold the originating URL from the destination. It also carries the security benefit for free: because` noreferrer` nulls the opener as well, adding` noopener` alongside it buys nothing. That makes the common` rel="noopener noreferrer"` pairing doubly redundant on modern browsers, since` noreferrer` on its own covers both concerns.


If you genuinely need the opened page to keep its` window.opener` reference (a popup that posts a message back, say), opt in explicitly with` rel="opener"` . The WebKit release notes that introduced the change describe it the same way: the secure behavior is now the default, and` rel="opener"` is how you deliberately reverse it.


One honest caveat for legacy support: adding` rel="noopener"` anyway is harmless.[Chrome’s Lighthouse audit docs](https://developer.chrome.com/docs/lighthouse/best-practices/external-anchors-use-rel-noopener) note that spelling the attribute out still buys some cover for anyone stuck on an older engine such as Edge Legacy. It’s noise on modern browsers, but it isn’t wrong.


## COOP: the scalable, site-wide control


To sever` window.opener` sharing across an entire document in one place, send the` Cross-Origin-Opener-Policy: same-origin` response header instead of decorating every link. The[Cross-Origin-Opener-Policy (COOP) header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cross-Origin-Opener-Policy) decides whether a newly opened top-level document joins your browsing context group or gets one of its own. Under` same-origin` , cross-origin documents land in a separate group and the references between them and their opener are cut, which closes the opener channel once, centrally, rather than per-anchor.


```text
# nginx
add_header Cross-Origin-Opener-Policy   "same-origin"  ;
```


```text
// Express
app  .  use  ((  req  ,   res  ,   next  )   =>   {
res  .  set  (  '  Cross-Origin-Opener-Policy  '  ,   '  same-origin  '  );
next  ();
});
```


One constraint: COOP is delivered only as an HTTP response header. There is no` <meta http-equiv>` equivalent, so if you can’t set response headers on your infrastructure, you can’t apply COOP. It’s widely supported in current browsers and worth enabling as defense-in-depth. A session replay of a real user clicking an external` target="_blank"` link is a practical way to confirm the original tab was never navigated, and to reproduce any unexpected-navigation report against the exact browser the user was on.


## The verdict: what to do now


For new code targeting modern browsers, don’t hand-add` rel="noopener"` ; the browser sets it for you. You can safely relax lint rules that force it on every` target="_blank"` , such as[react/jsx-no-target-blank](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-no-target-blank.md) ; keep the rule enabled only if you must cover Edge Legacy or other pre-2021 engines. Add` rel="noreferrer"` when, and only when, you want to suppress the` Referer` header. Use` rel="opener"` in the rare case you need the opener reference back. For a scalable, document-wide guarantee, send` Cross-Origin-Opener-Policy: same-origin` .


Worth flagging: earlier guidance, including OpenReplay’s own[older post recommending rel="noreferrer noopener" on every link](https://blog.openreplay.com/strengthen-security-and-privacy-with-the-rel-attribute/) , treats` noopener` as something you must always type and doesn’t account for the implicit default. That reflects a habit the industry held onto long after the browsers moved. The accurate 2026 position is narrower:` noopener` is now the default, so hand-adding it is obsolete, while` noreferrer` ,` rel="opener"` , and COOP each still do a distinct job. Reach for those deliberately, and let the browser handle the rest.


## FAQs


Does rel='noreferrer' include rel='noopener'?


Yes. Setting rel='noreferrer' implies rel='noopener' automatically, so it nulls window.opener in addition to stripping the Referer header. This means the common rel='noopener noreferrer' pairing is doubly redundant on modern browsers: noreferrer alone covers both the opener nulling and the referrer suppression. Add noreferrer only when you actually want to withhold the originating URL from the destination page.


What happens if I omit rel='noopener' entirely on a target='_blank' link today?


Nothing dangerous on modern browsers. A bare target='_blank' already sets window.opener to null because Chrome, Edge, Firefox, and Safari apply noopener behavior implicitly, a rule codified in the WHATWG HTML standard. Caniuse puts that at roughly 95% of global browser usage. Reverse tabnabbing is inert by default; the only gap is legacy engines like non-Chromium Edge Legacy.


Can I set Cross-Origin-Opener-Policy using a meta tag instead of a header?


No. COOP is delivered only as an HTTP response header, and there is no meta http-equiv equivalent. If your infrastructure cannot set response headers, you cannot apply COOP and must fall back to per-link rel attributes or the implicit noopener default. When you can set headers, sending Cross-Origin-Opener-Policy: same-origin severs window.opener sharing across an entire document in one central place rather than decorating every anchor.


How do I opt back into window.opener when I actually need it?


Use rel='opener' explicitly on the link. Because the secure behavior of nulling window.opener is now the browser default, rel='opener' is how you deliberately restore the opener reference, for example when a popup needs to post a message back to the page that opened it. This reverses the implicit noopener behavior for that specific link without affecting others.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
