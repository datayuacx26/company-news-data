---
schema_version: "1.0.0"
document_id: "8bd88197fc2fd50b99e484b35198fb525e3311035b23c96249f6ed75c7ebcbe8"
company_key: "yc-starsling"
company: "StarSling"
source_id: "yc-starsling-atom-ebb22850b8ad"
canonical_url: "https://starsling.dev/blog/introducing-ci-secure-an-ai-coding-agent-skill-that-closes-the-critical-attack-vectors-in-your-github-actions"
published_at: "2026-08-13T17:01:34+00:00"
first_seen_at: "2026-08-13T09:21:18.511395+00:00"
fetched_at: "2026-08-13T17:01:35.627893+00:00"
content_hash: "sha256:2c07029523fd42278014f6de84befee576ac47e4ab9d643f5953ff77ad790800"
---

# Introducing ci-secure: an AI coding agent skill that closes the critical attack vectors in your GitHub Actions

Today we're releasing[ci-secure](https://starsling.dev/ci-secure) , a free and[open-source](https://github.com/starslingdev/skills) agent skill that scans your GitHub Actions workflows for the ten critical CI/CD attack vectors and hands your coding agent a fix for every one it finds. It works with all coding agents supported by[Vercel's Skills CLI](https://github.com/vercel-labs/skills#supported-agents) , including OpenCode, Claude Code, Codex, and Cursor.


Point it at a local checkout and it reads every file under` .github/workflows` , matches each workflow against the ten vectors, and reports every finding as a plain-English attacker scenario: the workflow, the line it starts from, and what an attacker could actually do from there. Then it asks which findings you want closed and applies each fix to your working tree for you to review. The scanner is deterministic and runs entirely on your machine; its one dependency is PyYAML.


## What ci-secure does


It works in four steps:


- **Scans every workflow.** The scanner parses your workflow YAML locally. Nothing is uploaded anywhere; the only network calls are the optional impostor-SHA check, the one vector that cannot be answered from YAML alone, and the dormancy lookup that tells you when a finding sits in a workflow that has not run lately. Both go to GitHub under your own` gh` login, and when the SHA check cannot run the report says so.
- **Matches the ten vectors.** Deterministic detectors test each workflow against the ten vectors listed below, the same ten every time. Nothing else is checked, and nothing on the list is skipped.
- **Explains each finding as an attacker scenario.** Not a rule id and a severity color: what an attacker could actually do, in plain English, with the workflow and the line it starts from. Zero findings is a first-class result, and a check that could not run is reported as skipped rather than passing silently.
- **Fixes what you pick.** The skill asks which findings to close and applies each fix to your working tree: change a trigger, quote an interpolation, re-pin a SHA, add` --ignore-scripts` to an install. You review the diff. It never commits, pushes, or opens a PR on its own.


## What it deliberately is not


Critical exploit-chain checks only: this is not a comprehensive security audit.


It checks exactly ten things, and every one of them has three properties in common:


- **An outsider can actually reach it.** Each vector starts from someone with no access to your repo, an account that can open a fork PR, craft a branch name, or publish an upstream artifact, and ends at a concrete compromise: code execution holding a write token, secret theft, or poisoning what you ship.
- **It has actually happened.** Every vector on the list is backed by a named public incident or published security research. Each entry below names its evidence; nothing here is theoretical.
- **You can fix it today.** Every finding closes with a change you can land the day you read it: change a trigger, remove an interpolation, re-pin a SHA, drop a permission line. Nothing requires a hardening program.


Everything broader than these attack paths, grading how your CI configuration is set up overall, is the[CI Score](https://starsling.dev/ci-score) 's job.


## The ten vectors, and the incidents behind them


- **Template injection.** Attacker-controlled text, a PR title or a branch name, is pasted straight into a shell step and runs as your workflow. *([nx / s1ngularity 2025](https://www.wiz.io/blog/s1ngularity-supply-chain-attack) ,[elementary-data 2026](https://www.stepsecurity.io/blog/elementary-data-compromised-on-pypi-and-ghcr-forged-release-pushed-via-github-actions-script-injection) ,[Ultralytics 2024](https://blog.yossarian.net/2024/12/06/zizmor-ultralytics-injection) )*
- **Fork code run with privileges.** An untrusted trigger checks out an outsider's fork and executes it while holding your repository's token. *([GitHub Security Lab pwn-request research](https://securitylab.github.com/resources/github-actions-preventing-pwn-requests/) ,[Trivy 2026](https://snyk.io/articles/trivy-github-actions-supply-chain-compromise/) )*
- **Poisoned shared cache.** A cache entry written from a fork is restored by your trusted runs, which then execute whatever it planted. *([TanStack 2026](https://tanstack.com/blog/npm-supply-chain-compromise-postmortem) ,[Ultralytics 2024](https://blog.yossarian.net/2024/12/06/zizmor-ultralytics-injection) )*
- **Impostor action SHA.** An action is pinned to a commit the canonical repository never contained, so the pin points at attacker code. *([tj-actions/changed-files 2025](https://www.wiz.io/blog/github-action-tj-actions-changed-files-supply-chain-attack-cve-2025-30066) ,[Chainguard imposter-commits research](https://www.chainguard.dev/unchained/what-the-fork-imposter-commits-in-github-actions-and-ci-cd) )*
- **Whole-context secrets dump.** The workflow serializes every secret it holds into logs or the environment, where anything running can read them. *([tj-actions payload behavior 2025](https://www.cisa.gov/news-events/alerts/2025/03/18/supply-chain-compromise-third-party-tj-actionschanged-files-cve-2025-30066-and-reviewdogaction) )*
- **GITHUB_ENV hijack.** An attacker-controlled write to GITHUB_ENV or GITHUB_PATH rewires every step that runs after it. *([GitHub Security Lab environment-injection research](https://securitylab.github.com/resources/github-actions-untrusted-input/) )*
- **Write token on an untrusted trigger.** A workflow grants write permissions on an event an outsider can raise, so their event arrives holding your write token. *([elementary-data 2026](https://www.stepsecurity.io/blog/elementary-data-compromised-on-pypi-and-ghcr-forged-release-pushed-via-github-actions-script-injection) )*
- **Credentials in caches or artifacts.** Key files land in a cache or an uploaded artifact, where other jobs, or the public, can fetch them. *([Trivy runner-credential harvesting 2026](https://www.stepsecurity.io/blog/trivy-compromised-a-second-time---malicious-v0-69-4-release) )*
- **Unverified curl piped to bash.** A script is fetched and executed with secrets in scope and no checksum, so the remote host's compromise becomes yours. *([Codecov bash-uploader breach 2021](https://about.codecov.io/security-update/) )*
- **Dependency install scripts in a privileged job.** A package you depend on is taken over and its install script runs the next time your workflow installs dependencies, inside a job holding secrets or a write token. The attacker never touches your repository; the script executes with the job's credentials before a single test has run. *([s1ngularity postinstall payloads 2025](https://www.wiz.io/blog/s1ngularity-supply-chain-attack) ,[the Miasma npm compromise 2026](https://www.microsoft.com/en-us/security/blog/2026/06/02/preinstall-persistence-inside-red-hat-npm-miasma-credential-stealing-campaign/) )*


Three of these were[narrowed by GitHub earlier this year](https://github.blog/security/supply-chain-security/disrupting-supply-chain-attacks-on-npm-and-github-actions/) . Fork-written caches are read-only on GitHub since June 2026, but the path stays open on GitHub Enterprise Server, on third-party cache backends, and when a trusted trigger runs untrusted code.` actions/checkout` now refuses fork checkouts on privileged triggers by default, but a SHA- or patch-pinned checkout never receives that change and a manual` git fetch` bypasses it entirely. Workflow-trigger policies shipped, but they are opt-in and evaluate-mode, so nothing changes on a default configuration.


Two related practices,[pinning actions to a commit SHA](https://starsling.dev/best-practices/github-actions/pin-action-shas) and[scoping id-token: write per job](https://starsling.dev/best-practices/github-actions/scope-id-token-per-job) , harden your configuration but are not attack paths an outsider can walk on their own, so[ci-score](https://starsling.dev/ci-score) grades them instead.


## Try it


Install the skills and run it in any repo:


```text
npx   skills   add   starslingdev/skills
```


Then invoke` /ci-secure` in your agent. Prefer not to install it? Paste this into your coding agent instead and it will fetch and run the skill for you:


```text
Run `npx skills use "https://github.com/starslingdev/skills" --skill "ci-secure"` and follow the generated skill instructions now.
```


It shows you every finding with its attacker scenario, fixes only the ones you pick, and touches only your working tree. It is open source under the MIT license. Read[the skill](https://github.com/starslingdev/skills/tree/main/skills/ci-secure) in the repo, or see how it works on the[ci-secure page](https://starsling.dev/ci-secure) .


## One install, three skills


The install command above adds two more GitHub Actions skills alongside ci-secure.[ci-speedup](https://starsling.dev/ci-speedup) finds what is actually slowing your CI down, from your own runs, and hands your agent the fix.[ci-score](https://starsling.dev/ci-score) grades your workflow configuration against eleven published pass-or-fail checks. All three live on the[skills page](https://starsling.dev/skills) .


## Let StarSling run this for you automatically


The skill is you checking the workflows once, with your own agent. Workflows drift, though: a new workflow lands with an unquoted interpolation, a fork trigger quietly gains a write permission, a pin moves. On paid plans, once optimization PRs are enabled,[StarSling Runners](https://starsling.dev/) keep watching your workflows as they change, and open every fix as a PR your team reviews and merges.


If your team runs CI on GitHub Actions,[install the StarSling GitHub App](https://github.com/apps/starslingdev) to keep it in shape automatically.


Happy Slinging 💫


Yonas
