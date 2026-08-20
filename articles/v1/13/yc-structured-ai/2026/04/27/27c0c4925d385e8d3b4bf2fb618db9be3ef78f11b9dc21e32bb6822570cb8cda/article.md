---
schema_version: "1.0.0"
document_id: "27c0c4925d385e8d3b4bf2fb618db9be3ef78f11b9dc21e32bb6822570cb8cda"
company_key: "yc-structured-ai"
company: "Structured AI"
source_id: "yc-structured-ai-news-import-70ac70f80ba3"
canonical_url: "https://getstructured.ai/blog/plausible-is-not-compliant/"
published_at: "2026-04-07T00:00:00+00:00"
first_seen_at: "2026-07-24T02:36:35.541532+00:00"
fetched_at: "2026-07-28T21:25:41.488077+00:00"
content_hash: "sha256:4f8287c7cf0efd80ecb1b4daaa748d8d5ed8b7ba2acce11886ab66235f9b1238"
---

# AI Agents That Catch Compliance Failures Before Sign-Off

Back when I worked in practice, I really struggled with the ability to build my own technical tools. That gap has since been alleviated for most industries, there are now AI tools that let you build your own apps, wire up your own agents, and automate workflows that used to take hours. But in the built environment, the barriers remain real: significant liability, data security constraints, governance overhead, and tools that simply were not designed for our discipline or trained on real construction documents.


**A new wave of AEC startups has changed some of this.** But a closer look at the landscape reveals a consistent bias: the majority of investment is going towards generative design work, computational massing, visualisation, early-stage design exploration. Compliance, coordination, and QA/QC, the administrative layer that gets less attention, in the age of the Building Safety Act, carries increasing legal weight and professional liability. **The bottleneck in practice isn't producing the design. It's verifying it.** As AEC Magazine noted earlier this year, *"the binding constraint has rarely been geometry. It is decision velocity under pressure."*


## Case Study: Pilbrow & Partners


Why accuracy is a professional obligation, not a product feature


Writing in[RIBA Journal](https://www.ribaj.com/intelligence/implementing-the-building-safety-act-principal-designer-quality-assurance-site-supervision/) in July 2024, Giuseppe Messina of Pilbrow & Partners described how his practice responded to the Building Safety Act: a bespoke compliance matrix, built per RIBA Plan of Work stage, designed to demonstrate how new designs comply with statutory building regulations and feed into the digital record for the required golden thread of information.


The matrix is conceived as structured logic, a systematic set of checks against defined criteria, producing a traceable record.


**The matrix is already the check. The question is who runs it?**


The execution today is manual: a principal designer and project architect working through each sub-category of each Approved Document by eye. At the volume and cross-referential complexity of a full construction document set, that process will miss details, not through negligence, but because no reviewer can hold 500 pages of interdependent information simultaneously in view. Under the BSA, a missed detail after Gateway 2 submission triggers resubmission to the Building Safety Regulator, currently taking up to 20 weeks for approval.


When compliance logic is encoded as an agent and run automatically before submission, practices catch what manual review misses. The knowledge is still the architect's. The matrix is still the firm's. What changes is execution: consistent, traceable, running on every project regardless of who is available that week. Building the agent that can carry that responsibility reliably is where the hard work begins.


## Building accuracy for the sign-off


Compliance in AEC does not resolve to a single set of rules. Applicable regulations shift per building type, per location, per borough, per construction classification. A model without the context of those variables for a specific project has no basis for a reliable check. And a model that produces different outputs each time it runs, the non-determinism inherent in standard LLM inference, cannot form part of an auditable process. As Clifton Harness wrote in[AEC Magazine this March](https://aecmag.com/ai/ai-in-aec-collapse-cognition-not-geometry) : *"plausible is not the same as compliant."*


Then there is the accountability question. As AEC Magazine's piece on[the missing AI middle](https://aecmag.com/ai/architecture-the-missing-ai-middle/) put it: *"Who carries the liability when an automated decision results in a failure down the line?"* For a principal designer's sign-off, every output needs to be traceable to a defined spec. A framework that produces unauditable outputs cannot carry that responsibility.


Building a compliant agent means solving for these requirements structurally and designing the execution pipelines around it from the start. Deterministic page filtering. Drawing-type classification before inference runs. Spatial resolution that pins a finding to a precise location on a sheet. Cross-discipline reasoning that holds the entire document set in view simultaneously. These are infrastructure problems, not model problems. The foundation model is the last layer, not the first.


"Software is going from helping an individual be slightly more productive to actually accomplishing a job autonomously."


Bret Taylor, Sierra


Encoding compliance logic in a way that is repeatable, auditable, and consistent across every project type is not a development task. It is a knowledge translation problem, one that requires both the domain expertise to know what to check and the infrastructure to execute those checks reliably against real construction documents. Building that infrastructure is exactly what we set out to do.


## Classification, resolution, grounding


1. **Drawing type classification across more than 15 construction drawing types before a single check runs** because disciplines require fundamentally different extraction logic. A waterproofing detail is interrogated for fall arrows, membrane continuity, and floor waste positions. A panel schedule is parsed for breaker ratings, circuit labels, and component taxonomy. The same page-level scan applied to both returns nothing useful from either.
2. **Cross-reference resolution: every callout on every sheet is a pointer to another location** in the set. When it points to a cross-reference that resolves to the wrong sheet or a note that contradicts the spec, that is a coordination failure. It only surfaces if the platform has built an index of the entire document set as a connected graph.
3. **Spatial grounding: when a check identifies an issue on a large engineering sheet** , the finding must resolve to a precise location: the specific annotation, keynote, or symbol that triggered it. On a 34" × 44" drawing, that precision is what makes a finding actionable rather than something a reviewer has to hunt for themselves.


Replicating this in-house is a months-long engineering effort, before a single check is written. But time is not the moat. What cannot be replicated is the accumulated domain knowledge from processing real document sets across project types, jurisdictions, and disciplines. The logic calibrates with every drawing processed and a firm building from scratch starts with the scaffolding and none of the calibration. What our system makes possible is checks that firms build, own, and run themselves.


## Own your compliance logic


Most solutions in the market say they don't want to replace the architect. But the product decisions eventually reveal different priorities. At Structured AI we provide agentic tools that can be customised per firm, for code compliance, QA/QC, and the checks that matter to your specific practice and project types. We are tackling a mountain of compliance that determines building blocks, the safety and efficiency of our built environment.


The most important principle is: **startups shouldn't just provide tools for practitioners to use. We should actively give them the capabilities to expand and scale themselves.**


When one of our engineers built a custom compliance check for a client, encoding a specific regulatory requirement that the firm's team needed to run repeatedly across their document sets, the response wasn't that this was a useful product feature. It was recognition that this was something they had been unable to do on their own. The check runs deterministically. Every issue it surfaces links back to a specific location on a specific drawing. It can be tested, refined, and owned by the firm's own team.


Our engineering team will be posting technical demos of these features as they develop.


**Take a regulation document, convert it into a check, and run it consistently across every project. Query and explore through document chat, test in Playground, deploy into regular QA/QC runs. Practitioners take charge of their own AI deployments, within an environment built to understand what a construction document actually is.**


Want to see how Structured AI can work for your team?


[Book a Demo](https://getstructured.ai/contact/)
