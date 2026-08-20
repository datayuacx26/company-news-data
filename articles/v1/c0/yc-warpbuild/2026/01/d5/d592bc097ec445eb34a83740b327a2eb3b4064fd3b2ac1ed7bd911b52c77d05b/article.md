---
schema_version: "1.0.0"
document_id: "d592bc097ec445eb34a83740b327a2eb3b4064fd3b2ac1ed7bd911b52c77d05b"
company_key: "yc-warpbuild"
company: "WarpBuild"
source_id: "yc-warpbuild-news-import-6421ae0a6624"
canonical_url: "https://warpbuild.com/blog/github-actions-monorepo-guide"
published_at: "2026-01-13T00:00:00+00:00"
first_seen_at: "2026-07-22T19:23:06.881853+00:00"
fetched_at: "2026-07-28T22:23:44.568277+00:00"
content_hash: "sha256:c8030bcfd0ef19ec76f186a48f0fdb257e8c735a2f77bb8970d7297f67a4dca0"
---

# The Complete Guide to GitHub Actions for Monorepos: Turborepo, Nx, and pnpm Workspaces

## Key Takeaways


- **Run less, not faster.** Affected-only execution is the biggest lever. Running 4 packages instead of 45 beats any caching optimization.
- **Remote caching compounds.** Local caching helps one run. Remote caching lets every run share work across your team.
- **Matrix parallelism requires concurrency.** 30 matrix jobs on a 20-concurrency system just moves the bottleneck to queue time.
- **Pin your base SHA.** Dynamic affected detection can race with merges to main.


---


## Why Monorepos Break CI


Single-repo CI is predictable: code changes, tests run, build happens. Three jobs per PR, done.


Monorepos scale combinatorially. A 30-package repo with naive config runs all 30 test suites on every push. Add matrix testing across Node versions and you're at 60-90 jobs per PR. A one-line README fix triggers the same CI load as a core library rewrite.


Dependencies make it worse. Change package C and you need to test A and B too (they import it). Naive configs either test everything (wasteful) or only the changed package (misses downstream breakage).


