---
schema_version: "1.0.0"
document_id: "3746a995c75c5abd4c39134d35fdcbcaced58dcca397f6ac1bd98a5be18ab340"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/optimize-your-dockerfile-for-5x-faster-builds"
published_at: "2026-01-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:54:41.720106+00:00"
content_hash: "sha256:b2e193aff8d3134fbb69b554d409dc96e8af71213f9ab7a54948985730951baf"
---

# Optimize your Dockerfile for 5x faster builds

Slow Docker builds are annoying. You make a small code change, run` docker build` , and wait. This post shows you how to optimize your Dockerfile for faster builds.


These Dockerfile optimization techniques will work whether you're building locally or in CI, and for any language. The examples in this post use a Node.js application with about 40 dependencies (including common packages like ORMs, UI libraries, and AWS SDKs) to represent a production React/Node application.


Here's a preview of the biggest wins after optimization:


Metric Before After Improvement


Rebuild time (after code change) 37.4s 7.1s 5.3x faster


Final image size 2.49 GB 1.11 GB 55% smaller


**Note:** Build times can vary by 10-15% between runs depending on system load, network conditions, and npm registry response times. The values in this post are averages across three builds.


## The problem: A slow Dockerfile


The example Dockerfile takes about 37 seconds to rebuild after code changes. The optimizations in the following sections bring that down to about 7 seconds. 37 seconds isn't *the worst* , but saving 30 seconds per build adds up. More importantly, the benefits of these optimizations scale with your app.


Here's an unoptimized Dockerfile that rebuilds everything on every code change:


```text
FROM   node:22
WORKDIR   /app
COPY   . .
RUN   npm ci
RUN   npm run build
CMD   [  "npm"  ,   "start"  ]
```


When you change a single line of application code, Docker invalidates the cache at the` COPY . .` instruction and rebuilds everything after it. So even though` package.json` didn't change, Docker reinstalls all the npm packages.


**Build times:**


- First build: 37.1s
- After code change: 37.4s (still rebuilds everything)


The rebuild is about as slow as the first build because the cache invalidates when Docker reaches the first changed instruction (` COPY . .` ) and forces a complete reinstall of dependencies. The main problem? The order of the instructions. The Dockerfile copies all the files before installing dependencies, so any file change forces a full dependency reinstall.


## Order instructions to maximize cache reuse


The Docker build cache invalidates at the first changed instruction and rebuilds everything after it. To minimize rebuilds, order your Dockerfile from least to most frequently changed.


For most apps, dependencies change less often than your source code, so you should install dependencies before you copy source files. For example:


```text
FROM   node:22
WORKDIR   /app
COPY   package.json package-lock.json ./
RUN   npm ci
COPY   . .
RUN   npm run build
CMD   [  "npm"  ,   "start"  ]
```


**Build time improvement:**


- Before: 37.4s (rebuilds everything)
- After: 7.1s (reuses dependency cache)
- Improvement: 5.3x faster


Reordering the Dockerfile instructions cut rebuild time by 81% (from 37.4 seconds to 7.1 seconds). The builder reuses the cached` npm ci` layer when only the application code changes and the package manifest files remain unchanged.


### How the Docker build cache reuses layers


The builder calculates a cache checksum from` package.json` and` package-lock.json` . As long as those files don't change, the` npm ci` layer stays cached—even when you modify other source files.


