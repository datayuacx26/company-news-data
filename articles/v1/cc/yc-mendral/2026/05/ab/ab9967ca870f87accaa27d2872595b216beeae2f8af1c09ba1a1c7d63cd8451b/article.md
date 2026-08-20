---
schema_version: "1.0.0"
document_id: "ab9967ca870f87accaa27d2872595b216beeae2f8af1c09ba1a1c7d63cd8451b"
company_key: "yc-mendral"
company: "Mendral"
source_id: "yc-mendral-news-import-538be81b6878"
canonical_url: "https://mendral.com/blog/supply-chain-attacks-dont-wait-for-cves"
published_at: "2026-05-22T00:00:00+00:00"
first_seen_at: "2026-08-10T00:32:33.388808+00:00"
fetched_at: "2026-08-10T00:32:34.981599+00:00"
content_hash: "sha256:39cd413e06f036dc0a2571102c4a9426552161083186916a5b8d87d2b1f06282"
---

# Supply Chain Attacks Don't Wait for CVEs

Five minutes. That's the gap we watched in production last week: a dependency was published to npm five minutes before a customer's PR commit, hadn't been indexed by deps.dev yet, and carried no advisory anywhere. A vanilla scanner would have let it through. Our agent flagged it because the publish timestamp was inside the active attack window.


That's the supply chain attack pattern now. By the time a CVE exists, the bad version has already pulled into thousands of CI pipelines. The publish-to-detection window is measured in hours, sometimes minutes. CI is the chokepoint, and the PR is where the decision has to happen.


## The last twelve months on npm and Actions


A short timeline of what's actually shipped at developers in the last year. Every one of these is documented, with public writeups from researchers and emergency advisories from CISA or maintainers.


-


**tj-actions/changed-files (March 2025).** Attackers retroactively modified version tags on a GitHub Action used in over 23,000 repos. The injected payload dumped runner memory into workflow logs, exposing secrets in any public repo that ran the action during the two-day window. Tracked as CVE-2025-30066. CISA put out an emergency alert. Wiz traced the initial vector back to a separate compromise of` reviewdog/action-setup` (CVE-2025-30154), which had leaked a token used to push to the tj-actions repo.


-


**Nx s1ngularity (August 2025).** A malicious Nx version executed a post-install script that scanned the developer's filesystem, called locally installed AI CLIs (Claude, Gemini, q) for reconnaissance, and uploaded harvested secrets to a public repo created inside the victim's own GitHub account. GitGuardian counted 2,349 credentials from 1,079 developer machines. Wiz documented a second wave where the stolen GitHub tokens were used to flip private repos public.


-


**chalk / debug / ansi-styles and 15 others (September 2025).** A phishing email landed on the npm account of one maintainer (Josh Junon, "qix"). Within minutes, malicious versions of chalk, debug, ansi-styles, strip-ansi, supports-color, and 13 other packages went live. Combined weekly downloads: 2.6 billion. The payload was a cross-platform remote-access tool. Google later attributed the infrastructure to UNC1069, a North Korean threat actor.


-


**Shai-Hulud worm (September-November 2025).** The first self-replicating worm in npm. It harvested credentials on install, then used the stolen npm tokens to publish itself into more packages owned by the same maintainer. CISA tracked over 500 directly compromised packages and tens of thousands of malicious republished versions. Shai-Hulud 2.0 in late November added a step that hit AWS, GCP, and Azure metadata services to grab workload credentials, and pre-install execution to widen the blast radius.


-


**Trivy (March 2026).** Attackers poisoned 75 of 76 GitHub Action tags for Aqua's Trivy scanner. Any pipeline pinned to a compromised tag silently exfiltrated secrets. We had moved one of our customers, Anyshift, from Trivy v0.30.0 to v0.35.0 eleven days before. v0.35.0 was the only tag that wasn't poisoned. From their CTO, Stephane Jourdan: "Mendral saved our a** during the Trivy supply chain attack. The agent had pinned the secure version two weeks before, so we avoided rotating every key across our infra."


