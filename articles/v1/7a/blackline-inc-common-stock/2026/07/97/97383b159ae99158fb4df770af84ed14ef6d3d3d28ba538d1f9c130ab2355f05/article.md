---
schema_version: "1.0.0"
document_id: "97383b159ae99158fb4df770af84ed14ef6d3d3d28ba538d1f9c130ab2355f05"
company_key: "blackline-inc-common-stock"
company: "BlackLine Inc."
source_id: "blackline-inc-common-stock-news-import-2d1fe8859cfb"
canonical_url: "https://www.blackline.com/blog/human-in-the-loop-at-scale/"
published_at: null
first_seen_at: "2026-07-24T21:08:53.773154+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:8d913bbf74ad2d4c205357bd6f11ffc90acdc337109c9905180fe7ed665b1de7"
---

# Human-in-the-Loop at Scale: Designing Oversight That Doesn't Slow You Down

Key Takeaways


##


• Rather than acting as a process bottleneck, well-designed human oversight accelerates financial workflows by focusing review capacity on high-risk exceptions.


• Exception-first architecture automatically processes standard transactions and silently routes only complex anomalies for human review.


• Asynchronous, non-blocking approval flows decouple system processing from manual reviews to eliminate critical-path close bottlenecks.


• Audit-trail-first design models every human review as an immutable, first-class system event to simplify and streamline compliance audits.


• Confidence-based routing and automated, SLA-aware escalation ensure exceptions are routed and resolved without delaying period-end close.


## The False Tradeoff


When organizations add human-in-the-loop (HITL) oversight to AI-driven workflows, the instinct is to treat it as a gate: the system proposes, a human approves, the workflow proceeds. This feels safe. It also feels slow — because it is.


In financial operations, this tension is acute. Atanu Neogi's post on[AI guardrails and simulatability](https://www.blackline.com/blog/ai-guardrails-to-mitigate-risk/) makes the case clearly: financial workflows have very low simulatability, meaning the cost of an AI error — a misapplied journal entry, a non-compliant reconciliation — cannot be absorbed the way a bad code deploy can. Human oversight is not optional.


But the conclusion many teams draw from this is wrong. They translate "mandatory human oversight" into "a human must approve every action," which collapses back into manual workflows with an AI wrapper. The right conclusion is: human oversight must be *targeted* , *asynchronous* , and a *rchitecturally embedded* . The engineering challenge is building systems where humans intervene precisely when they add value — and nowhere else.


## The Air Traffic Control Model


Air traffic controllers don't manually guide every plane from gate to gate. They monitor a rich, structured view of the airspace, issue instructions only when a decision exceeds the system's safe operating parameters, and their every call is logged. The aviation system is designed so controllers are never overwhelmed — it actively manages their cognitive load.


This is the model for scalable HITL in financial systems. The AI handles the routine. The system monitors for anomalies. Humans are a scarce, high-value resource allocated by the platform to situations that require their judgment. Their decisions are recorded with full context.


*The engineering insight:* ***scale HITL by treating human attention as a finite resource and designing the system to allocate it intelligently.***


## Pattern 1: Exception-First Architecture


The dominant HITL anti-pattern is approval-first design: the AI acts, then a human confirms. For high-volume financial workflows — millions of transactions, thousands of reconciliations — this creates an unmanageable review queue.


Exception-first design inverts this. The system defines a confidence envelope: The system processes and logs actions within this envelope automatically. Actions that fall outside it — by threshold, pattern, rule, or risk score — are routed for human review.


