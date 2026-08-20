---
schema_version: "1.0.0"
document_id: "2b52517373e82a52d0d486fb68ca67b3b0b993337888bafde8ee02abe7d21b53"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/kimi-k3"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T14:00:11.907558+00:00"
fetched_at: "2026-08-13T14:00:13.011407+00:00"
content_hash: "sha256:902e02a5faa89bb97195d66c2e3e36b8e726576435558001c3e4eb3ca7741e91"
---

# What is Kimi K3? A Complete Developer Guide for 2026

## TL;DR


- Kimi K3 is Moonshot AI's new 2.8-trillion-parameter[open-source](https://www.firecrawl.dev/blog/best-open-source-web-crawler) model. As of August 2026 it's the largest open-weight model released, with 104B active parameters per token across 896 experts.
- It supports a 1,048,576-token context window (exactly 4x Kimi K2.7 Code's). It handles text,[images](https://www.firecrawl.dev/blog/website-to-image-imagen4-gemini-firecrawl-technical) and video natively.
- Kimi K3 goes toe-to-toe with frontier proprietary models like Claude Fable 5 and GPT-5.6 Sol in benchmark testing.
- You can access it through Hugging Face, OpenRouter, Fireworks AI, Baseten, Together AI, or Moonshot's own platform. Most first-party providers charge the same $3/$15 per 1M tokens, and OpenRouter's cheapest route undercuts them.
- [Self-hosting](https://www.firecrawl.dev/glossary/web-scraping-apis/what-is-self-hosted-web-scraping) is technically possible, but "possible" means a minimum of 8 enterprise-grade accelerators and a hardware bill in the high six figures. Crypto miners don't even have enough hardware to run K3.
- We got Kimi K3 running in OpenCode and gave it live web access using the Firecrawl[MCP](https://www.firecrawl.dev/blog/best-mcp-servers-for-developers) . The Firecrawl MCP extends Kimi K3's knowledge base and gives it access to live web data.


---


## What is Kimi K3?


Kimi K3 is Moonshot AI's newest open-weight model, and as of August 2026 the largest one anyone has released at 2.8 trillion parameters. It succeeds Kimi K2.7 Code, reads text, images and video natively, and holds up to 1,048,576 tokens in a single context window. Moonshot opened API access on July 16, 2026, then published the full weights on July 27.


## What are Kimi K3's specs?


Kimi K3 has some really impressive specs, especially for an open source model. Kimi K3 comes in with 2.8 trillion parameters. Models like[GLM-5.2 and Kimi K2.7 Code](https://www.firecrawl.dev/blog/glm-5-2-vs-kimi-2-7-code) range from roughly 700 billion to just over 1 trillion. Its architecture provides Kimi K3 with 896 different experts when performing different tasks. It also boasts a context window of over 1,000,000 tokens. Kimi K2.7 Code had a context window of just 262,144, so K3 quadruples it. Kimi K3 supports text, images and video natively.


- **Total Parameters** : 2.8 trillion
- **Activated Parameters per Token** : 104B
- **Architecture** : Mixture-of-Experts (MoE), Stable LatentMoE
- **Number of Experts** : 896 total, 16 selected per token, 2 shared
- **Layers** : 93 (69 KDA + 24 Gated MLA, 1 dense layer)
- **Context Window** : 1,048,576 tokens
- **Quantization** : MXFP4 MoE expert weights / MXFP8 activations, quantization-aware trained from SFT onward (non-expert components stay in higher precision)
- **Modality** : Text, image and video (native); vision encoder MoonViT-V2 (401M params)
- **License** : Custom "Kimi K3 License" (not plain MIT)


These stats were from[Kimi K3's Hugging Face page](https://huggingface.co/moonshotai/Kimi-K3) .


Moonshot's[Kimi K3 technical report](https://arxiv.org/abs/2607.24653) credits its architecture and training changes, including Kimi Delta Attention, Attention Residuals and Stable LatentMoE, with roughly a 2.5x gain in overall scaling efficiency over Kimi K2.


Kimi's weights were released on July 27, 2026, as you can see in Kimi.ai's X post below.


### Kimi K3 coding benchmarks


On Moonshot AI's technical blog, they go through Kimi K3's benchmarks, which are on par with frontier proprietary models as well. Every number in this section comes from that blog. A few of them shifted slightly in the arXiv technical report, which was revised after launch, so it's worth checking both if a specific score matters to you.


*Source:[https://www.kimi.com/blog/kimi-k3](https://www.kimi.com/blog/kimi-k3)*


- **DeepSWE** : Nearly tied GPT-5.5, just barely edging it out 67.5 to 67.0. Kimi K3 was behind only GPT-5.6 Sol (73.0) and Claude Fable 5 (70.0).
- **FrontierSWE** : Beat GPT-5.6 Sol, GLM-5.2, Opus 4.8 and GPT-5.5 with a score of 81.2, coming in second only to Claude Fable 5 (86.6).
- **Kimi Code Bench 2.0 (internal)** : Finished second behind Claude Fable 5 (76.9) with a score of 72.9.
- **Terminal Bench 2.1** : Nearly tied GPT-5.6 Sol (88.8) with a score of 88.3, taking second place.
- **Program Bench** : Beat all other models with a score of 77.8, including GPT-5.6 Sol (77.6).
- **SWE Marathon** : Scored 42.0 and beat all other models tested. Worth noting that Claude Fable 5 hit harness fallbacks on 35% of these tasks, which drags its 35.0 down.


### Kimi K3 general agent performance


*Source:[https://www.kimi.com/blog/kimi-k3](https://www.kimi.com/blog/kimi-k3)*


- **GDPval-AA V2 Elo** : Finished behind Fable 5 and GPT-5.6 Sol, beating all other models.
- **AA-Briefcase Elo** : Finished second (1548) only to Claude Fable 5 (1583).
- **Automation Bench** : Beat all other models (30.8). The closest runner up was GPT-5.6 Sol (29.7).
- **JobBench** : Finished second (52.9) to Claude Fable 5 (57.4).
- **SpreadsheetBench 2** : Beat all other models with a score of 34.8. Claude Fable came in second with a score of 34.7.
- **BrowseComp** : Outperformed all other models with a score of 91.2. GPT-5.6 Sol came in second at 90.4. That 91.2 uses a context-compaction strategy; run with the full 1M window and no context management, K3 scores 90.4 and ties Sol.


### Kimi K3 visual agent performance


*Source:[https://www.kimi.com/blog/kimi-k3](https://www.kimi.com/blog/kimi-k3)*


Kimi K3 came in second to Claude Fable 5 on both tests, and both sets of numbers are the Python-tool-assisted runs. On CharXiv (RQ) with Python, Kimi K3 scored 91.3 against Fable 5's 93.5. On ZeroBench with Python, Kimi K3 tied GPT-5.5 at 41.0 against Fable 5's 46.0, while GPT-5.6 Sol and Claude Opus 4.8 both landed below 40. That qualifier matters: without tool access, K3 drops to 84.8 on CharXiv and 23.0 on ZeroBench, where it ties Fable 5 rather than trailing it.


Kimi K3 is highly efficient at handling visual tasks. If your project needs agentic[browser automation](https://www.firecrawl.dev/blog/browser-automation-tools-comparison) or anything else requiring computer vision, Kimi K3 is definitely worth a look.


## Where can I get Kimi K3?


Provider Input (per 1M tokens) Cached Input (per 1M tokens) Output (per 1M tokens)


Hugging Face Free (weights download) n/a n/a


OpenRouter (cheapest route) $2.80 $0.29 $14.00


Fireworks AI $3.00 $0.30 $15.00


Baseten $3.00 $0.30 $15.00


Together AI $3.00 $0.30 $15.00


Official Kimi Website (platform.kimi.ai) $3.00 $0.30 $15.00


- **[Hugging Face](https://huggingface.co/moonshotai/Kimi-K3)** : This is the canonical source for Kimi K3's weights direct from Moonshot AI. Start here if you're self-hosting.
- **[OpenRouter](https://openrouter.ai/moonshotai/kimi-k3)** : A routing layer that gives you a single API endpoint across a variety of providers hosting Kimi K3. It comes with automatic failover and three routing modes (Balanced, Nitro for speed, Exacto for tool-calling accuracy). Its headline $2.80/1M input and $14/1M output is the cheapest routed provider, not an OpenRouter-wide rate: individual endpoints run from $2.80 to $6.00 input and $14.00 to $22.50 output, and Moonshot's own endpoint here is $3.00/$15.00. The full 1,048,576-token context is available, though max output tokens vary by provider.
- **[Fireworks AI](https://fireworks.ai/models/fireworks/kimi-k3)** :[Managed](https://www.firecrawl.dev/blog/claude-managed-agents) API access with Fast, Priority, and US-only serverless tiers (for regulated industries) as well as on-demand dedicated GPU deployment and LoRA[fine-tuning](https://www.firecrawl.dev/blog/custom-instruction-datasets-llm-fine-tuning) support. Standard pricing runs $3.00/1M input, $0.30/1M cached input, $15.00/1M output. The faster tiers cost more on top of that (Priority +25%, Fast +50%, US-only +10%). Zero data retention is enabled by default across all Fireworks inference.
- **[Baseten](https://www.baseten.co/library/kimi-k3/)** : Offers a pay-per-token Model API ($3.00/1M input, $0.30/1M cached, $15.00/1M output). Baseten also sells dedicated and self-hosted deployments on its platform generally, though K3 launched there on the Model API only. Note their model page lists the license as MIT, which is wrong; Hugging Face is the authority here.
- **[Together AI](https://www.together.ai/models/kimi-k3)** :[Serverless](https://docs.firecrawl.dev/quickstarts/aws-lambda) and dedicated inference with a detailed public benchmark comparison against Claude Fable 5, Claude Opus 5, GPT-5.6 Sol, GPT-5.6 Luna, and Grok 4.5. Pricing: $3.00/1M input ($0.30 cached), $15.00/1M output. Their model page lists a July 27, 2026 release, while their own K3 guide puts availability on Together at August 1.
- **[Official Kimi Website](https://www.kimi.com/)** : Moonshot's own consumer chat interface and the` platform.kimi.ai` API (model name` kimi-k3` ), with an OpenAI/Anthropic-compatible API format. This is the most direct line to Moonshot if you want to skip third-party providers. Pricing is also $3.00/1M input ($0.30 cached) and $15.00/1M output.


Every first-party provider charges the same $3.00/1M input, $0.30 cached, $15.00/1M output as Moonshot itself. The only way to pay less is OpenRouter's cheaper routes, which start at $2.80/1M input and $14/1M output. Bear in mind that OpenRouter is a router, not a single price: its more expensive endpoints run to $6.00 input and $22.50 output, so pin your route if cost matters.


## What do I need to self host Kimi K3?


Spec Requirement


Repository size ~1.561 TB (96 Safetensors shards)


VRAM planning floor 1,680 GB (vLLM estimate)


Minimum single-node floor 8× B300, GB300, or MI355X (288GB each)


Hopper/Blackwell 16-GPU tier 16× H200 (141GB) or 16× B200 (180GB)


Recommended production tier 64+ accelerators


Storage (staging) 4TB NVMe


If you're looking to self host Kimi K3, you need a datacenter for best results. However, with a medium-sized pile of datacenter hardware, self hosting is possible. These numbers were collected by[Kingy AI](https://kingy.ai/ai/ai-guides/run-kimi-k3-locally-hardware-vram-cost/) , which pulled most of them from[vLLM's Kimi K3 recipe](https://recipes.vllm.ai/moonshotai/Kimi-K3) and the Hugging Face repo.


Buying that hardware outright is a high-six-figure decision. By our own math, eight B300s alone run about $424,000 at roughly $53,000 each, and a complete node lands somewhere in the $400,000 to $500,000 range once you add CPUs, NVSwitch fabric, networking, storage and the liquid cooling it requires. An AMD MI355X build comes in lower, closer to $250,000 to $350,000. Renting is far better documented: Kingy AI clocks 8x B300 at about $59 an hour, which is roughly $43,000 a month running continuously.


- **Repository size** : 1,561,018,243,668 bytes (~1.561 TB) across 96 Safetensors shards. The shards on their own come to 1.5609 TB; the rest is config and tokenizer files.
- **VRAM planning floor** : 1,680GB. This is vLLM's pre-release estimate (2.8 trillion parameters at half a byte each, plus 20% headroom), not a measured requirement, and it does not budget for KV cache.
- **Minimum viable single-node floor** : 8 × B300, GB300 or MI355X (288 GB each)
- **Hopper/Blackwell 16-GPU tier** : 16 × H200 (141 GB each) or 16 × B200 (180 GB each). This tier fits the weights but leaves little for KV cache: vLLM's shipped Hopper recipe caps context at 32,768 tokens, and the 1M window only appears in its Blackwell profile with fp8 KV cache.
- **Moonshot's recommended production tier** : 64 or more accelerators, in supernode configurations.
- **Storage (staging)** : 4TB NVMe. No vendor specifies this; it's Kingy AI's rule of thumb for holding the checkpoint plus room to convert or quantize it.


Practically speaking, you can host Kimi K3 with a "small" datacenter. The requirements above are the bare minimum to get the model working.


Those numbers all describe the full-precision MXFP4 checkpoint. Community quantization has since lowered the floor: Unsloth's 1-bit GGUF builds shrink K3 to about 594 GB, small enough for a high-RAM workstation or as few as four datacenter GPUs, at a real cost in accuracy. It's a way to poke at the model, not a way to serve it.


LinkedIn user[Luke Mercado](https://www.linkedin.com/in/lukemercado/) summed it up pretty well in the meme below.


If you're using it in production, you kind of need your own datacenter. You can read Moonshot AI's full technical report on arXiv:[Kimi K3: Open Frontier Intelligence](https://arxiv.org/abs/2607.24653) .


## Using Kimi K3 in OpenCode


Now it's time to get started with Kimi K3 and[OpenCode](https://www.firecrawl.dev/blog/best-opencode-skills) . We'll get started by installing via npm.


```text
npm   install   -g   opencode-ai
```


After you've installed, you need to log in to your model provider.


```text
opencode   auth   login
```


Launch OpenCode.


```text
opencode
```


## How do I give Kimi K3 web access?


To give Kimi K3 web access, first we'll install the[Firecrawl CLI](https://www.firecrawl.dev/blog/introducing-firecrawl-skill-and-cli) .


```text
npm   install   -g   firecrawl-cli
```


Next, you can run` setup mcp` to configure an MCP connection.


```text
firecrawl   setup   mcp
```


The Firecrawl CLI will give you an option to select a number of environments like[Claude Code](https://www.firecrawl.dev/blog/claude-code-skill) , OpenCode,[Cursor](https://www.firecrawl.dev/blog/firecrawl-mcp-in-cursor) and more.


When it's finished, relaunch OpenCode. Ask Kimi K3 if it can find the MCP.


### Running OpenCode on Windows


The above methods ran into issues on Windows. However, OpenCode works just fine using WSL in Ubuntu.


One thing to know here, and it applies on every platform rather than just WSL: if you've already signed in with` firecrawl login` ,` firecrawl setup mcp` will refuse to write your stored API key into a client config. That's a deliberate security rule, and the CLI tells you as much. You can either export` FIRECRAWL_API_KEY` into your environment yourself, or skip the problem entirely with the launcher, which configures the MCP and starts OpenCode with the key already in its environment.


```text
firecrawl   launch   opencode
```


Firecrawl launches OpenCode for us with the MCP server configured. Here, I prompt Kimi K3 to find the[latest news](https://www.firecrawl.dev/blog/best-news-api) .


```text
do a search for the latest news using the firecrawl mcp
```


Kimi K3 then finds the latest news using[Firecrawl search](https://www.firecrawl.dev/blog/firecrawl-search-openrouter) . We get a variety of stories on politics, wars, elections and diseases. The search worked and our harness is good!


Perhaps one day the news will be good too.


## Anyone can use Kimi K3 (just not on their own machine)


Accessing Kimi K3 through various providers is easy. If you're calling Moonshot's hosted API directly, or a third party aggregator, you can get it up and running.


I will say that running OpenCode on native Windows is not so easy. As mentioned earlier, I ran into issues. My issue was an[OpenCode bug where a crash leaves the terminal full of raw ANSI escape codes and a hidden cursor](https://github.com/anomalyco/opencode/issues/21277) , forcing you to close PowerShell and start over. The issue is still open. If you're running OpenCode on Windows, use some form of WSL. It will save you from stress and headache.


Don't expect to run Kimi K3 locally without a[Series A](https://www.firecrawl.dev/blog/firecrawl-v2-series-a-announcement) or some type of seed funding.
