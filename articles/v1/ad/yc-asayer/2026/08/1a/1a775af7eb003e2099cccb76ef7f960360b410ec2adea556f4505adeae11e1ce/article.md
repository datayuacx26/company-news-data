---
schema_version: "1.0.0"
document_id: "1a775af7eb003e2099cccb76ef7f960360b410ec2adea556f4505adeae11e1ce"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/jspi-javascript-wasm-bridge/"
published_at: "2026-08-02T00:00:00+00:00"
first_seen_at: "2026-08-02T13:12:05.513475+00:00"
fetched_at: "2026-08-05T03:48:28.288743+00:00"
content_hash: "sha256:a27bd7d635cb4a03964f5344e437b6b3531e7253819d3dc6f9db10b25eb4b871"
---

# JSPI Explained: A Better Bridge Between JavaScript and Wasm

JavaScript Promise Integration (JSPI) lets a WebAssembly module call a Promise-returning JavaScript import as if it were a synchronous function: the module suspends when the import returns a Promise and resumes with the resolved value, with no manual callback management. That single capability closes a long-standing gap — synchronous Wasm compiled from C, C++, or Rust could not` await` an asynchronous browser API like` fetch` or IndexedDB without heavy tooling. This article covers the problem JSPI solves, the current two-function API, a working` fetch` example, and where it ships as of 2026. One warning up front: most JSPI tutorials still show a removed` Suspender` -object API — everything below uses the current surface.


## Key Takeaways


- JSPI’s public API is exactly two pieces:` new WebAssembly.Suspending(fn)` marks a Promise-returning import, and` WebAssembly.promising(exportFn)` wraps an exported Wasm function so calling it returns a Promise.
- Feature-detect with` 'Suspending' in WebAssembly` — never` 'Suspender' in WebAssembly` , which checks for the removed pre-2024 API.
- As of mid-2026 JSPI is a phase-4 (effectively standardized) proposal shipping in Chrome 137+, available in Safari 27 beta, and in Firefox running Nightly-only behind a pref with default-on intended for Firefox 153.
- If the imported Promise rejects, JSPI throws an exception into the suspended computation rather than returning an error value to Wasm.
- Unlike Binaryen’s Asyncify, JSPI uses native engine stack-switching, so the binary keeps its straight-line synchronous code with no instrumentation bloat.


## The painful bridge: synchronous Wasm meets an async web


The core mismatch is architectural. WebAssembly compiled from C, C++, or Rust assumes blocking calls — a function calls another, waits for the return value, and continues. The web platform is the opposite:` fetch` , IndexedDB, and most modern browser APIs return Promises and resolve later, driven by the event loop. When Wasm calls a JavaScript function that hands back a Promise, the module has no native way to pause, wait for resolution, and resume where it left off.


