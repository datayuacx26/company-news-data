---
schema_version: "1.0.0"
document_id: "db91dcc06a30865b279601fdf020545490671a5b30403d2d1c6f5d7dd55efad9"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/benchling-structure-prediction"
published_at: "2026-06-25T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:4fcc1c2febea915ccd9391d13f496b58a350717af9de66010a9a99a514cf4b3f"
---

# Structure prediction has been around for years. Why isn’t everyone using it?

With the development of AlphaFold in 2018 and AlphaFold2 in 2020, fast structure prediction changed computational biology. Scientists can now generate high-quality protein structures in minutes using models like Boltz, Chai, OpenFold, and many others.


So why isn't it part of everyday science? Even with modern tools, creating a useful prediction is complex and cumbersome. You need to select the right model, transform your sequences into specialized inputs, run the model at scale, and interpret complex results.


For structure prediction to become routine, it needs to be easy to run, fast enough for campaign scale, and simple to interpret. Today, we're making that possible. Benchling AI agents now call directly into our Model Hub, allowing any scientist to run and analyze complex predictions with just a prompt.


## Agents that simplify complex runs and analysis


Creating a high-quality structure prediction is rarely a simple task. You choose a model (maybe multiple), hook up an MSA (and its 1.3 TB in data), configure the chains or multimer complex, launch the job, then make sense of confidence scores and compare results across models. Every one of those steps is a place where someone new to modeling can get stuck.


Now, agents take on that orchestration. A scientist describes what they want in plain language — fold these variable regions, model this complex, compare two models on the same input — and the agent configures the run, executes it, and helps interpret the results in the context of the surrounding experimental record. The work of operating the model moves to the agent, so the scientist can focus on the question they set out to answer.


After running large batches of predictions, our agents simplify evaluating and comparing the results. They reason through the output files, writing code and producing charts. The scientist gets back detailed analysis (confidence scores, iPTM, and more) that can be pushed directly into the notebook or exported into presentations.


## Structure prediction that scales


Scientific model workloads are bursty by nature: quiet for days, then tens of thousands of predictions required in a single afternoon. We see customers struggle to run prediction at the scale their campaigns demand. They are waiting in multi-week HPC queues, rationing against a fixed GPU reservation, or maintaining their own orchestration to keep a pipeline running.


[Benchling Inference](https://www.benchling.com/blog/announcing-benchling-inference-powered-by-baseten) , the compute layer underneath our Model Hub, is built to remove those constraints:


-


**Scalable batch predictions.** Select hundreds or thousands of sequences and submit them in a single run. Benchling manages the compute capacity and infrastructure for you, so you can focus on the science.


-


**More throughput for the same cost.** Newer GPUs deliver up to 4x the structure predictions with the same compute budget. Our work with NVIDIA brings you further speed optimization for models like OpenFold.


-


**Secure access to complex infrastructure.** Running high-quality structure prediction requires an MSA server, which is expensive and difficult to operate. We provide a secure, GPU-accelerated MSA service, so your sequences stay within Benchling’s secure cloud and never hit a public endpoint.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## The right models for the job — now including ESMFold2


Different questions call for different models, which is why we’ve been building a broad roster. For structure prediction, our[Model Hub](https://www.benchling.com/ai/models) already supports many of the latest structure prediction models.


This week, we’ve also added[ESMFold2](https://www.benchling.com/blog/behind-the-model-with-sal-candido-of-biohub) **** to that lineup. ESMFold2 is the successor to ESMFold that sets a new state of the art for single-sequence structure prediction and enables the generation of new functional proteins through searching the ESMC model’s latent space. The model predicts high-resolution, all-atom 3D structures of biomolecular complexes directly from sequence, with optional multiple sequence alignment (MSA) input for enhanced accuracy on challenging targets.


Agents can now help find the right model for your task, including for complex multimers like protein-ligand folding.


## From capability to utility


For many teams, structure prediction has either lived in a side Jupyter notebook maintained by a single computational scientist, or in an ad hoc collection of public tools that rarely become part of everyday work. With Benchling, any scientist can now run structure prediction simply by asking an agent, directly where they already work.


**Already a Benchling customer?** Structure prediction is in your account now. Learn more in our[Help Center](https://help.benchling.com/hc/en-us/articles/45774533344781-Run-Scientific-AI-models-in-Benchling-with-Model-Hub) .


**New to Benchling?**[Request a demo](https://www.benchling.com/request-demo) .
