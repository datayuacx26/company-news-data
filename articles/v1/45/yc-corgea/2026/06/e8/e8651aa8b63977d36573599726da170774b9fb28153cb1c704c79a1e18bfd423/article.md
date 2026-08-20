---
schema_version: "1.0.0"
document_id: "e8651aa8b63977d36573599726da170774b9fb28153cb1c704c79a1e18bfd423"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/introducing-corgea-skill-scanning"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-24T03:05:55.735939+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:947bdb3d420a1912dc8982d36f34a03655e2b3103aecb8e2de7b2d106dd74a45"
---

# Introducing Corgea Skill Scanning

Yesterday we launched[Security Design Reviews](https://corgea.com/blog/introducing-security-design-reviews) , catching threats before code exists. Today: what happens when developers extend AI coding agents with custom skills that nobody has reviewed yet.


AI agents are only as safe as the instructions they follow. A single unreviewed` SKILL.md


`


file can tell an agent to run unsafe commands, install risky packages, access sensitive files, leak secrets, weaken security controls, or perform actions outside the intended workflow.


Today we’re launching **Corgea Skill Scanning** and the **Skills Registry** : the safety gate and governed distribution layer for custom agent skills across your organization.


## How skill review works


When a user submits a skill to the Corgea Skills Registry, Corgea first validates that the file follows the expected Agent Skills format. The` SKILL.md


`


must include required frontmatter such as` name


`


and` description


`


, and the skill name is scoped to the customer’s company registry.


Each submitted skill version starts in **Pending Review** . Corgea then reviews the skill content for security risk. The review looks at the instructions, declared tools, commands, package usage, and any behavior the skill encourages the agent to perform. The goal is to detect malicious or unsafe behavior before the skill becomes available to the rest of the organization.


### What Corgea is designed to catch


Examples of risky behavior Corgea is designed to catch include:


- Instructions to exfiltrate source code, secrets, tokens, or environment variables.
- Commands that delete files, rewrite history, disable security tools, or alter system configuration.
- Requests to install or execute untrusted packages or scripts.
- Attempts to bypass approval flows, authentication, or access controls.
- Hidden instructions that tell the agent to ignore user, company, or platform safety rules.
- Network calls or uploads that are unrelated to the skill’s stated purpose.
- Overly broad tool access that is not justified by the skill’s description.


Every version is reviewed independently. Approving version` 1.0.0


`


does not automatically approve version` 1.0.1


`


, because even a small change to a skill can materially change what an AI agent is allowed or encouraged to do.


## Review states


Corgea tracks four review states:


- **Pending Review:** submitted and waiting for review.
- **Approved:** reviewed and installable by users in the company.
- **Rejected:** blocked because security concerns were found.
- **Failed:** review could not complete and needs follow-up.


Only approved skill versions are installable. Pending, rejected, and failed versions can remain visible in the registry with their status, but their installable content is not served through the installation flow. If a skill is rejected, Corgea records the security concern so the author knows what needs to be changed.


## Skills Registry


The Corgea Skills Registry is the governed system of record for custom agent skills. It gives organizations a central place to create, review, version, approve, reject, and distribute skills across their company.


Each skill is a named container, and each submitted` SKILL.md


`


becomes a version of that skill. Versions are stored separately and reviewed independently, giving teams a clear audit trail of what changed, when it changed, who submitted it, and whether it was approved by automated review or a human reviewer.


The registry is company-scoped. A customer’s skills, versions, review notes, security concerns, and installability status belong to that customer’s organization. This allows teams to build internal agent capabilities without exposing them outside their company.


### Role-based governance


The registry also supports role-based governance. Users with the right permissions can:


- View available skills.
- Create new skills.
- Submit new versions.
- Review, approve, or reject versions.
- Delete skills when needed.


Human override is supported for review workflows. Authorized security or admin users can approve or reject a version manually, add review notes, or submit a corrected version for another review. This keeps security teams in control while still allowing automated review to handle routine checks.


Once a version is approved, Corgea exposes the install command:


```text
corgea   skill   install   <  skill-nam  e  >
```


The registry tracks version history, latest approved version, reviewer source, review notes, security concerns, and install counts. Developers get a simple way to discover and install trusted skills, while security teams get the controls and auditability required to manage AI agent behavior safely.


## The big picture


Corgea Skill Scanning acts as the safety gate, and the Skills Registry acts as the governed distribution layer. Together, they let developers create and share useful custom agent skills while ensuring only reviewed, approved, and auditable agent behavior is trusted across the organization.


*Day 2 of Corgea Launch Week. Follow along on[LinkedIn](https://linkedin.com/company/corgea) and[X](https://x.com/corgea) .*
