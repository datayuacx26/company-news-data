---
schema_version: "1.0.0"
document_id: "42207323998971f4b8c9c1f2bbfd327a98c8ffc0b34616a55ed565a2f2eec539"
company_key: "yc-prodigal"
company: "Prodigal"
source_id: "yc-prodigal-rss-445231af9883"
canonical_url: "https://www.prodigaltech.com/blog/how-ai-handles-subprime-auto-loan-delinquencies-at-scale"
published_at: "2026-06-14T05:27:14+00:00"
first_seen_at: "2026-07-25T19:50:44.288158+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:b60f8a69b404ada7e9f1f28dda26a5a5e1e51ba075a748401fbec97a153e9cc7"
---

# How AI handles subprime auto loan delinquencies at scale -

Key takeaways


-


Subprime auto loan delinquencies reached 6.9% at the 60-day mark in January 2026, the highest level in over three decades, according to Fitch Ratings
-


The cure amount on a delinquent subprime auto account changes every day — per diem accrues continuously, late fees compound, and partial payments do not reset the delinquency clock
-


General-purpose AI cannot perform accurate real-time cure calculations on a live account; AI built specifically for auto collections can, and the difference shows up in recovery rates at scale
-


A subprime auto lender working with Prodigal saw an 8% lift in pre-charge-off recovery, a 23% reduction in cost-to-collect, and a 45% increase in self-serve payments
-


The Notice of Intent to Repossess is the right deployment boundary — AI handles the pre-NOI stage at scale, human collectors handle the complex late-stage work that requires judgment and authority


Subprime auto loan delinquencies hit 6.9% at the 60-day mark in January 2026, the highest level in over three decades, according to Fitch Ratings.


Every delinquent account eventually becomes a phone call. A borrower asking what they owe, why the balance has grown, and what it takes to get current.


In subprime auto, the cure amount changes every day as per diem keeps running, late fees accumulate, and what the borrower owed yesterday is not what they owe today.


The cure calculation


What a borrower actually owes at 30 days past due


Missed installment amount


Base payment


Interest accrued since last payment cleared


Per diem × days


Outstanding late fees


Per contract


Partial payment shortfall from prior month


If applicable


Total cure amount


Changes every day


Why the number keeps changing


Per diem accrues every calendar day, including weekends


A partial payment reduces principal but does not reset the delinquency clock or stop late fees


A payment extension adds one month to loan maturity and one extra month of interest at the end of the term


The cure amount the borrower heard about yesterday is already different today


The AI agent on the call needs **the exact number at that exact moment** — from the live servicing system, not a script.


## **The math behind every delinquency call**


A subprime auto loan accrues interest every single day. Per diem does not stop because a payment was missed, a partial payment was made, or an extension was granted. Every day that passes, the amount required to cure the account is different from the day before.


When a borrower calls at 30 days past due, the figure they need is not the missed installment. It is that installment, plus the interest accrued since the last payment cleared, plus any outstanding late fees.


If they sent a partial payment the previous month, that payment reduced the principal modestly but did not reset the delinquency clock. The late fees and the per diem keeps accumulating.


This is the financial reality that sits beneath every subprime auto loan delinquency conversation, and borrowers rarely arrive with a clear understanding of it.


The AI agent on the call needs to have that understanding completely, from the live account, not from a script.


## **What general-purpose AI gets wrong**


A general-purpose AI platform can hold a collections call, it can ask questions, process responses, and follow a script with competence.


What it cannot do is reach into a servicing system, retrieve the current state of a delinquent subprime auto account, and perform the calculations that make the conversation meaningful.


When a borrower says they made a payment last week and expects the account to be current, a general-purpose platform has no way to verify whether the payment cleared, how it was applied, or what the remaining cure amount looks like after per diem has continued to run.


It cannot tell the borrower what they owe today, and it cannot explain why the number differs from what they expected.


An AI agent built for auto collections can do each of those things. It connects to the live account, understands how interest accrues, how late fees compound, how extensions affect loan maturity, and how partial payments impacts outstanding balances.


