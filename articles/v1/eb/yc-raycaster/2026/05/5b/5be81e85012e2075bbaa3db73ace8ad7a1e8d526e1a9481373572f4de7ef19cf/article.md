---
schema_version: "1.0.0"
document_id: "5be81e85012e2075bbaa3db73ace8ad7a1e8d526e1a9481373572f4de7ef19cf"
company_key: "yc-raycaster"
company: "Raycaster"
source_id: "yc-raycaster-news-import-a1693d0ab1b3"
canonical_url: "https://www.raycaster.ai/blog/approved-as-noted"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-24T10:29:25.527103+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:865549641d57dae02e6a7b93dc736c7ef0b93aa7008cb2b0005e1fbf0892c170"
---

# The Most Important AI Object in Construction Is the Disposition Code

“Approved as Noted” looks like a small status label. It is not. On an EPC project it is the moment technical judgment becomes an operational instruction — whether procurement releases the purchase order, whether the vendor begins fabrication, whether the crew shows up on Monday.


The codes themselves are older than any software. Oracle Primavera's submittal documentation lists[Approve, Approve as Noted, Reject, and Revision Needed](https://docs.oracle.com/cd/E80480_01/help/en/user/113127.htm) as standard review statuses. Most EPC contractors run a close variant: A, B, C, D. The codes survive into the audit binder where, eventually, the regulator will look. They are the durable language of the work.


They are also where most AI document tools are at their weakest.


## Four codes that move a project


Each code carries a downstream meaning that is not negotiable. A reads to procurement as “release.” B reads to the vendor as “you may proceed; close these specific notes in the next revision.” C reads to the field as “wait.” D reads to the project as “this approach is not the approach.” “Approved as Noted” — the B in most schemes — sits in the consequential middle: work may proceed, but the comments matter, and the project is now exposed to whichever comments don't get incorporated.


That middle is where the schedule risk hides. It is also where AI is asked to be most precise.


## A summary is not a disposition


Large language models are good at producing plausible review summaries — sentences like “the document generally aligns with the project requirements, with several areas requiring clarification.” That may be true. It is also useless to the project. A reviewer cannot route a $2B refinery package on prose like that. The questions she has to answer are sharper: which comments are blocking, which are minor, which must be incorporated before work proceeds, which can be closed in the next revision, which finding maps to which clause, drawing, or requirement — and does this package, on balance, deserve A, B, C, or D?


Until an AI system can answer those, “helpful summary” is not a workflow. It is a polite distraction. The disposition is the bridge between evidence and action, and if the AI can't carry the evidence across that bridge, the human reviewer ends up doing the work twice.


## The mismatch failure


The highest-risk failure of an AI review system is not the spectacular hallucination. It is the subtle mismatch between the comments and the disposition.


- The AI flags a missing acceptance criterion and recommends approval anyway.
- The AI recommends revise-and-resubmit on a stack of formatting comments.
- The AI marks “Approved as Noted” without distinguishing which notes must be incorporated before fabrication.
- The AI calls a clause “non-compliant” but cannot cite the governing standard.


None of those are wording errors. They are workflow errors. A reviewer who catches them on the first pass learns, correctly, that the AI is not yet trustworthy for the decision — and goes back to doing the review by hand.


## Model the judgment


The product implication is that the objects of the review must be explicit, named, and inspectable. At Raycaster we treat them that way: action items with a title, a description, a location, a quoted excerpt, and a severity. Citations that point back to the source document and the governing guideline. Annotations placed on the file. Recommended actions the vendor can take. Disposition codes — A, B, C, D — that carry the same meaning the project already gives them.


The reviewer still owns the final call. The system presents the evidence in a form she can trust at a glance and challenge in detail. That is the real usability bar for AI in construction document review. Not “can it summarize the file?” but “can it help me make the right call, mark it up, and send it back?”


The future of AI in EPC is not a smarter chatbot. It is a better review station — one that takes the disposition as seriously as the project already does.
