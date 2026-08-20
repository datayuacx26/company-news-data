---
schema_version: "1.0.0"
document_id: "88f3bd11202e0b2f584e8b83adf874e34dfef89730d6d9c27fb2333acef4b4bc"
company_key: "yc-replit"
company: "Replit"
source_id: "yc-replit-news-import-9d99ff8f4466"
canonical_url: "https://replit.com/blog/security-center"
published_at: "2026-05-07T17:00:00+00:00"
first_seen_at: "2026-07-23T23:34:32.089077+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:60faf022f435021593d88d9483e28b53afe8390c881d5fe036719271def5dda4"
---

# Security Center 2.0: Act on vulnerabilities in bulk across all your apps

Replit is the most secure place to vibe code. Today, we're making it dramatically easier to use the Security Center to understand your security posture across all Replit projects in your business. We’re also making it possible to remediate urgent security holes, such as active critical vulnerabilities on multiple published projects, in seconds.


You can reach the Security Center from the Replit Homepage, or your Settings. When you land there, the first thing we want to answer is simple: *which of my projects are actually at risk right now?*


## Start with what's urgent


At the top of Security Center, we break your exposure down by critical and high severity vulnerabilities. You'll see how many of your projects have critical or high CVEs, how many of those projects are published, and — most importantly — how many are both published *and* public.


That last number on the right is the one you probably want to look at first. From the same top section, you can optionally kick off a security scan that runs a dependency scan across every project in your account and produces a software bill of materials. We run these in the background every few hours, as well as whenever a new CVE is published, so your projects stay up to date.


## Take action across all your projects


Below the overview is a table listing all dependency vulnerabilities, grouped by project. The goal of this table is to let you take action on vulnerabilities across your entire fleet of projects from one place, instead of hunting through them individually. At launch, we support the following set of bulk actions:


- Notifying all project owners for selected apps that their projects are vulnerable
- Unpublishing apps meeting the selected criteria — depending on your risks, it can be better to be down than insecure


A typical flow might look like this: filter down to critical vulnerabilities, narrow further to projects that are published and public, and then notify the owners of every project in that filtered set.


## Help people ship the fix, not just flag the problem


Notifying and bulk unpublishing are blunt instruments. In some cases, you’ll need to *help* the project owner ship the fix. From the dependency vulnerability table, you can click “ **Fix with Agent”** on a project. Behind the scenes, this creates a task with the proposed patch for the selected vulnerabilities in a project. The project owner only needs to review the changes, apply them, and republish the project.


We deliberately keep this a per-project action rather than a bulk one. A project might have other changes in flight. Once again, the parallelism offered by Replit background tasks enables making changes without disrupting current work in flight. But projects still deserve a human review before republishing.


## Software bill of materials


Finally, if you are an Enterprise customer, the software bill of materials (SBOM) obtained from bulk scans lives at the bottom of Security Center, the same way it does today. It's the standardized artifact your security and compliance teams use to answer questions like "are we affected by this newly disclosed CVE?". An SBOM is a complete inventory of every dependency your projects pull in — direct and transitive — along with their versions and licenses.


To retrieve an SBOM for your workspace, you’ll need to click “Run Scan” at the top of the Security Center.
