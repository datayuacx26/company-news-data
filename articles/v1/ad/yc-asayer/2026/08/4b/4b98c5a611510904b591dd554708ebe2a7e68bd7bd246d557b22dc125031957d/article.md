---
schema_version: "1.0.0"
document_id: "4b98c5a611510904b591dd554708ebe2a7e68bd7bd246d557b22dc125031957d"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/getting-started-npm-workspaces/"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-07T14:49:13.247278+00:00"
fetched_at: "2026-08-07T14:49:14.251890+00:00"
content_hash: "sha256:0cfc59be809de6f6d16419c486caba47be983ace334076e59a67b8d8a39c2274"
---

# Getting Started with npm Workspaces

npm workspaces, built into npm since version 7, let you manage several packages in one repository — a monorepo — from a single root: one` npm install` hoists shared dependencies into a single root` node_modules` and symlinks your own packages there, so cross-package imports resolve with no` npm link` and no republishing. If you have an app plus a shared library, or a component library plus its docs site, and you’re tired of` npm link` , copy-pasting code, or juggling separate repos, this is the built-in feature that removes that friction — no third-party tool required. This guide covers the minimal config, the exact command flags, the real limitations, and when to layer a build orchestrator on top.


## Key Takeaways


- npm workspaces ship with npm 7+; the current release is[npm 11.18.0](https://www.npmjs.com/package/npm?activeTab=versions) , and you can confirm your version with` npm -v` .
- The minimal setup is two files: a root` package.json` with` "private": true` and` "workspaces": \["packages/*"\]` , plus one` package.json` per package — then a single` npm install` at the root wires everything together.
- To depend on a sibling package, add it by name with a` "*"` range; npm symlinks it on install, so edits to the source are visible in every consumer immediately, with no rebuild or republish.
- npm workspaces resolves and links dependencies but does not run tasks in dependency order, cache build outputs, or compute an “affected” graph.
- Reach for Turborepo or Nx *on top of* npm workspaces, not instead of it — npm resolves and links packages; those tools add task orchestration and caching.


## How do npm workspaces work?


npm workspaces turn a single repository into a monorepo by hoisting shared dependencies into one root` node_modules` and symlinking your own packages alongside them. When you run` npm install` at the root, npm scans every workspace, installs third-party dependencies once at the top level, and links each local package into` node_modules` by its` name` field. If two of your packages depend on each other, the reference resolves through that symlink —[the npm CLI automates the linking as part of npm install and removes the need to manually run npm link](https://docs.npmjs.com/cli/v11/using-npm/workspaces) .


The same` workspaces` field and symlink model also power Yarn, pnpm, and Bun, so the mental model transfers across package managers. The feature landed in npm 7; anything newer works.


## What is the minimal npm workspaces setup?


The minimal setup is two files: a root` package.json` that declares where packages live, plus one` package.json` per package. Create this structure:


```text
my-monorepo/
├── package.json          # root — private, lists workspaces
└── packages/
├── utils/
│   └── package.json   # @myorg/utils
└── app/
└── package.json   # @myorg/app
```


The root` package.json` needs two fields:


```text
{
"  name  "  :   "  my-monorepo  "  ,
"  private  "  :   true  ,
"  workspaces  "  : [  "  packages/*  "  ]
}
```


` "private": true` prevents you from accidentally publishing the root, and the` packages/*` glob tells npm to treat every directory under` packages/` as a workspace. Give each package a scoped name like` @myorg/utils` to avoid registry collisions:


```text
{
"  name  "  :   "  @myorg/utils  "  ,
"  version  "  :   "  1.0.0  "  ,
"  main  "  :   "  dist/index.js  "
}
```


Run` npm install` **once at the root** . There is a single lockfile at the root and no` node_modules` inside individual packages — everything hoists up.


## Add a cross-package dependency


To depend on a sibling package, add it by name with a` "*"` range; npm symlinks it on install, so edits to the source are visible in every consumer immediately. In` @myorg/app` :


```text
{
"  name  "  :   "  @myorg/app  "  ,
"  dependencies  "  : {
"  @myorg/utils  "  :   "  *  "
}
}
```


Run` npm install` at the root again. npm creates a symlink from` node_modules/@myorg/utils` to` packages/utils` , and you import it like any published module:


```text
import   {   formatDate   }   from   "  @myorg/utils  "  ;
```


Because it’s a symlink, changing the source in` packages/utils` is reflected in` app` with no rebuild or republish — this is the payoff over` npm link` . One cross-tool caveat: npm does **not** support the` workspace:` version protocol that pnpm and Yarn Berry use.[Passing a workspace: specifier makes npm fail with EUNSUPPORTEDPROTOCOL](https://github.com/npm/cli/issues/8845) , so with npm you reference internal packages by name and range (` "*"` ), not` workspace:*` .


## The daily commands


The flags trip people up because singular and plural mean different things. Add a dependency to one package with` -w` , to every package with` --workspaces` ; run a script in one workspace with` -w` , and across all of them with` --workspaces --if-present` , which skips packages that don’t define that script.


```text
# Install a dep into ONE workspace
npm   install   lodash   -w   @myorg/app


# Install a dev dep into one workspace
npm   install   -D   vitest   -w   @myorg/utils


# Install a dep into EVERY workspace
npm   install   eslint   --workspaces


# Run a script in ONE workspace
npm   run   build   -w   @myorg/utils


# Run a script across ALL workspaces, skipping those without it
npm   run   test   --workspaces   --if-present
```


` -w` is shorthand for` --workspace` , and` --workspaces` (or` -ws` ) targets all of them. Wire the root scripts once so` npm run build` fans out:


```text
{
"  scripts  "  : {
"  build  "  :   "  npm run build --workspaces --if-present  "  ,
"  test  "  :   "  npm run test --workspaces --if-present  "
}
}
```


To verify the graph is linked, run` npm ls -ws` or query it with` npm query .workspace` .


## The limits: what npm workspaces won’t do


npm workspaces resolves and links dependencies, but it does not run tasks in dependency order, cache build outputs, or compute an “affected” graph. If your app imports a library, you must build the library first —[running a script across workspaces will error when they depend on each other because npm does not execute in topological order](https://github.com/npm/cli/issues/4139) , an enhancement that remains open. Order it explicitly, or use[npm-run-all](https://github.com/mysticatea/npm-run-all) :


```text
{
"  scripts  "  : {
"  build:utils  "  :   "  npm run build -w @myorg/utils  "  ,
"  build:app  "  :   "  npm run build -w @myorg/app  "  ,
"  build  "  :   "  npm run build:utils && npm run build:app  "
}
}
```


Two more gotchas:


-


**Nested` node_modules` .** When two packages require incompatible versions of the same dependency, npm stops hoisting and installs a nested copy inside one package. Pin a single shared version with the root` overrides` field to keep the tree flat:


```text
{   "  overrides  "  : {   "  lodash  "  :   "  ^4.17.21  "   } }
```


-


**Install-script defaults are tightening.**[npm v12, estimated to release in July 2026](https://github.blog/changelog/2026-06-09-upcoming-breaking-changes-for-npm-v12/) , changes` allowScripts` to default off, so` npm install` will no longer run dependency` preinstall` ,` install` , or` postinstall` scripts unless explicitly allowed. If your workspaces rely on a` postinstall` or` prepare` build step, plan to approve it — these changes surface as warnings on npm 11.16.0 or newer so you can prepare early.


Note that “no native React/Vue/Vite integration” is a scope statement, not a defect: workspaces are framework-agnostic by design. Scaffolding apps is not their job.


## When to reach for Turborepo or Nx


Reach for[Turborepo](https://turborepo.com/docs) or[Nx](https://nx.dev/) *on top of* npm workspaces, not instead of it: npm resolves and links your packages, while those tools add task orchestration, caching, and affected-graph builds for larger repos. They’re complementary layers.


Concern npm workspaces Turborepo / Nx


Install & link packages ✅ Delegates to npm


Task dependency order ❌ manual scripts ✅ topological


Build/test caching ❌ ✅ local + remote


”Affected” builds ❌ ✅ change-based graph


Add one when ordered scripts get unwieldy, CI rebuilds everything on every change, or you want to run tasks only for packages a commit touched. Note that modern[Lerna is now Nx-backed](https://lerna.js.org/) — the old “npm + Lerna” advice has folded into this same layering.


npm workspaces covers roughly the first 80% of small-monorepo needs with zero extra tooling. Scaffold the two-file config, wire your flags, order your builds, and add an orchestrator only when the pipeline — not the dependency resolution — becomes the bottleneck. Run on an[Active LTS Node release](https://github.com/nodejs/Release) (Node 20 reached end-of-life on 2026-04-30) and confirm` npm -v` reports 7 or newer before you start.


## FAQs


Does npm workspaces still need one lockfile per package, or one at the root?


npm workspaces produces a single package-lock.json at the repository root, not one per package. A root npm install resolves every workspace's dependencies together and records them in that one lockfile, while individual packages get no node_modules directory of their own because dependencies hoist to the root. This single-lockfile model is what keeps versions consistent across all packages and is why you always run install from the root.


Why does 'npm run build --workspaces' fail when my packages depend on each other?


It fails because npm does not run workspace scripts in topological (dependency) order; it runs them in the order workspaces are listed, so a consumer can build before the library it imports exists, producing 'cannot find module' or failed-resolve errors. This remains an open npm enhancement (issue 4139). Fix it by defining explicit ordered scripts that build the library first, or by using a tool like npm-run-all, Turborepo, or Nx.


Can I use the 'workspace:*' protocol with npm like I do in pnpm or Yarn?


No. npm does not support the workspace: version protocol used by pnpm and Yarn Berry, and passing a workspace: specifier makes npm fail with EUNSUPPORTEDPROTOCOL (documented in npm/cli issue 8845). With npm, reference internal packages by their name and a normal range such as '@myorg/utils': '*'; npm symlinks them on install. If you migrate a pnpm or Yarn repo to npm, rewrite every workspace: specifier to a plain range.


Do I still need 'npm link' when using workspaces?


No. npm workspaces automate linking as part of npm install, symlinking each local package into the root node_modules by its name field, which removes the need to manually run npm link. Once a package lists a sibling as a dependency with a '*' range, a single root npm install wires the symlink, and edits to the source package are visible in every consumer immediately with no rebuild or republish.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
