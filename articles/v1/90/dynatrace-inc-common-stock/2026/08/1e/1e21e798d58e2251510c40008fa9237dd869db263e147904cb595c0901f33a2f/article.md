---
schema_version: "1.0.0"
document_id: "1e21e798d58e2251510c40008fa9237dd869db263e147904cb595c0901f33a2f"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/cve-2026-34046-broken-authorization-in-langflow-exposes-user-flows/"
published_at: "2026-08-07T17:25:56+00:00"
first_seen_at: "2026-08-07T18:11:57.790101+00:00"
fetched_at: "2026-08-07T18:11:58.556433+00:00"
content_hash: "sha256:f3587afb9bb18ec085ae79b147d73b95303c6928c7fe9d43ed13cc210762fb02"
---

# CVE-2026-34046: Broken authorization in Langflow exposes user flows

Dynatrace Security Researchers discovered a zero-day vulnerability in Langflow that allows any authenticated user to read, modify, or delete flows belonging to other users. Disclosed as CVE-2026-34046, this is a high severity vulnerability with a CVSS score of 8.7, affecting versions up to and including 1.5.0. To remediate this vulnerability in your environment, we strongly encourage to upgrade to version 1.5.1 or later.


## How Dynatrace detected a zero-day in Langflow prior to public disclosure


