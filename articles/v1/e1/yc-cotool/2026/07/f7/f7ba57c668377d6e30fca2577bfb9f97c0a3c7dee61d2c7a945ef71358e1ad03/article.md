---
schema_version: "1.0.0"
document_id: "f7ba57c668377d6e30fca2577bfb9f97c0a3c7dee61d2c7a945ef71358e1ad03"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-db39e53ee409"
canonical_url: "https://www.cotool.ai/blog/agents-as-software"
published_at: null
first_seen_at: "2026-07-21T15:09:13.517815+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:0220d0020286f0374ca442bbd7b46a270e5999d7fab8a6121720b235ddb8615f"
---

# Agents as Software

As intelligence becomes increasingly abundant, the role of defensive security will evolve. Highly intelligent models are available to basically everyone. The mean time-to-exploit zero-day vulnerabilities is already down from 23 days in 2025 to 20 hours in 2026.[1](https://zerodayclock.com/) With a lower barrier to entry for attackers and more novel exploits readily available, security teams will inevitably spend more time threat hunting. We have seen good gains in AI-powered code scanning and red-teaming, but with the continuing increase in surface area with new apps, infra, and new supply-chain threats, the battleground will move beyond AppSec towards defensive operations. SecOps teams will need to harness some serious intelligence.


The key question is how security teams would *use* this intelligence. They could package it into human role-scoped agents, like a Tier 1 analyst agent and a Detection Engineer agent, then have them triage alerts and write detection rules. But this division of labor is suboptimal for both humans and agents: human roles were designed for human constraints, and agents and humans have different constraints.


Rather than building an “autonomous security engineer” in a single agent loop, teams could deploy hundreds of agents responsible for detecting and responding directly in their environments. This frees agents from the constraints of personified human roles, and deploys intelligence exactly where and when it’s needed at scale. Meanwhile, humans still set the mandate for security operations. In Cotool, agents don’t have to work like humans. They can work like software.


### **Contextual Intelligence**


For this vision of agents-as-software to work in SecOps, agents need a strong contextual intelligence of your specific environment, and must be aligned with your team’s goals. Two different classes of security work emerge:


**I. Work that has a known trigger, e.g., triaging an alert.**


Cotool has been doing this from day one. An alert triggers, and something or someone needs to handle it. Agents do this well. Instead of merely resolving a false positive, they can investigate quickly, decide if it's a false positive, and even tune the detection that triggered to be less noisy in the future. Again, these agents are not constrained to only do one human job. They fit your org’s processes and do whatever you decide is needed for triage.


Keeping these agents aligned with the team is straightforward: teams define the trigger, what tools the agent can access, and provide instructions for how they should go about their work.


**II. Work that has no known trigger, e.g., continuous threat hunting.**


This is the real mystery in security: how do we know what to detect? This is much harder than (I) because there isn’t an artificial alert or signal to always listen for. This is where model intelligence and context matter most. Over the past few months, Cotool has been releasing new types of agents that better address this problem **.**


### **Detection Agents**


To solve (II), we initially built Detection Agents. These agents run on a configurable schedule, and have an objective set by the user. For a popular use case like insider threat, our users want to regularly check for strange behaviors that would be hard to reliably detect with atomic queries. The agent wakes up, explores the environment by querying around based on its stated goal, and reports back with an actionable recommendation if something needs your attention.


This works well for the scenarios you already know you want to detect, but it still asks too much of even the most expert human security practitioners. Who can confidently enumerate everything they need to detect and miss nothing? Asking engineers to describe how they think every attack or exposure might happen, then translate these to a set of Detection Agents in Cotool, is imperfect. The core mystery of what to detect remains in the hands of the human user. If agents are going to help solve this problem, they’ll need to bridge knowledge from the human team with their own contextual understanding of an environment. Once we have this context as the foundation, we can run proactive investigations autonomously.


### **Agentic Threat Hunting**


This is where we’re going. Rather than repeatedly exploring a static set of predefined objectives on a schedule, Cotool uses a dedicated top-level agent to continuously orchestrate threat hunts.


We already ingest the right context to do this well:


1. telemetry from your environment (SIEM, endpoint, auth, cloud)
2. mandate from your team (crown jewels, concerns, allowlisted patterns)
3. threat intel from the outside world (vulnerable packages, trending attacks)


This AI + human-made context layer forms a kind of threat model that’s specific to your environment. The orchestrator sits between this threat model context layer and the agent harness layer. Based on that context, it decides what agents to spawn. The orchestrator sets each spawned agent’s intent, investigation plan, tools, and execution cadence.


With this approach, Cotool is constantly evaluating your environment and proactively executing the right agents to hunt for attacks or exposures you may have otherwise missed. When an agent finds a hit, it’s completely contextualized and explainable. Results are easy to understand because the agent clearly shows its intent, plan, and what data it analyzed during this trajectory.


Human users take on more critical oversight roles: they align context and define the overall system mandate. They can also explicitly point out false positives, which automatically updates the context layer to improve the orchestrator’s assumptions in future hunts.


Unlike one-off agent runs that you could set up and trigger manually using any harness, this strategy intelligently chooses which hunts to run in your environment and operates at any scale.


Unlike atomic detection rules, agents-as-software is organic. These are agents that operate based on a deep context of your environment that comes from automatic explorations, and context from your human team. These agents ‘discover’ problems as opposed to atomic rules that only alert in very specific cases.


### **What About Humans?**


With agents doing so much security work, humans must take on an elevated role. We become the directors, the ones with the vision.


A classical relationship emerges: if agents will be the watchers of our environments, looking out for anomalies and potential problems, then humans will be the ones who watch the watchers.


Our human role is to care. It works best when teams have strong opinions about their environment and its risk profile. It comes with more power and responsibility.


Luckily for us, these high-agency types are exactly the kinds of customers we have today. They’re handling huge security operations, even when they are a small team. If you’re part of a blue team and are compelled by any of this, please[reach out](https://www.cotool.ai/request-demo) to see more!
