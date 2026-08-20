---
schema_version: "1.0.0"
document_id: "69680d1a4c45693de6c693cdea10e8d4c4d5a160dcca834beaecb2ae229268d9"
company_key: "yc-capitol-ai"
company: "Capitol AI"
source_id: "yc-capitol-ai-news-import-f6452f568795"
canonical_url: "https://www.capitol.ai/blog/how-to-make-agentic-ai-accountable"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-23T04:42:41.632332+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:0f8fbd9cbd90dd8c7a3f53b9746b6c059942c396d3196abb31fee38f623078de"
---

# How Regulated Institutions Keep Agentic AI Accountable

*By Shaun Modi, CEO, Capitol AI*


**TL;DR** Agentic AI is useful for a regulated institution only when the work product can be trusted. Trust takes time, process, and requires more than just a capable model - it requires accountability. The organization needs to know which approved workflows the agents should run and who is responsible for it, what data has entered the process, where in the process a human must review or approve the work, and whether the final output meets a defined quality standard. If something breaks, there needs to be a way to find the reason it broke and fix it. The practical foundation is a governed workflow with versioning, execution history, evaluation, and deliberate human control.


## **AI adoption has created a governance problem for enterprise**


Enterprise AI adoption shows no signs of slowing but it usually enters the institution one person at a time. For example, an analyst builds a useful prompt for ChatGPT to pull data from an online database using a connector or a business unit links a model to a document archive to synthesize the data quicker for a certain workflow. Someone else can build an internal automation workflow driven by an LLM-based AI agent, of which there are plenty these days. Each experiment may be productive on its own, but the organization quickly loses sight of what AI tools are used, who owns them, which data they access, and if sensitive internal data is uploaded to the cloud. There is little an organization can do to control for these use cases besides tightening the rules and limiting usage. This may come at a high cost of productivity and is not the right long-term solution for any organization wishing to remain competitive in the age of AI.


The challenge is not as simple as improving model quality since even the strongest frontier model can produce work that the institution would not approve for a variety of reasons. When a risk committee asks which process produced a report, which sources were used, or whether a junior employee changed the instructions, pulling chatbot history is not adequate. Some enterprise tools do retain prompt history and admin logs, but not with a claim-level audit trail and a reproducible record tied to an approved workflow.


Even the draconian decision to restrict or ban AI rarely resolves this. People continue using the tools because the productivity gain is so significant, the incentive of time savings is so great, and the competition with the person in the next seat is intense. A more reasonable path is to replace improvised use with a sanctioned operating model that is easier to use than the workaround. For starters, that model needs an inventory of approved workflows, clear ownership, role-based access, visibility into usage and cost, and a way to compare / benchmark one workflow or run with another. The right governance question isn’t whether employees use AI (they will anyway), but whether the institution can build proper guardrails and control how AI is used to the point where they can trust the output.


## **Enterprise AI governance starts with three critical roles**


A practical governance model separates the people who oversee and govern the environment (admins), the people who design institutional workflows (Subject-matter experts, senior knowledge workers), and the people who then run these workflows (everyday employees who create deliverables and service clients as well as executives who want fast outcomes).


1.


**The admin** controls access and entitlements. Admins decide who can build the skills and workflows, who can run them, which datasets a workflow may use, and which controls must be present before the workflow is made available. They also decide if and where a human is needed in the loop.


2.


**The builder** turns institutional knowledge into a reusable workflow within the guardrails set up by the admin. A builder might be a senior analyst, a diligence lead, or a compliance officer, usually a senior employee and SME. They define the sequence of work, the permitted sources to be used in each step, the tools called at each step, the expected artifact, and the tests applied to the result. They set the evals and decide what it means for the output to be “good”.


3.


**The user** runs an approved workflow without changing its data source or underlying logic, they are the beneficiary of the work done by the other roles. The interface can remain conversational, but the process behind it is controlled by the admin and builder. The user can be someone senior (C-Suite) who only wants the "answer" according to an established process, or it can be someone junior who is working on a deliverable.


This division of responsibility and accountability lets an institution scale expertise and codify institutional memory. It allows access to approved workflows without giving every user the ability to redesign (or change) the process. A junior analyst can run a workflow built by a senior SME, while the approved sequence, data access, and quality controls remain locked for the next time someone else needs it.


## **A scalable, governed workflow is built from multiple smaller steps**


Enterprise access controls are needed, but they do not themselves define how the work should be done - the workflow needs to be documented and codified. A user can have the right permissions and a good prompt yet still produce inconsistent results because the task is not done in a determined and defined way. Also, different users will see different results over time interacting with an AI agent simply because they have differing styles of work and the agent remembers and adapts.


A proper governed workflow breaks the work into explicit steps or **nodes** . Each node is responsible for its own thing. One node may reason about the goal and determine what to do next, another will pull information from approved sources, yet another may perform some analysis, and a later node may check the numbers, request human approval, or generate a slide deck or spreadsheet if that was the original goal. Each step has a defined purpose, receives only the inputs it needs, and passes an expected output to the next step, which makes the process easier to inspect and maintain than a prompt.


Breaking the task into smaller, sequenced operations does not magically make an LLM deterministic and fail proof. It does make the process easier to test, audit, improve, and repeat. The institution is then no longer relying on every user to remember the right prompt or apply the right review process. More importantly, each time any user needs a “pitch deck,” the same workflow is triggered and a consistent output is produced across different users.


## **Human review should happen at a defined point, with deliberate control by the organization**


Having a human in the loop is a start, but the organization must be deliberate about the way the human interacts with, guides, and approves the workflow. A regulated process needs to identify where review occurs, what information the reviewer sees, what authority that person has, and what the system does while it waits.


