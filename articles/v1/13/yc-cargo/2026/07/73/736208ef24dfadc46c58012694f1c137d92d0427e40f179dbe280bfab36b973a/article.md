---
schema_version: "1.0.0"
document_id: "736208ef24dfadc46c58012694f1c137d92d0427e40f179dbe280bfab36b973a"
company_key: "yc-cargo"
company: "Cargo"
source_id: "yc-cargo-news-import-f4a8864e899b"
canonical_url: "https://www.getcargo.ai/blog/gtm-as-code"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-27T05:22:40.350880+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:bfd2f83a2aa67b0df4adf0b81e373e50582948c66f861f6efd8ef359fd5eab55"
---

# GTM as Code: Run Your Go-to-Market as Software

> GTM as code means revenue logic stops living in people’s heads and disconnected workflow builders. It becomes explicit, auditable, reusable and testable.


The new thing is not version control. It is who can now author the system. AI coding agents can understand a repository, change interconnected revenue resources, explain the diff and prepare a deployment. That makes the repository a practical operating surface for GTM, not a developer-only backend.


Infrastructure made this move with Terraform. Analytics made it with dbt. GTM is the third time this movie plays, and this time AI has removed the authoring barrier that kept revenue logic trapped in visual workflows.


## The black box you run revenue on#


Every company has revenue logic. It decides which accounts matter, which contacts to target, what signals mean buying intent, how leads are scored, who owns each opportunity, and which action happens next.


In most companies, this logic is scattered everywhere. Some lives in CRM filters. Some in automation tools. Some in documents. Some in spreadsheets. A significant part lives only in the head of the RevOps person who configured the system six months ago.


The engine makes thousands of decisions every day, and the company cannot clearly see how.


Why was this account classified as Tier 1?


Why was this lead assigned to Sarah?


Which version of the scoring model is running right now?


What changed last Tuesday?


Most GTM teams cannot answer these questions without opening five tools, asking three people, and reconstructing the logic by hand.


There is a name for this way of working: ClickOps. Someone opens a workflow, changes a filter, presses save and hopes every dependent system still behaves correctly. The problem is not that the change happened through an interface. The problem is that the resulting logic is difficult to review, reproduce, test or reconcile with the rest of the engine.


That is not a revenue engine. It is accumulated operational debt.


## Why now: AI changed the entry point#


For a decade, the visual interface was where GTM systems were built, because clicking was the only practical way in for an operator who does not code. Every vendor optimized for it. The canvas, the filter panel, the drag-and-drop builder: the interface was the product.


AI coding agents removed that constraint.


An agent can now read the repository that defines your revenue engine, take a change described in plain language, modify the interconnected pieces, explain the resulting diff and prepare the deployment. The operator no longer chooses between learning to code and dragging boxes through a canvas. They describe the change, the agent implements it, and they review the business impact.


The authoring model shifts from a human clicking every box to a human defining intent, an agent writing the change, and a human reviewing it before it ships.


AI makes the interface cheaper. It makes the underlying system more valuable.


## What GTM as code actually means#


GTM as code is a way to manage the company’s revenue architecture with the same discipline engineering uses to manage product infrastructure.


GTM as Code is the practice. The Revenue Engine is what the practice produces: a system your company can inspect, trust, and improve.


Engineering teams crossed this line years ago. Nobody reconstructs how production works from screenshots and memory: the system is defined, changes are reviewed before they ship, previous versions can be restored. Revenue deserves the same discipline. Not because GTM needs more engineering complexity, but because revenue logic is now too important and too interconnected to remain informal.


Concretely, the architecture has five parts.


The revenue engine is defined in a repository: the source of truth. Cargo’s CDK provides the code primitives used to define its models, connectors, workflows, tools, agents and plays. AI agents author proposed changes against that repository. The CLI previews each change and deploys the approved version to the runtime that executes it. And the interface is where people observe the engine, operate it, and arbitrate the decisions that still require judgment.


## Every revenue decision needs a receipt#


“Auditable” sounds like compliance language. The business meaning is simpler: every decision the engine makes can be traced back to the rule and the version that produced it.


Finance would never accept a P&L made of numbers nobody can trace. Every important financial figure has a source, a calculation, an owner, and a history. GTM teams accept the opposite every day: their systems decide prioritization, ownership, and expansion, and the reasoning is invisible.


Run your own stack against the Receipt Test. Five questions:


1. What happened?
2. Which data was used?
3. Which rule produced the outcome?
4. Which version of that rule was running?
5. Who changed it, and can we restore the previous version?


If your revenue engine can answer all five, you have receipts. If not, you have a black box with a pipeline attached.


And a change needs its own receipt: the author, the diff, the expected impact, the approval, and the version to restore. If either chain breaks, the engine records activity without being accountable.


