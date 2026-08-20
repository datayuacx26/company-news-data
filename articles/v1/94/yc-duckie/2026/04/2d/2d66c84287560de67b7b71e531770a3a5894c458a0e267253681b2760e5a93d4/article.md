---
schema_version: "1.0.0"
document_id: "2d66c84287560de67b7b71e531770a3a5894c458a0e267253681b2760e5a93d4"
company_key: "yc-duckie"
company: "Duckie"
source_id: "yc-duckie-news-import-5dbf38d576c9"
canonical_url: "https://duckie.ai/blog/ai-ticket-resolution-vs-deflection-why-90-resolution-beats-40-deflection"
published_at: "2026-04-21T00:00:00+00:00"
first_seen_at: "2026-07-24T05:20:41.598001+00:00"
fetched_at: "2026-07-28T21:56:42.162124+00:00"
content_hash: "sha256:fa63e5b15258eb73b95449733d8c59f400b257827cff08c17000da7409e51aa7"
---

# AI Ticket Resolution vs Deflection: 90% vs 40% in 2026

AI resolution closes tickets end-to-end (90%+); deflection only suggests articles (30-40% ceiling). See the 4-tier spectrum and why resolution wins in 2026.


AI ticket resolution closes tickets end-to-end without human intervention - processing refunds, resetting passwords, and updating accounts to completion. Deflection answers questions and suggests articles but hands off to humans for actual action. Most AI support tools deflect at 30-40% rates. True agentic AI resolves at 90%+. The difference isn't incremental - it's a fundamentally different category of automation.


This distinction matters because deflection-only automation doesn't solve your scaling problem. You still hire to handle escalations. Resolution means your AI fleet actually closes tickets.


## What Is AI Ticket Resolution?


AI ticket resolution means the AI closes the ticket completely - verification, action, and confirmation - without a human touching it. When a customer requests a refund, the AI doesn't just draft a response or point to a help article. It checks the purchase, verifies eligibility, processes the refund through Stripe or your payment system, and confirms completion to the customer.


Resolution requires three technical capabilities most AI support tools lack:


- **System integration** : Direct API access to backend systems (payment processors, CRMs, internal databases)
- **Tool use** : The ability to execute actions, not just generate text responses
- **Multi-step verification** : Check, verify, act, confirm, all without human approval


Companies achieving true resolution rates: Grid at 91%, Automox at 95%, Vanquish at 94%. These aren't projections. These are published metrics from production environments handling 15,000+ tickets per month.


## What Is Deflection?


Deflection means the AI answers a question, suggests a knowledge base article, or drafts a response a human must review. The ticket stays open. A human agent still needs to take the final action.


Zendesk, Intercom Fin, and most AI support vendors measure deflection rates, typically 30-40% at best. Deflection has value. It reduces the load on your team. But it doesn't close tickets. It redirects them.


The ceiling on deflection is structural. Chatbots and AI assistants can answer "How do I reset my password?" but they can't reset the password. They can explain refund eligibility but they can't process the refund. That's why deflection plateaus at 40%.


## The 4-Tier Spectrum: Deflect → Suggest → Act → Resolve


AI support exists on a spectrum. Most vendors blur the line between tiers. Here's how to tell where any tool actually sits:


**Tier 1: Deflect** : Answers questions, suggests knowledge base articles. Example: "Here's our refund policy article." Ticket stays open, customer reads article.


**Tier 2: Suggest** : Drafts responses for human review and approval. Example: "Draft response: We can process your refund. \[Agent approves and sends\]." Human agent closes ticket.


**Tier 3: Act** : Takes actions but requires human verification. AI processes refund, flags for human confirmation before finalizing. Human verifies and closes ticket.


**Tier 4: Resolve** : Takes verified actions and closes tickets end-to-end. Checks order, verifies eligibility, processes refund, confirms to customer. AI closes ticket.


Most tools marketed as "AI support agents" operate at Tier 1 or 2. Very few reach Tier 4.


To evaluate where a tool sits, ask: **"Can it process a refund from start to finish without a human approving it?"** If the answer is no, it's not resolving tickets.


## Why Deflection Plateaus at 30-40%


Deflection hits a ceiling because most support tickets require action, not just information.


A 2024 analysis of 50,000 support tickets across fintech and e-commerce showed:


- 35% required account changes (password resets, subscription updates, address changes)
- 28% required financial actions (refunds, payment troubleshooting, billing adjustments)
- 22% required data lookup and troubleshooting (order status, technical diagnostics)
- 15% were pure informational questions


