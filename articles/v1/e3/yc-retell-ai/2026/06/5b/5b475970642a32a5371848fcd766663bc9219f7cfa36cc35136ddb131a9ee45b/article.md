---
schema_version: "1.0.0"
document_id: "5b475970642a32a5371848fcd766663bc9219f7cfa36cc35136ddb131a9ee45b"
company_key: "yc-retell-ai"
company: "Retell AI"
source_id: "yc-retell-ai-news-import-48ab15cc20a2"
canonical_url: "https://www.retellai.com/blog/introducing-conductor"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:02.485527+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:c64d1c8ebb910bb5aad7a10da29f60441d48a16ce0852bbc5312f62a95fea8aa"
---

# Introducing Conductor: Retell’s Copilot for Building Voice Agents | Retell AI

Meet Conductor, Retell's copilot for building voice agents. Describe what you want in plain English and Conductor builds, reviews, tests, and improves a real voice agent inside Retell, showing you every change before it ever reaches a live call.


General-purpose copilots aren't cutting it for this. A voice agent isn't just text. It's a working system of prompts, steps, tools, and settings where a small change in one place can affect everything else. Ask a generic assistant to build one and you'll often get something that looks right but fails validation or falls apart once real calls start coming in. Conductor turns operational intent into production-ready voice agents, while most copilots stop at generating text. Or, as we say internally: stop configuring agents, start describing them.


## What makes Conductor different


Conductor is the most operationally aware copilot for building voice agents in the contact center. Three things set it apart:


- **Graph-native review.** Most copilots hand you a JSON file and leave you to figure out what actually changed. Conductor surfaces every change right where it happens, so you can review it in context, accept what you want, and reject what you don't.
- **Operationally aware.** Complex agents are tangled, so one small edit can cascade in ways you never saw coming. Conductor studies the calls that go wrong, writes test cases to reproduce them, and updates your agent before they bite.
- **A proprietary framework.** We packaged everything we learned from thousands of deployments into Conductor, so it doesn't just help you build. It hands you expert best practices and lets anyone build like one.


It's already doing this for us: Conductor has written 70% of our simulation tests and made half of our own agent edits, which is what lets our FDEs ship fast and build more.


Watch Conductor build, review, and improve an agent in 90 seconds.


Here's what that looks like in practice:


- **2000+ pieces of deployment best practices** baked in, distilled from thousands of real enterprise agents.
- **100% valid agent modifications.** Every change is validated against the platform before you ever see it.
- **2.5x faster** from idea to first production agent, with about **60% less build time.**
- **Conductor did 70% of the simulation testing and 50% of the agent edits** from internal testing


## Why we built this


Every generation of software gets a new interface. SaaS gave us dashboards: precise and controllable, but only if you already know where to click. AI chat gave us natural language: fast and flexible, but often detached from the actual state of the product. Conductor is neither of those. It's the model on top of the dashboard.


Codex and Claude Code changed how software gets built. Engineers now orchestrate and review the work instead of doing all of it by hand. We think that shift is coming to every software platform, where the UI becomes an agent that does the work on your behalf while you stay in command. Contact centers are where it matters most. A voice agent isn't a text box. It's a live operational worker with call flows, escalation rules, transfer behavior, CRM actions, and compliance guardrails. A builder for that world can't just write a prompt. It has to understand the work around the prompt.


Think of it as a self-driving layer on a manual car: automation when it helps, your hands on the wheel the moment control matters.


## From a sentence to a running agent


You don't start with a spec, you start with a sentence. Tell Conductor what the agent should do, like "an appointment scheduler for a dental office," and it builds a working single-prompt agent end to end. It writes the prompt and configures the settings that actually decide call quality: voice, model, interruption handling, post-call analysis. Because it's trained on thousands of agents already running in production, it builds with patterns that are proven, not guessed.


From there you keep talking to it in simple, plain English, one request at a time to do the following:


- Test edge cases and fix the agent
- Review recent calls and update the agent


Each request comes back as a concrete change you can review and apply. When a single prompt isn't enough, convert it into a full conversation flow in one click, or build a flow from scratch, and Conductor lays out every step, every transition, and the logic that connects them.


