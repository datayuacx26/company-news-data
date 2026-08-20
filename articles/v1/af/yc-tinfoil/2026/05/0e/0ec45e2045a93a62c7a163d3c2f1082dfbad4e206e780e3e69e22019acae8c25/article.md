---
schema_version: "1.0.0"
document_id: "0ec45e2045a93a62c7a163d3c2f1082dfbad4e206e780e3e69e22019acae8c25"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2026-05-01-supply-chain-client"
published_at: "2026-05-01T12:00:00+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:d76fd5cfb9841fdcacd9e76d7ebab5110f6f321b6011ce234380f11c58522f26"
---

# Supply Chain Security - Part 1

[← Back to Posts](https://tinfoil.sh/blog)


# Supply Chain Security - Part 1


May 1, 2026


•


16 min read


Miles Dai


*This is a two-part series covering our approach to supply chain security at Tinfoil. In this post, we'll explore how we harden client-side code and lock down our build infrastructure against these attacks. In the next part, we'll examine the chain of trust behind our enclave images.*


Recently, we have been seeing a wave of high-profile software supply chain compromises, largely targeting build and CI/CD pipelines. In just the last two months, successful attacks against[axios](https://www.cisa.gov/news-events/alerts/2026/04/20/supply-chain-compromise-impacts-axios-node-package-manager) ,[Trivy](https://www.stepsecurity.io/blog/trivy-compromised-a-second-time---malicious-v0-69-4-release) ,[LiteLLM](https://docs.litellm.ai/blog/security-update-march-2026) ,[checkmarx KICS](https://checkmarx.com/blog/checkmarx-security-update/) , and the[Bitwarden CLI](https://community.bitwarden.com/t/bitwarden-statement-on-checkmarx-supply-chain-incident/96127) have had a large impact on many downstream users. While this is the latest wave of incidents, supply chain attacks have a[long](https://attack.mitre.org/campaigns/C0024/)[history](https://arstechnica.com/information-technology/2017/06/a-new-ransomware-outbreak-similar-to-wcry-is-shutting-down-computers-worldwide/) and remain a[security challenge](https://words.filippo.io/compromise-survey/) . We expect these compromises to become more frequent as AI compresses the software development lifecycle and also helps to automate these attacks.


In this post, we will share Tinfoil’s approach to thinking about supply chain security. Tinfoil’s primary goal is to protect user data from being accessed by anyone besides the user. Our[architecture](https://docs.tinfoil.sh/verification/verification-in-tinfoil) relies on the Tinfoil client encrypting the data and sending it into the enclave to be decrypted. In this post, we discuss supply chain hardening on the first half, the Tinfoil clients and SDKs. In Part 2, we will address the enclave itself.


*tl;dr: Tinfoil uses many countermeasures across our client-side software that have been demonstrated to reduce the supply chain attack surface. The most notable defenses include:*


- *Pinning all third-party GitHub Actions to commit hashes rather than version tags*
- *Avoiding long-lived package repository API keys from CI in favor of OIDC-based Trusted Publishing upload flows*
- *Running the[Zizmor](https://docs.zizmor.sh/) analyzer across our GitHub Actions workflows to prevent dangerous patterns in our CI*
- *Requiring 2FA on all member GitHub accounts in the Tinfoil organization and all accounts with access to package repositories*


## What is Supply Chain Security?


Broadly speaking, software supply chain security is the practice of ensuring that the code, tools, and infrastructure used to build and ship software have not been tampered with at any point before they reach the end user.


The Tinfoil supply chain for client-side code consists of three stages:


1. **Development Stage** : Code is contributed to a Tinfoil repository. This includes contributions by Tinfoil employees, third-party dependencies (e.g. libraries, tooling), and, occasionally, contributions from external contributors.
2. **Build Stage** : The code in the repository is built and packaged into artifacts such as compiled binaries that are ready for distribution.
3. **Publishing Stage** : The artifacts are pushed to package repositories (e.g. npm, PyPI, GHCR) for distribution to end users such as our SDK consumers and CLI users.


In the following sections, we will start by discussing how we secure the GitHub Organization before detailing how we harden each stage in our supply chain.


## GitHub Organization Security


Supply chain security is impossible if code repositories are not secure. Only Tinfoil employees should have the ability to merge new code, and all code coming from third parties must be reviewed before being merged.


To secure the Tinfoil GitHub organization, we require all members to have strong 2FA enabled (TOTP or stronger). While GitHub does not currently support requiring exclusively phishing-resistant authentication (i.e. Passkeys and WebAuthn), we enforce this as a company policy1 and commit to not using fallback OTP mechanisms to resist phishing attempts. When we must use GitHub Personal Access Tokens (PATs), we use[fine-grained PATs](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token) with scopes that have the minimum possible permissions.


Additionally, we use branch protection rules to prevent pushing directly to production branches and tag protection rules to prevent overwriting or deleting tags.


## Development Stage Security


During the software development stage, supply chain security requires being cautious about third-party dependencies. At Tinfoil, we aim to reduce the use of external packages and to rely more on trustworthy sources. It is difficult to provide clear-cut rules for this, but here are some helpful heuristics for evaluating a package:


- Does the dependency provide non-trivial functionality? Could it be replaced with a standard library alternative or a simple implementation?
- Does the package come from a reputable source?
- What is the popularity of the package? It is important to consider all aspects of the repository such as open issues, PRs, and forks, and not just stars since there is a rising trend of repositories[faking popularity](https://arxiv.org/pdf/2412.13459) by artificially inflating their star counts.
- Does it have an active community? Does the package appear to be actively maintained?


Any third-party dependencies that are deemed necessary also must be pinned to a specific version, ideally through a hash. The dependency resolution algorithm used must be deterministic, and the results of that resolution should be checked into the repository. This typically takes the form of a **package lock file** which records the exact versions of all dependencies being used. This ensures that packages are always built with the same set of dependencies and that changes to the dependencies are included in version control. The Go and JavaScript ecosystems have built-in solutions using` go.mod` /` go.sum` and` package-lock.json` files respectively. For Python, we recommend using a package manager with good lockfile support. At Tinfoil, we use` uv` , a modern Python package manager to produce` uv.lock` files.2


After building a clear picture of the dependency tree, we can begin performing vulnerability scans. **Vulnerability scanning tools** collect data from public vulnerability databases and check the dependency tree for affected packages. A good scanner will also perform static analysis on your codebase to determine if your code is actually using the specific vulnerable symbols to reduce the rate of false positives and the risk of alert fatigue.


Go provides an officially supported solution through[govulncheck](https://pkg.go.dev/golang.org/x/vuln/cmd/govulncheck) which we run in a nightly CI job. In the Python ecosystem, the state-of-the-art is[pip-audit](https://github.com/pypa/pip-audit) , currently being maintained by PyPA. uv also provides a built-in[uv audit](https://docs.astral.sh/uv/reference/cli/#uv-audit) command that performs a similar function. In the JavaScript ecosystem, there is the built-in[npm audit](https://docs.npmjs.com/cli/v10/commands/npm-audit?v=true) command.


To further safeguard against malicious dependencies, we have also enabled **dependency cooldowns** in the package managers that support it. Dependency cooldowns tell the package manager to wait for a certain "cooldown period" (usually a few days) before incorporating a new release into the project. This gives upstream maintainers some time to notice and yank malicious releases before they land in any Tinfoil repositories.


## Build Stage Security


At Tinfoil, we use GitHub Actions as our CI/CD provider due to its tight integration with GitHub and also some interesting built-in security tools (discussed later). However, the same flexibility that makes it so useful also presents many security footguns.


One of the most dangerous but commonly-used features is the` uses` keyword that allows workflows to pull in reusable actions. Two properties combine to make reusable actions risky:


- They can come from third parties
- They are usually referenced using git tags which are mutable


Third party reusable actions are inherently risky because they inject code into your build process; this code could modify source files just before compilation or exfiltrate secrets from the environment. Like any other third-party dependency, the action must be reviewed and pinned to a known-good version. However, most documentation examples (and LLM-generated code) use git tags to specify the action version (e.g.` org/action-name@v2` or` org/action-name@v2.1.0` ). But this is still not safe because GitHub repository owners (or other malicious actors) can actually edit the tag to point to a different commit, causing you to pull in different code on the next workflow run. To fully constrain the code you are running in your pipeline, the action must be pinned with its commit hash at the desired version.3 For example:


Failing to use this pattern could allow malicious code to be injected into your build flow if the action developer’s account is compromised. This is a very common attack vector used in the supply chain incidents we are seeing today. Recent attacks on third-party actions such as` tj-actions/changed-files` in March 2025 and` aquasecurity/trivy-action` in March 2026 both began with attackers gaining access to the GitHub repository to commit malicious code. The attackers then overwrote the version tags in the repository to point to that commit. Immediately, all users of these actions who had not pinned with a hash would have automatically pulled and executed the new malicious version of the action when their workflow ran.


Of course, pinning actions to stable versions is not useful if the action is already vulnerable or malicious. In general, it is better to remove third-party actions if they are not providing significant functionality. For example, popular third-party actions to create GitHub PRs and releases can usually be replaced with simple GitHub CLI commands. For third-party actions that provide non-trivial functionality, it is crucial to both evaluate the source code and the project to determine if you trust their code to run in your release flow. While auditing our own CI pipelines, we found that we were able to replace several instances of third-party actions with built-in tools.


CI/CD pipelines also tend to contain many secrets used with external systems like package repositories. These tokens are high-value targets for attackers since they tend to allow for easy privilege escalation. Tokens must therefore be scoped as tightly as possible within CI/CD pipelines to limit the blast radius of any compromise. GitHub Actions allows for tokens to be restricted to specific environments and jobs. Tinfoil uses both of these approaches to ensure that only the jobs that require secrets have access to them.


This section covered many of the best practices that Tinfoil uses to secure its GitHub Actions setup, but it is not exhaustive. We also rely on[Zizmor](https://docs.zizmor.sh/) , a static analyzer, to check our workflows for dangerous patterns against a large list of[audit rules](https://docs.zizmor.sh/audits/) .


## Publishing Stage Security


Tinfoil publishes SDKs in various languages to their respective package repositories. This was traditionally done using an API key provided by the package repository and storing it as a GitHub Actions Secret. However, if an attacker were to gain access to this secret, they would have the ability to publish new, malicious packages to the Tinfoil repository even from outside GitHub Actions. This was the root cause of the recent LiteLLM attack, where attackers were able to steal the static PyPI API key, likely via the` trivy-action` attack discussed earlier. The API key allowed the attackers to directly push malicious packages to LiteLLM’s official PyPI account. Because the cost of such a compromise is so high, package repositories have begun providing alternatives to static API keys.


Modern package publishing uses a **Trusted Publishing** approach based on OIDC for authentication. The basic operation consists of first registering your GitHub identity as a trusted publisher with the package repository during the initial setup. Then, during the publishing step, GitHub essentially vouches for your identity, and the package repository grants the action a short-lived token. While the details of OIDC can be gnarly, there are now officially-supported tools for npm and PyPI that make it extremely easy to set up Trusted Publishing flows.4


- [Instructions for npm](https://docs.npmjs.com/trusted-publishers)
- [Instructions for PyPI](https://docs.pypi.org/trusted-publishers/)


Wherever possible, Tinfoil uses trusted publishing workflows to ensure that no long-lived API keys reside in the GitHub Actions repositories.


## Enabling Downstream Supply Chain Security


As the “chain” in supply chain suggests, our concerns do not stop at just the security of our own packages. There are also steps we can take to ensure downstream consumers of our SDK are empowered to secure their own supply chains.


First, as part of our package release flow, we generate information about the code, platform, and environment used to build each release artifact (along with the hashes of the artifacts). This information is known as **provenance** and is cryptographically signed by GitHub Actions when it is generated.5


This provenance information is then uploaded to a public transparency log. A consumer of our SDK can fetch the provenance information, check that it is signed by the runner, verify that the correct commit was used during the build process, and validate that the hash of the generated artifact matches their local version.6 While provenance information does not inherently prevent any attacks, it can provide visible signals to package users if a suspicious version of a package was uploaded. For example, during the LiteLLM compromise, users would have been able to see that the new package was not uploaded from a GitHub Actions run but rather from an unknown source.


The inclusion of provenance data with releases is generally still quite low. At the time of writing,[approximately 12% of the top 50 most downloaded npm packages ship this data](https://dev.to/thecryptodonkey/i-audited-the-top-50-npm-packages-almost-none-ship-with-supply-chain-attestations-3ki8) . At Tinfoil, we would be very excited to see more adoption of this technology across the ecosystem. Tinfoil currently includes provenance information for the JavaScript and Python SDKs through each project’s respective trusted publishing workflows.7


An example of this provenance information can be found on the[tinfoil npm module](https://www.npmjs.com/package/tinfoil) page. In npm, you can check the integrity of the provenance information using` npm audit signatures` . You can also see the raw provenance information yourself:


Second, Tinfoil has begun incorporating **Software Bill-of-Materials (SBOMs)** into some of our releases to assist our downstream users with vulnerability management.8 An SBOM essentially lists a package’s set of transitive dependencies to let consumers know all the dependencies they are pulling in when they use a Tinfoil SDK. This can be helpful for auditing purposes and for vulnerability management. If a vulnerability is reported in a package deep in the dependency tree, an SBOM can help you understand the impact quickly.


## Tooling Recommendations


Tinfoil primarily uses Go, Python, and JavaScript. Below, we list some tools we use for each language that are easy to use and provide quick wins.


Go JS Python


Vulnerability scanning[govulncheck](https://pkg.go.dev/golang.org/x/vuln/cmd/govulncheck)[npm audit](https://docs.npmjs.com/cli/v10/commands/npm-audit?v=true)[uv audit](https://docs.astral.sh/uv/reference/cli/#uv-audit) or[pip-audit](https://github.com/pypa/pip-audit)


Dependency cooldown npm's[min-release-age](https://docs.npmjs.com/cli/v11/using-npm/config#min-release-age) uv's[exclude-newer](https://docs.astral.sh/uv/reference/settings/#exclude-newer)


Version pinning Built-in support via go.mod and go.sum Built-in support with npm via package-lock.json uv — allows creation of a uv.lock file. Additionally, PEP 751 introduced the[pylock.toml](https://packaging.python.org/en/latest/specifications/pylock-toml/) file that also serves this purpose.


Trusted Publishing N/A Built in to[npm](https://docs.npmjs.com/trusted-publishers)[pypi-publish](https://github.com/marketplace/actions/pypi-publish) GitHub Action


SBOM Generation[cyclonedx-bom/cyclonedx-py](https://pypi.org/project/cyclonedx-bom/)


For Github Actions scanning, we recommend using[Zizmor](https://docs.zizmor.sh/) .


## Conclusion


Despite what it may seem like, supply chain security is not an intractable problem. There are simple steps you can take today to significantly raise the bar against an attack. Here, we outline the most impactful actions from our experience:


1. Enable 2FA on your GitHub account if it is not enabled already. Bonus points for using phishing-resistant methods such as Passkeys.
2. Pin the hashes of any third-party actions used in your GitHub Actions workflows. This simple step would have prevented many of the supply chain attacks mentioned in the introduction.
3. Run the Zizmor scanner against your GitHub Actions workflows. It will almost certainly flag some warnings for you to fix.
4. Adopt trusted publishing workflows if possible. The tooling that is available for this is now incredibly simple to use, and the benefits of removing long-lived API keys is significant.
5. Consider enabling dependency cooldown settings in your package managers.


Lastly, we should note that supply chain security evolves with the ecosystem. This blog post establishes what we understand to be best practices in today’s environment, but the work is not done. Tinfoil aims to remain at the forefront of this space, and we will continue to update our systems as the community learns more. Compared to your average software company, supply chain security plays an even greater role in the context of Trusted Execution Environments. In Part 2 of this series, we will take a deeper dive into how we securely build the images we deploy into our enclaves.


## Footnotes


## Footnotes


1.


Company policy is absolutely another tool used to secure the supply chain! Supply chain issues frequently touch on human elements of the development process, and any proper analysis must take a holistic approach that includes both the software and the developers. In fact, NIST dedicates an entire category of practices in its Secure Software Development Framework ([SP 800-218](https://csrc.nist.gov/pubs/sp/800/218/final) ) to just “Preparing the Organization,” which is to say, ensuring that the *people and processes* are prepared to perform secure software development.↩


2.


Compared to other languages, Python has had a long history with fragmented lockfile solutions. Each packing tool developed its own solution until PEP 751 finally defined pylock.toml a standardized lockfile format in March 2025. Today, migration for many tools remains a work in progress. Tinfoil chooses to use` uv.lock` because we use uv tooling for its speed and other features. Since uv is able to generate` pylock.toml` from` uv.lock` , we can remain compatible with the standard.↩


3.


The rabbit hole actually goes deeper. It is important to check that the hashes being pinned refer to commits that are located on the intended repository and not a fork. On GitHub, it is possible to refer to a commit in a forked repository with different maintainers and have the action still resolve successfully, a situation known as an[imposter commit](https://www.chainguard.dev/unchained/what-the-fork-imposter-commits-in-github-actions-and-ci-cd) . Zizmor (discussed below) in online mode will also check for this[issue](https://docs.zizmor.sh/audits/#impostor-commit) .↩


4.


Tinfoil also publishes several Go packages. The Go ecosystem uses a different approach to package distribution where the code repository itself serves as the code storage backend rather than using a central registry. This approach avoids the issue of API keys since the code is never “published” to a registry. The security of the “publishing” step just falls back to the security of the GitHub repository. This is discussed more in[The Go Blog](https://go.dev/blog/supply-chain) .↩


5.


Technically, Sigstore’s Fulcio CA is the one that ultimately endorses the signing key used to sign the provenance certificate. The process uses Sigstore’s[keyless signing](https://docs.sigstore.dev/cosign/signing/overview/) flow which has a few more moving parts, but functionally, it is the GitHub Actions runner endorsing the fact that the artifact was generated on a legitimate runner and with a specific version of the code.↩


6.


Long-time followers of our blog may recognize some recurring themes surrounding Sigstore and artifact attestation from some of our[previous](https://tinfoil.sh/blog/2025-01-10-tinfoil-enclaves-overview)[posts](https://tinfoil.sh/blog/2025-01-13-how-tinfoil-builds-trust) . This is exactly the same technology! Tinfoil has been adapting the same tools used for software supply chain security to similarly attest to the machine images we generate for our enclaves.↩


7.


Once again, Go’s distribution model differs from JS and Python. It does not have the same need for provenance information since the distribution of a Go package does not involve uploading an artifact to a package repository.↩


8.


Efforts to standardize SBOM formats and tooling are still in their early days. The ecosystem remains fragmented (e.g. SPDX vs CycloneDX), and there are many subtleties involved in producing genuinely useful SBOMs. We are starting out by incorporating a CycloneDX document in the Python SDK wheel as specified by PEP 770. If you are a Tinfoil user and an SBOM user, we would love to hear from you!↩


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2026-03-10-tinfoil-containers)[Next Post](https://tinfoil.sh/blog/2026-06-09-auditing-frontier-model-without-weights)
