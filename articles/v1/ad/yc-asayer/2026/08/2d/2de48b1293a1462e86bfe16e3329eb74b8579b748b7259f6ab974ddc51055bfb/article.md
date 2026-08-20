---
schema_version: "1.0.0"
document_id: "2de48b1293a1462e86bfe16e3329eb74b8579b748b7259f6ab974ddc51055bfb"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/bunx-when-to-use/"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-09T22:46:23.853414+00:00"
fetched_at: "2026-08-09T22:46:25.551132+00:00"
content_hash: "sha256:0839dfeb6b82521e254146c74dafd362d0c2307b3642cc70d33d73521a49bf0e"
---

# What bunx Is and When to Use It

` bunx` is Bun’s package runner and an alias for` bun x` ; it downloads and runs a package’s binary from npm without a global install, the same job[npx](https://docs.npmjs.com/cli/v11/commands/npx) and` yarn dlx` do.


If you’ve ever sat waiting on` npx` to spin up a scaffold command you run a dozen times a day, that little delay is exactly what` bunx` is meant to cut. If you already type` npx create-next-app` or` npx shadcn@latest` on a daily basis,` bunx` is the near drop-in you reach for once Bun is on your machine, with one runtime wrinkle (` --bun` ) and one real gotcha (tools that hardcode the literal string` npx` ) worth understanding before you switch.


This article gives you the mental model: what` bunx` is, how it resolves packages, why it starts faster than` npx` , what the` --bun` flag actually does, and a decision rule for when to use it.


## Key Takeaways


- ` bunx` is Bun’s package runner and an alias for` bun x` ; it runs an npm package’s binary without a global install, exactly like` npx` or` yarn dlx` .
- Like` npx` ,` bunx` checks for a locally installed copy first and only then auto-installs from npm; both tools cache resolved packages, so the honest difference is that` bunx` runs on Bun’s lower-overhead runtime and stores packages in Bun’s own global cache.
- The` --bun` flag forces a CLI such as Vite, Next, or Prisma to execute on the Bun runtime instead of Node, and it must appear *before* the executable name (` bunx --bun vite` ).
- Reach for` bunx` for one-off scaffolding and CLI tools; stay on` npx` only when a tool hardcodes` npx` or breaks on Bun’s runtime.
- A shell alias like` alias npx=bunx` works interactively but is invisible to non-interactive spawns; put a real executable on your` PATH` instead.


## What is bunx?


` bunx` runs an executable from an npm package without installing it globally, and it ships automatically with Bun.[The docs confirm](https://bun.com/docs/pm/bunx) that` bunx` is an alias for` bun x` and is auto-installed when you install` bun` . It’s Bun’s equivalent of` npx` or` yarn dlx` .


The invocation shape is identical to` npx` :


```text
# npx
npx   create-next-app@latest   my-app


# bunx
bunx   create-next-app@latest   my-app
```


Packages declare their binaries in the` "bin"` field of` package.json` ;` bunx <package>` finds that binary and runs it. Version pinning works the same way as npx. Append` @version` to the package name:


```text
bunx   uglify-js@3.14.0   app.js
bunx   shadcn@latest   add   button
```


When the binary name differs from the package name, use` -p` /` --package` to name the package explicitly, then the binary:


```text
bunx   -p   @angular/cli   ng   new   my-app
```


## How does bunx resolve a package?


` bunx` checks for a locally installed copy of the package first, then falls back to auto-installing it from npm, and it stores what it installs in Bun’s global cache for reuse. This is documented behavior: “As with` npx` ,` bunx` checks for a locally installed package first, then falls back to auto-installing it from npm.” Resolved packages land in Bun’s global cache so later runs skip the download.


One correction worth making: modern` npx` (npm v7+, i.e.` npm exec` ) does **not** download-then-discard on every run. It also keeps a persistent per-user cache and reuses packages on repeat invocations. So the real difference is not “npx throws away, bunx keeps”; both cache. The genuine differentiator is where the cache lives (Bun’s own global store) and the runtime overhead of getting from invocation to execution.


## Why bunx Is Faster Than npx


` bunx` starts faster because it runs on Bun’s runtime, which is[built on JavaScriptCore](https://bun.com/) (Safari’s engine) rather than spinning up Node, so the fixed cost of launching the package runner is lower. Bun’s team frames the gain concretely: the introduction of bunx billed it as installing and running an executable from npm, 100x faster than npx, a figure the docs attach specifically to *locally installed* packages.


Treat that number as Bun’s published claim for the warm, already-installed case, not a universal benchmark. The startup story is the part that generalizes: for a cold-start CLI invocation (the thing you do dozens of times a day scaffolding projects), Bun’s lower process-launch overhead is where the time comes back. For a first-time install that has to hit the network, both tools pay the download cost, and any speed difference narrows to install throughput plus that startup delta rather than a 100x gap.


If you want a number you can stand behind, measure it yourself and separate cold-cache from warm-cache runs:


```text
# warm cache (both already resolved) vs cold — measure, don't assume
hyperfine   '  npx cowsay hi  '   '  bunx cowsay hi  '
```


## The —bun Flag


The` --bun` flag forces a CLI such as Vite, Next, or Prisma to execute on the Bun runtime instead of Node, overriding the` #!/usr/bin/env node` shebang the tool normally ships with. By default, Bun respects that shebang and spins up a` node` process to run the file;` --bun` tells it to use Bun’s runtime instead:


```text
bunx   --bun   vite   dev
```


The flag is position-sensitive. It **must occur before the executable name** . Anything after the name is passed straight through to the tool as its own argument:


```text
bunx   --bun   my-cli     # good — runs my-cli on Bun
bunx   my-cli   --bun     # bad — passes --bun to my-cli
```


Use` --bun` when you actually want the tool running on Bun, to pick up Bun’s faster startup or its native TypeScript handling. Leave it off (the default) when a tool depends on Node-specific behavior; some build tools and CLIs assume Node internals, and forcing them onto Bun’s runtime can surface compatibility failures. In the wild, a common failure mode is a CLI that works under plain` bunx toolname` but throws once` --bun` swaps the runtime out from under it. The fix is usually to drop` --bun` and let the Node shebang stand.


## When to Use bunx (and When to Stay on npx)


**Decision rule:** use` bunx` for one-off scaffolding and CLI tools (` bunx create-next-app my-app` ,` bunx prisma migrate` ,` bunx prettier foo.js` ) and stay on` npx` only when something hardcodes the literal string` npx` or breaks on Bun’s runtime.


Task npx bunx


Scaffold an app` npx create-next-app my-app`` bunx create-next-app my-app`


Run a dev server` npx vite`` bunx vite`


Run migrations` npx prisma migrate`` bunx prisma migrate`


Add a component` npx shadcn@latest add button`` bunx shadcn@latest add button`


Format a file` npx prettier foo.js`` bunx prettier foo.js`


The one real gotcha is tooling that calls` npx` by name. A shell alias like` alias npx=bunx` works when you type commands interactively, but shell aliases exist only in interactive shells: they’re invisible to non-interactive spawns. A tool that shells out to` npx` (for example` uv run` invoking it internally) won’t see the alias at all.


The fix is to put a real executable named` npx` on your` PATH` so *any* process that spawns` npx` resolves to your shim. The[htdocs workaround](https://htdocs.dev/posts/replacing-npx-with-bunx-a-simple-workaround/) is three lines:


```text
mkdir   -p   ~/.local/bin
printf   '  #!/bin/sh\nexec bunx "$@"\n  '   >   ~/.local/bin/npx
chmod   +x   ~/.local/bin/npx
```


Make sure` ~/.local/bin` comes early on your` PATH` . Because this is a real file on disk, not a shell alias, non-interactive spawns resolve it too. If you want a fallback that only routes through Bun when it’s installed, a conditional wrapper function with a` --real` escape hatch,[as shown by nrjdalal](https://nrjdalal.com/blog/npm-with-bun) , is the more elaborate cousin of the same idea.


## Conclusion


Treat` bunx` as` npx` on a faster runtime: same resolution order, same version-pinning syntax, same command shape, with a lower-overhead start and packages cached in Bun’s own store. Add the` --bun` flag only when you want the tool itself running on Bun, keep it before the executable name, and drop a real` npx` shim on your` PATH` for the handful of tools that demand the literal command. Install Bun, swap one` npx` for` bunx` on your next scaffold, and time the difference yourself.


## FAQs


Is bunx a drop-in replacement for npx?


bunx is a near drop-in replacement for npx: it shares the same command shape, the same version-pinning syntax with an at-version suffix, and the same local-first resolution order. The one exception is tools or scripts that call the literal string npx internally, which won't recognize bunx unless you place a real executable named npx on your PATH. For those cases, bunx is not automatically substituted.


Does bunx work without installing Bun separately?


No, bunx requires Bun. bunx is an alias for the bun x command and is auto-installed whenever you install Bun itself, so there is no standalone bunx package. Once Bun is on your machine, bunx is available with no extra setup. If Bun is not installed, the bunx command does not exist and you must fall back to npx or another package runner.


What is the difference between bun x and bunx?


There is no functional difference: bunx is simply an alias for bun x, so the two commands run identically. Both invoke Bun's package runner to execute a package's binary without a global install. Use whichever spelling you prefer. bunx exists mainly as a shorter, npx-familiar form that developers coming from npm recognize immediately.


Why does bunx --bun break some CLIs that run fine without it?


Because --bun forces the CLI onto Bun's runtime instead of Node, overriding the Node shebang the tool ships with. Some build tools and CLIs depend on Node-specific internals, so swapping the runtime surfaces compatibility failures. A tool that works under plain bunx toolname can throw once --bun is added. The fix is to drop --bun and let the tool run on Node as its shebang intends.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
