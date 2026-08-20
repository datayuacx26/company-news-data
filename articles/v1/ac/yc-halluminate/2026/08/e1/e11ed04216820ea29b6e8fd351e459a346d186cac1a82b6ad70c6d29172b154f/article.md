---
schema_version: "1.0.0"
document_id: "e11ed04216820ea29b6e8fd351e459a346d186cac1a82b6ad70c6d29172b154f"
company_key: "yc-halluminate"
company: "Halluminate"
source_id: "yc-halluminate-news-import-9819d7d93bdb"
canonical_url: "https://www.halluminate.ai/blog/skyvern-webbench-scores"
published_at: null
first_seen_at: "2026-08-11T04:50:25.359940+00:00"
fetched_at: "2026-08-11T04:50:26.871958+00:00"
content_hash: "sha256:0ff11ed99000ceceac3f4c957bcc9af4d93163b6e4767ab5accfe90c61bb9cc0"
---

# Skyvern Web Bench Scores

In Halluminate’s[Web Bench launch evaluation](https://www.halluminate.ai/blog/benchmark) , the best fully automated agent on write-style work was:


**Skyvern 2.0 — 46.6% success on NON-READ tasks**


That is the number behind most “Skyvern WebBench score” and “Skyvern write tasks benchmark” lookups. The rest of this post explains what NON-READ means, how Skyvern was run, and why a mid-40s write score can still be the leaderboard story.


## Read looks solved. Write does not.


Web Bench tasks on live sites fall into READ versus everything else (CREATE, UPDATE, DELETE, file manipulation). The launch study’s shape was consistent across strong systems:


- Many agents clear **>70%** of READ tasks
- Fully automated overall SOTA: **Anthropic Computer Use at 66.0%**
- Fully automated NON-READ leader: **Skyvern 2.0 at 46.6%**
- OpenAI Operator with human-in-the-loop was the strong overall baseline for comparison


Write trajectories are longer. They collide with login, form validation, confirmation modals, and state that has to exist before an update or delete can succeed. We paired create/update/delete work so tasks stayed self-contained; failing either half of a pair still counts against you. That is closer to real automation than a scrape-only suite.


If you are buying or benchmarking a browser agent for RPA-shaped work, the NON-READ slice is the number that should move budget—not a blended score inflated by extraction.


## How Skyvern was tested


The study included several agent and infrastructure combinations:


- Skyvern 2.0 on Skyvern infrastructure
- Skyvern 2.0 on Browserbase infrastructure
- OpenAI Computer Use on Skyvern infrastructure
- Anthropic Computer Use on Skyvern infrastructure
- Browser Use Cloud API on a smaller **658-task** subset
- Human-in-the-loop baselines (Operator, Convergence AI)


We graded with human annotators watching trajectories and outputs, including a “bad task” label for live sites that changed out from under the dataset. Methodology, charts, and failure taxonomy are in the[full Web Bench post](https://www.halluminate.ai/blog/benchmark) . Trajectory browser from the launch partnership:[eval.skyvern.com](https://eval.skyvern.com/) . Skyvern’s own write-up is[here](https://blog.skyvern.com/web-bench-a-new-way-to-compare-ai-browser-agents/) .


## Infrastructure can erase a model win


A large share of Web Bench failures were not “the LLM chose the wrong button.” They were proxy blocks, captchas, and auth walls. We later isolated that layer in[BrowserBench](https://www.halluminate.ai/blog/browserbench) , where infrastructure alone could swing accuracy by **25–50%** .


So when you compare Skyvern to another agent on live sites, hold the browser stack constant—or report both. Otherwise you are scoring the ISP and the fingerprint more than the planner.


## Live leaderboard vs this page


[webbench.ai](https://webbench.ai/) can move as new runs land. This page freezes the **launch-study** figures from the Halluminate report so “what did Skyvern score on Web Bench?” has a stable citation. For the open task set itself, see[Halluminate/WebBench on Hugging Face](https://huggingface.co/datasets/Halluminate/WebBench) .


If you are comparing open-source stacks next,[Browser Use on Web Bench](https://www.halluminate.ai/blog/browser-use-benchmark) covers how that Cloud API subset was handled. For the wider eval landscape, start at[browser agent benchmarks in 2026](https://www.halluminate.ai/blog/browser-agent-benchmarks) .
