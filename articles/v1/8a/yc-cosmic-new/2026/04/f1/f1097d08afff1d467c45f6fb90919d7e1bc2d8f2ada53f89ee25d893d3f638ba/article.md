---
schema_version: "1.0.0"
document_id: "f1097d08afff1d467c45f6fb90919d7e1bc2d8f2ada53f89ee25d893d3f638ba"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-rundown-qwen-36-kimi-k26-eu-battery-mandate"
published_at: "2026-04-20T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T20:52:31.063760+00:00"
content_hash: "sha256:2ddfa96a3fc541225253eb41334dd5c5e80aded3c15d193c5fc71f51d7a006d9"
---

# Cosmic Rundown: Qwen 3.6, Kimi K2.6, and the EU's Battery Mandate

## Qwen 3.6-Max-Preview Ships


Alibaba's Qwen team released[Qwen 3.6-Max-Preview](https://qwen.ai/blog?id=qwen3.6-max-preview) , their latest flagship model. The release positions it as sharper and smarter than previous versions, with improvements across reasoning and code generation tasks.


The "preview" label signals this is still evolving, but early benchmarks suggest it competes directly with Claude and GPT-4 class models. For teams building AI-powered applications, another strong open-weight contender means more options for self-hosting and fine-tuning.


[Hacker News discussion](https://news.ycombinator.com/item?id=47834565)


## Kimi K2.6 Targets Open-Source Coding


Moonshot AI released[Kimi K2.6](https://www.kimi.com/blog/kimi-k2-6) , focused specifically on advancing open-source coding capabilities. The model is designed for developers who need strong code generation without relying on closed APIs.


This continues the trend of specialized coding models. Rather than building general-purpose assistants, teams are shipping models optimized for specific developer workflows.


[Hacker News discussion](https://news.ycombinator.com/item?id=47835735)


## EU Mandates Replaceable Batteries by 2027


Starting in 2027,[all phones sold in the EU must have user-replaceable batteries](https://www.theolivepress.es/spain-news/2026/04/20/eu-to-force-replaceable-batteries-in-phones-and-tablets-from-2027/) . This applies to tablets too.


For hardware manufacturers, this is a significant design constraint. For everyone else, it means devices that last longer and generate less e-waste. The regulation follows the EU's successful push for USB-C standardization.


[Hacker News discussion](https://news.ycombinator.com/item?id=47834195)


## Atlassian Turns On AI Training by Default


Atlassian[enabled default data collection to train AI models](https://letsdatascience.com/news/atlassian-enables-default-data-collection-to-train-ai-f71343d8) across its products. If you use Jira, Confluence, or other Atlassian tools, your data may now be feeding their AI systems unless you opt out.


This follows a familiar pattern: ship the feature, enable it by default, bury the opt-out in settings. Worth checking your organization's Atlassian admin settings if this matters to your data policies.


[Hacker News discussion](https://news.ycombinator.com/item?id=47833247)


## ggsql: Grammar of Graphics for SQL


The Posit team (formerly RStudio) released[ggsql](https://opensource.posit.co/blog/2026-04-20_ggsql_alpha_release/) , bringing the Grammar of Graphics paradigm to SQL queries. If you've used ggplot2 in R, this applies similar composable syntax to data visualization directly from SQL.


The alpha release targets data analysts and engineers who want expressive visualization without pulling data into R or Python first.


[Hacker News discussion](https://news.ycombinator.com/item?id=47833558)


## 44% of Deezer Uploads Are Now AI-Generated


Deezer reported that[44% of songs uploaded daily are AI-generated](https://techcrunch.com/2026/04/20/deezer-says-44-of-songs-uploaded-to-its-platform-daily-are-ai-generated/) . Nearly half. Every day.


This creates real problems for music platforms: discovery algorithms get flooded, human artists get buried, and licensing questions multiply. Expect more platforms to implement AI content detection and disclosure requirements.


[Hacker News discussion](https://news.ycombinator.com/item?id=47835928)


## Quick Hits


**NSA using Anthropic's Mythos despite blacklist** -[Axios reports](https://www.axios.com/2026/04/19/nsa-anthropic-mythos-pentagon) the NSA is deploying Anthropic's Mythos model even though it's on a Pentagon blacklist. The gap between official policy and operational reality continues.


**Simon Willison's Claude Token Counter** - Simon released an[updated token counter](https://simonwillison.net/2026/Apr/20/claude-token-counts/) with model comparisons. Useful for estimating costs across Claude versions.


**WebUSB Extension for Firefox** - A new[WebUSB extension](https://github.com/ArcaneNibble/awawausb) brings USB device access to Firefox, narrowing the gap with Chromium browsers for hardware projects.


**Sauna effect on heart rate** - Research from TryTerra shows[how sauna sessions affect heart rate](https://tryterra.co/research/sauna-effect-on-heart-rate) , relevant for health tracking applications and wearable developers.


## What This Means for Your Stack


The AI model releases keep coming faster. Qwen 3.6 and Kimi K2.6 both push the state of the art for coding assistance, giving teams more choices for building AI-powered developer tools.


If you're building content systems, the Atlassian news is a reminder: data governance matters. Know where your content goes and who trains on it. Platforms like[Cosmic](https://www.cosmicjs.com/) keep your content under your control, with AI features that work for you rather than extracting value from your data.


The EU battery mandate won't affect most software teams directly, but it signals continued regulatory pressure on hardware design. If you're building IoT or embedded systems, factor in longer device lifespans and user serviceability.
