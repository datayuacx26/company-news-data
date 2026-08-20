---
schema_version: "1.0.0"
document_id: "8459c6604bc7c42b32682741e92ed24d06320e016430ebedd2228798a81614c1"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/benchling-and-highres-lab-automation"
published_at: "2026-04-07T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:799e5f0049020ac776312063dc2200dc596a6cb9c5263587b632fe05dbb96ab9"
---

# Benchling and HighRes: Lab automation just got easier

*Learn more about*[Benchling Automation](https://www.benchling.com/automation) *, which connects your lab instruments to your scientific record, runs analysis automatically, and feeds every result directly into the next decision.*


Today, we're announcing an integration between Benchling and[HighRes](https://www.highres.com/lab-orchestration) ® that connects the platform scientists use to design and track experiments with the robotic systems that run them. Scientists can now set up an experiment, trigger an instrument run, and get analyzed results back in their notebook automatically, with no file handling or manual data wrangling required.


This integration addresses two problems we hear consistently from scientists running automated workflows.


1.


The first is accessibility. Lab automation has historically required significant custom integration work. Stitching together instruments, data pipelines, and analysis tools by hand is a barrier most biology teams don't have the time and resources to take on.


2.


The second is speed. Computational tools have gotten faster at generating hypotheses and designing experiments, but those gains only hold if the experimental cycle can keep up. The bottleneck is the manual work between systems. A plate-based assay might take a few hours to run, but before it starts, someone has to export the plate layout, reformat it for the instrument, and confirm the right file landed in the right place. After it finishes, someone has to find the output, download it, clean it up, and get it back into the notebook. By the time results are usable, half a day is gone. For teams running repeated cycles, those steps add up and slow things down.


### Inside the integration


The Benchling and HighRes integration is built for the kinds of structured, parameterized, and instrument-dependent experiments that run repeatedly in drug discovery: cell viability assays, compound dose-response studies, ELISAs.


Here's what the new integration looks like when working on an ELISA:


**1. The experiment is set up in Benchling.** A scientist designs an experiment and initiates a workflow in Benchling. A checklist enforces required human verification for safety-critical steps.


**2. Automated experiment handoff goes to HighRes.** Benchling passes structured experiment parameters to Cellario™ OS via API: sample information, plate barcodes, plate layout, and everything else the workcell needs to get started.


**3. Robotic execution occurs via Cellario OS.** Cellario OS schedules and executes the assay. The workcell scans plates, transfers them to the liquid handler, and runs the full assay workflow, all without manual intervention.


**4. Data is retrieved and analyzed automatically.** Results return to Benchling automatically, triggering pre-configured analysis (curve fitting, heatmaps, normalization, custom code) all inside the platform with full traceability.


**5. Results are recorded in the notebook entry.** Charts, fitted models, processed data, all analytical outputs land in the scientist's entry where they initiated the experiment, and are immutable, contextualized, ready for the next decision.


This end-to-end automated workflow demonstrates four capabilities now available in Benchling:


1.


[Automation Designer](https://www.benchling.com/blog/end-to-end-automated-workflows) , enabling the data orchestration that connects experiment setup, instrument execution, and data analysis into configurable, end-to-end automated workflows


2.


**HighRes orchestration integration** , allowing Benchling to coordinate directly with Cellario OS and HighRes workcells


3.


[Custom Code](https://www.benchling.com/blog/end-to-end-automated-workflows) **embedded within Automation Designer** , so advanced analysis runs inside Benchling with full traceability


4.


**Zero-click analysis automation** , where every step from protocol execution to analyzed results completes without needing manual intervention


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


### Open interfaces enable interoperability


Interoperability across instruments is a prerequisite for end-to-end automation. Both HighRes and Benchling are designed to work with a wide range of instruments. Benchling instrument connectors (180+ models)[normalize vendor formats](https://www.benchling.com/blog/open-source-data-standards-allotrope) into the Allotrope Simple Model (ASM), a JSON-based standard. HighRes provides an open API and 500+ instrument drivers for orchestration across vendors.


When instruments, automation systems, and data pipelines are connected, they can exchange data through standard interfaces without custom integrations for each step.


### Run iterative experiments faster, with AI


AI tools can suggest the next experiment in seconds. But if it takes days to set up the run, export the data, clean it, and get results back into the notebook, the speed advantage disappears.


For teams running iterative experiments, this integration addresses that directly. The next run can start as soon as the current results are reviewed. Steps that previously stretched across days of manual handoffs now happen automatically, in the time it takes to make a scientific call.


### Join us to see how it works


We're hosting a webinar on May 14th to walk through the Benchling and HighRes integration, including a live demo of an end-to-end workflow.[Register here](https://events.teams.microsoft.com/event/51915d13-bd96-438a-93b7-136344a9bbf3@e5670c64-747d-4682-9c55-bae5601793fa) to join.
