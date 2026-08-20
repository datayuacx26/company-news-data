---
schema_version: "1.0.0"
document_id: "7b385c1708dd42c8cc82b44f4bc20c2b2bd2c77a62f95f20cec4687b7b22cffc"
company_key: "yc-halluminate"
company: "Halluminate"
source_id: "yc-halluminate-news-import-9819d7d93bdb"
canonical_url: "https://www.halluminate.ai/blog/browser-use-benchmark"
published_at: null
first_seen_at: "2026-08-11T04:50:25.359940+00:00"
fetched_at: "2026-08-11T04:50:26.871958+00:00"
content_hash: "sha256:044259d9029a42753c6d4f740d94ba38f6aeeab0e1c76851b50ad38548805017"
---

# Browser Use on Web Bench

[Browser Use](https://browser-use.com/) is one of the open stacks teams reach for when they want a browser agent without building a harness from scratch. The useful benchmark question is not “does it demo well on one site?” It is whether it survives a broad, live-web mix of reads and writes.


Halluminate included **Browser Use Cloud API** in the[Web Bench](https://www.halluminate.ai/blog/benchmark) launch study. This page is the honest version of that result: what we ran, where the comparison is imperfect, and how to evaluate Browser Use yourself without fooling your own roadmap.


## What we ran


Web Bench’s open set is ~2,454 tasks across 452 real websites, drawn from a larger ~5,750-task pool. Tasks are tagged READ, CREATE, UPDATE, DELETE, and file manipulation. Launch grading was human review of trajectories and outputs.


Browser Use was executed through its Cloud API. Because of cost, that configuration used a **658-task subset** , not the full open set used for several other agents. Any head-to-head with full-set runs needs that caveat in the same sentence as the score.


Other systems in the same study included Skyvern 2.0, OpenAI Computer Use, Anthropic Computer Use, and human-in-the-loop Operator / Convergence baselines. Charts and protocol:[/blog/benchmark](https://www.halluminate.ai/blog/benchmark) . Public board:[webbench.ai](https://webbench.ai/) .


## What the study taught us about this category


Even without turning this page into a fake precision shootout on the subset, the category-level findings apply directly to Browser Use deployments:


1. **READ is the easy mode.** Strong agents often clear **>70%** of extraction-style tasks.
2. **WRITE is the product.** Best fully automated NON-READ in the launch study: **Skyvern 2.0 at 46.6%** . Overall fully automated SOTA: **Anthropic Computer Use at 66.0%** .
3. **Infrastructure is a co-author of your score.** Proxy, captcha, and auth failures were a major bucket—later isolated in[BrowserBench](https://www.halluminate.ai/blog/browserbench) , where infrastructure alone moved accuracy by large margins.


If your Browser Use workflow is “log in, fill the form, submit, confirm,” a read-heavy internal eval will lie to you.


## How to benchmark Browser Use without self-deception


1. **Match the task mix to the product.** Overweight CREATE / UPDATE / DELETE if that is what you sell.
2. **Prefer live sites for the gate** , simulated sites for the loop. Use[Westworld](https://www.halluminate.ai/blog/westworld) or staging apps to iterate; use Web Bench-style tasks before you call a release done.
3. **Freeze infrastructure when you compare agent versions.** Same proxy, same stealth provider, same auth strategy.
4. **Grade outcomes** , not screenshots of the agent looking busy.
5. **Report N.** If you sample 658 tasks, say 658. Subset results are useful; silent subsets are marketing.


## Where this sits next to Skyvern and WebArena


Skyvern’s launch-study write-task leadership is documented separately in[Skyvern Web Bench scores](https://www.halluminate.ai/blog/skyvern-webbench-scores) . WebArena remains the right tool for reproducible research clones—see[WebArena vs Web Bench](https://www.halluminate.ai/blog/webarena-vs-web-bench) —but it will not tell you whether Browser Use clears bot checks on a random retailer.


For the full map:[browser agent benchmarks in 2026](https://www.halluminate.ai/blog/browser-agent-benchmarks) . Dataset:[Halluminate/WebBench](https://huggingface.co/datasets/Halluminate/WebBench) . For custom eval design, jerry@halluminate.ai.
