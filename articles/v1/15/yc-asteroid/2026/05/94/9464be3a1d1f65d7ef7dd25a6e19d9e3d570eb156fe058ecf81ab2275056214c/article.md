---
schema_version: "1.0.0"
document_id: "9464be3a1d1f65d7ef7dd25a6e19d9e3d570eb156fe058ecf81ab2275056214c"
company_key: "yc-asteroid"
company: "Asteroid"
source_id: "yc-asteroid-news-import-1b74f0bf6416"
canonical_url: "https://asteroid.ai/blog/healthcare-workflow-automation/"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-07-24T08:09:50.096820+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:20b78d5d8598d563cb8fa72e4ed176490d1ceb0ec8003e1bd698f41c4a642853"
---

# Healthcare workflow automation: a builder's atlas

**Healthcare workflow automation has held the same architectural shape for fifteen years.** Each stage of the rev cycle is owned by a category leader, each well-built at its own stage:


- **Patient intake** belongs to Phreesia and Clearwave, whose kiosks and digital forms run in most US health systems’ waiting rooms.
- **Eligibility verification** runs through Availity and Change Healthcare, the largest clearinghouses for 270/271 EDI checks.
- **Prior authorization** is owned by Cohere and Surescripts, who handle PA submission, status tracking, and ePA routing.
- **Claims and remittance** flow through Waystar and Optum, the major rev-cycle clearinghouses for denials and reconciliation.
- **Scheduling** sits inside athenahealth and Phreesia, with EHR-native scheduling everywhere else.


Underneath all of these sits the EHR (Epic at most large US health systems, eClinicalWorks and athenahealth elsewhere) as the system of record every workflow eventually has to read from or write data back to. The chain between these layers is held together by humans re-typing what one tool printed and the brittlest RPA scripts in production. Healthcare interoperability (FHIR, HL7, the Cures Act information-blocking rules) has been pursued for fifteen years as the answer to all of this, and at the top of the market it works. Modern product surfaces like Tennr, Cedar, and Notable have emerged on top of this stack. They all hit the same structural ceilings underneath.


That shape has held because the wall in front of it was structural: ~1,000 commercial payers, 50 state Medicaid programs, every flavor of EHR, the specialty registries nothing integrates with, and fax machines still running in 2026. Integration economics couldn’t reach the long tail. Pre-LLM software couldn’t span the chain with judgment. **Both ceilings have moved in the last 18 months.**


This is a builder’s atlas: a category map for the platform CTOs and engineering leaders *building* healthcare automation as a product. Where the categories sit, what changed, and where the differentiated wedge has moved now that the agent runtime itself is on its way to becoming a commodity. Most of the platforms building here haven’t repositioned yet.


Today


### The platform's back-end is humans + RPA glue


Your platform


what your customer sees


Manual ops + RPA


Your team logs in and types the data. Or scripts you maintain do it.


Doesn't compound at scale


EHR portals


Epic, eCW, Athena…


Payer portals


~1,000 commercial
+ 50 Medicaid


Specialty + tail


RBM, BH, fax, paper…


## What healthcare workflow automation actually is


The term covers any system that takes a multi-step healthcare operations workflow (patient onboarding, eligibility checking, prior authorization, claims, scheduling, records release, care navigation) and turns some part of it from manual work into automated work. **In 2026, that scope is wider than it sounds.**


Most healthcare workflows are chains of 8-15 stages spanning multiple systems: the EHR for clinical context, the payer portal for eligibility and prior auth, the clearinghouse for 270/271 EDI, an inbound fax for the referral, sometimes a phone call to a benefits administrator, and a write-back step into a billing system. Automating one stage of one of these chains has been done for a decade. That’s the patient-facing portal market, the eligibility-clearinghouse market, the e-form market. Automating *the chain* (including the judgment calls between stages) is where the actual work has been.


Healthcare automation has two sides:


- **The patient-facing surface** (forms, scheduling, communication). More developed: category leaders sit at billions in market cap, and the structural shape of the market is largely set. Still far from won, with vertical specialization and AI-native rebuilds finding real ground.
- **The payer-facing back-end** (eligibility, PA, claims, write-back into the EHR). The open frontier.


## The major categories (a builder’s atlas)


A pragmatic taxonomy of where healthcare workflow automation operates today. Each category in this list will get its own deep-dive leaf in the coming weeks: design implications, structural ceilings, where browser agents fit, and what the production reality looks like for a builder shipping in that vertical.