## ”What changed last Tuesday” finally has an answer#


Say your company changes its definition of a Tier 1 account.


Before: employee count above 50, ICP score above 70.


After: employee count above 100, ICP score above 80, at least two RevOps people on the team.


Under ClickOps, someone edits a few filters. The change takes five minutes. The consequences last for months. Six weeks later, nobody remembers the previous thresholds. The documentation still describes the old definition. Another workflow quietly keeps using the old cutoff. The CRM report and the routing system now disagree about who is Tier 1, and nobody knows.


Under GTM as code, the same change is a proposal before it becomes a fact:


**Tier 1 definition, proposed change**


- Minimum employee count: 50 → 100
- Minimum ICP score: 70 → 80
- New requirement: at least 2 RevOps employees


**Preview before release**


- 184 accounts leave Tier 1
- Sarah’s territory loses 31 accounts
- 2 automated plays stop targeting the removed accounts


The team reviews the business impact through the diff and the deployment preview, rather than reconstructing the logic across hidden filters. Then it ships. Compare what each path preserves. ClickOps loses the previous logic, the author, the dependency map, the documentation’s accuracy, and any real path back. The repository keeps all five: if the new model performs badly in production, you restore the previous version in one step.


The CDK makes revenue logic inspectable, versioned and reproducible. The value was never the syntax. The value is knowing exactly what changed, why, and what is running right now.


## The economics of an invisible engine#


Most GTM failures are not dramatic enough to trigger an incident. A high-intent account routed to the wrong territory. A customer with expansion signals excluded because one field is missing. A scoring rule updated in one workflow but not the other. Each looks small. Across millions of account updates, routing decisions, and alerts, it becomes silent revenue leakage. Without receipts, you see pipeline dip and conversion drop, and you investigate symptoms across five tools. With receipts, the investigation starts from the actual decision path.


The invisible engine also taxes speed. Teams slow down when changing one scoring rule might silently alter territories, campaigns, alerts, and rep queues. People either stop touching the system or break it confidently. When every change is previewed and every release is reversible, the team experiments more, not less.


Explicit logic pays twice over. It can be reused: a new market or business unit starts from the working engine, changing territory, language, thresholds and providers, instead of rebuilding workflows whose logic immediately starts to drift. And it can be tested: apply next quarter’s scoring rules to last year’s pipeline before trusting them with this quarter’s. Would the new model have rejected your best customers? Would the new routing overload one seller? You answer with evidence, before production, instead of with a rep complaint six weeks after.


One more cost disappears: the playbook and the production system stop being two diverging artifacts. The rule in the playbook and the rule running in production become the same thing.


## The interface changes jobs#


None of this makes the interface disappear. It changes what the interface is for.


When the repository holds the source of truth, the interface stops being the place where the engine gets built box by box. It becomes the place where people observe execution, investigate a decision, approve a change, override an outcome, and resolve the cases that genuinely need human judgment.


A custom interface earns its place when it concentrates judgment. It gives a person the evidence to resolve an ambiguous case, records who decided and why, and turns that decision into structured feedback for the next case. That is not ClickOps: the decision is recorded, attributed, and incorporated into the source of truth.


Humans do not leave the loop. They move up in it. Agents can write the implementation. Humans still have to define the policy.


“Prioritize good accounts” is not a policy. “Prioritize companies with an ICP score above 80, two or more RevOps employees, recent funding, and no active opportunity” is a policy. AI lowers the technical barrier. It does not remove the need for rigorous GTM thinking. It exposes vague thinking faster.


## This is not theoretical#


The primitives are public:@cargo-ai/cdk is the package used to define Cargo resources in code inside a repository. The Cargo CLI previews and deploys those definitions.


We run Cargo’s own pipeline this way. Changes to our engine ship like product releases: previewed, reviewed, reversible.


And it is not only us. Braintrust runs its GTM out of a Git repository: revenue logic in code, versioned, and theirs to keep.


And the demand side is already there. We have had prospects ask about version history before we introduced the idea ourselves. They were not asking for a developer feature. They wanted to know whether a critical revenue change could be traced and reversed.


## Codification is the prerequisite, not the destination#


One last thing, because it frames where this goes.


Codification is not the final destination. It is what makes the next step possible. Once revenue logic is explicit, versioned and testable, agents can reason about it, simulate changes, and eventually propose improvements. A machine cannot safely improve an engine whose rules are scattered across disconnected interfaces and human memory.


The future revenue engine will not be built by clicking through an ever-growing maze of workflows. Humans will define its intent. Agents will write and test its logic. The repository will preserve its memory. And the interface will concentrate human judgment where it still creates leverage.
