---
schema_version: "1.0.0"
document_id: "e01b29c4c5a5baf8fe5e84d8a4c75001912760880529f80eb05468e65f02998e"
company_key: "yc-netbeez"
company: "NetBeez"
source_id: "yc-netbeez-rss-1a1c74f0723d"
canonical_url: "https://netbeez.net/blog/netbeez-mcp-server/"
published_at: "2026-03-18T16:29:21+00:00"
first_seen_at: "2026-07-20T23:24:05.097992+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:28a8faa028bdd2226d00631d2591ca6e12f7c4e266393a7f685d627ed6599268"
---

# NetBeez Releases Its MCP Server

## NetBeez MCP Server


I am happy to announce that NetBeez has released its MCP server. The MCP server enables the integration of NetBeez data and intelligence into Large Language Models (LLM). As a result, Network Operations and Engineering teams can achieve more than they could prior to the integration release. Let’s see in more detail how.


## Benefits


There are several benefits with running an MCP: from troubleshooting network problems to building custom reports and public dashboard. One thing I realized is that, before the recent advancements with AI, some of these tasks required training, coding and network engineering expertise. Now, this is all much easier for entry level personnel. AI greatly reduces the entry barriers to network performance monitoring. Let me give you some examples …


### Network Troubleshooting


LLMs are capable of retaining a large context window when processing NetBeez data. Network Operations teams can use them to troubleshoot complex network problems. Before releasing the MCP server, a L1 help desk analyst needed to be trained on how to use the NetBeez dashboard and navigate through the different screens available. Now, they can just simply have a conversation with the LLM to get to the same resolution (at least for the most simple network issues).


## Prompt:


› use the netbeez tools and tell me why the agent francesca had sporadic incidents some hours ago


• francesca had intermittent local connectivity loss on Tuesday, March 17, 2026, not a Google-specific issue.
Between 12:32:05Z and 13:47:03Z (13:32 to 14:47 CET), NetBeez opened two ping warning alerts at the same time:
– 890617 ping to www.google.com
– 890621 ping to the local _gateway_


Both alerts said the 5-minute error rate exceeded the 3% watermark:
– Google ping: 4.2%
– Gateway ping: 4.1%


Why this points to the agent’s local network:
– The gateway and internet destination failed together.
– Raw ping results show simultaneous null samples with error_code 195 at 12:31:16Z, 12:35:25Z, and 12:38:19Z.
– Just before some drops, latency on both paths spiked sharply, which is typical of a flaky Wi-Fi/local-link condition.


Why the incidents looked sporadic:
– NetBeez did not create separate root-cause alerts each time.
– One degraded period stayed active from 12:32:05Z to 13:47:03Z, and the platform emitted several short incidents inside that window
as the rolling error rate crossed the incident threshold:
• 12:32:16Z–12:34:40Z
• 12:50:00Z–12:50:59Z
• 13:07:13Z–13:09:40Z
• 13:24:53Z–13:32:29Z


What I did not find:
– No agent connect/disconnect logs in that window.
– No evidence that HTTP, DNS, or traceroute were the trigger.


So the most defensible conclusion is: francesca’s wireless/home-network path was unstable for about 75 minutes, causing brief packet-loss bursts to both the local gateway and the internet, which NetBeez surfaced as sporadic incidents.


### Generate Reports


You can interact with the LLM and ask it to create custom reports. This is a great use case if you need to share NetBeez findings with leadership or external users that are not expected to use the NetBeez dashboard. I was able to develop a python script that generates the following dashboard without any coding skills.


### Build Dashboards


An LLM has coding abilities, making it relatively trivial building ad-hoc dashboards that Network Operation teams can share within their organization. For instance, I was able to build the following grafana dashboard in one hour or so with no experience whatsoever in Grafana. The only dependency was to install the Grafana MCP server. I used as template the offline report I previously created (see above screenshot).


## How to Get Started


Go to the GitHub[https://github.com/netbeez/nb-mcp-server](https://github.com/netbeez/nb-mcp-server) and find the command line installation script. The MCP server supports the most common LLM, such as:


- OpenAI Codex,
- Cursor,
- Claude Desktop,
- Windsurf,
- Kiro.


Once the installation is complete, you will have to define the NetBeez server’s URL and create an API key that will be used by the LLM for queries.


### Features


The NetBeez MCP server has the following features:


- 32 tools for querying and managing agents, agent groups, targets, tests, scheduled test templates, alerts, incidents, WiFi profiles, statistics, path analysis, and running ad-hoc speed/VoIP/Iperf tests (including multiagent run status)
- 3 contextual resources providing the LLM with NetBeez data model knowledge, cross-agent correlation methodology, and troubleshooting workflows
- 4 prompt templates for common workflows: troubleshoot target, analyze agent health, investigate incident, network overview
- Dual transport: stdio (for Cursor/Claude Desktop) and HTTP (for remote clients)


### Conclusion


The NetBeez MCP server marks a meaningful step forward in how teams interact with network performance data. By combining NetBeez’s deep visibility with the accessibility and intelligence of LLMs, organizations can move faster, reduce dependency on specialized expertise, and empower a broader range of users to diagnose issues and build insights. Whether it’s troubleshooting incidents, generating reports, or creating dashboards, the MCP server transforms complex workflows into simple conversations. As AI continues to evolve, this integration positions NetBeez at the forefront of a more intuitive, efficient, and scalable approach to network operations.


Get your free trial


now


Monitor your network from the user perspective


You can share


[Twitter](http://twitter.com/share?url=https://netbeez.net/blog/netbeez-mcp-server/)[Linkedin](https://www.linkedin.com/sharing/share-offsite/?url=https://netbeez.net/blog/netbeez-mcp-server/)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://netbeez.net/blog/netbeez-mcp-server/) Copy link


Link copied