The suggestions get sharper when Conductor knows exactly what you mean, so you can hand it context instead of describing it. Attach a specific step and say "make this confirm the spelling." Attach a real call that went wrong and ask "why did the agent end the call here, and how do I fix it?" Attach a saved test case, or a file like an SOP or a screenshot, and Conductor works from the real thing. The rhythm that emerges is simple and fast: make a change, test it, refine, and repeat until you're happy. That loop is mostly why teams reach a production-ready agent about 2.5× faster.


## Reviewable by design: the first graph-native review interface


Conductor is more than an assistant that helps you build. It's the first graph-native review interface, with granular visibility into every part of your flow, so you accept the changes you want and reject the ones you don't. It never edits your agent on its own and it proposes a change and shows you exactly what it does, right where it happens:


- **In place, on the actual setting.** Each change appears on the step or setting it affects, with a toggle to flip between the new value and the current one, so you compare old and new side by side.
- **One at a time, never lost.** When a change touches several settings, a 1 / 3 counter walks you through them, scrolling each into view and highlighting it, even if it lives on a different part of the agent.
- **A real diff when you want it.** Open the line-by-line view to see exactly what was removed and what was added.
- **Accept, reject, undo.** Decide one at a time or all at once, and undo before you move on. If a change can't be applied, Conductor flags it so your agent never ends up half-finished.


This matters more as agents grow. A single prompt tweak is easy to eyeball but a production agent with dozens of steps, tools and settings is not, and a wrong transfer condition or a misrouted tool call is the difference between a handled call and a frustrated customer. The AI proposes the change and you decide, stay informed and in control at every point. This is what makes Conductor safe to point at a live agent in a regulated contact center.


## Operationally aware, and always improving


Those are exactly the kinds of issues Conductor is designed to catch. A small change that looks harmless can have unexpected consequences, whether that's sending callers to the wrong place, leaving them stuck without a fallback, or breaking a CRM update behind the scenes.


Conductor is operationally aware and continuously improving. Ask it "what edge cases should I worry about for this agent?" and it surfaces the situations that trip agents up, then rehearses them. Ask it to "run a simulation where the caller wants to reschedule" and it plays the scenario against your agent without making a real phone call, then shows you how it behaved.


One of the most useful parts of Conductor kicks in after your agent is already live. When a call goes wrong, you can point Conductor to that conversation right from the chat. It analyzes what happened, turns the call into a test case, suggests a fix, and reruns simulations to make sure the problem is actually resolved before anything reaches production. The same review process still applies, so you're always in control.


In a way, every bad call becomes an opportunity to make the next version better, which helps teams move faster, fix issues easier, and spend more time building.


## A proprietary agent framework


We believe anyone should be able to build voice agents. So to democratize agent building, we took everything we learned deploying thousands of voice agents across enterprise contact centers and packaged it into a proprietary agent framework, all living in Conductor.


That field experience is the difference between a tool that writes text and one that knows how voice agents actually behave on a live call. Conductor doesn't just help you build, it actually hands you those best practices at your fingertips, so it builds like an expert and teaches you to do the same, whether you're shipping your first agent or your hundredth agent.


## Enterprise-safe by default


Conductor adheres to your role-based access control (RBAC) at a granular level, which is most of what makes it safe to roll out across a whole team. It inherits the permissions of whoever is using it. If you can't view a call, neither can Conductor. If you can't edit an agent, it can't edit one for you. Its context is bounded by your access, because contact center data is sensitive, and Conductor inherits the review and approval controls already in Retell.


## Try Conductor today


Any copilot can hand you a draft. What decides whether a voice agent holds up on real calls is everything that comes after, the reviewing and testing and fixing, and that's the part Conductor takes on with you. You describe what you want, it builds it, and it shows you every change before anything goes live. When a call goes wrong, it turns that into the fix for the next version, drawing on what we learned from thousands of real deployments. That's where the name comes from.


Conductor is live now in Retell. Open any agent, click the sparkle icon, and describe what you want.


[Read the docs](https://docs.retellai.com/conductor/overview)
