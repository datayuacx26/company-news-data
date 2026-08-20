---
schema_version: "1.0.0"
document_id: "d41fdf796fc43764a8695ed2738e7ab61a11101cd44801a744cdf79e2065d320"
company_key: "yc-scispot-io"
company: "Scispot"
source_id: "yc-scispot-io-news-import-92ee0439da27"
canonical_url: "https://www.scispot.com/blog/how-a-peptide-lab-fixed-broken-experiment-completion"
published_at: "2026-07-13T20:11:05.904+00:00"
first_seen_at: "2026-07-24T00:53:05.726137+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:2c242aeb2c76757202e4a600bdda0ddfac81612da01dad00fd37ff4913257cc1"
---

# How a peptide lab fixed broken experiment completion

## **Lab context**


This lab works on bioactive peptide candidates, with computational biology and wet lab teams sharing workflows for proteomics and other assays.


Their day-to-day reality is multi-step experimental pipelines, reusable templates, and pressure to keep results consistent, auditable, and ready for regulated and commercial use.


## **The core problem**


As peptide programs scaled, the lab’s existing workflow patterns started to fail in subtle but serious ways.


The issue was not “no software” but that protocols, templates, and completion logic did not reflect what actually happened in a live proteomics environment.


## **Unreliable completion signals**


Experiments were often marked complete while work was still in progress, while similar runs remained incomplete despite following the same protocol structure.


Progress indicators inside the system were inconsistent, which forced scientists to second-guess workflow state and spend extra time validating what was truly done.


## **Protocol and template friction**


The lab relied on templates and dedicated folders for recurring assays, but the way templates were embedded and reused led to inconsistent behavior.


Protocol instances were sometimes duplicated or embedded out of sequence, which made completion logic hard to trust across experiments.


## **Manual effort to interpret state**


Because the system view of experiments was unreliable, scientists had to manually dig into history, toggle steps, and cross-check records to understand real status.


This overhead made it harder to repeat work with confidence, especially when many people depended on the same assays.


## **Why the old stack hit a wall**


The lab’s previous setup combined instruments, isolated scripts, templates, and business systems without a single operational backbone.


That was manageable for smaller workflows, but once proteomics processes became central, the mismatch between lab reality and system behavior created too much friction and risk.


## **Loss of trust in “system view”**


Disjointed protocol usage meant even well-designed templates failed to give consistent completion signals across experiments.


Over time, trust in the “system view” eroded, and teams pushed verification into email, meetings, and manual checks, which slowed them down and made scaling feel risky.


## **Introducing a connected operating layer**


The lab adopted Scispot as a connected operating layer focused on fixing real workflow execution, not just adding another app.


This environment combined structured labsheets, protocol execution, notebook-based analysis, and automation on a single data backbone.


## **Labsheets tuned to peptide work**


Scispot set up labsheets that matched the lab’s peptide lifecycle and plate-based assays, turning scattered tracking into one structured view.


These labsheets captured peptide inventories, assay plates, and job tracking in a consistent format that scientists could query and automate.


## **Embedded automation close to data**


An embedded Jupyter-style environment let the data science team keep Python automation close to lab data instead of in isolated scripts.


They used it to sync reference data, ingest instrument outputs, compute derived metrics, and push results back into labsheets without manual file handling.


## **Redesigning workflow behavior**


In joint working sessions, the lab and Scispot diagnosed why completion behavior was inconsistent: protocol instances were embedded and reused in ways that conflicted with the platform’s intended flow.


They reshaped protocol usage so templates were embedded in sequence and experiments followed a clearer pattern that the system could represent reliably.


## **Practical workarounds while iterating**


While the deeper workflow redesign was underway, Scispot proposed immediate workarounds, such as re-ticking steps to refresh progress so experiments could move forward without blocking daily work.


In parallel, the teams tested a lean “completion” template with straightforward controls, including a single tick box, giving scientists a simple, trustworthy way to mark work as done.


## **Grounding implementation in real runs**


Implementation stayed close to real use: live proteomics workflows, structured templates, and concrete assay runs rather than abstract diagrams.


Scientists raised issues based on daily friction, and Scispot responded with targeted changes to protocol patterns and automation, making the system feel closer to how the lab already worked.


## **Signs of sustained adoption**


Account-level signals, like ongoing billing and connected operations around finance systems, indicated that Scispot became part of the lab’s regular environment, not a short-term pilot.


That ongoing engagement gave both sides room to keep iterating on workflows instead of freezing when complexity showed up.


## **Impact on workflow reliability**


Turning a vague “the workflow doesn’t work” complaint into a clear root cause and concrete next steps gave the lab a practical improvement plan.


Scientists gained a more predictable way to handle protocol completion and relied less on guesswork when checking experiment status.


## **Less ambiguity around “done”**


Better protocol structure plus simpler completion controls created a cleaner mental model for when an experiment was truly complete.


That reduced manual reconciliation and made it easier to review experiment history without second-guessing the system.


## **Ongoing operational relationship**


Despite the initial friction, the lab stayed on Scispot and continued to run connected workflows for both scientific and business operations through the platform.


This sustained use suggests that the approach of diagnosing, adapting, and supporting was strong enough to keep the relationship healthy as workflows evolved.


## **Base for the next stage**


With a single operational layer tying experiments, protocols, and automation together, the lab now has a clearer base for more complex or regulated workflows.


Future work, such as new assay classes, tighter traceability, or richer analytics, can build on a system that already reflects how scientists run experiments instead of forcing them into rigid templates.
