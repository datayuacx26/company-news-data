---
schema_version: "1.0.0"
document_id: "05e343e5bffdb071243cfa0f2f2bce3891301d10191db9020d0fcefbef7d2a47"
company_key: "yc-healthspark"
company: "HealthSpark"
source_id: "yc-healthspark-news-import-606f8b7ac6c4"
canonical_url: "https://joinhealthspark.com/blog/era-eft-setup-for-pts"
published_at: "2026-03-22T00:00:00+00:00"
first_seen_at: "2026-07-25T07:54:14.074127+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:66b38cbf59528747c3324afe2f84c8137ff91de4f461a9a8dde5a4daaa9cf11e"
---

# Setting up ERA and EFT: getting paid electronically

This article is informational and not legal, compliance, or billing advice. Payer rules and dollar amounts can change — confirm current figures and requirements before billing.


## What ERA and EFT are, briefly


ERA (Electronic Remittance Advice) is the electronic version of the paper EOB — it tells you what the payer paid, reduced, or denied on each claim. EFT (Electronic Funds Transfer) is the payer depositing the payment directly into your bank account instead of mailing a check.


On paper: check in the mail, EOB in a separate envelope, manual reconciliation, 4-to-6-week lag between visit and cash in hand. On ERA/EFT: money hits the account the same day the remittance file arrives, and posting is automated if your system can read the ERA file.


Key takeaway


Every payer enrolls ERA and EFT separately. There is no single enrollment that covers everyone. Expect 2 to 3 months from zero to having the major payers live — and that is before you start on secondary insurances.


## The reality: every payer is its own enrollment


There is no universal ERA/EFT switch. Every payer has its own enrollment form, its own portal, its own verification process, and its own timeline. Medicare enrolls through one system. Medicaid enrolls through another. Every commercial payer runs its own process. You are filling out the same information 8 or 10 times, in 8 or 10 different formats, for 8 or 10 different processing teams.


Each payer enrollment needs the same building blocks, but you will submit them individually to each:


- The payer's enrollment form (paper, PDF, or portal — all three still exist depending on the payer)
- A voided check or bank letter proving routing and account numbers
- A W-9 matching the tax ID the payer has on file from credentialing
- Pre-note testing — many payers send a zero-dollar test deposit before enabling real EFT, which itself can take a week or two


Expect 2 to 4 weeks per payer from submission to live. From zero, with 6 to 8 major commercial payers plus Medicare and Medicaid, realistic total is 2 to 3 months to be fully set up on primary payers. That assumes nothing errors out.


## Then secondary insurance doubles the work


A large share of your Medicare patients will have a Medicare Supplement (Medigap) plan as secondary insurance — Plan G, Plan F, Plan N, and so on. Those plans are underwritten by independent insurers: AARP/UnitedHealthcare, Mutual of Omaha, Cigna, Humana, Blue Shield, Anthem. Each is a separate carrier. Each requires its own ERA and EFT enrollment.


Medicare crossover sends the secondary claim automatically after primary adjudicates. The problem: if you are not enrolled for ERA/EFT with the Medigap carrier, the secondary payment comes as a paper check weeks later, and you have no electronic remittance to post against. You bill electronically, you receive electronically on primary, and then the supplement payment arrives in an envelope.


Commercial secondary insurance (for example, a patient with BCBS primary and Aetna secondary through a spouse) has the same issue. Two separate enrollments, two separate timelines. A patient with one primary and one secondary effectively doubles your enrollment surface area for that carrier combination.


## What this looks like in practice


You finish enrolling Medicare EFT and feel done. The next patient walks in with a Mutual of Omaha Medigap plan. Their primary Medicare payment comes electronically. Their secondary arrives as a paper check 5 weeks later with a mailed EOB, and you have to manually reconcile both sides for a single episode of care. Multiply by every Medigap carrier your patients happen to have.


Most solo PTs discover the secondary-insurance problem only after they start seeing Medicare patients. By then they have paper checks accumulating from carriers they have never enrolled with, and no workflow for handling them.


Where PTs get in trouble


- **Enrolling only the big names.** You set up the top 5 and call it done. Then patients show up with secondary Medigap carriers, regional plans, and employer-negotiated carriers you have never heard of — each one a new enrollment.
- **Enrolling EFT without ERA.** Money shows up, but without the 835 file you cannot automatically tell what it was for. Always enable both on every payer.
- **Ignoring secondaries on Medicare patients.** You enrolled with Medicare. You assumed you were set. The Medigap supplement payments keep arriving as paper checks for months.
- **Mismatched tax ID or bank info.** If the W-9 or account details do not match what the payer has from credentialing, the enrollment stalls silently. Nobody tells you — you notice when the checks keep coming in the mail.
- **No posting workflow for the 835 files.** ERA files still need to be ingested and posted to patient ledgers. If your system cannot read 835s, the time savings from electronic remittance is lost.


Inside HealthSpark


HealthSpark runs ERA and EFT enrollments across every contracted payer during onboarding, then expands coverage as your patients bring in new Medigap and secondary carriers. The 835 files are ingested and posted automatically. You never fill out an enrollment form or open a payer portal.


## How HealthSpark handles this end-to-end


- **Primary payer enrollment:** ERA and EFT are set up with every contracted payer at onboarding — all in parallel, not one at a time
- **Secondary and Medigap coverage:** when a patient brings in a supplement plan you are not enrolled with, we add that carrier to the queue and handle the enrollment — so the paper-check pile does not form
- **835 ingestion and posting:** remittance files are parsed automatically and posted to the right patient ledger
- **Single consolidated payout:** you receive one twice-monthly payout from HealthSpark, not a dozen payer deposits to track
- **Failure monitoring:** if a payer stalls an enrollment or changes its process, we catch it and re-run — so you do not discover weeks later that something never went live


You see patients. We take every payer portal off your plate.