The engineering work is in defining the envelope precisely. For[financial close](https://www.blackline.com/products/financial-close/) , this means:


• **Monetary thresholds** : Journal entries above a defined amount require preparer review before posting.


• **Statistical anomaly detection** : Reconciliations where the AI-assigned match confidence falls below a defined percentile are flagged.


• **Rule-based exceptions** : Entries that touch high-risk account types (intercompany, accruals, manual adjustments) require an additional review step regardless of confidence.


• **Change-relative triggers** : Amounts that deviate materially from the same period in prior years surface for human attention.


The result is not that humans review less — it is that they review what matters. A well-tuned exception-first system routes the 3–5% of transactions that carry 95% of the risk. Human reviewers spend their time on genuine anomalies, not confirming that routine entries are routine.


## Pattern 2: Async, Non-Blocking Approval Flows


In synchronous HITL, the workflow pauses and waits. A journal entry sits in "pending review" until a controller opens their queue. During peak close periods, this becomes a critical-path bottleneck.


Async approval flows decouple AI execution from human review. The system continues processing; the human review runs in parallel and resolves the item before it reaches a downstream dependency gate. This requires careful workflow design:


• **Staged finalization** : Entries are processed and held in a pre-committed state. They become final only after the review resolves — but the system can continue building dependent calculations against the pending value, with[reconciliation](https://www.blackline.com/resources/glossaries/balance-sheet-reconciliation/) on final commit.


• **Mobile-first review surfaces** : Reviewers should be able to approve or escalate an exception in under 30 seconds from a mobile device. Long approval interfaces are unused approval interfaces.


• **Batched review windows** : Rather than interrupting a reviewer for every exception as it arrives, the system groups exceptions into structured review sessions aligned with the reviewer's workflow (e.g., a morning queue before the daily close window).


The key constraint: async flows require the system to be explicit about what is finalized versus pending, and downstream systems must handle that ambiguity cleanly. This is harder to build than a synchronous gate — but it is the only pattern that scales.


## Pattern 3: Audit-Trail-First Architecture


Compliance in financial systems doesn't just require that a human reviewed something. It requires


*proof* that a human reviewed it, with context: who reviewed it, when, what information they had, what decision they made, and whether they had the authority to make it.


In most systems, the audit trail is a side effect — a log entry appended after the fact. In audit-trail-first architecture, the human decision is a first-class event in the data model.


This means:


-


Every review action writes an immutable event: reviewer identity, timestamp, the state of the record at time of review, the decision, and any attached rationale.


-


Review events are stored independently of the records they reference, so audit queries don't require joining across operational tables.


-


The system enforces segregation of duties at the event level: accountant who prepares a journal entry cannot also be the reviewer whose review event marks it approved.


-


Review events are queryable as a timeline, so auditors can reconstruct exactly what was known and decided at each point in the close cycle.


This architecture is more expensive to build upfront. It pays dividends at the first SOX audit — and at every one after.


## Pattern 4: Confidence Routing and SLA-Aware Escalation


Not all exceptions are equal, and not all reviewers are interchangeable. A well-designed HITL system routes exceptions to the right reviewer, not just any reviewer.


**Confidence-based routing** maps exception type and risk score to reviewer tier. A low-confidence match on a high-volume, low-value transaction goes to a reconciliation preparer. A high-risk manual journal entry above materiality threshold goes to a controller. A potential internal control deficiency escalates to a finance manager with a tighter SLA.


**SLA-aware escalation** ensures that exceptions don't age silently. Every exception carries a resolution deadline tied to the downstream process it must clear (the daily close, the period-end lock, the consolidation run). As an exception approaches its SLA, the system escalates: first a reminder to the assignee, then a notification to the backup reviewer, then an alert to the team lead. The goal is zero surprises at period close.


The routing configuration is itself auditable — changes to routing rules are versioned and logged. When an auditor asks why a particular entry was reviewed by a preparer rather than a controller, the system can show the routing logic that applied at that moment.


## What This Looks Like in Practice


**Pattern**


**Naïve HITL**


**Scalable HITL**


**Review Scope**


Every transaction


Exception-only (3–5% of volume)


**Approval Flow**


Synchronous gate


Async, non-blocking


**Audit Record**


Log entry (afterthought)


First-class event (by design)


**Routing**


Static queue


Confidence + role-based


**Escalation**


Manual follow-up


SLA-driven, automated


**Reviewer Experience**


Undifferentiated queue


Prioritized, context-rich, mobile-ready


[Verity AI](https://www.blackline.com/products/verity-ai/) & the BlackLine[Studio360 platform](https://www.blackline.com/products/studio360/) implement these patterns as platform primitives — not configurations each implementation team must build themselves. Exception routing, audit event structure, and SLA-aware escalation are built into the substrate, so AI agents and human reviewers operate as a coordinated, standardized system across every entity and ledger.


## Back to the Runway: Governance as Infrastructure


Earlier, I used air traffic control as a model for how humans should be positioned inside a HITL system — attentive, decisive, never overwhelmed. But the aviation analogy goes deeper than ATC, and it points to something more fundamental.


Commercial aviation operates at a scale most industries can barely imagine: over 100,000 flights per day, across every time zone, operated by thousands of different crews in hundreds of different aircraft. Yet the safety record is extraordinary — and it is not an accident.


Aviation achieved this through governance as infrastructure. The International Civil Aviation Organization (ICAO) established universal protocols: standardized checklists, mandatory crew training, common communication procedures, and shared oversight standards. The result is that whether a flight departs from London, Lagos, or Osaka, every crew follows the same procedures, and every passenger gets the same safe outcome. That is not coincidence. That is a system.


The key insight is that aviation did not achieve scale by making pilots better. It achieved scale by making the system reliable — so that any qualified crew, anywhere, produces a correct outcome by design.


This is exactly the problem financial operations faces today. Every company closes its books differently. Every team has its own reconciliation practices, its own tolerance for manual workarounds, its own interpretation of what "reviewed" means. AI agents operating in this environment inherit all that inconsistency — and amplify it.


The engineering mandate is to build the ICAO equivalent for finance: universal protocols embedded in the platform itself. The same exception thresholds, the same audit event structure, the same escalation logic — applied consistently across every entity, every ledger, every period close. So that any finance team, anywhere, using any ERP, produces a correct, auditable, compliant close by design.


The same way aviation doesn't rely on pilot heroics to produce safe flights, finance shouldn't rely on controller heroics to produce clean books.


***Build the system right, and correctness becomes the default — not the exception.***


Discover how top-performing companies are reducing cycle times and scaling human oversight with automation from BlackLine.


[Get the Guide](https://www.blackline.com/resources/financial-close-benchmark-guide/)
