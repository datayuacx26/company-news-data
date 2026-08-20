---
schema_version: "1.0.0"
document_id: "37a4fd17874aafe5838a8a9276c664ca2d561ed8a305a8691369a299fb5f706d"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/aws-decides-whether-you-get-a-rep/"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-29T23:25:13.086112+00:00"
fetched_at: "2026-07-29T23:25:14.431269+00:00"
content_hash: "sha256:e1090c66cd7b780e8453a9e9fcb6bbf6e1194f5b42abd725ffb882a70d239f39"
---

# AWS Now Decides Whether You Get a Rep. Here's How That Decision Gets Made.

Introducing co-sell quality prediction, AI-generated business problems, and predicted AWS contacts in Suger.


On June 16, 2026, AWS made it official: Partner Central agents now qualify every co-sell opportunity in real time. Every opportunity receives an Opportunity Quality Score that “measures co-sell readiness and directly influences how AWS engages.”


Each opportunity is now matched to one of three co-sell motions:


- **AWS field-engaged** — an AWS sales team collaborates directly on your deal.
- **Agent-engaged** — the agent strengthens your submission to try to earn AWS engagement.
- **Partner-led** — you drive the deal yourself, with agent support.


The score determines which one you get. And the motion recalculates in real time as the opportunity improves.


So co-sell is no longer a queue you wait in. It’s a threshold you either clear at submission or don’t. A weak referral doesn’t get ignored anymore — it gets formally routed to “Partner-led,” which is a polite way of saying you’re on your own.


That makes one question urgent for every ISV: what actually moves the score?


---


## **Finding #1: the score has nothing to do with your deal**


Every single LOWEST referral scored exactly 0. Not 12, not 8. Zero.


Every HIGHEST referral landed between 50 and 75. Note the ceiling: even AWS’s own “best” examples top out at 75. Nobody is anywhere near 100. There is enormous headroom in a field where most submissions are scoring nothing.


Deal size didn’t correlate. Neither did expected AWS revenue, industry, or region. What the score measures is narrative completeness — whether an AWS seller (or now, an agent deciding your motion) can read your referral and immediately know who the customer is, what’s broken, why they have to act now, and what you need from AWS.


It rewards a specific, actionable customer story. It punishes empty templates and product pitches.


Which means the thing gating your access to AWS field engagement is, functionally, the quality of a paragraph.


---


## **Finding #2: there are two completely different ways to fail**


This is the part that surprised us.


The referrals scoring zero weren’t losing on nuance. Nearly half of them were submitted with the template still sitting in the field — placeholder bullets, unanswered prompt questions like “what is the customer trying to solve?”, boilerplate stubs nobody deleted. A third had no customer context at all. A third described what the ISV’s product does instead of what the customer needs.


These aren’t storytelling failures. They’re submissions that were never really written. And they appear to be unrecoverable — no amount of detail elsewhere in the record pulls them out of a zero.


The HIGHEST-scoring referrals, meanwhile, were missing many of the same finer details as the zeros: no concrete ask of AWS, no named customer contact, no quantified scope. Those gaps are near-universal. They are not what separates the buckets.


What separates the buckets is whether anyone wrote a real customer story at all.


That’s the whole game, and it splits into two very different problems:


- **Tier one — the zeros.** Empty templates, product pitches, blank required fields. High volume, entirely mechanical, and completely preventable.
- **Tier two — the ceiling.** Even good submissions leave points on the table on the refinements. Worth optimizing, but only after tier one is solved.


Most ISVs think they have a tier-two problem. Our data says the majority have a tier-one problem they don’t know about, because nobody sees the referral again after it leaves the CRM.


The encouraging part: every signal the score rewards is either already sitting in your CRM or is one question away from the rep who’s on the deal. Almost none of it requires new information. It requires the information to make it into the right field, in the right form, before you hit submit.


Which is exactly what we built.


---


## **What we shipped**


AWS’s agent will recommend improvements after you submit, and the score recalculates. That’s genuinely better than the old silence. But it’s still a loop that starts after your deal has been sorted into a motion — and after your rep has moved on to the next thing.


We’d rather you never submit a zero. So we moved the evaluation upstream, into the submission itself.


### **1. Predicted quality score, before you submit**


Suger now shows a live Predicted AWS quality score on the co-sell submission screen, broken out by dimension, with the specific fixes ranked by how many points they’re worth.


It reads like AWS reads. A submission that scores 52 tells you why:


