---
schema_version: "1.0.0"
document_id: "d61a400653def968cdc82bca60b7b8cb8e743a2942ed740599cd7cf335b8102a"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-fc8f7e880d65"
canonical_url: "https://emergent.sh/blog/building-security-into-wingman-from-the-start"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-21T18:00:00.368523+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:2d6c0cba2d75f775b3f11e1d761cfdbc9cf2a532061c2b27fe5a3b2dfbe3b80a"
---

# Building Security Into Wingman From the Start

Wingman is Emergent's personal AI assistant for the places where work already happens. Users can interact with it through channels such as iMessage, WhatsApp, and Telegram; connect it to applications such as Gmail, Calendar, Slack, Drive, GitHub, Notion, and their CRM (over 300 applications in all) and ask it to complete work that spans messages, documents, meetings, repositories, and scheduled automations. Wingman understands context, coordinates across tools, and runs tasks autonomously in the background, so you can come back later when something needs your attention.


Those capabilities require equally serious safeguards. A connected agent has access to more than a chat transcript: it can read an email, follow a link from a calendar invite, summarize a shared document, inspect a GitHub issue, search a CRM, create a schedule, send a message, update a permission, or run code in a sandbox. The same capabilities that make Wingman useful would create new attack surfaces if untrusted content were allowed to steer the agent's actions.


So we built Wingman's security in from the architecture up, around trust boundaries, action controls, sandboxing, auditability, and continuous adversarial evaluation, and we keep validating those choices through internal red teaming, continual regression evals, external assessments (including a pre-launch engagement with Straiker's adversarial and research arm, STAR Labs), and a coordinated researcher-disclosure program. The result is a system where no single layer has to hold on its own: if one defense is bypassed, the others still stand.


This post walks through that work: the risk model we designed around, the layers of defense, how we validate them, and how we keep evolving the controls as the product grows.


## Why Connected Agents Need a Different Security Model


Traditional software keeps a clear line between data and instructions. Emails, calendar invites, GitHub issues, shared documents: all of it is data.


For an AI agent, that line is harder to hold. The agent reads content in order to decide what to do next. If the content carries malicious instructions, social engineering, fake task boards, encoded messages, or links to attacker-controlled pages, it can possibly hamper the agent's reasoning. This is the core of indirect prompt injection: instructions hidden in content the user did not author, but that the agent is asked to process.


Traditional software keeps a hard line between data and instructions, so authority stays explicit and untrusted data cannot cause actions. An agent reads content in order to decide what to do next — so untrusted content can influence actions across authorized systems. Everything that follows is about closing that gap.


Wingman makes that risk more consequential because it can act across user-authorized systems, taking context from one service and performing actions in another, often without a user watching. The security question is not only "can the model identify a bad prompt?" It is "can the product prevent untrusted content from causing unauthorized side effects?"


Our core design principle is conservative:


Content that did not come directly from the authenticated user should not automatically inherit the user’s authority.


Everything else follows from that.


## A Defense-in-Depth Architecture


No single defense mechanism is sufficient for agent security because the risk spans the whole path from content ingestion to external side effects. Model instructions can teach the agent how to treat untrusted content. Classifiers can identify suspicious inputs and risky actions. Confirmation flows can bring the user back into the loop for high-impact decisions. Sandboxes can limit what code and tools can affect. Logging can make decisions reviewable and replayable. Each layer reduces a different failure mode, and yet none of them is enough on its own.


Wingman's defenses are distributed across the task lifecycle: before content enters the reasoning loop, while the agent plans, before tools execute, during sandboxed work, when background tasks run, and after actions are logged for review.


Live requests, external content, and scheduled runs all clear the same checkpoints — scheduled work is a parallel task type that passes extra safeguards (06) before joining the pipeline. Evaluation and audit (07) run asynchronously over the recorded path rather than sitting in the request path. Numbers match the sections below.


The following sections describe each layer and the rationale behind it.


### 1. Trust Zones and Provenance


Wingman tracks different categories of context, including direct user instructions, connector content, fetched web content, tool outputs, files, browser state, scheduled-task context, and generated artifacts, and treats them with different levels of trust, even when they appear together in the same prompt. The practical goal is provenance: when the agent proposes an action, the system should know what content influenced it, where that content came from, and whether the source had authority to request that kind of action.


This matters most in multi-hop workflows. If an external calendar invite links to a page, and the page asks the agent to read a mailbox and post publicly, the linked page should not become trusted merely because the agent fetched it. It should inherit the trust context of the original invite. Realistic attacks rarely place the payload in the first artifact; they use the first artifact to route the agent to a second one. Provenance has to follow the path, not the hop.


### 2. Policy Outside the Model


