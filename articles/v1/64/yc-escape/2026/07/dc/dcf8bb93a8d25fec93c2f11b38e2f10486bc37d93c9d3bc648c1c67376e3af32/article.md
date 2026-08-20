---
schema_version: "1.0.0"
document_id: "dcf8bb93a8d25fec93c2f11b38e2f10486bc37d93c9d3bc648c1c67376e3af32"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/fix-issues-in-cursor-claude-codex-or-custom-agent/"
published_at: "2026-07-16T11:37:00+00:00"
first_seen_at: "2026-07-24T11:57:14.180697+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:b2b8b8663f41f3630a85ca7116a338e66e0fb8e098413d3a4a883de35896806d"
---

# Fix issues with a prompt built for your coding agent

There's a translation step between a security finding and a fix, and it's usually done by hand.


Someone opens the issue, reads the description, checks the CVSS score, follows the link to the affected asset, works out which endpoint is actually involved, then writes all of it back out as a prompt for whatever agent they code with. The information was already there. It just wasn't in the shape the agent needed.


Escape now does that step for you.


Prompt your agent with findings directly from the Escape platform


From any issue in the platform, you can send the finding directly to Cursor, Codex, or Claude. Escape composes a prompt from what it already knows:


- Issue context
- CVSS score
- Affected asset and target URL
- Any associated CVEs
- Identified root cause
- Why the issue is security relevant
- Summary of the impact
- Evidence Highlights
- Remediation guidelines


**If your team uses a different agent, copy the prompt and paste it wherever you want.** Same content, no integration required. And since most teams standardize on one agent, you can set a default and skip the picker entirely.


## Two ways to close the loop


If you saw our[webinar on Claude in security engineering](https://escape.tech/blog/claude-in-security-engineering-from-proof-of-concept-to-production/#automated-remediation) , you've already seen the fully wired version. Escape exposes an MCP server, a public API, and a CLI, so Claude Code can pull findings directly, locate the vulnerable code, apply the fix, and trigger a retest to confirm it held. Our CEO, Tristan demoed that loop live.


That path is powerful, and it takes setup. This feature is the same idea without the wiring: one action from the issue view, no MCP server, no API token, no configuration. If you're not ready to give an agent standing access to your findings, you can still hand it a well-formed prompt.


## What it doesn't do


This produces a prompt, not a direct patch. The agent still does the work, you still review the diff, and your existing process for testing and shipping a change is unaffected. Escape isn't writing to your repository or opening pull requests on your behalf.


That's deliberate. An automated fix pipeline for security issues sounds appealing until you consider what a confidently wrong change to an auth check costs. The useful thing to automate here is the context transfer, which is tedious and mechanical. The judgment stays with your engineers.


## Why it matters more than it looks


Backlogs aren't usually full of hard problems. They're full of straightforward ones that were never quite worth the context switch. When the distance between reading a finding and attempting a fix is one click, the calculation changes for exactly those issues.


Then verify it the way you'd verify any change. If you're running continuous testing, the next assessment tells you whether it held.
