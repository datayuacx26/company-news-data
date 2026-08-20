---
schema_version: "1.0.0"
document_id: "3df7766710e3560c103d4a43516e86224f13525d3b7b4f5caf4e6f80c6fe7e2a"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-6c10cddc543b"
canonical_url: "https://www.pagerduty.com/blog/ai/sre-agent-enhancements-faster-triage-greater-access-controls-deeper-system-connectivity/"
published_at: "2026-08-05T11:00:31+00:00"
first_seen_at: "2026-08-05T13:12:01.402416+00:00"
fetched_at: "2026-08-05T13:12:02.429109+00:00"
content_hash: "sha256:883ef0043c224b5f92b318dac46aaf07442594d206ae9ffcde3cd4e617211dfd"
---

# SRE Agent Enhancements: Faster Triage, Greater Access Controls, Deeper System Connectivity by Ariel Russo

This blog post is part of PagerDuty’s ongoing series on how we’re helping customers navigate their journey towards autonomous operations. Read on to learn about how recent SRE Agent Enhancements build towards this vision.


---


During an incident, everything is competing for attention at once. Responders lose time swiveling between tools, insights gathered by AI stay siloed instead of feeding into the next decision, and the pressure to move fast means learnings rarely stick. The same issues creep back in a few weeks later, and the cycle starts over.


Earlier this year, PagerDuty introduced SRE Agent as a virtual responder, one that gathers signals from across your stack to help teams triage, diagnose, and remediate, using memory from past incidents and continuous learning to improve future responses. Since then, we’ve been rolling out enhancements that make the agent faster to configure, easier to trust, and more capable the moment an incident fires. Here’s what’s new.


#### **Triage Before a Human Even Looks**


SRE Agent can now be intelligently **triggered through Escalation Policies** (EA) or **incident workflows** (GA). Configure it to jump into action the moment an incident triggers, or set criteria based on priority or severity, and the agent joins the incident pre-armed with triage data and memory of past incidents.


That means investigation and analysis can be well underway before a responder ever acknowledges the page. When you finally do open the incident, you’re not starting from zero.


#### **A Faster Way to Extend the Agent**


We also introduced a new configuration experience for agent connectors, tools, and skills.


**Connectors** (GA) plug the agent into third-party data sources like Grafana, New Relic, and Datadog through MCP or API, just enter credentials and authorize.


**Tools** (GA) let the agent retrieve logs, metrics, and traces from observability platforms like Datadog, or pull context from knowledge bases like Confluence and GitHub.


**Skills** (EA) arm the agent with custom instructions and domain expertise tailored to your environment, and teams can create them directly from Claude or PagerDuty for use in Slack or the PagerDuty web platform.


Together, these let SRE Agent deduce troubleshooting steps before a human even opens the incident.


#### **Governance Built for Enterprise Rollout**


Customers told us they wanted more control over which teams could use agents, and how that access scales across the org. **PagerDuty Advance team-level permissions** (GA) let you scope AI to specific teams, giving admins the governance layer needed to roll out agentic AI with confidence rather than guesswork.


#### **Recommendations, With the Reasoning Behind Them**


Beyond investigation and diagnosis, SRE Agent can now recommend the right course of action through **Recommended Incident Workflows** (GA). It analyzes your existing configured workflows and suggests the one that best fits the current incident, along with the reasoning behind the call.


That reasoning matters as much as the recommendation itself. Responders don’t just get told what to do, they see why the agent landed there, so they can validate the call and build trust in the system over time.


#### **Seeing It Come Together**


Here’s what that looks like end to end: an incident triggers, and SRE Agent, already assigned through the escalation policy, begins autonomous triage immediately. By the time a responder opens the incident, the agent has already gathered context, investigated likely causes, and identified a recommended workflow, complete with reasoning drawn from how similar incidents were resolved in the past. The responder reviews the recommendation, runs the workflow, and the agent confirms the fix worked and resolves the incident. It even generates a new runbook, so remediation is faster the next time a similar issue comes up.


That’s the flywheel behind Autonomous Operations: today’s incident becomes tomorrow’s prevention, powered by data at scale, intelligent automation, and a system that keeps getting smarter.


#### **Try It Yourself**


These capabilities are rolling out now, with several available as part of Early Access. Watch the[full demo](https://youtu.be/NcyxNteOOyY) to see in action the SRE Agent enhancements for faster triage, greater access controls, and deeper system connectivity.


**Want early access SRE Agent on Escalation Policies?** Sign up at


[pagerduty.com/early-access](https://pagerduty.com/early-access) .