The pattern repeats. Phishing or token theft, malicious version published, malware activates on install, secrets out within minutes. The publish-to-takedown window is measured in hours (the chalk/debug versions were live for less than two), but that's enough time for thousands of CI pipelines to install the bad version. The blast radius is whatever was sitting in your CI environment: AWS keys, GitHub PATs, npm tokens, cloud workload credentials, signing keys.


## Why scanners are reactive by design


Most supply chain tooling cross-references your lockfile against an advisory database. Dependabot, Snyk, the GitHub Dependency Review action, Grype, the older Trivy scans. They watch the GitHub Advisory Database, OSV, and the NVD. If a CVE exists for a version you pin, you get pinged.


That model works for vulnerabilities discovered through research. A logic bug in OpenSSL takes months to weaponize, and the advisory tends to land before active exploitation. It does not work for malicious-publish attacks. The advisory only exists after security researchers have spotted the bad version, written it up, and pushed it through coordinated disclosure. By that point, the attack has already happened.


There's also the lockfile drift problem. Your` package.json` declares loose ranges. Your` package-lock.json` pins exact versions. If those two disagree (and they often do), your scanner reads one source while CI installs from another. We see this constantly: production deps declared in the manifest that aren't in the lockfile, so the scanner never examines them. Audit gap. We saw it again in the customer PR last week.


## CI is the chokepoint


Every supply chain change flows through one place: a PR that touches a dependency manifest, lockfile, Dockerfile, or GitHub Actions workflow. That PR has full context. You know which package is being added, which version, which file. You can read the source code of the new version before merge. You can look at the publish date, the maintainer history, the changelog, the transitive deps.


That's where the decision has to happen. Not on a dashboard you check on Tuesdays, and not in a backlog of advisory alerts that runs days behind reality. At the PR, blocking the merge, with enough context that the engineer reviewing the change can decide in 30 seconds.


That's where Mendral lives.


## What Mendral does


Mendral is an AI DevOps engineer: three always-on agents (security, reliability, performance) connected through a GitHub App to your repos and CI, plus any custom automation you define.


The security agent has been running on customer pipelines since launch. It already handles two pieces of the supply chain problem.


### Vulnerability response (the CVE analyzer)


When a CVE drops on a package you actually use, the agent reads the advisory, traces how the package is used in your codebase, and determines whether the vulnerable code path is reachable from your code. If it is, the agent opens a fix PR with either the upgrade or the code patch that closes the path. If the vulnerable function isn't reachable from your usage, the agent says so and skips the noise.


That's the same approach that kept Anyshift on a safe Trivy version during the March 2026 attack. The agent had been continuously upgrading their pinned scanner version because Trivy releases tend to ship security improvements. When the attack hit, the version they were on (v0.35.0) was the only one that wasn't compromised. No incident response. No credential rotation. A routine maintenance PR became a supply chain firewall.


### Supply chain agent (shipped last week)


The new layer handles dependency *changes* at the PR. Two parts: a gatekeeper that reviews any PR touching deps, and an upgrade engine that opens weekly upgrade PRs.


## The gatekeeper


When a PR opens that touches` package.json` ,` package-lock.json` ,` requirements.txt` ,` go.mod` , a Dockerfile, or anything under` .github/workflows` , the agent runs a security review before merge. It checks:


- Typo-squatting (packages with names that mimic popular ones).
- Malware indicators in lifecycle scripts:` postinstall` ,` preinstall` ,` prepare` .
- Known CVEs at the pinned version, with the fix version if one exists.
- Suspicious maintainer changes or recent ownership transfers.
- Lockfile and manifest desync (production deps declared but not pinned, or vice versa).
- Problematic GitHub Actions config: unpinned actions, actions using` pull_request_target` with checkout of PR head, missing permissions blocks.


Packages published in the last 7 days get extra scrutiny. Anything under 72 hours triggers a deeper read on source code and transitive deps, because that's the active attack window.


The review gets posted as a single PR comment, findings ranked by severity. One comment with the actual risk surfaced, not a wall of CVE noise that scrolls past the diff.


