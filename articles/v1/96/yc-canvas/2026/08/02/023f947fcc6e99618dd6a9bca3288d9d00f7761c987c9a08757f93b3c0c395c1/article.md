---
schema_version: "1.0.0"
document_id: "023f947fcc6e99618dd6a9bca3288d9d00f7761c987c9a08757f93b3c0c395c1"
company_key: "yc-canvas"
company: "Canvas"
source_id: "yc-canvas-news-import-8207a64dd502"
canonical_url: "https://www.canvas.inc/research/meta-agent"
published_at: null
first_seen_at: "2026-08-09T20:23:24.393484+00:00"
fetched_at: "2026-08-09T20:23:25.334371+00:00"
content_hash: "sha256:c30b41481f245a24637b6401f76eab76fc2d2e5982eefbaa7fa4abfb573a93d1"
---

# Meta-Agent

## Why


Recent work shows that optimizing the harness layer can materially improve agent performance.


On TerminalBench-2, vanilla Claude Code with Claude Haiku 4.5 scores 27.5%. The best hand-engineered harness on the same model reaches 35.5%, with no fine-tuning ([Meta-Harness, Lee et al. 2026](https://arxiv.org/abs/2603.28052) ).


Recent systems have also shown that harnesses can be improved automatically through iterative search ([Autoresearch, Karpathy 2026](https://github.com/karpathy/autoresearch) ;[Meta-Harness, Lee et al. 2026](https://arxiv.org/abs/2603.28052) ). But those methods usually depend on strong evaluation signals during search, such as labels, tests, or deterministic checks. Agents typically run on messy customer workflows where labeled reward is sparse or unavailable.


[meta-agent](https://github.com/canvas-org/meta-agent) is built for exactly that production setting: it reads traces from the running agent, uses an LLM judge to score them, and proposes harness updates from the failure patterns it finds.
