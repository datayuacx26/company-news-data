---
schema_version: "1.0.0"
document_id: "b368076f69a3bcf6db4da11ea3f4e6c574bef2cc629080b1f388aafda0fce3e7"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/ai-agent-pricing-models"
published_at: "2026-04-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:66383d391e30416bcfa4e070436ac2089ebbb5b6a21a0f58fdd00e2ac5e97975"
---

# AI Agent Pricing Models 2026: Per-Resolution vs Per-Seat Compared

**AI agent pricing in 2026 falls into three models: per-seat (a flat monthly fee per human user), per-ticket (a fee for every inbound conversation), and per-resolution (a fee only when the AI resolves a conversation end-to-end).** Published per-resolution rates from the major customer-service vendors range from $0.50 (Quickchat AI) to $2.00 (Salesforce Agentforce), a 4x gap on the same unit of value. Decagon, Sierra, and Ada do not publish their rates.


Most procurement conversations about AI agents end up in the same place. The vendor quotes a number, the buyer has nothing to compare it to, and a contract gets signed on the basis of a demo. This post lays out the three AI agent pricing models on the market in 2026, the incentives each one creates, the published prices for the major vendors, what “resolution” actually means inside a vendor contract, and a worked total cost of ownership at three deployment sizes (2,000, 10,000, and 50,000 monthly conversations).


At a glance, per-resolution is the only model where the vendor’s revenue grows when the AI gets better. The other two reward something else, usually higher volume of one kind or another on the buyer’s side. Pricing-model alignment matters more than sticker price during vendor evaluation.


## TL;DR: 2026 AI agent pricing at a glance


Pricing model What’s billed Typical 2026 rate Vendor incentive


Per-seat A flat fee per human agent licensed $30 to $80 per agent per month Hire more humans


Per-ticket Each inbound conversation, regardless of outcome $0.30 to $1.00 per inbound Higher inbound volume


Per-resolution Each conversation resolved without human handoff $0.50 to $2.00 per resolution Higher resolution rate


Quickchat AI lists Enterprise from $0.50 per resolution. Intercom Fin lists $0.99. Zendesk AI Agents charges roughly $1.50. Salesforce Agentforce launched at $2.00 per conversation. The rest of the post breaks down the math, the incentives, and the contract terms behind each rate.


## The three AI agent pricing models


### Per-seat


The buyer pays a fixed monthly fee for every human user with access to the platform. This is the model that legacy helpdesks (Intercom, Zendesk, Freshdesk, HubSpot) have extended to their AI add-ons. Typical AI seat fees sit at $30 to $80 per agent per month on top of the base helpdesk plan.


The implicit assumption is that a team’s value scales with headcount. That assumption was reasonable when the platform was a tool humans used to handle tickets. It breaks once the platform itself does most of the work. A 15-person team with an AI handling 75% of inbound conversations pays for 15 seats whether the AI resolved 100 conversations that month or 10,000.


The vendor’s incentive under per-seat is for the customer to hire more humans. That sometimes shows up as soft pressure to expand seat counts during sales cycles, sometimes as feature gating that pushes teams toward upgrade tiers tied to seat counts. It is also the reason per-seat AI add-ons tend to be conservative on AI Action depth: an AI that completes refunds, updates CRM records, and manages subscriptions is an AI that justifies fewer seats.


### Per-ticket


The buyer pays a fee for each inbound ticket, whether the AI resolves it or not. This appears in some enterprise contracts as a hybrid of helpdesk pricing and AI usage, typically billing $0.30 to $1.00 per inbound ticket regardless of outcome.


Per-ticket has an unusual property. The vendor’s revenue grows when inbound volume grows, even if more of those tickets are AI-handled. Teams that successfully reduce inbound volume through proactive outreach or product fixes see their per-ticket bills go down, but teams that improve resolution rate see no benefit. The model rewards the wrong direction of effort.


Per-ticket also disincentivizes proactive outreach (the AI sending an outbound message because of a delayed shipment, a failed payment, or a churn signal). Each outreach is an outbound message rather than a ticket, but it generates a conversation that often becomes one. Teams using per-ticket vendors usually avoid outbound use cases for budget reasons.


### Per-resolution


**Per-resolution pricing is a model where a buyer pays only when an AI agent resolves a conversation end-to-end without human handoff.** Each successful outcome triggers a fixed fee; conversations that escalate to a human, abandon, or fail to reach a resolution are not billed. The model aligns vendor revenue with the resolution rate the buyer is trying to maximise.


