---
schema_version: "1.0.0"
document_id: "cf7f2cda2209df577f33f71730cf79ebaea8548af69ecea09c72917a40da983a"
company_key: "yc-zepto"
company: "Zepto"
source_id: "yc-zepto-rss-dc680377f8f2"
canonical_url: "https://blog.zepto.com/build-at-zepto-speed-how-we-cut-frontend-webpack-build-times-by-95-676440fd7963"
published_at: "2026-07-07T07:25:22+00:00"
first_seen_at: "2026-08-10T05:06:20.120332+00:00"
fetched_at: "2026-08-10T05:06:23.353340+00:00"
content_hash: "sha256:d24ad33aac92403e1ef568ab808acedb62be9e595cc81f859216ffaa2e702165"
---

# Build at Zepto Speed: How We Cut Frontend Webpack Build Times by 95%

# Build at Zepto Speed: How We Cut Frontend Webpack Build Times by 95%


[Zepto Tech](https://medium.com/@tech.culture?source=post_page---byline--676440fd7963---------------------------------------)


9 min read


·


Jul 7, 2026


--


Press enter or click to view image in full size


When frontend build times start climbing, the industry playbook is usually the same: migrate to a faster bundler.


We faced the same pressure. Build times across our Micro-Frontend platform had grown beyond 20 minutes, and Webpack seemed like the obvious culprit.


Before committing to a migration, we asked a simple question: **Where is the build pipeline actually spending its time?**


The answer challenged several assumptions and ultimately helped us reduce build times from 20 minutes to 2 minutes — without replacing Webpack.


While there is already extensive coverage of bundler migrations and build optimisation, this article focuses on a large-scale MFE platform running on Docker-based infrastructure, where build performance depends on much more than the bundler alone. Alongside production profiling, we also consolidated publicly available benchmarks for loaders, plugins, minifiers, and package managers into a single analysis, providing a practical reference for evaluating build-performance tradeoffs.


## Why Build Times Became a Problem


In a typical month, over 600 pull requests from 10+ engineers generated more than 5,000 CI runs across our frontend platform. As engineering velocity increased, 20-minute build times evolved from a developer inconvenience into a platform-wide bottleneck — slowing feedback loops, creating CI queue contention, and delaying production fixes. At this scale, the build pipeline consumed more than **1,600 hours of CI time every month** , making build performance a critical engineering productivity concern.


Our frontend monorepo compounded the challenge at scale:


```text
20 production apps: React and Next.js applications  7 shared libraries  1 centralized E2E test suite  ~3,950 TSX/JSX files and 6,600+ TypeScript source files  ~350 reusable component modules  144 npm dependencies (74 production, 70 development)  Workspace: single hoisted monorepo  Team: 10+ engineers, 600+ PRs/month  CI: 5,000+ build runs/month
```


## Why We Chose Profiling Over Migration


With build times exceeding 20 minutes, migrating to a faster bundler seemed like the obvious fix — but across 20 production apps, 7 shared libraries, and 144 dependencies, the risk was significant. A single regression in a shared library could cascade across the entire monorepo, and re-validating every build output could consume weeks with no guaranteed payoff.


Instead, we profiled the entire pipeline first. The results revealed major bottlenecks *outside* the bundler — in type-checking, test orchestration, and dependency resolution — areas no bundler migration would have touched.


## Measure Before You Optimise


One of the most common mistakes in performance engineering is optimising before measuring.


Modern frontend build systems are complex. A Webpack build passes through multiple stages before producing a deployable artifact:


- Module compilation
- Loader execution
- Chunk generation
- Asset optimisation
- Minification
- Asset emission


Without visibility into those phases, every optimisation becomes an educated guess.


Before changing a single loader, plugin, or dependency, we focused on understanding where the build pipeline was actually spending its time.


## Using Webpack’s ProgressPlugin


We started with Webpack’s built-in ProgressPlugin.


```text
const webpack = require('webpack');
```


```text
plugins: [    new webpack.ProgressPlugin()  ]
```


The output quickly revealed something interesting:


```text
[webpack] 92% sealing > asset processing  TerserPlugin took 825.31s
```


More than **13 minutes** were being spent inside a single plugin.


To put that into perspective, the rest of the optimisation stage looked like this:


```text
copy-webpack-plugin      30ms  WriteIndexHtmlPlugin     22ms  RealContentHashPlugin    58ms  CompressionPlugin        70ms  LicenseWebpackPlugin     1.15s  TerserPlugin             825.31s
```


At that moment, one thing became obvious:


**Webpack wasn’t the bottleneck.**


JavaScript minification was.


That discovery fundamentally changed the direction of the optimization effort.


## Going Deeper with Speed Measure Plugin


The` ProgressPlugin` helped us identify slow build stages. Next, we wanted loader-level visibility to catch exactly what was holding back our compilation. To achieve that, we integrated **Speed Measure Plugin (SMP)** .


```text
const SpeedMeasurePlugin = require('speed-measure-webpack-plugin');  const smp = new SpeedMeasurePlugin();  module.exports = smp.wrap(config);
```


By wrapping our configuration, SMP gave us a granular, step-by-step breakdown of how long each loader chain took to process modules. This allowed us to explicitly benchmark our pipeline **Before** and **After** introducing modern compilers like SWC.


However, the data also gave us an objective reality check:


- **CSS processing remains expensive:** The chain containing` mini-css-extract-plugin` ,` css-loader` , and` postcss-loader` actually ticked up from **8.66 seconds** to **9.36 seconds** .
- **The Culprit Found:** CSS and un-loaded modules accounted for nearly **16 seconds** of our remaining 20.78-second general output time.


Instead of debating tooling preferences blindly, we now had hard data pointing exactly to where we needed to slice out latency next.


## Defining the Optimisation Strategy


Before implementing changes, we established a set of engineering objectives.


## Build Performance


- Reduce cold build latency
- Improve incremental build performance
- Reduce transpilation overhead
- Reduce minification overhead
- Improve CPU utilisation


## CI Efficiency


- Maximize Docker cache reuse
- Reduce dependency installation time
- Improve pipeline determinism


## Platform Stability


- Maintain enterprise-grade tooling stability
- Avoid high-risk migrations
- Preserve ecosystem compatibility
- Balance performance against long-term maintainability


These objectives shaped every technology decision that followed.


## Why We Didn’t Migrate to Vite


We’d be lying if we said we didn’t look at Vite. Everyone looks at Vite.


It’s genuinely excellent software. For greenfield projects or simpler setups, it’s often the right call. But we did something the hype cycle rarely encourages: we benchmarked it against our *actual* pipeline, not the synthetic toy examples in most migration guides.


The finding was uncomfortable. The 13+ minutes of Terser time we’d just discovered? It would have followed us to Vite. Minification is minification regardless of bundler. We’d have spent four weeks migrating and arrived at roughly the same problem, just with a different config syntax and a busted plugin ecosystem to untangle.


We noted Vite in our decision log — *revisit if bottlenecks shift to compilation* — and moved on.


We looked at **Rspack** too, a Rust-based bundler with Webpack-compatible config that’s genuinely exciting. But recent security disclosures and an ecosystem that’s still finding its feet made us nervous.
Rspack remains on our radar and will be reevaluated as the ecosystem matures.


## Modernising the Webpack Pipeline


Once the strategic decisions were made, the next step was replacing the slowest parts of the pipeline.


## Replacing Babel with SWC


JavaScript and TypeScript transpilation represented one of the largest optimisation opportunities.


We replaced Babel with SWC (Speedy Web Compiler), a Rust-based compiler built for high-throughput JS/TS transformation.


```text
{    test: /\.(jsx?|tsx?)$/,    use: [      'thread-loader',      {        loader: 'swc-loader'      }    ]  }
```


Public benchmarks demonstrate the scale of the improvement.


Press enter or click to view image in full size


The difference is difficult to ignore.


Benefits included:


- Dramatically faster transpilation
- Parallel compilation through thread-loader
- Lower CPU blocking
- Faster CI execution


## Eliminating the Biggest Bottleneck with esbuild


The profiling exercise had already identified the primary offender:


Terser.


## Get Zepto Tech’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


To address this, we adopted esbuild for JavaScript minification.


```text
new EsbuildPlugin({    target: 'es2015',    minify: true  })
```


Before standardising on **esbuild** , we leveraged the **minification benchmarks from the**[privatenumber/minification-benchmarks](https://github.com/privatenumber/minification-benchmarks) **project** — which compares esbuild, terser, swc, uglify-js, and other minifiers across **minified size, gzipped size, and minification time** .


Press enter or click to view image in full size


The results showed:


- **esbuild delivered competitive minified and gzipped sizes** — slightly larger than the best-in-class but within ∼5–8 % of smallest results.
- **Minification time for esbuild (~295 ms)** was dramatically faster than heavy tools like **terser (~6.7 s)** , which becomes costly at monorepo scale.
- While tools like **@swc/core and oxc-minify** achieved smaller outputs, their relative time and ecosystem integration considerations influenced our choice.


## Faster CSS Optimisation with LightningCSS


For CSS optimisation, we adopted LightningCSS from the Parcel team.


```text
new CssMinimizerPlugin({    minify: CssMinimizerPlugin.lightningCssMinify,  })
```


We checked on official benchmark data published by **Lightning CSS** (Parcel team) comparing it against **cssnano** and **esbuild (CSS minify)** :


🔗[GitHub — parcel-bundler/lightningcss: An extremely fast CSS parser, transformer, bundler, and minifier written in Rust.](https://github.com/parcel-bundler/lightningcss#benchmarks)


Press enter or click to view image in full size


Benchmark data showed three consistent advantages:


- ⚡ **Fastest across all benchmarks**
- 📦 **Consistently smaller output size**
- 🚀 Significant gains on large CSS bundles (e.g., Tailwind)


LightningCSS delivered both **better compression and lower minification time** , making it the optimal choice for CI-scale builds where CSS optimisation directly impacts total build latency.


The impact was substantial:


- JavaScript minification became roughly 10× faster
- CSS optimisation improved by approximately 6×


## Maximising CPU Utilisation


Modern CI agents provide multiple cores, but many build pipelines fail to use them effectively.


To improve CPU utilisation, we introduced thread-loader:


```text
use: ['thread-loader', 'swc-loader']
```


This allowed expensive transformations to run across multiple CPU cores.


Benefits included:


- Reduced cold build bottlenecks
- Better resource utilisation
- Higher CI throughput


## Optimising Dependency Management with pnpm


Compilation wasn’t the only bottleneck.


Dependency installation was also consuming valuable CI time.


To reduce dependency installation overhead in our monorepo, we evaluated multiple package managers (` npm` ,` yarn` ,` pnpm` ,` bun` ) using publicly available benchmarks comparing **install speed, disk usage, and dependency resolution models** .


Press enter or click to view image in full size


Although some newer runtimes like **Bun** show strong performance in synthetic benchmarks, we selected **pnpm** due to its **mature ecosystem support, Node.js compatibility, and stable adoption in large production systems** .


Unlike npm’s copy-based installation model, **pnpm uses a content-addressable global store** where packages are downloaded once and **hard-linked into project**` node_modules` . This eliminates duplication across workspaces and enables efficient dependency reuse.


Press enter or click to view image in full size


## Benefits


- **Reduced install time** — dependencies are downloaded once and reused across projects.
- **Lower disk usage** — no duplicated packages across multiple` node_modules` .
- **Strict dependency resolution** — prevents phantom dependencies and improves build determinism.
- **Better Docker layer caching**


## Impact


- **Reduced CI install time** through global dependency reuse
- **Lower disk usage** by eliminating duplicate` node_modules` packages
- **Improved Docker build caching** via pnpm store reuse
- **More deterministic builds** with strict dependency resolution


```text
RUN --mount=type=cache,target=/root/.local/share/pnpm/store \      pnpm install --frozen-lockfile
```


## The Hidden Bottleneck: Jenkins Authentication


One of the most interesting optimisations had nothing to do with frontend tooling.


AWS CodeArtifact authorisation tokens expire every 12 hours.


That caused:


- Cache invalidation
- Re-authentication overhead
- Docker layer busting


To address this, we generated tokens once per Jenkins job and reused them throughout the pipeline.


```text
export CODEARTIFACT_AUTH_TOKEN=$(aws codeartifact get-authorization-token \    --domain <domain> \    --domain-owner <owner> \    --query authorizationToken \    --output text)
```


The result:


- Better Docker layer reuse
- Fewer redundant installs
- More stable pipelines


## Docker Build Optimisation


Finally, we restructured our Dockerfiles into deterministic layers:


1. Base runtime layer
2. Dependency installation layer
3. Source copy layer
4. Webpack build layer


Combined with:


- — mount=type=cache
- — mount=type=secret


this significantly increased cache reuse across builds.


## Results


The combined effect of these optimisations was substantial.


Press enter or click to view image in full size


## Engineering Takeaways


Several lessons emerged from this initiative:


- Build performance is a systems problem, not just a bundler problem.
- Profiling is more valuable than assumptions — measure before you migrate.
- Modern Rust-based tools such as SWC, esbuild, and LightningCSS deliver compounding performance gains.
- pnpm and effective Docker caching improve both build speed and pipeline determinism.
- Infrastructure decisions, from dependency management to token lifecycle handling, can have as much impact as frontend tooling.


Most importantly:


**The biggest bottleneck is often not where everyone assumes it is.**


## Conclusion


When build latency became a recurring bottleneck, replacing Webpack seemed like the obvious solution.


Profiling revealed a different reality. The largest costs were not in bundling, but in transpilation, minification, dependency management, caching, and container build workflows.


By modernising those components instead of replacing the bundler, we reduced build times from over 20 minutes to approximately 2 minutes while preserving ecosystem stability and avoiding a high-risk migration.


**Sometimes the fastest path forward isn’t a new platform — it’s understanding where your existing one spends its time.**
