---
schema_version: "1.0.0"
document_id: "49dd658b81c9a8ae7df3100dbc51080b4b122be82d430f94bba6a699a4848710"
company_key: "yc-raycaster"
company: "Raycaster"
source_id: "yc-raycaster-news-import-a1693d0ab1b3"
canonical_url: "https://www.raycaster.ai/blog/document-review-is-not-search"
published_at: "2026-05-23T00:00:00+00:00"
first_seen_at: "2026-07-24T10:29:25.527103+00:00"
fetched_at: "2026-07-28T22:12:51.498055+00:00"
content_hash: "sha256:29222f47c1e75243217cc4a062e3a21b2a03285fb19d63b070eba2f71cc6c40c"
---

# EPC Document Review Is Not a Search Problem

On April 2, 2026,[Trimble announced it was acquiring Document Crunch](https://www.nasdaq.com/press-release/trimble-acquire-document-crunch-add-ai-powered-risk-management-and-document) , the construction-AI startup best known for reading contracts and submittals against project specifications. Forty-seven days later, on May 19, Trunk Tools[shipped a Forma integration](https://www.globenewswire.com/news-release/2026/05/19/3297911/0/en/Trunk-Tools-Enhances-Jobsite-AI-Capabilities-with-new-Autodesk-Forma-Integration.html) that plugs TrunkSubmittal and TrunkReview directly into Autodesk's new cloud platform. Inside seven weeks, the two most visible “AI for construction documents” companies were either inside an AECO incumbent or distributed by one.


That category — automatic spec-versus-submittal review for general contractors — is closing. The EPC version of the problem is different, and it isn't closed yet.


An EPC discipline engineer doesn't check whether a vendor's shop drawing complies with a contract. She checks whether the vendor's pressure-test procedure satisfies ASME B31.3, the project's welding specification, the purchase order's special requirements, and the comments she wrote against the last revision — and then she tells procurement whether fabrication can release on Monday. That is not search. It is also not contract compliance. It is a different job, in a different vocabulary, against a different stack of authorities.


## Two deals in seven weeks


Document Crunch and Trunk Tools are excellent products, and the deals validate the category. They are also a tell about where the AECO incumbents see the easy money. Trimble was explicit in its announcement that the wedge is “critical risk provisions, payment disputes, specification non-compliance and notification failures.” That is a commercial-construction risk language. It is the language of a general contractor reading a master specification against a stack of vendor submittals.


Trunk Tools' own benchmark — “on a $100 million project, teams can manage over 3,000 submittals, 1,000 RFIs, and 250 specifications” — is the right benchmark for the commercial side. It maps cleanly to the CSI MasterFormat universe that AECO software has spent twenty years organizing.


The energy and EPC world has a different document register. A typical large EPC project carries supplier document requirement lists (VDRLs / SDRLs) with hundreds of vendor packages, each shaped by purchase-order-specific engineering attachments, applicable industry codes (API, ASME, NACE, IEC, IEEE), and project-specific design specifications. The review is not against a master spec book authored by the architect. It is against a stack of authorities that is different for every piece of equipment.


## What EPC review actually is


The volume problem is real and well documented. ASCC, summarizing Navigant research across 1,362 projects,[reports an average of 800 RFIs per project, eight hours to review and respond to each one, and roughly $1,080 in cost per RFI](https://ascconline.org/Home/News/articleType/ArticleView/articleId/121/Request-for-Information-RFI-and-Rework) — about $860,000 and 6,000 hours per project just on RFI traffic. A 2016 ASCE study found[only about one-fifth of submittals were approved as submitted in one studied case, with more than a third requiring a complete revision before acceptance](https://scholarworks.aub.edu.lb/bitstreams/f7ff8f2e-5517-4542-a22e-df42dc56aabb/download) .


Those numbers describe the cost of doing the work. They do not describe what the work is. The reviewer is rarely asking “where in the corpus does this document mention hydrotest hold time?” The real question is harder:


> Does this submitted procedure satisfy the project specification, the applicable industry standards, the purchase order requirements, and the prior review comments well enough for work to proceed?


That is not a retrieval question. It is a judgment question, and it is the question every existing AECO AI tool sidesteps when it returns a chat answer.


## The four moves


Strip away the software and the review is four moves.


**Scope.** Before she reads a single line, the engineer has already decided what the document will be measured against. A pump datasheet is not checked against the same corpus as a coating procedure or a switchgear commissioning script. The first job of a serious EPC review system is not to summarize the file; it is to assemble the right yardstick — standards, project specs, purchase order attachments, prior revision comments, discipline checklists. The wrong scope produces a confident, plausible answer to the wrong question.


**Inspect.** Good findings are specific and boring: a required clause is missing, a referenced edition is outdated, a value in §4.2 contradicts §7.1, a witness point in the ITP never makes it into the procedure, last revision's comment was acknowledged but not actually addressed. These are the gaps a careful human catches at midnight and a tired one misses. AI earns its keep when it catches them every time and shows its work.


**Mark up.** “The procedure may be incomplete” is not a comment. “Page 7, §4.2: the hydrotest acceptance criteria do not state the required hold time per Spec 1546-770-QAC-0003 §6.4” is a comment. The difference is whether the vendor can act on it tomorrow.


**Decide.** Approve. Approve with comments. Revise and resubmit. Reject. Those four codes — codified years ago in tools like[Oracle Primavera](https://docs.oracle.com/cd/E80480_01/help/en/user/113127.htm) and inherited by every major EPC contractor — determine whether fabrication releases, whether the vendor owes another revision, and whether risk is accepted.


## The comment belongs on the page


EPC review already runs on marked-up PDFs, stamps, comment bubbles, transmittals, and disposition codes. Vendors expect that format. Document controllers route it in Aconex transmittals and Supplier Documents packages. Auditors and regulators read it years later. When an AI tool produces a separate chat transcript or a tidy summary report, the engineer becomes a translator: she reads the answer, re-finds the page, re-types the comment, and clears the chat. The reconciliation cost eats the speedup.


The right place for a finding is the document. The right location is the page, the clause, the cell, the section. The right form is something the vendor can open in Bluebeam, Adobe, or Aconex and respond to with a revision the EDMS will accept. Anything else is workflow theater.


## The disposition is the product


The disposition code looks like a label and behaves like a gate. A — Approved releases procurement. B — Approved with Comments lets fabrication start while the vendor closes specific notes. C — Revise and Resubmit blocks work until the next revision arrives. D — Rejected tells the vendor the submitted approach will not be accepted at all. The codes are not Raycaster's — they belong to the industry, and they have for decades. The product question is whether the AI treats them as first-class objects or as a sentence at the end of an answer.


The treatment matters because the highest-risk failure of an AI review system is not the spectacular hallucination. It is the subtle mismatch between the comments and the disposition. The AI flags a missing acceptance criterion but recommends approval. The AI recommends revise-and-resubmit on a stack of formatting comments. The AI marks an “Approved as Noted” without indicating which notes are blocking. None of those are wording errors. They are workflow errors, and they cost trust on the first pass.


## What this changes


Andreessen Horowitz's recent thesis on AEC software made an unusually direct argument: the prize is the[services budget above Revit](https://a16z.com/every-building-youve-ever-been-in-was-designed-by-software-built-in-1997/) — the manual engineering work that has never been software-addressable before. EPC vendor document review is one of the largest items on that line, and it has not been won by the AECO incumbents because the review against engineering codes and project specifications is a different problem than the review against contracts and master specs.


The winning interface for EPC document review will not look like a chat box. It will look like a review station: a queue ordered by due date, discipline, and milestone risk; a standards panel showing what each document is being reviewed against; cited findings grouped by severity, anchored to the page; a side-by-side against the prior revision; a disposition that persists into the audit trail and the EDMS the project already runs on.


That is the product Raycaster is building, and that is the wedge. The buyer of EPC document review does not need a smarter chatbot. They need a system that knows the difference between a question about a file and a decision about a project. That difference is the entire job.
