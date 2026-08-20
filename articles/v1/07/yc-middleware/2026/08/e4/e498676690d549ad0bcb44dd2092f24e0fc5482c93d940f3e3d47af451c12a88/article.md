---
schema_version: "1.0.0"
document_id: "e498676690d549ad0bcb44dd2092f24e0fc5482c93d940f3e3d47af451c12a88"
company_key: "yc-middleware"
company: "Middleware"
source_id: "yc-middleware-news-import-fe54c9028845"
canonical_url: "https://middleware.io/blog/rum-source-map/"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T09:40:34.263987+00:00"
fetched_at: "2026-08-05T09:40:36.438037+00:00"
content_hash: "sha256:91c4f2eb6f8572cfd9f4b004558f84fb7e2612dc8c1c8ec05aa4bffe56b71479"
---

# How to Upload Source Maps for Real User Monitoring So Stack Traces Aren’t Minified

Source maps are JSON files that map your minified, bundled JavaScript back to the original file and line number you wrote.[Real User Monitoring (RUM) tools](https://middleware.io/blog/what-is-real-user-monitoring/) use them to turn a.js:1:48213 into checkout/validateCard.ts:42. This guide covers why minified traces show up, how upload-and-match works, and how to automate it in CI/CD.


The real challenge isn’t using source maps, it’s keeping them matched to the exact build your users are running. A mismatched map is worse than no map: it produces a wrong trace with false confidence. By the end, you’ll be able to generate, upload, and clean up source maps as an unattended release step.


### Symbolicate Every Frontend Error, Automatically


Middleware’s RUM Error Tracking view flips between a MINIFIED and ORIGINAL stack side by side, so you can confirm a map matched before you start debugging.


#### Key takeaways


- Minified stack traces in RUM almost always trace back to one of three causes: source maps were never uploaded, the uploaded map’s version doesn’t match the deployed build’s app.version, or the map was generated but never emitted to disk.
- Source map matching relies on an exact version or build-ID string set at both SDK-init time (in the browser) and upload time (in the CLI/plugin); a mismatch silently falls back to raw minified frames.
- Most RUM platforms support source map upload two ways: a CLI tool you call directly, or a bundler plugin (Webpack, Next.js) that runs automatically during your production build.
- The right place for source map upload is a dedicated CI/CD step that runs after the build and before artifact cleanup, never a manual, pre-deploy checklist item.
- Source maps should generally be stripped or removed from public production output after upload; some platforms (like Splunk RUM) retain uploaded maps permanently and don’t offer deletion, which matters for compliance-sensitive codebases.
- devtool: “source-map” (Webpack) or productionBrowserSourceMaps: true (Next.js) is a prerequisite most teams forget no source map file, no unminified stack trace, regardless of what RUM tool you use.


## Why Are My RUM Error Stack Traces Showing Minified Code Instead of My Actual Source?


A minified trace is the default, not the exception; the browser only knows the file it executed, not the TypeScript or JSX you wrote. Getting a readable trace is opt-in, and it breaks in one of three places:


- **No source map was generated.** Production builds often disable them for bundle-size or security reasons (devtool: false in Webpack, or productionBrowserSourceMaps never set in Next.js) there’s nothing to upload even if you tried.
- **A map exists but was never uploaded.** It sits in dist, deployed alongside your JS, either publicly exposed or excluded from the bundle entirely.
- **A version mismatch.** The RUM SDK tags errors with an app.version set at init. The uploaded map is tagged with a version string too. If the two strings don’t match exactly a stale build, a hotfix that bumped one but not the other, a map uploaded as latest the platform can’t tell which map fits which bundle, and silently falls back to the minified frame.


## How Source Map Symbolication Actually Works


Three artifacts have to line up the deployed minified file, the deployed source map, and the RUM SDK’s runtime version and the platform ties them together with a shared key.


- **Build time:** the bundler emits a .js file plus a .js.map file, and appends a //# sourceMappingURL=app.min.js.map comment. The source map spec (TC39/ECMA-426, formerly “Source Map Revision 3”) defines the JSON: a compressed mappings table linking generated positions to original ones, plus a source array of original file paths.
- **Upload time:** a CLI or bundler plugin sends that .map file to your RUM provider, tagged with an app version, release tag, or Git hash, so the platform can key the map to the exact build that shipped it.
- **Error time:** when a user’s browser throws, the platform looks up the map keyed by that version string and rewrites each frame filename, line, column, often the original function name before it hits your dashboard.


## How Source Maps Work in Middleware


The mechanics above hold industry-wide, but the exact fields and commands change from platform to platform. Here’s how Middleware’s implementation works end to end, before it’s set against the rest of the field.


**SDK init** – In the browser, Middleware’s RUM agent tags every session and error with a version identifier, set alongside projectName and serviceName. This string has to travel unchanged from the browser to the upload step; it’s the key the platform later uses for symbolication.


**Upload** – The sourcemap-uploader CLI or the Webpack/Next.js plugin sends the .map file with an explicit –appVersion flag (or the equivalent plugin argument), which must equal the SDK’s version string exactly. Omit it, and the map uploads as latest: the platform silently falls back to minified frames for that build instead of throwing an error.


**Next.js setup** – The @middleware.io/sourcemap-uploader package installs alongside the OpsAI APM agent and wires into next.config.js directly, so the version string is set once instead of duplicated across a separate CLI invocation.


**Dashboard** – That same version tag drives the Performance Monitor and Error Tracking views. A canary with two active versions shows error rate and loading time broken out per version instead of blended together, and Error Tracking uses the tag to confirm a map matched flipping between the MINIFIED and ORIGINAL stack for a given error.


**Cleanup** – Once a map is confirmed working, the same find/sed pattern from Step 5 below strips the .map files and sourceMappingURL comments from public build output, so the map exists only inside the platform.


## Comparing Source Map Upload Across RUM Platforms


With Middleware’s own implementation laid out, here’s how that same generate → upload → match mechanism plays out, and differs, across the[other major RUM platforms](https://middleware.io/blog/real-user-monitoring-tools/) . The core mechanism is consistent industry-wide; the matching key and cleanup story are what differ.


**Platform** **Upload method** **Version-matching key** **Cleanup / retention notes**


Middleware sourcemap-uploader CLI or Webpack/Next.js plugin appVersion flag must equal SDK’s app.version Provides a documented find/sed cleanup step to strip .map files and sourceMappingURL comments post-upload


Datadog datadog-ci sourcemaps upload (Datadog CLI) Release version tag; auto-collects Git repo, commit hash, and file paths if run inside a Git working directory Enforces a 500 MB combined size limit per minified/map pair; recommends code-splitting if exceeded


Sentry Sentry CLI, or native Webpack/Vite/Rollup/Esbuild plugins Debug ID embedded directly into the build artifact (not a manually-set version string) Setup wizard (npx @sentry/wizard -i sourcemaps) auto-detects a misconfigured pipeline


Splunk RUM splunk-rum CLI or Webpack build plugin Computed source-map ID injected as a code snippet into each minified file Maps are stored permanently Splunk RUM currently offers no deletion mechanism


Elastic APM Manual upload to the APM Server’s source map endpoint serviceName + serviceVersion (or a Git commit ref) set in the RUM agent Legacy /v1/rum/sourcemaps endpoint was deprecated as of APM Server 7.0+


### Get Readable Stack Traces on Every Deploy


Source maps are only doing their job if the version string on the map matches the version string your users are actually running; everything in this guide exists to make that match automatic instead of hoped-for. Wire the upload into CI, strip the maps from your public output, and confirm the match with a real error before you consider the pipeline done.


## How to Upload Source Maps for Real User Monitoring So Stack Traces Aren’t Minified


Fixing this is a five-step sequence, and skipping or reordering any one of them is the single biggest cause of “I uploaded the source map and it’s still minified” support tickets.


### 1. Generate source maps in your production build


Nothing downstream works if the bundler never emits a .map file. For Webpack, set devtool: “source-map” explicitly the default production mode often disables this for bundle-size reasons:


```text


// webpack.config.js
module.exports = {
mode: "production",
devtool: "source-map",
};


```


For Next.js, the equivalent is a config flag rather than a devtool setting:


```text


// next.config.js
module.exports = {
productionBrowserSourceMaps: true,
};


```


Verify this by inspecting your actual output directory (dist, or .next) for both .js and .js.map files don’t assume the config took effect until you’ve confirmed the file exists.


### 2. Set and propagate a single version identifier


Pick one version string, a semver tag, a Git short hash, or your CI build number and use it in exactly two places: the RUM SDK’s defaultAttributes at init time, and the uploader’s version flag at build time. The safest source for this string is your CI environment itself (e.g., $CI_COMMIT_SHA or $GITHUB_SHA), not a hardcoded value in source control, since a hardcoded value is the thing most likely to go stale after the next deploy.


if you can’t answer “where does this exact string come from” for both the SDK config and the uploader command, you have a version-drift bug waiting to happen.


### 3. Upload via CLI or bundler plugin


For a one-off or scripted upload, Middleware’s CLI takes a straightforward form:


```text


sourcemap-uploader upload \
--apiKey= \
--path="/path/to/sourcemaps" \
--appVersion="1.4.2"


```


For projects already using Webpack or Next.js, the plugin approach avoids a separate CLI invocation by hooking into the existing build see Middleware’s[Next.js source map configuration guide](https://docs.middleware.io/opsai/apm_configuration/nextjs) for the full setup:


```text


const MiddlewareWebpackPlugin =
require("@middleware.io/sourcemap-uploader/dist/webpack-plugin").default;


module.exports = {
productionBrowserSourceMaps: true,
webpack: (config) => {
config.plugins.push(new MiddlewareWebpackPlugin(""));
return config;
},
};


```


The plugin form is generally the better default for teams with an existing build pipeline; it removes an entire class of “forgot to run the upload script” failures because it can’t be skipped without also skipping the build itself.


### 4. Automate the upload as its own CI/CD pipeline step


The most reliable pattern is a dedicated step that runs after your build produces artifacts but before deployment or cleanup, not a developer’s local machine, and not a manual runbook item. A minimal GitHub Actions step looks like this:


```text


- name: Build application
run: npm run build


- name: Upload source maps
run: |
npx sourcemap-uploader upload \
--apiKey=${{ secrets.RUM_API_KEY }} \
--path="./dist" \
--appVersion="${{ github.sha }}"


- name: Deploy
run: npm run deploy


```


Using github.sha (or your CI system’s equivalent commit reference) as the version string means the upload step and the SDK’s runtime version can both derive from the same source of truth automatically, rather than a human keeping two config files in sync.


The pipeline ordering matters as much as the command itself; a source map upload that runs after deploy, or in a separate unsynchronized job, reintroduces the exact version-mismatch risk this whole process exists to eliminate.


### 5. Strip maps and references from public production output


Once the map is uploaded and confirmed working, remove it and its reference comment from what actually ships to users an unlisted .js.map file sitting in a public bundle directory is routinely scraped and gives away your original, unminified source:


```text


# Delete source maps
find /path -type f -name '*.js.map' -delete
find /path -type f -name '*.css.map' -delete


# Delete source map references
find /path -type f -name '*.js' -exec sed -i -E 's/sourceMappingURL=[^ ]*\.js\.map//g' {} +
find /path -type f -name '*.css' -exec sed -i -E 's/sourceMappingURL=[^ ]*\.css\.map//g' {} +


```


This step has to run after the upload step in the same pipeline, never before reversing the order means you’re stripping a map that was never sent anywhere.


## Deciding Where This Fits in Your Release Process


The right starting point depends on deploy frequency and how much automation you already trust:


- **Shipping a few times a week:** Start with the bundler plugin one config change, no new pipeline stage.
- **Multiple deploys per day:** The dedicated CI step tied to a commit SHA is close to mandatory; manual uploads can’t keep pace.
- **Compliance-sensitive:** Prioritize the cleanup step and check your vendor’s retention policy Splunk RUM, for example, currently offers no way to delete uploaded maps.
- **Debugging “it’s still minified” right now:** Work backward through steps 1–4 confirm the .map file exists, then the version strings match, then that the upload actually ran on the last deploy.
- **Running a canary or multi-version rollout:** Upload per-version, not just once on main each live version needs its own matched map.


## Where This Connects to Broader Observability


A readable stack trace is the start of debugging, not the end. The next question is always “which users hit this, on which build, and what did they do beforehand.” That’s a RUM problem, not just a source-map problem.


Middleware keeps unminified stack traces in the same Error Tracking view as[session replay](https://middleware.io/product/real-user-monitoring/session-replays/) , and, with trace correlation configured, links each error to the backend trace that request touched.


### Stop Guessing Which Build Broke


Middleware correlates unminified RUM errors with the exact app.version and deployment they came from, so a spike after a release is obvious at a glance not something you piece together from timestamps.


## FAQs


### What's the best way to automate source map uploads in a CI/CD pipeline for RUM tools?


Add a dedicated pipeline step, after the build and before deploy, that runs your RUM provider’s CLI or bundler plugin using a version string pulled from CI itself (a commit SHA or build number) rather than a hand-maintained config value. This keeps the uploaded map’s version and the SDK’s runtime app.version derived from the same source automatically.


### Why do source maps sometimes still show minified code after I've uploaded them?


The most common cause is a version mismatch; the string used at upload time doesn’t exactly match the app.version (or equivalent) set in the SDK at runtime. Less commonly, the upload ran before the build finished, uploaded the wrong directory, or the build never generated a .map file at all.


### Do I need to keep source maps on my production server after uploading them?


No, and generally shouldn’t. Most teams delete .map files and strip sourceMappingURL comments from shipped JS/CSS immediately after a confirmed successful upload, since a publicly reachable map file exposes your original source.


### Does every RUM platform use the same matching mechanism for source maps?


No. Most platforms (Middleware, Datadog, Splunk, Elastic) match on an explicit version or commit-hash string you set yourself, while Sentry uses a Debug ID embedded directly in the build artifact by its bundler plugins, removing the manual version-matching step at the cost of tighter coupling to Sentry’s own tooling.


### Can I upload source maps manually instead of in CI/CD?


You can, but it doesn’t scale past the first few deployments. A manual step is the first thing skipped under deadline pressure, and a single missed upload means every error from that release ships with an unreadable stack trace until the next deploy fixes it.
