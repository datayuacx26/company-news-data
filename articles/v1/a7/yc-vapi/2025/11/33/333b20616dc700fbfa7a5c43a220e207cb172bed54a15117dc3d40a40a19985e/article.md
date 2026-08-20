---
schema_version: "1.0.0"
document_id: "333b20616dc700fbfa7a5c43a220e207cb172bed54a15117dc3d40a40a19985e"
company_key: "yc-vapi"
company: "Vapi"
source_id: "yc-vapi-news-import-8dd0a49247bf"
canonical_url: "https://vapi.ai/blog/introducing-squads"
published_at: "2025-11-13T00:00:00+00:00"
first_seen_at: "2026-07-24T05:51:02.715878+00:00"
fetched_at: "2026-07-28T22:25:17.571501+00:00"
content_hash: "sha256:3b4722de771ff6dd8034bf6e8352d8e3baa693046dfbfaf238fbb2c1e076efa4"
---

# Introducing Squads: Teams of Assistants - Vapi AI Blog

Today, we're launching Squads, a new way to build voice AI applications.


Individual agents can only safely handle a small number of tasks before they run out of context or start behaving inconsistently due to their non-deterministic nature.


With Squads you see and control exactly how voice AI handles complex conversations. Instead of cramming every scenario into a single prompt, you can compose specialized assistants that hand off conversations while maintaining context. You decide what each assistant handles, how they hand off, and what context to share. Together, these specialists handle complexity that would break any single assistant.


# Why we built this


Every voice AI developer is familiar with this progression. You start with a simple assistant that books appointments. It works well. Then the Product asks for payment processing. Then customer service. Then order tracking.


Your once-clean prompt becomes thousands of conflicting instructions. The assistant that used to book appointments reliably now confuses Tuesday with Thursday because somewhere deep in the prompt, you added logic for handling refunds.


The existing approaches all have problems:


Single mega-prompts become unmaintainable and unreliable as complexity grows.


Multiple phone numbers fragment the customer experience and confuse users.


External orchestration introduces latency, drops calls, and adds another failure point.


Workflow tools from other providers conceal the routing logic, making debugging nearly impossible when issues arise.


Meanwhile, companies like Fleetworks need to handle dispatch, scheduling, billing, and support within a single conversation. One prompt can't handle this elegantly or reliably. Today, Fleetworks runs 240,000 calls a day through Squads. It's remarkable to see something that started as a sketch during a product meeting now orchestrating nearly a quarter million conversations daily, handling the exact complexity that would break any single assistant.


# How Squads work


Squads enable you to build specialized assistants that excel in one area each. A greeting agent qualifies and routes the caller. A scheduling agent books appointments. A support agent handles complaints. They pass control silently and share precisely the context needed.


## Visual flow design


The Squad builder provides a canvas where you design conversation flows visually. You connect assistants with routing logic and see how calls will flow through them. Each node represents an assistant, each edge represents a potential handoff.


As you map out your flows, you'll face a design choice: how should assistants connect to each other? You could create direct paths between every specialist, but that quickly becomes a tangled web.


A cleaner approach that many teams choose: use one assistant as your traffic controller. After your greeting, route calls through this central assistant, which determines the caller's needs and directs them to the appropriate specialist. Think of it like a receptionist who knows exactly which department handles what. Instead of building a maze where every specialist needs to know about every other specialist, everything flows through this smart routing point. When specialists finish their task, they send callers back to ask, "anything else I can help with?" This keeps your Squad organized, no matter how many specialists you add.


## Context control per handoff


For each handoff between assistants, you control what context passes through:


**None** for fresh starts when switching to sensitive domains like payments


**Last N messages** when you need just enough context to continue


**All messages** when full conversation history matters


This isn't a global setting. Each handoff can have different context rules based on what makes sense for that specific transition.


## Seamless transitions


The handoff happens without audio artifacts. No transferring you now unless you explicitly add it. No dead air. No clipped words. The conversation continues naturally while control passes to the specialized assistant.


## Unified experience


One phone number or chat interface. One conversation thread. One transcript. Multiple specialized assistants work together behind the scenes. Your customers experience a single, coherent interaction, while you reap the benefits of a modular design.


## Control where it matters


With Squads, you control the routing prompts sent to LLM.


When your greeting assistant needs to decide whether to transfer a customer to support or sales, you write the exact prompt and tools that are sent to the LLM. Not a natural language condition that might work. Not a black box you can't inspect. Your prompt, your tools, your logic, your control.


This matters when debugging production issues. If routing isn't working correctly, you can fix it immediately. You're not filing a support ticket and hoping someone else debugs their proprietary system.


# Building your first Squad


Creating a Squad follows a simple process:


1. Add assistants to your canvas
2. Connect them with handoff edges
3. Configure context rules for each handoff
4. Test with real calls to see the flow
5. Deploy the same Squad for voice and SMS


The builder shows live call progress. You can see which assistant is active, which tools are being called, and how handoffs are executed. Green paths show successful transfers. Red highlights errors that need attention.
