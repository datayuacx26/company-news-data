---
schema_version: "1.0.0"
document_id: "8050c6bfe9c5ff15d619091d20ac5a70c9ac1646e3ec9fa9901219615af5e3a3"
company_key: "yc-retool"
company: "Retool"
source_id: "yc-retool-news-import-762698831de2"
canonical_url: "https://retool.com/blog/vitejs-migration"
published_at: "2025-04-16T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:05.217948+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:2e855c03ad790b165724bb3d1dd498de908779b3f71a1ab7ce258db333c5d0c7"
---

# How we migrated to ViteJS

01 Fully migrating to ESM02 Our biggest unexpected challenge: changes to the bundling algorithm03 Feature flagging the entire frontend04 Impact and looking ahead


Retool is the best software development solution for building mission-critical business apps.[Frontend development](https://retool.com/blog/frontend-dilemma) moves fast, and we invest heavily in our internal tech stack to provide the best experience for our builders. Keeping our internal tech stack on the cutting edge also enables our own engineering team to be as productive and creative as possible. To that end, we recently completed a large-scale migration to ViteJS as our core frontend development library and bundler.


ViteJS is a relatively new technology, but it’s quickly risen in popularity and has been voted the best frontend library in the State of JavaScript two years in a row ([2023](https://2023.stateofjs.com/en-US/libraries/#tier_list) and[2024](https://2024.stateofjs.com/en-US/libraries/#tier_list) ). The philosophy and motivations behind the ViteJS project are nicely presented in the[“Why ViteJS”](https://vite.dev/guide/why) article. Web development hasn’t felt this fast and responsive since the days before NodeJS+NPM introduced slow compilation steps into the development flow.


We’d been building with Webpack since Retool was founded. We still think Webpack is a fantastic library—it’s more battle-tested than ViteJS (and, in some cases, still performs better). However, we needed three main things that forced us to consider new libraries:


- Improved production bundling times
- Improved local development productivity with faster hot reloading
- First class treatment of modern web development paradigms (ESM)


We identified ViteJS as the most promising solution because it addressed all of these concerns, had the best community support, and a strong roadmap ([Rolldown](https://github.com/vitejs/rolldown-vite/pull/84) ). In a few short days we built a quick prototype to de-risk the investment, identify next steps, and build consensus internally. Over the following six months, we worked diligently and collaboratively to perform one of the largest migrations in our history.


## Fully migrating to ESM


The first challenge was a “pre-factor” of the entire frontend codebase to migrate any CommonJS code to ECMAScript. ViteJS will not work directly with CommonJS code (having CJS code in node_modules is okay, though).


For the most part, this was a straightforward find and replace of CommonJS syntax with ESM syntax, for example replacing` require()` with` import` and` module.exports` with` export` . At the end of this exercise the goal was to be able to set` ”type”: “module”` in our[package.json file](https://nodejs.org/api/packages.html#type) .


Semantic differences caused a bit of a headache in some places. For example, replacing` require()` with` import` in a function requires changing the function to be async (which could have ripple effects and necessitate a bigger refactor.) Or if the` require()` is done conditionally at the module scope, it can only be replaced with[Top Level Await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await#top_level_await) , which Jest does not support.


In general, Jest did not support ESM and as of this writing, still only has[experimental support](https://jestjs.io/docs/29.5/ecmascript-modules) (Jest is waiting on better support from NodeJS). This created the tedious requirement that our frontend codebase needed to be compliant with both CommonJS (for Jest/NodeJS) and ESM (for ViteJS).


To work around the Top Level Await constraint we used a custom Jest[transformer](https://jestjs.io/docs/code-transformation) to replace a top-level` await import()` with a` require()` statement. While not a functionally identical transformation, it was good enough for test code.


During this phase several third-party NPM packages were problematic because of[conditional exports](https://nodejs.org/api/packages.html#conditional-exports) which changed how the packages behaved under CommonJS vs. ESM. We often updated the package to resolve this, as newer versions were likely to have better dual CJS / ESM support. Other times we had to tweak` default` imports since this is a[major source of incompatibility](https://www.typescriptlang.org/docs/handbook/modules/appendices/esm-cjs-interop.html) between CJS and ESM.


Older third-party packages also commonly assumed a NodeJS environment and used builtins like` process` and` require(‘os’)` . Again, we typically fixed this by upgrading the package (using NodeJS[polyfills](https://www.npmjs.com/package/rollup-plugin-polyfill-node) is another option.)


## Our biggest unexpected challenge: changes to the bundling algorithm


The biggest unexpected challenge was that the ViteJS bundler (which uses Rollup for bundling) created a larger bundle with more chunks than with Webpack. This means the browser had to download more JS to render the same UI. If we had shipped this it would have immediately caused a significant regression in page load times, so we needed to understand and fix the problem.


ViteJS/Rollup has a much simpler chunking algorithm compared to Webpack. Unlike Webpack (and its SplitChunkPlugin), Rollup does not duplicate code, which makes it more difficult to avoid bundle bloat.


A chunk is the final rendered JS file that the user downloads. It can be a combination of one or more original source files from your codebase. Files need to be combined together because loading them individually would be overwhelming for the browser. Different source files need to be colocated in the same chunk but they might not all be related. Bundle bloat happens when you download a chunk because you need one of the files but don’t end up using the others.


In Rollup, since a code module can only be included in one chunk, there is a higher chance it will be colocated with unrelated code that’s not used. In Webpack the source module will be duplicated in order to avoid this problem. The downside to the Webpack approach is that too much duplication can also lead to bundle bloat. For example a common library like lodash might get duplicated a lot because it is used in many unrelated sections of the codebase. When Webpack duplicates a library like this, your users still have to download and evaluate that duplicated code (a less than perfect approach).


Fixing the ViteJS bundle bloat ultimately required a deeper understanding of the Rollup chunking algorithm to determine the changes we could make to our codebase to improve bundle efficiency. Thankfully, the Rollup bundling algorithm is *much* easier to understand than the Webpack algorithm (this[block comment](https://github.com/rollup/rollup/blob/c4bb050938778bcbe7b3b3ea3419f7fa70d60f5b/src/utils/chunkAssignment.ts#L152) in particular is perfect).


After studying the algorithm, we identified several changes to reduce bundle bloat. The first was to remove any usage of dynamic` import()` that was called unconditionally. Each dynamic` import()` statement creates a new chunk candidate for Rollup to consider. Unless the code being imported is in a conditional block, it doesn’t need to be a dynamic import and should be statically imported.


The second tweak we made by identifying dynamic imports that import other code that almost always gets imported. We built a static analysis tool to quantify source tree overlap from different` import()` entry points. When an entry point would import a source subtree that had high overlap with its parent (e.g. over 90%), then it would be a good candidate to change into a static import. Effectively we’re saying, “I’m okay with importing an extra 10% of non-overlapping code because it will reduce the number of chunks.” And when this extra 10% is in the most visited part of your app, you are optimizing the bundle for the happy path which is an easy tradeoff to make.


With these tweaks made, we eventually got to a point where the total bundle download size for our most visited UI (the app viewer page), was smaller than what it had been with Webpack.


## Feature flagging the entire frontend


In the final stretch of the migration we faced a completely new problem: how to confidently deploy a change of this nature. We reached a point where the Retool app was functionally identical when built with either ViteJS or Webpack. However, under the hood, the generated JS bundles were very different. This was where the uncertainty and risk originated; it was difficult to predict how an innumerable amount of end users’ browsers would react to this entirely new bundle of JS code.


We deployed the migration in phases, as we do with most large feature releases. We started with an internal[dog-fooding](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) release for a few weeks, followed by a slow incremental roll-out to production users over a month. During this time we relied on our robust observability framework to detect issues and rollback if necessary.


And the release went better than expected! On the first day we caught one minor issue via observability. Once that was fixed, we began the rollout process and didn’t encounter any further issues that necessitated a rollback.


The novel technical challenge with the rollout was how to use client-side feature flags to determine which JS artifact to download. Our existing feature flag framework was robust and reliable, but feature flag values don’t exist until the JS is already downloaded and running in the client browser. So we’re left with a chicken-and-egg problem: how can you check the feature flag to determine which bundle variant to download, when you need to download the bundle to check the feature flag?


The solution was to load the default variant (Webpack), check the feature flag, set a cookie, then on subsequent requests the web server used that cookie to determine which variant to serve. We used a combination of the[cookie embedded variable](https://nginx.org/en/docs/http/ngx_http_core_module.html#var_cookie_) and the[map](https://nginx.org/en/docs/http/ngx_http_map_module.html) construct in Nginx to achieve this. The request flow isn’t perfect since it required an initial bootstrap request, but it wasn’t perceivable and was good enough considering we could reuse our existing feature flag system and didn’t have to build a sophisticated solution to handle this one-off project.


## Impact and looking ahead


We set out on this project intending to address three major pain points in our frontend dev stack, so how did we do?


- Improve production bundling times
- Improve local development productivity with faster hot reloading
- Upgrade to modern web development paradigms (ESM)


We saw the biggest impact with item two:[improved developer productivity](https://retool.com/blog/how-to-improve-developer-productivity) . Our frontend dev server now starts instantly and most changes hot reload instantly. The changes percolate faster than it takes your eye to move from code editor to web browser. This was a significant improvement from Webpack, where the dev server would take between 30 seconds and two minutes to start and 10 seconds or more to rebuild on change.


We asked our engineers how they felt about ViteJS in one of our quarterly developer experience surveys and feedback was positive. They’d certainly noticed the improvement and preferred it over Webpack.


We got some critical feedback about slow initial app loads. Indeed, this is a weakness of ViteJS—there’s no bundling phase (which makes it fast to start serving files), so the browser has to download each source file individually. When you have a large codebase, this can overwhelm the browser and take a few seconds to fully load. This is a known limitation and something the Vite team is thinking about addressing. But even with this issue, the Retool app was still faster to initial load than it was with Webpack.


Unfortunately we did not see any improvement with the first item: production bundling times. ViteJS/Rollup production mode bundling was just as slow as Webpack for our application (each one taking about four minutes). In retrospect, this shouldn’t have been surprising. Both tools implement bundling in JavaScript, which means you incur a performance overhead for using a high level language (although V8 is incredibly fast once JIT compilation kicks in.) Luckily, the Vite team had already identified this and has been working dutifully on building a faster bundler called Rolldown.


Looking ahead, we're now in a strong position to navigate upcoming trends in frontend development. Our developers are more productive, our codebase is cleaner, and we learned a lot about the current state of frontend development. We might have even learned a little too much (enough to form some spicy opinions). But whatever your opinions about frontend development may be, we think you’ll agree Good Software™ is all that matters. And building Good Software in Retool should be much smoother and easier now.


light Reader