We do not treat the model as the final authority for security-sensitive actions. The model can reason about a task and propose actions, but product policy decides whether those actions are allowed, blocked, logged, or require confirmation. That policy layer evaluates source trust, user intent, connector permissions, data sensitivity, action type, destination, and whether the task was initiated live or through an automation.


Prompt injection is, in effect, adversarial pressure on the model's decision-making. If the only control is asking the model to ignore bad instructions, the attacker gets to keep trying to manipulate the control itself. Wingman's control plane is designed so that tool calls are reviewed as actions with consequences, not just as text generated by a model.


### 3. Sensitive-Action Gates


Agent security is ultimately about actions. Wingman applies additional gates to high-impact operations: external sends, public posts, permission grants, destructive changes, repository-impacting operations, credential-adjacent actions, code execution, and scheduled-automation creation or mutation. Depending on context, those gates can include explicit user confirmation, stricter policy checks, source and destination validation, reduced tool capability, sandbox isolation, or outright blocking.


The confirmation experience is itself part of the security surface. A useful confirmation makes clear what action is about to happen, which destination is involved, what content caused it, and why it may be sensitive. We continue to improve those surfaces so users can make informed decisions quickly without normal work becoming noisy.


### 4. Cross-Connector Data Flow Controls


The highest-risk agent behaviors span multiple tools: reading from a drive and sending through a mailbox, pulling CRM data and forwarding it externally, reading email and posting to a public account. Wingman evaluates these source-to-destination paths rather than treating each connector call in isolation. The policy question is not "does the user have access to both tools?" but "should data from this source move to that destination for this user request?"


This matters because ordinary personal-assistant tasks routinely cross systems. The product needs to allow legitimate workflows while stopping unexpected movement of sensitive data through context-aware policy, not blanket disabling of useful integrations.


Your browser does not support the video tag.


Every flow — legitimate or laundered — meets the same source→destination policy wall. The untrusted label rides every hop, so the wall still stops the last step even when the first artifact looked benign. Ordinary work passes; the two attack shapes do not.


### **5.** Sandboxed Execution


Wingman uses the same orchestration and sandboxing foundations that power Emergent’s app builder. That infrastructure lets agents perform useful technical work while constraining the environment where code runs. Generated code, fetched code, and repository-sourced scripts run with strict limits on filesystem access, secrets, network egress, process behavior, and persistence; destructive operations and scheduler-driven execution get separate scrutiny.


Red-team exercises consistently reinforce why this matters: untrusted instructions will try to turn normal developer workflows into code-execution paths. The runtime has to be built so that a malicious task cannot freely affect the host, the user’s unrelated data, or other connected systems.


### **6.** Scheduled Task Controls


Background tasks are one of Wingman’s most useful capabilities and a distinct trust boundary. A scheduled run may execute when the user is absent, may process new connector content that did not exist when the schedule was created, and may repeat an action many times. Creating or modifying a schedule is not equivalent to answering a chat prompt.


Wingman’s scheduler controls are designed around explicit user intent, review for high-impact recurring actions, and policy re-checks at execution time. The system does not assume that an action is safe forever because it passed once. Context changes, content changes, and connected permissions change.


### **7.** Adversarial Evaluation and Auditability


We evaluate Wingman as an acting system, not just as a model that produces text. An agent can produce a harmless-looking final response while attempting a risky tool call mid-task; the security outcome is the action path, not the last message. Our evaluations track that full path: the user’s request, retrieved content, provenance labels, intermediate reasoning, proposed tool calls and parameters, policy decisions, confirmations, denials, sandbox activity, scheduled runs, and final side effects on seeded integrations.


The same evaluation mindset we use to measure whether Wingman completes useful tasks also applies to whether it completes them safely. Security is not a separate manual review that happens after performance testing; it is part of the eval suite. We use Harbor, an open-source framework for evaluating agents in sandboxed environments, as one component of that pipeline alongside our internal scenario suites.


A security eval can fail even when the final assistant message sounds reasonable, because the verifier looks at what the agent actually attempted. This includes which tools were called, what data was read, whether a cross-connector transfer or public action was triggered, whether a scheduled task was created, and whether the system stopped at the right policy boundary. Every meaningful finding becomes a replayable regression case. The standard is not “did the model say the right safety sentence” but “did the system prevent the unsafe action path from completing?”


Your browser does not support the video tag.


The green ending is a trap the eval sees through: a harmless-sounding final message still fails, because the flagged tool call is what’s scored. Below: the shape of the external pressure-test that exercised exactly these paths.


## How We Validate These Controls


Defense-in-depth is only meaningful if it is exercised. Wingman's validation program combines several tracks because each catches a different class of mistake:


**Architecture review and threat modeling.** Before a new connector, tool, or workflow ships, we review the source and sink behavior it introduces: what content can enter the agent through that surface, what actions the agent can take because of it, what data sensitivity is involved, and how the new path interacts with existing controls.