In mid‑2025, Dynatrace Security Researchers investigated[CVE‑2025‑3248](https://www.dynatrace.com/news/blog/remediating-cve-2025-3248-how-dynatrace-application-security-protects-agentic-ai-applications/) , a critical remote code execution vulnerability in Langflow, a framework for building agentic AI applications. While analyzing the Langflow codebase as part of that research, we identified a separate and distinct vulnerability in its authorization logic. We reported this issue to the Langflow maintainers on July 8, 2025. On March 27, 2026, it was formally published as[CVE-2026-34046](https://nvd.nist.gov/vuln/detail/CVE-2026-34046) .


Customers using the[Dynatrace Vulnerability Feed](https://www.dynatrace.com/news/blog/introducing-the-dynatrace-vulnerability-feed-accurate-transparent-and-threat-aware/) can have visibility about similar types of vulnerabilities months earlier thanks to real-time, curated vulnerability intelligence enriched with runtime context.


This post covers the details of CVE-2026-34046, the disclosure timeline, and how the Dynatrace Vulnerability Feed enables customers to detect and act on emerging threats earlier than traditional approaches.


### Why this matters


In modern AI-driven applications, flows often contain sensitive data, API keys, and proprietary logic. Without runtime visibility, vulnerabilities like this can remain undetected or be misprioritized, increasing the risk of data exposure and unauthorized access.


## Disclosure timeline


**Date** **Event**


July 8, 2025


Dynatrace reports the vulnerability to


Langflow


maintainers


July 9, 2025


Langflow[commits a fix](https://github.com/langflow-ai/langflow/commit/d437d018ceba95242fcf4c1279df214d96314f2e) (PR[#8956](https://github.com/langflow-ai/langflow/pull/8956) )


March 25, 2026


Langflow maintainers publish the advisory to their security repository


March 27, 2026


Advisory published to the[GitHub Advisory](https://github.com/advisories/GHSA-8c4j-f57c-35cf) Database and NVD as CVE-2026-34046


The fix arrived one day after our report, but the advisory took longer. Open-source projects are important, but often deal with significant time constraints. During this window, organizations without access to enriched or proprietary intelligence would have had no visibility into this risk. This is where Dynatrace can help.


## What you need to know


CVE-2026-34046 is a broken authorization vulnerability in Langflow’s flow management API, rated **high severity with a CVSS score of 8.7** .


The root cause lies in the` _read_flow` helper function in` src/backend/base/langflow/api/v1/flows.py` , which is used when reading, updating, and deleting flows. When` AUTO_LOGIN` was disabled, meaning authentication was active, no ownership check was enforced. The query returned any flow matching the given UUID, regardless of who owned it.


The vulnerable function ([source](https://github.com/langflow-ai/langflow/blob/73c1f203b0205d00928a0af7fa0e0fc93170f03e/src/backend/base/langflow/api/v1/flows.py) ):


```text
async def _read_flow(
session: AsyncSession,
flow_id: UUID,
user_id: UUID,
settings_service: SettingsService,
):
"""Read a flow."""
auth_settings = settings_service.auth_settings
stmt = select(Flow).where(Flow.id == flow_id)
if auth_settings.AUTO_LOGIN:
# If auto login is enabled, user_id can be current_user.id or None
# so write an OR
stmt = stmt.where(
(Flow.user_id == user_id) | (Flow.user_id == None)  # noqa: E711
)


return (await session.exec(stmt)).first()
```


The ownership check only runs when` AUTO_LOGIN` is enabled. In authenticated multi-user deployments — exactly where access controls matter most — no such check was enforced. Any logged-in user could read, modify, or delete flows belonging to other users simply by knowing or guessing a flow UUID.


Because Langflow flows frequently contain embedded API keys and proprietary agent logic, unauthorized read access alone can have serious consequences.


Affected versions:` langflow <= 1.5.0` and` langflow-base <= 0.5.0.`


To remediate, upgrade to` langflow >= 1.5.1` and` langflow-base >= 0.5.1.`


## Why timeliness matters for your security coverage


When a high‑profile vulnerability is discovered in a widely used project, maintainers tend to move quickly on advisories. The earlier langchain vulnerability, CVE‑2025‑3248, is a good example: It was patched and disclosed within a relatively short timeframe. As we described in our write up of CVE‑2025‑55182 (React2Shell), critical vulnerabilities in widely used libraries often receive fast, coordinated disclosure because the exposure is large and the urgency is clear.


CVE‑2026‑34046 followed a different path. Although a fix was committed the day after our report, it was listed as an internal refactor rather than a security fix along with delayed advisory.


While this is not uncommon in smaller open-source ecosystems, it reinforces an important reality: relying exclusively on public vulnerability databases introduces blind spots. Security teams need continuous, real-time insight into emerging risks—regardless of when or whether they are formally disclosed.


## How Dynatrace customers were protected


Earlier this year, Dynatrace introduced the[Dynatrace Vulnerability Feed](https://www.dynatrace.com/news/blog/introducing-the-dynatrace-vulnerability-feed-accurate-transparent-and-threat-aware/) delivering continuously updated, high-fidelity vulnerability intelligence enriched with real-time context from running applications. Unlike traditional feeds that depend on public disclosures, Dynatrace integrates proprietary research and global intelligence to surface risks earlier.


In the case of CVE-2026-34046, Dynatrace customers had visibility into this vulnerability months before it appeared in public databases. This early insight enables teams to proactively assess risk, validate exposure, and take action—without waiting for external advisories.


Combined with runtime context, Dynatrace goes beyond detection by identifying which vulnerabilities are actually reachable and exploitable in production environments.


The Dynatrace Vulnerabilities App provides detailed, real-time insights into detected vulnerabilities, including exploitability, affected services, and risk context. This allows teams to focus remediation efforts on vulnerabilities that pose actual risk in their environment.


## Remediation


Securing agentic AI frameworks requires more than reactive patching. Organizations need continuous visibility into vulnerabilities, real-time insight into exploitability, and the ability to act with confidence based on actual runtime risk.


If CVE-2026-34046 is detected in your environment, upgrade the following packages:


**Python package** **Remediated version**


langflow 1.5.1 or higher


langflow-base 0.5.1 or higher


[Dynatrace Runtime Vulnerability Analytics](https://docs.dynatrace.com/docs/secure/application-security/vulnerability-analytics) surfaces which vulnerabilities are actually reachable and exploitable in your production environment, providing runtime context that moves teams from theoretical exposure to confirmed risk. For agentic AI frameworks like Langflow — where flows can carry credentials and sensitive logic — that precision matters.


Dynatrace Runtime Vulnerability Analytics continuously analyzes application behavior to determine which vulnerabilities are truly reachable and exploitable. By combining this runtime intelligence with curated vulnerability data, Dynatrace enables teams to prioritize risk accurately, reduce noise, and remediate faster—based on real exposure, not theoretical findings.


Explore our playground to see how Dynatrace helps you detect and prioritize vulnerabilities.
[Explore Playground](https://www.dynatrace.com/signup/playground/application-security/)
