---
schema_version: "1.0.0"
document_id: "71cc8d5d39c422daae5d5de2a2774e444271def50d9345c2bda5293acb312e50"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/mcp-server"
published_at: "2026-04-07T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:25:41.488077+00:00"
content_hash: "sha256:6b5fc290adcfee5a96b39c278862a664032dee8701fcd5a6433a29cf7515ee5e"
---

# DeepSource MCP Server

Last updated on Apr 7, 2026


Three weeks ago, we shipped the CLI v2 to bring DeepSource into the terminal. Today we're taking that further. The DeepSource MCP server brings DeepSource directly inside every AI agent.


When we launched[AI Review](https://deepsource.com/blog/deepsource-next) , we talked about code review for a world where AI agents write most of the code. The MCP server closes the loop. Instead of your agent writing code, waiting for a human to relay review feedback, and then fixing issues, the agent connects to DeepSource directly, reads the review, and fixes the code itself.


## Why MCP


The[Model Context Protocol](https://modelcontextprotocol.io/) is becoming the standard way for AI agents to interact with external tools and data sources. Every major AI coding assistant supports it, and so do all general purpose agents.


The DeepSource MCP server gives these agents structured access to everything DeepSource knows about your code: analysis findings, PR report card grades, dependency vulnerabilities with reachability data, code coverage metrics, and compliance reports. 30 tools across 8 categories, authenticated via OAuth with no manual token setup.


This is different from simply giving an agent a CLI. MCP is a native protocol, so the agent discovers available tools automatically, calls them with structured parameters, and gets typed responses it can reason over.


## Autonomous PR review loop


Your agent opens a pull request. DeepSource reviews it — security issues, anti-patterns, complexity, coverage gaps. Through MCP, the agent reads the findings directly, understands what needs fixing, applies the changes, and pushes again. No human in the middle relaying feedback. The PR goes green on its own.


## Vulnerability remediation loop


Your agent queries DeepSource for dependency vulnerabilities and gets back more than CVE identifiers. It gets reachability analysis — which vulnerabilities actually affect your code paths, CVSS and EPSS scores, and available fix versions. It upgrades the packages that matter, skips the noise, and creates PRs with targeted fixes.


## Tools


We've shipped 30 tools across the following categories:


- **Code issues:** Query analysis findings from any PR or scan, look up rules by shortcode, get full source context for specific occurrences
- **Pull requests:** Get DeepSource's review results, issues raised and resolved per PR
- **Metrics:** Track line coverage, branch coverage, and other quality metrics with historical trends
- **OSS vulnerabilities:** List dependency vulnerabilities with CVSS scores, EPSS exploit probability, fix versions, and reachability status
- **Reports:** Access OWASP Top 10, SANS Top 25, and other compliance reports
- **Configuration:** Manage issue suppression rules programmatically


See the full[tools reference](https://deepsource.com/docs/developers/mcp/tools) for parameters and response formats.


## Get started


The MCP server is available to all DeepSource users on the Team and Enterprise plans.


```text
npx   add-mcp   https://mcp.deepsource.com/mcp


```


Read the[docs](https://deepsource.com/docs/developers/mcp) for client-specific setup, or jump straight to the[tools reference](https://deepsource.com/docs/developers/mcp/tools) .