- **Patient intake automation:** onboarding, demographics, insurance capture, eligibility, consent, referrals. The 12-stage chain that defines the front door of the rev cycle.
- **Eligibility verification automation:** real-time and batch coverage checks; the 270/271 EDI layer plus the long tail of non-EDI payer portals. The clearinghouse market handles the top tier; the rest is portal work. In the library:[the eligibility and benefits check](https://asteroid.ai/workflow-library/eligibility/eligibility-benefits-check) .
- **Prior authorization automation:** submission, status checking, peer-to-peer scheduling, determination tracking. The single most-cited rev-cycle pain point in every survey of provider organizations, and the explicit subject of CMS-0057-F (payer FHIR APIs for PA mandated by 2027). Deep-dive:[prior authorization automation in production](https://asteroid.ai/blog/prior-authorization-case-study/) . In the library:[PA submission with attachments and clinical Q&A](https://asteroid.ai/workflow-library/prior-auth/pa-submission-attachments-clinical-qa) .
- **Referral intake automation:** receiving, parsing, and routing inbound referrals from PCP → specialist or hospital → post-acute. Heavy mix of fax, EHR, and unstructured PDFs. In the library:[inbound referral capture and EHR referral creation](https://asteroid.ai/workflow-library/referral-intake/inbound-referral-capture) .
- **Claims processing automation:** denial management, resubmission, appeals, remittance reconciliation. The back half of the rev cycle. In the library:[denial management on payer portals](https://asteroid.ai/workflow-library/claims/denial-management) .
- **Records release / requests automation:** the stack that handles ROI requests, subpoenas, and clinical-records queries. Heavily fax- and portal-based; a category most people don’t realize exists until they sell into it.
- **Scheduling and appointment management:** patient-side and provider-side calendars, recall, no-show recovery. Crowded with category leaders. In the library:[appointment booking and availability export](https://asteroid.ai/workflow-library/scheduling/appointment-booking-availability-export) .
- **Care navigation and care coordination:** multi-stakeholder workflows in oncology, behavioral health, transitions of care. Less mature, more vertical-specific.
- **Billing and revenue cycle automation:** the umbrella above claims; pricing, eligibility-driven billing, patient financial responsibility. Deep-dive:[medical billing automation in production](https://asteroid.ai/blog/medical-billing-automation/) .


## Structural ceilings in healthcare workflow automation and how they’re changing


Across all of these categories, the same two issues show up. Whether you’re building intake automation, PA automation, or claims automation, the wall you hit is structurally identical.


### Ceiling 1: every back-end system owns one stage; you have to build the glue


Phreesia owns intake forms. Availity owns eligibility. Cohere owns prior auth. Each is well-built at its own stage, and Epic competes with each of them through first-party modules from underneath, so the “category leader” picture is really “two or three contested incumbents at every stage.” But **the handoffs between them have been the platform’s job to build, and the chain has been held together by the platform’s ops team plus the brittlest RPA scripts in production software.** Nothing pre-LLM could span the chain end-to-end with judgment: read an eligibility response, classify it (“active / needs PA / inactive / wrong payer”), route to the right next step, fill the next form, pause if ambiguous.


That cross-stage layer with judgment has been the missing piece across *every* healthcare workflow automation category. The shape of the legacy back-end is dozens of well-built tools sitting next to each other, with the platform’s people reading the output of one and typing the input of the next.


### Ceiling 2: the long tail of payers and portals defeats integration economics


The category-leader integrations cover the top tier. ~1,000 commercial payers, 50 state Medicaid programs, dozens of specialty portals (radiology benefit managers, behavioral-health carve-outs, state immunization registries, county records sites) sit underneath that line. Building proper integrations to all of them is uneconomic, even if you’re a publicly traded vendor with a thousand engineers.


Result: humans logging into portals remains a load-bearing step in 2026, even at the most modern, well-tooled platforms. **The long tail is a structural feature of healthcare’s payer landscape that no integration-economics-driven approach can solve.** Fax is still part of this picture; an embarrassingly large fraction of US healthcare workflows still pass through fax-to-PDF and back.


### Where healthcare interoperability stops


The standard-issue industry answer to all of this is healthcare interoperability: FHIR APIs, HL7 messaging, the 21st Century Cures Act’s information-blocking rules. The industry has been pursuing it for fifteen years, and at the top of the market it works. Major EHRs publish FHIR endpoints, the largest payers expose claims and coverage data through standardized APIs, and the ONC’s certification regime forces a baseline of conformance. CMS-0057-F, finalized in 2024, will mandate payer FHIR APIs for prior authorization in 2027.


But interoperability standards are an integration-economics solution. Building, certifying, and maintaining a FHIR endpoint requires real engineering investment from every participant, and the long tail of US healthcare doesn’t have the engineers, or the regulatory pressure that would force them to. The bottom 800 commercial payers, state Medicaid shops still on legacy mainframes, radiology benefit managers, rural health systems on customised eCW: none of them are showing up to a FHIR working group. **The interoperability ceiling and the long-tail ceiling are the same wall seen from two angles.**


Browser agents operate whatever interface is in front of them, the same way a human would. The standards push covers the top tier of US healthcare. The agent layer covers everything beneath.


## What changed in 2025-2026


Three architectural shifts moved both ceilings. None of them are magic. Together, they reset what’s economic to automate.


With agents


### A browser-agent fleet is the back-end


Your platform


what your customer sees


Browser agent fleet


Reads, classifies, routes, fills. Pauses if ambiguous.


Pause for human review


EHR portals


Epic, eCW, Athena…


Payer portals


~1,000 commercial
+ 50 Medicaid


Specialty + tail


RBM, BH, fax, paper…


### 1. Cross-stage glue with judgment


Browser agents can read an output from one stage of a workflow, classify it, open the right downstream portal, fill the form, and submit. RPA could chain steps, but every branch had to be hard-coded; the moment a payer’s eligibility response had a new status code, the script broke. Browser agents handle the routing decision, the formatting decision, and the “is this ambiguous enough that I should pause?” decision in the same loop. The cross-stage chain becomes one orchestration layer instead of many brittle scripts.


### 2. Per-payer / per-portal customization at the cost of a prompt edit


Adding a new payer portal used to be an engineering project: discovery, integration spec, test environment, edge cases, monitoring. With browser agents, it drops to *describe the form, fork the prompt, edit the URL.* The fork-and-deploy unit replaces the build-an-integration unit. **Non-technical operators can ship per-payer agents with no engineering involvement.** Observed in production at platforms shipping nine or more payer agents from a single non-technical operator, fork-by-fork. The bottom 80% of payers becomes economically addressable for the first time.


### 3. The pause-for-human-review primitive


RPA was binary: fully autonomous or fully manual. Browser agents introduce a third state: **work to a checkpoint, pause, request human review, resume after correction.** That’s what makes agents deployable in clinical contexts at all. Without the pause primitive, edge-case errors propagate into claims and into billing. With it, the human becomes the *adjudicator* , the one making the load-bearing call. The agent does the typing.


## The architectural shift and what it means for builders


**The agent runtime is becoming a commodity. The differentiated wedge in healthcare workflow automation has moved one layer up the stack, into the apprenticeship loop, the human-review queue, and the cross-stage orchestration layer.**


The companies that win the next decade of this category will own the pause-for-review primitive, the per-payer fork-and-deploy UX, and the iteration-by-execution loop that turns customer-specific workflow corrections into production specs without an engineering ticket. The agent is the table-stakes layer underneath. What you build *on top* of it is the product.


For a platform CTO designing in this space, that argues for a small number of concrete bets:


- **Treat the human-review queue as a first-class data model.** The queue UX, the resume semantics, the audit trail, the metrics are all load-bearing. This is where your differentiated UX lives.
- **Adding a new portal should be a fork of an existing agent.** An ops person should be able to copy an agent, point it at a new URL, and ship in an afternoon. There are too many portals to staff this with engineers, and the platforms that cover the long tail are the ones built so non-engineers can do the forking.
- **Differentiate on the apprenticeship layer.** The iteration loop that turns an execution failure into a corrected spec without an engineering ticket is where the real product lives.
- **Plan for production scale honestly.** 1,000+ parallel browser sessions, session isolation, anti-bot evasion, cost-per-execution under control. This is where build-vs-buy needs evaluating most. Most platforms underestimate the operational distance from “agent works in dev” to “agent runs reliably under burst load.”
- **Long-tail addressability is the wedge most platforms underestimate.** The companies that handle the bottom 80% of payers and portals (the ones the category leaders skip) are the ones who win categories the incumbents can’t reach.


## What’s next in this series


This hub keeps expanding. Each category in the atlas above gets its own deep-dive leaf: the design implications, the structural ceilings, the production realities a builder needs to know before committing engineering time. Live so far:


- [Medical billing automation: what production deployments actually look like](https://asteroid.ai/blog/medical-billing-automation/)
- [Automating prior authorization with AI browser agents](https://asteroid.ai/blog/prior-authorization-case-study/)


For the workflow templates themselves, category by category, start at the[workflow library](https://asteroid.ai/workflow-library) .


For context on who’s writing this: most of the platforms we work with at[Asteroid](https://asteroid.ai/) are healthcare technology companies trying to scale their growth thesis through legacy systems that were never built for agent-scale traffic. Exactly the builder-tier this post was written for. We’ve spent the last two years helping them solve the cross-stage chain: the orchestration, the per-payer customization, and the human-review primitive most teams discover they need only after their first agents have gone to production. If the structural argument here matches the wall you’re hitting, get in touch.
