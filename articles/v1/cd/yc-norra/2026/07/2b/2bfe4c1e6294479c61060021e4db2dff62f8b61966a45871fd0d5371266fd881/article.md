---
schema_version: "1.0.0"
document_id: "2bfe4c1e6294479c61060021e4db2dff62f8b61966a45871fd0d5371266fd881"
company_key: "yc-norra"
company: "Norra"
source_id: "yc-norra-news-import-9994af1578fd"
canonical_url: "https://www.norra.io/blog/snf-survey-readiness-checklist"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-24T06:25:05.148229+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:67086eeb11276055907a55d8a205b5e13d09c7530a14fa1a31b59a26d6dad6e6"
---

# The Skilled Nursing Survey-Readiness Checklist

Survey readiness in a skilled nursing facility comes down to one thing: being able to prove, on demand, that care happened and that the equipment behind it worked. When a surveyor points at a resident's care plan and asks to see the lift, the specialty mattress, or the bed alarm it names, the facility has minutes, not hours, to produce the item and its history. This checklist walks the equipment, documentation, and location items that surveyors actually probe, maps them to the F-tags they cite, and shows where Norra, the AI equipment manager built for skilled nursing, removes the equipment scramble. Norra does not make a building compliant. It handles one specific slice: keeping durable medical equipment locatable, maintained, and instantly documentable.


## Why survey readiness is a documentation problem


The annual recertification survey is unannounced. That single fact reshapes everything: readiness cannot be a project that spins up when surveyors are rumored to be in the region, because by then the trail either exists or it does not. Surveyors work from the resident's care plan outward. They read what the plan promises, then verify that the promise was kept, that the equipment named is present and working, and that a record proves both. A gap between the plan and the reality on the floor is where a citation, an F-tag, gets written.


Equipment sits at the center of a surprising number of tags. A care plan calling for a mechanical lift, a pressure-redistributing surface, oxygen, or fall precautions is a promise that specific equipment will be at the bedside, in safe condition, on the day a surveyor walks in. If the item is missing, in a back room, or out of service with no record, the clinical requirement fails at the equipment layer. That is why the checklist below leads with equipment and location, and why the two most equipment-relevant tags, F689 and F908, are worth understanding before survey week.


## The two F-tags equipment most often touches


