---
schema_version: "1.0.0"
document_id: "6afd2e051ba5b911e572fb33d06d545856c95cab1d6e14de3ead8144e7686658"
company_key: "yc-substrate"
company: "Substrate"
source_id: "yc-substrate-news-import-ed25994f1cce"
canonical_url: "https://www.substrateai.com/blog/introducing-the-substrate-eligibility-agent"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-07-24T02:38:43.425400+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:3b0c4acf4ba14752c919120d571825015ed59195e6a01e8368b0c8483922880b"
---

# Introducing the Substrate Eligibility Agent

### Background


For many health care practices, eligibility-related denials are the most common denial category.[In 2024, 24% of denials were caused by registration and eligibility](https://www.optum.com/content/dam/o4-dam/resources/pdfs/e-books/2024-denials-index-for-providers.pdf) . In our network, we’ve seen practices with eligibility based denials taking up as much as 35% of denials by claim count, and 60% by dollar value. Industry-wide front-end issues (eligibility, registration, prior auth, and coverage) account for roughly half of all denials.


These denials are mostly unrelated to the quality of care delivered, and thankfully, they’re mostly avoidable.


### Front end errors become back end denials


When eligibilty, registration and benefit registration is done correctly, patients get a reasonable estimate of what they owe, and clinicians can provide care knowing they’ll be reimbursed. When done poorly, front end errors cause several problems.


- **Higher revenue leakage:** By Contributing to your overall denials volume, they reduce the amount of claims your billers can touch, and ultimately recover
- **Slower collections:** The more denials you have to work, the slower you get paid.
- **Higher cost to collect:** it can cost an incremental[$25 per claim](https://www.rivethealth.com/blog/denials-revenue-cycle-management) *just to get reimbursed for care you already provided.* This eats into already thin margins.
- **Degraded patient experience:** Increased denials often mean inaccurate or higher patient bills.


‍


### Eligibility based denials are caused by both organization and technology


Org structure can make things worse. Receptionist and front desk teams handling intake and registration rarely report into the same leader as revenue cycle, but front-end errors impact the outcomes that revenue cycle leaders are held accountable for.


Technology also contributes. Today, most eligibility tools tell you whether the patient’s insurance is “valid” and that's as good as it gets. Specific details including the patients specific benefits, their copay and other cost share, and whether they have other insurances, are often left to the front desk to call the payer or check the portal. Often, this means they're just not addressed at all.


Most eligibility products are for front end users. Substrate’s Eligibility Agent is the first built specifically for denials. If your team works denials, spends time on claim status, logs into portals, or calls payers, the Substrate Eligibility Agent is for you .


### Why do Eligibility Related Denials Occur?


Eligibility-related denials are often caused by front end issues during registration. Common examples include:


- The patient's insurance is not valid on that particular date of service.
- Misspellings of a patient name.
- Mistyping of a patient date of birth.
- Mistyping of a patient member ID.
- Not getting a prior auth when one was required for a specific procedure.
- Not getting a PCP referral when one was required for a specific procedure.
- The patient having benefits provided or paid by another plan, not the main plan. Things such as carve-outs
- coordination of benefits issues. Very often patients will have multiple insurances, and you have to submit the claim to the correct insurance first.
- Picking the wrong payer; sometimes you verify eligibility with one Blue, and that is successful, but the claim needs to be submitted to another one


Some payers will pay a claim even when the patient name is misspelled. Others will deny a claim over a missing middle initial.


We built the Substrate Eligibility Agent to solve these problems.


### How The[Substrate Eligibility Agent](https://www.substrateai.com/substrate-eligibility-agent) Works


The[Substrate Eligibility Agent](https://www.substrateai.com/substrate-eligibility-agent) uses the same tools that your front desk or receptionist uses. These include multiple clearinghouses (including yours), multiple API providers and payor portals.


It combines data from a coverage discovery, eligibility response, and claim status, to figure out exactly why a claim was denied.


Finally, unlike most eligibility tools, the Substrate Eligibility Agent is built for denials; it explicitly combines eligibility (270/271) with claim status (276/277), EOBs, and payor portal responses. It can also be deployed during registration or claim creation, but the denials focus is truly unique.


[Substrate's Eligibility Agent](https://www.substrateai.com/substrate-eligibility-agent) is built on the denial patterns of millions of real claims across thousands of payers. It knows that Anthem-CA has strict subscriber ID formatting requirements. It knows that UHC eligibility must be verified with payer ID 87726 but claims might need to be submitted to another plan. It knows these things because it has seen what happens when practices get them wrong.


‍


We start by ingesting several types of context


- Patient provided context including their insurance cards and ID
- Practice context such as specialty, NPIs, Tax Ids
- Encounter details such as date of service, place of service and intended procedures
- Payer context from your system, EDI, APIs Browser Agents, and Payer medical necessity policies


‍


We take these inputs and execute in 3 phases:


- **Payer resolution** : figure out the right payers and payer clearinghouse IDs (eg 87726 for United) to interact with around this claim
- **Patient Eligibility** : extract relevant demographic, payer, plan and benefit details.
- [Claim Status:](https://www.substrateai.com/claim-status) Understand where the claim is in the RCM lifecycle, and if it’s denied, the stated reasons (this goes beyond the 835/EOB - the portals give the agent access to the same data available to a human biller, not just the clearinghouse data).


‍


The agent synthesizes these disparate sources to tell tell why the claim was denied and answer the specific questions that the practice has. With each question we try to answer, we give the biller discrete steps of what to do next to rectify the problem. Some examples (including real screenshots) below:


‍


#### Questions about the Encounter


‍


Is the plan valid on the encounter’s date of service:


‍


Are the intended services included in the patient’s plan benefits *with this specific payer*


#### Questions about the patient


Is this patient the subscriber or dependent, *and was that communicated to the payer on the claim?* In these cases a biller sees a denial reason like this:


“85 = Entity not primary; “


“Entity” sometimes refers to the provider, someteims the patient, sometimes the subscriber and sometimes the payer. In this case by supplementing the claim status response with an eligibility check, it’s able to tell that the patient is actually a dependent on the policy, rather than being the subscriber.


It then outlines the specific discrepancies between what the practice has on file and what the patient has on file


Are any of the patient demographics incorrect? This will capture details like:


- Do we have the right member ID?
- Is the patient's name spelled correctly?


and so on.


‍


What spelling of the patient name does the payer have on file?


What date of birth does the payer have on file for this patient *and is it the same date of birth that the provider has* ?


Is the member id entered correct? Often, but not all the time, this error just indicates that you have the wrong payer entirely.


‍


#### Identifying Coordination of benefits issues


What payer id should eligibility be sent to vs where the claim should be sent?


Is the patient covered by multiple payers, and if so which one is primary?


‍


These are just a few examples of what the eligibility agent can solve for health care practices. There are obviously several more.


‍


### Who is this for?


#### AR teams working Denials


AR teams dealing with high denial volumes, or spikes in denials can use the Substrate Eligibility Agent to


- Categorize denials.
- Do all the research required to solve and hand off a fully researched claim to billers to re-bill.
- Touch every single claim, including the low-dollar ones, so that revenue can be recovered instead of written off.


‍


#### Physician Group Leaders


If you run a physician group, either as the CEO, CFO, or COO, or managing director, you have an extremely vested interest in getting this problem right. Eligibility-based denials are high frequency, they clog your queues and worklists and take time to remediate. The Substrate Eligibility Agent compresses all that research for your team, does it instantly, and returns an answer directly into your System of Record. Using this agent increases the net revenue you recover and the leakage you recapture, without expanding headcount.


‍


#### Healthcare Finance Leaders & Health System CFOs


As a health care finance leader, if you had a tool that could shrink your bad debt accrual, would you use it? You're staring down a ton of headwinds to revenue, across revenue leakage, speed to collect, and increasing cost to collect. The Substrate Eligibility Agent can help drive net revenue recovery, recapture leakage *and save* on cost to collect.


‍


On a $400M practice leaking 5% of net revenue, recapturing just 2 points is roughly $8M of recurring revenue. Faster collections compounds the effect: a practice collecting ~$1.1M/day that takes three days out of its AR feels a one-time cash improvement of ~$3.3M to the balance sheet (which also improves the working capital position of the business). The Substrate Eligibility Agent is built to drive exactly these outcomes; shrinking bad debt accrual, accelerating cash, and recapturing leakage, so the project pays for itself on the merits.


‍


#### Revenue Cycle Operations Leaders


As a VP of Revenue Cycle Operations, you're responsible for overall collections success rates, but you often don't control the actions, practices, and turnover of the front desk. This tension often means that you're held accountable for mistakes made by teams that are not within your span of control.


‍


The Substrate Eligibility Agent helps you take back control of eligibility-related issues. Instead of your team spending a ton of time checking patient insurance cards, logging into payer portals, or calling payers to check eligibility, the Substrate Eligibility Agent will do all that for you. Your billers will get fully researched claims they can immediately rebill, and you can redeploy your team into higher complexity denials.


‍


#### Getting Started


The Substrate Eligibility Agent is designed to work alongside your team. This means you don't have to go through a massive change management process, move to a new PM, or change your front desk process The Substrate Eligibility Agent uses all of these systems as sources of data, and writes back so your worklists get automatically matriculated, and your billers know just what to do next.


Book a[demo](https://www.substrateai.com/demo?utm_source=eligibility+launch+post+6.24) here!


‍


‍
