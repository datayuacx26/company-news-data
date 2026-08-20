---
schema_version: "1.0.0"
document_id: "aa3caff3f6c29442774e0165bd49313fdfd8093d65096755258bd8c8042e5bd4"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-news-import-4d91977bfa54"
canonical_url: "https://www.gendigital.com/blog/news/company-news/sage-ai-agent-security"
published_at: "2026-02-19T08:33:58+00:00"
first_seen_at: "2026-07-23T22:48:21.730721+00:00"
fetched_at: "2026-07-28T22:19:38.682305+00:00"
content_hash: "sha256:a82e2b5794a3e0299b8232b950877df78c2148dd0054453e176c93e48915a45e"
---

# Introducing Sage: Safety for Agents

By[Threat Research Team](https://www.gendigital.com/blog/authors/threat-research-team)


AI agents are starting to do the parts of computing that used to be “hands on keyboard”. They run terminal commands, edit files, install packages, and fetch things from the web, because that’s what it takes to get real work done. Tools like Claude Code, Cursor, and OpenClaw give these agents direct access to your machine and your sensitive information. That's what makes them useful. It's also what makes them risky. **These agents make mistakes** . They install packages that don't exist because they hallucinated the name. They pick up credentials and pass them where they shouldn't. They'll download and run programs from the internet without a second thought if the task seems to call for it. All of this happens fast, with confidence, and without asking. If you’ve watched an agent confidently suggest *curl … | bash* or “just add your API key here” into a file it shouldn’t touch, you’ve seen the problem.


**Agents are also targets** . Malicious URLs, poisoned dependencies, and compromised plugins can turn an agent into an attack vector, one that already has access to your files, your terminal, and your network. This is already happening.[We recently identified](https://x.com/GenThreatLabs/status/2018323584834318419) around 400 malicious skills on ClawHub, a public registry for agent extensions. At the time of discovery, they accounted for 12% of all available skills. These weren’t obviously shady throwaways; many were weaponized versions of legitimate skills, designed to trick agents into downloading malware.


These are the same agents we're starting to trust with payments, infrastructure, and sensitive workflows. The gap between what agents can do and what keeps them in check is real, and it's growing.


Today, **Gen** , the company behind Norton, Avast, LifeLock, and AVG, **is releasing Sage to close that gap** . With Sage, the Agent Trust Hub now evaluates and governs agent behavior during execution. Not just before installation. Not just at verification. But in the exact moment an action is about to happen.


## What Sage does


Think of Sage as a lightweight safety shield for AI agents. It sits inside the agent's workflow and checks every action before it happens: shell commands, URL fetches, file writes, package installs. For each action, Sage returns a verdict:


-


Allow — no threat detected, the action proceeds


-


Ask — something looks suspicious, the user decides


-


Deny — confirmed threat, the action is blocked


The agent never notices unless something is wrong.


Sage catches both sides of the risk. It protects agents from external threats like malicious URLs, compromised packages, and dangerous downloads. And it protects users from agents themselves, when they try to run destructive commands, expose credentials, or write to sensitive system files.


Here's a concrete example. An agent automating a setup task decides to download and execute a script from the internet. No review, no confirmation. Sage recognizes the pattern and blocks it before anything runs.


## Part of a larger commitment


**Sage is not a standalone effort** . It’s part of a broader strategy to embed continuous trust into the AI skills ecosystem through the Gen Agent Trust Hub.


The Skill Scanner of the[Gen Agent Trust Hub](https://ai.gendigital.com/agent-trust-hub) **** helps determine whether an agent extension is trustworthy before it gets installed, using cloud-based verification and classification. Sage picks up where that leaves off. Once the agent is running, it enforces safety locally, on your machine, checking every action at the moment it's about to execute.


Together, they cover the full lifecycle: from the decision to install something to the moment it actually runs. More capabilities are on the way as the agentic ecosystem grows.


## Open source, on purpose


The agentic platforms themselves are designed for open integration: open-source tools, community plugins, public hook APIs. A closed security layer would be harder to adopt in a space that moves fast and depends on community integrations.


At the same time, there is no established playbook for how to secure AI agents. By open-sourcing Sage, we're proposing one and **inviting the community** to help define what safe agents should look like in practice.


**This first version is lightweight** . It's not a full antivirus product as we know it, but it addresses the problems where they are. It covers the high-risk paths we think matter most: commands, file changes, URL fetches, and package installs, including catching hallucinated or malicious dependencies before they land on your machine. Sage ships with over 200 detection rules covering command injection, persistence, credential exposure, obfuscation, and supply-chain attacks.


Sage currently supports Claude Code, Cursor, and OpenClaw, with more platforms on the way. If you find a detection that's too noisy or not strict enough, open an issue. If you want to add a rule, improve a check, or add support for another agent platform, send a pull request.


We built Sage to be extended.


## Try it


Learn more about Sage at[https://ai.gendigital.com/sage](https://ai.gendigital.com/sage) . For technical details, documentation, and source code, visit the project on[https://github.com/avast/sage.](https://github.com/avast/sage)


If you use Claude Code, Cursor, or OpenClaw, install Sage and put it in the path of real work. The fastest way to improve a safety layer is to expose it to real commands, real repos, and real mistakes. That's what we're doing here — shipping early, in the open.


More on this topic


[AI gone wild – From Research](https://www.gendigital.com/blog/insights/research/ai-gone-wild)


[OpenClaw: Handing AI the keys to your digital life – From Research](https://www.gendigital.com/blog/insights/research/openclaw-autonomy-risks)


[Artificial Mindless Intelligence: The real AI risk no one is talking about (yet) – From Research](https://www.gendigital.com/blog/insights/research/artificial-mindless-intelligence)


[Threat Research Team](https://www.gendigital.com/blog/authors/threat-research-team)


Threat Research Team


A group of elite researchers who like to stay under the radar.


Follow us for more
