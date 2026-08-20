---
schema_version: "1.0.0"
document_id: "afde2eab37e41655143ff0ce7d8785311535afa7655327742f91d0f1fd9deae3"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/model-synthesis"
published_at: "2026-07-23T20:03:12.073+00:00"
first_seen_at: "2026-07-23T21:01:38.295176+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:bb1340e0d6bd7c9b1f75b297422358136da4baf33d857e81baa272a68758212d"
---

# Outperforming Fable 5 at half the price: meet model synthesis, a new server-side tool on DigitalOcean Inference Engine

Anyone building with AI runs into the same tradeoff: how to get the most intelligence per dollar, the right model at the right cost for each task. DigitalOcean[Inference Engine](https://www.digitalocean.com/products/inference-engine) is built to help you make that tradeoff, and one way is finding the right model for each job. But sometimes one model isn’t enough. On deep-research tasks, we found that running several models and synthesizing their outputs beats relying on one: an all-open-source panel (GLM 5.2 + Kimi K2.6) scored higher than every single model we tested, including Fable 5, at about half its cost per task.


[Model synthesis](https://docs.digitalocean.com/products/inference/how-to/use-server-side-tools/#use-fusion-tool) , a new server-side tool on DigitalOcean[Inference Engine](https://www.digitalocean.com/products/inference-engine) , does that orchestration for you. It runs from a model configuration you define: a panel of models that process each request in parallel, and a synthesizer model that reviews the panel’s outputs and combines them into one response. Start from an optimized preset or define the panel and synthesizer yourself.


It pays off. We benchmarked model synthesis on DRACO, a 100-task deep-research benchmark, across 15 open-source and frontier model configurations. The key results:


-


GLM 5.2 + Kimi K2.6 panel scored 65.65% on quality at $0.83 per task, outperforming Fable 5 (62.21% at $1.59 per task).


-


Four open-source combinations land in the ideal quadrant, offering higher quality at lower cost.


-


Fable 5 + GPT-5.6 frontier panel scored 69.01% on quality at $4.76 per task, the highest quality and highest cost of any model configuration tested.


We measured each configuration on two axes: quality (higher is better) and cost per task (lower is better). The chart below plots each configuration’s quality against its cost per task: the ideal quadrant would be the top left, where quality is highest and cost per task is lowest. The best open-source configurations land in the ideal quadrant, with higher quality at a lower cost per task than frontier single models. (The open-source single models sit lower and further left: cheaper, but at lower quality.)


*Figure 1. Quality versus cost per task in USD (up and to the left is better; the horizontal axis is log scale, so each gridline doubles the cost). The shaded area marks configurations that beat Fable 5 on both counts, higher quality and lower cost. The open-source GLM 5.2 + Kimi K2.6 panel is the standout: it scores higher than every single model, including Fable 5, at about half of Fable 5’s cost. The frontier Fable 5 + GPT-5.6 pairing tops quality but at a premium cost, and the cheap open-source single models sit lower and to the left, cheapest but at lower quality. In each panel, the first model listed is the synthesizer.*


## How we tested it


We benchmarked model synthesis against single models on DRACO, a public deep-research benchmark. DRACO is built for exactly the kind of question a single quick answer cannot handle; it poses 100 demanding research tasks across ten real-world domains, from law and medicine to finance and product comparison. Each task is open-ended and evidence-heavy, questions where a strong answer has to gather facts from many angles, weigh them, and lay out a thorough, well-cited response rather than a one-line reply. Every task comes with a detailed rubric of the specific points a good answer should cover, and an independent judge scores each answer on how many of those points it includes and supports. Because the same judge grades every configuration against the same rubric, the scores are directly comparable. That breadth and evidence work is precisely what the synthesis tool is designed to improve, which is why we chose DRACO. For each configuration, we recorded a quality score and cost per task. We compared 15 configurations: 4 single models (the frontier GPT-5.6 and Fable 5, plus open-source DeepSeek V4 Pro and GLM 5.2) and 11 model configurations that vary the panel and the synthesizer, 1 frontier pairing and 10 open-source. The full set appears in Tables 1 and 2.


## Results


The best open-source model configuration scored higher than every single model tested on this benchmark, and most model configurations beat the strongest single model. Compared with frontier single models, the best open-source model configuration (GLM 5.2 + Kimi K2.6) delivers higher quality than both Fable 5 and GPT-5.6 at a lower cost per task. The trade-off is cost against the cheapest single open models: GLM 5.2 and DeepSeek V4 Pro run for less per task, but at much lower quality. Table 1 ranks the configurations we measured end-to-end by quality, with cost per task.


**Table 1.** Higher quality score is better; lower cost per task is better. *The GLM 5.2 + Kimi K2.6 panel and the DeepSeek V4 Pro + Kimi K2.6 panel synthesized by DeepSeek were scored on the same benchmark but not separately metered for cost in this run, so they appear in Table 2 by score. Their per-task cost sits in the same range as the other two-model panels. Note: Costs and cost per task are based on token pricing and usage rates at the time of testing (July 2026).*


Two patterns stand out. First, the **synthesizer choice drives quality** . On every panel, GLM 5.2 synthesized best, DeepSeek V4 Pro next, and Kimi K2.6 last. Second, the **best two-model panel is as strong as the three-model panel** . Table 2 shows every panel and synthesizer combination by quality.


**Table 2.** Every open-source model configuration by quality score. *Rows are the panel; columns are the synthesizer. Green cells beat the strongest single model (GPT-5.6 at 64.32). Single models for reference: GPT-5.6 64.32, Fable 5* 62.21, DeepSeek V4 Pro 58.15, GLM 5.2 58.12. *Frontier panel for reference: Fable 5* + GPT-5.6 69.01.**


*Figure 2. Every model configuration is ranked by quality score (DRACO, 100 tasks; higher is better). Open-source model configurations are shown in green, single models in gray, and the frontier configuration in black. Most open-source configurations clear the best single model.*


## The synthesizer you choose drives the quality


Across every panel, GLM 5.2 was the strongest synthesizer, DeepSeek V4 Pro was second, and Kimi K2.6 was last. Swapping only the synthesizer moved a model configuration by 3 to 5 points, more than the difference between a two-model and a three-model panel. In fact the best two-model panel (65.65) beat the three-model panel (65.21), so adding a third open model was roughly a wash on quality. The practical lesson is to pick a strong synthesizer first and keep the panel lean.


## Efficiency: high quality for about half the cost


For the quality it delivers, the best model configuration wins on cost. GLM 5.2 with Kimi K2.6 scores 65.65 at $0.83 per task, higher than every single model while costing about half of Fable 5 ($1.59) and roughly 40 percent less than GPT-5.6 ($1.39). You get better quality than a frontier model and pay less for it. The only cheaper option is a bare single open model (GLM 5.2 at $0.25 or DeepSeek V4 Pro at $0.31), several times cheaper but also the lowest quality. So if all you need is rock-bottom cost, a single open model is hard to beat; but for the best quality per dollar, an open-source panel with a GLM 5.2 synthesizer beats the single models outright at about half the cost of the strongest ones.


*Cost per task (USD) is based on the specific pricing available at the time of testing (July 2026) and is subject to change based on third-party API rate adjustments. (lower is better)*


## What this means for you


The through-line here is intelligence per dollar: matching the amount of intelligence you use to what each task actually needs. Model synthesis gives you a dial for that, not a tradeoff to solve from scratch. For high-volume, cost-sensitive work (bulk classification, routine summarization), a single open model like GLM 5.2 gets you the lowest cost per task. For research-heavy work where answer quality matters more than the bill (due diligence, competitive analysis, legal or medical research), pair GLM 5.2 and Kimi K2.6 in a call with GLM as the synthesizer. Same tool, same API, just a different setting, and you get there without needing a frontier model at all.


DigitalOcean Inference Engine was designed to get you the right intelligence at the right cost.[Inference Router](https://docs.digitalocean.com/products/inference/how-to/use-inference-router/) and model synthesis work together to achieve that.[Inference Router](https://docs.digitalocean.com/products/inference/how-to/use-inference-router/) picks the right model for a given request based on your preferences, cost, latency, and quality. Model synthesis is what you reach for when one model isn’t enough and you want several working on the same problem. Together they give you a range, from routing to a single model up to running a full panel, without changing how you call the API.


## Get started


Model synthesis is now available in Public Preview on[DigitalOcean Inference Engine,](https://www.digitalocean.com/products/inference-engine) alongside[Evaluations](https://docs.digitalocean.com/products/inference/how-to/evaluate-models/) . Choose an optimized preset or select the models and configuration directly, all through a single inference call.


**Presets**


Use our[optimized presets](https://docs.digitalocean.com/products/inference/how-to/use-server-side-tools/#get-started) out of the box depending on your use case:


-


**Budget:** The lowest cost configuration, using a smaller model panel and lighter reasoning settings.


-


**Balanced:** A mid-size panel with moderate reasoning depth, balancing quality and cost.


-


**Quality:** The largest model panel and deepest reasoning configuration, at higher cost and latency.


DigitalOcean’s Inference Engine resolves each preset to a validated synthesizer, model panel, and reasoning configuration. As models, pricing, and performance improve, the underlying configuration can be updated without requiring changes to your integration.


```text
curl -X POST https://inference.do-ai.run/v1/chat/completions \
-H "Authorization: Bearer $DO_INFERENCE_KEY" \
-H "Content-Type: application/json" \
-d '{
"model": "glm-5.2",
"messages": [{"role": "user", "content": "Your research question here"}],
"tools": [{
"type": "model_synthesis",
"preset": "balanced"
}]
}'


```


### Direct configuration


The top-level model is the synthesizer, and the analysis_models list inside model synthesis is the panel. Reasoning effort is set in two places: the top-level value controls the synthesizer, and the value inside the tool controls the panel.


```text
curl -X POST https://inference.do-ai.run/v1/chat/completions \
-H "Authorization: Bearer $DO_INFERENCE_KEY" \
-H "Content-Type: application/json" \
-d '{
"model": "glm-5.2",
"messages": [{"role": "user", "content": "Your research question here"}],
"reasoning_effort": "high",
"tools": [{
"type": "model_synthesis",
"model": "glm-5.2",
"analysis_models": ["glm-5.2", "kimi-k2.6"],
"reasoning_effort": "high"
}]
}'


```


*Disclaimer & Methodology: Quality is measured on a public deep-research benchmark (DRACO, 100 tasks across ten domains), scored by an independent LLM judge. Metrics shown (quality and costs) reflect controlled testing using the DRACO benchmark (100 tasks) in July 2026. These results are for informational purposes only and do not guarantee future performance. Cost per task (USD) is derived from measured token usage and per-model pricing (lower is better). *The Fable 5 results (both single and synthesis configurations) cover 93 tasks due to Fable 5 guardrails blocking 7 inputs.*
