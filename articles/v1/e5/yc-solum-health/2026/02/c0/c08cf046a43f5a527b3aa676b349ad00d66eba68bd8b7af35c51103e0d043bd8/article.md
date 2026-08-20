---
schema_version: "1.0.0"
document_id: "c08cf046a43f5a527b3aa676b349ad00d66eba68bd8b7af35c51103e0d043bd8"
company_key: "yc-solum-health"
company: "Solum Health"
source_id: "yc-solum-health-news-import-cf3e7d0d0486"
canonical_url: "https://getsolum.com/blog/medicare-eligibility-verification-2026"
published_at: "2026-02-18T21:37:37.788+00:00"
first_seen_at: "2026-08-10T03:24:35.281168+00:00"
fetched_at: "2026-08-10T03:24:36.479787+00:00"
content_hash: "sha256:94ffb20642898e0cad304d2df816ae52f8c1ef8d7348848ff09f4007f3fdef33"
---

# Your Medicare eligibility checks could go dark this spring

[Home](https://getsolum.com/) /


[Blog](https://getsolum.com/blog/) /


Your Medicare eligibility checks could go dark this spring


Practice Management


# Your Medicare eligibility checks could go dark this spring


2026-02-18


LinkedInTwitter


I've spent enough time wandering through clinic front offices to know which tasks barely register on the staff's radar, and checking a patient's Medicare eligibility is one of them; click a button, wait a beat, see the green checkmark. Done. Move on to the next chart.


That reflex is about to break.


CMS is overhauling the system that makes those quick[eligibility checks](https://getsolum.com/glossary/eligibility-verification) possible, and unless your practice takes a specific administrative step before the spring 2026 cutover, the green light stops appearing altogether. Your software won't return errors you can troubleshoot, it just won't return anything useful at all.


What follows is a concrete walkthrough of what changed, which forms need filing, who's authorized to sign them, and what breaks if you don't act. Because fixing a[denied claim](https://getsolum.com/glossary/claim-denial) costs an average of $57.23 per occurrence, and I'd rather you spent that money somewhere other than paperwork rework.


## The quiet shift from vendor-backed access to provider-level attestation


Until now, most practices accessed Medicare eligibility data through a comfortably invisible arrangement. Your[EHR](https://getsolum.com/glossary/electronic-medical-record-emr-systems) vendor or clearinghouse held a trading partner agreement with CMS, and your practice rode along under that umbrella.


That arrangement is ending. CMS is migrating the HIPAA Eligibility Transaction System (HETS) to a new trading partner management model. Under this model, each individual[NPI](https://getsolum.com/glossary/npi-national-provider-identifier) must be explicitly linked to its vendor's Unique Identifier (UID) through a formal EDI enrollment filed with your regional Medicare Administrative Contractor (MAC).


Your clearinghouse can no longer vouch for you behind the scenes, you have to vouch for yourself.


Without active HETS EDI enrollment, your eligibility queries will return "unauthorized" errors. Front desk can't confirm whether a Medicare patient has active coverage, and if your practice handles even moderate Medicare volume, that disruption hits scheduling, intake, billing, and collections within days.


## Who can actually sign this thing?


Enrollment is handled through whichever MAC processes your Medicare claims. The steps are the same regardless of which MAC you use:


**Figure out your MAC jurisdiction.** Check your most recent Medicare remittance advice or call CMS, the common jurisdictions include NGS (J6/JK), Noridian (JE/JF), Palmetto GBA (JJ/JM), and CGS (J15).


**Get your vendor's Unique ID.** Every clearinghouse and EHR system that transmits eligibility queries through HETS has a four-character UID assigned by CMS, therefore ask your vendor for it today.


**Log into your MAC's EDI portal** and find the HETS attestation section. You'll need your NPI, your Provider Transaction Access Number (PTAN), and the vendor UID.


**The Authorized Official named in PECOS must electronically accept the HETS Rules of Behavior.** This formally links your NPI to the vendor's UID.


**Save your confirmation.** Standard processing runs about five business days. If you use multiple vendors, you'll need a separate attestation for each NPI-to-UID relationship (up to 25 entries per form submission).


## What actually breaks if you miss this


When CMS completes the migration, any NPI without an active HETS EDI enrollment loses the ability to submit eligibility queries. That's it.


Your practice management system won't return coverage data, instead, it will return errors. Front desk staff either wave patients through without verification (risking denials later) or halt intake until someone figures out what went wrong. One recent survey found 41% of providers now report more than one in ten claims denied, with intake errors ranking as the third most common cause.


Manual workarounds exist, but they're brutal. Portal lookups take 15 to 20 minutes per patient, don't feed data back into your billing system, and for a practice seeing 30+ Medicare patients daily, that's hours of productivity gone. API-driven[automated checks](https://getsolum.com/glossary/automated-eligibility-check) finish in seconds.


The average cost to adjudicate a single denied claim hit $57.23, with providers needing an average of three rounds of review per insurer, each taking 45 to 60 days. Preventing eligibility-related denials at the front door is cheaper than fighting them on the back end by a wide margin.


## The MBI wrinkle your front desk should know


By 2026, Medicare Beneficiary Identifiers (MBIs) are the only accepted patient identifier for eligibility transactions. Old Social Security Number-based Health Insurance Claim Numbers are no longer valid in HETS.


If a patient presents an older Medicare card, staff need to look up the current MBI before running any eligibility check. What you need is to build MBI verification into your standard intake checklist. It's cheap insurance against expensive disruptions.


## Act now, not in May


Start with your PECOS records. Confirm your Authorized Official is current and all practice NPIs have accurate contact information. Then, contact every vendor that submits eligibility queries on your behalf and request their HETS UID in writing. If a vendor can't provide one, that's a red flag worth investigating.


Complete the MAC portal attestation for each NPI-to-UID pairing now. Processing delays, PECOS corrections, and vendor coordination can stretch a one-week task into a multi-week project. Don't wait until the deadline is breathing down your neck.


#### JP Montoya


Founder & CEO, Solum Health · Forbes Technology Council Member · Speaker on Healthcare AI


JP Montoya builds and scales healthcare administrative automation at Solum Health, working with ABA, PT, ST/OT, and other therapy practices across 40+ states. A Forbes Technology Council member and speaker on healthcare AI, he writes about healthcare operations, AI implementation, and practice management from direct experience building and running clinical workflows.
