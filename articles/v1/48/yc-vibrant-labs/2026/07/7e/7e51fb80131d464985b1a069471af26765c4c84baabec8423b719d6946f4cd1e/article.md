---
schema_version: "1.0.0"
document_id: "7e51fb80131d464985b1a069471af26765c4c84baabec8423b719d6946f4cd1e"
company_key: "yc-vibrant-labs"
company: "Vibrant Labs"
source_id: "yc-vibrant-labs-news-import-1f38412d73f2"
canonical_url: "https://vibrantlabs.com/research/enterprise-worlds-itsmbench"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T08:51:09.944686+00:00"
fetched_at: "2026-07-29T08:51:11.077400+00:00"
content_hash: "sha256:0448cc3d7f2aec12247a78cb0de05f9a296ae2c3d51de0cc64b7abd5bf2abc43"
---

# Enterprise-Worlds: ITSMBench

## TL;DR


We’re open-sourcing[Enterprise Worlds](https://github.com/vibrantlabsai/Enterprise-Worlds) , a suite of executable enterprise gyms for training and evaluating agents on realistic, policy-governed operational work.


Our first release, **ITSMBench** , is an IT service management environment with persistent enterprise state, simulated users, and 93 tools that agents must actively navigate. Agents must resolve ambiguity, follow policy, update systems of record, and complete workflows where partial success can still leave the organization in a bad state


Our results show that frontier models still lack the reliability needed for enterprise operations. They can often solve a task at least once, but repeated-success scores drop sharply, revealing a gap between capability and dependable execution


## Introduction


Agent work is only valuable if the surrounding organization can absorb it safely.


A coding agent's change still needs review, approval, deployment, monitoring, and a rollback path. A sales agent's closed deal still needs account setup, permissions, billing, and contract updates. A recruiting agent's hire still needs identity, devices, access groups, and policy checks. As models get better at producing work, the bottleneck shifts to the operational systems that turn that work into durable state.


Enterprise Worlds is a family of executable enterprise environments for measuring that problem directly. Each release is a world with persistent state, tools, policies, and simulated users. The aim is to grow evaluation from isolated tasks to operational jobs: agents that do not just assist with work, but become accountable for completing it correctly. ITSMBench is the first benchmark in that family: an IT service management environment for testing whether agents can carry out policy-governed work inside a system of record.


### ITSMBench: the first Enterprise Worlds benchmark


ITSM is a useful first domain because it sits directly on the control surface of a company: identity, access, incidents, change requests, approvals, ownership, and audit trails.


Most larger companies run some version of a service desk. Someone cannot log in, a laptop will not charge, a team needs a database provisioned, a production service has to be retired, or an incident needs escalation. Around that work sits a system of record: a ticket, a state machine, an owner, a clock, an approval chain, and a policy saying who is allowed to do what.


Those systems are more than support queues. They determine who can access production, how changes are approved, how incidents are escalated, and what audit trail remains after the work is done. That makes ITSM a compact testbed for enterprise operations.


An ITSMBench task runs against an executable ITSM tenant. It contains:


-


**The world:** includes a single-tenant company and a managed-service-provider world serving 20 organizations. The worlds contain 900+ interconnected records across 16 entity types, including users, organizations, incidents, problems, changes, services, configuration items, SLAs, knowledge articles, notifications, groups, roles, and permissions.


-


**The policy:** the operator rulebook given to the agent. It defines identity verification, role and organization scope, table-level integrity rules and ITIL workflow constraints.


-


**The action space:** 93 typed tools for lookup, mutation, and aggregation. Tools enforce valid options, required arguments, and foreign-key integrity, and they generate IDs and timestamps deterministically so runs can be replayed and compared.


-


**The user:** simulated operator with a persona and private` known_info` . They do not hand the agent a perfect task spec up front. Facts surface over conversation when the agent asks the right question.


-


**The reward:** computed after the run from the final environment state. The database state is hashed and compared against the expected final state, with natural-language assertions for requirements that are not database write operations.


The seed data and action space are adapted from ServiceNow's[EnterpriseOps-Gym](https://enterpriseops-gym.github.io/) ; ITSMBench builds on that foundation with multi-turn tasks, user simulation, deterministic rewards, and benchmark tasks targeted at operational correctness.


ITSMBench measures three capabilities that static tool-use benchmarks usually miss:


-


**Multi-turn ambiguity resolution.** Real users rarely provide complete task specifications in one message. The agent has to identify what is missing, ask at the right moment, use the answer to disambiguate records or actions, and stop asking once the task is sufficiently specified.


-


**Policy interpretation under ambiguity.** The agent receives the same rule-book a human operator would follow, then has to apply it to a concrete workflow. That means verifying identity, inferring downstream consequences, separating` REQUIRED` actions from` PROPOSED` and` ASK` actions, and staying within the operator's authority.


-


**Persistent-state operation.** The agent must locate and update the right records in a live relational world. The lookup is often the hard part: a wrong user, incident, configuration item, or tenant scope can make every later tool call invalid.


**One task, end to end**


Consider an admin off-boarding. The user’s request is short: Zachary Moore is leaving today, and his account should be deactivated.


The difficulty begins after the account is deactivated. Under the policy, setting` users.active = false` turns every active reference to Zachary into part of the workflow. The assistant has to verify the operator, find the departing employee, deactivate the account, inspect open incidents, configuration items, change requests, problems, group memberships, and access records, then distinguish which consequences are required from which require operator confirmation.


That last distinction is the task. Group cleanup is a required revocation step. Reassigning Zachary’s incidents, changes, problems, or owned CIs is only proposed: the assistant must find the affected records and present the hand-off, but it cannot transfer ownership without authorization.


So the task tests more than tool-use capabilities. It tests whether an agent can read a one-line request as a policy-governed workflow, recover the hidden dependency graph, ask for the right confirmations, and avoid writing records that the operator did not authorize.


**Example task: employee off-boarding**


**User request:** "Zachary Moore is leaving the company effective today. Please deactivate his account."


**Operator context:** Karen Watkins (` USER_039` ) is an IT administrator at ServiceNow. She can confirm her own identity, and she knows the departing employee's name.


**Hidden workflow:** Deactivating Zachary is only the visible step. Once` users.active = false` , the agent must inspect every active dependency on that user:


-


assigned incidents


-


owned configuration items


-


assigned change requests


-


assigned problem records


-


group memberships


-


access records


**Required action:** Remove Zachary from active groups after operator confirmation.


**Proposed-only actions:** Reassignment of incidents, changes, problems, or owned CIs must be surfaced to the operator, but not performed unless explicitly authorized.


**Evaluator expectation:** The final world state must show that:


-


Karen's identity was verified


-


Zachary Moore was resolved to the correct user record,` USER_045`


-


Zachary's account was deactivated


-


dependent records were discovered


-


membership in` GROUP_007` was removed


-


no unauthorized reassignment or ownership transfer was performed


### Leaderboard


We evaluate each model on 53 ITSMBench tasks with 4 independent trials per task. A trial passes only if the final database state matches the expected final state.


We report two task-level views:


-


` pass@k` : did at least one of` k` independent attempts solve the task? This matches coding workflows, where failed candidates can be discarded and a test runner or reviewer can select the working attempt.


-


` pass^k` : did all` k` independent attempts solve the task? This is the enterprise reliability view: every failed attempt may write records, send notifications, miss approvals, or leave stale access behind.


The gap between` pass@k` and` pass^k` is the benchmark’s reliability signal. Claude Opus reaches` pass@4 = 0.887` , meaning it solves most tasks at least once across four trials. But its` pass^4 = 0.396` : fewer than half of tasks are solved correctly in all four attempts. GLM shows the same pattern even more sharply, with` pass@4 = 0.887` but` pass^4 = 0.208` .


The reliability-gap chart shows the distance between capability and repeatability. Many models can solve a task at least once, but fail to reproduce the same correct final state across repeated trials. ITSMBench also measures whether that capability is stable enough to trust with persistent enterprise state.


The table reports both views: trial-level pass rate, task-level` pass@k` , and` pass^k` reliability across repeated attempts.


Model


pass^1


pass^2


pass^3


pass^4


pass@1


pass@2


pass@3


pass@4


Qwen3.7-plus


0.476


0.318


0.231


0.189


0.476


0.635


0.708


0.736


Claude Opus 4.8 medium adaptive


0.698


0.563


0.467


0.396


0.698


0.833


0.873


0.887


GLM-5p2


0.547


0.365


0.274


0.208


0.547


0.730


0.821


0.887


GPT-5.6 Luna


0.505


0.349


0.274


0.226


0.505


0.660


0.741


0.792


Muse Spark 1.1


0.500


0.302


0.203


0.151


0.500


0.698


0.797


0.849


Nemotron-3 Ultra NVFP4


0.222


0.104


0.061


0.038


0.222


0.340


0.415


0.472


### Error Analysis


We analyzed failed trajectories across runs to identify recurring model failure patterns. The categories are not mutually exclusive: one failed run can miss a required downstream action and also fabricate an input. The point is to show which parts of enterprise workflows still stress current agents.


### Failure #1: Qwen3.7-plus and GPT-5.6: reporting work it never did


*"You have been working incident INC-000054… you want it put on hold because the ticket is blocked until the caller responds."* Putting an incident on hold has a mandatory consequence: the SLA clock must be paused, or the ticket keeps burning time the team will be measured on.


Qwen3.7-plus set the incident to` on_hold` correctly, then closed with a summary:


> **Incident INC-000054 is now on hold** with the reason "Awaiting Caller"; the SLA timer has been paused, so you're all set on that front.


It never called an SLA tool.` TSLA_021` stayed` in_progress` . The same thing happens on three further tasks: Qwen on` iter_007` ( *"the SLA timer is now paused automatically"* ), GPT-5.6 on` iter_008` and` iter_044` ( *"resuming its SLA"* ). In every case, zero SLA calls. Twice the model justifies it with the word **"automatically"** , having invented a cascade the environment doesn't implement and then reported its imagined effect as fact.


**What it should have done:** ensure the incident's SLA row ended in` paused` , using` find_incident_slas` and` update_incident_sla_details` where the environment exposes that state change. Whether described operationally as automatic or not, the verifier reads the database. The reward is read off of the database, not the transcript, so a confident summary is generally ineffective.


This is the only failure mode that leaves a *false* trace instead of no trace. Every other one is visible to an operator reading the conversation; this one reads as a clean success.


#### Failure #2: Opus 4.8 and GLM-5p2: inventing a value rather than leaving it blank


*"One of your critical incidents has run past its service-level commitment and needs to be formally treated as an SLA breach."* The reference marks the breach with two fields:` stage: breached` and` has_breached: true` . It sets no timestamp, because nothing in the world establishes one.


Three trials supplied one anyway, and no two agreed: Opus wrote` 2024-02-19T15:50:00` , then` 2024-02-19T15:00:00` on a rerun, and GLM wrote` 2024-02-20T10:00:00` . In each trial that invented timestamp is the **only** thing wrong; the breach flag, the alert and the escalation note are all correct.


A derived value would be identical across trials. Three different answers from the same evidence is the signature of invention.


**What it should have done:** omit the field. The policy is explicit: *"Never invent or fabricate IDs, serial numbers, contact details, dates, or outcomes… The same applies to optional or default arguments."* An agent that can't leave a field blank can't be trusted with an audit trail.


The same reflex shows up as guessed identifiers in **16 different tasks** : operators speak in display numbers (` INC-000054` ), tools take internal IDs (` INC_054` ), and rather than call the lookup, models rewrite the string. GLM literally sent` find_incident_by_id("INC_000054")` . It usually works on this seed data, which is exactly why the habit survives.


#### Failure #3: GPT-5.6: inventing an authorization rule, then enforcing it


*"You are an IT service manager working an MSP client's incident queue."* Servicing other organizations' tickets is the entire premise of the MSP world.


GPT-5.6 verified the operator, saw the incident carried a different` org_id` , and stopped:


> Because cross-organization problem creation/linking is prohibited, I cannot open the problem from your scope. No records were changed.


No such rule exists. The policy defines access scope by **role** : Administrator, Manager, Agent, Reporter. It says nothing about organizational boundaries. When the simulated operator pushed back correctly ( *"it's an MSP client ticket, so it would be under the client's org"* ), the model escalated rather than reconsidered, and produced a three-step remediation procedure that appears nowhere in the policy.


**What it should have done:** proceed. A differing` org_id` is not an authorization failure; the operator's role already granted the permission.


The failure mode is the inverse of the usual safety worry. This isn't an agent acting without authority. It is an agent manufacturing authority it must wait for, and blocking real work in the name of a rule nobody wrote.


#### Failure #4: all four models: sending the customer email before asking what it should say


*"Send the caller a reminder marked as sent using the provided subject and message exactly."* The wording belongs to the operator; the policy lists *"the exact text of a customer-facing message"* as something the agent must **ask** for.


All four models wrote their own version and sent it. When the operator then supplied the wording they'd actually asked to use, each model sent a **second** email. There is no delete tool, so the caller received both. Every failing trial on this task carries the same mismatch:` notification: unexpected \['NOTIF_006'\]` . The pattern recurs on four further tasks: **15 trials where more mail went out than the task called for.**


**What it should have done:** ask for the subject and message before calling` send_notification` , then send once.


Every other over-write in this benchmark is a database field that could be corrected. This one has already reached a person's inbox.


#### Failure #5: Opus 4.8, Qwen3.7-plus, and GLM-5p2: the audit field left on its default


*"…you want that article officially attached to the incident to reflect that it was applied during the investigation."*` link_knowledge_to_incident` takes a` used_as` argument recording *how* the article relates to the ticket: merely` suggested` , actively` applied` , or the` resolution` itself. The tool defaults to` suggested` .


Across five tasks and three models, the article search and the target incident were **always correct** and the field was **always wrong** . On one task the word "applied" appears in the operator's own brief and the model still submitted` suggested` . On another, Opus ran seven searches to find the right article, then mislabelled the link it had worked so hard to identify.


**What it should have done:** pass` used_as: "applied"` . The policy mandates it for precisely this trigger, and the whole point of the field is that a future auditor can tell a suggestion from a fix.


This is what makes enterprise work hard to fake. The retrieval is the visible part and the models are good at it. The one-word field that makes the record mean something is where they stop paying attention.


### Conclusion and next steps


ITSMBench is the first release of Enterprise Worlds. It tests a narrow but important slice of enterprise work: agents operating inside a policy-governed system of record where partial success is still failure.


The larger goal is to move from isolated operational tasks to long-horizon enterprise work. In IT service management, that means asking whether an agent can eventually own more of the service-desk function itself: handling queues, following policy, coordinating with users, updating records, escalating when needed, and preserving the audit trail over many related actions.


Future releases of Enterprise Worlds will push in that direction. We plan to scale the worlds, lengthen the task horizons, expose agents to broader tool surfaces, and introduce workflows where external systems fail mid-run.


### Cite us


If you use Enterprise Worlds, ITSMBench, or the released task data in your work, please cite:


```text
@misc{enterprise_worlds_itsmbench_2026,
title        = {Enterprise Worlds: ITSMBench},
author       = {Ragas},
year         = {2026},
howpublished = {\url{ <https://github.com/vibrantlabsai/Enterprise-Worlds>


```


```text
@misc{enterprise_worlds_itsmbench_2026,
title        = {Enterprise Worlds: ITSMBench},
author       = {Ragas},
year         = {2026},
howpublished = {\url{ <https://github.com/vibrantlabsai/Enterprise-Worlds>


```


```text
@misc{enterprise_worlds_itsmbench_2026,
title        = {Enterprise Worlds: ITSMBench},
author       = {Ragas},
year         = {2026},
howpublished = {\url{ <https://github.com/vibrantlabsai/Enterprise-Worlds>


```


```text
@misc{enterprise_worlds_itsmbench_2026,
title        = {Enterprise Worlds: ITSMBench},
author       = {Ragas},
year         = {2026},
howpublished = {\url{ <https://github.com/vibrantlabsai/Enterprise-Worlds>


```


### References


-


ServiceNow EnterpriseOps-Gym:[https://enterpriseops-gym.github.io/](https://enterpriseops-gym.github.io/)
