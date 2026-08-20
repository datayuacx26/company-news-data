---
schema_version: "1.0.0"
document_id: "56c7cf2abb476f75448b5478065693c4477c601fe277f4adcdd58fd3297e84cc"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/codex-plugin"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:4f26a58d29c8b9e3158493e19eea4eb1c6cbcf24e5eff39979ac67fd8d9bc11f"
---

# Resend Plugin for Codex

The best agent experience requires context provided by tooling like MCP servers, skills, and CLIs. Historically, getting the most out of Codex with Resend meant installing the MCP server and every skill separately.


Today, we're excited to announce the[official Resend plugin for Codex and ChatGPT](https://chatgpt.com/plugins/plugin_asdk_app_6a3c407853888191beddc2151c2b6f8b?open_in_codex) .


The plugin bundles the[MCP server](https://resend.com/docs/mcp-server) and every[Resend Skill](https://resend.com/docs/resend-skill) into a single install.


## What you can do with it


- **Send from a conversation** : send a transactional email without leaving Codex.
- **Debug deliverability** : pull up logs, inspect a bounce, or check why an email failed.
- **Manage Contacts and Segments** : add contacts, build segments, and clean up email lists in plain language.
- **Run Broadcasts** : draft, schedule, and send a broadcast to an audience.
- **Check Domains and API Keys** : verify domain status, review DNS records, and rotate API keys.


## How to use it


To set up the Resend connector:


1. Go to **Plugins** and search for Resend.
2. Click **Connect** .
3. Sign in with your Resend account and grant the permissions Codex requests.


It offers the same[MCP experience](https://resend.com/changelog/remote-mcp-server) but with more security, better management, and improved DX.


## Manage connector access


You can view or revoke its OAuth permissions anytime from your Resend account's Team settings.


## Email expertise, on by default


Once installed, Codex activates the right skill automatically:


- **` resend`** for the SDK and platform APIs
- **` react-email`** for building templates as React components
- **` email-best-practices`** for SPF, DKIM, DMARC, compliance, and deliverability
- **` agent-email-inbox`** for safely processing inbound email with an agent
- **` resend-cli`** for shell and CI workflows


The MCP server (or[Resend CLI](https://resend.com/cli) ) lets Codex perform actions like sending emails, managing templates, and processing inbound email. Skills ensure the tooling follows best practices in a token-efficient way.


## Conclusion


We're excited to see how this connection makes it easier to build with Resend and Codex. Head to the[Codex page](https://resend.com/codex) for everything Resend brings to your agent.


If you have any questions, please reach out to us and we'll be happy to help.
