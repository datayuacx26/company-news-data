---
schema_version: "1.0.0"
document_id: "7f6a8f5a8ae72c0827bda082c46eafb78f5082bb0075f2b63f41761b18604ec6"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/end-dual-cjs-esm-builds-nodejs/"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T08:20:17.967412+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:9d24b990251cf2437f8d66fdbbf596a652bdc7527f073e535e7076751a940e4b"
---

# The End of Dual CJS/ESM Builds in Node.js

As of June 2026, every supported Node.js release can` require()` an ES module, which removes the single reason most libraries ever shipped dual CommonJS/ESM builds — for a large and growing share of packages, ESM-only is now the default-correct choice. The asymmetry that defined a decade of packaging pain — CommonJS could` import` nothing,` require` nothing from the ESM world — no longer holds on any runtime you should still be targeting. The dual` exports` map, the parallel` tsup` /` unbuild` output, the` .d.cts` /` .d.ts` declaration juggling: most of that machinery exists to solve a problem Node has now solved in core.


This article makes the 2026 argument the older dual-build guides can’t: here is the exact version timeline where the asymmetry died, what` require(esm)` actually does and the one hard limit it carries, and a decision framework for whether you still need a CommonJS build at all. The constraint hasn’t vanished — it has moved. The new compatibility contract isn’t “ship two formats”; it’s “keep your synchronous load path free of top-level await.”


## Key Takeaways


- As of Node.js 25.4.0 (released January 19, 2026),` require(esm)` is marked stable, and the same change was backported to the active LTS lines — meaning every currently supported Node.js release ships the ability to` require()` an ES module.
- ` require(esm)` first landed behind` --experimental-require-module` in Node 22, was unflagged in Node 23, backported to LTS in v22.12.0 (December 3, 2024) and v20.19.0, and declared stable at the end of 2025.
- ` require(esm)` has exactly one hard limit: it cannot load an ES module whose graph uses top-level await, which throws` ERR_REQUIRE_ASYNC_MODULE` and tells you to use` import()` instead.
- For an ESM-only author, the first top-level` await` anywhere in your require-reachable graph is a breaking change for every CommonJS consumer — treat it as a semver-major.
- If your package targets Node 22.12+ and avoids top-level await in code CJS users will` require()` , shipping ESM-only is now the default-correct choice; keep a CJS build only for pre-20.19 runtimes or TLA modules.


## Why dual CJS/ESM builds existed in the first place


Dual builds existed because CommonJS could not` require()` an ES module. The two systems load differently:` require()` is synchronous and returns` module.exports` the moment the call completes, while ESM was treated as unconditionally asynchronous. A synchronous caller cannot wait on an asynchronous load, so` require('some-esm-package')` threw` ERR_REQUIRE_ESM` . The reverse direction always worked — ESM can` import` CommonJS — which produced the lopsided situation library authors lived with for years: ship ESM for modern consumers, ship CommonJS for everyone still calling` require()` , and wire both through conditional` exports` .


