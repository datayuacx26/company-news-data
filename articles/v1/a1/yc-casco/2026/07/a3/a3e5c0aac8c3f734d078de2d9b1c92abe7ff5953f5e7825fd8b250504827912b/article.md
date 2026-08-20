---
schema_version: "1.0.0"
document_id: "a3e5c0aac8c3f734d078de2d9b1c92abe7ff5953f5e7825fd8b250504827912b"
company_key: "yc-casco"
company: "Casco"
source_id: "yc-casco-news-import-3e4503ef629e"
canonical_url: "https://casco.com/blog/introducing-the-casco-mcp-server"
published_at: "2026-07-27T16:00:00+00:00"
first_seen_at: "2026-07-27T18:15:34.626919+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:d6e8582d1d23eeacf0ed567fb1529c26853abe60e01740268b543725bfe11221"
---

# Announcing the Casco MCP

# Announcing the Casco MCP


Written by


[Rene Brandel](https://linkedin.com/in/renebrandel)


on


Mon Jul 27 2026


The Casco MCP server connects Casco's autonomous security testing findings directly to AI assistants and coding agents like Claude Code, Codex, and Cursor. Add[https://mcp.casco.com](https://mcp.casco.com/) to your client, sign in with OAuth, and your agent can pull validated vulnerability findings, assess impact, and draft fixes without a single copy/paste.


Now that the announcement is out of the way, here's why we built it.


## The copy/paste tax on security work


Security findings don't live where the real work happens. They live in a dashboard, a ticket, or a PDF. The people who fix them live in a terminal, an IDE, or a chat window with their AI assistant. The gap between those two places is filled with a lot of copy/paste.


Developers feel it every time a finding lands in their queue. As a Redditor on[r/devops](https://www.reddit.com/r/devops/comments/1r4xpz9/security_findings_come_in_jira_tickets_with_zero/) put it plainly:


> “I'm supposed to drop whatever sprint work I'm on, research the CVE, find where we use that package, assess actual risk, test the upgrade, and hope nothing breaks.”
>
>
> — u/Bitter-Ebb-8932


Security engineers feel it too. They combine data from multiple systems to perform impact analysis, and the combining is manual. Someone on[r/devsecops](https://www.reddit.com/r/devsecops/comments/1ele47c/centralized_management_of_security_tool_findings/) described the workaround everyone eventually builds:


> “I ended up writing a lot of glue code and data munging scripts that were built for my exact use case.”
>
>
> — u/exploding_nun


And the tooling that promises to close the gap often makes it worse. From a[thread on SAST tools](https://www.reddit.com/r/devsecops/comments/1j8zyoa/whats_your_favorite_sast_tools/) :


> “Devs don't want security plugins. They show all vulnerabilities instead of a contextualized view for devs.”
>
>
> — u/ConsistentComment919


Three different people from three different roles were dealing with the same problem. Findings sit in one system. Context sits in another. Humans have to be the integration layer.


Meanwhile, most developers now have their own agentic coding workflows. They don't want another dashboard. They want their findings piped into the agent that can actually fix the issue, with enough context to fix it well.


## What is MCP?


MCP (Model Context Protocol) is an open standard that lets AI assistants and coding agents connect to external tools and data sources. Instead of pasting information into a chat window, your agent queries the source directly. Claude, Codex, and Cursor all speak it.


An MCP server is the other end of that connection. The Casco MCP server exposes your Casco security findings to any MCP-compatible client, so your agent of choice can work with real, validated vulnerability data. Access is OAuth authenticated, read-only, and scoped to your organization.


We've been working with MCP security since the protocol's early days. We even[broke it a few times](https://casco.com/blog/mcp-tool-poisoning) to understand where it's fragile. Now we're shipping our own server.


See the[Casco MCP feature page](https://casco.com/features/mcp) for the full product walkthrough and supported workflows.


## So what can you do with the Casco MCP server?


**Remediate in your own workflow.** Pipe Casco findings straight into your coding agent. The agent gets the vulnerability details, the affected endpoint, and the evidence, then proposes a fix that matches how your codebase actually works. Your remediation speed matches your agentic coding speed.


**Run impact analysis without leaving your assistant.** Ask your agent what a finding means for your systems. It pulls the finding from Casco, combines it with whatever other context you've connected, and gives you an answer. No tab-switching, no glue code, no data munging scripts built for your exact use case.


**Draft reports automatically.** Need a summary of open findings for leadership, or a remediation report for a customer? Ask your agent. It reads the findings directly and drafts the report. You review instead of assemble.


## How to connect


The Casco MCP server is a hosted remote server. There's nothing to run locally. You point your client at one URL:


```text
https://mcp.casco.com
```


Setup is relatively quick. Here's how.


### Claude Code


Install Casco once at user scope so the connection is available across all your Claude Code projects:


```text
claude   mcp   add   --transport   http   --scope   user   casco   https://mcp.casco.com
```


Then run` claude` , enter` /mcp` , select` casco` , and complete the browser authentication flow. You'll see the Casco tools listed once the connection shows as ready.


### Codex


Add the hosted Casco server with one command:


```text
codex   mcp   add   casco   --url   https://mcp.casco.com
```


Complete the OAuth sign-in in your browser and choose your organization. Open Codex and use` /mcp` to confirm Casco is ready.


### Cursor


Register Casco as a global remote server. Save this in` ~/.cursor/mcp.json` :


```text
{
"mcpServers"  : {
"casco"  : {
"url"  :   "https://mcp.casco.com"
}
}
}
```


Then find` casco` in Cursor's MCP settings, enable it, and complete the OAuth flow in your browser.


### Claude (claude.ai and Claude Desktop)


You can also connect Casco to Claude in the browser or desktop app:


1. Go to **Settings → Connectors** .
2. Click **Add custom connector** .
3. Name it Casco and paste` https://mcp.casco.com` as the remote MCP server URL.
4. Click **Add** and complete the OAuth sign-in.


## Try this first


Once connected, start with:


> Use Casco to list my applications.


From there, ask your agent to pull the details on a specific finding, assess which applications a vulnerability affects, or draft a remediation summary for your next security review.


## The bigger picture


Our mission at Casco is to make all software effortlessly secure.[Autonomous security testing finds the vulnerabilities](https://casco.com/blog/building-self-securing-software) . The MCP server closes the distance between finding and fix. When your security findings live inside the same agent that writes your code, the window between discovery and remediation collapses.


No more glue code. No more copy/paste. No more being the integration layer.


Get year-round security with autonomous security testing. Get started at[https://casco.com](https://casco.com/) .


*A shoutout to the[Manufact](https://manufact.com/) team. If you're looking to build an MCP server of your own, we highly recommend them.*


Quick answers


## Frequently asked questions


What to know before connecting Casco to your coding agent.


###


###


###


###


###


###
