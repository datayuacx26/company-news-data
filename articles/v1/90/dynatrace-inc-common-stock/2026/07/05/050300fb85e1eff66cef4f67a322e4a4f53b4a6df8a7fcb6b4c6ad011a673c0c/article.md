---
schema_version: "1.0.0"
document_id: "050300fb85e1eff66cef4f67a322e4a4f53b4a6df8a7fcb6b4c6ad011a673c0c"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/how-cisa-bod-26-04-is-reshaping-the-vulnerability-remediation-approach/"
published_at: "2026-07-23T14:12:08+00:00"
first_seen_at: "2026-07-23T15:12:36.951244+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:18d6a341f6aafc848398333825218de9bf15cef0def0532dd76ee113e1efb881"
---

# How CISA BOD 26-04 is reshaping the vulnerability remediation approach

The U.S. Cybersecurity and Infrastructure Security Agency (CISA)’s recently introduced[Binding Operational Directive (BOD) 26-04](https://www.cisa.gov/news-events/directives/bod-26-04-prioritizing-security-updates-based-risk) is fundamentally changing how organizations need to approach vulnerability remediation.


A Binding Operational Directive (BOD) is a compulsory direction to U.S. federal, executive branch, departments, and agencies for purposes of safeguarding federal information and information systems.


Instead of relying on traditional static severity scores, the directive mandates a **risk-based prioritization model** that shifts the focus and prioritization to remediation efforts on vulnerabilities that are most at risk to be exploited and cause real-world impact. See[cisa.gov](https://www.cisa.gov/news-events/directives/bod-26-04-prioritizing-security-updates-based-risk) for more details.


Specifically, organizations that fall under U.S. federal agencies, and any vendors that work in that space, are now mandated to evaluate vulnerabilities based on four key factors:


- Whether the asset is publicly exposed – is the asset internal or external facing?
- Whether exploitation is already observed – is it a[Known Exploited Vulnerability (KEV)](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) ?
- Whether attacks can be automated – can threat actors automate all the steps needed to exploit the vulnerability?
- The potential impact of a successful exploit – once exploited, can the threat actor gain partial or total control over the vulnerable asset?


When all four of these conditions are met, remediation timelines shrink dramatically, highlighting the urgency of modern threat response.


Image:[CISA](https://www.cisa.gov/news-events/directives/bod-26-04-prioritizing-security-updates-based-risk) remediation timelines graph ([image credit : cisa.gov](https://www.cisa.gov/news-events/directives/bod-26-04-prioritizing-security-updates-based-risk) )


Security teams already face fundamental challenges trying to manage vulnerabilities in scaling environments:


- The volume of vulnerabilities continues to grow – Over 48,000 new CVEs were published in 2025 — roughly 131 every single day — and 2026 is projected to exceed 50,000 for the first time ([source](https://www.cve.org/About/Metrics) ).
- Exploitation timelines are shrinking due to automation and AI – Mandiant’s M-Trends 2026 report found that 28.3% of CVEs are now being exploited within 24 hours of disclosure.
- Traditional prioritization methods (like the CVSS score) no longer reflect real-world risk and impact.


CISA’s directive acknowledges the new reality: Not all vulnerabilities are equal and treating them as such leads to ineffective security outcomes. While the directive currently applies to U.S. federal agencies, its approach is already strengthening best practices across industries, and Dynatrace believes the risk-based approach may help organizations improve prioritization.


## Why isn’t CVSS enough to prioritize vulnerabilities?


Historically, organizations have relied on severity-based prioritization when dealing with vulnerabilities, such as Low/Medium/High/Critical scoring and CVSS scoring. But these approaches lack context. They often overemphasize theoretical risk while missing what’s actively being exploited, leading teams to waste time on unlikely threats and delaying response to real ones.


Attackers are speeding up, using AI to discover and weaponize vulnerabilities faster than ever, shrinking the window between disclosure and exploitation.


That’s creating a gap. **Security teams are drowning in data—alerts, findings, remediation guidance—but still struggle to act on the right signals.** The challenge is no longer finding vulnerabilities; it’s knowing which ones actually matter.


## What does risk-based vulnerability prioritization look like in practice?


The move towards a **risk-based prioritization model** outlined in BOD 26-04 reflects a fact that the security industry has already acknowledged: The need to move toward **runtime-informed, risk-driven security** where prioritization is based on real exposure and impact, not just theoretical severity.


Dynatrace helps organizations operationalize this model by combining:


CISA requirement


Dynatrace capability


**Publicly exposed asset**


Is the vulnerable asset reachable from the internet?


**Smartscape topology**


Automatically maps every service, process, and host — including public-facing entry points — which helps to determine exposure in real time, not from stale inventory data.


**KEV status**


Is this CVE on CISA’s Known Exploited Vulnerabilities catalog?


**Threat intelligence enrichment**


Continuously correlates detected vulnerabilities against KEV feeds and threat-intel sources via the Dynatrace Vulnerability feed, surfacing catalog matches alongside CVSS scores in a single view.


**Automatable exploitation**


Can this vulnerability be exploited programmatically at scale?


**Exploitability context**


Evaluates whether vulnerable code paths are actually reachable and executable in your environment — distinguishing theoretical risk from confirmed, actionable exposure.


**Technical impact**


What is the blast radius if this vulnerability is exploited?


**Runtime dependencies & business-service context**


Links vulnerable components to upstream and downstream services, APIs, and business transactions — helping you to better comprehend real-world impact across your entire application stack.


## How to use runtime context to identify real exposure


### Full-stack visibility shows what’s actually exposed.


Understanding whether an asset is truly exposed requires real-time visibility across applications, infrastructure, and services. Dynatrace helps security teams see which services are internet-facing, where vulnerabilities exist at runtime, and how those assets are connected across the environment. This aligns directly with BOD 26-04’s emphasis on asset exposure as a primary risk signal.


### Context turns vulnerability data into risk signals.


Modern prioritization requires more than vulnerability data alone. Dynatrace continuously analyzes vulnerabilities against live runtime topology, service dependencies, application relationships, runtime behavior, observability data, and threat intelligence from the Dynatrace vulnerability feed. This gives teams valuable context to prioritize based on real attack paths, actual exploitability, and business impact — not static severity scores alone.


For example, the above image shows an example of prioritization, where both vulnerabilities have a CVE score of 10 and are present in the KEV catalog. But if only one is internet-facing, reachable at runtime, or connected to a critical business service, the remediation priority changes. That is the shift BOD 26-04 pushes organizations toward: multi-factor risk evaluation based on real exposure and impact.


## Continuously reprioritize as risk changes


### Active risk signals need continuous monitoring.


CISA places strong emphasis on whether a vulnerability is actively exploited, automatable, or newly added to the KEV catalog. In practice, that means teams need to monitor active threats, changes in exposure, and emerging exploit activity as conditions change. Dynatrace helps by mapping those signals to affected applications, infrastructure dependencies, and runtime behavior, so teams can better discern where exposure is highest and where to act first.


### Faster remediation depends on sharper prioritization.


With remediation windows shrinking to days — or less — execution becomes the bottleneck. A unified platform approach helps teams reduce noise, align security and operations, and focus remediation where it will reduce the most risk. With[Dynatrace Runtime Vulnerability Analytics](https://docs.dynatrace.com/docs/secure/application-security/vulnerability-analytics) , teams can track remediation progress in real time and automatically reprioritize as risk signals change, whether from a shift in exposure, a new KEV listing, or evolving threat context.


## What´s next


With the release of BOD 26-04, CISA recognizes a broader industry shift:


- From vulnerability management → to exposure management.
- From severity-based prioritization → to runtime, risk-based decision making.
- From periodic scanning → to continuous, contextual security insights.


We highly recommend that organizations, whether you are a U.S. federal agency or not, familiarize themselves with the specifics of CISA’s BOD 26-04, and the additional information CISA has around this Directive.


In essence, all of this means organizations will increasingly need to:


- Integrate runtime observability with security workflows
- Focus on real exploitability and blast radius
- Automate prioritization and response


As the threat landscape continues to evolve—especially with the growing role of AI—security teams that can **connect data, context, and automation** will be best positioned to reduce risk effectively.


Do you want to know more about CISA BOD 26-04 and how Dynatrace can specifically help your organization? Learn how we can help your organization to reduce risk effectively.
[Learn more](https://www.dynatrace.com/signup/playground/application-security/)
