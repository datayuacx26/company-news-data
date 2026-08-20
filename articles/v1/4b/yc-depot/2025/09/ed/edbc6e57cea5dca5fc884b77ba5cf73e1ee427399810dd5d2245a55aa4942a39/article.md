---
schema_version: "1.0.0"
document_id: "edbc6e57cea5dca5fc884b77ba5cf73e1ee427399810dd5d2245a55aa4942a39"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/we-analyzed-66821-github-actions-runs"
published_at: "2025-09-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:dea19197335e85c015725e01d67fb53e4e554d26d57184f8f39437b2faa4d32b"
---

# We analyzed 66,821 GitHub Actions runs: 9 hidden gems you're missing

Do you stick to official GitHub Actions in your workflows? Makes sense. Why risk your CI pipeline on some random third-party action?


Turns out, the biggest CI wins could be hiding in the actions marketplace, where other developers have solved the exact problems you're facing.


We analyzed 66,821 workflow runs across all organizations using[Depot's runners](https://depot.dev/docs/github-actions/runner-types) to uncover the third-party GitHub Actions ecosystem. The results revealed some hidden gems that could transform your workflows.


## Scope of third-party adoption


- **189** third-party actions with multi-organization adoption (we filtered out single-organization actions to focus on broadly useful tools)
- **63** authors/organizations


As you’d expect, the actions run the gamut from specialized tools for specific languages to general productivity enhancers.


## 9 third-party actions that are worth your time


[Depot customers](https://depot.dev/customers) are concerned about CI speed and naturally have adopted actions to speed up performance. Here are 9 actions with relatively low adoption that deliver significant value.


### 1. Build step optimization


[dorny/paths-filter](https://github.com/dorny/paths-filter) - *Used by 11% of organizations*


I think this is super cool. This action detects which files changed in a PR and sets outputs you can use to conditionally run jobs. It’s great when you want to control running individual jobs or steps only when certain file changes happen.


```text
-   uses  :   dorny/paths-filter@v3
id  :   changes
with  :
filters  :   |
backend:
- 'src/api/**'
frontend:
- 'src/web/**'


-   name  :   Run backend tests
if  :   steps.changes.outputs.backend == 'true'
run  :   pnpm run test:api
```


### 2. Fast Python package management


[astral-sh/setup-uv](https://github.com/astral-sh/setup-uv) - *Used by 7% of organizations*


Installing Python dependencies can take a really long time. Astral’s uv comes to the rescue.


For my most recent Python project, I’ve switched over to uv and am getting about 6x faster installs. It’s great. By default the` astral-sh/setup-uv` action caches and that makes things even better.


```text
-   uses  :   astral-sh/setup-uv@v6
-   name  :   Install dependencies
run  :   uv pip install -r requirements.txt
```


### 3. Compilation caching


[mozilla-actions/sccache-action](https://github.com/Mozilla-Actions/sccache-action) - *Used by 6% of organizations*


We're always trying to speed up compiles. This action speeds up compilation for Rust, C++, and other compiled languages by caching compilation results across CI runs. Depot runners are already set up to[use sccache to speed up builds](https://depot.dev/blog/sccache-in-github-actions#enter-sccache) .


For rocker, my Rust rewrite of docker, using sccache cut the build and test time by half.


```text
-   uses  :   mozilla-actions/sccache-action@v0.0.9
-   name  :   Build project
run  :   RUSTC_WRAPPER=sccache cargo build --release
```


### 4. System package caching


[awalsh128/cache-apt-pkgs-action](https://github.com/awalsh128/cache-apt-pkgs-action) - *Used by 2% of organizations*


In CI, installing packages from` apt` can take a long time. This action can cache packages eliminating repeated package downloads and installations. There are[caveats](https://github.com/awalsh128/cache-apt-pkgs-action/tree/master?tab=readme-ov-file#caveats) if your package has pre- or post-scripts. Well worth a try.


```text
-   uses  :   awalsh128/cache-apt-pkgs-action@v1
with  :
packages  :   libssl-dev
```


### 5. Robust CI pipelines


[nick-fields/retry](https://github.com/nick-fields/retry) - *Used by 4% of organizations*


As much as we engineers don’t want it to be true, it’s not uncommon for tests to be flaky.` nick-fields/retry` can automatically retry failed steps with configurable backoff. I’m not exactly sure how to categorize this one; feels like it removes some of the need to restart CI runs, so, performance? In any case, it might be controversial, but it’s definitely pragmatic.


```text
-   uses  :   nick-fields/retry@v3
with  :
timeout_minutes  :   10
max_attempts  :   3
command  :   pnpm run integration-tests
```


### 6. Better PR feedback


[marocchino/sticky-pull-request-comment](https://github.com/marocchino/sticky-pull-request-comment) - *Used by 4% of organizations*


Updates a single comment on PRs. This is nice as it can help reviewers get context from a PR instead of digging through CI logs. This example shows how to put the contents of a file as a comment in the PR.


```text
-   uses  :   marocchino/sticky-pull-request-comment@v2
with  :
path  :   coverage-results.md
```


### 7. Beautiful test result reports


[dorny/test-reporter](https://github.com/dorny/test-reporter) - *Used by 3% of organizations*


Another nice action by` dorny` . With this test failures are immediately visible in PR checks with detailed context. For me, it is pretty painful to search through go test logs for the word` fail` . Too many tests or logs have that as their name! Here is a way to get a nice simplified view. I should note that this supports more than go tests!


```text
-   name  :   Run tests
run  :   go test -json ./... > testresults.json
-   name  :   Test Report
uses  :   dorny/test-reporter@v2
with  :
name  :   Go Tests
path  :   testresults.json
reporter  :   golang-json
```


### 8. Universal binary installer


[taiki-e/install-action](https://github.com/taiki-e/install-action) - *Used by 3% of organizations*


This simplifies and speeds up getting the right tools into the CI environment. It installs precompiled binaries from GitHub releases with automatic caching and platform detection. The GitHub repo includes a[list](https://github.com/taiki-e/install-action/blob/main/TOOLS.md) of all the tools it supports ready to go. Really nice and simple.


```text
-   uses  :   taiki-e/install-action@v2
with  :
tool  :   cargo-nextest,just,cargo-hack
```


### 9. Enforce PR standards


[amannn/action-semantic-pull-request](https://github.com/amannn/action-semantic-pull-request) - *Used by 3% of organizations*


I mean, I like and use conventional commits, but I figure your commit message is your own. If you want to enforce semantic PR titles for automated changelogs and better commit history, you can use this.


```text
-   uses  :   amannn/action-semantic-pull-request@v5
with  :
types  :   |
fix
feat
docs
ci
chore
```


## Honorable mentions


The downside of having a really interesting dataset is that you have to pick only a few. I figured I’d share a few more metrics as bellwethers for our industry.


**AI and automation** :


- ` anthropics/claude-code-action` - AI-powered code review (4% adoption)


**Testing and quality** :


- ` chromaui/action` - Visual regression testing (3% adoption)
- ` cypress-io/github-action` - E2E testing with zero config (3% adoption)
- ` codecov/codecov-action` - Coverage reporting (7% adoption)


**Infrastructure** :


- ` pulumi/actions` - Infrastructure as code deployments (3% adoption)
- ` hashicorp/setup-terraform` - Terraform workflows (4% adoption)


**Security and secrets** :


- ` dopplerhq/cli-action` - Centralized secrets management (3% adoption)
- ` 1password/load-secrets-action` - 1Password integration (<1% adoption)


**Language-specific** :


- ` pnpm/action-setup` - Fast Node.js package management (17% adoption)
- ` ruby/setup-ruby` - Ruby environment setup (5% adoption)
- ` dtolnay/rust-toolchain` - Rust toolchain management (6% adoption)
- ` oven-sh/setup-bun` - Bun JavaScript runtime (6% adoption)


## The bottom line


The GitHub Actions third-party ecosystem has matured into a sophisticated toolkit where specialized solutions often outperform general-purpose alternatives. Maybe it's time to see what you're missing?


## Related posts


- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Comparing GitHub Actions and Depot runners for 2x faster builds](https://depot.dev/blog/comparing-github-actions-and-depot-runners-for-2x-faster-builds)
- [Faster Claude Code agents in GitHub Actions](https://depot.dev/blog/claude-code-in-github-actions)


Chris Goller


Principal Software Engineer at Depot
