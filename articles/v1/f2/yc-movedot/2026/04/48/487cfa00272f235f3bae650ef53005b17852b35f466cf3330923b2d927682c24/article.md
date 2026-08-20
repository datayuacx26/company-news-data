---
schema_version: "1.0.0"
document_id: "487cfa00272f235f3bae650ef53005b17852b35f466cf3330923b2d927682c24"
company_key: "yc-movedot"
company: "MOVEdot"
source_id: "yc-movedot-news-import-b09513d212fd"
canonical_url: "https://movedot.ai/blog/ai-workflows-the-new-programming-language-for-engineers"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-25T16:12:42.390551+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:966c7d154ca529a66c0b29f87f8f7d78e12f95604ab6d7758ff0c5669228212b"
---

# AI Workflows: the new programming language for engineers | MOVE. Blog

Every experienced engineer carries a methodology in their head. What channels to check first after a test. How to tell a sensor anomaly from a real vehicle event. When to trust simulation or when to rerun. Which KPIs matter for which development phase.


This knowledge is real, it is valuable, and it has never been captured properly.


Today it lives as a mix of Python and MATLAB scripts, Excel templates, written procedures that are usually outdated, CAE solver configs, and a whole lot of tribal knowledge passed from one engineer to the next. Fragmented across people, files, and tools.


Fragmented today


Fig. 01


Tool


Python / MATLAB


Post-processing


Tool


Excel templates


KPI macros


Tool


CAE configs


Solver settings


Human


Tribal knowledge


In engineers' heads


Human


Test procedures


Written, often outdated


Human


Driver feedback


Subjective notes


The gap


No reasoning layer connects any of this


1 hour answer becomes a full day of manual routing


And here is the critical part: none of it reasons. If something unexpected happens, a sensor drifts, a test condition changes, or a file format is different, instead of getting an answer in an hour, the engineer spends an entire day routing and fixing everything manually. The entire organization depends on the availability and experience of specific people to keep methodology alive.


## What an AI Workflow actually is


An AI Workflow, a similar concept to a "Skill" in software development, is a structured, executable piece of engineering methodology. It combines instructions, the engineering intent and what the workflow is trying to achieve, deterministic steps like scripts, calculations, signal processing, and KPI extraction, simulation calls, connectors to data systems and test equipment, and engineering context like vehicle specs, tire data, boundary conditions, reference lap targets, and test standards.


Workflow architecture


Fig. 02


Start with the engineering intent and the context it needs


Input


Instructions


Engineering intent and goals


Input


Engineering context


Specs, tire data, targets


Deterministic


Scripts


KPIs, signal processing


Deterministic


Simulation


CAE solvers, models


Deterministic


Connectors


Data, PLM, equipment


Reasoning layer


Read outputs, validate, detect anomalies, decide next step


Outcome


Continue


Outcome


Adapt


Outcome


Flag engineer


Adapt loops back to re-evaluate


All of that is wrapped and orchestrated by a reasoning layer: AI that reads the outputs, interprets results, detects anomalies, iterates, and decides what to do next.


The AI's reasoning is the glue. It does not replace the Python scripts. It does not replace the simulation runs. It orchestrates them. It reads what came out, decides whether the result makes sense, handles the unexpected, and either continues, adapts, or flags the engineer.


This is fundamentally different from a script. A script breaks when its assumptions do not hold. An AI Workflow handles the mismatch.


## The sensor example


Here is a scenario every test engineer knows. A post-processing script runs after a vehicle test. Halfway through, it is calculating lateral acceleration from an IMU. The values look plausible, but they are subtly wrong. The sensor had a calibration drift that nobody caught during the session.


A traditional script produces wrong KPIs, or it crashes. Either way, you find out too late.


An AI Workflow handles this differently. It detects the statistical anomaly in the signal. It cross-references with redundant channels: GPS-derived lateral g, steering angle combined with speed. It flags the sensor as suspect. And then it either re-routes the calculation using the redundant source, or pauses and asks the engineer with a specific, reasoned explanation of what it found and why.


