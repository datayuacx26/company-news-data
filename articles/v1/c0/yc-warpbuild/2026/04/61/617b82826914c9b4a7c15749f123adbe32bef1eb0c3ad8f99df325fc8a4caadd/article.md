---
schema_version: "1.0.0"
document_id: "617b82826914c9b4a7c15749f123adbe32bef1eb0c3ad8f99df325fc8a4caadd"
company_key: "yc-warpbuild"
company: "WarpBuild"
source_id: "yc-warpbuild-news-import-6421ae0a6624"
canonical_url: "https://warpbuild.com/blog/cirrus-ci-shutting-down"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-07-22T19:23:06.881853+00:00"
fetched_at: "2026-07-28T22:15:58.980681+00:00"
content_hash: "sha256:514744b9d1de01287ee9c961a95e69b6aedcba4337287f788625d975bdeba070"
---

# Cirrus CI Is Shutting Down: Migrate to GitHub Actions + WarpBuild

On April 7, 2026, Cirrus Labs[announced](https://cirruslabs.org/) that Cirrus CI is shutting down. The service will stop running jobs on **June 1, 2026** , as the Cirrus Labs team joins OpenAI's Agent Infrastructure group. If you're a Cirrus CI user, you need to migrate your workflows before the deadline.


Cirrus CI earned its place in the CI landscape. It was one of the first platforms to offer affordable macOS CI on Apple Silicon through[Tart](https://github.com/cirruslabs/tart) , their open-source virtualization tool. Per-second billing, no concurrency limits, and generous free tiers for open source made it a home for projects like PostgreSQL, Bitcoin Core, and Podman. The team genuinely cared about developer experience, and it showed.


The good news: Cirrus Labs is relicensing Tart, Vetu, and Orchard under more permissive licenses, so the community retains those tools. The less good news: no migration guidance was provided. If your CI depends on` .cirrus.yml` , it will stop working on June 1.


Here's how to move forward.


## What This Means for Your Team


- **June 1, 2026:** Cirrus CI stops running jobs entirely.
- **Cirrus Runners:** No longer accepting new customers. Existing contracts honored through their periods.
- **Your` .cirrus.yml` workflows will break** after the shutdown date. Unlike other recent CI provider shutdowns that only required swapping a runner label, this one requires migrating your entire CI configuration to a new platform.


You have two decisions to make: which CI platform and which runner infrastructure. Our recommendation: **GitHub Actions** as the CI platform, and **WarpBuild** for the runner infrastructure.


## Why GitHub Actions + WarpBuild


GitHub Actions has become the default CI platform for teams on GitHub. The ecosystem is massive - over 50,000 reusable actions in the marketplace, native integration with pull requests, issues, deployments, and OIDC-based authentication. Most CI tooling ships GitHub Actions support first.


If you were on Cirrus CI, you chose it for specific reasons: macOS support, per-second billing, no concurrency limits, open-source friendliness. GitHub Actions with WarpBuild runners gives you all of that - and more.


Here's how the key Cirrus CI features map over:


Cirrus CI GitHub Actions + WarpBuild


Per-second billing, no concurrency limits Per-minute billing, **unlimited concurrency** - no caps, no add-on pricing


macOS M1 VMs (Tart) **macOS M4 Pro** - three generations newer, 30%+ faster


Linux containers Linux x86-64 and ARM64 runners with NVMe SSDs


Windows containers Windows Server 2022 and 2025 runners


Built-in caching **Unlimited cache** with 7-day retention via` actions/cache`


Persistent workers **BYOC** - full cloud-account integration on AWS, GCP, Azure


Free for open source GitHub Actions free minutes for public repos


What you gain that Cirrus CI never offered:[snapshot runners](https://www.warpbuild.com/docs/ci/features/snapshot-runners) (2-10x faster Linux builds), a 50,000+ action marketplace, reusable workflows, matrix builds with native syntax, and GitHub Environments with deployment protection rules.


## macOS Runners - M4 Pro, Three Generations Ahead


If you used Cirrus CI for macOS builds, this is the most important section.


Cirrus CI ran macOS VMs on M1 hardware via Tart. WarpBuild runs on **Apple M4 Pro** - three generations newer with a PassMark single-core score of 4625, 30% faster than M2 Pro and significantly faster than M1. Builds that took 10 minutes on Cirrus CI's M1 runners typically complete in 6-7 minutes on WarpBuild's M4 Pro, meaning the total cost per build is often lower despite a higher per-minute rate.


Runner CPU Memory Storage Price


` warp-macos-15-arm64-6x` 6 vCPU 22GB 120GB SSD $0.08/min


` warp-macos-15-arm64-12x` 12 vCPU 44GB 270GB SSD $0.16/min


WarpBuild supports macOS 13, 14, 15, and 26, with Xcode, iOS Simulator, Fastlane, and CocoaPods pre-installed. These runners are[60% faster than GitHub's macos-latest-xlarge and 2x cheaper](https://www.warpbuild.com/products/ci/macos-runners) .


macOS hardware comparison


Cirrus CI: Apple M1 (Tart VMs) → WarpBuild: Apple M4 Pro - 60% faster than GitHub-hosted M1 runners, up to 8x faster than Intel-based runners. For iOS and macOS builds, this is the single biggest upgrade available today.


## What Else WarpBuild Brings


### Snapshot Runners: 2-10x Faster Linux Builds


This is something Cirrus CI never had. WarpBuild[snapshot runners](https://www.warpbuild.com/docs/ci/features/snapshot-runners) save and restore the entire VM state between builds. Instead of reinstalling dependencies, rebuilding caches, and setting up environments from scratch every time, your CI starts exactly where it left off. Currently available for Linux runners, snapshots are especially powerful for builds with heavy dependency trees - cutting build times by 2-10x.


### Unlimited Concurrency, No Credit System


Cirrus CI used a credit-based pricing model - buy credits, spend them across different runner types at different rates. WarpBuild uses flat per-minute pricing with no concurrency caps. Run as many parallel jobs as your workflows need. No credits to manage, no surprise bills when your team scales up.


When you factor in faster hardware (and snapshot runners for Linux workloads), the cost per build on WarpBuild is typically lower than what you were paying on Cirrus CI. Fewer minutes per build means lower total spend.


### Unlimited Cache


WarpBuild provides[unlimited cache storage](https://www.warpbuild.com/docs/ci/features/caching) with 7-day retention from last access. It works with the` WarpBuilds/cache` action - no proprietary tooling. WarpBuild has none.


### Bring Your Own Cloud (BYOC)


Cirrus CI offered persistent workers for self-hosted setups, but not full cloud-account integration.[WarpBuild BYOC](https://www.warpbuild.com/docs/ci/byoc) lets you run CI runners in your own AWS, GCP, or Azure account. WarpBuild manages the control plane while runners execute in your infrastructure - giving you up to 10x cost savings compared to hosted options, plus static IPs, custom machine images, and full network control.


### Full OS and Architecture Coverage


- **Linux:** x86-64 and ARM64, Ubuntu 22.04 and 24.04, up to 32 vCPU
- **Windows:** Server 2022 and 2025, up to 32 vCPU
- **macOS:** 13, 14, 15, 26 on M4 Pro


### Enterprise Ready


WarpBuild is[SOC2 Type 2 compliant](https://www.warpbuild.com/blog/soc2) , supports SSO through Microsoft Entra ID, Google, Okta, Auth0, and JumpCloud, and offers a 99.9% SLA with dedicated support across 29+ regions globally.


## Migrating from Cirrus CI to GitHub Actions + WarpBuild


This migration is bigger than a runner label swap - you're moving from Cirrus CI's` .cirrus.yml` configuration format to GitHub Actions workflow files. For simple setups, expect a few hours. For complex multi-platform configurations, plan for a day or two.


### Step 1: Sign Up and Install the WarpBuild Bot


Create an account at[app.warpbuild.com](https://app.warpbuild.com/) and install the WarpBuild GitHub bot on your repositories. See the[quick start guide](https://www.warpbuild.com/docs/ci/quick-start) for details.


### Step 2: Convert Your .cirrus.yml to GitHub Actions Workflows


Here's how Cirrus CI concepts map to GitHub Actions:


Cirrus CI GitHub Actions


` task`` jobs.<job_id>`


` script`` jobs.<job_id>.steps\[*\].run`


` cache`` actions/cache@v4`


` matrix`` jobs.<job_id>.strategy.matrix`


` depends_on`` jobs.<job_id>.needs`


` only_if` /` skip`` jobs.<job_id>.if`


` env`` env` (workflow or job level)


` osx_instance`` runs-on: warp-macos-15-arm64-6x`


` container`` jobs.<job_id>.container`


` arm_container`` runs-on: warp-ubuntu-latest-arm64-4x`


` windows_container`` runs-on: warp-windows-latest-x64-4x`


Here's a real-world before and after. A typical Cirrus CI config with macOS and Linux tasks:


.cirrus.yml (Before - Cirrus CI)


```text
macos_task  :
osx_instance  :
image  :   ghcr.io/cirruslabs/macos-runner:sonoma
env  :
SCHEME  :   MyApp
build_script  :
-   xcodebuild -scheme $SCHEME -sdk iphoneos build
test_script  :
-   xcodebuild -scheme $SCHEME -sdk iphonesimulator test


linux_task  :
container  :
image  :   node:20
node_modules_cache  :
folder  :   node_modules
fingerprint_script  :   cat package-lock.json
install_script  :
-   npm ci
test_script  :
-   npm test
```


.github/workflows/ci.yml (After - GitHub Actions + WarpBuild)


```text
name  :   CI
on  : [  push  ,   pull_request  ]


jobs  :
macos-build  :
runs-on  :   warp-macos-15-arm64-6x
env  :
SCHEME  :   MyApp
steps  :
-   uses  :   actions/checkout@v4
-   name  :   Build
run  :   xcodebuild -scheme $SCHEME -sdk iphoneos build
-   name  :   Test
run  :   xcodebuild -scheme $SCHEME -sdk iphonesimulator test


linux-test  :
runs-on  :   warp-ubuntu-latest-x64-4x
container  :
image  :   node:20
steps  :
-   uses  :   actions/checkout@v4
-   uses  :   actions/cache@v4
with  :
path  :   node_modules
key  :   ${{ runner.os }}-node-${{ hashFiles('package-lock.json') }}
-   name  :   Install
run  :   npm ci
-   name  :   Test
run  :   npm test
```


For a full list of available runner labels and configurations, see the[cloud runners documentation](https://www.warpbuild.com/docs/ci/cloud-runners) .


### Step 3: Set Up Caching


Cirrus CI had built-in` cache` directives with folder and fingerprint configuration. In GitHub Actions, use` actions/cache@v4` with WarpBuild's unlimited cache backend:


Caching in GitHub Actions


```text
-   uses  :   actions/cache@v4
with  :
path  :   |
~/.cache/pip
node_modules
key  :   ${{ runner.os }}-deps-${{ hashFiles('**/package-lock.json') }}
restore-keys  :   |
${{ runner.os }}-deps-
```


### Step 4: Migrate Secrets and Environment Variables


Cirrus CI used encrypted variables in` .cirrus.yml` . In GitHub Actions, store secrets in **Settings → Secrets and variables → Actions** at the repository or organization level. Reference them in workflows as` ${{ secrets.MY_SECRET }}` .


For deployment-specific secrets, use[GitHub Environments](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment) with protection rules - something Cirrus CI didn't offer natively.


### Step 5: Enable Snapshot Runners for Linux (Optional)


Once your Linux workflows are running on WarpBuild, you can enable[snapshot runners](https://www.warpbuild.com/docs/ci/features/snapshot-runners) for an additional 2-10x speedup. This saves your full VM state - installed dependencies, compiled caches, configured tools - and restores it on the next run. Snapshots are currently available for Linux runners and are especially powerful for builds with heavy dependency installation steps.


## What About Cirrus CI's Open Source Tools?


Cirrus Labs is relicensing Tart, Vetu, and Orchard under more permissive licenses and has stopped charging licensing fees. If you self-hosted macOS VMs using Tart, those tools remain available and the community can continue to build on them.


But if you used Cirrus CI's managed infrastructure and don't want the operational burden of running your own macOS VM fleet, WarpBuild handles all of that - on newer M4 Pro hardware, without any infrastructure maintenance on your end.


## Frequently Asked Questions


**When does Cirrus CI shut down?**


Cirrus CI stops running jobs on **June 1, 2026** . All` .cirrus.yml` workflows will stop working after this date.


**What is happening to Cirrus Labs?**


The Cirrus Labs team is joining OpenAI's Agent Infrastructure team. Their open-source tools (Tart, Vetu, Orchard) will continue under more permissive licenses.


**What about Cirrus Runners?**


Cirrus Runners is no longer accepting new customers. Existing customers will have their contracts honored through their contracted periods, but the service is winding down alongside the CI platform.


**Is WarpBuild a drop-in replacement for Cirrus CI?**


Not exactly. Cirrus CI was a standalone CI platform with its own` .cirrus.yml` configuration. WarpBuild provides fast runner infrastructure for **GitHub Actions** . You'll need to migrate your CI configuration to GitHub Actions workflow files and use WarpBuild runner labels - this post provides astep-by-step guide .


**Does WarpBuild support macOS runners?**


Yes. WarpBuild provides macOS runners on **Apple M4 Pro** hardware - three generations newer than Cirrus CI's M1-based Tart VMs. Available in 6 vCPU ($0.08/min) and 12 vCPU ($0.16/min) configurations, supporting macOS 13, 14, 15, and 26. See the[macOS runners page](https://www.warpbuild.com/products/ci/macos-runners) for details.


**How long does migration take?**


For simple workflows with a few tasks, a few hours. For complex multi-platform configurations with many tasks, matrix builds, and custom scripts, plan for 1-2 days. The main effort is converting` .cirrus.yml` syntax to GitHub Actions workflow syntax - once that's done, pointing at WarpBuild runners is a one-line change.


## Get Started


Cirrus CI served the developer community well for nearly a decade. WarpBuild is where that journey continues - with M4 Pro macOS runners, snapshot builds, unlimited concurrency, and infrastructure that's actively improving every week.


Don't wait until June 1.[Sign up for WarpBuild](https://app.warpbuild.com/) and start migrating your workflows today. The[quick start guide](https://www.warpbuild.com/docs/ci/quick-start) will get you running in under 10 minutes, and the[cloud runners documentation](https://www.warpbuild.com/docs/ci/cloud-runners) has the full list of available runner configurations.


Need help migrating a complex Cirrus CI setup? Reach out to[\[email protected\]](https://www.warpbuild.com/cdn-cgi/l/email-protection#63101613130c1117231402111301160a0f074d000c0e) - we're happy to assist.
