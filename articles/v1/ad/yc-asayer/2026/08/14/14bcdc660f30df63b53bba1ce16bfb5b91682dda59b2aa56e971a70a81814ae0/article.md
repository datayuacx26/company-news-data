---
schema_version: "1.0.0"
document_id: "14bcdc660f30df63b53bba1ce16bfb5b91682dda59b2aa56e971a70a81814ae0"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/5-javascript-apis-frontend-developers-should-know/"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T08:48:12.350603+00:00"
fetched_at: "2026-08-13T08:48:14.609766+00:00"
content_hash: "sha256:dcfa7d6f4b0a7d38d05eb55eebe982c5f7849d7a6c5cdca005c7ca7e3ac6a54a"
---

# 5 JavaScript APIs Every Frontend Developer Should Know

The five native browser APIs most worth learning right now are IntersectionObserver, ResizeObserver, AbortController, the Clipboard API, and the View Transitions API: each replaces a common dependency with a platform primitive that ships in every current browser.


Most of us learn this the hard way, pulling in a whole library for a copy button or losing an afternoon to a search box that keeps painting stale results, only to find out later that the browser had the fix all along. If you already reach for` fetch` ,` localStorage` , and the DOM but haven’t touched the newer observer and utility APIs, this is the shortlist that lets you delete a library or two. Everything below is a *native web platform API* , not a hosted REST service like TMDB or Unsplash, so there’s no key, no server, and no runtime cost beyond the browser you already target. Each entry gets a one-line definition, a minimal working snippet, a real use case, and an honest support note.


## Key Takeaways