That meant real tooling overhead. Bundlers like[tsup](https://tsup.egoist.dev/) and[unbuild](https://github.com/unjs/unbuild) emit both formats; a` package.json`` exports` map routes` import` to the` .mjs` entry and` require` to the` .cjs` one; TypeScript needs colocated` .d.ts` and` .d.cts` declarations so both resolution modes typecheck. Anthony Fu’s[2021 dual-build guide](https://antfu.me/posts/publish-esm-and-cjs) and Mayank’s[2023 walkthrough](https://mayank.co/blog/dual-packages/) document this machinery in depth — and both are still accurate about *how* to do it. They’re just answering a question that, for current runtimes, no longer needs asking.


Dual builds also carried a structural risk: the **dual-package hazard** . When a dependency graph loads your package through` import` in one place and` require` in another, Node can load two separate copies — the ESM build and the CJS build — as distinct module instances. Any singleton, cache, registry, or` instanceof` check then sees two divergent states. The dual build that solved the interop problem quietly created a state-duplication problem.


## require(esm): the exact versions where the asymmetry died


The fix came from a correction of a long-held assumption. As Node core contributor Joyee Cheung[documented](https://joyeecheung.github.io/blog/2024/03/18/require-esm-in-node-js/) , ESM itself was not actually designed to be unconditionally asynchronous — rather, it was designed to be only conditionally asynchronous, only when the graph contains top-level await, so it would seem natural for` require()` to at least support ESM graphs that contain no top-level await. That insight made a synchronous` require()` of (most) ES modules possible, and` require(esm)` was built on it.


The rollout happened in stages across release lines. Here is the timeline as of June 2026:


Node.js line` require(esm)` status Support phase (June 2026)


18.x Never received the backport EOL — must move to 20+


20.x Unflagged at **v20.19.0** EOL April 30, 2026


22.x On by default at **v22.12.0** (Dec 3, 2024) Maintenance LTS


23.x Unflagged (non-LTS) EOL


24.x Stable marking backported at **v24.15.0** (Apr 15, 2026) Active LTS


25.x **Marked stable at v25.4.0** (Jan 19, 2026) EOL June 1, 2026


26.x Stable Current


The headline: in the[v25.4.0 release](https://nodejs.org/en/blog/release/v25.4.0) , the change “module: mark require(esm) as stable” (PR #60959) removed the experimental marker, and that same commit was backported into the LTS line at[v24.15.0](https://nodejs.org/en/blog/release/v24.15.0) . The feature had been unflagged-by-default well before stabilization:[Node 22.12.0 was the first LTS release with it on by default](https://socket.dev/blog/require-esm-backported-to-node-js-20) , and it was[backported to Node 20 at v20.19.0](https://joyeecheung.github.io/blog/2025/12/30/require-esm-in-node-js-from-experiment-to-stability/) . Node 18 never got the backport.


Per the[Node.js release schedule](https://endoflife.date/nodejs) , the supported lines in June 2026 are 22 (Maintenance LTS), 24 (Active LTS, active support until October 20, 2026, then security maintenance through April 30, 2028), and 26 (Current). All three sit above the unflag threshold. With Node 18 never backported and Node 20 having reached end-of-life on April 30, 2026, the lowest version any supported project should target already includes` require(esm)` .


## What require(esm) changes for library authors


A CommonJS consumer on a current Node version can now` require()` an ESM-only package directly. The original justification for shipping a CJS build — that` require()` callers would otherwise be locked out — no longer holds on any supported runtime. As the[Node.js documentation](https://nodejs.org/api/modules.html) describes, if the ES module being loaded meets the requirements,` require()` can load it and return the module namespace object; in this case it is similar to dynamic` import()` but is run synchronously and returns the namespace object directly.


This also retires the dual-package hazard. Because a CommonJS caller now loads the *actual* ES module instead of a parallel CJS copy, there is one module instance, one singleton, one cache — the divergent-state problem that justified careful dual builds simply doesn’t arise when there’s only one build.


One interop detail matters when you drop the CJS wrapper.` require(esm)` returns a namespace object, not a bare value, so a default export lands on` .default` rather than being the return value itself, similar to the results returned by` import()` . If you want a CommonJS-style single return value, the ES module can export the desired value using the string name` "module.exports"` to customize what` require(esm)` returns directly.


You can detect support at runtime when you need a fallback path by checking whether` process.features.require_module` is` true` .


```text
// Runtime feature detection — true on Node 20.19+, 22.12+, and all of 24/26.
if (  process  .  features  .  require_module  ) {
const   lib   =   require  (  "  some-esm-only-package  "  );
// default export is on .default
const   fn   =   lib  .  default   ??   lib  ;
}
```


## The one limit: top-level await is the new compatibility contract


` require(esm)` has exactly one hard limit: it cannot load an ES module whose graph uses top-level await. Because` require()` must stay synchronous, an ESM file that pauses its own evaluation on a top-level` await` cannot be loaded this way. If the module being` require()` ’d contains top-level await, or the module graph it imports contains top-level await,[ERR_REQUIRE_ASYNC_MODULE](https://nodejs.org/api/modules.html) will be thrown, and users should load the asynchronous module using` import()` instead. The thrown message is explicit: “require() cannot be used on an ESM graph with top-level await. Use import() instead.”


The critical word is *graph* . The limit isn’t about the file you require — it’s about everything that file transitively imports.


A real, dated incident shows the blast radius. In April 2026,[lru-cache@11.3.0 introduced a top-level await in its ESM build](https://github.com/isaacs/node-lru-cache/issues/397) , which broke any CJS module that transitively loaded the ESM build of lru-cache, most notably jsdom via @asamuzakjp/css-color (which is pure ESM with no CJS entrypoint). The chain ran jsdom (CJS) → a pure-ESM color package → lru-cache’s now-async ESM entry. The exports map correctly routed` require` to CJS and` import` to ESM; but when the CJS package required a pure-ESM package, Node resolved the ESM graph, and within that graph lru-cache’s ESM entrypoint — now containing TLA — made the entire graph impossible to` require()` synchronously. The maintainer reverted the top-level await in a subsequent patch, so the breakage is resolved — but it proves the failure mode bites in production. The same` ERR_REQUIRE_ASYNC_MODULE` cascade hit[Prettier](https://github.com/prettier/prettier/issues/16930) and[firebase-tools](https://github.com/firebase/firebase-tools/issues/8054) when Node 22.12.0 turned the feature on.


` require(esm)` reframes the whole problem: it removes the interop reason for dual builds, but it makes TLA-freedom a contract. For an ESM-only author, the first top-level` await` you add anywhere in your require-reachable graph is a breaking change for every CommonJS consumer. As Evert Pot[argues](https://evertpot.com/using-top-level-await-is-bc-break/) , if it’s the first await you might inadvertently break Node.js users who used require() to bring in your module — meaning the first top-level await in your project or any of your dependencies might now constitute a new major version if you follow semver. Treat it as a semver-major.


Top-level await is genuinely uncommon in library code. When Cheung[first tested the implementation](https://joyeecheung.github.io/blog/2024/03/18/require-esm-in-node-js/) , none of the ~30 high-impact ESM-only packages tested contained top-level await — which is why synchronous` require(esm)` covers the overwhelming majority of real packages.


## Do you still need a CJS build in 2026?


For most new packages, no. Default to ESM-only and reach for a dual build only when a specific constraint forces it. Branch on three questions:


1. **What’s your minimum Node target?** If it’s Node 22.12+ (and with Node 20 now EOL, it should be), every consumer can` require()` your ESM. Ship ESM-only. If you genuinely must support pre-20.19 runtimes still in the wild, you still need a CJS build for them.
2. **Does your require-reachable graph use top-level await?** If yes — in your code or a synchronously-loaded dependency — CJS consumers will hit` ERR_REQUIRE_ASYNC_MODULE` . Either remove the TLA (often a lazy` import()` instead of a top-level one), or keep a CJS entry and document that` require()` users are unsupported.
3. **Do you control your consumers?** Application authors on a pinned current Node can go ESM-only freely. Library authors with unknown downstream consumers should still publish a clean` exports` map and treat TLA as a versioning event.


If none of these forces a second format, the dual build is dead weight: extra tooling, slower CI, a larger published artifact, and a reintroduced dual-package hazard for zero benefit.


## Migrating to ESM-only: the checklist


Going ESM-only is mostly a` package.json` simplification plus disciplined module syntax. The steps:


1. **Set` "type": "module"`** so` .js` files are parsed as ESM.
2. **Collapse the` exports` map** to a single ESM entry. The dual map becomes one line:


```text
{
"  type  "  :   "  module  "  ,
"  exports  "  :   "  ./dist/index.js  "  ,
"  engines  "  : {   "  node  "  :   "  >=22.12.0  "   }
}
```


The retrospective’s recommended` engines` value is` "^20.19.0 || >=22.12.0"` ; since Node 20 is EOL,` >=22.12.0` alone is defensible.


1. **Use explicit` .js` extensions** in relative imports — ESM requires them:` import { x } from "./util.js"` , not` "./util"` .
2. **Set` "moduleResolution": "NodeNext"`** in` tsconfig.json` so TypeScript emits and resolves ESM correctly, including the mandatory extensions.
3. **Replace CommonJS globals.** ESM has no` __dirname` ,` __filename` , or` require` . Reconstruct them from` import.meta` :


```text
import   {   fileURLToPath   }   from   "  node:url  "  ;
import   {   dirname   }   from   "  node:path  "  ;


const   __filename   =   fileURLToPath  (  import  .  meta  .  url  );
const   __dirname   =   dirname  (  __filename  );
```


1. **Audit for top-level await** across your own code and dependencies before publishing. If you intend to use TLA later, plan the major version bump now rather than shipping it as a patch.


## Where this leaves library authors


The interop wall that justified dual CJS/ESM builds is gone on every Node.js version worth supporting:` require(esm)` is stable as of v25.4.0 and present across the 22, 24, and 26 lines. The remaining constraint is narrow and nameable — keep top-level await out of the path a` require()` caller will traverse, and treat your first one as a breaking change. For a new package targeting current Node, ship ESM-only, collapse the` exports` map, and audit your graph for TLA before you publish.


## FAQs


Can I require an ESM-only package on Node.js 22?


Yes. Node 22 enabled require(esm) by default starting at v22.12.0, released December 3, 2024, so a CommonJS file running on any 22.12 or later version can require() an ESM-only package directly, provided that package's graph contains no top-level await. The feature was later marked stable in Node 25.4.0 and backported to the 24.x LTS line at v24.15.0, but it has been functional on Node 22 since the v22.12.0 release.


What is the difference between ERR_REQUIRE_ESM and ERR_REQUIRE_ASYNC_MODULE?


ERR_REQUIRE_ESM was the old error thrown whenever CommonJS tried to require() any ES module, and it no longer occurs on supported Node versions because require(esm) handles synchronous ESM loading. ERR_REQUIRE_ASYNC_MODULE is the narrower modern error thrown only when the required ESM graph contains top-level await, since require() cannot wait on an asynchronous evaluation. Its message instructs you to use import() instead. The first error meant ESM was unsupported; the second means one specific ESM feature is.


Does require(esm) return the default export directly?


No. require(esm) returns the full module namespace object, not a bare value, so a default export lands on the .default property rather than being the return value itself, matching the behavior of dynamic import(). This differs from a traditional CommonJS module where require() returns module.exports directly. If you need a single return value, an ES module can export it using the string name 'module.exports', which customizes what require(esm) returns. Always check for .default when migrating consumers off a CJS wrapper.


How do I check at runtime whether require(esm) is available?


Check whether process.features.require_module is true. This boolean is set by the Node.js runtime and returns true on every version that supports requiring ES modules, which includes Node 20.19 and later, 22.12 and later, and all of the 24 and 26 lines. Use it to branch between a synchronous require() and an asynchronous import() fallback when you must support a mix of older and newer runtimes within the same codebase.


Is shipping ESM-only safe if my dependencies use top-level await?


Not for CommonJS consumers. The require(esm) limit applies to the entire require-reachable graph, not just your own files, so a top-level await anywhere in a synchronously loaded dependency will throw ERR_REQUIRE_ASYNC_MODULE for anyone using require(). A documented 2026 incident saw lru-cache add top-level await to its ESM build and break jsdom transitively before the maintainer reverted it. Audit your full dependency graph before going ESM-only, or keep a CJS entry and mark require() users unsupported.


Open-source session replay


## Complete picture for complete understanding


Capture every clue your frontend is leaving so you can instantly get to the root cause of any issue with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
