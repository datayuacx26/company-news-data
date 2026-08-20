---
schema_version: "1.0.0"
document_id: "200f14aad62ce30e64517a7766961fd64a5fabe63fa4e9ab2228a764e420f2e8"
company_key: "yc-fresco"
company: "Fresco"
source_id: "yc-fresco-news-import-11bc7e0494a7"
canonical_url: "https://fresco.build/blog/sfh-export-workflow"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-25T05:48:27.827048+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:d906b429fe1d67377f5809b416bd09c6d872b0c90e105d3bd20f773933e64da3"
---

# SFH export workflow for Division 8 estimators

Workflow


# SFH export workflow for Division 8 estimators


July 10, 2026 · 4 min read


The SFH export is the last step people notice, but it is not the hard step. The hard step is getting every opening clean enough that the import does not send the estimator back into the drawings.


Fresco treats the


[SFH workflow](https://fresco.build/sfh-workflow) as a handoff from reviewed takeoff data into the quoting system. That means the estimator should not be exporting a raw door schedule. They should be exporting a cleaned-up set of openings with door material, frame material, hardware set, handing, ratings, and frame context already reviewed.


## Start with the fields SFH will reject


Door and frame material are the first two fields to check. If either one is blank, the SFH import workflow can reject the opening or force cleanup after import. Either outcome wastes the point of the export.


This is why Fresco warns on missing door or frame material before the SFH .txt file is downloaded. The warning is not a cosmetic validation message. It is the product telling the estimator: fix this while the schedule, plan, and spec evidence are still close by.


- Door material and door material category.
- Frame material and frame material category.
- Opening type, hand, pair conditions, and interior/exterior call.
- Hardware set reference and any schedule-to-spec conflict.
- Jamb depth, frame gauge, face dimensions, labels, manufacturer, and series where they affect price.


## Do not use the export as the review step


A clean


[Division 8 takeoff](https://fresco.build/division-8-takeoff-software) should be reviewed before it leaves the takeoff environment. Once the file is inside the quoting system, the estimator can still fix rows, but they have lost some of the source context that made the decision easier.


The better sequence is boring and it works: resolve the obvious exceptions, fix missing material fields, check the high-dollar openings, then export. If the project has addenda, repeat the review before sending a fresh file downstream.


## What Fresco should make easier


Fresco is useful here because the SFH handoff is downstream of a door, frame, and hardware review surface. The estimator can look at the opening record, source schedule, plan context, and hardware set evidence before deciding what should be carried into the export.


- Find openings that are missing required door or frame material.
- Keep hardware set review tied to the opening instead of a disconnected note.
- Preserve frame and door attributes that matter when pricing in SFH.
- Export an SFH / AAOS-compatible tab-delimited .txt file after review.


## What the estimator still owns


The estimator still owns the bid. No export should be treated as a sign-off on rated openings, electrified hardware, aluminum conditions, pairs, borrowed lites, weird elevations, or anything the architect contradicted three sheets later.


That is not a limitation of SFH or Fresco. It is the reality of Division 8 estimating. Software can shorten the search and keep the evidence organized. It should not hide the judgment calls.


## A practical SFH handoff sequence


- Review missing doors, duplicate plan tags, and schedule rows that do not appear on the plans.
- Resolve door and frame material blanks before export.
- Spot-check hardware sets, ratings, pairs, exterior doors, and unusual frame conditions.
- Export the SFH-compatible tab-delimited .txt file from Fresco.
- Import into SFH, then spot-check the rows the estimator already knew were risky.


## When not to push the file yet


Do not push the export just because the button is available. If the


[door frame hardware estimate](https://fresco.build/door-frame-hardware-estimating-software) still has unresolved material blanks, unexplained hardware set conflicts, or rated openings with weak source evidence, the import will only move the cleanup to a worse place.


The best SFH file is not the fastest file. It is the one where the estimator already knows which openings are clean, which openings are qualified, and which openings still need a judgment call.


## Frequently Asked Questions


### Does Fresco replace SFH?


No. Fresco sits upstream of SFH. It helps prepare and review the takeoff before exporting data into the SFH quoting workflow.


### What file does Fresco export for SFH?


Fresco exports an SFH / AAOS-compatible tab-delimited .txt file for reviewed opening and hardware data.


### Why does Fresco warn about missing material before SFH export?


Because SFH needs door and frame material on openings. Missing material is one of the fastest ways to turn an export into import cleanup.


### Should I still review the imported file in SFH?


Yes. The export should reduce re-entry, not remove estimator review. Spot-check high-risk openings after import.


### Is the SFH workflow only about doors?


No. The handoff depends on door, frame, and hardware context staying attached to each opening.


See what Fresco can do on your next project.


[Get a free takeoff](https://fresco.build/)
