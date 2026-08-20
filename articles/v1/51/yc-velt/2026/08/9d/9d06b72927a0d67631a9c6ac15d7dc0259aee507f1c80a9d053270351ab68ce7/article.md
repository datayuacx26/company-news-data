---
schema_version: "1.0.0"
document_id: "9d06b72927a0d67631a9c6ac15d7dc0259aee507f1c80a9d053270351ab68ce7"
company_key: "yc-velt"
company: "Velt"
source_id: "yc-velt-news-import-562f4f2d0c0b"
canonical_url: "https://velt.dev/blog/superdoc-approval-workflows-audit-trails"
published_at: "2026-08-12T23:14:37.827+00:00"
first_seen_at: "2026-08-13T01:46:35.563149+00:00"
fetched_at: "2026-08-13T01:46:38.150320+00:00"
content_hash: "sha256:f2af8ccd690e88de98fbdc142d0ffab4959c4f2e27e2f16f84639a0ad471cea8"
---

# How to Add Review, Approval & Audit Trails To Your SuperDoc Editor (a build-vs-buy breakdown) August 2026

You finish building on top of SuperDoc and then someone asks: who approved this version, and when? That question points directly to review and approval infrastructure: SuperDoc has no approval states, no audit log, and no sign-off mechanism. For teams in compliance-sensitive workflows, that gap isn't a minor inconvenience, it's a real risk. This post walks through what this infrastructure actually requires, and where the build-vs-buy line falls.


**TLDR:**


- SuperDoc has no native approval states, audit logging, or formal sign-off mechanism, leaving review workflows broken by default
- Building comment anchoring, approval workflows, and audit trails from scratch takes 4 to 6 months once DOM instability and edge cases are factored in
- A compliant audit trail requires append-only storage, SHA-256 hashing per event, and identity-linked reviewer records tied to your auth system
- Tamper-proof audit trails and multi-stage approval workflows fall in buy territory; building them yourself risks failing a compliance audit
- Velt adds review and approval infrastructure to SuperDoc via SDK, covering comments, approval workflows, presence, notifications, audit trails, and recording without replacing the editor


## Why SuperDoc Editor Teams Hit a Review Bottleneck


