---
schema_version: "1.0.0"
document_id: "fe3d5848297e9dbf58d9c9ed11ead55068df69adf35d05383551a0d0c95243b0"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/ai-catalog-updates-for-governance-and-operations/"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:07.155656+00:00"
fetched_at: "2026-07-28T20:48:41.890548+00:00"
content_hash: "sha256:b988df297e761b8a583af29f09c8b8f9c6123389908a64f509c9e54443018e4f"
---

# AI Catalog updates for governance and operations

Enterprise AI adoption often stalls not because the technology isn't ready, but because admins can't answer the question their security team is asking: What's actually running in our environment, and who put it there?


[GitLab 19.1](https://docs.gitlab.com/releases/19/gitlab-19-1-released/) ships event-driven triggers for Duo Flows alongside the governance controls and config validation that make running them safely possible. Together, they give enterprise teams the governance and reliability to run AI workflows continuously, without waiting for a human to pull the trigger.


## Run flows automatically on real GitLab events


Until now, every GitLab Duo Flow trigger required a human action in the GitLab UI: Mention, Assign, or Assign Reviewer. That made it more challenging to drive flows programmatically, integrate them into production pipelines, or fire them on a schedule. For teams running production workflows at scale, the limitations included: no automated conflict summaries, no compliance checks on ready-for-review MRs, and no incident creation on pipeline failure. None of these workflows were possible without someone manually initiating a flow.


With triggers and new pipeline event filters, flows run every time the conditions your organization defines are met, with no manual handoff required. The[AI Catalog](https://docs.gitlab.com/user/duo_agent_platform/ai_catalog/) stops being something your team uses and starts being something that works for your team, continuously, in the background, while developers focus on the work that actually requires judgment.


This is where the compounding value of agentic workflows becomes real. When flows respond to events automatically, the handoff delays that slow down feedback loops today start to shrink.


GitLab 19.1 includes four new event-driven triggers:


- **Merge request code conflict detected** fires the moment a conflict is detected in a merge request. That's exactly when an automated conflict summary and resolution proposals are most useful, before the developer has begun investigating the problem and before the conflict compounds. Previously, a developer would have to notice the conflict, open the MR, and manually trigger a review. Now the flow runs the moment the conflict appears.
- **Draft to ready for review** fires when a developer marks an MR ready. That's the right moment for an automated compliance review or readiness checklist to fire automatically. A flow that used to require a reviewer or team lead to manually initiate a pre-merge check now runs every time, the moment the signal arrives.
- **Merge request approved** fires when a merge request receives approval, automatically triggering post-approval steps like deployment readiness checks, compliance logging, or handoff notifications, without a human having to remember to kick them off.
- **Work item created** fires when a new work item is created. That opens up immediate triage flows, automatic label assignment, and routing logic that today requires either manual effort or brittle webhook setups outside GitLab.


While Pipeline events aren't new, the ability to filter by specific pipeline states is. You can now configure a trigger to fire only on failure, only on success, or only on cancellation, rather than on every pipeline state change. That means you can create incidents on failure and promote artifacts on success without alerting on every pipeline state change.


Merge conflict detected and Draft to Ready for Review are enabled by default, so teams gain value the moment they update to 19.1.


*Event-driven triggers*


A developer experience improvement is also landing in 19.1. For developers actually running those flows locally, repeated tool invocations — npm installs, file edits, multi-step refactors — used to require re-approval every time. A new pattern-based approval tier (now in beta) lets you approve all uses of a tool for the session, so the agent can iterate without interrupting you.


Watch the event triggers in action:


## Keep unapproved AI agents and flows out of your environment


An external agent surfacing in a namespace during a hackathon, or a team member enabling a community-contributed flow without security review, is enough to stop an enterprise trial in its tracks. For regulated organizations, unvetted AI content isn't a minor concern; it's a blocker.


Two new settings give instance admins and top-level group owners direct authority over exactly what runs.


- **Disable custom agents and flows** restricts users from creating or enabling custom-built agents and flows, limiting them to foundational content.
- **Restrict the AI catalog to your group hierarchy** blocks users from enabling AI Catalog items that originate outside their namespace, including community-contributed and third-party content.


Together, these settings bring AI agents and flows under the same admin oversight already governing other sensitive platform capabilities, so admins can open up broad usage without inviting agent sprawl into production.


## Catch flow misconfigurations before they ship


Event-driven flows that fire automatically raise the stakes on getting accurate configuration right inside an enterprise. Whether a flow is misconfigured or over-fires, it creates noise and issues across the organization.


GitLab 19.1 moves validation upstream. When a user saves or updates a flow in the AI Catalog, GitLab validates the flow config against the Duo Workflow Service before persisting it. If there's a problem — a missing input, an unknown tool parameter, anything the validator catches — structured errors appear in the UI before the flow is saved. Valid flows save and trigger exactly as before.


The result: Every flow in the AI Catalog is correctly configured before anyone depends on it in production. Configuration problems surface at save time, when they're cheap to fix, not in the middle of the night when a pipeline fails.


## Control which AI models are available to your team


Another governance capability is available as a public beta. Admins can now configure an allowlist of approved AI models and set an organization-wide default, rather than choosing between a pinned model or unrestricted selection.


This means organizations can restrict to data-residency-compliant or pre-approved providers while preserving end-user flexibility within that guardrail. This initial iteration applies to GitLab Duo Agentic Chat, with broader coverage across additional surfaces to follow soon.


## Build on a foundation that’s ready for production


With these updates, flows can finally operate the way production automation is supposed to: continuously, reliably, and without waiting for someone to press a button. Admins have the governance authority to control exactly what runs in their environment, and developers get configuration feedback at the right moment instead of the worst one.


> Start[your free trial](https://about.gitlab.com/gitlab-duo-agent-platform/) of GitLab Duo Agent Platform or[explore the documentation](https://docs.gitlab.com/user/duo_agent_platform/) to dig into agents, flows, and the AI Catalog.


##


Is AI achieving its promise at scale?


[Get your AI maturity score](https://about.gitlab.com/assessments/ai-modernization-assessment/)


Quiz will take 5 minutes or less
