---
schema_version: "1.0.0"
document_id: "53a334a0ac1bd8c459702cb8f5a27199d8774c341055f3c6c625bcae702abd75"
company_key: "yc-velt"
company: "Velt"
source_id: "yc-velt-news-import-562f4f2d0c0b"
canonical_url: "https://velt.dev/blog/why-add-comments-wrong-way-collaboration-features"
published_at: "2026-05-25T21:46:57.030+00:00"
first_seen_at: "2026-07-22T18:33:38.447297+00:00"
fetched_at: "2026-07-28T21:44:24.455372+00:00"
content_hash: "sha256:2eab5fd680a7b26e485007784bc9cbf63a907e6ae1dc3243cef4ac4de42db272"
---

# Why 'Add Comments' Is the Wrong Way to Think About Collaboration Features (May 2026)

B2B SaaS teams at companies like HubSpot, Notion, and Jira have all shipped some form of in-app commenting. You add a comment box, ship it, move on. That's the pattern everyone follows because it feels like table stakes. But comments capture conversations, not decisions. When your users need to route approvals through stakeholders, track who signed off on which version, or satisfy audit requirements, the comment thread has no answer. What you're really building is[a review layer, far beyond commenting](https://velt.dev/) . The framing shift matters because it changes everything downstream.


**TLDR:**


- Review capacity is the bottleneck in 2026, not content creation
- Comments capture feedback; approval workflows capture decisions
- Velt provides review and approval infrastructure for SaaS products
- Context-aware comments bind to data IDs, not pixel coordinates that break
- Velt is review infrastructure with comments, approvals, audit trails, and notifications


## The Review Capacity Problem That Comments Can't Solve


Most product teams frame collaboration features the same way: add[a comment box](https://velt.dev/blog/what-is-commenting-sdk) , ship it, done. That mental model belongs to a world where content creation was the bottleneck. It isn't anymore. AI generates content, code, designs, and business artifacts at a pace that comment threads weren't built to handle. The creation side scaled. The review side didn't. But what teams actually need is review capacity: the ability to route work through the right people, collect structured feedback, track approval states, and maintain a record of what changed and why. Commenting is one small piece of that. Presence, approval workflows, audit trails, and notifications are the rest.


The framing matters because it changes what you build. Teams that think "add comments" ship a comment box. Teams that think "review infrastructure" ship something that actually moves work forward.


## What Collaboration Features Actually Mean in 2026


Commenting is table stakes now. Any app built in the last two years ships some form of comment thread. The question teams are actually asking in 2026 is whether their feedback workflows hold up under production conditions like version-controlled assets, multi-stakeholder review cycles, compliance sign-offs, and audit requirements. But that question deserves a different answer than "add a comment box."


What teams need is review and approval infrastructure: the full set of behaviors that lets feedback move through an organization and reach a decision. That includes:


- Threaded comments anchored to specific elements, not floating in a sidebar with no context
- Approval states that track who signed off, on what version, and when
- Presence indicators so reviewers know who else is looking at the same asset
- Notifications that route the right person at the right step, instead of everyone all at once
- Audit trails that satisfy compliance requirements without manual documentation


The gap between "we have comments" and "we have review infrastructure" is where most SaaS products stall. Users leave feedback, nothing gets resolved, and[the workflow falls back to Slack threads](https://velt.dev/blog/best-commenting-sdk-use-cases) and email chains because the in-app experience lacks structure.


## The Approval Workflow Gap


Comments capture feedback. Approval workflows capture decisions. These are different things, and conflating them is how products end up with threads that go nowhere. A comment is someone saying "change the headline." An approval is someone saying "this is ready to publish, and my name is on it." The accountability gap between those two states is where content gets stuck in most SaaS products.


Most teams build the comment side and assume the approval side will sort itself out. It doesn't. Without explicit sign-off states, someone has to manually chase down whether a given comment was resolved and approved by the right person. That chase happens in Slack, which has no memory, no audit trail, and no way to tie the decision back to the artifact.[Effective approval workflows require clear documentation](https://www.spendflo.com/blog/approval-workflows) of each decision point and stakeholder responsibility.


What we hear consistently: teams come in looking for commenting. When approval workflows come up, the response is "oh, we need that too." They had the problem. They just hadn't framed it that way yet. A proper approval workflow has at least three moving parts: explicit sign-off states (pending, approved, rejected), assignment routing that sends the right document to the right reviewer at the right step, and a timestamped record that ties each decision to a specific user and version. None of those are comments. They're structured workflow states that sit on top of the review layer.


Velt ships all three as part of its approval workflow infrastructure. You configure the assign-to UI, set the approval states, and Velt handles routing, state management, and attribution. The result is a full audit trail that answers "who approved this, on which version, and when" without anyone chasing down a Slack thread. For teams in regulated industries like finance or healthcare, that record isn't optional. It's the whole point.


## Audit Trails: The Missing Piece in Most Collaboration SDKs


When a compliance auditor asks "who approved this contract and when," a comment thread has no good answer. Comments capture conversations. Audit logs capture decisions, and that difference is where regulated industries get stuck. Healthcare, finance, and legal teams need more than a record of what was said. They need to know who accessed a document, what changed at each version, which approvals were given by whom, and exact timestamps attached to each action. That's an audit trail, not a comment.


Most commenting systems blur this line entirely. No immutable records, no formal retention policies, no compliance-ready exports. When a clinical document needs filing or a financial statement needs sign-off, "check the comment thread" doesn't hold up under scrutiny.[Audit trail compliance requirements](https://www.inscopehq.com/post/audit-trail-requirements-guidelines-for-compliance-and-best-practices) demand chronological records with attribution and timestamps.


[Velt's audit logs](https://velt.dev/activity-logs) track comment creation, edits, deletions, presence changes, and approval states across the full collaboration lifecycle. All of it is accessible via REST API or prebuilt UI components, attributed to the right user, at the right timestamp.


## Context-Aware vs. Pixel-Based: Why Most Commenting Breaks


Pixel-coordinate commenting sounds fine until someone resizes their browser, updates their layout, or adds a data row. The comment is anchored to a screen position, not an element. When the interface changes, the thread floats off into nowhere or lands on the wrong thing entirely. This is UI drift, and it quietly kills in-app commenting adoption. Users leave feedback, the interface updates, and suddenly no one can tell what a comment was actually about. The thread becomes a liability instead of a record.


### How Velt Handles This Differently


Most commenting tools were built around a document metaphor, where content is static and coordinates hold. SaaS apps don't work that way.


Velt binds[comment threads to data IDs](https://velt.dev/blog/best-contextual-commenting-systems-november-2025) : a` slide-id` , a` widget-id` , a` video-timestamp` . Not pixel coordinates. When the layout reflows, the thread stays attached to its element. No coordinate remapping, no orphaned comments, no lost context.


That binding is what makes review and approval infrastructure viable inside a real app. Without it, you're shipping a feature that degrades the moment your product ships its next update.


### What this looks like in code


Here's a minimal React setup that wires up Velt with data-ID anchoring. Comments bind to` data-velt-document-id` elements, so threads survive layout changes.


```text
// 1. Install: npm install @veltdev/react


// App.jsx — wrap your app with VeltProvider
import { VeltProvider, VeltComments, VeltCommentTool } from '@veltdev/react';
import DocumentView from './DocumentView';
import AuthInit from './AuthInit';


export default function App() {
return (
<VeltProvider apiKey="YOUR_VELT_API_KEY">
{/* Mounts the comment layer — no other config needed to start */}
<VeltComments />
<AuthInit />
<DocumentView />
</VeltProvider>
);
}


// AuthInit.jsx — identify the current user
import { useEffect } from 'react';
import { useVeltClient } from '@veltdev/react';


export default function AuthInit() {
const { client } = useVeltClient();


useEffect(() => {
if (!client) return;


const user = {
userId: 'user-123',
organizationId: 'org-abc',
name: 'Jane Smith',
email: 'jane@example.com',
photoUrl: 'https://i.pravatar.cc/300',
};


client.identify(user);
}, [client]);


return null;
}


// DocumentView.jsx — bind comments to data IDs, not pixel coordinates
import { useEffect } from 'react';
import { useVeltClient } from '@veltdev/react';


export default function DocumentView() {
const { client } = useVeltClient();


useEffect(() => {
if (!client) return;
// Set the document context so threads are scoped to this asset
client.setDocument('campaign-brief-v3', {
documentName: 'Q3 Campaign Brief',
});
}, [client]);


return (
<div>
{/* Each section gets a unique data-velt-document-id.
Comments attach to the ID, not the pixel position.
Resize the window, reorder sections — threads stay put. */}
<section data-velt-document-id="headline-block">
<h2>Headline</h2>
<p>Q3 product launch — driving sign-ups through paid search.</p>
</section>


<section data-velt-document-id="budget-block">
<h2>Budget</h2>
<p>$40,000 across Google and LinkedIn.</p>
</section>


{/* Comment tool lets reviewers pin threads to any element */}
<VeltCommentTool />
</div>
);
}
```


## Review Infrastructure as a Category


The term "commenting" undersells what users actually need. When product teams, compliance reviewers, or content approvers work inside a SaaS app, they're not simply leaving notes. They're running a workflow: flagging issues, assigning owners, tracking resolution, signing off, and generating an audit trail. That workflow is review infrastructure, and it's a distinct category from general-purpose collaboration. General collaboration tools (presence, cursors, and real-time sync) handle simultaneous editing. Review infrastructure handles the full lifecycle of feedback: where it's anchored, who owns it, what state it's in, and whether it's resolved.


Velt is built for this use case. The distinction matters because building review infrastructure yourself means solving for comment threading, approval states, notifications, presence, and audit trails as separate problems. Teams that try this typically spend months before shipping anything.


Velt ships all of it as review and approval infrastructure, integrated as a single SDK.


Feature Velt Liveblocks


Primary Use Case Review and approval workflows with compliance requirements, audit trails, and structured feedback loops Real-time multiplayer experiences like whiteboards, design tools, and single-canvas collaboration


Comment Anchoring Data ID-based binding (comments attach to element IDs like slide-id or widget-id, survive layout changes) Pixel coordinate-based (comments can drift when UI reflows or elements move)


Approval Workflows Built-in approval states, sign-off tracking, and decision capture with full attribution Not included (requires custom backend logic to build approval flows)


Audit Trails Immutable logs of all actions (comments, edits, approvals, presence) with timestamps and user attribution, compliance-ready exports Basic activity tracking (not designed for compliance or formal audit requirements)


Permissions Model Document-aware permissions that respect your app's access control at the element level Room-based permissions (better for shared canvas experiences than document workflows)


Backend Requirements Minimal (approval routing, state management, and permissions handled by SDK) Custom sync logic required for complex workflows beyond basic real-time primitives


## How Velt Thinks About the Review


## Layer


Velt is review and approval infrastructure for SaaS products. That means contextual comments, approval workflows, presence, notifications, audit trails, and recording shipped as a single integrated SDK, not as separate features bolted together after the fact.


The five categories where this matters most:


- Content production teams that can't publish unchecked work, where[review loops stay inside the tool](https://velt.dev/comments) instead of in a shared Google Doc comment thread that no one checks.
- Compliance and FP&A software where every decision requires a timestamp, an owner, and a record that survives an audit.
- Physical-world operations where supply chain and logistics decisions get made inside the tool and need traceable sign-off.
- Internal apps where review workflows are expected by the people using them but rarely get built by the teams shipping them.
- Analytics products where insight discussions drift to Slack the moment there's no in-app structure to hold them.


In each case, the core problem is the same. Review state lives outside the product. Velt puts it back inside, anchored to the right element, routed to the right person, with a record that holds up when anyone asks what happened and why.


## Final Thoughts on Moving Beyond Comment Boxes


Your users don't need another place to leave comments. They need review workflows that route feedback to the right people, track decisions, and keep a record that survives an audit.[Review infrastructure](https://velt.dev/) treats this as a complete system, not as features you patch together over six months. Velt handles commenting, approvals, presence, notifications, and audit trails as a single SDK.[Book a demo](https://velt.dev/book-demo) if you want to see what that looks like in production, or keep building it yourself and ship sometime next quarter.


## FAQ


### What's the difference between commenting infrastructure and review infrastructure?


Commenting captures feedback; review infrastructure captures decisions. Review infrastructure includes comments plus approval states, audit trails, presence indicators, notifications, and assignment workflows that let feedback move through an organization and reach a decision with full accountability.


### Can I build review workflows without custom backend logic?


Yes. Velt ships approval states, assignment tracking, and audit trails as part of the SDK. You drop in the components and configure them; Velt handles routing, state management, and permission enforcement without requiring backend work on your end.


### Review infrastructure Velt vs Liveblocks?


Velt is built for review and approval workflows with document-aware permissions, approval states, and audit trails out of the box. Liveblocks is a real-time primitive better suited for multiplayer whiteboards or single-canvas experiences where you're building custom sync logic. If you need review workflows with compliance requirements, Velt is the better fit.


### How does Velt prevent comments from breaking when the UI changes?


Velt binds comment threads to data IDs (like` slide-id` or` widget-id` ), not pixel coordinates. When your layout reflows or elements move, the thread stays attached to its element. This prevents UI drift where comments float off into nowhere or land on the wrong thing after updates.


### What makes an audit trail different from a comment history?


An audit trail is an immutable record of who did what and when, with formal timestamps and attribution that survives compliance audits. A comment history shows conversations but lacks the formal retention, attribution, and compliance-ready exports required for regulated industries like healthcare, finance, and legal.
