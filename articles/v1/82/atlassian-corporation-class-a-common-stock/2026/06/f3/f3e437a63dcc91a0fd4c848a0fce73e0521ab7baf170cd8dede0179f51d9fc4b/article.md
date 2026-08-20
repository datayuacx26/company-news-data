---
schema_version: "1.0.0"
document_id: "f3e437a63dcc91a0fd4c848a0fce73e0521ab7baf170cd8dede0179f51d9fc4b"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-df2ad9c38740"
canonical_url: "https://www.atlassian.com/blog/company-news/claude-agent-for-jira"
published_at: "2026-06-18T15:55:00+00:00"
first_seen_at: "2026-07-24T17:38:22.240177+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:1b7148bb3bdf5767c54bd91b2e9befd575825705b3a000e4d17414412bbff662"
---

# Introducing Claude Agent for Jira

Built on Anthropic’s Claude Managed Agents infrastructure, Claude Agent for Jira lets you assign work items directly to Claude.


Your agent’s work now sits in Jira alongside everything else: tracked in your project, visible in workflows, and connected to the goals it’s helping ship. The agent automatically processes the work item context, implements the required changes in a secure sandbox, and opens a draft pull request for your review.


## The plan-to-code bottleneck


Moving from a planned task to pull request carries repetitive overhead. A[longitudinal study across 400 engineering organizations](https://getdx.com/blog/ai-productivity-gains-are-10-percent-not-10x/) found that even with AI adoption as high as 90% for coding tasks, overall productivity gains have plateaued at 10–15%. For routine tasks, developers often spend significant time copying codebase and business context into chat interfaces, setting up local branches, and translating issue descriptions into agent-readable specs.


This integration eliminates that context switching. By connecting Jira directly to code execution, agents always have the full context needed to build accurately. Every action stays visible and traceable in the same system your whole team already uses to plan, review, and ship.


## Using coding agents in Jira


1. **Delegating tasks to Claude.** Mention Claude Agent for Jira in a comment, select Claude in the assignee dropdown menu, or create automation rules that automatically assign certain tasks to Claude. The agent reads directly from the work item, including acceptance criteria, target repository, and relevant design/engineering standards.
2. **Sandboxed execution.** Operating within Anthropic’s isolated infrastructure, Claude clones the repository, analyzes the codebase, implements the changes on an independent branch, and pushes the code.
3. **Human-in-the loop review.** The agent opens a draft pull request in your connected GitHub repository and streams real-time status updates directly back to the Jira work item. Your engineering team retains complete control over code review, verification, and final merging.


## Full context for agents, no matter where you work


Whether you want a managed cloud workflow or prefer working directly from your local environment, Jira adapts to your Claude setup with all of your context to deliver more accurate results.


- **Managed in Jira Cloud:** Use the new Claude Agent for Jira to delegate tasks straight from the Jira UI using Anthropic’s hosted infrastructure.
- **Local terminal and IDE:** You can connect our[model context protocol (MCP) server](https://support.atlassian.com/atlassian-rovo-mcp-server/docs/getting-started-with-the-atlassian-remote-mcp-server/) or download the[Teamwork Graph CLI](https://developer.atlassian.com/cloud/twg-cli/getting-started/installation/) to update work items, tag teammates for review, and surface Confluence specs and decisions, and more – directly from your coding environment.


## Availability


Claude Agent for Jira is available now on the Atlassian Marketplace for Jira Cloud customers on Standard, Premium, or Enterprise plans with Rovo enabled.


## Getting started


Install Claude Agent for Jira from the[Atlassian Marketplace,](https://marketplace.atlassian.com/apps/1150553788/claude-agent-for-jira) connect your Anthropic API key and GitHub service account, and assign your first work item. The agent reads the task, codes in a sandboxed session, and opens a draft PR. You’ll see progress directly on the work item as it works.


→ Install the agent[Claude Agent for Jira | Atlassian Marketplace](https://marketplace.atlassian.com/apps/1150553788/claude-agent-for-jira?)
→ Learn more[Set up Claude Agent for Jira | Jira Cloud | Atlassian Support](https://support.atlassian.com/jira-software-cloud/docs/set-up-claude-agent-for-jira/)