Before JSPI, the standard workaround was[Binaryen’s Asyncify](https://emscripten.org/docs/porting/asyncify.html) , a whole-program transform that rewrites the Wasm binary so it can unwind its own stack into linear memory and rewind it later. It works, but the cost is real: the transform inflates binary size and adds per-call overhead to instrumented functions. For a high-throughput compute routine that only occasionally needs to` fetch` config or read from IndexedDB mid-computation, paying that tax across the entire module is a poor trade.


## What JavaScript Promise Integration does


[JavaScript Promise Integration](https://github.com/WebAssembly/js-promise-integration/blob/main/proposals/js-promise-integration/Overview.md) bridges synchronous WebAssembly and asynchronous Web APIs by mapping a synchronous Wasm call into an asynchronous one: it suspends the module when a Promise-bearing import is called and resumes it when the Promise settles. It allows the WebAssembly application to invoke so-called Promise-bearing imports and access the value of the Promise, without having to explicitly manage the asynchronous callbacks normally associated with Promises.


Importantly, this is not a language change. The proposal makes no changes to the JavaScript language nor to the WebAssembly language. There are no new WebAssembly instructions or types specified. Semantically, all of the changes outlined are at the boundary between WebAssembly and JavaScript. That boundary framing matters for the API design and for how suspension is scoped.


## The two-piece API and a working fetch example


JSPI’s entire public surface is two elements. There are[two elements in the JSPI API](https://github.com/WebAssembly/js-promise-integration/blob/main/proposals/js-promise-integration/Overview.md) : the WebAssembly.Suspending constructor and the WebAssembly.promising function.` new WebAssembly.Suspending(fn)` marks a Promise-returning import; the` WebAssembly.promising` function is used to wrap an exported WebAssembly function into one that returns a Promise. Note the casing —` Suspending` is a constructor (capitalized),` promising` is a function (lowercase).


Here is the canonical shape adapted from the spec’s example: a` fetch` -based import wrapped with` Suspending` , an export wrapped with` promising` , and the resulting Promise awaited from JavaScript.


```text
// An async import that returns a Promise resolving to a number.
const   computeDelta   =   ()   =>
fetch  (  '  https://example.com/data.txt  '  )
.  then  (  res   =>   res  .  text  ())
.  then  (  txt   =>   parseFloat  (  txt  ));


const   importObject   =   {
js  :   {
// Mark the Promise-returning import as suspending.
compute_delta  :   new   WebAssembly  .  Suspending  (  computeDelta  ),
},
};


const   {   instance   }   =   await   WebAssembly  .  instantiateStreaming  (
fetch  (  '  module.wasm  '  ),
importObject  ,
);


// Wrap the export so calling it returns a Promise.
const   updateState   =   WebAssembly  .  promising  (  instance  .  exports  .  update_state  );


const   result   =   await   updateState  ();   // suspends inside Wasm on compute_delta, resumes with the value
```


Inside` update_state` , the Wasm code calls` compute_delta` with an ordinary synchronous call signature. When that import returns a Promise, the module suspends; when the Promise resolves, the resolved value becomes the import’s return value and execution continues.


## Behavior worth knowing


Three details separate JSPI in practice from the naive mental model.


**Suspension is delimited by the JS/Wasm boundary.** The` Suspending` import and the` promising` export form a pair — the innermost call into a wrapped export determines the cut point for what suspends. Only WebAssembly computations may be suspended using JSPI; this is enforced by requiring that[only WebAssembly frames are active](https://github.com/WebAssembly/js-promise-integration/blob/main/proposals/js-promise-integration/Overview.md) between the call to a promising function and any call to a Suspending wrapped import.


**It only suspends if a Promise is actually returned.** Instead of always suspending when calling a JavaScript function from a suspending import,[we only suspend when the JavaScript function actually returns a Promise](https://v8.dev/blog/jspi-newapi) . A plain return value passes straight through with no trip to the event loop.


**A rejected Promise throws into Wasm.** If the Promise is rejected, then instead of resuming the WebAssembly module with the value, an exception is propagated into the suspended computation. In practice rejection is usually handled on the JavaScript side, because a language like Rust often can’t act on that exception directly — the[wasm-bindgen project has discussed](https://github.com/wasm-bindgen/wasm-bindgen/issues/3633) adding an explicit error-indicating type, an open discussion rather than settled API.


## Browser and toolchain status (2026)


JSPI reached phase 4 of the W3C WebAssembly process — it is[phase 4 in the W3C WebAssembly WG](https://v8.dev/blog/jspi) , meaning the specification has been voted on by the W3C Wasm CG — it is effectively standardized. This specification was[standardized by the W3C WebAssembly CG in April 2025](https://github.com/web-platform-tests/interop/issues/1093) .


Environment Status (mid-2026)


Chrome / Edge Shipping stable since[Chrome 137](https://developer.chrome.com/blog/new-in-chrome-137) (May 2025)


Safari Available in[Safari 27 beta](https://webkit.org/blog/17967/news-from-wwdc26-webkit-in-safari-27-beta/)


Firefox Nightly-only behind a pref; default-on intended for Firefox 153


Node.js Behind` --experimental-wasm-jspi`


For Firefox, use Mozilla’s own status rather than the “Firefox 139” figure floating around: per the[Intent to Ship](http://www.mail-archive.com/dev-platform@mozilla.org/msg01810.html) (10 Jun 2026), this feature has been developed and shipped behind a pref, enabled Nightly-only since Fx152. Mozilla intends to enable WebAssembly JS-Promise-Integration (JSPI) by default on all platforms as of Firefox 153. At the time of writing,[caniuse](https://caniuse.com/wf-wasm-jspi) still lists stable Firefox as not yet default-on, so confirm before relying on it there.


On toolchains, most C/C++ projects need no source changes. If you are an Emscripten user, then using the new API will typically involve no changes to your code. You must be using a version of Emscripten that is[at least 3.1.61](https://v8.dev/blog/jspi-newapi) . Detect support cleanly:


```text
if (  '  Suspending  '   in   WebAssembly  ) {
// JSPI is available — wire up Suspending / promising
} else {
// fall back to an Asyncify-built module
}
```


Check[WebAssembly.Suspending](https://developer.mozilla.org/en-US/docs/WebAssembly/Reference/JavaScript_interface) , not` Suspender` : the old API will continue to operate at least until October 29, 2024 (Chrome M128). After that, we plan on removing the old API. Note that Emscripten itself will no longer support the old API as of version 3.1.61. An earlier` Suspender` -object API existed and was removed — if a tutorial shows` WebAssembly.Suspender` or` new WebAssembly.Function(...)` with` returnPromiseOnSuspend` , it is out of date.


## JSPI vs Asyncify, briefly


The decisive difference is where the suspension logic lives. Asyncify puts it in your binary; JSPI puts it in the engine. Because the mechanisms used when suspending and resuming WebAssembly modules are[essentially constant time](https://v8.dev/blog/jspi) , we don’t anticipate high costs in using JSPI — especially compared to other transformation based approaches. That means smaller output and lower per-call overhead, with native stack-switching instead of a whole-program rewrite. The current implementation allocates fixed-size stacks per suspended computation;[growable (segmented) stacks](https://v8.dev/blog/jspi-newapi) are on the roadmap to support large numbers of coroutines, not yet shipped.


If you compile to Wasm and have hit the wall where synchronous code needs an async Web API, JSPI is the current answer: wrap the import in` WebAssembly.Suspending` , wrap the export in` WebAssembly.promising` , guard with` 'Suspending' in WebAssembly` , and keep Asyncify only as a fallback for engines that haven’t shipped it yet.


## FAQs


What is the difference between JSPI and Asyncify?


Asyncify is a whole-program Binaryen transform that rewrites the entire Wasm binary to unwind and rewind its own stack into linear memory, inflating binary size and adding per-call overhead to instrumented functions. JSPI moves that logic into the engine using native stack-switching, so the module keeps straight-line synchronous code with no instrumentation. V8 describes JSPI's suspend and resume mechanisms as essentially constant time, whereas Asyncify taxes the whole module.


Do I need to change my Emscripten C or C++ source to use JSPI?


No. Emscripten emits the current JSPI API automatically from version 3.1.61 onward, so most C and C++ projects need no source changes to move from the old Suspender-object API to the new Suspending and promising surface. You only need to build with Emscripten 3.1.61 or later; the pre-2024 API was dropped from Emscripten at that same version, so older toolchains still emit the removed surface.


What happens if the imported Promise rejects?


A rejected Promise does not return an error value to Wasm; instead JSPI propagates an exception into the suspended computation. In practice rejection is typically handled on the JavaScript side, because a language like Rust often cannot act on that thrown exception directly. The Wasm import signature may report a plain integer even though it represents a Promise, and wasm-bindgen has an open discussion about adding an explicit error-indicating type rather than settled API.


Does calling a JSPI import always suspend the module?


No. JSPI only suspends when the JavaScript import actually returns a Promise. If the imported function returns a plain synchronous value, the result passes straight through to the Wasm caller with no suspension and no trip to the event loop. This behavior is defined at the JavaScript and WebAssembly boundary, so the same wrapped import can behave synchronously or asynchronously depending on what the underlying function returns at runtime.


Open-source session replay


## Complete picture for complete understanding


Capture every clue your frontend is leaving so you can instantly get to the root cause of any issue with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
