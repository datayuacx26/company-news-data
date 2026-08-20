---
schema_version: "1.0.0"
document_id: "f61522c5ad3484b73ef72861ee19cd8105f35320e6abe12586477fa233a8dabd"
company_key: "yc-kestrel-ai"
company: "Kestrel AI"
source_id: "yc-kestrel-ai-news-import-7c48f7782d06"
canonical_url: "https://usekestrel.ai/blog/workflow-observability"
published_at: "2026-06-20T00:00:00+00:00"
first_seen_at: "2026-07-25T10:40:48.612135+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:49b9ba13819a1aee9f14665984d31d94c00447a9dc8fb873357fec2afd2f03bc"
---

# Workflow Observability: See Every Run, Step by Step

Once you start running automations against production, "does it work?" isn't enough. You need to know what ran, what's running right now, what's waiting on a human, what failed and why — and which workflows are doing the heavy lifting versus quietly breaking. So every workflow you build in Kestrel ships with **execution observability** out of the box: a real-time dashboard, a full execution history, and step-level detail for every run. The demo above walks through the whole picture end to end.


That matters because automation changes the question you're asking. A script you run by hand is easy to reason about — you're right there watching it. But a workflow that fires on a pod crash at 3 a.m., branches on the type of fix, posts to Slack, waits for an approval, and opens a pull request is doing real work while no one is looking. When it does the right thing, you want the receipts; when it doesn't, you want to know immediately, and you want enough context to fix it without spelunking through logs. Observability in Kestrel is built for exactly that: it treats every run — successful, in-flight, blocked, or failed — as something you can see, inspect, and act on.


## A bird's-eye view across every workflow


The **Observability** dashboard gives you a bird's-eye view across all your workflows, opening with the numbers that matter at a glance: total **executions** , **success rate** , **average duration** , **average approval wait time** , and **active workflow count** .


Below that, a set of charts breaks down what's actually happening across your organization — execution volume over time broken down by workflow, a ranking of your most-used workflows, failure rate by workflow, execution duration, integration usage, trigger sources, approval wait times, and approval outcomes. You can immediately see which workflows run the most and which ones need attention.


Each of those numbers answers a question you'd otherwise have to chase down by hand. **Success rate** and **failure rate by workflow** tell you which automations to trust and which one quietly started failing after a dependency changed. **Average duration** and the duration breakdown surface the workflow that used to take ten seconds and now takes two minutes. **Average approval wait** shows where humans are the bottleneck — a gate that routinely sits for hours is a sign you've scoped approval too tightly. **Trigger sources** and **integration usage** show what's actually driving your automation, and **execution volume over time** makes spikes and drop-offs obvious at a glance. It's the difference between hoping your automation is healthy and knowing it is.


Click to expand


## Every run, in one list


The **Executions** page lists every workflow run across your organization, so you always know exactly what's running and what's waiting for approval. Each row shows the workflow, its status, what triggered it, when it ran, and how long it took. If a run failed, the error is shown inline — you don't have to dig for it.


Filter the list by status (Completed, Failed, Running, Awaiting Approval, Cancelled), by workflow name, or by trigger source — Kubernetes, AWS, PagerDuty, PostHog, Vercel, Slack requests, and more — or search across workflow names and error messages. When a run fails, the error sits right there in the row, so the list doubles as a triage queue: scan it, spot what broke, and click straight into the run that needs attention. (More on fixing failures below.)


## Step-by-step, in real time


Click into any execution and you see the workflow as a DAG, with real-time status on each step. Every step shows its duration and a green checkmark when it completes, the current step is highlighted, and any failure is surfaced right on the node.


Click to expand


Click any block to inspect it. For the **trigger** , you see the original signal — what happened, the affected resource, severity, and the exact timestamp. For **action** steps, you see the full output: AI investigation results and root cause, generated fixes, API responses, and any resources that were created. You get complete visibility into everything your workflow did, step by step.


Take a real incident-response run as an example. The trigger node shows the Kubernetes pod crash that kicked it off — the namespace, the workload, the reason, and when it fired. The next node, an RCA step, shows the agent's investigation and the root cause it landed on. A condition node shows which branch it took and why — the exact field value it evaluated. The Slack node shows the message that went out and to which channel; the approval node shows who signed off and when; and the GitHub node shows the pull request it opened, with a link. Nothing is a black box — for any step you can see the inputs it received, the work it did, and the outputs it produced, which is exactly what you need when you're explaining an incident after the fact or convincing yourself a workflow is safe to leave running unattended.


## Approval gates your team can respond to anywhere


Observability isn't only about watching runs — it's about acting on the ones that are waiting for you. You can define **approval gates** to pause a workflow until someone signs off, and the **Pending Approvals** page shows every blocked execution in one place: the workflow, the approval type, the required approvers, and how long it's been waiting.


There are three approval types. **Manual** approvals you action from the dashboard. **Slack** approvals where your team clicks **Approve** or **Reject** directly in a Slack channel, right where they already work. And **PR / MR** approvals, where merging the pull request continues the workflow — so your existing code-review process becomes the approval step.


### Manual (in-app)