**F689, Free of Accident Hazards, Supervision, and Devices** , maps to[42 CFR 483.25(d)](https://www.ecfr.gov/current/title-42/chapter-IV/subchapter-G/part-483/subpart-B/section-483.25) under the Quality of Care requirements. The regulation requires that the resident environment remain as free of accident hazards as is possible and that each resident receive adequate supervision and assistance devices to prevent accidents. F689 is a broad clinical tag, and most of it is about supervision and care planning that no software touches. But the device half is concrete: a transfer lift, a bed alarm, or a fall mat that a care plan requires and staff cannot locate is precisely the kind of missing assistance device that contributes to a citation here. Norra's role is narrow and honest: it makes sure the device is findable, so the equipment cannot be the reason the tag is cited. The deeper breakdown lives in the[F689 accident hazards guide](https://www.norra.io/blog/f689-accident-hazards-nursing-home-survey) .


**F908, Essential Equipment, Safe Operating Condition** , maps to[42 CFR 483.90(d)(2)](https://www.ecfr.gov/current/title-42/chapter-IV/subchapter-G/part-483/subpart-B/section-483.90) under the Physical Environment requirements. It requires the facility to maintain all mechanical, electrical, and patient-care equipment in safe operating condition. This is the maintenance tag: surveyors expect preventive-maintenance records, manufacturer-recommended service intervals, and proof that a broken item was pulled from service rather than left in circulation. The full requirement is covered in the[F908 essential equipment guide](https://www.norra.io/blog/f908-essential-equipment-maintained-snf) .


Both tag numbers, titles, and citations above are drawn from the current eCFR text and CMS survey guidance in the[State Operations Manual Appendix PP](https://www.cms.gov/medicare/provider-enrollment-and-certification/guidanceforlawsandregulations/nursing-homes) . Verify the current interpretive guidance before your survey, because CMS updates Appendix PP periodically.


## The equipment readiness checklist


Run this on a continuous cadence, not in survey week. Each item is something a surveyor can and does ask to see.


**Locate every safety-critical item.** For each resident whose care plan names equipment, confirm the actual item is at or near the bedside: the lift, the specialty mattress or support surface, the bed alarm, the fall mat, the oxygen concentrator. A named item that lives in a storage bay two units away is a finding waiting to happen. This is the single most common equipment gap, and it is a pure location problem.


**Confirm the crash cart and emergency equipment.** The emergency crash cart, defibrillator, and any facility-required emergency equipment must be present, sealed or checked per your policy, and immediately locatable. Surveyors treat emergency-equipment gaps seriously. A[tracked crash cart](https://www.norra.io/blog/crash-cart-emergency-equipment-tracking-snf) that reports its own room removes the "where is it" question entirely.


**Pull maintenance records on demand.** For each mechanical, electrical, and patient-care item, be able to produce its maintenance history in minutes: last service date, next due date, and any out-of-service events. This is the heart of F908. A binder that requires flipping is a slow answer; a record keyed to the physical item is a fast one. See the guidance on[preventive-maintenance logs for survey compliance](https://www.norra.io/blog/preventive-maintenance-logs-snf-survey-compliance) .


**Remove broken equipment from service.** Any item that failed a check must be visibly tagged out and physically separated from in-use equipment, with a record of when it was removed. A broken lift sitting in a hallway next to working ones is an F908 hazard even if no one intended to use it.


**Reconcile the fleet against the master list.** Confirm that what your inventory says you own matches what is actually in the building. Drift here is what produces duplicate purchases and, worse, a care plan pointing to an item that left the building months ago.


**Account for every rental.** Each rented item should map to a current resident or need. A rental that no one can tie to a live need is both a survey question and a cost leak. The full routine is in the[DME tracking checklist](https://www.norra.io/blog/dme-tracking-checklist-nursing-homes) .


## The location and documentation layer


The checklist above has a common failure mode: it depends on a person walking the building, clipboard in hand, holding a mental map of where everything is. That map is stale the moment care resumes, because equipment follows residents and needs, not fixed homes. A lift moves to a transfer, a mattress is placed on admission and stripped on discharge, a concentrator moves the day an oxygen order changes. By survey week, the walk-around picture and the floor no longer match.


This is the specific problem Norra solves. Every tracked item carries a proprietary smart tag, and plug-in gateways report each item's room automatically, with no staff scanning and no clipboard. The location stays current on its own. When a surveyor asks to see the lift named in a care plan, staff type the item and see its room. When the question is maintenance, the item's record travels with it. The equipment slice of the survey becomes a lookup instead of a scramble, at a fraction of the cost of traditional tracking systems.


Be clear about the boundary. Norra does not ensure compliance, prevent citations, or handle the clinical substance of any tag. It will not treat a pressure ulcer, run an infection-control program, or supervise a resident. What it does is eliminate one recurring, avoidable contributor to equipment-related findings: equipment that is missing, unmaintained, or undocumented when someone needs to see it. Missing and undocumented equipment is a common contributor to citations under tags like F689 and F908, and that specific risk is the one Norra reduces.


## What readiness looks like in practice


A survey-ready facility does not prepare for the survey. It runs the equipment, documentation, and location checks continuously, so the answer to any equipment question already exists. The weekly pass confirms safety-critical items are where their care plans say and that rentals still map to needs. The quarterly reconciliation catches fleet drift before it becomes a duplicate purchase or a phantom item on a care plan. When the surveyors arrive unannounced, nothing changes, because the trail was never allowed to go cold.


Norra was built for exactly this cadence. Proven across a multi-facility skilled nursing network, it has cut equipment spending by as much as 70 percent, driven 90 percent fewer new rental orders, saved over 1,100 staff hours a year, and reached zero unnecessary rentals. Y Combinator-backed and a MatrixCare marketplace partner, it keeps the equipment slice of survey readiness in a permanent ready state, so the surveyor's equipment questions are the easy ones. For the broader survey playbook beyond equipment, see[how to prepare for a state survey in skilled nursing](https://www.norra.io/blog/how-to-prepare-for-state-survey-skilled-nursing) and the guide to[equipment documentation for the nursing home survey](https://www.norra.io/blog/equipment-documentation-nursing-home-survey) .