A human review checkpoint can be placed anywhere in the workflow, but its location should reflect the actual risk. A team might require approval after a financial calculation, before a client-facing document is generated, before an external tool is called, or only when an evaluation falls below a threshold. The workflow should pause, present the relevant evidence to the reviewer, record the decision, and continue only under the conditions the organization has defined.


## **Evaluations turn expert judgment into a repeatable test**


Human review is expensive when every output receives the same level of attention. Evaluations help the institution distinguish routine work from exceptions that need an expert.


Evaluations convert expert judgment into repeatable, quantifyable tests. Subject-matter experts and administrators define the standards that matter for the task, and those tests run against workflow outputs before the work moves forward. An evaluation can pass a result, flag it for remediation, or route it to a reviewer. Different evaluation types answer different questions:


**Eval type**


**What it measures**


Pass / fail


Whether a defined requirement was met, such as including a disclosure or using only approved sources.


Score-based


How well the output performs against a rubric and whether it clears a required threshold.


Reference output


How closely the output resembles an approved example or benchmark.


Goal achievement


Whether the workflow completed the objective it was designed to accomplish.


Output quality


Quality dimensions such as clarity, completeness, relevance, and tone.


Quantitative checks


Whether numeric output is well formed, internally consistent, and within expected ranges.


Grounding, provenance and citations


Whether a claim or data is grounded in a verifiable source, tracing its provenance, and validating the citations in the relevant artifacts or reports for any such claim or data.


Evaluations do not remove human judgment but rather make it possible to apply that judgment at scale. Outputs that meet the standard are used and those that miss it can be sent back with feedback or routed to an SME. This self-improving loop is important - as SMEs can catch mistakes and make corrections, each failed eval makes the system smarter for next time. The result of each evaluation is stored, so if a certain workflow keeps failing, teams can understand the recurring failure patterns, see why and where it breaks and improve the workflow for everyone who runs it thereafter.


Over time, the workflow becomes a codified and current expression of the institution's process rather than a collection of prompts known by a few individuals. Successful methods are reused which allows institutional knowledge to compound instead of languishing in isolated chats or leaving with the employee who created them.


## **Remember: repeatability is good but it does not equal determinism**


Large language models are non-deterministic by design. The same model can produce a different response when it receives the same request twice and this is not something you can easily “fix”. A robust enterprise architecture should acknowledge that limitation and work with it in mind.


The goal is to manage that variability. For instance, smaller nodes reduce the number of things the model must do at once. Defined sources constrain retrieval and this also helps against hallucinations. Evaluations test the output keeping results within accepted thresholds even if some variability occurs. Human approval then catches exceptions and the run history makes it possible to investigate a result and audit the workflow without repeating the entire process.


That is the practical standard now for high-stakes AI: a controlled process that improves the probability of a reliable result, catches errors before they move downstream, and preserves enough evidence for the right person to review the work.


## **Data controls are a critical part of the architecture**


Agentic workflows often touch more of an institution's data than a chatbot does and that makes deployment architecture and provider terms central to the governance model.


Deployment should match the sensitivity of the workflow and the institution's security requirements. For example, some use cases will run in managed SaaS, while others require single-tenant infrastructure, a private cloud or VPC, or a fully on-premise environment. In every case, data access should be governed by role (as discussed above), source, workflow, and purpose rather than granted wholesale to the model.


Where external model APIs are used, the workflow should send **only the scoped data required for that step** , with redaction or filtering applied where appropriate. Zero-data-retention arrangements can further reduce exposure, while a model-agnostic architecture allows the institution to select different approved models for different tasks without rebuilding the operating process around a single provider.


Auditability, quality control, and data security are therefore not separate workstreams. They are parts of the same operating model: approved workflows, controlled access, scoped data movement, recorded execution, measured output, and human review where required. If one of those elements is missing, the institution may still have an impressive demonstration, but it does not yet have a dependable production system.


Capitol's view is that high-stakes AI should produce more than an answer. It should produce a finished artifact together with the evidence needed to understand how that artifact was created. The platform is designed around sovereign data, governed workflows, model independence, evaluations, execution traceability, and human control. Book a demo to see how those principles are applied to a real institutional workflow.


## **Frequently Asked Questions**


**How do you audit an AI-generated report?** Review the execution record that produced it. A complete audit trail should include the inputs, the output of each workflow step, the tools and models used, evaluation results, generated artifacts, and any human approvals. The reviewer should be able to move from the finished report back through the workflow to the supporting source material.


**Can you add a human approval step to an agentic workflow?** Yes. The approval point should be placed where the organization has identified meaningful risk, and the reviewer should receive the evidence needed to make a decision. The workflow pauses, records the approval or rejection, and proceeds according to the rules defined for that process.


**How do you reduce the risk of employees moving sensitive data into public AI tools?** Provide a governed alternative that helps them complete the work they are trying to do. Approved workflows, role-based data access, scoped model inputs, suitable deployment options, and organization-wide visibility reduce the incentive to move proprietary information into unsanctioned tools.


**Is agentic AI output deterministic?** Language models remain non-deterministic, even when the surrounding workflow is highly structured. A governed system manages that variability by decomposing the task, constraining sources and tools, using deterministic code where it is better suited, evaluating outputs, routing exceptions to human reviewers, and preserving the complete run for inspection.


**How does Capitol think about enterprise AI?** Capitol believes the durable value sits above the model layer. Models will improve and change, but institutions still need control over their proprietary data, the ability to encode expert process into reusable workflows, freedom to choose the best model for each step, and a defensible record behind every finished artifact. The goal is to help institutions understand the world better without surrendering control of the data, methods, and judgment that make them distinctive.