proAgent in action


How proAgent handles a partial payment delinquency call


Borrower is 35 days past due. They sent a partial payment three weeks ago and believe the account is current.


Borrower


"I already sent in a payment last month. I don't understand why my account is still showing past due."


⟳ proAgent pulls live account data: payment posting date, amount applied, remaining balance, current per diem, outstanding late fees, days past due as of today.


proAgent


"I can see that payment posted. It was applied to your outstanding interest and a portion of the principal. Because it was a partial payment, it did not bring the account fully current, and your account remains past due. Late fees have continued to accrue since then."


Borrower


"How much do I actually owe to get this resolved?"


⟳ proAgent calculates current cure amount from live account: remaining past-due balance, interest accrued since partial payment cleared, outstanding late fees.


proAgent


"To bring your account fully current today, the total includes the remaining past-due balance, interest accrued since your last payment, and the outstanding late fee. I can walk you through a payment arrangement, or if you are able to pay the full amount, I can confirm that right now."


Borrower


"I can do the full amount. Can we do that on Friday?"


⟳ proAgent logs promise to pay: date, amount, channel. Account flagged for follow-up if payment does not post by end of day Friday.


proAgent


"I have noted your payment for this Friday. Per diem will continue to accrue until the payment posts, so the final amount due on Friday will reflect that additional interest. I will send you a confirmation with the details."


What proAgent accessed


Data source


Live servicing platform


Payment status


Partial payment verified and applied


Cure amount


Calculated in real time as of today


Per diem


Disclosed and applied to Friday commitment


Outcome


Promise to pay confirmed. No escalation.


**No human collector needed.** The borrower's confusion was resolved with accurate account data, not a script. The call ended with a confirmed commitment and a logged follow-up.


## **Where AI belongs in the collections lifecycle**


The subprime auto collections lifecycle has a clear inflection point which is the Notice of Intent to Repossess.


Before that point, the lender is collecting. Accounts are delinquent but curable. The conversations are high-volume, structured, and rule-governed:


- Explaining cure amounts and per diem accrual accurately
- Capturing payment commitments and confirming arrangements
- Routing extension requests and hardship cases
- Managing objections from borrowers who are financially stressed but not yet in crisis


This is where AI in auto collections operates best. The work is defined, repeatable, and sensitive to scale.


After the Notice of Intent to Repossess, the lender is recovering. What happens next requires human judgment to assess the borrower's situation, exercising authority on concessions, reading whether a promise to pay will hold.


A well-structured deployment lets AI handle the pre-NOI stage at scale and in compliance. The human team concentrates on the late-stage accounts where their judgment is what saves them.


The deployment boundary


Where AI operates and where humans take over


15–30 DPD Early stage


30–45 DPD Core AI window


45–60 DPD Escalation signals


NOI Inflection point


60–90 DPD Human led


90+ DPD Recovery


Before NOI


The lender is collecting


AI handles this


Accurate cure amount calculation on every call


Capturing payment commitments and confirming arrangements


Routing extension requests and hardship cases


Managing objections at scale, 24 hours a day


Notice of Intent to Repossess


The inflection point


After NOI


The lender is recovering


Human collectors


Assessing the borrower's actual financial situation


Exercising authority on concessions and settlements


Reading whether a promise to pay will hold


Deciding when to proceed to repossession


## **What Prodigal has seen in practice**


A subprime auto lender came to Prodigal with a portfolio under pressure. Delinquency was rising, agents were stretched, and leadership did not want to solve the problem by adding headcount. The goal was collections that felt modern, empathetic, and designed for self-service.


**proCollect** unified data across CRM, dialer, payment portal, email, and text platforms. AI and machine learning models scored every consumer daily on intent to pay, channel affinity, and affordability.


Outreach was timed to when payment likelihood was highest. Over 200 persona-matched templates were deployed across first-time delinquents, habitual late payers, and borrowers in hardship.


The lender started on 33% of their pre-charge-off portfolio while the rest ran on the legacy process. Results improved month over month.


By month 3, with 75% of accounts on the new program, the outcomes were:


