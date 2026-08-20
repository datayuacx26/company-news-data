---
schema_version: "1.0.0"
document_id: "b0fe724570655f7a1d35ab409cd154a4dbbb97d5cfd271e6a83007cd0a296ffb"
company_key: "yc-automat"
company: "Automat"
source_id: "yc-automat-news-import-800367705320"
canonical_url: "https://runautomat.com/blog/how-abb-put-ai-agents-to-work-across-four-departments-and-got-back-a-full-fte-year-of-capacity"
published_at: "2026-08-18T17:56:36.139+00:00"
first_seen_at: "2026-08-18T18:53:01.873812+00:00"
fetched_at: "2026-08-18T18:53:04.073416+00:00"
content_hash: "sha256:8041ea197b96e7d0adf946466b98bf7544929ba8080a5bb8413cac0979992143"
---

# How ABB Put AI Agents to Work Across Four Departments and Got Back a Full FTE-Year of Capacity | Automat

In under a year, ABB and Automat put AI agents to work across four corners of the business (Engineering, Sales Operations, Logistics, and Billing), automating 16,200+ work items end-to-end and returning roughly a full FTE-year of capacity to ABB teams. The agents operate ABB's existing systems the way a person does: no APIs, no RPA scripting, no rip-and-replace. What began as a small, self-contained pilot in 2024 is now production infrastructure, with new programs ramping.


‍


## The challenge


ABB's operations run on document-heavy, multi-system manual work spread across departments: engineering drawings, purchase orders, shipping paperwork, and cost reconciliations, with no two source documents looking quite alike. The team had tried traditional, script-based RPA before Automat, and it broke constantly. Those tools need a fixed set of buttons and fields to click on, and SAP's own screens don't offer that, so every SAP update or screen change meant broken automations and manual patchwork. Document formats varied by supplier and carrier, which meant template-trained tools needed constant retraining. The underlying problem wasn't a lack of automation attempts; it was that the tools available couldn't keep up with how varied and visual the actual work was.


‍


## The partnership at a glance


What started in 2024 as a small, self-contained pilot automating a single, well-defined workflow has grown into a true cross-functional rollout: four departments, multiple programs live in production, and several more ramping. Automat's agents now run daily, unattended, handling the parts of the job that used to eat a full-time employee's week, with a single human approval click as the only manual step left in the loop.


‍


## Cutting manual work across Engineering, SalesOps, and Logistics


**Engineering: drawing dimension extraction**


Every motor shaft ABB manufactures starts as an engineering drawing, and every drawing carries dozens of measurements an engineer used to have to read off the page and type in by hand. Automat's agents took over that read: they look at raw drawings the way an engineer would and pull every critical measurement automatically, no template setup, no manual transcription. Early runs weren't perfect, so ABB's engineers reviewed the outputs and fed corrections back in, and the extraction kept getting sharper from there.


*Illustrative example: a motor shaft engineering drawing showing the kind of dimension callouts Automat's agents read automatically.*


- 10,412 drawings processed
- 24 measurements per unit, roughly 124 seconds each
- Accuracy climbed from 37% to 100% between June 2025 and February 2026, as edge cases (multiple keyways, non-standard bearings, ambiguous callouts) were folded into the extraction logic


‍


**Sales Operations: order validation and cost-of-goods reconciliation**


Prior to Automat, incoming orders used to mean someone on ABB's sales ops team checking it line by line against its supporting paperwork, a routine that ate hours and still let mistakes slip through. Automat's agent now makes that first pass itself: reading each order, checking it against its documents, and flagging anything that looks off, before a person ever opens the file. A human still has the final say, one click to approve or override, but the tedious first read is gone. The same team applies the same approach to its monthly cost-of-goods reconciliation across ABB's distributor network, turning what used to be a recurring close-out slog into a same-day check.


- 4,218 orders reviewed against their supporting purchase documents
- 2,262 recommended optimizations implemented before they became downstream production or billing problems
- 84.1% decision coverage
- 54 cost-of-goods reconciliations completed across 6 distributors, each with a one-click human approval


‍


> Our AI agents now manage complex sales and operations workflows within SAP and process data from thousands of technical drawings in collaboration with our engineering teams.
>
>
> - Terrell King, U.S. System & Sales Manager


‍


**Logistics: shipping documentation**


ABB ships through seven different carriers, and until recently, someone on the logistics team had to log into SAP, visit each carrier's website one at a time to track down pickup and delivery confirmations, then copy everything back into SAP by hand. Automat's agent now runs that entire loop itself: exporting open shipments from SAP, checking every carrier's site for tracking and proof-of-delivery, and writing the results back into SAP exactly the way a person would, just continuously, and without the typos.


- 1,486 shipments documented end-to-end across all 7 carriers ABB ships with
- Proven capable of ~300/day
- Now running on a daily schedule


‍


## The outcomes


- ≈ 1 FTE-year of manual work returned to ABB teams
- 16,200+ work items automated end-to-end
- 1,980 hours of manual work eliminated across four mature programs
- 2,262 optimizations implemented (accuracy / risk-reduction proof)
- Engineering drawing accuracy climbed from 37% to 100% (Jun 2025 to Feb 2026)


‍


> Since implementing the Automat Automation Platform, we’ve achieved immediate business impact across Sales, Engineering, and Logistics: hundreds of hours saved, improved data accuracy and governance, and a more efficient, scalable experience for our users.
>
>
> - Terrell King, U.S. System & Sales Manager


‍


## Why it matters to ABB


- **Capacity returned.** Roughly a full FTE-year redirected from repetitive document work to higher-value work.
- **Risk reduced.** Every order and every drawing gets reviewed at a scale and consistency no manual process can sustain.
- **Hands-off.** The automations run end-to-end. The only routine human step is a single approval click.
- **Adaptable and maintained.** Built to keep working through ABB's own system and process changes, not to break the moment something shifts.
- **Headroom to do more.** Today's results run on a single shared machine; dedicated capacity would multiply throughput without multiplying headcount.


‍


## Why traditional RPA couldn't do this


ABB runs its ERP through SAP's own screens, which don't give scripted bots the fixed buttons and fields they need to work reliably, so any script-based automation breaks the moment SAP is updated. Prior attempts at RPA here ran into exactly that wall. Automat's agents work differently: they look at the screen the way a person does, reading labels and layouts instead of hunting for fixed technical hooks, and they read documents (engineering drawings, purchase orders, shipping paperwork) without needing a template built and maintained for every format. That's what let a single pilot scale across four departments instead of staying stuck as one brittle bot.


‍


## The road ahead


- Shipping documentation moving fully onto a daily schedule (already largely there).
- The newer engineering drawing program reaching full production volume.
- Dedicated, always-on capacity to run automations in parallel rather than sharing a single machine.
- A stronger platform home for ABB's automations: self-serve dashboard, enterprise SSO/security, and room to scale.


‍
