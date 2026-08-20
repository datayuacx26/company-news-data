---
schema_version: "1.0.0"
document_id: "222b57d6c0a7ef1efab3216d9e0a786e7532a5db774e1ecbf9e55d1c4bb287e8"
company_key: "roblox-corporation-class-a-common-stock"
company: "Roblox Corporation"
source_id: "roblox-corporation-class-a-common-stock-news-import-13cc9c892e31"
canonical_url: "https://about.roblox.com/newsroom/2026/07/roblox-unveils-security-research-tools-black-hat-bsides-las-vegas"
published_at: "2026-07-31T12:00:00+00:00"
first_seen_at: "2026-07-31T22:38:45.244022+00:00"
fetched_at: "2026-07-31T22:38:45.780295+00:00"
content_hash: "sha256:37c233c7980e9e28c3e562522b1aaf7813063b9baadaac6af04968d4fbab97ee"
---

# Roblox Unveils New Security Research and Tools at Black Hat and BSides Las Vegas

Share


[Engineering](https://about.roblox.com/newsroom?filter=engineering)


# Roblox Unveils New Security Research and Tools at Black Hat and BSides Las Vegas


A look at the proactive research, agentic AI defenses, and open-source tools helping to secure Roblox


By


Nicole Grinstead, Chief Information Security Officer, and Hao Zhang


Published


Jul 31, 2026


Next week, members of the Roblox InfoSec team will gather with some of the world’s leading researchers in Las Vegas for[BSidesLV](https://bsideslv.org/) ,[Black Hat](https://blackhat.com/us-26/?_mc=sem_bhus_sem_bhus_le_tspr_corebrd_gabrdsearch_2026&utm_medium=cpc&utm_source=google&utm_campaign=bh-usa%7Cdelprod2026%7Cbottomfunnel%7Cdel%7Cnam%7C&utm_term=&gad_source=1&gad_campaignid=23689321641&gbraid=0AAAAApkI8zT1nJub-K_VlIGLQak0sqGLr&gclid=CjwKCAjwmozTBhAeEiwAkEGZzm9ZFYxv54YA7d8Rd1plsO9fwHsOWWDabGpyHO7jz8srJY9T7eeGwxoCW3QQAvD_BwE) , and[DEF CON](https://defcon.org/) , also known as Hacker Summer Camp. Our InfoSec team will share a glimpse into new research around AI agents pushing credentials to public repositories, building our own agentic defenses and open-source scanning tools, privacy and cloud governance, and more.


Roblox manages a platform where millions of creators build games and businesses, 123 million daily active users connect around the world,1 and billions of dollars move across the ecosystem. We maintain a resilient platform through a proactive, adaptable, and scalable approach to cybersecurity. Building and implementing new technology to help empower creators and improve safety requires continuous innovation, deep technical research, and a forward-looking security posture.


Check out all the talks below and join us at our networking event. Our InfoSec research and engineering leaders are eager to share insights and open-source tools with the broader security community.


### BSidesLV: Spanning the Eras: Egress Domain Governance from On-Premises to Agentic Sandboxes


Tuesday, Aug. 4 | 5:00 p.m.–5:30 p.m. (Room: Firenze)


Security teams often detect suspicious egress domains or raw IP addresses but cannot determine which internal service generated the traffic. In modern hybrid-cloud environments, egress traffic may originate from many ephemeral containers across the same or different hosts. Hosts are also online and offline, which makes service attribution extremely difficult. In this talk, we’ll introduce a practical automation system that combines service attribution and principled governance workflow to enable safe and efficient egress security controls in modern hybrid cloud environments.


**Speaker:** Biao Gao, Senior Security Software Engineer, Roblox


[Session details](https://hackertracker.app/bsideslv2026/content/67039) .


### **BSidesLV: Minutes from Malice: Detect Cloud Exposures in Minutes**


Date: Wednesday, Aug. 5 | 11:00 a.m.–11:30 a.m. (Room: Firenze)


Cloud infrastructure changes continuously, making cloud network exposure dynamic and challenging. Traditional scanners aren’t enough when resources can receive temporary IPs. This talk presents an open-source framework using cloud inventory APIs for near real-time exposure monitoring across hybrid cloud and on-prem networks at any scale.


**Speaker:** Qiancheng (Mark) Wu, Senior Software Engineer, Roblox


[Session details](https://hackertracker.app/bsideslv2026/content/66961) .


### **Black Hat: From Prompts to Pipelines: Building Agentic Detection Engineering and Threat Hunting**


Wednesday, Aug. 5 | 11:05 a.m.–11:45 a.m. (Room: Oceanside B, Level 2)


Detection engineering and threat hunting remain bottlenecked by the gap between threat intelligence and deployed defenses. The team is presenting two agentic AI frameworks built for these problems: (1) AI Detection Engineer, a multi-agent pipeline that decomposes the detection authoring workflow into five specialized stages with trust boundaries and network isolation: research, gap analysis, rule engineering, adversarial review, and live runtime validation; and (2) Threat Hunter Graph, a LangGraph-based state machine that autonomously plans, executes, pivots, and judges threat hunts against live SIEM data.


**Speakers:**


- Shoufu Luo, Principal Security Software Engineer, Roblox
- Zhenda Hu, Software Engineer, Roblox


[Session details](https://blackhat.com/us-26/briefings/schedule/#from-prompts-to-pipelines-building-agentic-detection-engineering-and-threat-hunting-52947) .


### **Black Hat: Privacy at Scale: Roblox’s Infrastructure for Honoring User Privacy Rights**


Wednesday, Aug. 5 | 2:35 p.m.–3:15 p.m. (Room: Mandalay Bay H, Level 2)


Modern online platforms operate complex distributed systems that store user data across hundreds of services and datastores. Honoring user privacy rights requires infrastructure capable of orchestrating data access and erasure requests across heterogeneous storages and service layers. We’ll share operational challenges we’ve encountered in distributed privacy enforcement, our system architecture, and several key supporting components, including a centralized metadata catalog and an orchestration layer that coordinates request execution.


**Speakers:**


- Hao Zhang, Engineering Manager, Roblox
- Yiwen Luo, Principal Privacy Software Engineer, Roblox
- Minkyong Kim, Director of Engineering, Roblox
- Nicole Grinstead, Chief Information Security Officer, Roblox


[Session details](https://blackhat.com/us-26/briefings/schedule/#privacy-at-scale-robloxs-infrastructure-for-honoring-user-privacy-rights-53294) .


### **Black Hat:** **Caging the Agent: How Roblox Built Multi-Layer Sandboxes to Secure Claude Code at Enterprise Scale**


Thursday, Aug. 6 | 3:35 p.m.–4:15 p.m. (Room: South Seas A&B, Level 3)


A hidden instruction in a GitHub Issue convinced Claude Code to upload Roblox’s credentials to a public repository. EDR saw nothing—it was a normal process making a normal network request. The good news: It happened in an internal testing environment. We’ll share how and why this happens, the kill chain, the architecture that contains it, and three problems that sandboxing alone cannot solve.


**Speakers:**


- Harshit Kumar, Principal Security Software Engineer, Roblox
- Jaskaran Singh, Principal Security Software Engineer, Roblox
- Ahmad Alomari, Senior Manager, Application Security, Roblox


[Session details](https://blackhat.com/us-26/briefings/schedule/?#caging-the-agent-how-roblox-built-multi-layer-sandboxes-to-secure-claude-code-at-enterprise-scale-53708) .


## Meet Us at the Infosec Networking Mixer, Hosted by Roblox


Thursday, Aug. 6, 2024 | 7:00 p.m.–9:00 p.m.


*Exact location to be provided in RSVP confirmation email.*


Join us for an evening of networking and tech talks! We’ll explore the frontlines of modern cybersecurity—from AI-driven developer tools to our security strategies for a platform operating over 400 AI models and processing 1.5 million inferences per second across on-premise and public cloud GPUs. Follow the link below to request an invitation.


[Event Details](https://robloxinfosecvegas2024.splashthat.com/) .


To learn more about our approach to cybersecurity at Roblox, visit our[Security page](https://about.roblox.com/security) .


1 *As of Q2, 2026.*
