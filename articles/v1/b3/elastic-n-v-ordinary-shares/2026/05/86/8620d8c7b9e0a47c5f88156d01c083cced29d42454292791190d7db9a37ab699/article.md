---
schema_version: "1.0.0"
document_id: "8620d8c7b9e0a47c5f88156d01c083cced29d42454292791190d7db9a37ab699"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/elastic-security-mcp-app"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T22:14:43.061733+00:00"
content_hash: "sha256:8d1ff2ceec96538cebe2a5404fae0401176ae54f31fcbaa9c808eb50fdad2ade"
---

# Elastic Security MCP App: Interactive security operations inside your AI Tools

12 May 2026 •


[David Elgut](https://www.elastic.co/security-labs/author/david-elgut)


# Elastic Security MCP App: Interactive security operations inside your AI Tools


Elastic Security is the first security vendor to ship an interactive UI in AI tools. Triage alerts, hunt threats, correlate attack chains, and open cases, all from inside your AI conversation.


9 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates) ,


[Generative AI](https://www.elastic.co/security-labs/category/generative-ai)


Every SOC analyst knows the drill: an alert fires, and the next ten minutes are spent switching between a triage dashboard, a threat hunt, a case file, and the AI tool that told you to look in the first place.


Recently, we introduced[MCP Apps for Elastic](https://www.elastic.co/search-labs/blog/mcp-apps-elastic) , built on the open MCP Apps extension to the Model Context Protocol, that lets an MCP tool return an interactive UI alongside its text response, rendered inline in Claude Desktop, Claude.ai, VS Code Copilot, Cursor, or any compatible host. This post goes deep on the[Elastic Security MCP App](https://github.com/elastic/example-mcp-app-security) , We’ll go over six interactive dashboards covering the core SOC loop, from alert triage to closed case, without leaving the conversation.


Elastic already ships AI agents inside the platform:[Attack Discovery](https://www.elastic.co/guide/en/security/current/attack-discovery.html) and[Agent Builder](https://www.elastic.co/elasticsearch/agent-builder) work natively with your security data in Kibana. But analysts and security engineers also spend time in Claude, VS Code, and Cursor, writing detection logic, researching threats, and increasingly triaging findings. The question isn't whether to use Elastic's built-in AI or external tools. It's whether the external tools can give you the same interactive, visual workflow you get in Kibana. That's what the Security MCP App solves.


Security operations are inherently visual and interactive. An analyst scans alerts grouped by host, expands a process tree, traces a parent-child chain, and drags a suspicious entity onto an investigation graph. That loop doesn't survive compression into text. The Elastic Security MCP App brings those surfaces into the AI conversation, so the answer *is* the workflow, not a summary of it.


##


Why the Elastic Security MCP App matters for the SOC


When an agent tells a SOC analyst, "There are 47 alerts on host-314, here's a summary," it hasn't done any work. It's just pointed at where the work starts. The actual work lives in the alert list, the process tree, the investigation graph, and the case file. You can't do it from a paragraph of text.


The security MCP App returns the workflow itself. The analyst prompts the agent, and the agent returns an interactive dashboard in the chat where the analyst can drill into alerts, run threat hunts, correlate attack chains, and open cases, without losing the thread of the conversation. Everything you do in the MCP App writes back to[Elasticsearch](https://elastic.co/elasticsearch) and Kibana through the same APIs the product uses. From Cases, alerts, and findings to hunt queries; you lose none of this context because it does not just live in the chat, but it is all stored in your Elastic cluster and Kibana environments, waiting to be picked back up when you are ready.


##


Six interactive dashboards


We chose six elements that map to the core SOC loop: detect, triage, hunt, correlate, respond, and test. Each one is a React UI that renders inline when the agent calls the corresponding tool:


Tool What it does Interactive UI


Alert Triage Fetch, filter, and classify security alerts Severity grouping, AI verdict cards, process tree, and network events


Attack Discovery AI-correlated attack chain analysis with on-demand generation Attack narrative cards with confidence scoring, entity risk, and MITRE mapping


Case Management Create, search, and manage investigation cases Case list with alerts, observables, comments tabs, and AI actions


Detection Rules Browse, tune, and manage detection rules Rule browser with KQL search, query validation, and noisy-rule analysis


Threat Hunt ES|QL workbench with entity investigation Query editor, clickable entities, and investigation graph


Sample Data Generate ECS security events for common attack scenarios Scenario picker with four pre-built attack chains


Each tool returns a compact text summary that the model can reason over, alongside the interactive UI the analyst acts on. The UI can also fetch fresh data behind the scenes through the MCP host bridge. The full tool model and bridge API live in the[repo's architecture doc](https://github.com/elastic/example-mcp-app-security/blob/main/docs/architecture.md) .


The app also ships with[Claude Desktop skills](https://github.com/elastic/example-mcp-app-security/tree/main/skills) ,` SKILL.md` files that teach the agent when and how to use each tool. You can download the pre-built skill zips from the[latest release](https://github.com/elastic/example-mcp-app-security/releases/latest) .


##


From alert to case


The five skills cover the core SOC loop. Each one picks up a prompt, calls a tool, and returns an interactive dashboard alongside a text summary that the model reasons over. The walkthrough below starts from scratch; if you're following along, the first step populates the cluster so the rest of the loop has data to work with.


**Generate sample data.** Starting with a fresh cluster? The Sample Data skill generates realistic[ECS](https://www.elastic.co/docs/reference/ecs) security events for four common attack scenarios: ransomware, lateral movement, credential theft, and data exfiltration. Ask the agent to generate sample data, pick a scenario, and within seconds, you have a populated alert queue to work from. Everything that follows in this walkthrough uses these events.


**Triage alerts.** Ask the agent to triage by host, rule, user, or time window. The Alert Triage skill returns a dashboard of AI verdicts above the raw alert list, with one verdict per detection rule classifying that rule's activity as benign, suspicious, or malicious, each with a confidence score and a recommended action. Click any alert to open a detailed view with a process tree, network events, related alerts, and MITRE ATT&CK tags. No tab switching between your AI tool and the alerts dashboard inside Kibana; everything happens in real-time inside the conversation.


**Hunt for threats.** Ask the agent to hunt across your indices. The Threat Hunt skill returns an[ES|QL](https://www.elastic.co/docs/explore-analyze/query-filter/languages/esql) workbench with the query pre-populated and auto-executed, with every entity in the results clickable for drill-down. The model writes a short read-out below the table: what's unusual, what's connected, and what's worth a closer look. It then offers the next pivot: go deeper into the threat hunt, or hand off to another skill. Attack Discovery is the natural next step; it gathers more context on the alerts you've triaged and the threats you've hunted, correlating them into attack chains.


**Run Attack Discovery.** The Attack Discovery skill triggers the[Attack Discovery API](https://www.elastic.co/guide/en/security/current/attack-discovery.html) and returns a ranked list of findings. Each finding is a set of related alerts stitched into one attack chain, with MITRE tactics, a risk score, a confidence label, and the impacted hosts and users surfaced up front. The agent's summary lands below the findings in the same rank order, and the conversation now holds everything needed to act: hunt queries, triage decisions, correlated chains, all staged for the next step.


**Open cases without leaving the chat.** Approve findings in bulk or ask the agent to open cases for specific alerts. The Case Management skill creates one case per approved finding (source alerts attached, and MITRE tactics inherited from the attack chain) and renders the live case list inline. Click a case for its detail view, which includes a row of AI action buttons: *Summarize case* , *Suggest next steps* , *Extract IOCs* , and *Generate timeline* . Each one drops a structured prompt back into the chat, so the agent picks up the case context without needing a reintroduction. The agent's summary sits below the case list and covers the full IR queue, including the cases just opened and earlier findings that still need one.


Every step in this walkthrough runs the same loop: a prompt comes in, the skill picks it up, and the tool returns a compact text summary for the model to reason over, alongside an interactive UI that the analyst acts on. Chain the skills together, and they compose into an end-to-end SOC flow; hunt, triage, correlate, open cases, and drive the next pivot, all with the model carrying the session context across every step. Invoke any one on its own, and it's still the full dashboard, pointed at whatever slice of your data you name. Either way, the work accumulates inside the conversation; no tab switching, no copy-paste, no hand-offs.


One more skill rounds out the app: a detection-rule browser for tuning noisy rules, filtering by rule type, and flagging high-noise detections. A follow-up post will go deep on all six dashboards: investigation graph, attack-flow canvas, and end-to-end walkthrough.


Here’s the full walkthrough of this demo.


##


How Elastic's InfoSec team uses the Security MCP App


The MCP App's value compounds when the conversation has access to more than just Elastic Security. In a real SOC workflow, a single alert often leads to questions that span multiple systems: cases in Kibana, threads in Slack, issues in Jira, and cloud infrastructure logs. Traditionally, an analyst would pivot across each of those tools manually, assembling context one tab at a time.


With the Security MCP App connected alongside MCP servers for Slack, Jira, and cloud platforms, the agent can pull the full picture into one conversation: review a case and its attached alerts, cross-reference Slack channels for related outages or planned changes, check Jira for known issues, and compile a forensic summary covering root cause, actions already taken, and outstanding tasks, all before the analyst writes a single note. Once the analysis is reviewed and approved, the agent writes the findings back: a structured comment on the Kibana case, a summary posted to the relevant Slack channel, and alerts closed with context attached.


Cloud-based alerting benefits the same way. Strange activity in a cloud environment often turns out to be a known outage or an infrastructure change already under discussion in Slack or Jira. The agent can check those sources in seconds, correlate the context, and either close the alert with an explanation or escalate it with the full picture already attached.


> The MCP App for Elastic Security bridges the gap between automated detection and manual hunting. By bringing our security data directly into a single interface within Claude Desktop, we surfaced 'silent' threats in under an hour — risks that didn't trigger standard alerts but required immediate action. It's a force multiplier for our analysts. — Mandy Andress, Chief Information Security Officer (CISO), Elastic


##


How it works


Each MCP App is a small Node.js server whose tools return both a compact text summary for the model and a React UI that the host renders inline. The server exposes two layers: model-facing tools the LLM calls (returning lightweight summaries for reasoning), and app-only tools the UI calls behind the scenes for interactivity, like expanding process trees or running ES|QL queries. Each view is a self-contained React app rendered in a sandboxed iframe. Because it's built on the open MCP App spec, the same server runs on any compatible host; see the[repo's architecture doc](https://github.com/elastic/example-mcp-app-security/blob/main/docs/architecture.md) for the full design


##


The agentic SOC, interactive


Two properties about this pattern are worth stating directly. First, the tool result is no longer the end of the work; it is the start of it: the conversation returns an interface you can act on, not a summary you have to act from. Second, this only works because Elasticsearch and Kibana already expose the security APIs. The MCP App is a thin interactive layer over the detection, investigation, and case management capabilities Elastic Security already ships.


Attack Discovery already powers the correlated findings view inside this app. Inside the stack, the same agentic pattern goes further:[Elastic Workflows](https://www.elastic.co/search-labs/blog/elastic-workflows-automation) automate the deterministic steps (enrich entities, create cases, and isolate hosts), while[Agent Builder](https://www.elastic.co/elasticsearch/agent-builder) reasons over the data and invokes those workflows as tools. The MCP App brings that same security surface into the external conversation; Workflows and Agent Builder deepen it inside the stack. Different entry points, same Elastic Security APIs underneath.


That architectural choice is deliberate. The MCP server runs on the analyst's own machine and connects directly to Elasticsearch using their API key. The LLM receives only compact summaries for reasoning, while the UI independently loads full investigation data through the same server. It adds a surface for analysts who already work in Claude, VS Code, or Cursor without introducing a dependency they have to adopt or a governance model they have to rebuild. The same role-based access controls you enforce through your Elasticsearch API keys apply to every action the app takes, which means the operational result is straightforward: analysts spend less time switching tools and more time closing cases.


##


Try the Elastic Security MCP App


The Elastic Security MCP App requires Elasticsearch 9.x with Security enabled, plus Kibana for cases, rules, and Attack Discovery. The fastest path is the one-click` .mcpb` bundle from the[latest release](https://github.com/elastic/example-mcp-app-security/releases/latest) ; double-click it in Claude Desktop, and you'll be prompted for your Elasticsearch URL and API key. Setup guides for[Cursor](https://github.com/elastic/example-mcp-app-security/blob/main/docs/setup-cursor.md) ,[VS Code](https://github.com/elastic/example-mcp-app-security/blob/main/docs/setup-vscode.md) ,[Claude Code](https://github.com/elastic/example-mcp-app-security/blob/main/docs/setup-claude-code.md) ,[Claude.ai](https://github.com/elastic/example-mcp-app-security/blob/main/docs/setup-claude-ai.md) , and building from source are in the[repo](https://github.com/elastic/example-mcp-app-security) .


Don't have an Elasticsearch cluster yet? Start a free[Elastic Cloud trial](https://cloud.elastic.co/registration) . For more on the building blocks behind the app, see the related Security Labs posts on[Elastic Workflows and Agent Builder](https://www.elastic.co/security-labs/from-alert-fatigue-to-agentic-response) ,[Agent Skills](https://www.elastic.co/security-labs/agent-skills-elastic-security) , and[Attack Discovery](https://www.elastic.co/security-labs/speeding-apt-attack-discovery-confirmation-with-attack-discovery-workflows-and-agent-builder) .


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


#### Jump to section


- [Why the Elastic Security MCP App matters for the SOC](https://www.elastic.co/security-labs/elastic-security-mcp-app#why-the-elastic-security-mcp-app-matters-for-the-soc)
- [Six interactive dashboards](https://www.elastic.co/security-labs/elastic-security-mcp-app#six-interactive-dashboards)
- [From alert to case](https://www.elastic.co/security-labs/elastic-security-mcp-app#from-alert-to-case)
- [How Elastic's InfoSec team uses the Security MCP App](https://www.elastic.co/security-labs/elastic-security-mcp-app#how-elastics-infosec-team-uses-the-security-mcp-app)
- [How it works](https://www.elastic.co/security-labs/elastic-security-mcp-app#how-it-works)
- [The agentic SOC, interactive](https://www.elastic.co/security-labs/elastic-security-mcp-app#the-agentic-soc-interactive)
- [Try the Elastic Security MCP App](https://www.elastic.co/security-labs/elastic-security-mcp-app#try-the-elastic-security-mcp-app)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Elastic%20Security%20MCP%20App:%20Interactive%20security%20operations%20inside%20your%20AI%20Tools&url=https://www.elastic.co/security-labs/elastic-security-mcp-app)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/elastic-security-mcp-app)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/elastic-security-mcp-app&title=Elastic%20Security%20MCP%20App:%20Interactive%20security%20operations%20inside%20your%20AI%20Tools)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/elastic-security-mcp-app&title=Elastic%20Security%20MCP%20App:%20Interactive%20security%20operations%20inside%20your%20AI%20Tools)
