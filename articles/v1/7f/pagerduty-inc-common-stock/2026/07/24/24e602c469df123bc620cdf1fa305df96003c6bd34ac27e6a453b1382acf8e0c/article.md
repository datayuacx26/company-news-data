---
schema_version: "1.0.0"
document_id: "24e602c469df123bc620cdf1fa305df96003c6bd34ac27e6a453b1382acf8e0c"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-6c10cddc543b"
canonical_url: "https://www.pagerduty.com/blog/ai/takeaways-pagerduty-on-tour-2026/"
published_at: "2026-07-23T13:00:08+00:00"
first_seen_at: "2026-07-25T01:13:16.087688+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:7411de68a12239c6775d50ec7d5a85d1a3a5703a3f4cfd5bf0a35cb5523379fc"
---

# 3 Things IT Leaders Are Learning About AI-First Operations: Key Takeaways From PagerDuty on Tour 2026 by PagerDuty

In December 2025, an AI coding agent at AWS suddenly decided to


[delete and rebuild an entire production environment](https://www.engadget.com/ai/13-hour-aws-outage-reportedly-caused-by-amazons-own-ai-tools-170930190.html) , causing a 13-hour service disruption and a PR headache for Amazon.


As rapid adoption of AI leads to more high-profile, revenue-impacting incidents, resilience has moved from a technical concern to a board-level financial risk.


Across more than 30 sessions at PagerDuty On Tour this year, leaders running operations at Workday, Cursor, Intuit, and Accenture kept circling the same question: How should organizations approach resilience in the agentic era?


Olympic boxer Harry Garside, speaking at PagerDuty On Tour Sydney, said it best: “If you’re not creating space after failing, you run the same pattern again.” In other words, resilience isn’t the absence of failure. It’s the deliberate practice of learning from it.


In order to adapt, the most resilient teams are now using AI and human input to codify incident data and engineering knowledge into self-improving agentic systems.


Here are three takeaways from PagerDuty On Tour 2026 on how leading organizations are learning their way toward resilience.


##### **1. You can’t vibe-code your way to resilience**


Vibe coding (the practice of letting an AI agent build and deploy code from plain language input) is becoming the default workflow for many teams. It’s efficient and often works well in production.


But code produced this way is rarely understood by a human before it ships. So what happens at 2 a.m. when something breaks, and no one knows why (or how to fix it)?


One leader at a publicly traded cloud communications company framed it nicely, saying, “you can vibe code and build something, but you can’t vibe code and troubleshoot that.” He points out that the lack of context behind quick-shipped code also makes that code slower and harder to troubleshoot. Effective incident triage and resolution require deep business and system architecture knowledge.


Similarly, Dave Hogan, Principal Architect of Service Reliability Engineering at Workday, argues that true resilience (more than faster incident resolution) comes down to having good processes and clear service ownership. These “operational foundations” ensure that whoever is working on troubleshooting has the knowledge and context they need to do it efficiently. Get those foundations wrong, he says, and you’re “asking for AI chaos.”


But Hogan also admits that human knowledge alone doesn’t scale. When operational expertise only lives in the heads of your best engineers, it creates siloes. To run modern services reliably, that knowledge has to be codified into shared operational memory the whole team can access and execute on.


PagerDuty’s SRE Agent achieves this by learning from your team’s real-world incident history: what broke, how your engineers diagnosed it, and what they did to fix it. When the next incident occurs in a vibe-coded service no one fully understands, the agent can surface that human context around how your team handled similar incidents before. It can even resolve well-understood incidents autonomously based on human-established resolution procedures—instead of


*deleting the whole production environment* .


##### **2. “Shift Left” to keep up with the growing volume of new code**


AI coding agents are making development faster than ever. But they’re also pushing more change into production than human reviewers can keep up with.


According to the Google SRE Book, roughly


**70%** of


[incidents trace back to changes in a live system](https://sre.google/sre-book/introduction/#:~:text=SRE%20has%20found%20that%20roughly%2070%25%20of%20outages%20are%20due%20to%20changes%20in%20a%20live%20system.) . More change at higher speed means more risk.


Most teams respond to this problem in one of two ways:


- **Slow down AI code production** to review everything (at the cost of AI’s efficiency gains).


- **“Rubber-stamp” the output** to preserve speed (at the cost of safety).


Neither works as a long-term solution. Leaders are resolving this problem by “shifting left”—using AI to flag operational risk signals in the developer’s environment before changes become incidents.


At PagerDuty On Tour 2026 in San Francisco, Archana Kataria from Intuit showcased her team’s new AI-powered risk score. As a developer writes code, the AI evaluates the change against the service type, the blast radius, and the number of touchpoints affected, then assigns a risk score that tells the developer whether it’s safe to ship.


PagerDuty’s Claude Code plugin does similar work, using the Model Context Protocol to scan 90 days of past incidents for patterns that signal trouble. If a junior engineer bundles too many updates into one pull request, the plugin warns the developer that the change resembles an earlier incident and could break something.


##### **3. Humans are building toward agent autonomy with self-improving systems**


Many speakers at PagerDuty On Tour 2026 expressed their visions for “autonomous IT operations”—a future where AI agents handle incidents on their own. But non-deterministic outputs make AI agents notoriously difficult to trust with human work. The unpredictable risk makes the idea of autonomous agents performing real-world engineering work seem like a pipe dream.


To help build agents that are more effective at augmenting human engineering work, organizations are designing feedback loops that automatically capture human operational knowledge and use it to train AI agents.


The result is what Richard Young, Director of Partner Solutions Architecture at Arize AI, calls “self-improving agents”—AI tools that get better over time by learning from the human work and context flowing through the system. Discussing the Arize and PagerDuty integration, Richard Young explains: “The failures that evaluations catch in production actually become the datasets that you use to fix things in development.”


Alexi Robbins, Head of Engineering for Agents at Cursor, put this concept into practice with the team’s “WTF” (Work on the Factory) skill, which lets agents log their own friction points into a tracker. Manager agents categorize the issues, then humans step in to review and fix the underlying tooling.


PagerDuty’s Scribe Agent is the first step toward self-improving systems. By capturing incident timelines, responder decisions and outcomes, and lessons from post-incident commentary, Scribe Agent helps engineers gradually build trust in the system. As agents learn from human inputs and best practices, the autonomous future gets closer.


##### **Learning to build resilience that compounds**


The operations teams leading the next decade in resilience will be the ones who turn each hit into a learning opportunity.


At PagerDuty On Tour London,


[David Williams](https://www.linkedin.com/in/xcreek/) , Senior Vice President of Product at PagerDuty, described the same pattern: Every incident generates data that makes humans and agents smarter and faster for the next one.


A truly autonomous system requires trust in the agents automating the work. The more your system learns from your team’s real workflows, the more confident you can be in letting it act on its own. PagerDuty automates that learning process—codifying every incident into operational memory so your system becomes smarter and more resilient over time.


**Want to learn more?** Watch the full session,


[“From Reactive to Preventative: The Path to Autonomous Operations,”](https://www.pagerduty.com/resources/news-announcements/video/pagerduty-on-tour-video-content/) on demand.
