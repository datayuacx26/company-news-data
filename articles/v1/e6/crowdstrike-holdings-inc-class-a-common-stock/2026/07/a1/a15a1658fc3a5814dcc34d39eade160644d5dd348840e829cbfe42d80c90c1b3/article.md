---
schema_version: "1.0.0"
document_id: "a15a1658fc3a5814dcc34d39eade160644d5dd348840e829cbfe42d80c90c1b3"
company_key: "crowdstrike-holdings-inc-class-a-common-stock"
company: "CrowdStrike Holdings Inc."
source_id: "crowdstrike-holdings-inc-class-a-common-stock-rss-29758b507457"
canonical_url: "https://www.crowdstrike.com/en-us/blog/falcon-aidr-protects-copilot-studio-agents-and-claude-code/"
published_at: null
first_seen_at: "2026-07-31T22:59:00.935125+00:00"
fetched_at: "2026-07-31T22:59:02.625755+00:00"
content_hash: "sha256:cbe01f887db5513de07402c19e51fdff385c92e15a5eb4b8c6c16fa71dcf8954"
---

# Falcon AIDR Now Protects Copilot Studio Agents and Claude Code

Employees are already using AI at work. They build agents in Microsoft Copilot Studio, write code with Claude Code, and paste sensitive data into chatbots in the browser. Each of these actions can expose sensitive information outside of approved workflows, and most of it happens where traditional endpoint, network, and data loss prevention (DLP) security controls can't see the prompt or the tool call.


CrowdStrike Falcon® AI Detection and Response (AIDR) today is extending its AI visibility, detection, and response capabilities to Microsoft Copilot Studio and Claude Code.


## Microsoft Copilot Studio: Stop Risky Tool Calls Before They Run


Copilot Studio lets teams build custom agents that reach into enterprise tools and data. That reach is a feature, but it's also an exposure: An agent can be prompted to call a tool that hands back something it shouldn't.


Falcon AIDR plugs into Copilot Studio as an external threat detection provider. Before an agent runs a tool, Falcon AIDR checks the tool name and its input parameters against the organization’s policy and returns an allow or block decision inside Copilot Studio's response window.


The payoff is a decision point inside the agent's workflow. If a manipulated conversation pushes an agent toward a tool the business policy forbids, Falcon AIDR blocks it before it executes. Checks are recorded on the Falcon AIDR Findings page, ready to correlate with the rest of the organization’s telemetry in CrowdStrike Falcon® Next-Gen SIEM.


**See it in action:**


## Claude Code: Govern AI-assisted Development Without Interfering


Developers use Claude Code because it can interact with files, shell commands, and the network. Falcon AIDR now connects to Claude Code’s own hook event system to check, and block, prompts and tool activity as they happen. Setup is as simple as adding a block of JSON in the Claude Code settings file; there's no agent to install and nothing to add to the build, and the Falcon AIDR hook coexists with any hooks a team already runs.


Using this capability, developers keep their workflow, and security gains real visibility and control. A prompt carrying a secret or personally identifiable information (PII) gets caught before Claude ever sees it, a dangerous tool call gets stopped before it runs, and events can be tied back to a specific Claude Code session and user in Falcon Next-Gen SIEM.


**Watch this short demo:**


## Falcon Browser Extension Gains AIDR Capability


Falcon AIDR support is now available in the Falcon browser extension, giving organizations monitoring and control for browser-based AI activity and creating a cleaner path to AI visibility and protection with a unified browser extension. Security teams can manage coverage through the Falcon console, align Falcon AIDR policy assignment with host groups they already use, and reduce the need for separate browser extension workflows, all resulting in faster coverage with less operational lift.


Because activity is tied back to Falcon sensor-provided endpoint data, AI detections in the browser include host and user context. With Falcon Next-Gen SIEM, teams can correlate AIDR findings with endpoint detection and response (EDR), identity, and network telemetry from the same machine, gaining a more complete view of workforce AI activity and the risk around it.


**See how it works:**


## Secure AI Wherever It Runs


AI is acting with businesses’ data right now, and they need to see and shape this activity the moment it happens. These three feature releases further enhance visibility and control by feeding straight into the agentic SOC through Falcon Next-Gen SIEM.


Start by watching and reporting to learn how teams actually use AI. Turn on blocking and data transformation once the risk is identified. The same policy model reaches across the rest of Falcon AIDR, from workforce and agent protection to AI applications and the cloud. Wherever teams put AI to work next, they can bring visibility and control with them.


**[Schedule a demo to see Falcon AIDR in action](https://www.crowdstrike.com/en-us/platform/falcon-aidr-ai-detection-and-response/demo/) .**


#### Additional Resources


- *Visit the[Falcon AI Detection and Response product page](https://www.crowdstrike.com/en-us/platform/falcon-aidr-ai-detection-and-response/)*
- *Learn more about AIDR and CrowdStrike's vision for securing the agentic enterprise in this video on demand:[AIDR: Defining the Next Era of Cybersecurity](https://www.crowdstrike.com/en-us/resources/crowdcasts/aidr-defining-the-next-era-of-cybersecurity/)*
- *[Join us](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/?utm_medium=evt&utm_source=blog&utm_campaign=fal-con-26&utm_term=crwdblogs&utm_language=en-us) at Fal.Con 2026 as we bring together cyber leaders from across the industry to help secure the AI revolution.*
