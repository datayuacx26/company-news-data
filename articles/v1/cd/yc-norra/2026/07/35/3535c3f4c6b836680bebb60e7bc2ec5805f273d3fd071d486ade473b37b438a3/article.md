---
schema_version: "1.0.0"
document_id: "3535c3f4c6b836680bebb60e7bc2ec5805f273d3fd071d486ade473b37b438a3"
company_key: "yc-norra"
company: "Norra"
source_id: "yc-norra-news-import-9994af1578fd"
canonical_url: "https://www.norra.io/blog/f684-quality-of-care-equipment-nursing-home"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-30T16:03:26.361910+00:00"
fetched_at: "2026-07-30T16:03:28.316080+00:00"
content_hash: "sha256:b5901fe5e29bcbf52fa76fb9950195ffbe5b7bd5f0e5633d079964357b006dd1"
---

# F684 Quality of Care: Where Equipment Availability Fits

F684 is the broad Quality of Care tag under[42 CFR 483.25](https://www.law.cornell.edu/cfr/text/42/483.25) , the federal standard that each resident receives the treatment and care needed to attain or maintain the highest practicable physical, mental, and psychosocial well-being. It is a wide tag. Equipment is not the substance of it, but equipment that is missing, broken, or undocumented is a common contributor to findings under it.


That distinction is the whole point of this guide. F684 is a clinical and care-planning standard, and no software makes a facility compliant with it. What a facility can do is remove the recurring equipment problems that sit underneath quality-of-care findings: the ordered device that no one can locate, the unit in use with no maintenance record, the care plan that names a surface the building cannot produce on the survey day. Norra, the AI equipment manager built for skilled nursing, addresses that narrow slice and nothing beyond it.


## What F684 actually covers


Under CMS guidance in the State Operations Manual Appendix PP, F684 is the F-tag for the general Quality of Care requirement at 42 CFR 483.25. The regulation opens by stating that quality of care is a fundamental principle that applies to all treatment and care provided to facility residents, based on the comprehensive assessment and plan of care and consistent with resident choices.


Section 483.25 is a large regulation. Alongside the general principle that F684 carries, it contains numbered subsections that each address a specific care area, and several of those have their own dedicated F-tags. In practice a surveyor cites the specific tag when a concern maps cleanly to a named subsection, and cites F684 for the general quality-of-care principle or for care areas that do not have a dedicated tag of their own. That is why F684 reads as broad: it is the catch-all for the standard as a whole.


Because it is broad, F684 can attach to a wide range of care failures. A single citation might rest on the care a resident did or did not receive, the assessment behind it, whether the plan was followed, and whether the record supports the story. Equipment is one thread inside that, never the whole cloth.


You can read the full regulation at the[Cornell Legal Information Institute mirror of 42 CFR 483.25](https://www.law.cornell.edu/cfr/text/42/483.25) , and the surveyor guidance in the[CMS State Operations Manual Appendix PP](https://www.cms.gov/Regulations-and-Guidance/Guidance/Manuals/downloads/som107ap_pp_guidelines_ltcf.pdf) .


## Where equipment fits, and where it does not


Delivering the care that F684 requires often depends on a specific device being present, working, and appropriate to the resident. A safe transfer needs a functioning lift and the correct sling. A pressure-injury prevention plan needs the ordered specialty surface actually on the bed. A respiratory order needs the right unit, maintained and running. When the plan calls for a device and the device is there and working, equipment is invisible. It becomes visible only when it fails.


Here is the honest boundary. Norra does not assess a resident, write a care plan, deliver a treatment, or supervise care. Those are the substance of F684, and they belong to the clinical team. What Norra does is make sure the equipment that the care plan depends on is locatable, that its maintenance history exists, and that both can be produced on demand. That is a supporting role, and stating it that plainly is the point: a vendor that claims to keep you compliant with a clinical tag is overreaching.


## The equipment failure modes that surface under F684


Across a survey, a handful of equipment patterns recur underneath quality-of-care questions:


- **The ordered device that cannot be found.** A care plan specifies a lift, a specialty mattress, or a positioning device, and on the survey day no one can produce it or show where it is. The care question is real, and the missing equipment is what makes it concrete.
- **The unmet need behind a missing device.** A transfer, a turn, or a positioning task did not happen the way the plan required because the equipment was not available at the moment of care.
- **The undocumented unit.** A device was in use, but there is no location history and no maintenance record to support that it was present and functioning. Absence of evidence becomes the finding.
- **The device that is present but not maintained.** Equipment that is technically in the building but overdue for service or visibly out of order undercuts the care it was supposed to support.


Every one of these is an availability or documentation gap. None of them is a clinical judgment. That is precisely the layer Norra is built to close.


## What Norra does about the equipment slice


Norra puts a proprietary smart tag on each piece of durable medical equipment and reports its room automatically through plug-in gateways. Staff never scan anything. The result is a live, room-level record of where every lift, bed, specialty surface, and respiratory unit is right now, plus the history of where it has been.


For a broad tag like F684, that record does three specific things. It answers "where is the device this care plan depends on" in seconds instead of a hallway search. It preserves a location and status history, so "was this unit present and in service" is a lookup rather than a reconstruction. And it surfaces equipment that has gone missing or idle before it becomes the gap a surveyor names. Norra reduces the equipment-availability and documentation risk that contributes to F684 findings. It does not, and cannot, deliver the care the tag is really about.


Capability Manual log or spreadsheet Scan-based system Norra


Locate an ordered device on the survey day Slow, often stale Only if every move was scanned Live room-level location


Produce a device's location history Rarely kept Gaps wherever a scan was skipped Complete automatic history


Maintenance record tied to the unit Separate binder, easily lost Separate system Attached to the item record


Staff effort to stay current High, ongoing High, every move None, automatic


Catch a missing or idle unit early Only at the next audit Only if noticed Flagged continuously


## Turning the equipment answer into a survey answer


The value of a live equipment record shows up most on survey week, when the question shifts from "is care good" to "prove it." For the equipment slice of an F684 concern, proof means producing a device and its history on the spot. A facility running Norra can pull up the exact room a unit is in, its recent movement, and its maintenance status without pulling a nurse off the floor to go looking. The equipment-scramble that usually precedes a survey turns into a lookup. For the broader documentation discipline this supports, see[equipment documentation for a nursing home survey](https://www.norra.io/blog/equipment-documentation-nursing-home-survey) .


Because 42 CFR 483.25 spans many care areas, the same equipment discipline helps across the neighboring tags too. The accident-and-hazard standard has its own dedicated tag, covered in[F689 accident hazards](https://www.norra.io/blog/f689-accident-hazards-nursing-home-survey) , and respiratory care has its own, covered in[F695 respiratory care equipment](https://www.norra.io/blog/f695-respiratory-care-equipment-nursing-home) . If you want the wider survey picture,[the most-cited F-tags in nursing homes](https://www.norra.io/blog/most-cited-f-tags-nursing-homes-2026) shows where quality-of-care findings land relative to everything else. Across all of them, the equipment question is identical: the right device, present, working, and documented.


## The honest summary


F684 is a broad quality-of-care standard, and compliance with it rests on clinical care, assessment, and documentation that no tracking tool provides. Missing, unmaintained, or undocumented equipment is one recurring contributor to findings under it, and that specific risk is the one Norra reduces. Norra keeps durable medical equipment locatable, maintained, and instantly documentable, and it does that at a fraction of the cost of traditional tracking. Proven across a multi-facility skilled nursing network, Norra is Y Combinator-backed and a MatrixCare marketplace partner. It is the equipment layer under your quality-of-care work, not a substitute for it.