- IntersectionObserver reports when an element enters or leaves the viewport without a scroll listener, and has been[Baseline across browsers since March 2019](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) .
- ResizeObserver reacts to an element’s *own* content- or border-box size, which is what makes container-aware components possible where the window` resize` event can’t help.
- AbortController is the standard fix for React race conditions: pass` controller.signal` to` fetch` and call` controller.abort()` in effect cleanup so a stale response can’t overwrite fresh UI.
- ` navigator.clipboard.writeText()` is the promise-based replacement for the` document.execCommand('copy')` hack, but it only runs in a[secure context](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard/writeText) (HTTPS or localhost).
- Same-document View Transitions reached[Baseline Newly available on October 14, 2025](https://web.dev/blog/same-document-view-transitions-are-now-baseline-newly-available) and degrade gracefully, so they’re safe to ship behind an` if (document.startViewTransition)` guard.


## IntersectionObserver: the web API that replaces scroll listeners


IntersectionObserver asynchronously reports when a target element enters or leaves the viewport (or any ancestor), so you can lazy-load, trigger infinite scroll, and fire visibility analytics without a single scroll listener, and it’s been Baseline across browsers since March 2019. Scroll listeners fire dozens of times per second on the main thread and force you to compute geometry yourself; the observer batches that work off the critical path and calls you only when visibility actually changes.


```text
const   observer   =   new   IntersectionObserver  ((  entries  )   =>   {
for (  const   entry   of   entries  ) {
if (  entry  .  isIntersecting  ) {
entry  .  target  .  src   =   entry  .  target  .  dataset  .  src  ;   // load only when visible
observer  .  unobserve  (  entry  .  target  );
}
}
});


document  .  querySelectorAll  (  '  img[data-src]  '  ).  forEach  ((  img  )   =>   observer  .  observe  (  img  ));
```


This lazy-loads images as rows scroll into view. The same` isIntersecting` check drives infinite-scroll sentinels and “seen” analytics. Session replays of long feeds make the payoff visible: offscreen images and tracking events resolve only as their rows enter the viewport, not all at once on load. Retires` react-intersection-observer` and hand-rolled scroll math for most cases. (Note: the` trackVisibility` option is still experimental and not Baseline. Stick to` isIntersecting` .)


## ResizeObserver: element-level resize reactions


Reach for[ResizeObserver](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver) , not the window` resize` event, whenever a component must react to *its own* size: it reports element-level content- and border-box changes, which is what makes container-aware components possible. A` window.resize` handler tells you the viewport changed; it says nothing about a panel that shrank because a sidebar opened.


```text
const   ro   =   new   ResizeObserver  ((  entries  )   =>   {
for (  const   entry   of   entries  ) {
const   {   width   }   =   entry  .  contentRect  ;
entry  .  target  .  classList  .  toggle  (  '  is-narrow  '  ,   width   <   400  );
}
});


ro  .  observe  (  document  .  querySelector  (  '  .card  '  ));
```


Here the card switches to a compact layout based on its own width: a container query in JavaScript. It’s Baseline across current browsers and retires libraries like` element-resize-detector` . If you see a` ResizeObserver loop` console warning, it’s benign in most cases and means you mutated size inside the callback.


## AbortController: cancel fetches and fix race conditions


[AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController) is the standard fix for React race conditions: pass` controller.signal` to` fetch` , call` controller.abort()` in your effect cleanup, and a stale response can never overwrite fresh UI after a fast re-navigation.` AbortSignal` has been available across browsers since April 2018.


```text
useEffect  (()   =>   {
const   controller   =   new   AbortController  ();
fetch  (  `  /api/results?q=  ${  query  }`  , {   signal  :   controller  .  signal   })
.  then  ((  res  )   =>   res  .  json  ())
.  then  (  setResults  )
.  catch  ((  err  )   =>   {
if (  err  .  name   !==   '  AbortError  '  )   throw   err  ;
});
return   ()   =>   controller  .  abort  ();   // cancel on unmount or new query
}, [  query  ]);
```


The classic bug this kills is the one a session replay surfaces vividly: a user types fast, an earlier slow response lands last, and stale data paints over the current view. Aborting the in-flight request on cleanup makes that impossible. For timeouts specifically, you can skip the manual controller.[AbortSignal.timeout()](https://developer.mozilla.org/en-US/docs/Web/API/AbortSignal/timeout_static) works across browsers since April 2024 and aborts with a` TimeoutError` , while a user-triggered abort throws` AbortError` , so one` catch` can tell them apart:


```text
fetch  (  url  , {   signal  :   AbortSignal  .  timeout  (  5000  )   });
```


This retires` isMounted` flags and` axios.CancelToken` .


## Clipboard API: copy-to-clipboard without the execCommand hack


` navigator.clipboard.writeText()` is the modern, promise-based replacement for the` document.execCommand('copy')` hack. Just note that it only works in a secure context (HTTPS or localhost).


```text
copyBtn  .  addEventListener  (  '  click  '  ,   async   ()   =>   {
await   navigator  .  clipboard  .  writeText  (  snippet  );
copyBtn  .  textContent   =   '  Copied!  '  ;
});
```


This is the “copy code snippet” button on every docs site. Writing is Baseline and reliable; reading is best-effort.[readText()](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard/readText) resolves with an empty string when the clipboard has no text, and Firefox restricts it more tightly than writing, so copy is the dependable path and paste-reading is best treated as progressive enhancement. Retires` clipboard.js` .


## View Transitions API: animate DOM state changes natively


View Transitions animate between DOM states with` document.startViewTransition(() => updateDOM())` , and because it degrades gracefully (no animation, but the DOM still updates in unsupported browsers) it’s safe to ship today behind an` if (document.startViewTransition)` guard. Same-document transitions reached Baseline Newly available on October 14, 2025, working in Chrome 111+, Edge 111+, Firefox 144+, and Safari 18+.


```text
function   updatePage  (  newContent  ) {
if (  !  document  .  startViewTransition  ) {
render  (  newContent  );   // fallback: instant update, no animation
return  ;
}
document  .  startViewTransition  (()   =>   render  (  newContent  ));
}
```


Use it for tab switches, list-to-detail navigations, and filter changes, the crossfades and shared-element morphs you’d otherwise pull in Framer Motion for. The[startViewTransition() guard](https://developer.mozilla.org/en-US/docs/Web/API/Document/startViewTransition) means unsupported browsers just apply the change immediately. Note that[cross-document transitions](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@view-transition) are narrower: Chrome 126+, Edge 126+, and Safari 18.2+, but not yet supported in Firefox, so don’t conflate the two.


## Bonus APIs: structuredClone and the fetch gotcha


Pick one of these on your next ticket and delete the dependency it replaces: swap a scroll listener for IntersectionObserver, or wrap a search fetch in an AbortController. One honorable mention worth keeping in reach:[structuredClone()](https://developer.mozilla.org/en-US/docs/Web/API/Window/structuredClone) deep-copies objects without a` JSON.parse(JSON.stringify(...))` round-trip and preserves Dates, Maps, Sets, and typed arrays, though it throws` DataCloneError` on[functions and DOM nodes](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm) , so it’s for data, not live objects (Baseline since March 2022). And if` fetch` is still your baseline, remember that its promise[doesn’t reject on HTTP 404 or 500](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) , only on network failure, so check` response.ok` yourself.


## FAQs


What is the difference between IntersectionObserver and ResizeObserver?


IntersectionObserver reports when an element enters or leaves the viewport or an ancestor, so it drives lazy-loading, infinite scroll, and visibility analytics. ResizeObserver reports changes to an element's own content-box or border-box dimensions, so it drives container-aware layouts. IntersectionObserver answers 'is this visible,' ResizeObserver answers 'how big is this element'. Different questions, and neither replaces the window resize event's viewport-level role.


Should I use AbortSignal.timeout() or a manual AbortController for cancelling fetch requests?


Use AbortSignal.timeout() when you only need a fixed timeout, since it aborts automatically after the given milliseconds with a TimeoutError and needs no controller variable. Use a manual AbortController when cancellation is triggered by an event you control, such as React effect cleanup on unmount or re-navigation, where you call controller.abort() yourself. The two throw different errors, TimeoutError versus AbortError, so a single catch block can distinguish them.


Why does navigator.clipboard.writeText() fail on my local development site?


navigator.clipboard.writeText() only works in a secure context, meaning HTTPS or localhost. If you access a dev server over a plain HTTP address like a local IP or a non-localhost hostname, the browser blocks clipboard writes and the promise rejects. Use localhost or serve over HTTPS. Reading with readText() is restricted even further, especially in Firefox, so treat writing as the dependable path and paste-reading as progressive enhancement.


Is the View Transitions API safe to use if some browsers don't support it?


Yes, because it degrades gracefully. Guard the call with if (document.startViewTransition) before invoking it; in browsers that lack support, the DOM update still runs, it just applies immediately without animation. Same-document transitions reached Baseline Newly available on October 14, 2025, in Chrome 111+, Edge 111+, Firefox 144+, and Safari 18+. Cross-document transitions are narrower at Chrome 126+, Edge 126+, and Safari 18.2+, and are not yet supported in Firefox.


Open-source session replay


## Gain control over your UX


See how users are using your site as if you were sitting next to them, learn and iterate faster with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
