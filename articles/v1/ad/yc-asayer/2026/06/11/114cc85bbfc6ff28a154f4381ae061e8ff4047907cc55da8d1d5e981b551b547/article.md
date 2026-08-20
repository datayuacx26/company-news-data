---
schema_version: "1.0.0"
document_id: "114cc85bbfc6ff28a154f4381ae061e8ff4047907cc55da8d1d5e981b551b547"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/using-polyfills-2026/"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-25T17:38:53.548823+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:d5c59ed8687b8bb3e409d9ebb3b394505ac2e913db23550ede57c6469eb010b4"
---

# Is Anyone Still Using Polyfills in 2026?

Yes — but almost none of the ones your bundler inherited. The blanket-polyfill default that made sense in 2019, shipping` @babel/preset-env` with` useBuiltIns: 'entry'` against a broad Browserslist target, now sends dead weight to the overwhelming majority of users to support a browser share that rounds to zero. A[polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill) is still the right tool for a short, named list of genuinely unsupported features in 2026 — and a liability for everything else.


This article resolves a single decision: whether the polyfills in your current build pipeline still earn their bytes. It names which APIs are dead weight, which still legitimately need a polyfill, what the polyfill.io supply-chain incident changed about how you load them, and the exact commands to audit what you’re actually shipping.


## Key Takeaways


- For a React, Vue, Svelte, or Next.js app targeting evergreen browsers in 2026, the correct number of polyfills for most teams is close to zero — with named exceptions for Decorators, Temporal during its cross-browser transition, and locked enterprise browser constraints.
- ` Array.flat` ,` Object.entries` ,` Promise.allSettled` ,` structuredClone` , optional chaining, nullish coalescing, and` fetch` are all broadly supported in 2026; polyfilling them ships bytes that serve almost no real user.
- The remote-polyfill-service pattern (loading a script from polyfill.io) is dead after the 2024 supply-chain incident; Cloudflare and Fastly stood up mirrors, but the pattern itself is no longer defensible.
- Run` npx browserslist` to see the exact browsers your config targets, then` source-map-explorer` to find how much` core-js` weight you’re shipping.
- web.dev’s Enhancement / Additive / Critical classification is the strongest framework for deciding whether a missing feature warrants a polyfill at all.


## The 2026 native-support baseline: what you no longer need to polyfill


Most of the JavaScript features that build configs still routinely polyfill are now supported across every evergreen browser, which means polyfilling them ships bytes almost no real user needs. The features that anchored polyfill tutorials for a decade —` Math.trunc` ,` Array.prototype.flat` , optional chaining — are now broadly supported and are exactly the dead weight a 2026 audit should remove.


