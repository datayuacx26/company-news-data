---
schema_version: "1.0.0"
document_id: "e1867692196eae4d3966100344f4fbfb381a40c3b37a9af8199366184ba90eed"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-25a2b11929a5"
canonical_url: "https://zeropath.com/blog/commenda-case-study"
published_at: "2026-02-26T00:00:00+00:00"
first_seen_at: "2026-07-24T06:49:27.067531+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:60d17a8cf140c33e2f3506239b3e40a71205c507ac60a35fd7defce70600ef7e"
---

# Why Commenda Chose ZeroPath to Secure Their Global Tax Platform

## Commenda: Tax Compliance for Growing Multinationals


Commenda is a global tax compliance and entity management platform that helps businesses incorporate, manage, and stay compliant across 70+ countries. The platform handles everything from corporate governance and cap table management to indirect tax, transfer pricing, and financial reporting for growing international companies.


Commenda processes highly sensitive financial and identity data at a global scale, where the margin for security error is effectively zero.


---


## The Challenge: Scaling Security Without Dedicated Security Headcount


[Yaacov Tarko](https://www.linkedin.com/in/yaacov-tarko/) , Founder and CTO, built Commenda's engineering organization from the ground up. Today, the team includes engineers across a range of experience levels, with hiring continuing to accelerate. AI-assisted development tools like Cursor have driven 2–3× year-over-year productivity gains, meaning code output has scaled even faster than headcount.


They build tax-compliant software where business logic has to be correct every time. Every pull request still gets a rigorous, line-by-line review. Yet even the most thorough manual code reviews can miss vulnerabilities that require understanding what code is *supposed* to do, not just what it does.


- A rapidly expanding code surface area, accelerated by AI-assisted development
- Engineers across experience levels building secure development practices
- Increasing enterprise customer expectations for security evidence
- SOC 2 Type 2 and ISO 27001 certifications on the roadmap
- The tail risk of a security incident that could seriously damage the business


Without dedicated security headcount, Yaacov was responsible for security alongside leading engineering and product. He needed a way to run security the way a company with a full security team would, without actually building one from scratch.


---


## Why Commenda Chose ZeroPath


As Commenda prepared for SOC 2 Type 2 and ISO 27001 certification, Yaacov knew he needed a code scanning tool that doesn't fill them with false positives and can actually find real vulnerabilities. Auditors expect PR-level scanning and continuous vulnerability monitoring. But he had no interest in buying a noisy scanner just to check a compliance box. If the team was going to adopt a tool, it had to actually make the product more secure.


That's when Yaacov connected with ZeroPath. It took them merely 5 minutes to start their first scan with no rollout project or rule tuning required.


What kept Commenda using ZeroPath was the quality of vulnerabilities it found. Where traditional SAST tools like Semgrep, Snyk, Aikido, etc. flag syntactic patterns, ZeroPath started surfacing vulnerabilities that required understanding code intent: authorization bypasses, access control gaps, and business logic flaws that no rule-based scanner could detect.


ZeroPath caught a business logic authorization issue in a PR review that could have given users access they should never have had. As Yaacov put it, "that's not something any pre-LLM tool could catch. You have to actually understand what the code means to know that it's incorrect."


Of the issues important enough to fix at Commenda, more than half were business logic vulnerabilities, the category that traditional scanners miss entirely.


---


## The Solution: ZeroPath in Commenda's Engineering Workflow


Today, ZeroPath is integrated into Commenda's weekly engineering rhythm. The team uses two complementary modes: PR checks provide merge-time guardrails and compliance visibility, while full-repo scans drive deeper discovery and surface the highest-value findings.


### How Yaacov runs security from his desk


Yaacov runs full scans on cadence, then logs into ZeroPath weekly to review findings. ZeroPath identifies likely code owners based on commit history, and Yaacov routes findings to the right engineers through Linear with severity-based SLAs. When a finding isn't resolved within the SLA window, he follows up directly. The entire security workflow, from discovery through triage, assignment, and tracking, takes him a couple hours per week.


For many findings, ZeroPath generates targeted fix PRs. Engineers review the proposed fix, apply context-aware adjustments when needed, and ship. The heavy lifting of finding the vulnerability, understanding it, and drafting a fix is already done before a developer even opens the PR. Most fixes take 15 to 20 minutes to validate and merge.


### Elevating engineering security practices


Not every engineer on Commenda's team is a security specialist, and they don't need to be. ZeroPath's detailed vulnerability explanations turn findings into learning opportunities. Yaacov describes them as what a security professional would write if they were teaching someone the bug from scratch. His engineers actually enjoy using the tool because they learn as they go, which shifted ZeroPath from a pure scanner into ongoing developer education and codebase visibility.


### Delivering compliance coverage auditors trust


Enterprise buyers and auditors expect clear evidence of secure review workflows. ZeroPath provides PR scanning proof for SOC 2, continuous monitoring through scheduled full-repo scans, and auditable finding-to-fix trails, all from a single platform.


Instead of splitting across disconnected tools, Commenda gets compliance evidence and actionable findings in the same workflow. The PR checks satisfy auditor requirements while full scans do the real security work.


---


## The Results: Stronger Security, No Slowdown


By deploying ZeroPath, Commenda built a security program that scales with engineering output and AI-assisted development speed, without adding headcount or slowing delivery. As Yaacov puts it, ZeroPath does the hard part, and as AI-assisted coding accelerates the team, ZeroPath keeps pace.


4× more real vulnerabilities found


Before ZeroPath, bugs were caught only ad-hoc during code reviews. Systematic scanning now surfaces 4× more real security problems.


50%+ of critical findings are business logic bugs


The highest-value findings are the ones no legacy scanner could surface: authorization bypasses, access control gaps, and logic flaws that require understanding code intent.


Zero slowdown in development velocity


Automated fix generation and clear explanations keep remediation under 20 minutes per finding. Development hasn't slowed at all.


Complete security program without a security hire


Yaacov runs Commenda's entire security workflow (discovery, triage, assignment, and tracking) in a couple hours per week.


SOC 2 and ISO 27001 readiness streamlined


PR checks, scheduled scans, and auditable finding-to-fix trails give auditors everything they need from a single tool.


---


## About ZeroPath


ZeroPath is an AI-native application security platform that detects, explains, and helps fix real vulnerabilities, including business logic bugs, with precision and developer-friendly workflows. Leading engineering teams use ZeroPath to increase security coverage without slowing development or increasing headcount.


---


## Frequently asked questions


What is a business logic vulnerability, and why are they harder to find than traditional vulnerabilities?


A business logic vulnerability is a flaw in how an application enforces its own rules (authorization checks, payment flows, access controls) rather than a flaw in how it handles input. Unlike SQL injection or XSS, these bugs look like normal, syntactically correct code. There's no pattern to match against, which is why traditional SAST tools miss them entirely.


Detecting them requires understanding what the code is supposed to do, then reasoning about where that intent breaks down. At Commenda, ZeroPath discovered over 50% of critical business logic vulnerabilities that scanners like SonarQube, Semgrep, and Snyk Code had missed.


How does ZeroPath detect business logic vulnerabilities better than traditional SAST tools?


ZeroPath uses a multi-stage AI pipeline instead of static rule databases. It parses source code into an enriched graph capturing control flow and data flow across files, then runs source-to-sink taint analysis to trace untrusted data through authentication guards and business logic layers. An LLM-driven analysis layer then reasons about code intent, recognizing missing authorization checks, bypassable payment flows, or access control gaps, rather than matching signatures. Every finding is validated for reachability and exploitability, cutting false positives by 75%. Learn more in the full technical deep-dive:[How ZeroPath Works](https://zeropath.com/blog/how-zeropath-works) .


What integrations does ZeroPath support?


ZeroPath natively supports[GitHub, GitLab, Bitbucket, Azure DevOps, and more](https://zeropath.com/products/integrations) for PR scanning and full-repo scans. Findings sync directly to Jira and Linear with severity-based prioritization, and real-time alerts go to Slack and email. Teams can also import findings from existing scanners (Snyk, Semgrep, Checkmarx, SonarQube, Veracode, Fortify, and Synopsys) which ZeroPath re-validates with AI-powered CVSS 4.0 scoring. Enterprise SSO, audit logs, and team-based permissions are included out of the box.


How does application security testing help fintech companies stay compliant with SOC 2 and ISO 27001?


Both SOC 2 and ISO 27001 require demonstrable secure development practices: code review processes, continuous vulnerability monitoring, and documented remediation evidence. Application security tools that integrate into CI/CD pipelines satisfy these by providing PR-level scanning proof, scheduled full-repo scans for continuous monitoring, and auditable finding-to-fix trails.


At Commenda, ZeroPath delivers all three from a single platform. Engineering leadership runs the entire security workflow in a couple hours per week, with audit-ready documentation generated automatically, no separate compliance tooling required.
