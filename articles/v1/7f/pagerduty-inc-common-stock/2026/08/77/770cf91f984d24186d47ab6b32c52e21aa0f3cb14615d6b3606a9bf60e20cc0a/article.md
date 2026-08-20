---
schema_version: "1.0.0"
document_id: "770cf91f984d24186d47ab6b32c52e21aa0f3cb14615d6b3606a9bf60e20cc0a"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-6c10cddc543b"
canonical_url: "https://www.pagerduty.com/blog/ai/5-ways-ai-operations/"
published_at: "2026-08-11T13:00:07+00:00"
first_seen_at: "2026-08-11T14:47:56.237384+00:00"
fetched_at: "2026-08-11T14:47:57.497555+00:00"
content_hash: "sha256:6c6e7f20fb5ec11c494aee95cf3171b653ab7a11de54385f46c8e4f0a9572f20"
---

# 5 Ways IT Leaders Are Using AI to Improve Operations in 2026 by PagerDuty

As the world is racing to plug AI into nearly every part of business, especially software engineering, the stakes to maintain operational integrity have never been higher. AI-generated code and AI-agents ship faster than human SREs can prepare for, which can create costly issues down the line: incidents get harder to predict and more expensive to recover from. According to


[recent research from PagerDuty](https://www.pagerduty.com/resources/insights/learn/executives-adopting-ai-agents/) , 76% of companies that have deployed at least one AI agent believe AI-driven complexity will soon outpace the number of people their company has to manage it.


Left unmanaged, that complexity doesn’t just cause incidents. It erodes the ROI case for AI itself. Companies are pulling back AI budgets over runaway token costs, and Gartner predicts


[over 40% of agentic AI projects will be canceled](https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027) by the end of 2027.


The fix isn’t AI everywhere. It’s AI in the right places.


According to


[PagerDuty’s State of AI-First Operations report](https://www.pagerduty.com/state-of-ai-first-operations/) , 75% of companies applying AI to digital operations see greater resilience and operational maturity, but only 59% are actually doing it.


Across more than 30 sessions at PagerDuty on Tour 2026, IT leaders from Intuit, Roche, Cursor, and other organizations shared how they’re closing that gap: plugging AI into incident management, risk mitigation, and system architecture to build more resilient operations. Here are five ways they’re doing it.


##### **1. Shift left: Predict deployment risk**


Roughly


[70% of incidents trace back to changes](https://sre.google/sre-book/introduction/#:~:text=SRE%20has%20found%20that%20roughly%2070%25%20of%20outages%20are%20due%20to%20changes%20in%20a%20live%20system.) in a live system, according to Google’s SRE book. More AI-generated code moving through the pipeline at higher speed means more risk—unless you can catch that risk before it ships.


At PagerDuty on Tour 2026 in San Francisco,


**Intuit** ,


the fintech giant behind TurboTax and QuickBooks, also shared how their team is using AI to reduce risk by catching errors early. As a developer writes code, AI evaluates the change against the service it touches, its blast radius, and how many other systems it affects. The model then calculates a risk score for the new code that tells the developer whether it’s safe to ship.


**PagerDuty’s plugin for Claude Code** achieves the same effect. Using the Model Context Protocol, it scans 90 days of past incidents for warning patterns. For example, if a developer bundles too many changes into one pull request, the integration sends an alert that the change resembles a previous deployment that caused an incident before.


This way, teams can ship a high volume of code without sacrificing stability, and build the guardrails to let AI take on more of the coding work long-term.


##### **2. Understand impact: Map hidden architecture dependencies**


An incident rarely affects just one system. Most applications depend on other applications to function, so when an incident occurs, it can affect multiple connected systems, sometimes without firing any alerts.


Those connections aren’t always clearly mapped, which makes it hard for response teams to quickly understand the full impact of an incident. At PagerDuty on Tour 2026 in London, IT leaders at


**Roche** , a global pharmaceutical and diagnostics company, shared how they’re using AI to process historical and live data and reveal the connections between failing applications and the services underneath them, effectively mapping their infrastructure in seconds.


With visibility to the full blast radius of an incident, engineers can triage much faster, prioritize the right fixes, direct resources where they’re truly needed, and protect uptime.


##### **3. Reduce toil: PagerDuty’s SRE Agent automates triage and diagnostics**


SREs lose entire nights to firefighting, digging through alerts just to find one root issue. That toil doesn’t stay contained to the on-call engineer, either. According to


[PagerDuty’s 2026 State of AI-First Operations Report](https://www.pagerduty.com/blog/digital-operations/2026-state-of-ai-first-operations-report/) , more than two-thirds of organizations lose over $300,000 per hour during a major incident. Toil from those incidents can also cause massive burnout and attrition.


At every stage,


**PagerDuty’s SRE Agent** works to eliminate that toil:


- **Autonomous triage** : The agent can be triggered automatically from the moment an incident begins (in Early Access), arriving pre-armed with context drawn from past incidents and user interactions.


- **Extensible by design** : Connectors, tools, and skills let teams plug SRE Agent into their own stack (Grafana, Datadog, Confluence, GitHub, and more) and give it custom domain expertise.


- **Recommend and remediate** : SRE Agent suggests the best course of action with visible reasoning based on what’s worked before and executes the approved fix. It also generates a new runbook so the next incident resolves even faster.


- **Governed access:** Team-level permissions let organizations scope AI access by team, giving enterprises the control they need to adopt AI safely.


But building enough trust to rely on an autonomous system also requires consistent feedback, not a leap of faith.


**Cursor** , the AI-powered code editor, offers a glimpse of what that looks like in practice. At PagerDuty on Tour 2026 in San Francisco, Alexi Robbins, Head of Engineering for Agents at Cursor, described the company’s “WTF” skill, which prompts agents to log their own friction points into a central tracker automatically. Manager agents sort those logs into categories, then engineers step in to review the patterns and fix the underlying tooling.


##### That loop, agents surfacing their own weak spots and engineers feeding fixes back in, is the same governance principle behind SRE Agent’s skills and team-level permissions: Give agents real autonomy, but keep humans in the loop to build the trust that makes it safe to extend.


**** **4. Streamline incident response with proactive stakeholder communications**


In the middle of a high-stakes incident, efficient communication is crucial. Responders need to quickly get acquainted with the incident context and what actions have already been taken. But sometimes, a late-joining responder can derail critical discussion by asking, “What’s going on?”


If responders have to pause and catch these people up, it pulls focus away from actually fixing the problem, delaying time to resolution.


One cloud communications platform has solved this problem with PagerDuty’s Scribe Agent. Not only does the agent automatically transcribe the incident meeting and ingest conversation history, it also posts periodic progress updates to a dedicated incident channel, giving engineers and stakeholders the context they need the moment they join.


That way, the response team can stay heads-down on fixing the problem. By streamlining high-volume incident management workflows, teams have also dramatically reduced MTTR.


##### **5. Learn faster: Auto-generate post-incident reports**


Post-incident reviews are a must-have practice for any resilient engineering team. But while they can produce deep insights, they can also be cumbersome to create: Engineers often spend hours combing through chat threads, ticket history, and dashboards just to reconstruct what happened.


At PagerDuty on Tour 2026 in San Francisco,


**Intuit** discussed how they are speeding up the process with AI. As soon as the team resolves an incident, their AI agent scans conversations and Slack threads, then automatically drafts a root cause analysis document.


This simple workflow has saved Intuit’s teams several hours of manual work per incident. Engineers don’t have to worry about the toil of assembling the narrative. Instead, they can spend that time analyzing the “why”—the part that actually pays off in resilience.


##### **Create operational resilience one step at a time**


More AI isn’t automatically the win. Bolting agents onto your workflows without a plan just adds another layer of complexity to manage. Intuit, Roche, Cursor, and PagerDuty all show a better path: using AI to solve specific operational problems that build toward greater resilience.


Start by asking what AI does well and what your engineers still do better. Then find one high-toil or high-risk workflow, and decide how to split the work, with agents and humans each doing what they do best.


But individual agentic workflows only pay off when they connect into a larger system that can automate and learn from them. Without a way to build operational learning from past incidents, teams get stuck patching the same problems again and again.


Engineering teams can drive ROI by building a feedback loop, using post-incident data to strengthen infrastructure and prevent failures before they happen. That’s the model behind the PagerDuty platform. Using built-in agents to learn from every incident, the platform moves teams closer to fully autonomous operations. When your backend system automates the toil of repetitive firefighting, engineers get back their most valuable resource: uninterrupted time to build better, more resilient services.


**Want to see how IT leaders across the world are thinking about AI in operations?**[Get full access to every session on demand from PagerDuty on Tour 2026](https://www.pagerduty.com/resources/news-announcements/video/pagerduty-on-tour-video-content/) .