Pause until an authorized user approves in the Kestrel dashboard. Best for changes your platform team should green-light from one place.


Click to expand


### Slack


Send an interactive request to a Slack channel — your team clicks **Approve** or **Reject** right where they already work, no context-switch required.


Click to expand


### PR / MR approval


Tie the gate to a pull request. The workflow continues when the PR is approved or merged, and stops if it's closed. These gates are webhook-driven, so the workflow resumes the moment the event lands in GitHub or GitLab — no polling, no manual nudging.


Click to expand


PR-based gates support two behaviors, so the gate matches how your team already reviews changes. **Wait for PR/MR Approval** resolves the gate when the pull request is approved via review; **Wait for PR/MR Merge** resolves it when the pull request is merged. Merging a PR approves the step, and closing it without merging rejects it. Because these gates are webhook-driven, the workflow resumes the moment the event lands in GitHub or GitLab — no polling, no manual nudging — and the wait time shows up in your approval metrics like any other gate.


## Who is allowed to trigger a workflow


Observability and approvals answer "what happened" and "who signed off." The other half of safe automation is *who is allowed to start it in the first place* . For on-demand **developer request** workflows — the ones your team kicks off themselves rather than ones that fire on an infrastructure signal — you can restrict exactly who can run them. On the request trigger, **Allowed Users** limits the workflow to specific people, and **Allowed Groups** limits it to one or more groups from your organization (leave either empty to allow everyone). Define and manage groups in the **User Groups** tab of **Organization Management** , then reference them here to grant a whole team access at once.


You can layer these with the request's own scoping — **Allowed Clusters** , **Allowed Namespaces** , **Allowed AWS Accounts** , **Allowed Zones** , and more — so a self-service workflow can be opened up to a whole team while staying fenced into the exact resources they're permitted to touch. Every run that results still shows up in the same Executions list and dashboard, so self-service never means flying blind.


## Every blocked run, in one place


The **Pending Approvals** page collects every execution that's waiting on a human across your whole organization. From here you can **Approve** or **Reject** manual and Slack gates directly, **View Execution** to see the full run for context, or — for PR gates — **Review PR** to jump straight to the pull request. A **Recent Approvals** section keeps an auditable history of who approved or rejected what, and when.


For manual and Slack gates, the **Require Approval From** editor lets you build approval rules out of individual users and groups. By default anyone in the org can approve; add people or groups to a rule and *all* of them must approve, and add a second rule so *either* rule can satisfy the gate — everything from "any platform admin" to "both the on-call lead and a security reviewer." It's role-based by default: building, activating, and approving workflows is scoped to your platform administrators, while app developers get a focused view for submitting developer requests.


## When something fails


Failures are a first-class part of the view, not an afterthought. The Executions list shows the error inline, the failed step is marked on the DAG, and the step inspector shows the exact error. Even deterministic workflows fail — a Slack channel gets renamed, a repo permission changes, an API field expects an ID where you gave it a name. When that happens, the usual loop is tedious: read the error, guess the cause, open the builder, change a parameter, re-run, and hope. Kestrel collapses that loop into one step with the **Failure Diagnosis** agent.


### Diagnosis where the failure happened


Open a failed run from the Executions page and the failed step is already marked on the DAG. From there, launch the **Failure Diagnosis** agent right alongside the execution. It has full context on the run — the trigger signal, every step's output, and the exact error — so it doesn't need you to paste logs or explain what the workflow was supposed to do. Ask it anything about the failure:


- "Why did this workflow execution fail?"
- "What do I need to fix to make this workflow succeed?"
- "Is this a permissions issue or a configuration issue?"


Click to expand


### From explanation to corrected workflow


The agent doesn't just explain the failure in plain English — it produces a corrected version of the workflow with the problem fixed. Review the proposed change, then save it with one click; the corrected definition opens in the builder so you can confirm everything looks right before reactivating. If the original workflow was deleted in the meantime, the agent offers to create a fresh workflow from the corrected version instead.


### Pairs with replay


Failure diagnosis works hand-in-hand with execution **replay** . Once you've saved the corrected workflow, replay the failed run — from the beginning, or from the failed step to preserve the work the earlier steps already did. That last part matters in practice: if a five-step workflow failed at step four because of a transient API error, you don't want to re-run the first three steps and re-open a duplicate pull request — you want to pick up exactly where it stopped. Together, diagnosis and replay turn a failed run into a fixed, re-run workflow in minutes, not a debugging session. It's the same observability detail working both ways: the context that lets you understand a healthy run is what lets you repair a broken one.


Click to expand


## The full picture


That's the full picture — step-level logs for every execution, real-time status updates, and approval gates your team can respond to from the dashboard, Slack, or pull requests. Observability is on by default for every Kestrel Workflows account. Open **Workflows → Observability** for the dashboard, or **Workflows → Executions** to browse runs. Every execution — past, live, or waiting — is already there.


### Start building with $1,000 in credits


If you're interested in automating your platform workflows today, you can get started with $1,000 in usage credits. Describe what you want in plain English and Kestrel builds the workflow — then watch every run, step by step.


[Get Started](https://platform.usekestrel.ai/register)
