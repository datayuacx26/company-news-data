---
schema_version: "1.0.0"
document_id: "c000cc7a595ad9e437c9a2069886c14ee20d6860f10c2b15c903b4816f36f5d5"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-news-import-c17882ce6386"
canonical_url: "https://aws.amazon.com/blogs/security/introducing-aws-continuum-security-at-machine-speed/"
published_at: "2026-06-17T15:34:05+00:00"
first_seen_at: "2026-07-21T06:14:34.184257+00:00"
fetched_at: "2026-07-28T21:23:18.688239+00:00"
content_hash: "sha256:3addf17d19c7f8e406ec301904b7eb906181f5385a7b5665829a597941350a9c"
---

# Introducing AWS Continuum: Security at machine speed

## What we believe


We’ve been thinking deeply about enterprise security. The operating model that served us for the past decade (collect telemetry, store it, query it, build dashboards to watch it) is no longer keeping pace. We need to shift to the new world: telemetry, context, reasoning, and actions. An approach that produces outcomes. The latest cybersecurity frontier models further made this shift urgent. Models like Claude Mythos can now find software vulnerabilities and reason through complex attack paths at machine-speed, leading to an exponentially increasing backlog of vulnerabilities.


## Introducing AWS Continuum for code vulnerabilities


Today, we’re announcing[AWS Continuum](https://aws.amazon.com/continuum)


for code vulnerabilities, now available in gated preview. Continuum for code vulnerabilities addresses the full lifecycle of a code vulnerability at machine speed: from discovery through actions. It reasons over your environment, confirms what is real, and drives toward resolution. It’s model agnostic, using multiple frontier models where each performs best, and is built to incorporate the latest and most capable models as they emerge.


Continuum is built on lessons learned from running security across AWS and Amazon.com. Securing businesses that operate in different industries required a system that understands business context rather than applying generic rules uniformly.


## How it works


Continuum for code vulnerabilities reasons over your full environment. This context includes structured data already living in[Amazon Web Service (AWS)](https://aws.amazon.com/)


(your infrastructure, permissions, network topology, code) and the unstructured data that captures how your organization operates and your risk profile (your documents, communications, business priorities).


Continuum for code vulnerabilities operates in four continuous phases.


1. **Discovery:** Security teams tackle a backlog of vulnerabilities, and many are already using frontier models to find more. Continuum starts by ingesting that existing backlog and performing its own vulnerability scan of your environment. This creates a more comprehensive view of vulnerabilities and the associated attack paths.
2. **Prioritization:** Continuum uses context to evaluate, enrich, and prioritize every finding. Is the affected component deployed, is it reachable, is it in a production path, and what would the business impact be if exploited? The result is an evidence-backed list of priorities, allowing Continuum and your team to focus on what’s most important.
3. **Validation:** Continuum validates findings to surface false positives before they waste your team’s time. It contextualizes vulnerabilities against your environment. It then constructs working exploit examples in a sandboxed environment that provide concrete, reproducible evidence of the issue.
4. **Mitigation and remediation:** Continuum assesses existing defenses around a validated issue, including blocking and compensating controls along with detection mechanisms. It then draws on its understanding of the codebase, context, and findings to recommend mitigation or remediation of the vulnerability with a network change, policy change, or code patch. The patch recommendation is validated using the same system that confirmed the vulnerability. It also provides blast radius visibility and rollback paths where feasible.


This is just the beginning. We’re starting with code (1st and 3rd party) and then expanding to other aspects of security.


## Trust is graduated


Continuum starts in learn mode with a human in the loop. Every recommendation includes the reasoning behind it. As you gain confidence, you can graduate Continuum to enforce mode, enabling remediation that can be increasingly automated based on categories and risk profiles you define.


## Continuum capabilities


In addition to Continuum for code vulnerabilities, Continuum includes capabilities you might already know. The[AWS Security Agent](https://aws.amazon.com/security-agent)


penetration testing and code scanning functionality is now part of Continuum as Continuum pen testing and Continuum code scanning (Preview). We’re also launching Continuum threat modeling in preview, which automatically generates comprehensive threat models from design documents or source code and outputs results in STRIDE format. These capabilities serve as detection and analysis sources that feed into the broader Continuum loop of discovery, prioritization, validation, and remediation.


## Getting started


We’re working with customers across financial services, automotive, and technology to shape AWS Continuum. Customer feedback confirms the direction: security teams want tools that earn trust and take action.


AWS Continuum for code vulnerabilities is available in gated preview. Sign up to request access at[AWS Continuum](https://pages.awscloud.com/global-ln-gc-aws-continuum-interest.html?trk=274c9c25-85f4-42e0-953e-347c044a17bc&sc_channel=el) .


If you have feedback about this post, submit comments in the **Comments** section below.


---
