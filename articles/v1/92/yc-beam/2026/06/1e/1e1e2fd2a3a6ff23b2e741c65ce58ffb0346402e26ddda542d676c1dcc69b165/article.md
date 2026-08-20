---
schema_version: "1.0.0"
document_id: "1e1e2fd2a3a6ff23b2e741c65ce58ffb0346402e26ddda542d676c1dcc69b165"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/glm-5-1-guide"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-22T21:28:59.137691+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:a9d4ef1f427e3fce2da13bd944cb6c3c920d2f907727b3d1200702e13ef3b3eb"
---

# How to Set Up GLM 5.1 for Coding Agents

Z.ai shipped GLM-5.1 as open weights on April 7, 2026, and the pitch is simple: a model that holds its own against Claude and GPT on real coding work, costs a fraction of what they charge, and runs on your own hardware if you want it to.


## What is GLM 5.1


GLM-5.1 is a large reasoning model built for software engineering and long agent runs. "GLM" stands for General Language Model, and this is the 5.1 release in a line that's been moving fast all year. It thinks before it answers ([chain-of-thought](https://www.beam.cloud/blog/prompting-techniques) is on by default, and you can switch it off), it calls tools, it returns structured JSON, and it speaks the Model Context Protocol. It's text-only. If you need to feed it images, Z.ai keeps that in a separate vision line that isn't open-weight.


The company behind it is Z.ai, which is the international name for Zhipu AI, a Beijing lab that spun out of Tsinghua University in 2019. They went public on the Hong Kong exchange in January 2026, the first large-language-model company to list there. That matters for one reason most coverage skips: when you call the hosted Z.ai API, your data sits under Chinese law. Download the weights and run them yourself and that concern goes away.


Under the hood it's a Mixture-of-Experts model. Total parameters land somewhere around 750 billion, with sources splitting between 744B and 754B depending on how they count, and roughly 40 billion of those activate on any given token. The context window is 200K tokens, output can run to 128K, and the full BF16 weights take about 1.65TB on disk. There's no smaller "Air" version of 5.1; the lightweight models in the family are older releases like GLM-4.5-Air.


