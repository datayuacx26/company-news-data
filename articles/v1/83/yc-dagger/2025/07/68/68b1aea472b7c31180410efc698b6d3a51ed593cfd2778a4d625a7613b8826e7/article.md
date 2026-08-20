---
schema_version: "1.0.0"
document_id: "68b1aea472b7c31180410efc698b6d3a51ed593cfd2778a4d625a7613b8826e7"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/typescript-sdk-performance/"
published_at: "2025-07-10T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:cd2ce849e399f792d228f74a81bd486f11d5a51d367c3eaa63e624914d5d159c"
---

# We Bundled and Saved 50% on Cold Starts of our TypeScript SDK

Our TypeScript SDK was taking 20-30 seconds to initialize, creating friction for developers every time they’d call a Typescript module. Which they’d do hundreds of times during development. Here’s how we cut that time in half.


BEFORE


AFTER


## Background


The SDK has two parts: a client library that lets your code call the Dagger API, and tooling that lets you publish your own Dagger Functions.


Every Dagger Engine build shipped with the full TypeScript SDK, and` dagger init` or` dagger develop` copied it into your module—the folder with your pipeline code and a` dagger.json` manifest. On each` dagger call` the engine still had to:


1. Build a client library so the module can talk to the engine
2. Install the SDK’s packages
3. Install the module’s packages
4. Run one helper script: if you gave a function name, run it; otherwise, register all functions for later use.


All of this ran before your code, adding 20–30 seconds to every cold start.


## Performance Challenges


In the first version, we copied the SDK’s code into each user module as a sub‑package. That meant both the SDK and the module shared the same dependency list. If the SDK needed, say, GraphQL‑client vX but the module pinned vY, the package manager would flag the mismatch and fail during installation—or worse, pick a version that let the build succeed and then crash at runtime.


The TypeScript SDK also needed to support multiple runtimes (Deno, Bun, and Node.js) and multiple package managers (npm, pnpm, and multiple versions of yarn). Each runtime and package manager had its own specific behaviors and quirks:


- **Config drift:** Bun and Deno’s configuration files differed from Node.js, which meant additional custom scripting to get the configuration right for each runtime. We also had to enable various experimental features to make the SDK “just work” with Deno.
- **Install steps:** Package manager installation varied. For example, installing yarn required also installing Corepack, while pnpm required an extra workspace file. This added complexity and code to maintain compatibility between different package managers.
- **Speed:** Package manager performance was also variable. For example, pnpm was much faster (~1.5x) at downloading dependencies than npm or yarn, which resulted in a correspondingly better user experience.


The performance issue, in particular, was the main source of developer dissatisfaction. During active module development, users call their Dagger modules tens or hundreds of times. Waiting 20–30 seconds for each call made it a frustrating experience, to say the least.[Caching](https://docs.dagger.io/features/caching/) helps but not when it’s invalidated or can’t be reused, such as for ephemeral CI environments.


## Promise in Bundling


When we started looking into this issue, our first step was to benchmark the TypeScript and Go SDKs. It was immediately clear that the Go SDK was much faster, and most of the slowdown could be traced to dependency installation and management.


We tried to fix this by changing the tooling: switching from npm to yarn, installing only production dependencies, using the Dagger cache more efficiently, and reorganizing the SDK initialization steps. Although we saw a modest[performance improvement](https://github.com/dagger/dagger/pull/7096#issuecomment-2124627475) of 2–3 seconds, it was inconsistent and only visible in a few situations. It wasn’t enough.


The next and more meaningful improvement came from[bundling tsx](https://github.com/dagger/dagger/pull/9526) —the TypeScript command-line tool—in the Dagger Engine image to speed up module initialization. This avoided on-demand compilation during the SDK’s initialization process. It showed promise: With a cold cache, this improved module initialization times by 7–10 seconds. Maybe bundling was the answer…


## Not So Fast


Before rushing to bundle all the things, we paused to consider the potential tradeoff to a better developer experience: flexibility.


Solving for dev experience would mean setting restrictions on the choice of runtime, package manager and configuration. It would likely result in better performance, but at the cost of being harder to integrate with projects using different stacks or tooling.


Solving for flexibility would mean supporting a wide range of runtimes, package managers and configurations. This might make it easier for developers to adopt, but at the cost of speed and control.


We contemplated this[for a while](https://github.com/dagger/dagger/issues/8689) and benchmarked different options to ensure we’re making the best decision for users. Ultimately, we concluded that giving up a bit of flexibility is worth the price for meaningfully improving the daily experience of all users. The next step was to see exactly how big of an improvement we could get from bundling…


## Bundle Time


The biggest breakthrough came something that seemed crazy at first: bundling the *entire* SDK. Instead of copying the entire TypeScript SDK directory and downloading all the dependencies (~155 MB) to the user’s module, we replaced it with a smaller, more efficient bundled version (~4.5 MB). This optimization resulted in faster module initialization, produced fewer files in the user’s module directory, and eliminated many of the dependency management issues from before.


A few of the issues we addressed along the way:


- We tried a few different bundler options and finally settled on the Bun bundler, because it was fast and worked well. We still needed a tool to bundle the TypeScript type declarations, because Bun’s bundler only outputs JavaScript, so we ran Rollup—a widely used JS/TS bundler—for that one job.
- We ran into issues when introspecting the code of user modules: decorators were not working, promises were failing. We traced the problem to TypeScript being included in the final bundle. Once we removed TypeScript and made it a separate download, everything started working correctly.
- We needed a way to regenerate the client every time the Dagger dependencies of the user module changed, or a new dependency was installed. We did this by tweaking the` tsconfig.json` path configuration to restrict reloads to only specific parts of the SDK.


The final optimized bundle now consists of just five files: the bundled JavaScript code, bundled type definitions, a dynamically generated client, and support files for telemetry and imports. The only dependency that we download now is TypeScript itself.


The final result: Module initialization times dropped significantly, from[~21 seconds to ~11 seconds](https://github.com/dagger/dagger/pull/10094#issuecomment-2786296756) on average. At first glance 10 seconds may not seem like much, but multiplied tens or hundreds of times a day in the middle of deep work, it’s a difference that[users noticed](https://discord.com/channels/707636530424053791/1157515934597136404/1359203614920540271) .


—————————————————————————————————————————————————————


If you’re new to[Dagger](https://dagger.io/) : it’s a platform for composing and running software engineering workflows. Use it to automate tests, builds, code reviews, and more.[Try it now](https://docs.dagger.io/quickstart/) with your SDK of choice. They’re fast!
