---
schema_version: "1.0.0"
document_id: "1eb442b837881530b0e12b42c6936dfefa1c97c05ef834d28bbb406b205f2a81"
company_key: "netskope-inc-class-a-common-stock"
company: "Netskope Inc."
source_id: "netskope-inc-class-a-common-stock-rss-c0a3e1ef9778"
canonical_url: "https://www.netskope.com/blog/beyond-shadow-ai-the-netskope-ai-report"
published_at: "2026-07-28T04:15:00+00:00"
first_seen_at: "2026-08-10T01:01:10.452862+00:00"
fetched_at: "2026-08-10T01:01:12.343007+00:00"
content_hash: "sha256:9b52191554881af23833f3a1294d165fb10006bca1dfcc869cdd3448358b6ca3"
---

# Beyond Shadow AI: The Netskope AI Report

Today, we published[our latest AI report](https://www.netskope.com/resources/threat-labs-reports/netskope-ai-report-2026) , which documents a significant shift in security risks[since our last report](https://www.netskope.com/resources/cloud-and-threat-reports/cloud-and-threat-report-generative-ai-2025) . Our analysis, which spans from June 2025 through July 2026, highlights the underlying architectural evolutions that caused these shifts and the following three points:


**Downstream policy violations are now the primary architectural risk.**


While we spent the previous two years focusing on “upstream” data leakage (users pasting sensitive data into prompts), the risk surface has shifted. We have observed a 4x increase in Model Context Protocol (MCP) traffic in the past 10 weeks alone. MCP bridges internal data stores with external models, creating a direct path for AI services to return sensitive data that users or agents are not authorized to access. This is no longer a user behavior problem; it is an architectural vulnerability that requires granular, downstream inspection.


**Agentic coding apps have created new execution vectors.**


The adoption of agentic coding tools—specifically Claude Code and Codex—has moved from negligible levels a year ago to 75% and 58% of organizations, respectively. This creates a critical shift in the threat model: every agentic interaction is now a potential execution vector for malicious code.


**The AI supply chain is being weaponized.**


We are seeing attackers move beyond simple AI baits to trojanized developer tools and fake AI installers, and become adept at tricking AI apps into surfacing malicious content, including malicious code and links to malicious resources on the web.


You can review the full data set and our technical recommendations here:[https://www.netskope.com/resources/threat-labs-reports/netskope-ai-report-2026](https://www.netskope.com/resources/threat-labs-reports/netskope-ai-report-2026) .
