---
schema_version: "1.0.0"
document_id: "7f6cce30ad796d9b434411b211e2ec531214824f75cf6325965ff51d78c548f8"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/datadog-code-threats/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:6e355521de474fbe9d244d31b29b7bf8bbdba5940ba283cc257b2fb43698b735"
---

# Detect source code attacks with Datadog Code Threat Detection

Kassen Qian


Daniel Blazquez


Christoph Hamsen


Automated developer workflows and AI coding assistants have changed what a pull request (PR) can do. PRs can now modify CI configuration, trigger jobs, access secrets, and initiate release workflows before code ever reaches production. That reach makes them a critical and increasingly targeted part of the supply chain. In supply chain incidents such as[tj-actions](https://www.cisa.gov/news-events/alerts/2025/03/18/supply-chain-compromise-third-party-tj-actionschanged-files-cve-2025-30066-and-reviewdogaction) ,[Nx s1ngularity](https://securitylabs.datadoghq.com/articles/learnings-from-recent-npm-compromises/#s1ngularity-august-26-2025) , and[Shai-Hulud](https://securitylabs.datadoghq.com/articles/shai-hulud-2.0-npm-worm/) , attackers bypassed production applications to target the systems that created them, including source code repositories, CI/CD pipelines, and dependency caches.


[Datadog Code Threat Detection](https://www.datadoghq.com/product-preview/malicious-pr-protection/) detects GitHub PRs that are attempting to compromise your CI pipeline or inject malicious code into your codebase. It uses AI-assisted analysis to examine a PR’s full diff alongside repository metadata, dependency context, and actor information to surface behavior that traditional scanners and manual review may miss.


In this post, we’ll look at how teams candetect malicious code changes andinvestigate and escalate Code Threat signals in Datadog .


## Detect malicious code changes, not just vulnerable code


Most application security tools focus on vulnerabilities, which are unintended flaws in code that an attacker can exploit once the application is running. Unlike a vulnerability, a malicious code change takes advantage of the trusted workflows that teams depend on to ship software—the contributors, CI systems, package registries, and release pipelines that form the path to production. Attackers target that trust directly, and a single malicious PR can compromise downstream systems before anyone reviews it. In the[HackerBot Claw campaign](https://www.datadoghq.com/blog/engineering/stopping-hackerbot-claw-with-bewaire/) , for example, attackers targeted GitHub Actions and LLM-powered workflows through PRs in public repositories.


AI-assisted development expands this attack surface further. Coding agents and repository automation systems can now generate code, open PRs, respond to issues, and interact with CI/CD workflows with limited human oversight. As more PRs come from bots, agents, and external contributors, the volume can outpace manual review. Addressing both problems requires visibility into vulnerable code and code changes that abuse the development process itself.


Datadog Code Threat Detection brings source code attack detection to PRs monitored by Datadog Code Security so teams can verify trust at the code level, regardless of whether a change comes from a human contributor or a coding agent. It evaluates PRs for obfuscated code and suspicious dependency changes, as well as other behaviors associated with source code attacks, such as typo-squatting, credential exfiltration attempts, and CI/CD workflow tampering. Because these detections indicate active attacks, findings bypass the PR comment thread and raise a security signal in Datadog instead.


Datadog routinely evaluates Code Threat findings against a labeled dataset of threats that are built from internal testing, public incident reports, GitHub threat modeling, and software supply chain security research. This ongoing tuning improves detection probability of real attacks while keeping the number of false positives low.


## Investigate and escalate Code Threat signals in Datadog


Code Threat Detection generates signals directly in Datadog’s[Signals Explorer](https://app.datadoghq.com/security/code-security/signals) , where security and engineering teams can investigate suspicious PRs alongside other security signals. Each signal includes context about the detected behavior—whether a PR is attempting to tamper with CI/CD systems, exfiltrate credentials, abuse dependencies, or manipulate release workflows—so teams can investigate without switching tools.


With each signal, teams can open a case, archive it as a false positive, or escalate the issue to an incident.


Code Threat Detection is integrated with Datadog’s[Workflow Automation](https://docs.datadoghq.com/actions/workflows/) , so teams can route signals into the tools and workflows they already use. For example, a signal can trigger a workflow that automatically notifies owners via third-party systems such as Slack and PagerDuty. They can also create custom workflows that interact directly with GitHub for automated remediation.


## Get started with Code Threat Detection


Code Threat Detection extends Datadog Code Security to the PR level, giving teams a way to detect source code attacks before they reach the CI/CD pipeline or production environment. By evaluating PRs for obfuscated code, dependency abuse, credential exfiltration attempts, and CI/CD workflow tampering, Code Threat Detection addresses the part of the supply chain that vulnerability scanners don’t cover. It complements Secret Scanning, Software Composition Analysis, and Infrastructure as Code Security to give you detection across the full path to production.


Datadog Code Threat Detection is[available in Preview for Datadog Code Security customers](https://www.datadoghq.com/product-preview/malicious-pr-protection/) . To learn more,[read our Code Security documentation](https://docs.datadoghq.com/getting_started/code_security/?tab=staticcodeanalysissast) . For a deeper look at the engineering approach behind Code Threats, read our post about[detecting malicious code at scale with LLMs](https://www.datadoghq.com/blog/engineering/scaling-malicious-code-detection/) .


If you don’t already have a Datadog account, you cansign up for a free 14-day trial to get started.


-
-
-