[GitHub-hosted runners](https://docs.github.com/en/actions/using-github-hosted-runners/using-github-hosted-runners/about-github-hosted-runners) bottleneck this in two ways:


Constraint Impact


**Concurrency limits** Free: 20 jobs, Team: 60, Enterprise: up to 500. A 90-job PR queues constantly.


**Cache storage** Historically 10GB per repo. 30 packages with pnpm, dist, and test caches fill it fast. Caches evict, builds run cold.


Learn more about[common GitHub Actions challenges](https://www.warpbuild.com/blog/github-actions-challenges) and[caching strategies](https://www.warpbuild.com/blog/github-actions-cache) .


---


## Affected-Only Execution


Detect which packages changed, run CI only for those plus their dependents. This is the difference between 90 jobs and 8 jobs per PR.


### The Wrong Way


```text
# Don't do this - runs all 50 packages on every push
jobs  :
test  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   run  :   bun install
-   run  :   bun test
```


### The Right Way


The` --filter` flag tells Turborepo to only run tasks for packages changed since` origin/main` . The` ...` syntax includes dependents.


```text
jobs  :
test  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
with  :
fetch-depth  :   0   # Required for git comparison
-   run  :   bun install
-   run  :   bunx turbo run test --filter='...[origin/main...HEAD]'
```


See the[Turborepo GitHub Actions guide](https://turborepo.dev/docs/guides/ci-vendors/github-actions) .


Nx uses` NX_BASE` and` NX_HEAD` environment variables. The[nrwl/nx-set-shas](https://github.com/nrwl/nx-set-shas) action sets these correctly for PRs and push events.


```text
jobs  :
test  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
with  :
fetch-depth  :   0
-   uses  :   nrwl/nx-set-shas@v4
-   run  :   bun install
-   run  :   bunx nx affected -t test --base=$NX_BASE --head=$NX_HEAD
```


See the[Nx affected command docs](https://nx.dev/docs/features/ci-features/affected) .


### Common Footguns


Shallow clones break affected detection


Both tools need git history to compare changes. Default` actions/checkout` does a shallow clone with` fetch-depth: 1` . You need` fetch-depth: 0` for full history, or enough depth to reach your merge-base.


PR vs push behavior differs


On a PR, compare against the base branch. On push to main, compare against the previous commit. Turborepo handles this with` \[origin/main...HEAD\]` . Nx requires the` nx-set-shas` action.


Forks don't have the base branch ref


PRs from forks may need explicit fetch:


```text
-   run  :   git fetch origin main:main
```


Root changes affect everything


Changes to` package.json` ,` turbo.json` , or` bun.lockb` at the root might affect all packages. Both tools handle this, but verify your config triggers full CI when needed.


For explicit control over root changes:


```text
-   id  :   check-root
run  :   |
if git diff --name-only origin/main...HEAD | grep -qE '^(package\.json|turbo\.json|bun\.lockb)$'; then
echo "root_changed=true" >> $GITHUB_OUTPUT
else
echo "root_changed=false" >> $GITHUB_OUTPUT
fi


-   name  :   Run affected tests
if  :   steps.check-root.outputs.root_changed != 'true'
run  :   bunx turbo run test --filter='...[origin/main...HEAD]'


-   name  :   Run all tests (root changed)
if  :   steps.check-root.outputs.root_changed == 'true'
run  :   bunx turbo run test
```


---


## Caching That Works


Dependency caching is necessary but not sufficient. Build artifacts are where real time savings live.


### Dependency Caching


**bun** is excellent for monorepos. Its binary lockfile and global cache mean fast installs with minimal overhead.


```text
-   uses  :   oven-sh/setup-bun@v2
-   uses  :   actions/cache@v4
with  :
path  :   ~/.bun/install/cache
key  :   bun-${{ runner.os }}-${{ hashFiles('bun.lockb') }}
restore-keys  :   bun-${{ runner.os }}-
-   run  :   bun install
```


**pnpm** 's content-addressable store means identical dependencies across packages are stored once:


```text
-   uses  :   pnpm/action-setup@v2
with  :
version  :   9
-   uses  :   actions/setup-node@v4
with  :
node-version  :   '22'
cache  :   'pnpm'
```


### Build Artifact Caching


For TypeScript monorepos, cache` dist` folders and` .tsbuildinfo` files:


```text
-   uses  :   actions/cache@v4
with  :
path  :   |
packages/**/dist
packages/**/.tsbuildinfo
key  :   build-${{ runner.os }}-${{ hashFiles('packages/**/src/**', 'packages/**/tsconfig.json') }}
restore-keys  :   build-${{ runner.os }}-
```


Cache key strategy


Use` hashFiles` on source content, not` github.sha` . Every commit has a different SHA, so you'd almost never hit cache. Content-based keys mean identical source produces hits regardless of commit.


The` restore-keys` fallback means partial hits still help—you get a recent build even if today's exact hash isn't cached.


### Why Naive Caching Falls Short


- **Key granularity matters.** One key for all packages = any change invalidates everything. Per-package keys = 50 cache operations per job.
- **Size limits bite.** A 30-package TypeScript monorepo can exceed storage limits fast. Caches evict under LRU. Jobs run cold.
- **Restore time scales with size.** A 2GB cache takes 30-60 seconds to restore. If your build only takes 90 seconds, you've added 50% overhead.


---


## Matrix Strategies for Parallel Testing


Once you've detected affected packages, parallelize their execution across runners.


### Dynamic Matrix Generation


Generate the matrix from affected packages, not hardcoded:


```text
jobs  :
detect  :
runs-on  :   ubuntu-latest
outputs  :
packages  :   ${{ steps.affected.outputs.packages }}
base_sha  :   ${{ steps.set-base.outputs.base_sha }}
steps  :
-   uses  :   actions/checkout@v4
with  :
fetch-depth  :   0
-   id  :   set-base
run  :   |
BASE_SHA=$(git merge-base origin/main HEAD)
echo "base_sha=$BASE_SHA" >> $GITHUB_OUTPUT
-   run  :   bun install
-   id  :   affected
run  :   |
PACKAGES=$(bunx turbo run test --filter='...[${{ steps.set-base.outputs.base_sha }}...HEAD]' --dry-run=json | jq -c '[.tasks[].package] | unique')
echo "packages=$PACKAGES" >> $GITHUB_OUTPUT


test  :
needs  :   detect
if  :   ${{ needs.detect.outputs.packages != '[]' }}
strategy  :
matrix  :
package  :   ${{ fromJson(needs.detect.outputs.packages) }}
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   run  :   bun install
-   run  :   bunx turbo run test --filter=${{ matrix.package }}
```


Why pin the base SHA?


Between the detect job and test jobs, someone might merge to main. If you use` origin/main` directly, the affected calculation might not match reality when tests run. Pinning the merge-base SHA ensures consistency.


### When Matrix Helps vs Hurts


Matrix helps when... Matrix hurts when...


Package tests are slow (>2 min) Package tests are fast (<30 sec)


You have concurrency headroom Job startup exceeds test time


Tests are independent Concurrency limits mean jobs queue anyway


For fast tests, a single job running all affected packages sequentially might be faster than 10 matrix jobs each spending 30 seconds on setup.


### Test Sharding Within Packages


If one package has 80% of your test time, shard within it. Learn more about[running concurrent tests effectively](https://www.warpbuild.com/blog/concurrent-tests) :


```text
strategy  :
matrix  :
package  :   ${{ fromJson(needs.detect.outputs.packages) }}
shard  : [  1  ,   2  ,   3  ,   4  ]
steps  :
-   run  :   bunx turbo run test --filter=${{ matrix.package }} -- --shard=${{ matrix.shard }}/4
```


A 12-minute test suite becomes 3 minutes wall-clock (with sufficient concurrency).


### Concurrency Reality Check


30 jobs at 2 minutes each on 20 concurrent slots:


- First batch: 20 jobs run (2 min)
- Second batch: 10 jobs run (2 min)
- **Total: 4 minutes** instead of 2 with unlimited concurrency


This is where infrastructure becomes the bottleneck, not configuration.


---


## Remote Caching


[Turborepo's remote cache](https://turbo.build/repo/docs/core-concepts/remote-caching) shares build artifacts across CI runs. PR #2 doesn't rebuild what PR #1 already built.


### Why It Matters for CI


Without remote caching, every CI run starts cold. With it, PRs share work. A team running 50 PRs/day with 30-minute builds can save 20+ hours daily at 80% hit rate.


```text
jobs  :
build  :
runs-on  :   ubuntu-latest
env  :
TURBO_TOKEN  :   ${{ secrets.TURBO_TOKEN }}
TURBO_TEAM  :   ${{ vars.TURBO_TEAM }}
steps  :
-   uses  :   actions/checkout@v4
with  :
fetch-depth  :   0
-   uses  :   oven-sh/setup-bun@v2
-   run  :   bun install
-   run  :   bunx turbo run build test lint --filter='...[origin/main...HEAD]'
```


The environment variables authenticate with Vercel's remote cache. No additional config needed.


```text
env  :
TURBO_API  :   'https://your-cache-server.com'
TURBO_TOKEN  :   ${{ secrets.TURBO_TOKEN }}
TURBO_TEAM  :   'your-team'
```


You can self-host with S3, GCS, or other storage backends.


---


## Nx Commands Reference


### Affected vs run-many


```text
# Affected projects only (use for PR CI)
-   run  :   bunx nx affected -t test


# All projects (use for nightly/release builds)
-   run  :   bunx nx run-many -t test --all


# Specific projects
-   run  :   bunx nx run-many -t test --projects=app1,app2
```


The` --parallel` flag controls tasks within a single job (different from matrix parallelism across jobs):


```text
-   run  :   bunx nx affected -t lint test build --parallel=3
```


### Nx Cloud Distributed Execution


[Nx Cloud DTE](https://nx.dev/ci/features/distribute-task-execution) automatically distributes tasks across multiple agents:


```text
jobs  :
main  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
with  :
fetch-depth  :   0
-   uses  :   nrwl/nx-set-shas@v4
-   run  :   bun install
-   run  :   bunx nx-cloud start-ci-run --distribute-on="5 linux-medium-js"
-   run  :   bunx nx affected -t lint test build


agents  :
runs-on  :   ubuntu-latest
strategy  :
matrix  :
agent  : [  1  ,   2  ,   3  ,   4  ,   5  ]
steps  :
-   uses  :   actions/checkout@v4
-   run  :   bun install
-   run  :   bunx nx-cloud start-agent
```


For 100+ package monorepos, DTE can reduce CI from hours to minutes.


### Turborepo vs Nx


Turborepo Nx


**Setup** Simpler, faster to adopt More configuration options


**Cold start** Faster (~5MB runtime) Larger (~40MB)


**Remote cache** Vercel integration or self-host Nx Cloud


**Distributed execution** Manual with matrix Built-in with Nx Cloud


**Polyglot support** JS/TS focused Go, Rust, Java, etc.


**Best for** Under 50 packages, JS/TS 100+ packages, polyglot


Both integrate cleanly with GitHub Actions. Choose based on scale and whether you need distributed execution.


---


## Measuring Performance


Baseline commands


Run these before optimizing to quantify your opportunity:


```text
# How many tasks run today?
bunx   turbo   run   test   --dry-run   |   grep   "Tasks:"


# How many with affected-only?
bunx   turbo   run   test   --filter=  '...[origin/main...HEAD]'   --dry-run   |   grep   "Tasks:"
```


If full CI runs 45 tasks and affected runs 4, you're doing 10x more work than necessary.


### Key Metrics


Metric How to measure Target


**Affected ratio** Tasks with filter vs without Under 20% of total


**Cache hit rate** Run same build twice, count` FULL TURBO` Above 80%


**Queue time** "Queued" vs "In progress" timestamp Under 30 sec average


**Wall-clock vs CI minutes** Total time vs sum of job times High wall-clock + low minutes = queue saturation


---


## When Infrastructure Is the Bottleneck


Configuration optimization has limits. At some point, infrastructure is the constraint.


### Signs You've Hit the Limit


- Jobs queue even with optimized configs
- Cache operations take longer than builds
- Matrix strategies don't improve wall-clock time
- Cost scales linearly despite optimizations


### What Moves the Needle


1. **Affected-only execution** — configuration
2. **Remote caching** — configuration + service
3. **Unlimited concurrency** — infrastructure
4. **Faster cache I/O** — infrastructure
5. **Faster runners** — infrastructure


[GitHub-hosted runner limits](https://docs.github.com/en/actions/reference/limits) vary by plan: Free (20), Pro (40), Team (60), Enterprise (up to 500). Cache storage has historically been 10GB per repo.


WarpBuild removes these constraints. Unlimited concurrency means matrix strategies actually parallelize. 50GB+ cache storage means caches don't evict. Change` runs-on: ubuntu-latest` to` runs-on: warpbuild-ubuntu-22.04-x64-4x` and the infrastructure constraints disappear.


---


## What To Do Next


### Measure your waste


```text
bunx   turbo   run   test   --dry-run   |   grep   "Tasks:"
```


The difference is your optimization opportunity.


### Check cache hit rate


Look for` FULL TURBO` (hit) vs` cache miss` in Turborepo output. Below 80% on repeated runs means cache keys or storage need attention.


### Calculate queue time


In GitHub Actions, compare "Queued" to "In progress" timestamps. Over 30 seconds average means you're hitting concurrency limits.


### Implement affected-only execution


Start with` --filter='...\[origin/main...HEAD\]'` for Turborepo or` nx affected` for Nx. Pin your base SHA to avoid race conditions.


### Enable remote caching


Vercel's Turborepo cache or Nx Cloud. Setup takes 10 minutes. Run the same build twice and watch the second complete in seconds.


### Evaluate infrastructure


If you've optimized config and still hit limits, the constraint is infrastructure: unlimited concurrency and faster cache I/O are the next lever.


---


*Need unlimited concurrency and faster caching for your monorepo? WarpBuild removes GitHub's infrastructure constraints with a single line change.[Start free →](https://www.warpbuild.com/)*