Deflection-only AI handles the 15% informational category well. It partially handles the 22% lookup category (can find information, can't always act on it). It fails entirely on the 63% that requires account changes or financial actions.


That's the 30-40% ceiling. You can't deflect your way past it without the ability to take action.


## What Resolution Requires


True AI resolution requires three technical layers most vendors don't build:


### Backend System Integration


The AI must connect directly to your systems, not just your helpdesk. Payment processors (Stripe, PayPal), CRMs (Salesforce, HubSpot), internal databases, and custom APIs. Read and write access, not read-only.


Decagon and Sierra require months of professional services to build these integrations. Self-building platforms like Duckie generate integrations automatically from your existing ticket patterns.


### Tool Use and Function Calling


The AI must be able to execute functions, not just return text. This means LLM-based tool use (function calling in OpenAI's terminology, tool use in Anthropic's). When a ticket requires "process refund for order #12345," the AI calls processRefund in your payment system.


Most AI chatbots don't have tool use capabilities. They generate text. That's deflection.


### Multi-Step Verification


Resolution isn't reckless automation. It's verified automation. The AI follows the same runbooks your human agents follow:


- **Check:** Is this order refund-eligible? (check order date, return policy, refund window)
- **Verify:** Does this customer's history support the action? (fraud check, past refund rate)
- **Act:** Process the refund through the payment system
- **Confirm:** Update the ticket, notify the customer, log the transaction


Automox's Head of Support, Hannah Millar, describes it: "Duckie has achieved a 95% resolution rate, which is far beyond our expectations for AI tools. It's not just answering questions, it's resolving issues the same way our best agents would."


That's the bar for resolution: your AI follows your runbooks end-to-end.


## Real Resolution Rates From Production Systems


These aren't lab benchmarks. These are production metrics from companies handling real customer volume:


**Grid** (fintech platform, 15,000 tickets/month):


- 91% resolution rate
- Handles: KYC verification, payment disputes, account troubleshooting, refunds
- Deployment time: 2 weeks
- Matthew Kim, Product Manager: "Duckie does an amazing job at blending customizable workflows with hands-off-keyboard customer support."


**Automox** (cybersecurity SaaS, scaled 3x):


- 95% resolution rate
- Handles: Technical troubleshooting, account management, billing, password resets
- Deployment time: Under 3 weeks
- Team transformation: Support team manages AI fleet instead of handling tickets directly


**Vanquish** (trading platform, 24/7 coverage):


- 94% resolution rate
- Handles: Trading account issues, KYC, payment processing, platform troubleshooting
- Deployment time: 1 week
- Max Mastbaum, Co-Founder: "Setup was surprisingly painless... within a week we were live."


The pattern: 90%+ resolution is achievable when the AI has system access, tool use capabilities, and follows verified runbooks.


## Deflection vs Resolution: Economic Impact


The economics are fundamentally different.


**Deflection model:**


- 40% of tickets deflected (no human touches them)
- 60% escalated to human agents
- You still hire to handle the 60%
- Cost per ticket drops 40%, headcount grows at 60% of ticket growth rate


**Resolution model:**


- 90% of tickets resolved end-to-end by AI
- 10% escalated to human agents (complex edge cases, human judgment required)
- Headcount grows at 10% of ticket growth rate
- Team becomes managers of AI fleet: quality control, escalation handling, runbook refinement


Scenario: Your company goes from 10,000 tickets/month to 50,000 tickets/month over 18 months.


- **Deflection model:** Hire 15 agents to handle the 30,000 escalated tickets (down from 25 agents if no AI)
- **Resolution model:** Hire 2-3 agents to handle the 5,000 escalated tickets


Resolution changes the scaling curve. Deflection bends it. Resolution breaks it.


## When Deflection Is Enough


Deflection works if your tickets are primarily informational. Internal IT helpdesks with high FAQ volume. Simple product support where most questions are "how do I..."


But if your ticket mix includes account changes, payment actions, or troubleshooting that requires system access, deflection won't scale your team. You'll still hire.


## When Resolution Is Required


Resolution is required when:


- Ticket volume is growing faster than you can hire
- Most tickets require action, not just information
- Support costs are a meaningful percentage of revenue
- You need 24/7 coverage without night shift teams
- Your team is drowning in repetitive actions (password resets, refunds, account updates)


That describes fintech, e-commerce, SaaS, travel, telehealth, and most high-growth consumer businesses.


## How to Evaluate: Deflection or Resolution?


When a vendor says "AI support" or "AI agent," ask these five questions:


- **Can it process a refund from start to finish without human approval?** If no: deflection.
- **Does it connect to our backend systems (payment processor, CRM, database)?** If no: deflection.
- **What's your median resolution rate in production?** If <70%: deflection. If 90%+: resolution.
- **How long does deployment take?** If 6+ months: manual integration model. If 1-2 weeks: self-building.
- **Who owns the runbooks: your team or the vendor's professional services team?** If vendor: you don't control it.


Decagon, Sierra, Ada: months of professional services, vendor-owned runbooks, resolution rates vary wildly by deployment.


Duckie, self-building platforms: 1-2 weeks, your team owns runbooks, 90%+ resolution in production.


## The Path Forward: From Deflection to Resolution


If you're currently using a deflection-only tool, the path to resolution is:


- **Audit your ticket mix** : What percentage requires action vs information?
- **Identify high-volume actions** : Password resets, refunds, subscription changes, account updates
- **Map your runbooks** : How do human agents handle these today?
- **Evaluate resolution-capable platforms** : Can they integrate with your systems?
- **Pilot with one action type** : Prove resolution works before expanding


The companies hitting 90%+ resolution didn't get there overnight. They started with one action type (refunds for Grid, password resets for Vanquish), proved it worked, and expanded.


Resolution is the new bar for AI support. Deflection was the 2023 benchmark. Resolution is 2026.


## FAQ: AI Ticket Resolution vs Deflection


**What's the difference between AI deflection and AI resolution?**


Deflection means the AI answers questions or suggests articles but doesn't close the ticket. Resolution means the AI takes verified actions (processes refunds, resets passwords, updates accounts) and closes the ticket end-to-end without human intervention.


**What resolution rate should I expect from AI support?**


Deflection-only tools: 30-40%. True resolution-capable platforms in production: 90-95%. Grid achieves 91%, Automox 95%, Vanquish 94%. If a vendor won't share production resolution rates, assume they're measuring deflection.


**Can AI safely process refunds and account changes without human approval?**


Yes, when the AI follows verified runbooks. The AI checks eligibility, verifies the action against your policies, executes through your systems, and confirms. Same process your human agents follow. Automox runs 95% of tickets this way. The 5% escalated are edge cases requiring human judgment.


**Why do most AI support tools only deflect instead of resolve?**


Resolution requires three things most vendors don't build: (1) backend system integration beyond the helpdesk, (2) tool use capabilities (function calling), and (3) multi-step verification logic. Building this is hard. Deflection (answering questions with an LLM) is easy. Most vendors take the easy path.


**How long does it take to deploy AI that resolves tickets, not just deflects?**


Decagon and Sierra: 6-8 weeks with professional services. Self-building platforms like Duckie: 1-2 weeks. Grid was live in 2 weeks, Vanquish in 1 week. The difference is whether the AI builds its own runbooks from your historical tickets or whether humans have to write them manually.


**Does resolution mean my support team gets replaced?**


No. Your team transforms from ticket handlers to AI fleet managers. They handle the 5-10% of tickets that require human judgment, review escalations, refine runbooks, and control quality. Automox scaled 3x with the same team size by shifting to this model.


**What types of tickets can AI resolve end-to-end?**


In production today: refunds and payment adjustments, password and account resets, subscription changes and cancellations, order status and tracking, KYC and verification, basic technical troubleshooting, account data updates, billing inquiries. The pattern: any ticket where a human agent follows a defined runbook can be automated to resolution.


## Conclusion


Most AI support tools deflect. A few resolve. The difference is 30-40% vs 90%+. The difference is headcount growing at 60% of ticket growth vs 10%. The difference is bending the curve vs breaking it.


If your goal is to reduce your team's workload slightly, deflection works. If your goal is to scale support without scaling headcount, resolution is the only path.


The companies hitting 90%+ resolution - Grid, Automox, Vanquish - aren't using magic. They're using AI that integrates with their systems, follows their runbooks, and takes verified actions end-to-end. Deflection was the 2023 benchmark. Resolution is the new bar.


### See it in action


- [Integrations Helpdesks, CRMs, and backend systems Duckie plugs into.](https://duckie.ai/integrations)
- [Customer stories Real deployments, measurable results.](https://duckie.ai/customers)


## Frequently asked questions


AI ticket resolution means the AI closes the ticket completely - verification, action, and confirmation - without a human touching it. For a refund, the AI checks the purchase, verifies eligibility, processes the refund, and confirms completion to the customer.


Deflection answers a question or suggests a knowledge base article; the ticket stays open and a human still takes the final action. Resolution closes the ticket end-to-end: the AI executes the refund, reset, or account update itself and confirms it.


Chatbots can answer questions like 'How do I reset my password?' but they can't reset the password. They can explain refund eligibility but can't process the refund. Without system integration and tool use, deflection has a structural ceiling around 40%.


True agentic AI achieves 90%+ resolution in production. Published metrics include Grid at 91%, Automox at 95%, and Vanquish at 94%, live numbers from environments handling 15,000+ tickets per month, not projections.


The spectrum runs Deflect → Suggest → Act → Resolve. Deflect points to articles; Suggest drafts responses for humans to review; Act executes simple rule-based automations; Resolve closes tickets end-to-end with tool use, system integration, and multi-step verification.
