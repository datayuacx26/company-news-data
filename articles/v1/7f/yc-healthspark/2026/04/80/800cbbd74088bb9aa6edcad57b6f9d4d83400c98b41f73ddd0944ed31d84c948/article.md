---
schema_version: "1.0.0"
document_id: "800cbbd74088bb9aa6edcad57b6f9d4d83400c98b41f73ddd0944ed31d84c948"
company_key: "yc-healthspark"
company: "HealthSpark"
source_id: "yc-healthspark-news-import-606f8b7ac6c4"
canonical_url: "https://joinhealthspark.com/blog/medicare-advantage-card-trap"
published_at: "2026-04-11T00:00:00+00:00"
first_seen_at: "2026-07-25T07:54:14.074127+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:bf35c7ae9b040cd5e2e6ff28e80397831e48326a60f9a1084174b52024c45ad7"
---

# The Medicare Advantage card trap

This article is informational and not legal, compliance, or billing advice. Payer rules and dollar amounts can change — confirm current figures and requirements before billing.


## Why the card alone is not enough


When a patient enrolls in a Medicare Advantage plan, their care is administered by a private insurer — Humana, Aetna, UnitedHealthcare, Blue Shield, Kaiser, and so on. The Medicare Advantage plan replaces Original Medicare as the payer. But patients almost always keep their red-white-blue Medicare card in their wallet, and many do not realize it is no longer the card to present.


If you bill Original Medicare for a patient who is enrolled in Medicare Advantage, the claim will deny. The patient is not going to correct you at intake — they do not know. You have to catch it before you bill.


## What this looks like on your own


Avoiding the Medicare Advantage trap as a solo PT means running the same operational loop on every Medicare patient, every time:


- Run a Medicare eligibility check (HETS) on every new patient who presents a Medicare card, before the first visit
- Read the eligibility response carefully to catch the Medicare Advantage enrollment indicator, plan name, and effective dates
- Verify whether you are credentialed and contracted with the specific Medicare Advantage plan the patient is on — not just "do I take Humana" but "am I in-network with this specific Humana MA plan"
- Re-run eligibility on January 1 every year — patients switch plans during annual enrollment and will not tell you
- Re-run before any resumed care after a gap — enrollment can have changed between episodes
- If the patient is on a Medicare Advantage plan you are not contracted with, stop before scheduling and figure out the path (single case agreement, out-of-network, or redirect)


Every step is manual. Every step is easy to skip when a walk-in patient is standing in front of you waving a Medicare card. Skip it once, and you treat the patient, submit the claim, and discover the problem three weeks later when the denial arrives.


Key takeaway


Never trust the card. Run an eligibility check on every Medicare patient before the first visit. That single step catches the most common denial in PT billing.


## What the eligibility response tells you


A Medicare eligibility lookup returns a Medicare Advantage plan enrollment indicator when the patient is in an Advantage plan. The response includes the name of the Medicare Advantage plan, the effective dates, and contact information for the insurer.


If the response shows active Medicare Advantage enrollment, Original Medicare is not the payer. You bill the Medicare Advantage plan instead — which may require a separate contract, separate credentialing, different authorization rules, and a different fee schedule.


Where PTs get in trouble


- **Skipping the eligibility check on "Medicare" patients.** It feels unnecessary when the card is right there. It is the step that catches the trap.
- **Not re-checking annually.** Medicare Advantage switches happen every January, silently. A patient you billed Original Medicare for in December may be on Humana Medicare Advantage in February.
- **Starting care before confirming contract status.** Catching the enrollment is not enough. You still need to be contracted with that specific plan, or the visit does not pay.


Inside HealthSpark


HealthSpark runs a Medicare eligibility check on every patient at intake, surfaces Medicare Advantage enrollment as a top-level alert, and cross-references the specific MA plan against your active contracts. If the patient is on a plan you are not contracted with, we flag it before the first visit — not after the denial.


## How HealthSpark handles this end-to-end


The Medicare Advantage trap is a workflow problem. We handle the workflow:


- **Eligibility on every Medicare patient at intake:** automatic, before scheduling, before any visit is booked
- **Medicare Advantage detection:** if the patient is enrolled, we surface the plan name, effective dates, and carrier contact info prominently
- **Contract match:** the system cross-references the MA plan against your active HealthSpark contracts and tells you whether the patient can be seen in-network
- **Annual re-verification:** every January, we re-run eligibility on every active Medicare patient to catch enrollment changes
- **Gap-in-care re-verification:** before any resumed care after a pause, we re-check eligibility automatically


You treat the patient. We make sure you are billing the right payer.