> This opportunity contains a strong compelling event, clear requirements, and is well-quantified. However, it is critically undermined by an apparent copy-paste error, as the customer is ‘Spyce’ but the business problem describes ‘Northwind.’ It also completely lacks next steps and competitive details.


- **+25** Rewrite the Customer business problem to be about the actual customer, removing incorrect references. Add a specific ask for AWS.
- **+15** Add a Next step with a concrete action, a target date, and the customer contact involved.
- **+8** In Competitive tracking, name the incumbent or competing vendors being evaluated.


Apply those and the same record hits 92 — with a single 8-point gap left on competitive tracking. That’s not a marginal improvement. In the new motion model, that’s the difference between “Partner-led” and “AWS field-engaged” on a deal you already had.


The gate checks run as hard validation. Everything else is a nudge with a point value attached, so your reps know exactly what a fix is worth and stop guessing.


### **2. AI-generated business problems in field mapping**


The bigger unlock is that you don’t have to write the narrative at all.


Suger’s field mapping already pulls your Salesforce data into every co-sell field. We added an AI generation option on any field — including the Customer Business Problem, the field that carries more weight in the score than anything else in the record.


Instead of piping your raw opportunity description straight through (which is how the template text and product pitches get in there), you configure a prompt once. It reads the account name, industry, close date, renewal date, AWS services in scope, competitors, use-case notes, and expected spend from the opportunity, and drafts the strongest customer-centric business problem those facts support.


Configure it once, and every referral your team pushes clears the gates by construction. Three rules matter in that prompt:


- **Write about the customer, never the product.** A product pitch scores 0. The prompt has to be explicitly forbidden from describing your own capabilities.
- **Use only facts present in the data.** This text goes to AWS as a factual claim about a real customer. No invented deadlines, dollar figures, contacts, or compliance standards — ever.
- **Never emit placeholders.** If a fact is missing, write around it. Then surface the gap to the submitter as a data gap, rather than papering over it.


The output is a tight narrative that names the customer, anchors urgency to a real date, names the AWS services and compliance requirements in play, and closes with one concrete ask — plus a list of the two or three missing fields that would raise the score most if the rep filled them in.


You go from your reps writing 200 co-sell descriptions a quarter to your reps only reviewing the auto-generated drafts.


### **3. When the score still isn’t enough: predicted AWS contacts**


Even a well-scored referral can land in “Agent-engaged” or “Partner-led.” AWS coverage is finite, territories shift, and no number on a form guarantees a human on the other end.


So Suger doesn’t make the score your only path to an AWS human. If the motion model doesn’t hand you a rep, we’ll tell you who the rep could have been.


Our account mapping layer maintains a database of cloud reps across AWS, Azure, and GCP — enriched with the domains each rep actually manages, their industries, coverage location, region, account count, open pipeline, closed-won, and success rate. When you open an opportunity, Suger surfaces the predicted contacts for that account: the specific AWS sellers whose book of business already includes that customer domain.


That means if your referral doesn’t get an assignment through the ACE queue, you’re not stuck waiting. You have a named rep, their track record, and the accounts they cover — surfaced right in the Salesforce widget on the opportunity, not buried in a spreadsheet someone maintained six months ago.


We layer Collaboration Scoring on top: your team logs how each cloud rep actually engaged on each deal, and Suger aggregates it — weighted by outcome and recency — into a per-rep score visible in the widget, the rep profile, and the account mapping list. Institutional knowledge about how AWS reps operate stops living in one person’s head and surviving exactly as long as they do.


---


## **The short version**


AWS has told you exactly what it wants and built an agent to enforce it. The score measures narrative completeness, the recommendations are published, and your co-sell motion — whether an AWS seller ever touches your deal — is decided on that basis, in real time.


Nearly half of all zero-scored referrals we analyzed still had the template in the field. That is now a routing decision, not a paperwork problem.


Three moves close the gap:


1. **Predict the score before submission** , so fixes happen while the deal is still live and the fix is still cheap.
2. **Generate the business problem from CRM data** , so the field that matters most is never the weakest one in the record.
3. **Map the reps independently** , so a motion assignment isn’t your only route to an AWS human.


All three are live in Suger today.


[Book a demo](https://www.suger.io/schedule-demo/) and we’ll run a batch of your existing referrals through our scoring model — you’ll see your distribution, and your likely motion mix, before you change anything.


---


*Source:[AWS Partner Central agents now accelerate co-selling on every deal](https://aws.amazon.com/about-aws/whats-new/2026/06/accelerate-co-selling-with-agents/) , AWS What’s New, June 16, 2026.*
