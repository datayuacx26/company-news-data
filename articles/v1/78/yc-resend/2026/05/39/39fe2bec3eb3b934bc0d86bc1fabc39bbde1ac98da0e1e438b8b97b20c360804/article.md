---
schema_version: "1.0.0"
document_id: "39fe2bec3eb3b934bc0d86bc1fabc39bbde1ac98da0e1e438b8b97b20c360804"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/resend-claude-code-plugin"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:d5a8b959a938acc7a6d92c6f63bc4c324d68b4ba799f64b23c3b475c030cdb2a"
---

# Official Resend plugin for Claude Code

The best agent experience requires context provided by tooling like MCP servers, skills, and CLIs. Historically, getting the most out of Claude Code with Resend required installing the MCP server and every skill separately.


Today, we're excited to announce the[official Claude Code plugin for Resend](https://claude.com/plugins/resend) .


The plugin bundles the[MCP server](https://resend.com/docs/mcp-server) and every Resend Skill into a single install.


## How to use it


To install the plugin, open Claude Code and run the` /plugin` command. Type` resend` to search for the plugin and install it.


Alternatively, run the following command in your terminal:


```text
claude plugin   install   resend@claude-plugins-official
```


Add your Resend API key to your Claude Code environment to authenticate.


```text
{        "env"  :     {          "RESEND_API_KEY"  :     "re_xxxxxxxxx"        }     }
```


## Email expertise, on by default


Once installed, Claude Code activates the right skill automatically:


- **` resend`** for the SDK and platform APIs
- **` react-email`** for building templates as React components
- **` email-best-practices`** for SPF, DKIM, DMARC, compliance, and deliverability
- **` agent-email-inbox`** for safely processing inbound email with an agent
- **` resend-cli`** for shell and CI workflows


The MCP server (or[Resend CLI](https://resend.com/cli) ) can help Claude Code perform actions like sending emails, managing templates, and processing inbound email. And skills ensure the tooling follows best practices in a token-efficient way.


## Conclusion


We're excited to see how this connection makes it easier to build with Resend and Claude Code. See the plugin page at[claude.com/plugins/resend](https://claude.com/plugins/resend) for everything Resend brings to your agent.