Side-by-side outcomes


Fig. 03


Step 1


Post-processing starts


Anomaly


IMU calibration drift


Values look plausible, but subtly wrong


Traditional script


Run post-processing


Calculate lateral g from IMU


Values look plausible


But subtly wrong


Wrong KPIs produced


Or it crashes. Either way, too late.


AI Workflow


Run post-processing


Detect statistical anomaly


Signal does not match expected distribution


Cross-reference channels


GPS lateral g, steering angle + speed


Flag sensor as suspect


With a specific, reasoned explanation


Correct KPIs delivered


Re-routed via redundant source


Same data. Same test. Different outcome.


That is not AI replacing the engineer. That is AI making the methodology robust to real-world test conditions, which is exactly what experienced engineers do in their heads, but which has never been encodable in a script before.


## AI Workflows are alive


AI Workflows are not static. They evolve.


An edge case gets found during a durability test? The workflow gets updated to handle it. A new sensor type is introduced? The validation logic adapts. A senior engineer retires? Their judgment is already encoded and versioned in the workflows they built.


We think about this similarly to how software teams track code changes with git. Every modification to a workflow is versioned, documented, and traceable. You can see who changed what, when, and why. You can roll back. You can branch and experiment.


And because AI Workflows encode critical engineering knowledge, the kind of knowledge that defines a team's competitive edge, they need governance. Not everything should be visible to everyone. Some workflows are run-only: an engineer can use them but cannot see or modify the internal logic. Others are fully open for the team to iterate on. This layered access is about protecting the methodology that makes your team competitive.


## AI Workflows as building blocks


This is where the composability becomes powerful. AI Workflows should be small, focused units that can connect into more complex workflows. A sensor validation block. A KPI extraction block. An anomaly detection block. A report generation block.


Reusable workflow tree


Fig. 04


+


Click nodes to open branches


post_test_analysis


v3.2


Runs full post-test pipeline: validates sensors, compares KPIs across domains, generates summary report


Click to expand child workflows


Each one is tested, versioned, and maintained independently. Then they compose into larger analysis pipelines with clear dependency management.


The durability testing team might use the same sensor validation block as the NVH team, but plug it into a completely different pipeline. A performance engineering team reuses the KPI extraction block but connects it to their own report format. New blocks can be built without touching the existing ones, and when a block improves, every workflow that uses it benefits.


This is how methodology scales across an organization. It is also how it compounds: every edge case, every fix, every improvement stays in the system and gets reused.


## What concretely changes for the engineer


Before: an engineer runs a test. Data comes in. The engineer opens files, applies scripts, interprets results, and writes conclusions. The loop is slow because humans are in every single step.


After: an engineer defines or invokes an AI Workflow. The workflow processes data, handles anomalies, extracts KPIs, compares against targets, and generates a structured investigation summary. The engineer reviews, challenges, and decides. The loop is fast because reasoning is automated, not just calculation.


The engineer's role shifts from executing methodology to designing and reviewing methodology. That is a fundamentally different job, and a more valuable one.


Junior engineers can run senior-grade methodology from day one. Senior engineers can encode their judgment and have it scale across the entire organization. Teams build institutional knowledge that actually persists and compounds over time, instead of walking out the door when someone leaves.


And the bold claim: every previous way we have had to capture engineering methodology, scripts, procedures, templates, required the author to anticipate every case. AI Workflows are the first format where unanticipated situations can be handled, because reasoning is part of the workflow itself. The workflow does not just execute. It understands what it is trying to achieve, and it adjusts. It combines code with the messiness of the real world.


So this might be the new and last "programming language" engineers will need.


## This is what we are building


MOVEdot's platform is where engineering teams create, run, and manage AI Workflows.


If you are an engineer dealing with fragmented data, manual analysis loops, and methodology that lives only in people's heads, get in touch:founders@movedot.ai
