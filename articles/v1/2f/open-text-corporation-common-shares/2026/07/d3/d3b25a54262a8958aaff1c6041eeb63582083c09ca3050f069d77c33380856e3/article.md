---
schema_version: "1.0.0"
document_id: "d3b25a54262a8958aaff1c6041eeb63582083c09ca3050f069d77c33380856e3"
company_key: "open-text-corporation-common-shares"
company: "Open Text Corporation Common Shares"
source_id: "open-text-corporation-common-shares-rss-7d5a2b95ba15"
canonical_url: "https://blogs.opentext.com/level-up-your-agentic-appsec-powers/"
published_at: "2026-07-06T17:06:30+00:00"
first_seen_at: "2026-07-20T23:17:12.954110+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:e039c271022a15b37d30d3478d92679da448ed073702a7ed9892996f27c0c1f3"
---

# Level up your agentic AppSec powers

The first half of 2026 has been a blur for application security. Agentic AI has moved from experiment to expectation, reshaping how software is written, reviewed, attacked, and defended. Across security, engineering, and executive teams, many are still trying to grasp what this shift really means in practice.


As we kick off the second half of the year, one thing is clear: the agentic[AppSec evolution](https://community.opentext.com/cybersec/b/cybersecurity-blog/posts/ai_2d00_didnt_2d00_break_2d00_application_2d00_security) isn’t slowing down. However, the path forward is coming into focus. That path isn’t about replacing everything with AI, nor is it about pointing a coding agent at your codebase and hoping for the best. It’s about a pragmatic “yes, and…” mindset, which means layering agentic capabilities as part of a holistic approach.


Agentic analysis adds a powerful new dimension. It can reason over application logic in ways that resemble expert human review, uncovering issues tied to business logic, authorization design, and complex flows that are difficult to model with traditional techniques. But it works best when used deliberately, alongside the proven speed, scale, and determinism of an established[AppSec platform](https://www.opentext.com/products/application-security) .


## Introducing Fortify Agentic Analyzer


On July 1, we launched **OpenText™ Fortify Agentic Analyzer** , a new AI-powered capability for the Fortify portfolio. Fortify Agentic Analyzer is a multi-agent, multi-phase analysis harness that identifies security vulnerabilities through reasoning over your codebase. It expands the application security testing stack by complementing OpenText[Fortify SAST](https://www.opentext.com/products/static-application-security-testing) and other AST technologies, not replacing them. In addition to new vulnerability detection pathways, Fortify Agentic Analyzer can leverage results from prior SAST scans to deduplicate findings. This helps teams focus on what was previously missed and zero in on true blind spots.


The future of AppSec isn’t about choosing between traditional scanning and AI-driven analysis. It’s about orchestrating both.


## More than a prompt


There is a fundamental difference between “using an LLM on code” and delivering a repeatable, reliable security analysis capability. Fortify Agentic Analyzer delivers a dedicated harness that enables agentic reasoning at scale. It


- enumerates and scopes the codebase.
- orchestrates parallel analysis across agents.
- applies deterministic post-processing.
- filters and deduplicates findings.
- aligns results to the Fortify taxonomy.
- outputs industry-standard SARIF for downstream consumption.


This architecture ensures consistency, coverage, and integration into existing AppSec workflows, transforming agentic analysis from an experiment into an operational capability.


## Built for real-world AppSec programs


Agentic analysis is powerful, but it comes with real LLM token costs that can quickly add up. In this sense, it has more in common with a pen test than with SAST,[SCA](https://www.opentext.com/products/core-software-composition-analysis) or[DAST](https://www.opentext.com/products/dynamic-application-security-testing) . With these pragmatic cost considerations in mind, today it is best applied as a targeted, high-value analysis tool for high-risk applications, critical releases, and major changes or architectural shifts. In practice, this creates a clear model:


- Run SAST, SCA, and DAST continuously in the DevSecOps pipeline
- Apply agentic analysis selectively for deeper, risk-based insights
- Manage all findings centrally in the Fortify platform


This is what agentic AppSec looks like when grounded in enterprise reality.


## Flexibility and safety by design


Fortify Agentic Analyzer is built to give teams both flexibility and control. It follows a bring-your-own-LLM model, allowing customers to use their preferred providers and models, with flexible configuration across analysis stages. An embedded coding agent is included out of the box, with the option to use Claude Code as an alternative coding agent.


At the same time, it is designed with strong safety constraints. The analysis model enforces a clear principle: read-only, controlled execution. The agent is limited to inspecting code and reporting structured findings. It cannot modify source files, execute arbitrary commands, or independently access external systems.


This balance is intentional. Teams can take advantage of rapidly evolving AI capabilities while maintaining the control, predictability, and security required in enterprise environments.


## Available to existing Fortify customers


Fortify Agentic Analyzer is included with existing Fortify SAST licenses and is available to customers across OpenText[Fortify on Demand](https://www.opentext.com/products/fortify-on-demand) , on-premises, and private cloud deployments. To get started, reach out to your[OpenText account team](https://solutions.opentext.com/cybersecurity/?_gl=1*bdhda2*_gcl_au*NjI3NTI0MTcxLjE3ODE1NDQ0OTQuMjg1NzExNjM1LjE3ODI3OTkwNTkuMTc4Mjc5OTA2Ng..*_ga*MTUzMzAzNTg5My4xNzgxNTQ0NDk0*_ga_8LWPJ50ZXY*czE3ODMxMTU2ODkkbzI1JGcxJHQxNzgzMTE2MDE2JGo2MCRsMCRoNDczMTk1NzUz#gate-dafd963f-071a-4355-8334-73af28794874) .


## New Fortify skills for agentic workflows


In addition to Fortify Agentic Analyzer, we are also introducing another round of new OpenText[Fortify Agent Skills](https://github.com/fortify/skills/) that bring security earlier into AI-assisted development workflows:


- Fortify-change-review: Analyzes security-relevant code changes, whether generated by developers or coding agents, directly in the IDE. This provides immediate feedback upstream of CI/CD and integrated scanning
- Fortify-dependency-upgrade: Helps automate dependency upgrades, including complex version changes that may introduce breaking changes. This accelerates remediation in an environment where new vulnerabilities and exploits are rapidly increasing


Together, these skills extend security coverage across the development lifecycle from code creation to deep analysis and remediation.


## Defenders level up


Agentic AI is changing how software is built and how it must be secured. The winning approach isn’t “either/or” but instead augmenting your proven AppSec program with agentic capabilities. Fortify Agentic Analyzer and our growing set of agentic solutions are designed to help teams do exactlythat .


[Get started: Reach out to the OpenText account team today!](https://solutions.opentext.com/cybersecurity/?_gl=1*bdhda2*_gcl_au*NjI3NTI0MTcxLjE3ODE1NDQ0OTQuMjg1NzExNjM1LjE3ODI3OTkwNTkuMTc4Mjc5OTA2Ng..*_ga*MTUzMzAzNTg5My4xNzgxNTQ0NDk0*_ga_8LWPJ50ZXY*czE3ODMxMTU2ODkkbzI1JGcxJHQxNzgzMTE2MDE2JGo2MCRsMCRoNDczMTk1NzUz#gate-dafd963f-071a-4355-8334-73af28794874)