SuperDoc gives teams a capable rich-text editor built on[ProseMirror's document model](https://marijnhaverbeke.nl/blog/collaborative-editing.html) , with provider-agnostic real-time collaboration since v1.0 (December 2025). But the editor itself stops at the writing surface. The missing piece is review and approval infrastructure: the layer that handles comments, approval workflows, audit trails, and formal sign-off. Once a document moves into review, those gaps show up fast.


SuperDoc's February 2026 update added granular review permissions: a` permissionResolver` callback that controls who can resolve, reject, edit, or delete comments and tracked changes per user and per action. That's a useful addition for role-based review control. But it's not a document-level approval state machine. There's no discrete "pending," "in review," "approved," or "rejected" state attached to the document itself. Comments carry no status. Approvers have no formal sign-off mechanism that generates a timestamped record. That's the review and approval infrastructure gap SuperDoc doesn't fill on its own.


The audit trail problem is worse. When a compliance team or a manager asks who changed what, and when, SuperDoc doesn't log that at the document-action level. You either reconstruct the timeline from version history or you admit you don't have one.


These gaps aren't edge cases. They're the standard friction for any team using SuperDoc in a review-heavy workflow, whether that's legal redlines, content approvals, or financial document sign-off. The editor does its job. The review infrastructure around it doesn't exist yet.


## What Review, Approval, and Audit Trail Actually Require on a SuperDoc Editor


Before you can decide whether to[build or buy review and approval infrastructure](https://velt.dev/blog/build-vs-buy-collaboration-software) , you need a clear picture of what "review, approval, and audit trail support" actually requires at the implementation level. These aren't UI features you bolt on in a weekend. Each one carries real engineering weight.


### Comment Threading and Anchoring


Comments need to bind to specific elements in the SuperDoc editor, not float loosely at a line number or pixel coordinate. When a document reflows or a section gets reorganized, threads have to follow their anchor. That means stable element IDs, a sync layer that persists thread state, and resolution logic that handles concurrent edits without dropping context.


### Approval Workflows


Approval state isn't a boolean. You need role-aware routing, multi-stage sign-off chains, status transitions that trigger downstream actions, and a way to block publishing until the right people have signed off. Wiring an[approval workflow SDK](https://velt.dev/blog/approval-workflow-sdk-complete-developers-guide) into an existing editor means touching your auth layer, your data model, and your notification system.


### Audit Trails


Every status change, comment, edit, and approval decision needs to be captured as an immutable, timestamped event. That's an append-only event log with no update or delete path, cryptographic integrity checks, and queryable history. Compliance teams will ask for this retroactively if you don't build it upfront.


Building all three from scratch takes 4 to 6 months of engineering time across data modeling, UI components, permissions integration, and edge case handling.


## The Build Path: What JavaScript Teams Actually Ship


When JavaScript teams decide to build review and approval workflows from scratch inside a SuperDoc editor, the scope tends to grow fast. What starts as "let's add a comments sidebar" becomes a multi-month engineering project once the real requirements surface.


Here's what a typical self-build actually covers:


- A comment data model with threading, resolution states, and document anchoring that stays stable across DOM updates. If your element IDs shift, threads detach silently with no error thrown.
- An[approval state machine tracking draft, in-review, approved](https://velt.dev/blog/adding-review-states-to-your-app) , and rejected states, plus the logic that determines who can trigger each transition based on role.
- An[audit trail for your SaaS product](https://velt.dev/blog/how-to-add-audit-trail-to-saas-product) built on an append-only data store. Entries need SHA-256 hashes scoped to event ID, timestamp, user ID, and payload. No update or delete paths. S3 Object Lock in compliance mode if you're targeting compliance-driven industries.
- A notifications layer that fires on comment mentions, status changes, and approval decisions, wired into your existing auth model.
- A permissions layer that controls who can view, comment, approve, or export the audit log, scoped per document and per user role.


Most teams estimate 6 to 8 weeks. Engineering teams that have shipped it put the real number closer to 4 to 6 months, once edge cases like conflict resolution, offline states, and DOM instability get factored in.


That's the build path. Velt's review and approval infrastructure ships all of it as an SDK you drop into your SuperDoc integration.


## The Buy Path: Adding Review Infrastructure to SuperDoc Editor with Velt


Velt plugs directly into SuperDoc via a script tag or npm package and ships as review and approval infrastructure for the editor: comments, approval workflows, presence, notifications, audit trails, and recording all come prebuilt.


Here's what the integration path looks like in practice.


### What You Get Out of the Box


- Comment threads that bind to specific SuperDoc elements by data ID, not pixel coordinates. When a document reflows or a section moves, threads stay anchored to the right content without UI drift.
- Velt's[Declarative Approval Engine](https://velt.dev/approval-flows) with configurable states (draft, in review, approved, rejected) that attach directly to document versions. Reviewers can act without leaving the editor.
- [Immutable audit trails](https://velt.dev/audit-trail) that log every comment, status change, and user action with timestamps and user identity baked in. No separate logging service to wire up. Velt writes logs synchronously before the API response returns, so there's no window where an action occurred but the record doesn't yet exist.
- Role-based access controls that map to your existing auth model. Velt accepts a signed JWT, so permissions inherit from whatever identity provider you already run.


### What the Integration Looks Like


Install the SDK, initialize it with your auth token, and wrap your SuperDoc instance. Velt handles the real-time sync, the UI components, and the data persistence.


```text
import { VeltProvider, VeltComments, VeltPresence } from '@veltdev/react';


export default function SuperDocEditor({ documentId, authToken }) {
return (
<VeltProvider apiKey="YOUR_API_KEY">
<VeltComments />
<VeltPresence />
<div id="superdoc-container">
{/* Your SuperDoc editor instance */}
</div>
</VeltProvider>
);
}


```


The audit trail and approval state updates flow automatically from there. No separate event listeners, no custom database writes for review states. The review workflow and the audit trail are the same system in Velt, not two separate tools stitched together.


## Build vs. Buy: Decision Matrix for SuperDoc Editor on JavaScript


When deciding what to build yourself versus what to buy, the question goes beyond budget. It's about where your engineering time is best spent, and what breaks if you get it wrong.


For SuperDoc editors, the decision splits into three tiers based on feature complexity and compliance risk.


### Tier 1: Probably Build It Yourself


Simple read-only audit logging tied to a single user session, where no external compliance requirement exists, is something most teams can wire up in a weekend. The qualifying condition here is that your audit needs are purely internal and informal. The failure mode is assuming this scales when a client asks for tamper-proof records six months later. The minimum requirement is a stable data model from day one.


### Tier 2: Consider Carefully


Threaded comments, @mentions, and basic approval states fall in the middle. These seem simple but expand quickly. A DOM re-render breaks a comment anchor. A permission edge case surfaces at 2am. Budget 4 to 6 months of engineering time to do this properly.


### Tier 3: Buy It


Immutable audit trails with cryptographic integrity, multi-stage approval workflows, and real-time presence across a shared SuperDoc fall firmly in buy territory. The failure mode of building this yourself is shipping something that looks correct but fails a compliance audit. Velt is review and approval infrastructure for SaaS products: it ships comments, approval workflows, presence, notifications, audit trails, and recording ready to integrate, without requiring your team to solve distributed state, conflict resolution, or DOM anchoring from scratch.


Feature Build Time Estimate Buy with Velt


Basic audit logging 1 to 2 weeks Included


Threaded comments (DOM-aware) 4 to 8 weeks Included


Approval workflows 6 to 10 weeks Included


Tamper-proof audit trails 8 to 14 weeks Included


Real-time presence 3 to 6 weeks Included


## Compliance and Audit Trail Considerations for SuperDoc Editor


Audit trails aren't optional for teams working in compliance-sensitive industries. If your SuperDoc editor touches financial documents, legal contracts, healthcare records, or any content subject to compliance review, every edit, approval, and rejection needs to be logged with enough detail to reconstruct exactly what happened and when.


What "enough detail" actually means depends on your context, but the baseline for most compliance frameworks requires four things: who made the change, what they changed, when it happened, and whether someone approved it. A git commit log gets you halfway there. It doesn't capture approval states, reviewer identity tied to an auth system, or the difference between a comment that was acknowledged versus one that was acted on.


### What a Compliant Audit Trail Actually Needs


There are three layers worth thinking through before you build or buy.


- Immutability: log entries can't be edited or deleted after the fact. This typically means an append-only data model backed by something like[S3 Object Lock in compliance mode](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) , with SHA-256 hashes scoped to event ID, timestamp, user ID, and payload.
- Reviewer identity resolution: the audit log needs to tie actions to verified user identities from your auth system, not ephemeral session tokens or display names that can change. Teams weighing[manual review vs automated review](https://velt.dev/blog/manual-review-vs-automated-review) will find this distinction especially relevant when scoping compliance requirements.
- Approval state capture: the log needs to record document edits and[timestamped approval workflow transitions](https://velt.dev/blog/approval-audit-trail-saas-products) alike, capturing when a document moved from draft to in-review to approved or rejected, and who triggered each transition.


Velt ships all three out of the box as part of its review and approval infrastructure. You get tamper-evident audit trails, identity-linked approval states, and full workflow history without building the data model yourself. Because Velt captures the full annotation and decision lifecycle natively, teams that ship review infrastructure automatically get a compliance audit trail: one system, not two.


## Final Thoughts


The build-vs-buy question for review and approval infrastructure has a cleaner answer than most teams expect. The hard parts, immutable audit trails, multi-stage approval workflows, DOM-aware comment anchoring, and real-time presence, aren't hard because they're poorly documented. They're hard because they interact with each other in ways that only surface under production load: a DOM re-render detaches a comment thread, an async log pipeline creates a gap that a compliance auditor later flags, a permission edge case shows up at the wrong moment. Those failure modes are exactly what takes a self-build estimate from 6 weeks to 6 months.


The decision matrix in this post draws the line where the engineering risk and the compliance exposure both spike. For informal, internal-only logging with no regulatory requirement, building is fine. For anything a compliance team will review, the correct call is to use infrastructure that has already solved those failure modes. Velt ships review and approval infrastructure as a drop-in SDK layer for SuperDoc: comments, approval workflows, presence, notifications, audit trails, and recording, with the review workflow and the audit trail as a single system instead of two tools stitched together.


If your team is at the point where SuperDoc's editor is working well but the review layer doesn't exist yet, that gap is worth closing before a client or an auditor asks the question first.[Book a demo](https://velt.dev/book-demo) to see how Velt integrates with your SuperDoc setup.


## Frequently Asked Questions


These are the five most common questions teams ask before choosing between building review infrastructure themselves or adding Velt on top of their SuperDoc editor.


### Does SuperDoc have a built-in approval workflow?


SuperDoc ships with commenting, track-changes, and (since February 2026) granular review permissions: a` permissionResolver` callback that controls who can resolve, reject, edit, or delete per user and per action. What it doesn't ship is a document-level approval state engine or an immutable audit trail. There are no discrete document states like pending, in review, approved, or rejected, no multi-step sign-off chains, and no timestamped record of who triggered each workflow transition. Teams that need these either build them separately or add Velt's review and approval infrastructure on top of the[SuperDoc editing surface](https://velt.dev/libraries/superdoc) using the Declarative Approval Engine.


### How long does it take to build review and approval infrastructure for a SuperDoc editor from scratch?


The infrastructure baseline (DOM-aware comment anchoring, permission checks, real-time sync, and audit logging) takes 4 to 6 weeks before any business logic is written. A compliant audit trail with write-once storage, SHA-256 hashing scoped to event ID plus timestamp plus user ID plus payload, and S3 Object Lock in compliance mode adds another 2 to 4 months on top. Most teams estimate 6 to 8 weeks upfront; engineering teams that have shipped it put the real number at 4 to 6 months once conflict resolution, offline states, and DOM instability get factored in.


### What does Velt add to SuperDoc that SuperDoc doesn't already provide?


Velt adds the review and approval infrastructure layer that SuperDoc doesn't ship: comments, approval workflows, presence, notifications, audit trails, and recording, all bound to the SuperDoc editing surface without replacing the editor itself. Comments bind to DOM elements by data ID instead of pixel coordinates, so threads stay attached to the right content when a document reflows or a section moves. Approval state is managed by Velt's Declarative Approval Engine, which runs multi-step sign-off chains and writes every decision to an immutable audit trail automatically.


### Can Velt's audit trail satisfy compliance requirements?


Yes. Velt's audit trail uses append-only storage with no update or delete path, SHA-256 hashing scoped to event ID plus timestamp plus user ID plus payload, and S3 Object Lock in compliance mode for immutability. Logs are written synchronously before the API response returns, so there is no window where an action occurred but the record does not yet exist, a gap that async log pipelines introduce and that regulators treat as noncompliant.


### Does adding Velt require replacing SuperDoc?


No. Velt wraps the SuperDoc surface. The editor stays intact; Velt adds the review layer on top.


### Velt vs. building custom review workflows for a SuperDoc editor: which makes sense?


For simple internal audit logging with no compliance requirement, building yourself is reasonable. For threaded comments, approval states, and real-time presence, the scope expands fast enough that buying is worth considering seriously. For immutable audit trails with cryptographic integrity and multi-stage approval workflows, the kind compliance teams will study closely, Velt is the clear call. Velt is review and approval infrastructure for SaaS products: the review workflow and the audit trail are the same system, so teams that ship the review layer get the compliance record automatically. Building that layer yourself means shipping something that can look correct but fail a compliance audit, and backfilling the data model afterward is a rewrite, not a patch.


### How do I add an immutable audit trail to a SuperDoc editor?


You have two options: build an append-only event log yourself using S3 Object Lock in compliance mode with SHA-256 hashes per event covering event ID, timestamp, user ID, and payload, or use Velt's review and approval infrastructure, which ships this out of the box. Velt writes logs synchronously before the API response returns, meaning there is no window where an action occurred but the record does not yet exist, a gap that batch-reconstructed or async log pipelines introduce and that regulators treat as noncompliant.