For details on how the builder determines cache validity, see[Cache invalidation for ADD and COPY instructions](https://depot.dev/blog/ultimate-guide-to-docker-build-cache#cache-invalidation-for-add-and-copy-instructions) in our Ultimate guide to Docker build cache post.


### Optimize instruction order for any language


You can apply the principle of ordering instructions from least to most frequently changed for any language. Copy your dependency manifest files first, install dependencies, then copy your source code.


Python example:


```text
COPY   requirements.txt .
RUN   pip install -r requirements.txt
COPY   . .
```


Go example:


```text
COPY   go.mod go.sum ./
RUN   go mod download
COPY   . .
```


Ruby example:


```text
COPY   Gemfile Gemfile.lock ./
RUN   bundle install
COPY   . .
```


## Exclude unnecessary files from the build


Every file in your build context can invalidate the cache. If you copy a directory that includes files Docker doesn't need, changing those files forces a rebuild even though the files don't affect your application.


### Use a .dockerignore file


A` .dockerignore` file works like` .gitignore` but for Docker builds. To exclude files and directories from the build context, create a` .dockerignore` file in the same directory as your Dockerfile.


An example` .dockerignore` file:


```text
node_modules
npm-debug.log
.git
.gitignore
README.md
.env
.vscode
.idea
dist
build
*.log
```


Why exclude these files from your Docker build?


- Some files are rebuilt during the build.


- ` node_modules` ,` dist` ,` build` : Regenerated by` npm ci` and` npm run build` .


- Development files aren't needed at runtime.


- ` .env` : May contain secrets, changes frequently.
- ` .vscode` ,` .idea` : Editor configs.
- ` README.md` : Documentation.


- Large files slow down context transfer.


- ` .git` : Version history (can be hundreds of MB).
- ` *.log` : Accumulated logs.


### Build context size reduced


Excluding unnecessary files reduces the build context size, which speeds up the initial context transfer to the builder and prevents cache invalidation from irrelevant file changes.


**Build context size:**


- Without` .dockerignore` : 325 MB
- With` .dockerignore` : 1.2 kB
- Reduction: 99.6% (from 325 MB to 1.2 kB)


Without` .dockerignore` , Docker copies your entire local` node_modules` directory (which gets rebuilt anyway) and Git history (which isn't needed in the image).


## Separate build and runtime dependencies


Most apps need build tools that aren't required at runtime. Multi-stage builds let you use a full build environment to compile your application, then copy only the runtime artifacts to a minimal final image.


First, let's see what a single-stage build includes, then we'll split it into stages.


### Single-stage build with all dependencies


A single-stage build includes everything:


```text
FROM   node:22
WORKDIR   /app
COPY   package.json package-lock.json ./
RUN   npm ci
COPY   . .
RUN   npm run build
CMD   [  "npm"  ,   "start"  ]
```


This image includes:


- Build tools from the base image
- All npm dependencies (dev and production)
- Source code and build artifacts


Final image size: 2.49 GB


### Multi-stage build splits build and runtime


Multi-stage builds split the process into distinct stages:


```text
# Stage 1: deps - install all dependencies once
FROM   node:22   AS   deps
WORKDIR   /app
COPY   package.json package-lock.json ./
RUN   npm ci


# Stage 2: build - inherits from deps, compiles the application
FROM   deps   AS   build
COPY   . .
RUN   npm run build


# Stage 3: prod - minimal runtime image with production dependencies only
FROM   node:22-slim   AS   prod
WORKDIR   /app
COPY   package.json package-lock.json ./
RUN   npm ci --omit=dev
COPY   --from=build /app/dist ./dist
COPY   --from=build /app/public ./public
CMD   [  "npm"  ,   "start"  ]
```


The deps stage installs all dependencies once. The build stage inherits from deps (reusing the installed dependencies without reinstalling), copies the source code, and builds the application. The production stage starts fresh with` node:22-slim` and runs` npm ci --omit=dev` to install only production dependencies (no dev tools or testing frameworks).


The key here is that the build stage uses` FROM deps` , so it doesn't need to reinstall dependencies. This pattern is cleaner and more efficient than reinstalling dependencies in each stage.


**Image size improvement:**


- Single-stage build: 2.49 GB
- Multi-stage build: 1.11 GB
- Improvement: 55% smaller


The multi-stage build maintains fast rebuild times for code changes (between 7 and 8 seconds) because the deps stage remains cached when only source code changes. The size reduction is an additional benefit that doesn't compromise rebuild speed.


Cache benefits:


- Changes to the source code get rebuilt from the build stage forward, but don't affect earlier stages.
- Changes to the production stage don't invalidate the build stage cache.
- Each stage maintains its own cache layers independently.


**Note** : BuildKit executes independent stages in parallel. If your application has multiple components that can be built independently, you can build them simultaneously. For more information, see[Stage parallelism](https://depot.dev/blog/how-buildkit-parallelizes-your-builds#stage-parallelism) in the How BuildKit parallelizes your builds post.


## Use cache mounts for package managers


Cache mounts provide persistent storage for package manager caches across builds. Unlike the regular build cache that invalidates when dependencies change, cache mounts let the package manager maintain its own cache of downloaded packages.


While code changes are more common, optimizing dependency updates matters when you're actively developing and frequently adding packages.


Cache mounts are most useful when:


- Your project has hundreds of dependencies.
- You frequently add or update individual dependencies.
- Package downloads are slow (large packages or slow network).
- You're building on the same machine or builder repeatedly.


### Without cache mounts


The baseline for this test uses the multi-stage Dockerfile from the previous section and adds a new dependency. Unlike code changes, which take about 7 seconds because the deps stage remains cached, dependency changes force the deps stage to rebuild along with all downstream stages.


Standard dependency installation downloads packages every time dependencies change:


```text
COPY   package.json package-lock.json ./
RUN   npm ci
```


When you add a new dependency, the` npm ci` layer rebuilds with an empty npm cache, forcing npm to re-download all packages.


Build time without cache mounts: 32.3s


### With cache mounts


Cache mounts persist the npm cache directory across builds:


```text
COPY   package.json package-lock.json ./
RUN   --mount=type=cache,target=/root/.npm \
npm ci
```


The` --mount=type=cache,target=/root/.npm` option tells the builder to mount persistent storage at npm's cache directory. When dependencies change and the` npm ci` layer needs to rebuild, npm can reuse packages from this persisted cache instead of downloading them again.


**Build time improvement (adding one new package):**


- Without cache mount: 32.3s
- With cache mount: 28.5s
- Improvement: 3.8s faster (12% improvement)


The improvement here is pretty minimal because npm ci is already fast with modern npm and good network connections.


### Cache mounts for other package managers


Python (pip) example:


```text
RUN   --mount=type=cache,target=/root/.cache/pip \
pip install -r requirements.txt
```


Go example:


```text
RUN   --mount=type=cache,target=/go/pkg/mod \
go mod download
```


Ruby example:


```text
RUN   --mount=type=cache,target=/usr/local/bundle/cache \
bundle install
```


Each package manager has its own default cache location. The cache mount makes that location persistent across builds.


### Cache mount persistence


Cache mounts persist on a single Docker daemon or builder instance. When you build locally, the cache persists between your local builds. In CI environments, cache mount persistence depends on your setup.


- **Ephemeral CI runners** (GitHub Actions default runners, fresh containers): Cache mounts don't persist between builds. You'll need to configure external cache backends.
- **Persistent builders** (self-hosted runners, Depot, BuildKit with remote cache): Cache mounts persist and provide speed benefits.


For details on using cache mounts in CI, see[How to use BuildKit cache mounts in CI providers](https://depot.dev/blog/how-to-use-buildkit-cache-mounts-in-ci) .


## Choose the right base image


Your base image choice affects both image size and cache behavior. Smaller base images mean less data to download and store in cache layers.


### Base image options for Node.js


The following table lists Node 22 base images. The image sizes vary between architectures and versions, but the use cases remain the same.


Image Use case


` node:22` Full development environment with build tools and system libraries.


` node:22-slim` Roughly 75% smaller than the full base image. Production runtime without build tools and documentation.


` node:22-alpine` Roughly 85% smaller than the full base image. Minimal runtime based on Alpine Linux.


**Note:** Image sizes vary by Node version.


### When to use each base image


Use full base images (for example,` node:22` ) for:


- Build stages in multi-stage builds where you need compilation tools
- Development environments
- Applications with native dependencies that need build toolchains


Use slim images (for example,` node:22-slim` ) for:


- Production runtime stages
- Applications without native dependencies or with pre-compiled binaries
- When you need glibc compatibility (most Linux binaries expect glibc)


Use Alpine images (for example,` node:22-alpine` ) for:


- Minimal production deployments where size is critical
- Applications without native dependencies
- When you've verified your native dependencies work with musl libc


### Example build with optimized base images


Here's an example of a production multi-stage build with appropriate base images:


```text
# Stage 1: deps - install all dependencies
FROM   node:22   AS   deps
WORKDIR   /app
COPY   package.json package-lock.json ./
RUN   npm ci


# Stage 2: build - inherits from deps for compilation
FROM   deps   AS   build
COPY   . .
RUN   npm run build


# Stage 3: prod - uses slim image for smaller final image
FROM   node:22-slim   AS   prod
WORKDIR   /app
COPY   package.json package-lock.json ./
RUN   npm ci --omit=dev
COPY   --from=build /app/dist ./dist
COPY   --from=build /app/public ./public
CMD   [  "npm"  ,   "start"  ]
```


This example gives you full access to build tools during compilation but keeps the final image small.


## How to find optimization opportunities


To optimize effectively, measure where time is spent during your builds.


### View detailed build output


Use the plain progress output to show container output and see timing for each build step:


```text
docker   build   --progress=plain   .
```


The` --progress` flag output shows:


- Time spent on each instruction
- Whether layers came from cache (CACHED markers)
- Context transfer times
- Layer push/pull times


To find optimization opportunities, look for steps that take more than 10 seconds and aren't marked as` CACHED` .


### Identify optimization opportunities


Look for the following in your builds:


- Long-running RUN instructions that execute frequently (good candidates for better caching or cache mounts)
- Large COPY operations that invalidate often (add to .dockerignore)
- Sequential operations that could be parallelized (multi-stage builds)
- Repeated package downloads that could use cache mounts


## Complete optimized example


Optimization checklist:


- Reorder instructions (least to most frequently changed)
- Add` .dockerignore` file
- Use multi-stage builds
- Add cache mounts for package managers
- Choose appropriate base images


Here's the full optimized Dockerfile incorporating all techniques:


```text
# Stage 1: deps - install all dependencies once
FROM   node:22   AS   deps
WORKDIR   /app
COPY   package.json package-lock.json ./
RUN   --mount=type=cache,target=/root/.npm \
npm ci


# Stage 2: build - compile using the full dependency tree
FROM   deps   AS   build
COPY   . .
RUN   npm run build


# Stage 3: prod - minimal runtime image with only production dependencies
FROM   node:22-slim   AS   prod
WORKDIR   /app
COPY   package.json package-lock.json ./
RUN   --mount=type=cache,target=/root/.npm \
npm ci --omit=dev
COPY   --from=build /app/dist ./dist
COPY   --from=build /app/public ./public
CMD   [  "npm"  ,   "start"  ]
```


With the following` .dockerignore` file:


```text
node_modules
npm-debug.log
.git
.gitignore
README.md
.env
.vscode
.idea
dist
build
*.log
```


### Final results


Optimization Primary benefit Impact


Instruction reordering Code change rebuilds 5.3x faster (biggest win)


Multi-stage builds Image size 55% smaller


Cache mounts Dependency updates 12% faster


.dockerignore Build context 99.6% smaller context


## Conclusion


These optimizations made rebuild time over 5x faster and reduced image size by 55%. If you need a place to start, go with instruction reordering. It delivers the biggest speed improvement in 30 seconds of work.


Building your Docker image using Depot can make your builds even faster. See[Depot container builds](https://depot.dev/docs/container-builds/overview) for how it all works.


## FAQ


Why does changing my code trigger a full npm install in Docker?


Docker invalidates its cache at the first changed instruction and rebuilds all subsequent layers. If your Dockerfile copies everything with` COPY . .` before` npm ci` , any file change forces Docker to reinstall all packages. Reorder the instructions to copy only` package.json` and` package-lock.json` first, run` npm ci` , then copy your source code. This way the dependency layer stays cached when you change application code.


How much faster can I make Docker builds by reordering Dockerfile instructions?


Instruction reordering delivers the biggest improvement of all the optimization techniques. Code change rebuilds went from 37 seconds to 7 seconds in the test case, a 5.3x speedup. The fix takes about a minute: copy your dependency manifest files (like` package.json` and` package-lock.json` ) first, run your package manager install, then copy the rest of your source code. This way dependency layers stay cached when you change application code.


Should I use Alpine or slim images for my production stage?


Use slim images unless you have a specific reason to use Alpine. The slim variant is about 75% smaller than the full image and runs on glibc, which most Linux binaries expect. Alpine is even smaller (85% reduction) but uses musl libc instead of glibc, which can cause compatibility issues with native dependencies. If your application has native dependencies or uses packages with compiled binaries, start with slim. Only switch to Alpine after you've verified everything works correctly with musl libc.


Will cache mounts speed up my CI builds?


It depends on your CI setup. Cache mounts persist on a single builder instance, so they work with self-hosted runners or persistent builders like Depot. On ephemeral runners (GitHub Actions default runners), cache mounts don't persist between builds unless you configure external cache backends. Even without cache mount benefits, the other optimizations, especially instruction reordering, deliver significant speedups in any CI environment.


## Related posts


- [Build Docker images faster using build cache](https://depot.dev/blog/faster-builds-with-docker-caching)
- [How to use cache mounts to speed up Docker builds](https://depot.dev/blog/how-to-use-cache-mount-to-speed-up-docker-builds)
- [How Depot speeds up Docker builds](https://depot.dev/blog/depot-magic-explained)


Andrea Anderson


Technical Writer at Depot
