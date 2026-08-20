---
schema_version: "1.0.0"
document_id: "3199d52c29bf5ad4097b8a61e5ba13ca628b6441dfbbee2e8391d3f05a57e94a"
company_key: "yc-healthspark"
company: "HealthSpark"
source_id: "yc-healthspark-news-import-606f8b7ac6c4"
canonical_url: "https://joinhealthspark.com/blog/medicare-home-health-check"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-25T07:54:14.074127+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:d85c1ac75e4a1027edb0b203822b987fab21dbb91167e1617ebc4d0fb4d2297b"
---

# The home health overlap: check the episode before you bill Part B

This article is informational and not legal, compliance, or billing advice. Payer rules and dollar amounts can change — confirm current figures and requirements before billing.


## Why this is one of the most expensive mistakes in PT billing


Under Medicare, when a patient is inside an active home health episode, therapy services are covered through the home health benefit — not separately under Part B. The home health agency receives a bundled payment expected to cover the therapy the patient needs during that episode.


If you bill an outpatient Part B visit during the same window, the claim may pay initially and then be clawed back months later when Medicare reconciles the overlap. By the time the takeback arrives, you have treated the patient a dozen more times, each of them at risk.


## What this looks like on your own


Catching home health overlaps as a solo PT means doing all of the following, for every Medicare patient, before every episode of care:


- Run a Medicare eligibility check (HETS) and know exactly where in the response the home health episode data lives
- Read the response carefully enough to notice episode start/end dates — data that is easy to skim past if it is not flagged
- Re-check eligibility monthly (and before resuming after any gap in care), because an episode can open during your course of treatment
- Coordinate with the home health agency if one is active — which means phone calls, coordination of benefits, and sometimes waiting for a discharge before you can start
- Track reconciliation timing so you know a claim that paid today is not going to disappear in three months


The patient cannot help you. Most patients do not know they are in a home health episode. They think the home health agency discharged them weeks ago when, on paper, the episode is still open.


Key takeaway


The claim paying today does not mean the claim is safe. Home health overlaps are often reconciled months later — and the clawback compounds by the time you notice.


## What to do if the patient is in an episode


You have a few clean options. Coordinate with the home health agency so they discharge the episode before you start outpatient PT. Delay the start of outpatient care until the episode ends. Or, if the patient needs care the home health agency is not providing, document the clinical need and understand that Part B billing during the episode still carries denial risk.


If you find out mid-course that the patient was inside an active episode, the conservative move is to identify the affected dates of service, expect recoupment, and hold Part B billing until the episode is closed. Waiting for the clawback is worse than getting ahead of it.


Where PTs get in trouble


- **Trusting what the patient says.** Patients routinely think their home health agency has ended when the episode is technically still open.
- **Checking eligibility only once.** An episode can start mid-care. A pre-visit re-verification catches this before a second clawback.
- **Missing the overlap in the eligibility response.** Episode data is in the response but is easy to skim past when it is not specifically flagged.


Inside HealthSpark


HealthSpark runs a Medicare eligibility check on every patient at onboarding and re-runs it on a rolling basis during care. Active home health episodes are surfaced as a red flag on the patient record, not buried in the response text — so you see the conflict before you bill, not after the clawback.


## How HealthSpark handles this end-to-end


The home health trap is a visibility problem. We solve it by handling the visibility for you:


- **Automated eligibility at intake:** every new Medicare patient gets a HETS check before the first visit
- **Rolling re-verification:** eligibility is re-checked on a schedule and before any care resumes after a gap, so mid-course episode openings do not slip through
- **Home health episode flags:** active episodes are surfaced as a prominent alert on the patient record, not hidden in raw response text
- **Pre-claim block:** if an episode is active, the system holds Part B claims and prompts you to coordinate before submitting — so you never send a claim that is guaranteed to be clawed back
- **Coordination prompts:** when an episode needs to close before your care begins, we give you the contact path and track the discharge


The patient cannot tell you they are in an episode. Medicare will not warn you. We do.
