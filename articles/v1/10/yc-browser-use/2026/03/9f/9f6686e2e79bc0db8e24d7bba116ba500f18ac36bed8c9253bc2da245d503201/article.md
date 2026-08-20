---
schema_version: "1.0.0"
document_id: "9f6686e2e79bc0db8e24d7bba116ba500f18ac36bed8c9253bc2da245d503201"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-3219c37ba697"
canonical_url: "https://browser-use.com/posts/online-mind2web-benchmark"
published_at: "2026-03-25T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:29.258885+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:d20ccda77fb8e93fcfd421f6e0b925c17ae6bf89bc1441be14c761b0217f6592"
---

# How we built the best browser agent with Auto-Research

## The Benchmark


Online-Mind2Web is the most widely reported browser agent benchmark. 300 tasks across 136 real websites - shopping, finance, travel, government, and more. Every web agent is tested on it.


Difficulty Tasks Example


Easy 83 "Open the reviews of a recipe with beef sirloin" - allrecipes.com


Medium 143 "Find full-time legal jobs in San Diego County, min $4,000+/month" - ca.gov


Hard 74 "Find the cheapest hotel + flight + car package from New York to San Francisco, for two adults and a six-year-old child, with free breakfast and spa" - booking.com


**Total** **300**


## How Auto-Research for browser agents works


We give Claude Code a CLI to our eval platform and a prompt to run in a loop:


No orchestration code. Each Claude Code session gets a goal and runs 20 cycles on its own. With this[eval infra](https://browser-use.com/posts/our-browser-agent-evaluation-system) we run them in parallel and get a search tree over the space of possible best agents.


We don't care about low performers. We only care about getting the best agent. So we tell the coding agent to make big bets and avoid small changes, since small tweaks get lost in run-to-run variance.


## The debugging CLI for deep dives


One trace can be millions of tokens. That's why we designed the CLI in 3 hierarchical levels to let the agent find root causes as fast as possible.


TSV over JSON saves 40% of tokens for our data structure. Small format choices can make or break agentic debugging.


## The biggest improvement


Claude Code updated our browser agent harness into a coding agent. Instead of only tools like click and type, it added Python to parse HTML and extract data. This aligns much better with the LLM's training distribution and makes edge cases and data extraction dramatically easier.


The rest: fixing hundreds of edge cases with the loop.


**What we added:** The[stealthiest browser infrastructure](https://browser-use.com/posts/stealth-benchmark) and every day new tasks our power users ask us to fix. Online-Mind2Web improved as a side effect.


## The Leaderboard


Agent Score Is Data Public? Source


**Browser Use Cloud (` bu-max` )** **97%** ✓[GitHub](https://github.com/browser-use/online-mind2web)


GPT-5.4 Native Computer Use 93% ✗[Blog](https://openai.com/index/introducing-gpt-5-4/) *


UI-TARS-2 88% ✗[arXiv](https://arxiv.org/abs/2509.02544)


ABP + Claude Opus 4.6 86% ✓[GitHub](https://github.com/theredsix/abp-online-mind2web-results)


OpenAGI Lux 84% ✗[Blog](https://www.agiopen.org/blog)


TinyFish 81% ✓[TinyFish blog](https://www.tinyfish.ai/blog/mind2web)


Navigator (Yutori) 79% ✗[Yutori blog](https://yutori.com/blog/introducing-navigator)


ChatGPT Atlas Agent Mode 71% ✗[OpenAI blog](https://openai.com/index/introducing-gpt-5-4/)


Google Gemini CUA 69% ✓[Official leaderboard](https://huggingface.co/spaces/osunlp/Online_Mind2Web_Leaderboard)


Stagehand (Gemini 2.5 CU) 65% ✓[Stagehand evals](https://www.stagehand.dev/agent-evals)


OpenAI Operator 61% ✓[Official leaderboard](https://huggingface.co/spaces/osunlp/Online_Mind2Web_Leaderboard)


Claude Sonnet 4.0 CU 61% ✗[OpenAGI blog](https://www.agiopen.org/blog)


Stagehand (Sonnet 4.5) 55% ✓[Stagehand evals](https://www.stagehand.dev/agent-evals)


*OpenAI reported their score without publishing the judge, harness, or task-level results, so independent verification isn't possible.


## The judge matters


The original judge is screenshot-based. But browser agents now write code, call APIs and extract thousands of items. For traditional judges, that's hallucination. If your agentic capabilities increase, you need an agentic judge.


We built an agentic judge on the Claude Agent SDK. We aligned it with human judges, which was key to making the auto-research loop work.


## Are we overfitting?


The natural tendency of the auto-research loop is to overfit on single tasks. You need to prompt the research system hard to generalize. Most of my time merging cycles is rejecting task-specific solutions that overfit.


We use train/validation splits. The loop only sees training data. We then run on old datasets it has never seen and see score improvements across the board.


## No tasks removed


Many companies remove tasks they consider impossible before reporting their score. We use all 300 tasks. Our few failures come from unavailable sites, ambiguous prompts, or websites that changed since the benchmark was created.


## Rerunning our results is easy


Clone[github.com/browser-use/online-mind2web](https://github.com/browser-use/online-mind2web) , set your Browser Use API key, and run.


We also uploaded prompts, results, and judgments.


## We need harder benchmarks


We're building a benchmark with everything users actually care about. Currently benchmarks ignore tasks like: "Extract 1000 products with subpages and compare them across platforms" because it was unimaginable that a single browser agent could do this.


Stay tuned.


[Try the best web agent yourself](https://cloud.browser-use.com/)
