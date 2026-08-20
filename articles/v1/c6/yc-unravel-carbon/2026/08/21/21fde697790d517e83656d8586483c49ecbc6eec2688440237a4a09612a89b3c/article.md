---
schema_version: "1.0.0"
document_id: "21fde697790d517e83656d8586483c49ecbc6eec2688440237a4a09612a89b3c"
company_key: "yc-unravel-carbon"
company: "Unravel Carbon"
source_id: "yc-unravel-carbon-news-import-f0c750fc3a29"
canonical_url: "https://www.unravelcarbon.com/blog/why-a-correct-emissions-number-can-still-fail-an-audit"
published_at: null
first_seen_at: "2026-08-12T16:10:57.044516+00:00"
fetched_at: "2026-08-12T16:10:58.002039+00:00"
content_hash: "sha256:2595413c312f0ce4ab0fd6e82136512295bf004112282c51e870186bbb4fc2ea"
---

# Why a Correct Emissions Number Can Still Fail an Audit

A number can be accurate and still be indefensible. Most teams don't learn this until an assurance provider asks for the evidence behind a figure and finds there isn't any.


‍


Scope 1 and 2 are usually the easier part. Direct emissions and purchased energy are bounded and well documented. Scope 3 is where things tend to genuinely break down. The emissions tied to your supply chain sit scattered across a consultant's annual spreadsheet, a procurement system built for something else, and whoever remembers where last year's file went.


‍


## What makes it hard to keep emissions reporting audit-ready?


An audit doesn't only test whether your number is right. Auditors do recalculate and sample for correctness. The harder test, and the one manual processes fail, is whether you can prove the number is right on demand, months after you calculated it.


‍


## The evidence chain behind every figure


Behind any single emissions figure, an assurance provider is looking for a complete chain:


- The raw activity data it was calculated from
- The specific version of the emission factor applied, not just the factor, the version
- The unit conversion methodology used to get from raw data to reportable figure
- Sign-off: who reviewed and approved the number before it entered the report


Miss any one link and the number, however accurate, has no chain of custody. Without the full chain it stays an assertion, and an assurance provider can't record it as a finding.


‍


## When a spreadsheet is actually enough


Sometimes it is. If you have a small footprint, one owner, one reporting cycle, and no external assurance requirement, a disciplined manual setup works. That setup is two linked sheets:


- An activity log, one row per calculation, referencing (not copying) a specific factor from the second sheet. E.g. "1,500 litres of diesel, Jakarta fleet, March 2026 → factor DEF-042"
- An emission factors sheet, each factor with its value, version, effective date, and what it superseded. E.g. "DEF-042: diesel, 2.68 kgCO₂e/litre, DEFRA 2025, effective Apr 2025, supersedes DEF-031"


Every number in the activity log traces back to one specific, dated row in the factors sheet. Done rigorously, that's a real evidence chain. You could run this and pass assurance.


‍


The design holds up fine. What fails is the assumption behind it, that the conditions it needs will stay stable inside a real organization.


‍


## Why do companies struggle with manual carbon accounting workflows?


Three structural points, none of which require anyone to make a mistake:


1. **Sourcing is fragmented across systems that don't talk to each other.** Someone reconciles procurement platforms, travel booking tools, and supplier surveys by hand, and the reconciliation logic lives only in that person's head. When they change teams, the logic leaves with them.


1. **Factor references decay silently.** The factors sheet gets its 2026 update, and someone overwrites DEF-042 in place instead of adding a DEF-043. Every historical figure pointing to that row now means something different, with no record of what it used to say. Nothing looks broken, and nothing is, until someone asks a question the sheet can no longer answer.


1. **Ownership sits outside the company.** When an external consultant builds the report, the internal team can explain the conclusions but can't independently reproduce them. An assurance review tests whether you can rebuild the figure from source. Being able to describe it doesn't count.


One of our customers, one of the world's largest travel groups, ran into exactly this. Years of consultant-produced annual reports had been perfectly workable. The numbers were fine and nobody asked hard questions. Then ASRS reporting came into scope, and mandatory disclosure with assurance attached changed what "workable" meant. The data wasn't live, updates happened once a year, and the sustainability team could explain the conclusions without being able to independently rebuild any figure from source. The numbers were accurate and indefensible at the same time, and for the first time someone was required to check.


‍


The annual reporting cycle compounds all three. A figure calculated in March, using a factor someone else updated in September, reviewed by a team that's had turnover since, becomes something nobody can fully reconstruct by November. Correctness decays into unverifiability just by sitting still.


‍


## Is the fix more headcount?


The instinct is to treat this as understaffing, so you add analysts and close the gap. From what we consistently hear from teams running Scope 3 manually, and we're a software company so weigh that accordingly, effort isn't the constraint. Teams already spend significant time on reconciliation and still aren't confident the output would survive scrutiny. More people working in an unversioned system just produce unverifiable numbers faster. The failure is structural, and adding capacity doesn't touch it.


‍


## Quick summary


- Assurance tests both correctness and traceability. A spreadsheet only reliably solves for correctness.
- The evidence chain behind any figure: raw activity data, emission factor version, conversion methodology, sign-off. One missing link breaks it.
- A disciplined manual process works for one owner over one cycle. It doesn't survive ownership changes, in-place factor edits, and multi-team handoffs over years.
- Manual workflows fail at three structural points: fragmented sourcing, silent factor decay, and external ownership. Effort isn't where they fail.


‍


**How Unravel Carbon approaches this:** the platform attaches the evidence chain at the moment each number enters the system. Source document, factor version, conversion, and approval get captured together, so the trail exists before anyone asks and nobody has to reconstruct it under deadline pressure. When an assurance provider reviews your report, they can trace each figure directly instead of working around a static export. You walk into the review with the proof already in hand.


‍
