---
schema_version: "1.0.0"
document_id: "04eed6b7ebdb2b9f38018b3b082b648471528efec46d31704685a230b3ed691b"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/dependency-scanning-auto-remediation/"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:07.155656+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:e3a1d4918cf0c5e0b7755ef42768a53955abe1ebb8e3a5ec6def87f9d7b3fd6a"
---

# When a version bump breaks your build, GitLab fixes it

AI is writing more code and pulling in more dependencies, increasing application risk. Most of that exposure isn't from code your team actively chose. A 2025[study of the Maven ecosystem](https://arxiv.org/abs/2503.22134) found vulnerabilities reaching roughly 63% of latest releases through transitive dependencies, versus 31% through direct ones.


[Dependency Scanning Auto-Remediation](https://docs.gitlab.com/user/application_security/remediate/dependency_scanning_auto_remediation/) , now in beta, closes the loop for security. When[dependency scanning](https://about.gitlab.com/blog/sbom-based-dependency-scanning/) finds a vulnerable package, GitLab opens a merge request to update it, uses AI to fix any build-breaking changes, and iterates until your pipeline passes — with every change governed by your existing gates and audit trail.


As a result, security backlogs shrink without diverting developers, high-severity vulnerabilities get fixed within compliance deadlines, and breaking upgrades arrive as merge requests ready for approval.


## Why the dependency backlog keeps growing


Vulnerable and outdated components are a longstanding[OWASP Top 10](https://about.gitlab.com/blog/2025-owasp-top-10-whats-changed-and-why-it-matters/) risk and a leading source of remediation backlogs. Clearing findings is slow, manual work that competes with feature delivery, leaving high-severity vulnerabilities unresolved beyond the 30-day deadlines of PCI-DSS and[FedRAMP](https://about.gitlab.com/blog/gitlab-dedicated-for-government-now-fedramp-authorized/) . Meanwhile, even in established libraries, AI-assisted exploit engineering is accelerating disclosure and weaponization.


Roughly one in eight[dependency updates introduce a breaking change](https://link.springer.com/article/10.1007/s10664-024-10563-4) , and many labeled backward-compatible still break the build. Teams tend to defer complex changes, and the longer those vulnerabilities sit, the more serious they become.


## From backlog to fix, without diverting developers


Dependency Scanning Auto-Remediation turns vulnerable dependencies into reviewed, ready-to-merge fixes, so your team clears findings faster and spends less time resolving breaking changes. Teams see benefits in speed, effort, and control:


- **Shrink the dependency backlog.** Vulnerable dependencies get upgraded as they're found, so findings don't pile up and high-severity issues stay within compliance deadlines.
- **Reclaim time lost to breaking-change rewrites.** When a bump breaks the build,[GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) commits a fix, so developers review a working change instead of authoring one from scratch.
- **Keep every change governed.** Auto-remediation drafts the change, but nothing merges until a reviewer signs off, and every MR leaves an audit trail of what changed and who approved it.


## Quickly close vulnerabilities, even when they require code changes


Dependency Scanning Auto-Remediation bumps vulnerabilities and fixes breaking changes in two stages:


**Automated dependency version bumping** runs automatically when scanning detects a vulnerable dependency, opening a merge request to upgrade it to the nearest fixed version. When no eligible fix exists, the finding stays in your vulnerability report until a safe upgrade path becomes available. Every MR is attributed to a dedicated service account, making each change traceable to a distinct identity.


**Agentic breaking change resolution** handles the tough cases when a version bump introduces breaking changes. When a remediation MR's pipeline fails because the new version breaks your project, GitLab Duo Agent Platform automatically analyzes the pipeline errors, the dependency's changelog, and how your code uses the dependency. Then, within the same MR, it commits fixes to your code so your project works with the updated version. If it can't get the pipeline passing, it stops and posts what it found to the MR so you can take it from there. Supported ecosystems include Bundler, Maven, Gradle, and major Python and JavaScript/TypeScript package managers, with[Rust](https://gitlab.com/gitlab-org/gitlab/-/work_items/604602) and[Go](https://gitlab.com/gitlab-org/gitlab/-/work_items/604601) planned in the months ahead.


Auto-remediation never merges on its own. To speed up review, each MR spells out the vulnerability it addresses, the version it moves to, and the code GitLab Duo Agent Platform suggested to keep the build passing, so approvers don't have to reverse-engineer the change.


## How Dependency Scanning Auto-Remediation works


Auto-remediation runs automatically when SBOM-based dependency scanning detects a vulnerable dependency with an available fix. Practitioners can also initiate it for an individual finding from the vulnerability report. GitLab then opens a remediation MR that flows through your normal review and merge process; when agentic breaking-change resolution is enabled and the version bump breaks the pipeline, GitLab Duo Agent Platform attempts to fix the resulting code changes in that same merge request.


Built-in safeguards keep remediation automation from becoming noise. Cooldown periods stop busy projects from triggering remediation on every pipeline, and GitLab won’t re-create a closed MR unless a newer fix is available.


Configure remediation to match your risk tolerance. You can target vulnerabilities of any severity from low to critical, cap how far version bumps are allowed to go (patch, minor, or major), and store settings in project- or group-level configuration profiles (via API during beta).


Remediation runs through your organization's own pipeline, so it inherits your existing access controls and approval gates. You also get a complete, auditable record of what changed, who approved it, and why.


See Dependency Scanning Auto-Remediation in action:


## Start clearing your dependency backlog today


Dependency Scanning Auto-Remediation is in public beta. It is available on[GitLab.com](http://gitlab.com/) and rolling out to GitLab Self-Managed and GitLab Dedicated.


Ready to try it? Check out the[Dependency Scanning Auto-Remediation documentation](https://docs.gitlab.com/user/application_security/remediate/dependency_scanning_auto_remediation/) .


Automated dependency version bumping is included with GitLab Ultimate at no additional cost.


You can get access to agentic breaking-change resolution with a[free trial of GitLab Duo Agent Platform](https://gitlab.com/-/trials/new?glm_content=default-saas-trial&glm_source=about.gitlab.com/gitlab-duo-agent-platform/) . Already a GitLab Ultimate subscriber?[Turn on Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/turn_on_off/) and use the[GitLab Credits included with your subscription](https://docs.gitlab.com/subscriptions/gitlab_credits/#included-credits) .


Have feedback? Share it in the[feature feedback epic](https://gitlab.com/gitlab-org/gitlab/-/work_items/600511) .


##


Are you trading speed for security?


[Get your security maturity score](https://about.gitlab.com/assessments/security-modernization-assessment/)


Quiz will take 5 minutes or less
