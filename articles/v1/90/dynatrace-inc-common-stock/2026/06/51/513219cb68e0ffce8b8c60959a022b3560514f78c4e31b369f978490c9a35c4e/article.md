---
schema_version: "1.0.0"
document_id: "513219cb68e0ffce8b8c60959a022b3560514f78c4e31b369f978490c9a35c4e"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/in-context-security-findings-for-kubernetes/"
published_at: "2026-06-30T15:48:43+00:00"
first_seen_at: "2026-07-20T03:31:43.037247+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:6423eed48d7aac5235125f30f5f1de2c0a5961a2ec3d784abe4252692a56a6cb"
---

# Your business applications are at risk: Introducing in-context security findings for Kubernetes

Dynatrace is making Kubernetes® security faster to adopt, easier to understand, and actionable in real time. Dynatrace now embeds Kubernetes security findings directly into the Kubernetes app, giving developers and SREs real-time, contextual insight into vulnerabilities, threats, and misconfigurations — without switching tools.


**[Kubernetes Security Posture Management (KSPM)](https://www.dynatrace.com/knowledge-base/kubernetes-security-posture-management-kspm/)** is a specialized subset of Security Posture Management (SPM) that focuses on container orchestration platforms. Its main purpose is to identify misconfigurations and compliance gaps across clusters. Here are the latest enhancements:


- **KSPM is turned on by default** during Kubernetes cluster onboarding
- **Security findings are surfaced directly in the Kubernetes app**
- **Node Configuration Collector (NCC) occurs automatically** via the Dynatrace Operator. These improvements reduce operational overhead for platform and SRE teams while embedding security insights directly into runtime context, so teams can quickly understand, prioritize, and act without switching tools.


Dynatrace now unifies:


- **Vulnerability findings** (runtime-relevant CVEs)
- **Detection findings** (active threats)
- **Misconfiguration findings** (insecure configurations or compliance gaps)


The result is a **single, contextual view of risk** across your Kubernetes workloads.


Kubernetes monitoring deployment page with integrated SPM.


Kubernetes monitoring security findings


## The problem: Security findings without context slow teams down


Today, security and development teams operate with fragmented visibility:


- Developers receive CVEs or misconfiguration alerts **without runtime context**
- Security teams know what’s wrong but lack clarity on **where the issues are and who owns them**
- Findings and workloads exist in **separate tools, owned by different teams**


### The impact


This fragmented visibility ultimately introduces friction and delay with each hand-off. The result is lower remediation, unclear ownership, and increased exposure risk for business-critical applications.


Kubernetes monitoring security findings per namespace


## Security in context, without the context switch


Dynatrace closes this gap by embedding security findings directly into the Kubernetes app. A new **Security** tab is now available at the workload, namespace, and cluster levels, giving teams a unified view without leaving the platform they already work in.


## Three types of findings unified into a single view


The **Security** tab surfaces three categories of findings.


1. **Vulnerability findings** focus on CVE libraries loaded at runtime, so findings reflect what is actually running, not everything that’s installed.
2. **Detection findings** flag signals of active malicious behavior, tied directly to specific Kubernetes resources.
3. **Misconfiguration findings** highlight deviations from benchmarks starting with CIS, visible in near real time.


## Why this matters


Developers can now identify security issues while investigating performance or availability, see findings in the exact context of the affected workload, and act immediately, without waiting for an external ticket to make its way through the queue.


Consider a practical example: a developer investigating a workload discovers that Kubernetes secrets are exposed as environment variables instead of mounted as files. Without this integration, that issue might surface days later, stripped of actionable context and requiring cross-team coordination to resolve. With Dynatrace, it appears in real time, in the same workflow, with clear remediation guidance attached.


Security Posture Management app showing that Kubernetes secrets are exposed as environment variables


## From insight to fix, without leaving the platform


Starting from the **Security** tab, developers can jump straight into the Security Posture Management app, pre-filtered to the exact finding they’re investigating, no manual searching required. From there, Smartscape® topology provides full runtime context at a glance: the affected workload, namespace, cluster, and its dependencies, all mapped out instantly. (This same type of user journey is also supported for the Vulnerabilities app and Threats & Exploits.)


## Guidance that tells you what to do, not just what’s wrong


Rather than leaving teams to interpret raw findings, Dynatrace Intelligence delivers plain-language recommendations tailored to the specific benchmark and workload. In the secrets exposure example, the guidance is concrete: update your YAML configs to mount sensitive data as files rather than environment variables. No guesswork, no interpretation overhead.


The result is that teams can move from detection to remediation within a single session, without switching tools or waiting on another team to connect the dots.


## Onboard Kubernetes clusters to Dynatrace for monitoring with automatic rollout and enablement of KSPM scans


Dynatrace removes friction from Kubernetes security adoption by making it automatic from the start. KSPM is now turned in during Kubernetes onboarding, with the Dynatrace Operator automatically deploying NCC without any manual configuration.


### How it works


1. Start cluster onboarding in the Kubernetes app.
2. Select your monitoring mode ( **KSPM is turned on by default** ).
3. Install the Dynatrace Operator (Helm or GitOps).
4. NCC is deployed automatically for compliance data collection.
5. KSPM scans begin.
6. Misconfigurations appear **in context** within Kubernetes resources.


## What’s next


Dynatrace will soon expand coverage across diverse Kubernetes environments with the introduction of Kubernetes Security Essentials, a new KSPM profile that provides baseline best-practice security checks across Kubernetes distributions and versions, without requiring node-level configuration data.


## Broader coverage, wherever you run Kubernetes


This matters because not every environment supports full benchmark profiles. Kubernetes Security Essentials extends consistent security posture evaluation to platforms like GKE, OpenShift, or other lightweight distributions. Meaning, even the most lightweight or managed distributions can now benefit from the same foundational security checks as any other environment.


## Get started


Onboard your Kubernetes clusters with Dynatrace to:


- Gain visibility into your **security posture**
- Identify and prioritize **critical misconfigurations**
- Move from **finding to fixing faster**


Get started with[Kubernetes monitoring in Dynatrace today.](https://docs.dynatrace.com/docs/secure/xspm)[Get started today](https://docs.dynatrace.com/docs/secure/xspm)