Quickchat AI prices Enterprise per-resolution from $0.50. Intercom Fin publishes $0.99. Zendesk AI Agents charges roughly $1.50. Salesforce Agentforce launched at $2.00 per conversation. HubSpot’s Customer Agent moved to $0.50 per resolved conversation in April 2026 (down from $1.00 per conversation). Decagon, Sierra, and Ada all use per-outcome models in some form but do not publish rates.


This is the only model where vendor revenue tracks the metric the buyer cares about. If the AI improves and resolves 80% instead of 70% of conversations, the vendor earns more and the buyer gets a higher resolution rate. The incentives line up.


The risk of per-resolution is that “resolution” is a vendor-defined term. Two vendors quoting $0.99 may bill very differently, and the difference shows up in the invoice rather than the contract. The section below covers what to ask for in writing.


## Published prices in 2026


Here is the current state of published pricing across the major customer-facing AI agent platforms. Prices captured April 2026.


Vendor Model Published price Notes


[Quickchat AI](https://quickchat.ai/ai-agents) Per-resolution from $0.50 Published Enterprise rate; self-serve tiers start at $9/mo


Intercom Fin Per-resolution $0.99 Requires Intercom helpdesk subscription on top


HubSpot Customer Agent Per-resolution $0.50 Switched from $1.00/conversation to $0.50/resolution in April 2026


Zendesk AI Agents Per-resolution ~$1.50 Per “automated resolution”; helpdesk plan separate


Salesforce Agentforce Per-conversation $2.00 Launch pricing; tied to Salesforce platform fees


Decagon Quote-only Not published Per-outcome models reported in customer disclosures


Sierra AI Quote-only Not published Outcome-based; minimums above SMB price points


Ada Quote-only Not published Per-resolution with negotiated minimums


Cognigy Quote-only Not published Voice-heavy contracts; per-minute and per-resolution variants


Three observations matter. First, the gap between the published prices ($0.50 to $2.00) is 4x. The gap between the published and the unpublished is open. Vendors that refuse to publish a rate usually do so because the rate varies materially across customers, which means the price is a function of negotiation rather than a list. Second, the vendors with the highest list prices (Salesforce, Zendesk) layer per-resolution on top of platform subscriptions that themselves cost $50 to $150 per agent per month. The fully loaded cost on those platforms ends up closer to $4 to $6 per resolution once seat fees are amortized over resolution volume. Third, low headline rates and high resolution rates are not the same thing. Teams comparing on sticker price alone should benchmark resolution rate independently against their own knowledge base before signing. A cheap resolution that resolves 40% of inbound volume costs more in practice than a $0.50 resolution that resolves 80%.


For head-to-head technical comparisons of the largest platforms see[Quickchat AI vs Intercom Fin](https://quickchat.ai/intercom-fin-ai-alternative) ,[Quickchat AI vs Salesforce Agentforce](https://quickchat.ai/agentforce-alternative) , and[Quickchat AI vs HubSpot AI Breeze](https://quickchat.ai/hubspot-ai-breeze-agents-alternative) .


## What “resolution” actually means in a vendor contract


The single most important question to ask any per-resolution vendor is what the vendor counts as a billable resolution. The variations across vendors are large enough to change the bill by 30% to 50% on the same conversation volume.


Four common definitions appear in 2026 contracts:


1. **Closed without human handoff.** The conversation ended without a human ever joining. This is the most common definition and the easiest to measure, but it counts conversations where the customer abandoned in frustration as resolutions. It also counts conversations where the customer’s question was misunderstood and the customer simply gave up.
2. **Closed without human handoff, with a positive CSAT.** The conversation ended without a human and the customer rated it as helpful or completed an outcome. This is a stricter definition and a better proxy for value, but it under-counts because most customers do not fill out CSAT surveys. Vendors that use this definition usually publish lower headline resolution rates and produce smaller bills on the same volume.
3. **Closed with a logged outcome.** The conversation ended with a specific outcome tag (refund issued, order tracked, account updated). This is the strictest definition because it requires the AI to have completed an action, not just answered a question. Vendors that use this definition tend to have stronger AI Action infrastructure.
4. **Closed without escalation within X days.** The conversation ended without escalation and the customer did not return with the same question within a defined window (usually 7 days). This catches the case where the customer accepted the AI’s answer in the moment but came back the next day with the same problem. It is the closest proxy for “the AI actually solved this,” but it requires the vendor’s billing system to wait before invoicing.


A vendor that cannot answer “which of these definitions do you use” in writing is selling on a metric they control. Three follow-up questions belong in the procurement checklist. Is the definition in the contract? Is the audit data available in real time, or only on request? Can the buyer pull a per-conversation log showing why each conversation was billed?


Platforms that expose a full reasoning trace for every conversation make this audit trivial. Quickchat AI’s per-answer reasoning trace shows the source documents, retrieval scores, applied guidelines, and called actions for every billed conversation, so a finance team can spot-check 100 invoices in an hour. Vendors without that infrastructure require auditing through customer success tickets, which is slower and prone to disputes.


## Worked total cost of ownership


Costs reshape at scale, and the three pricing models do not reshape the same way. Here is a worked TCO at three deployment sizes, holding the AI’s resolution rate at 75% (the median for well-deployed customer service agents in 2026 across consumer verticals). To rerun these numbers with your own volume and cost assumptions, use the[chatbot ROI calculator](https://quickchat.ai/chatbot-roi-calculator) .


Assumptions used in every scenario:


- Resolution rate: 75%
- Per-seat AI add-on tier: $50 per agent per month (entry tier)
- Fully loaded support agent salary: $3,000 per agent per month (US blended; varies by geo and seniority)
- Per-ticket rate: $0.50 per inbound
- Per-resolution rates: $0.50 (Quickchat AI), $0.99 (Fin), $2.00 (Agentforce)
- Helpdesk seat fees not included; they exist separately under all models
- Per-seat rows include support agent salaries because per-seat AI add-ons are licensed against existing helpdesk seats and presuppose a full support team is in place. Per-ticket and per-resolution charge for AI work alone and operate independent of team size.


### 2,000 conversations per month (small support team, ~5 agents)


Model Calculation Monthly cost


Per-seat (5 × $50 AI add-on + 5 × $3,000 support agent) $250 + $15,000 $15,250


Per-ticket 2,000 × $0.50 $1,000


Per-resolution at $0.50 (Quickchat AI) 1,500 × $0.50 $750


Per-resolution at $0.99 (Fin) 1,500 × $0.99 $1,485


Per-resolution at $2.00 (Agentforce) 1,500 × $2.00 $3,000


At small volume, per-resolution at $0.50 is the cheapest option for AI work itself. The per-seat row looks expensive because it includes the fully loaded cost of 5 support agents on top of the AI add-on, which is the honest comparison: per-seat AI add-ons cannot operate without licensed helpdesk seats. Per-resolution and per-ticket charge for AI work alone and let the buyer choose team size independently. The other consideration at this scale is feature access. The entry tier of per-seat AI add-ons usually limits actions, integrations, and analytics, which can leave the small team with an AI that cannot finish the work the customer is asking for.


### 10,000 conversations per month (mid-market, ~15 agents)


Model Calculation Monthly cost


Per-seat (15 × $50 AI add-on + 15 × $3,000 support agent) $750 + $45,000 $45,750


Per-ticket 10,000 × $0.50 $3,000


Per-resolution at $0.50 (Quickchat AI) 7,500 × $0.50 $3,750


Per-resolution at $0.99 (Fin) 7,500 × $0.99 $7,425


Per-resolution at $2.00 (Agentforce) 7,500 × $2.00 $15,000


At mid-market volume, agent salaries dominate the per-seat row at $45,000 per month and the AI add-on is essentially noise. Per-resolution at $0.50 from Quickchat AI delivers the AI work for $3,750, which is half the rate of Fin ($7,425) and a quarter of Agentforce ($15,000). Per-ticket sits between the per-resolution variants. The case for per-seat at this scale weakens further because the entry-tier feature limits push 1,000 to 2,000 of the 25% human-handled tickets into longer handle times, which costs the same team another fully loaded agent in productivity.


### 50,000 conversations per month (enterprise, ~40 agents)


Model Calculation Monthly cost


Per-seat entry tier (40 × $80 AI add-on + 40 × $3,000 support agent) $3,200 + $120,000 $123,200


Per-seat enterprise tier (40 × $250 AI add-on + 40 × $3,000 support agent) $10,000 + $120,000 $130,000


Per-ticket 50,000 × $0.50 $25,000


Per-resolution at $0.50 (Quickchat AI) 37,500 × $0.50 $18,750


Per-resolution at $0.99 (Fin) 37,500 × $0.99 $37,125


Per-resolution at $2.00 (Agentforce) 37,500 × $2.00 $75,000


At enterprise volume, support agent salaries are the dominant line in any per-seat scenario at roughly $120,000 per month. The entry-tier vs enterprise-tier AI add-on (a 3x gap on the AI line) is small relative to that base. The realistic AI-work comparison is between the per-resolution rates alone: $18,750 (Quickchat AI), $37,125 (Fin), $75,000 (Agentforce). The same AI work is billed at four times the rate at the high end.


For the enterprise pricing detail including SLA, compliance, EU data residency, and dedicated infrastructure, see the[Quickchat AI Enterprise page](https://quickchat.ai/enterprise) and the[pricing page](https://quickchat.ai/pricing) .


## Behavioral incentives by pricing model


A pricing model encodes a behavior the vendor wants from its customers. The three models encode three different behaviors.


**Per-seat encodes hiring.** The vendor earns more when more humans are on the platform. AI features that reduce headcount run against that incentive, which is why per-seat AI add-ons tend to be conservative on action depth and aggressive on seat-tier feature gating.


**Per-ticket encodes higher inbound volume.** The vendor earns more when more tickets arrive. Proactive outreach, product fixes that reduce support volume, and self-service deflection through better docs all reduce vendor revenue. Per-ticket vendors rarely build outbound features, and the ones that do tend to price them separately.


**Per-resolution encodes higher resolution rate.** The vendor earns more when the AI resolves more conversations end-to-end. Better knowledge ingestion, broader action coverage, smarter handoff logic, and lower hallucination rate all increase vendor revenue at the same time they increase buyer value. The only gotcha is the definition of “resolution” itself, which is why the audit question above matters.


A useful exercise during vendor evaluation is to ask the salesperson which behavior of the buyer’s their commercial team optimizes for. The honest answer reveals whether the model is aligned with the buyer’s interests.


## Three transparency questions to ask before signing


Three questions sit underneath the pricing model and matter as much as the model itself.


**Is the price published?** Vendors that will not put a number on a price page have variable pricing across customers. The buyer with less negotiation leverage pays more. Quickchat AI publishes $0.50. Fin publishes $0.99. Zendesk publishes their AI Agent rates. Decagon, Sierra, and Ada do not. Unpublished pricing usually requires a signed NDA before a quote, and the quote often arrives with usage minimums that price out smaller buyers entirely. Published pricing favors smaller buyers; unpublished pricing favors vendors with strong sales motions and weaker self-serve fit.


**Are there setup or implementation fees?** A $0.50-per-resolution price next to a $25,000 implementation fee is not the same as a $0.99 price with no fee. Ask for a written quote with line items rather than a single number. Implementation fees of $3,000 to $30,000 are common for vendors targeting enterprise contracts; $0 implementation fees are common for self-serve and product-led vendors. The fee level often correlates with the depth of the sales motion: heavy implementation fees usually come with heavy sales involvement, and lower fees come with self-serve access.


**What happens at renewal?** A 12-month contract at $0.50 per resolution is meaningful only if the renewal clause is also at $0.50. Vendors that bury price escalators in renewal terms (“up to 15% increase per year”) effectively offer an introductory rate. Read the renewal clause. If the published rate matches the renewal rate, the vendor is being honest. If they differ, the published rate is a lure and the real rate appears in year two.


For a related discussion of where outcome-based pricing applies and where it does not, see[Outcome-Based Pricing Models](https://quickchat.ai/post/outcome-based-pricing-models) . For the broader chatbot cost landscape across rule-based, AI-powered, and custom-built deployments, see[How Much Does a Chatbot Cost in 2026](https://quickchat.ai/post/how-much-does-chatbot-cost) . For the deployment-side detail on what changes when an AI agent is doing the work, see[AI Agent for Customer Service](https://quickchat.ai/post/ai-agent-for-customer-service) . For the platform-level evaluation framework, see the[AI Agent Platforms 2026 buyer’s guide](https://quickchat.ai/post/ai-agent-platforms) .


## Frequently asked questions


**How are AI agents being priced in 2026?** AI agents in 2026 are priced under three main models: per-seat (a flat monthly fee per human user, typical $30 to $80 per agent per month), per-ticket (a fee per inbound conversation, typical $0.30 to $1.00), and per-resolution (a fee only when the AI resolves a conversation without human handoff, typical $0.50 to $2.00). Per-resolution is the model where vendor revenue and buyer value are aligned. The other two reward higher headcount or higher inbound volume on the buyer’s side. Most leading customer-service AI vendors (Quickchat AI, Intercom Fin, Zendesk AI Agents, Salesforce Agentforce) now publish per-resolution rates.


**How much does a customer service AI agent cost?** Published per-resolution rates from the major customer-service AI vendors in 2026: Quickchat AI $0.50, Intercom Fin $0.99, Zendesk AI Agents about $1.50, Salesforce Agentforce $2.00. Decagon, Sierra, and Ada do not publish rates. For a 10,000 monthly conversation volume at a 75% resolution rate (7,500 resolutions), the AI cost lands at $3,750 (Quickchat AI), $7,425 (Fin), or $15,000 (Agentforce). Helpdesk seat fees, implementation fees, and human-agent salaries sit on top of those AI rates and are usually the larger line in any total cost of ownership.


**What is the cheapest AI agent pricing model in 2026?** On AI charges alone, per-seat looks cheapest at small volumes because seat counts are small. The fully loaded comparison is different. Per-seat AI add-ons are licensed against helpdesk seats and presuppose a full support team is in place, so the real spend includes agent salaries on top of the AI fee. Per-resolution at $0.50 to $0.99 bills only for AI work and operates regardless of team size. Above ~3,000 monthly conversations, per-resolution typically beats per-seat once helpdesk fees, implementation costs, and renewal escalators are included alongside agent salaries.


**Why do some AI agent vendors not publish their prices?** The most common reason is that prices are negotiated per customer based on volume commitments, contract length, and the buyer’s negotiation leverage. Decagon, Sierra, and Ada all use per-outcome pricing but require a sales conversation and usually a signed NDA before disclosing rates. A second reason is that the vendor sells on outcomes (revenue uplift, cost reduction) rather than usage, and outcome contracts are bespoke. Buyers should expect that unpublished pricing favors vendors with strong sales motions, since the customers paying the most for the same product are usually those without competitive alternatives in their procurement process.


**What does per-resolution actually mean?** It varies by vendor. The four most common definitions are: closed without human handoff, closed without handoff with a positive CSAT, closed with a logged outcome action, and closed without escalation within a 7-day window. Each definition produces materially different bills on the same conversation volume. Quickchat AI bills on conversations closed without human handoff and exposes a per-conversation reasoning trace that finance teams can use to audit invoices. Other vendors should be asked for their written definition before a contract is signed.


**How does Quickchat AI’s $0.50 compare to Intercom Fin and Salesforce Agentforce?** Quickchat AI prices Enterprise per-resolution from $0.50, roughly half the published rate of Intercom Fin ($0.99) and a quarter of Salesforce Agentforce ($2.00 per conversation). The pricing difference is published, applies to teams of any size, and holds across renewals. For head-to-head feature comparisons see the alternative pages linked above.


**Should small support teams pick per-seat or per-resolution pricing?** Below ~3,000 monthly conversations, per-seat is often cheaper on absolute cost because the team has few agents. Above that, per-resolution usually wins because the AI handles a growing share of work that does not scale with headcount. The other consideration is feature access: per-seat AI add-ons on legacy helpdesks usually limit actions, integrations, and analytics on the entry tier, which can leave a small team with an AI that cannot complete the actions the customer needs. A free trial on a per-resolution platform is the fastest way to compare both at the team’s actual volume.


**What enterprise pricing terms should I negotiate?** Volume-based discounts on the per-resolution rate (typical breakpoints at 50K and 100K monthly resolutions), a fixed renewal rate for the first two renewal cycles, an SLA with a meaningful penalty (10% credit for missed uptime is the floor; 20% is reasonable), EU data residency where regulated customer data is in scope, and an audit clause that gives the buyer the right to inspect billed conversations on demand. The Quickchat AI Enterprise page covers what is in scope on enterprise contracts.


AI agent pricing models are usually treated as a procurement detail, but the model a vendor uses encodes the behavior the vendor wants from the buyer over the next three years. Per-seat pricing rewards hiring. Per-ticket pricing rewards higher inbound volume. Per-resolution pricing rewards a higher resolution rate. Quickchat AI publishes Enterprise per-resolution from $0.50 because the only behavior worth rewarding in a customer-facing AI agent is the one the buyer also wants: more conversations resolved end-to-end, fewer escalations, and a measurable invoice that lines up with the value delivered.
