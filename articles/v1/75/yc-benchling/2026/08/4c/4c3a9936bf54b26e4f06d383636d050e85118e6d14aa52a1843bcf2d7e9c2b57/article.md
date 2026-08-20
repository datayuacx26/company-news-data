---
schema_version: "1.0.0"
document_id: "4c3a9936bf54b26e4f06d383636d050e85118e6d14aa52a1843bcf2d7e9c2b57"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/can-llms-work-in-the-wet-lab"
published_at: "2026-08-13T07:00:00+00:00"
first_seen_at: "2026-08-14T07:08:34.320153+00:00"
fetched_at: "2026-08-14T07:08:38.831109+00:00"
content_hash: "sha256:e258c4f69e0793bd6e2ee82f28b9d2f54e02767c11950cbca95f79810e8d860f"
---

# Can LLMs work in the wet lab?

Today, we’re publishing the results from **BenchBench-Protocol** , a benchmark built from thousands of real-world experiments that tests whether models can troubleshoot and optimize wet lab protocols like expert scientists. Read the paper[here](https://assets.ctfassets.net/kzeezny59h5p/1spGaR3QP9B9CBfuXHMrLl/0ba5a1de599daf1e57faabc62564eb88/BenchBench-Protocol.pdf) .


LLMs know an extraordinary amount of biology. But knowing biology and *doing* biology are different things. A textbook can tell you how CRISPR works. It can’t teach you to troubleshoot a CRISPR protocol when cells die after transfection.


We’ve spent the last year bringing AI to scientists across academia and industry. While there’s universal excitement for AI, the gap is clear. AI needs to get better at reasoning like an expert scientist in the lab. Today, LLMs are often evaluated on general reasoning and scientific knowledge, not the problems scientists actually face in the lab. That[seam](https://www.benchling.com/blog/ai-scientist-that-deserves-the-name) between the digital and the physical worlds is where we are focused.


We recently built a team focused on evaluating and improving LLMs in wet lab biology. We are publishing the first in a series of benchmarks so model developers have a goal to improve on and scientists can choose models that best fit their work.


## **BenchBench-Protocol**


Debugging and optimizing protocols is a challenge scientists face every day. Scientists start from a published protocol for a new technique they’re trying to use. They get it to work in their lab through repeated trial and error: adjusting it for different instruments, swapping reagents for ones they already have, tuning incubation times and concentrations.


We wanted to measure how well LLMs reason through these problems. We collaborated with scientific experts to study thousands of real-world experiments. We identified the optimizations scientists made to protocols to get them to work, had them reviewed by multiple experts, and then turned them into tasks for an LLM.


Opus 5 performs the best on BenchBench-Protocol at 59.2%, followed by GPT 5.6 at 47.1%. Kimi K3 performs surprisingly well among open source models at 45.7%.


The failure modes point to gaps in the practical reasoning that makes an expert successful in the lab. Models give answers that sound sensible on paper but miss how experiments actually behave: assuming a measurement translates directly to a result without considering calibration, treating a sample as pure despite visible evidence it isn’t, or missing subtle changes in physical technique that can ruin an experiment.


These gaps are unsurprising given what LLMs have been trained on. To get better, models need to learn not just from the clean results in published papers, but from real experiments and their messy outcomes.


## **Supporting the ecosystem**


Reinforcement learning has made models dramatically better in other domains like coding: give them thousands of tasks, tell them whether their answers work, and they learn better ways to reason. We believe the same thing can happen in biology.


Careful engineering can turn messy scientific data into tasks for models to attempt, learn from, and be evaluated against. We’ve learned a lot about how to do this well: finding tasks that capture real scientific judgment, separating idiosyncratic decisions from sound scientific reasoning, and developing rubrics that can reliably tell the difference. We’re now scaling this work up to increasingly difficult problems, from sequence design and recommending the next experiment all the way up to critical drug program decisions.


Rigorous evaluations have become essential to how model labs improve frontier models. We expect they’ll become increasingly important to biopharma too, as companies look to understand how well AI can reason about their own science.


If you’re interested in evaluating and improving AI for your own company, we’re happy to help. Get in touchhere .