**Internal red teaming.** We test known attack classes across connectors, browser sessions, code execution, and scheduled tasks, especially around high-risk changes and release candidates.


**Continual regression evals.** Harbor-based sandboxes and internal scenario suites help catch both task-quality and security regressions as models, prompts, connectors, and policies change.


**External assessment.** Before public launch, we engaged Straiker STAR Labs to pressure-test Wingman end-to-end against attacker-controlled content across email, documents, calendars, repositories, social messages, browser pages, CRM data, and scheduled tasks.


**Coordinated disclosure.** We work with the broader security research community and continue to invest in that relationship as Wingman's surface grows.


These tracks are not interchangeable. Architecture review catches design-time risks. Internal red-team scenarios exercise known classes of failure. Continual evals make security behavior part of the same engineering loop as product behavior, so a model swap, prompt change, or new tool does not quietly weaken prompt-injection resistance, connector policy, sandboxing, or scheduler behavior. External review looks for uncomfortable paths internal teams tend to miss;[Straiker's blog](https://www.straiker.ai/blog/straiker-red-teams-emergent-wingman) describes that methodology in detail.


Inputs from these tracks feed an active hardening backlog. Recent priorities include preserving trust context across links, fetched pages, attachments, and scheduled runs; treating cross-connector data movement as a source-to-destination policy decision; tightening confirmation around external sends, public posts, permission grants, destructive actions, and scheduler changes; strengthening sandbox constraints for generated or externally sourced code; and focusing evaluations on side effects rather than model refusals. Meaningful findings become regression evals for future releases.


We do not treat any of this as a finish line. What it produces is a system in which architecture, product surfaces, and evaluation programs have all been pressure-tested against realistic attack paths before users rely on Wingman for serious work, and a discipline of catching the next class of issues before it ships.


## How These Defenses Hold Together


The layers above reinforce one another rather than stand alone: provenance shapes what the policy layer sees, connector permissions set the baseline access boundary, action policy reviews sensitive tool calls against intent and trust, and confirmation flows bring the user back into the loop for high-impact actions.


Concretely, our launch posture is organized around these control layers:


Layer


What it does


Provenance tracking


Distinguishes direct user instructions from connector content, fetched content, tool output, generated artifacts, and scheduled context.


Connector permission boundaries


Uses user-authorized connectors and service-level permissions as the baseline access boundary.


Action policy


Reviews sensitive tool calls against user intent, source trust, destination trust, data sensitivity, and action type.


User confirmations


Requires approval for selected high-impact actions such as external writes, destructive changes, permission grants, public posting, code execution, and automation changes.


Cross-connector controls


Restricts unexpected movement of sensitive data between services, especially toward external or public destinations.


Sandboxed execution


Runs agent’s work in constrained environments with boundaries around filesystem access, credentials, network behavior, and persistence.


Scheduler safeguards


Treats background and recurring tasks as high-impact workflows with explicit intent and policy checks.


Audit logging


Records tool calls, policy decisions, approvals, denials, scheduled runs, and action outcomes for review and regression testing.


Adversarial evals


Exercises realistic indirect prompt-injection scenarios across live and autonomous workflows.


Continual regression evals


Uses agent evaluations, including Harbor-based sandboxed tasks, to catch regressions in task performance, prompt-injection resistance, connector policy, sandboxing, and scheduler behavior.


The table is a summary, not a claim that every connector and workflow has identical depth. Coverage is not uniform, and we are explicit about that: depth varies by connector, action type, and feature surface. As Wingman adds integrations and workflows, our policy and evaluation coverage scales with them.


## What We're Taking Forward


Several principles now shape how we keep building:


**Prompt injection is an action-layer problem.** The important question is not only whether the model can be tricked, but whether a tricked model can cause a harmful side effect.


**Trust has to be carried, not inferred.** Content does not become safe because it was retrieved by a tool. Links, attachments, fetched pages, transformed data, and scheduled context need provenance.


**Cross-connector flows need policy.** Reading from one service and writing to another is a path that needs review against intent, source trust, destination trust, and data sensitivity.


**Autonomy changes severity.** Scheduler creation and mutation are core security controls, not convenience features.


**Validation has to be multi-track.** Internal review, continual evals, external red teams, and researcher disclosure each catch different mistakes.


Wingman will keep gaining capabilities, and the security program will scale with them, and we will keep being precise about what is shipped, what is running in shadow, and what is still being built.


Prompt injection is not a solved problem, here or anywhere in the industry. What we can do is keep narrowing the paths through which untrusted content becomes unauthorized action, and keep improving as attackers adapt.


If you believe you have found a security issue in Wingman or another Emergent product, please report it through Emergent's security disclosure process before any public disclosure. We work with the broader research community as a matter of practice, not as a one-time pre-launch event, and we welcome continued collaboration as Wingman grows.