Prodigal in practice


Subprime auto lender results with proCollect


Started on 33% of the pre-charge-off portfolio, expanded to 100% based on results.


8%


Lift in pre-charge-off recovery


23%


Reduction in cost-to-collect


45%


Increase in self-serve payments


78%


Increase in digital engagement


How results built over time


Month 1


+6%


Payments collected through stronger text and email engagement


Month 2


+27%


Lift in 30-day past due bucket as models improved with data


Month 3


+8%


Overall payments with 75% of accounts on the new program


Source: Prodigal client data — subprime auto lender, founded 2010. Results from AI-led omnichannel collections deployment using proCollect.


The lender subsequently expanded Prodigal to cover 100% of pre-charge-off accounts.


## **Before you deploy**


Most vendor demonstrations for AI in auto collections focus on conversation quality. The more revealing test is financial calculation accuracy on a live account under real conditions. Five questions worth putting to any vendor:


1. Can the platform access live account data from the loan origination system and servicing platform during a call and calculate the current cure amount in real time?
2. How does it respond when a borrower reports a partial payment and believes the account is settled?
3. Can it explain the financial impact of a payment extension, including the additional interest that accrues and the effect on loan maturity?
4. What happens when a borrower challenges the balance? Can the platform walk through the calculation in a way the borrower can follow?
5. Who is responsible for loan origination system and servicing platform integration, and what does that handoff look like after go-live?


A vendor who cannot demonstrate these capabilities on a real account in a test environment will not deliver them reliably in production.


## **Frequently asked questions**


Frequently asked questions


AI in subprime auto collections: common questions


Does an AI agent have the same FDCPA and TCPA obligations as a human collector?


Yes. The CFPB has made clear there is no carveout for AI from existing debt collection regulations. An AI agent making outbound collection contacts is subject to the same FDCPA disclosure requirements, TCPA prior express consent rules, time-of-day restrictions, and Reg F contact frequency limits as a human collector. The compliance advantage of a well-built AI agent is that these requirements are programmatically enforced on every contact, eliminating the human variance that creates exposure in traditional operations. **Prodigal's compliance guardrails are encoded at the account level** , not left to agent judgment.


How does TCPA prior express consent get verified before an AI agent places a call or sends a text?


Before any contact attempt, the platform queries the lender's own system of record to verify that valid prior consent exists for the specific number being contacted. **The lender's records remain authoritative** — there is no separate consent store that could conflict with or override them. Each consent verification is logged against the contact attempt, creating an audit trail for TCPA purposes. This matters particularly for subprime auto portfolios where borrowers frequently change phone numbers and consent status needs to be checked in real time, not assumed from a static list.


What happens to borrower data during an AI-handled collection call? Who has access to it?


During a call, the AI agent accesses account data — balance, payment history, contact records — solely for the purpose of handling that interaction. **Prodigal does not retain or independently process borrower data after the call ends.** Data access is limited to what is required for verification and conversation handling. For lenders with bank parents or under specific data governance requirements, Prodigal's Trust Center provides full documentation on data handling, access controls, and model training practices before any deployment begins.


What systems does an AI collections agent need to integrate with, and how complex is that?


For auto loan collections, meaningful integration requires connection to the loan management system for live account data, the CRM for contact history and disposition logging, the payment service provider (such as Repay or similar) for in-call payment processing, and communication channels including text platforms like Twilio and email systems like SendGrid for digital outreach. **proAgent integrates with all of these in real time** and syncs every interaction back to your CRM and LMS without requiring system replacement. Prodigal's implementation team owns this integration through go-live and beyond — it does not transfer to your IT team at launch.


Can an AI collections agent handle Spanish-speaking borrowers?


Yes. proAgent handles collections conversations in both English and Spanish across voice, SMS, and email. For subprime auto lenders with significant Spanish-speaking portfolios, this affects both recovery rates and language access compliance obligations. **The same cure calculation accuracy, payment processing capability, and compliance guardrails apply in both languages** — the Spanish-language experience is not a limited version of the English one.
