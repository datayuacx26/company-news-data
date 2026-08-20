---
schema_version: "1.0.0"
document_id: "374ed136313b04ed00f2f5d2fb53450559f15b4fab0bd57cbe5c060d80fc2c75"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/kimi-k3-release-date"
published_at: "2026-07-17T21:35:00+00:00"
first_seen_at: "2026-07-21T18:00:01.007987+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:9b1792aa25bd876c3a01cf80b21eaeb3a19953a830f74359d5de87893738fa84"
---

# Kimi K3 Release Date: Two Launches, Explained

Kimi K3 shipped in two stages. The model went live through Moonshot AI's own products and API on July 16, 2026, and the full 2.8 trillion parameter weights were published on July 27, 2026 alongside a technical report. Both dates have passed. You can use the model through Moonshot's API today, or download it and run it yourself.


If you have seen conflicting information about K3's release, the two-stage rollout is why. "Launched" and "downloadable" were ten days apart, and a lot of coverage written in that window is still circulating.


## What Happened on July 16


On July 16, Moonshot AI introduced Kimi K3 as its most capable model: 2.8 trillion parameters, a 1-million-token context window, native vision covering text, image, and video input, and always-on reasoning. At that point the model ran at maximum effort with no alternative setting.


From that date, K3 became available through Moonshot's own ecosystem:


- [Kimi.com](https://www.kimi.com/) (web chat)
- The Kimi app on iOS, Android, and HarmonyOS
- [Kimi Work](https://www.kimi.com/products/kimi-work) desktop (version 3.1.0 or later) for knowledge work tasks
- [Kimi Code](https://www.kimi.com/code) for terminal-based coding via the /model command
- The[Kimi API](https://platform.kimi.ai/) at $0.30 per million tokens for cache-hit input, $3.00 for cache-miss input, and $15.00 for output


Moonshot also noted that its inference architecture, Mooncake, achieves a cache hit rate above 90% in coding workloads, which brings the effective input cost closer to the $0.30 cached tier for iterative tasks.


## What Happened on July 27


Moonshot published the full model weights to its[Hugging Face organization](https://huggingface.co/moonshotai) on July 27, 2026, along with the Kimi K3 technical report covering architecture, training, and evaluation. The repository runs to roughly 1.56 TB (1.42 TiB).


Three things shipped with the weights that had been open questions since July 16.


**The license.** K3 did not ship under MIT, and it did not ship under the Modified MIT terms that earlier Kimi models used. Moonshot wrote a bespoke document called the Kimi K3 License, tagged license:other on Hugging Face. Internal use is unconditional. Two conditions are not: anyone giving third parties inference or fine-tuning access needs a separate agreement with Moonshot once combined licensee and affiliate revenue passes $20 million over any consecutive 12 months, and products above 100 million monthly active users or $20 million in monthly revenue must display "Kimi K3" in the interface. Access through Moonshot's own products or certified inference partners is exempt.


**Additional reasoning effort levels.** The reasoning_effort parameter now accepts low, high, and max, with max as the default. This was listed as an undated future update at launch. Thinking still cannot be disabled at any setting.


**A serving path.** Official deployment recipes exist for vLLM, SGLang, and TokenSpeed, including the KDA prefill cache implementation Moonshot contributed to vLLM ahead of the release. Community quantizations followed quickly for llama.cpp, Ollama, LM Studio, and Jan, and Together AI is now listed as an inference provider.


One note on the exact release moment, because coverage disagrees. Moonshot's stated target was July 27 at 00:00 UTC, which is 8:00 PM Eastern on July 26. Some outlets reported the files appearing around 7:30 PM Eastern on July 26 and described that as roughly a day early, but measured against a 00:00 UTC target it is closer to half an hour. Other trackers checking the Hugging Face page at midday UTC on July 27 still saw an upcoming-release placeholder. The weights are live now, and any precise timestamp you encounter is worth treating with caution.


## Why the Distinction Matters


The difference between "model is live via API" and "model weights are downloadable" is significant, and it is a pattern you will see with most major AI model releases.


When a model is API-only, you are renting access. You send requests to the provider's servers, pay per token, and work within their infrastructure. That is what Kimi K3 was between July 16 and July 27. It works well for most use cases, but you are dependent on the provider for uptime, pricing, and any usage restrictions.


When model weights are released as open-weight, the dynamic changes. You can run the model yourself. You can modify it. You can deploy it in environments where sending data to an external API is not an option. For companies building products on top of AI models, open weights mean control over costs at scale, the ability to fine-tune for a specific domain, and no single point of failure tied to one provider's API.


At 2.8 trillion parameters, Kimi K3 is the largest open-weight model released to date, which made this particular weights release one of the more closely watched events in the open-source AI space.


## Quick Reference: Kimi K3 Release Timeline


**July 16, 2026: Hosted launch.** Available via Kimi.com, the Kimi app, Kimi Work, Kimi Code, and the Kimi API. Pricing live at $0.30/$3.00/$15.00 per million tokens (cached input, uncached input, output). Reasoning ran at maximum effort with no alternative setting.


**July 27, 2026: Open weights.** The full 2.8 trillion parameter checkpoint published to Moonshot's Hugging Face organization under the Kimi K3 License, roughly 1.56 TB. Technical report published alongside, covering architecture, training, and evaluation.


**Also shipped July 27: Reasoning effort control.** reasoning_effort now accepts low, high, and max, with max the default. Thinking cannot be disabled.


## What This Means for Builders


Both routes are open now, and the choice is about operations rather than timing.


The API remains the simplest path for most work, including coding, research, and knowledge tasks. You get the model through platform.kimi.ai with the model ID kimi-k3, no infrastructure to provision, and pricing you can forecast per token.


Self-hosting is the route if you need data residency, fine-tuning, air-gapped deployment, or volume high enough that owned hardware beats per-token cost. Two things to check before committing. The hardware bar is real: Moonshot's own guidance recommends 64 or more accelerators, while SGLang's published serving recipes start at 8 GPUs on the newest Blackwell and MI350-class silicon and rise to 32 on H100. And the license conditions matter if you plan to resell model access, since the $20 million revenue trigger lands on exactly that business shape. Read the LICENSE file in the repository before you build on it.


Either way, the broader trend holds. Models at the frontier performance tier are becoming accessible outside closed APIs, which means the platforms and tools builders rely on, including AI-powered app builders like[Emergent](https://emergent.sh/) , continue to get more capable as the models powering them improve.


Stay tuned to[Emergent News](https://emergent.sh/news) for more on AI tools, launches, and what they mean for builders.