The genre-defining teaching example, javascript.info’s[Math.trunc polyfill](https://javascript.info/polyfills) , is the clearest illustration.` Math.trunc` has had universal browser support since 2015 — per[MDN’s compatibility data](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/trunc#browser_compatibility) , it shipped in Chrome 38, Firefox 25, and Safari 8. Writing or bundling a` Math.trunc` polyfill in 2026 is shipping a guard clause for a browser nobody runs.


The same is true across the API surface that stale configs still target. Support percentages below are sourced from[caniuse.com](https://caniuse.com/) global usage data (StatCounter, May 2026); “global support” refers to caniuse’s weighted browser share across all tracked versions.


Feature Global support (caniuse, 2026) Still worth polyfilling?


[Array.prototype.flat](https://caniuse.com/array-flat) 94.11% No


[Object.entries](https://caniuse.com/mdn-javascript_builtins_object_entries) 95.06% No


[Promise.allSettled](https://caniuse.com/mdn-javascript_builtins_promise_allsettled) 94.04% No


[structuredClone](https://caniuse.com/mdn-api_structuredclone) 93.84% No


[Optional chaining ( ?. )](https://caniuse.com/mdn-javascript_operators_optional_chaining) 93.99% No (syntax — transpile, don’t polyfill)


[Nullish coalescing ( ?? )](https://caniuse.com/mdn-javascript_operators_nullish_coalescing) 93.99% No (syntax — transpile, don’t polyfill)


[fetch](https://caniuse.com/fetch) 96.3% No


[Temporal](https://caniuse.com/temporal) 65.16% Yes — transitional


[Decorators](https://github.com/tc39/proposal-decorators) 0% native Yes


` Array.flat` ,` Object.entries` ,` Promise.allSettled` ,` structuredClone` , optional chaining, nullish coalescing, and` fetch` all sit around 94–96% global support — and if your Babel config is still polyfilling these, you are shipping bytes that serve almost no user.


> **Syntax vs. functions:** Optional chaining and nullish coalescing are *syntax* , not missing functions, so they are handled by a transpiler, not a polyfill. As javascript.info puts it: use a transpiler for modern syntax or operators, and polyfills for missing functions. The distinction matters when auditing — a` ??` “polyfill” in your config is a sign the config is conflating the two.


## Which polyfills still earn their place in 2026?


Polyfills remain the correct tool for a small, named set of cases: features with no native support anywhere, features mid-transition across browsers, and locked-browser environments where the evergreen assumption doesn’t hold. These are the exceptions that justify keeping a polyfill mechanism in your pipeline at all.


**Decorators.** The[TC39 decorators proposal](https://github.com/tc39/proposal-decorators) is at Stage 3 with no native browser implementation. If you use decorators — directly, or through a framework like Angular or a library that depends on them — you are relying on a transpiler plus, in some cases, runtime helpers. There is no “wait for native” option here yet; the feature isn’t shipping in browsers.


**Temporal, during its cross-browser transition.**[Temporal](https://tc39.es/proposal-temporal/docs/) is a finished TC39 proposal — it appears on the[TC39 finished-proposals list](https://github.com/tc39/proposals/blob/main/finished-proposals.md) — and it has begun shipping natively. Per[MDN’s Temporal compatibility data](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Temporal#browser_compatibility) , it is available in Firefox 139+, Chrome 144+, Edge 144+, and Node.js 26+, while Safari still lacks enabled support. That uneven rollout makes a Temporal polyfill a *transitional* necessity rather than a permanent one. TC39’s[proposal-temporal repository](https://github.com/tc39/proposal-temporal#polyfills) lists[@js-temporal/polyfill](https://www.npmjs.com/package/@js-temporal/polyfill) as an alpha-release option alongside alternatives — it is not the sole canonical choice.


**Enterprise and locked-browser environments.** Regulated financial systems, government intranets, and kiosk or POS devices sometimes pin a browser version for years. If your real user base includes browsers that can’t update, your Browserslist target is wider than the evergreen default and some polyfills are load-bearing. The key word is *real* — this case is asserted far more often than it’s measured. Validate it against analytics before keeping polyfills on its account.


**Limited-availability CSS features behind JS polyfills.** Some CSS capabilities, like container queries on older Safari, were polyfilled with JavaScript during their rollout. As[web.dev notes](https://web.dev/articles/baseline-and-polyfills) , the[container queries polyfill](https://github.com/GoogleChromeLabs/container-query-polyfill) uses` ResizeObserver` and` MutationObserver` to mimic native behavior — and its README marks it as no longer maintained. These polyfills carry behavioral caveats covered below.


The soft consensus from the wider commentary — “use targeted polyfills, base decisions on data” — is not wrong, but it understates how much has changed: for most apps in 2026, “polyfills continue to play an important role” is no longer true. The role is now narrow and named.


## The polyfill.io incident killed the remote-polyfill-service pattern


The remote-polyfill-service pattern — loading a` <script>` from a third-party CDN that returns browser-tailored polyfills — is no longer a defensible architecture after the 2024 polyfill.io supply-chain attack. The incident forced teams to audit what they were loading from third parties, and most found they didn’t need most of it.


The timeline:


- **Early 2024 — ownership change.** The polyfill.io domain changed hands.[Fastly’s community post](https://community.fastly.com/t/new-options-for-polyfill-io-users/2540) documents the change and Fastly’s response for affected users.
- **June 25, 2024 — malware report.** Security firm Sansec[reported](https://sansec.io/research/polyfill-supply-chain-attack) that the polyfill.io service had been injecting malware into the script it served to end users.
- **June 2024 — mirror and mitigation responses.**[Cloudflare announced](https://blog.cloudflare.com/automatically-replacing-polyfill-io-links-with-cloudflares-mirror-for-a-safer-internet/) automatic replacement of polyfill.io links with its own mirror, and Fastly offered a replacement endpoint.


The mirrors keep existing sites functioning, but they don’t rehabilitate the pattern. A standard production failure mode this exposed: teams had a` cdn.polyfill.io` script tag loading bundles of polyfills they’d stopped needing years earlier, executing third-party JavaScript on every page load to support browsers that had since updated. Don’t treat the Cloudflare or Fastly mirror as a “safe” drop-in replacement — treat the incident as the prompt to remove the script tag entirely and move any genuinely needed polyfills into your own bundle, where you control and review them.


## How to audit what your bundler is actually shipping


Auditing your polyfill footprint is a four-step sequence: read your real Browserslist target, inspect the bundle for` core-js` weight, correct the Babel config, and validate against real user data. None of this requires guessing — every step is a command or a config diff you can run today.


### Step 1: See your real target list


Browserslist queries decide which browsers your config supports, and the only reliable way to know what yours resolves to is to ask it directly:


```text
npx   browserslist
```


This prints the concrete browser-version list your current config targets. The[Browserslist defaults](https://github.com/browserslist/browserslist#full-list) (` > 0.5%, last 2 versions, Firefox ESR, not dead` ) already exclude IE11 via` not dead` . The real risk is an inherited *custom* query — a stray` > 0.2%` or a years-old explicit version floor — that quietly pulls in old targets. Run the command; if the output includes browsers no real user runs, that’s your dead weight.


### Step 2: Find the core-js weight in your bundle


[core-js](https://github.com/zloirock/core-js) is the polyfill library` @babel/preset-env` injects from, and it can be a substantial fraction of a bundle that doesn’t need it. Inspect a production build:


```text
npx   source-map-explorer   dist/assets/  *  .js
```


[source-map-explorer](https://github.com/danvk/source-map-explorer) renders a treemap of what’s in each bundle from its source maps. Look for` core-js` modules —` es.array.flat` ,` es.object.entries` ,` es.promise.all-settled` . Every one of those corresponds to a feature in the dead-weight table above. (For webpack projects,[webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) gives the same view.)


### Step 3: Correct the Babel config


The canonical config most teams inherited is incomplete. A common pattern looks like this:


```text
{
"  presets  "  : [
[  "  @babel/preset-env  "  , {
"  useBuiltIns  "  :   "  usage  "  ,
"  corejs  "  :   3
}]
]
}
```


The config above has two problems. First, there’s no` targets` field, so the config falls back to your Browserslist file — which may be the stale custom query from Step 1. Second,` corejs: 3` is unpinned; per the[core-js documentation](https://github.com/zloirock/core-js#babelpreset-env) , the version should specify a minor so` preset-env` injects polyfills matching the installed release. A 2026-appropriate version:


```text
{
"  presets  "  : [
[  "  @babel/preset-env  "  , {
"  targets  "  :   "  > 1%, last 2 versions, not dead  "  ,
"  useBuiltIns  "  :   "  usage  "  ,
"  corejs  "  :   "  3.40  "
}]
]
}
```


Pin` corejs` to the[minor version you actually have installed](https://github.com/zloirock/core-js/releases) rather than copying any number —` preset-env` uses it to decide which polyfills exist to inject.


The` useBuiltIns` value is the most consequential setting. Per the[@babel/preset-env docs](https://babeljs.io/docs/babel-preset-env#usebuiltins) :


- ` 'entry'` replaces a single` import 'core-js'` with the full set of polyfills your` targets` require — broad, and the source of most inherited bloat.
- ` 'usage'` adds polyfills only for the features your code actually references, per file. This is almost always what you want: it scopes polyfill injection to real usage instead of your entire target matrix.


### Step 4: Validate against real user data


A config is a hypothesis about your users; RUM data confirms or refutes it. web.dev recommends measuring real feature support before deciding to polyfill, and points to[RUM Insights](https://rumarchive.com/insights/#baseline) as a broad data source. The pattern there: features in the Baseline Widely available set are supported by 98% or more of users. If your analytics show a feature near that threshold, the polyfill for it is serving a slice that rounds to noise.


## When should you polyfill? The Enhancement, Additive, Critical framework


When a feature genuinely isn’t supported across your real user base,[web.dev’s Enhancement / Additive / Critical classification](https://web.dev/articles/baseline-and-polyfills) is the most useful heuristic for deciding whether to polyfill: if a missing feature is invisible to users, don’t polyfill; if it degrades gracefully, lean toward not polyfilling; only if absence breaks the experience does a polyfill earn its performance cost.


The three tiers, as web.dev defines them:


1. **Enhancement** — the feature improves the experience but its absence produces no visual change or lost functionality. Performance hints like` fetchpriority` are the example. Users on unsupporting browsers won’t notice. Don’t polyfill.
2. **Additive** — the feature may affect how a page looks or works, but not in a way that surfaces serious problems; a user might only notice by comparing browsers. If a polyfill exists, lean toward *not* using it, especially if you’re already polyfilling other things. Color functions and subgrid are examples.
3. **Critical** — absence causes a broken experience: runtime errors, broken layouts, unusable functionality. Here a polyfill (or a different approach entirely) is justified.


web.dev’s anchoring rule pairs cleanly with the support table above: if a feature is Widely available you shouldn’t reach for a polyfill — unless you have data about your users that explicitly tells you otherwise.


## Why polyfill bugs hide — and how session replay surfaces them


Polyfill bugs belong to a class that standard test suites miss entirely: they only manifest in the browser slice that triggered the polyfill, which is never the evergreen browser your CI runs against. Session replay is one of the few observability techniques that captures the actual DOM state of those users, because it records the real interaction and mutation sequence from the browser that ran the polyfill path.


Session replay surfaces three concrete divergence patterns between polyfilled and native code paths:


- **Layout-shift timing in the container-queries polyfill.** As[web.dev notes](https://web.dev/articles/baseline-and-polyfills) , the container-queries polyfill drives layout from` ResizeObserver` callbacks, which fire just before the browser paints a new frame — increasing presentation delay and affecting[Interaction to Next Paint](https://web.dev/articles/inp) . Delayed repaint timing can produce layout shifts that are only observable in the browsers that needed the polyfill.
- **Temporal timezone divergence.** During Temporal’s cross-browser transition, teams may need to test both native and polyfilled` Temporal` implementations to ensure consistent behavior across browsers. Your test suite may run against only one implementation path, while real users encounter another; replays from browsers where native and polyfilled implementations coexist can help surface those differences.
- **Focus-management gaps.** Accessibility-focused polyfills can introduce subtle keyboard-navigation and focus-management issues in browsers lacking native support — the kind of failure mode web.dev highlights more generally. A replay with keyboard-event capture shows the user tabbing through a modal backdrop, which a passing automated test against the polyfill won’t reveal.


In each case the common factor is the same: the bug lives in the user slice your dev environment never reproduces. That’s the structural reason to ship fewer polyfills — every polyfill path you remove is a divergence path you no longer have to monitor.


## Concrete actions to take this week


Reducing your polyfill footprint is a sequence of small, reversible edits, each validated against the bundle and your users. The goal is to ship native where you can, conditionally load for the genuine long tail, and drop everything else.


1. **Run` npx browserslist`** and delete any inherited custom query that targets browsers no real user runs. Falling back to a tighter, current target is the single highest-leverage change.
2. **Set` useBuiltIns: 'usage'`** and an explicit` targets` field in your` @babel/preset-env` config, and pin` corejs` to your installed minor version.
3. **Diff the bundle before and after** with` source-map-explorer` to confirm` core-js` weight actually dropped, then run your tests to verify nothing you needed disappeared.
4. **Remove any` cdn.polyfill.io` script tag** outright; move genuinely needed polyfills into your own bundle.
5. **Conditionally load the genuine exceptions.** For Temporal during its transition, load the polyfill only for browsers that need it rather than shipping it to everyone. For Decorators, rely on transpilation and runtime helpers where required. web.dev’s point that polyfills for limited-availability features should be conditionally loaded applies directly.
6. **Validate with RUM data** before and after, so the audit rests on your users, not the global average.


## Conclusion


The honest 2026 answer to whether anyone still uses polyfills is yes — but the defensible list is short and specific: Decorators, Temporal while it crosses the browser gap, and locked-browser environments you’ve actually measured. Everything else your config inherited is bytes spent on a browser share that rounds to zero, and the polyfill.io incident is the reminder that loading them carelessly carries real risk. Start with` npx browserslist` today; the gap between what it prints and the browsers your users actually run is your polyfill budget, and for most teams it’s far smaller than the config assumes.


## FAQs


What is the difference between a polyfill and a transpiler?


A transpiler rewrites modern syntax into an older equivalent at build time, while a polyfill adds a missing function or API implementation at runtime. Syntax features like optional chaining and nullish coalescing are handled by a transpiler because they are language operators, not callable functions. APIs like Promise.allSettled or structuredClone are handled by polyfills because they are missing methods that runtime code can supply. A nullish coalescing 'polyfill' in your config signals the two have been conflated.


What is the difference between useBuiltIns 'usage' and 'entry' in babel-preset-env?


With useBuiltIns set to 'entry', Babel replaces a single core-js import with the full set of polyfills your targets require, which is the source of most inherited bundle bloat. With 'usage', Babel injects polyfills only for the features your code actually references, scoped per file. The 'usage' value is almost always preferable because it ships polyfills for real usage instead of your entire target matrix. Both require a corejs version pinned to your installed minor release.


Is it safe to keep using polyfill.io through the Cloudflare or Fastly mirror?


The Cloudflare and Fastly mirrors keep existing sites functioning, but they do not rehabilitate the remote-polyfill-service pattern. After the polyfill.io domain changed ownership in early 2024 and Sansec reported on June 25, 2024 that the service was injecting malware, loading browser-tailored polyfills from a third-party CDN became indefensible for security-conscious teams. Remove the cdn.polyfill.io script tag entirely and move any genuinely needed polyfills into your own bundle, where you control and review them.


Do I still need core-js if my app targets only evergreen browsers?


For most evergreen-targeting apps in 2026, core-js ships close to zero useful polyfills, because features like Array.flat, Object.entries, Promise.allSettled, structuredClone, and fetch are already broadly supported. The named exceptions are Decorators, which has no native browser implementation, and Temporal during its uneven cross-browser rollout. Run source-map-explorer against a production build to see which core-js modules are present, then tighten your Browserslist target and switch useBuiltIns to 'usage' to drop the dead weight.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
