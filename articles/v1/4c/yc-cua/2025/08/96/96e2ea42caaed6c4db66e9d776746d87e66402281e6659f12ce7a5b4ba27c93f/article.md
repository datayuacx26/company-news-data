---
schema_version: "1.0.0"
document_id: "96e2ea42caaed6c4db66e9d776746d87e66402281e6659f12ce7a5b4ba27c93f"
company_key: "yc-cua"
company: "Cua"
source_id: "yc-cua-news-import-c869e0198521"
canonical_url: "https://cua.ai/blog/hud-agent-evals"
published_at: "2025-08-28T00:00:00+00:00"
first_seen_at: "2026-07-21T15:30:37.690309+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:9c85b082a654b1194707b9a39d06b241df61fe375dd4c9ed86ed1f72c2e77eed"
---

# Cua × HUD - Evaluate Any Computer-Use Agent

*Published on August 27, 2025 by Dillon DuPont*


You can now benchmark any GUI-capable agent on real computer-use tasks through our new integration with[HUD](https://hud.ai/) , the evaluation platform for computer-use agents.


If[yesterday's 0.4 release](https://cua.ai/blog/composite-agents.md) made it easy to compose planning and grounding models, today's update makes it easy to measure them. Configure your model, run evaluations at scale, and watch traces live in HUD.


## What you get


- One-line evals on OSWorld (and more) for OpenAI, Anthropic, Hugging Face, and composed GUI models.
- Live traces at[app.hud.ai](https://app.hud.ai/) to see every click, type, and screenshot.
- Zero glue code needed - we wrapped the interface for you.
- With Cua's Agent SDK, you can benchmark any configurations of models, by just changing the` model` string.


## What is OSWorld?


[OSWorld](https://os-world.github.io/) is a comprehensive evaluation benchmark comprising 369 real-world computer-use tasks spanning diverse desktop environments (Chrome, LibreOffice, GIMP, VS Code, etc.) developed by XLang Labs. This benchmark has emerged as the de facto standard for evaluating multimodal agents in realistic computing environments, with adoption by leading AI research teams at OpenAI, Anthropic, and other major institutions for systematic agent assessment. The benchmark was recently enhanced to[OSWorld-Verified](https://xlang.ai/blog/osworld-verified) , incorporating rigorous validation improvements that address over 300 community-identified issues to ensure evaluation reliability and reproducibility.


## Environment Setup


First, set up your environment variables:


```text
bash   export HUD_API_KEY="your_hud_api_key"       # Required for HUD access  export ANTHROPIC_API_KEY="your_anthropic_key" # For Claude models  export OPENAI_API_KEY="your_openai_key"       # For OpenAI models
```


## Try it


### Quick Start - Single Task


```text
python   from cua_agent.integrations.hud import run_single_task     await run_single_task(      dataset="hud-evals/OSWorld-Verified-XLang",      model="openai/computer-use-preview+openai/gpt-5-nano",  # or any supported model string      task_id=155  # open last tab task (easy)  )
```


### Run a dataset (parallel execution)


```text
python   from cua_agent.integrations.hud import run_full_dataset     # Test on OSWorld (367 computer-use tasks)  await run_full_dataset(      dataset="hud-evals/OSWorld-Verified-XLang",      model="openai/computer-use-preview+openai/gpt-5-nano",  # any supported model string      split="train[:3]"  # try a few tasks to start  )     # Or test on SheetBench (50 spreadsheet tasks)  await run_full_dataset(      dataset="hud-evals/SheetBench-V2",      model="anthropic/claude-sonnet-4-5-20250929",      split="train[:2]"  )
```


### Live Environment Streaming


Watch your agent work in real-time. Example output:


```text
md   Starting full dataset run...  ╔═════════════════════════════════════════════════════════════════╗  ║ 🚀 See your agent live at: ║  ╟─────────────────────────────────────────────────────────────────╢  ║ https://app.hud.ai/jobs/fe05805d-4da9-4fc6-84b5-5c518528fd3c ║  ╚═════════════════════════════════════════════════════════════════╝
```


## Configuration Options


Customize your evaluation with these options:


- **Environment types** :` environment="linux"` (OSWorld) or` environment="browser"` (SheetBench)
- **Model composition** : Mix planning and grounding models with` +` (e.g.,` "gpt-4+gpt-5-nano"` )
- **Parallel scaling** : Set` max_concurrent_tasks` for throughput
- **Local trajectories** : Save with` trajectory_dir` for offline analysis
- **Live monitoring** : Every run gets a unique trace URL at app.hud.ai


## Learn more


- Docs:[https://cua.ai/docs/cua/guide/integrations/hud](https://cua.ai/docs/cua/guide/integrations/hud)
- Live traces:[https://app.hud.ai](https://app.hud.ai/)