The screenshot above is the real review from the customer PR I mentioned at the top.` @voyantjs/[\[email protected\]](https://mendral.com/cdn-cgi/l/email-protection)` was published to npm five minutes before the PR commit and hadn't been indexed by deps.dev yet. The lockfile was missing four production deps that were declared in` package.json` . And the version of postcss pinned by Next had a known moderate CVE with a fix available. Three findings, one comment, surfaced to the reviewer before merge.


## The upgrade engine


The second piece runs on a schedule. Every Monday at 9am UTC, the agent scans each enabled repo for outdated deps, reads the changelogs, greps real usage in your codebase, and opens upgrade PRs grouped by risk.


The reviewer sees pre-triaged PRs in three buckets. **Safe** : patch bumps with no code changes (one PR per ecosystem). **Mechanical** : renames, import paths, signature updates, with the code changes included (one PR per package). **Risky** : major bumps or behavioral changes, with a reviewer-attention section spelling out exactly what the agent couldn't fully verify.


What separates this from Dependabot or Renovate is the code-change step. The agent doesn't just bump a version number. It reads the changelog and the actual diff, then makes whatever code changes the upgrade requires. Rename an import, update a function signature, replace a deprecated call. CI validates. If a bump can't be done safely, the agent says so in the PR description instead of opening a broken upgrade.


A few defaults worth calling out. A 7-day cooldown skips versions too fresh to trust. Inside the eligible window the agent picks the most recent version, it doesn't anchor to an older one. Packages with open advisories at the current version get passed over until a fix is published. Existing upgrade PRs aren't duplicated.


The example PR above bumps Next 16.2.4 to 16.2.5 with twelve CVE fixes (six high, four moderate, two low), including DoS via Server Components stream exhaustion, middleware/proxy bypass via segment-prefetch, SSRF via WebSocket upgrades, XSS via CSP nonce parsing, and cache poisoning in RSC responses. The agent read Vercel's changelog, confirmed no breaking changes for the customer's usage, and made it a drop-in. The other seven packages in the PR are patch and minor bumps verified against actual code paths.


## The settings


All three pieces are independent toggles per organization: vulnerability response (the CVE analyzer), weekly dependency updates (the upgrade engine, with the configurable cooldown), and review dependency changes (the gatekeeper).


The defaults are conservative. Disabled on new installs, no merge-blocking out of the box, and passing reviews stay silent so we only post when we find something. The "Fail check on" setting can mark the GitHub Check failed at "Critical only" or "High and critical" if you want it to block merge, but it ships set to Never because the agent starts informational. We'd rather you turn the dials up than have us shout at you before you trust the signal.


## What's still hard


A few things we haven't solved, in order of how much they bother us.


**Runtime-only payloads.** The agent reads the dependency's source, so code that fetches and runs a remote payload usually gets flagged right there. What it can't verify is what that endpoint actually serves. A fetch that looks benign at review time can serve malware later, or only to specific targets. Catching that means watching behavior in your production sandbox, which is a different problem. We're working on it, but it's not fully solved in this release.


**False positives on legitimate new releases.** A package published five minutes ago looks identical to malware until you read the source. We err on the side of flagging and explaining, not blocking. The reviewer makes the call. We'd rather you see one or two false positives a quarter than miss a real attack, but if we get noisy we'll hear about it.


**Transitive dep coverage.** Direct deps get the full source-code read. Transitive deps get a lighter check focused on lifecycle scripts, recent publish dates, and known IOCs. A determined attacker who buries malware four levels deep in the dependency tree could slip past. We don't claim to be the only line of defense.


**Depth varies across ecosystems.** The agent already works across the manifest-and-lockfile pattern wherever it shows up: npm, PyPI, Go modules, RubyGems, Maven, container base images, GitHub Actions. The depth isn't uniform yet. npm and Actions get the deepest scrutiny because that's where the active attacks have been, and we're getting better at the rest.


Every dependency you pin is a decision, and the only safe place to make it is the PR that changes it. By the time an advisory shows up, if one ever does, the bad version has already run in CI and the secrets are already gone. The signal that catches these attacks isn't in a database, it's in the diff: when the version was published, what the source actually does, whether the lockfile matches the manifest. The review has to happen there, before the merge, while the change still has all its context attached. That's the whole bet.
