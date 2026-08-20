---
schema_version: "1.0.0"
document_id: "4b7ecded7758153be0e2cf44541c3e1bc05bcc51f478bd79f44ace643a81c187"
company_key: "yc-synth-3"
company: "Synth"
source_id: "yc-synth-3-news-import-d5c503e27bee"
canonical_url: "https://www.usesynth.ai/blog/research-factory-craftax"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-24T04:58:53.081142+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:8cd932a305684ab51b1435694493dd730fd4ab03039f0cbe7aa6f64eef35ea63"
---

# Research Factory: Repeated Research With Held-Out Proof

## What Shipped


Research Factory turns one-off runs into a research programme with explicit operating limits and reviewable evidence.


Each Factory contains Efforts: durable objectives that can launch research runs, launch typed maintenance runs, and schedule the next cycle. A research run investigates the objective and produces evidence or a candidate. A maintenance run reads the current state and records what should happen next. Both remain normal Managed Research runs with the same receipts, budgets, and terminal states.


The first release includes:


- Factory budgets and active-run caps;
- durable Efforts with research and maintenance run kinds;
- typed Factory status, experiment history, candidate evidence, and WorkProducts;
- scheduled wakes with a dry-run preview;
- immutable candidate identity for benchmark-owned held-out grading; and
- Python SDK and MCP paths through` synth-ai\[research\]`` 0.16.0` .


The control loop is deliberately evidence-first. A Factory can produce a candidate, but it cannot declare that candidate a champion by reading its own training traces. Promotion requires an evaluator outside the worker's workspace to check out the immutable candidate and grade it on held-out inputs.


## The Craftax Acceptance Proof


We used Craftax to prove that lifecycle end to end. The Factory had to operate inside a bounded project and Effort, produce inspectable work, survive benchmark-owned grading, and clean up its rehearsal resources.


Acceptance signal Result


FactoryBench verdict **passed**


FactoryBench lifecycle gates **17 / 17 passed**


Terminal Luna runs **4 / 4 succeeded**


FactoryBench lifecycle score **1.0**


Frozen offline calibration **0.0 → 0.0294913** (` n=56` )


Calibration interval **\[0.0284091, 0.030303\]**


Required threshold all 17 gates pass and calibration` accepted=true`


Wall time **482.6 seconds**


Typed usage **0.459327internal/0.459327 internal / 0.459327 in t er na l / 0.41 charged**


The four runs produced four distinct readable reports and used` gpt-5.6-luna` throughout. Their typed usage receipts contain exactly the four budgeted run IDs: 1,191,567 input tokens (943,360 cached) and 15,184 output tokens. The 5ordinary−runand5 ordinary-run and


5


or


d


ina


ry


−


r


u


nan


d


15 Factory caps were respected, both Efforts, the Project, and the Factory were archived, and the local slot ended healthy and unclaimed.


These numbers describe different layers of the receipt. The` 1.0` score is the FactoryBench lifecycle verdict. The` 0.0294913` value is a frozen offline Craftax calibration used to prove the grading join. It is explicitly not a live JAX result, autonomous uplift claim, or customer-efficacy claim.


## Start With One Bounded Effort


Install the Research extra:


bash


```text
pip   install   "synth-ai[research]==0.16.0"
```


Then create one Factory and one Effort, link a runnable project, launch a research cycle, and inspect the typed status before scheduling another wake. The[Research Factory quickstart](https://docs.usesynth.ai/managed-research/factory-quickstart) shows the Python and MCP paths, including budget policy, maintenance runs, evidence inspection, dry-run scheduling, and rehearsal cleanup.


The full lifecycle lives under` SynthClient().research.factories` , including Factory and Effort handles, status, usage, events, previewed wakes, and typed maintenance operations.


## Release Boundary


This release proves a gated research lifecycle and immutable held-out grading. It does not claim a 24/7 reliability window, autonomous uplift from the local harness result, or Factory-to-pull-request code delivery.


Those are separate gates. Shipping the narrow boundary now keeps the receipt honest: objective and budget in, inspectable evidence and independently graded candidates out.


## Try It


Follow the[Research Factory quickstart](https://docs.usesynth.ai/managed-research/factory-quickstart) , or[start Managed Research](https://www.usesynth.ai/signup?product=managed-research&utm_source=blog&utm_medium=cta&utm_campaign=research-factory-craftax&growth_action_id=2026-07-13-research-factory-craftax-blog) with one bounded objective you can evaluate on held-out inputs.
