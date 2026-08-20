---
schema_version: "1.0.0"
document_id: "afe02c551bbbbc6b4fc92e3e83a0bd9f6e11c9baedf04b1aa3e5545bcb65015e"
company_key: "yc-centaur"
company: "Centaur"
source_id: "yc-centaur-news-import-7ab94b327191"
canonical_url: "https://centaur.ai/post/your-annotation-is-good-enough-for-r-d-but-not-for-the-fda"
published_at: "2026-03-20T04:42:38.540+00:00"
first_seen_at: "2026-07-27T00:36:30.031492+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:0be76a18c0c8aa4100711cb4bae65b8f329ff3cb0c96f5609610889d1f1253e1"
---

# Your Annotation is Good Enough for R&D, but not for the FDA.

Most teams building medical AI have robust annotation pipelines, credentialed reviewers, and real scientific standards. And for model development, that works. But the annotation built for iteration has a specific blind spot when it comes to regulatory defense, and it shows up in one place: what happens when your annotators disagree.
‍


In most enterprise workflows, disagreements get resolved with a tiebreaker. A senior clinician picks one answer, the conflicting label disappears, and everyone moves on. totally fine for R&D.
‍


The FDA sees that differently. When they review your training data, they're asking how you arrived at your ground truth labels. What was the consensus methodology? Who participated? How was disagreement documented and resolved? If the answer is "a senior person picked one," you've essentially let one person's opinion override the others and quietly deleted the disagreement. That doesn't read as a consensus to a reviewer. It reads as a shortcut.
‍


This is where enterprise annotation breaks. The annotators are qualified, and the labels are probably correct, but the resolution process was never built to be auditable. The FDA doesn't need to prove your labels are bad. They just need to ask how you got them and not like the answer.
‍


What holds up under that scrutiny looks like multi-expert consensus with documented methodology, credentialed reviewers whose qualifications map to the clinical domain, and annotation provenance that traces every label back to who created it and how disagreements got resolved. Most teams already have pieces of this. The gap is usually in how disagreement resolution gets recorded, if it gets recorded at all.
‍


What doesn't hold up is the stuff that's still standard at many companies: single-annotator pipelines, non-credentialed workforces, tiebreaker workflows where one person's judgment quietly becomes your ground truth. These are R&D tools applied to a regulatory problem.
‍


At Centaur.ai, we've now contributed to 10 FDA clearances across 6 companies, and a few of those companies have come back for second and third submissions. The thing that held across all of them is that consensus methodology and annotation provenance were treated as regulatory infrastructure from day one, not patched in before submission.
‍


If you're building toward a submission and your annotation workflow still resolves disagreement by escalating to one person, that's the conversation worth having.


‍
For a demonstration of how Centaur can facilitate your AI model training and evaluation with greater accuracy, scalability, and value, click here:[https://centaur.ai/demo.](https://info.centaurlabs.com/demo)