What it's good at is the long haul. Z.ai's launch demos had it building a Linux desktop environment from scratch over eight hours and 655 iterations, and pushing a[vector database](https://www.beam.cloud/blog/best-embedding-models) 's query throughput to nearly seven times the production baseline. The headline trick is sustained autonomous work: experiment, check the result, adjust, repeat, across hundreds of rounds and thousands of tool calls without losing the thread.


## Using GLM 5.1 for Coding Tasks


Here's where I'll ask you to read the numbers with one eyebrow raised, because most of GLM's benchmark figures are self-reported. The GLM-5 base model's 77.8% on SWE-bench Verified was checked by outside parties, which lends the line some credibility, but treat the 5.1 coding claims as a starting point and test on your own work.


With that caveat, the picture is strong. On SWE-Bench Pro, which measures real industrial code repair, GLM-5.1 scores 58.4 and edges out both GPT-5.4 (57.7) and Claude Opus 4.6 (57.3). It leads on CyberGym vulnerability discovery (68.7) and on agentic browsing tasks. Artificial Analysis, which runs its own independent evaluations, called it the top open-weights model on its intelligence index at release, sitting well above the median for comparable open models but still behind the closed frontier from Google, OpenAI, and Anthropic.


The gaps show up in pure reasoning. On the hardest math and knowledge tests, AIME, GPQA-Diamond, Humanity's Last Exam, the Western frontier models pull ahead. GPT-5.4 hits 98.7 on AIME 2026 against GLM-5.1's 95.3; Gemini 3.1 Pro leads GPQA at 94.3 to GLM's 86.2. Claude Opus 4.6 stays ahead on GPU-kernel optimization and edges it on the toughest tool-use evals. So the honest summary is this: for long, multi-file coding at volume, GLM-5.1 trades blows with anything on the market. For frontier math, knowledge reasoning, image input, or raw speed, the closed models still lead. GLM-5.1 is also on the slower side, running somewhere between 44 and 101 tokens per second depending on which host you pick, which makes it a poor fit for autocomplete-style work where you feel every pause.


## GLM 5.1 Pricing Options


You have four routes, and which one wins depends entirely on whether you're coding by hand or building a product.


If you just want to try it, go to chat.z.ai and use it free with a limited quota. Community front-ends expose it free too, and the BigModel platform hands new accounts a pile of trial tokens. No commitment, no card.


If you're coding day to day, the value pick is the GLM Coding Plan, a flat subscription rather than per-token billing. The Lite tier runs about $18 a month, or closer to $12.60 if you pay annually, and gives you roughly 80 prompts every five hours. Pro is $72 a month for about five times that usage, and Max is $160 for twenty times. All three plug straight into Claude Code, Cursor, Cline, and the rest. For comparison, that Lite tier undercuts Claude Pro ($20) and sits right alongside Cursor Pro ($20), except you're getting a model you can also self-host.


If you're building something programmatic, use the pay-per-token API. Z.ai's first-party pricing is $1.40 per million input tokens and $4.40 per million output, with cached input dropping to $0.26. Third-party hosts on OpenRouter often come in cheaper, around $0.95 to $1.05 in and $3.10 to $3.50 out, with DeepInfra and Wafer pushing the blended rate under $0.75 per million. Fireworks charges the same as Z.ai but runs faster; Together is the speed leader.


Put that next to the competition and the case makes itself. GLM-5.1's combined input-plus-output runs about $5.80 per million on the first-party API and less on third-party hosts. GPT-5.5 is $5 in and $30 out. Claude Opus 4.8 is $5 in and $25 out. You're paying roughly one-fifth to one-sixth the output price for[coding performance](https://www.beam.cloud/blog/best-coding-llm) that lands in the same range.


The fourth route is self-hosting, which only pays off at sustained high volume or when data can't leave your walls. The full FP8 weights need about 744GB of VRAM, so you're looking at eight H200s or a B200 node running[vLLM](https://www.beam.cloud/blog/vllm) or[SGLang](https://www.beam.cloud/blog/sglang) . A multi-GPU node bills the same whether it's busy or idle, so this is a volume play, not a hobby one.


## How to Run GLM 5.1 Locally


You can run GLM-5.1 on your own machine, but set expectations first. Because it's Mixture-of-Experts, every one of those ~750 billion weights has to live in memory even though only 40 billion fire per token. There's no getting around the footprint.


For production-grade quality you want the FP8 build, which needs around 744GB of VRAM. In practice that means eight H200s (141GB each) or a single HGX B200 node. Serve it with vLLM 0.19 or later, or SGLang 0.5.10 or later, both of which give you an OpenAI-compatible endpoint. A typical vLLM launch sets tensor parallelism to 8, FP8 quantization, and the GLM tool-call and reasoning parsers.


For local experimentation,[Unsloth](https://www.beam.cloud/blog/unsloth-fine-tuning) 's dynamic GGUF quants bring it within reach of serious workstations. The 2-bit dynamic build lands around 220 to 236GB, which fits a 256GB unified-memory Mac, or a single 24GB GPU paired with 256GB of system RAM and MoE offloading. The rule of thumb: your VRAM plus RAM needs to clear the file size, or llama.cpp starts spilling to SSD and the whole thing crawls. Expect 3 to 9 tokens per second on a consumer 2-bit setup. It works, it's just not fast.


Tooling-wise, llama.cpp, LM Studio, Ollama, and KTransformers all run the GGUF builds. One gotcha with Ollama: the` glm-5.1:cloud` tag routes to hosted inference rather than running anything on your machine, so for true local you want llama.cpp with the Unsloth GGUF. The weights live on Hugging Face under` zai-org/GLM-5.1` , the GGUFs under` unsloth/GLM-5.1-GGUF` , and the license is MIT, which means commercial use, modification, and redistribution with no royalties and no regional strings attached.


## Adding GLM 5.1 to your IDE


Z.ai exposes three endpoint families, and picking the wrong one is the most common setup mistake. There's the general pay-per-token API at` https://api.z.ai/api/paas/v4` , the Coding Plan endpoint at` https://api.z.ai/api/coding/paas/v4` , and an Anthropic-compatible endpoint at` https://api.z.ai/api/anthropic` . Coding Plan keys only work on the coding endpoints.


### Cursor


Cursor needs a Pro plan for custom models. Open Settings, go to Models, add a custom model using the OpenAI protocol. Override the base URL with` https://api.z.ai/api/coding/paas/v4` if you're on the Coding Plan, or the` paas/v4` URL if you're paying as you go. Paste your Z.ai key, then enter the model name in uppercase:` GLM-5.1` . Save it and pick it from the home screen. Direct Z.ai keys behave better in Cursor than routing through OpenRouter, and there's no first-party GLM integration to lean on, so the custom-model path is the path.


### VS Code


For Cline, Roo Code, or Kilo Code, add an OpenAI-compatible provider, point it at` https://api.z.ai/api/coding/paas/v4` , drop in your key, and set the model to` glm-5.1` . Continue.dev wants the same details in its config.json: provider` openai` , model` glm-5.1` , and the API base set to the paas/v4 URL. Zhipu also publishes CodeGeeX, its own VS Code extension, if you'd rather use the official tooling.


### Claude Code


This is the setup Z.ai leans on hardest, because it lets GLM-5.1 stand in for Claude. Edit` ~/.claude/settings.json` and point the Anthropic environment variables at Z.ai:


```text


```


Then run` claude` . Z.ai also ships a one-line installer and an` npx @z_ai/coding-helper` helper if you'd rather not edit JSON by hand. Three mistakes trip people up: using the general` paas/v4` URL instead of` /anthropic` , putting the key in` ANTHROPIC_API_KEY` instead of` ANTHROPIC_AUTH_TOKEN` , and forgetting to restart the terminal after saving. The model mapping happens server-side, so the interface may still say "Claude" while GLM runs underneath.


### Agent frameworks


GLM-5.1 shipped with day-one support for Claude Code, Cline, Roo Code, Kilo Code, OpenCode, Goose, Crush, and Z.ai's own ZCode. It handles both OpenAI-style and Anthropic-style tool schemas, so it drops into any framework that lets you point at a custom endpoint. For agent loops, turn the reasoning effort up and give it a generous tool-call budget. The long-horizon stamina is the whole reason to reach for it, so let it run.


## How GLM 5.1 compares to GLM 5.2


GLM-5.1 has already been superseded. Z.ai released GLM-5.2 on June 13, 2026, with a one-million-token context window and a higher SWE-Bench Pro score (62.1), at the same price. On the Coding Plan, calls that name GLM-5.1 now route to GLM-5.2 automatically. GLM-5.1 stays fully available as downloadable open weights and through the per-token APIs, and its benchmark profile is the reliable floor for what the line can do. But if you're choosing today, evaluate 5.2 before you settle.


So, who should use it? If your work is long, multi-file coding at volume, and cost or data control is on your mind, GLM-5.x is the strong pick, and the Coding Plan at $12.60 a month is hard to argue with. Pair it with a Claude or GPT subscription for the occasional task that needs frontier reasoning or image input, and you've covered both ends without paying frontier prices for the bulk of your work. If you need multimodal input, top-tier math, or sub-second latency, keep the closed models in the driver's seat. The good news is you no longer have to choose just one.


*Pricing and plan tiers shift often and are frequently promotional, so check z.ai before you pay. Parameter counts and exact release dates vary across sources; the figures here reflect the most consistent reporting as of late June 2026.*
