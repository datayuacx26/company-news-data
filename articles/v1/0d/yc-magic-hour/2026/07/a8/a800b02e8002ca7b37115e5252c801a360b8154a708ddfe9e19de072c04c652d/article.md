---
schema_version: "1.0.0"
document_id: "a800b02e8002ca7b37115e5252c801a360b8154a708ddfe9e19de072c04c652d"
company_key: "yc-magic-hour"
company: "Magic Hour"
source_id: "yc-magic-hour-news-import-988efd6b5de7"
canonical_url: "https://magichour.ai/blog/ltx-2-3-vs-wan-2-2"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-25T03:27:13.496224+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:a5bc0110b3bd5d717e623f3fd6327d00415944fd32dbffd5f9176b71f2dcc62b"
---

# LTX-2.3 vs Wan 2.2: the two open video models compared

## TL;DR


- Wan 2.2 makes better-looking video: the highest measured motion scores of the open models, better anatomy and face consistency, and an Apache 2.0 license with no rights claimed over your outputs.
- LTX-2.3 is roughly six times faster in our measurement, generates synchronized audio, runs clips to 30 seconds where Wan stops at 15, and is still getting open releases; Wan's open weights stop at 2.2. Companies past $10 million in annual revenue need a paid Lightricks agreement.
- The working pattern practitioners converge on: iterate and produce on LTX-2.3 for speed, sound, and length; use Wan 2.2 where realism, motion, or face consistency decides the job.


LTX-2.3 and Wan 2.2 are the two open-weights video models worth comparing in 2026, and they pull in opposite directions. Wan 2.2 renders better motion, anatomy, and faces. LTX-2.3 generates several times faster, adds synchronized audio in the same pass, and runs longer clips.


The projects are also on different paths. Wan's open weights stop at 2.2: Alibaba's newer Wan releases are closed,[to loud community disappointment](https://github.com/Wan-Video/Wan2.2/issues/181) . Lightricks keeps shipping open LTX releases. So Wan has the quality lead today, and LTX is the only one of the two still improving in the open.


We serve both families through our API, so we measured the speed and cost claims directly: the same 5-second, 720p prompt through both models. wan-2.2 took about four minutes at 16 fps; ltx-2.3 finished in under 45 seconds at 24 fps; both cost $0.72. The run logs and both output clips are in a[public repo](https://github.com/maniculehq/mh-ltx-vs-wan) so you can check them. The rest of the piece walks through the runs, the practitioner reports, the license terms, and the hardware each model needs.


> Both models run on[Magic Hour's video API](https://manicule.link/mh-ltxwan) with no GPU on your side. New accounts get 400 free credits plus 100 a day, and the code **FIRSTAPI** takes 10% off your first API credit purchase.


The run logs and both output clips are in a[public repo](https://github.com/maniculehq/mh-ltx-vs-wan) so you can check them.


## Which of the two should you use?


Wan 2.2 for output quality, LTX-2.3 for speed, sound, and length. Wan 2.2 is licensed[Apache 2.0](https://github.com/Wan-Video/Wan2.2) with no rights claimed over generated content. LTX-2.3 returned the same clip roughly six times faster at the same credit cost and runs jobs up to 30 seconds, but the[LTX-2 Community License](https://github.com/Lightricks/LTX-2/blob/main/LICENSE) requires a paid agreement from any company with $10 million or more in annual revenue.


Here is what the two runs returned, same prompt, same endpoint:


Model


Wall clock


Resolution


Frame rate


File size


Credits


Cost per clip


wan-2.2


About 4 minutes


1280x720


16 fps


2.78 MB


240


$0.72


ltx-2.3


Under 45 seconds


1296x720


24 fps


8.94 MB


240


$0.72


Cost per clip uses the credit-pack rate of $0.003 per credit. At the usage-based Starter rate the same clip comes to about $0.22.


Both jobs used the same prompt ("A red fox trotting through fresh snow in a birch forest at golden hour, camera tracking low alongside it") at 720p and 5 seconds, on one endpoint and one credit meter. You can run either model locally from the published weights instead.


## What did the runs show?


ltx-2.3 finished between 27 and 43 seconds after job creation. wan-2.2 finished between 232 and 247 seconds. That is a 5.4x gap at the narrowest reading of those windows, roughly 6x at the midpoints.


The outputs differ more than the request did. wan-2.2 returned 1280x720 at 16 fps in a 2.78 MB file; ltx-2.3 returned 1296x720 at 24 fps in an 8.94 MB file. Both jobs cost 240 credits, which works out to 48 credits per second of 720p video on either model. The raw create and status responses, the full run log, and both output clips are in the[public benchmark repo](https://github.com/maniculehq/mh-ltx-vs-wan) .


Local measurements show the same ordering.[A three-reviewer head-to-head on an RTX 4090](https://localaimaster.com/blog/local-ai-video-generation) timed Wan 2.2 14B at 4 minutes 20 seconds per 5-second clip and LTX at 1 minute 30, though its LTX baseline was the older LTX-Video 0.9.5, so the ordering transfers and the exact ratio doesn't. In the same head-to-head's three-reviewer scoring, Wan 2.2 took the highest motion-stability score of the open models, 8.7 against LTX-Video's 7.9.


## What do people running both models say?


People who run both models locally report the same split our runs measured.[One 5090 owner who runs both](https://www.reddit.com/r/comfyui/comments/1thhqdl/wan22_vs_ltx23_which_video_generation_model_do/) : "Does Wan have better motion, anatomy, and quality? Absolutely. But when I can make three and a half videos in LTXV in the time it takes to make one in Wan 2.2 thats a significant advantage." Their timings: 9 minutes for a 10-second 720p Wan clip at FP8, 2.5 minutes for the same clip on LTX at BF16, at 24 fps with sound.


The specific Wan strengths people name:


- Face and body consistency in image-to-video; LTX "really seems to struggle with not totally changing subjects in i2v"
- Anatomy: eyes and eyelids degrade on LTX with multiple people in a scene
- A deeper LoRA and checkpoint ecosystem from Wan's longer run as the community default


The specific LTX strengths:


- Speed, in every account that mentions it
- Synchronized audio in the same pass, which several users call the deciding feature on its own
- Longer single-shot clips: one user renders 20-second full-HD generations with audio, "impossible with wan unless using very complex workflows"
- Low-VRAM viability: a 6 GB user runs 720p 10-second clips on LTX-2.3 where Wan capped them at 480p and 5 seconds


The recurring LTX complaint is prompt adherence: "Prompt adherance is very poor", "just too unpredictable". The fix users converge on is rewriting the prompt with an LLM before generation.


[A second thread](https://www.reddit.com/r/comfyui/comments/1sik9h1/after_a_month_how_is_ltx23_now_compared_to_wan22/) adds the trajectory point: Wan's open releases stopped at 2.2, so its ecosystem is mature but frozen, while LTX keeps getting open releases and new LoRAs. One user with over 2,000 hours on Wan says LTX "has indeed, at this point, replaced WAN for me."


## Which of these models is open source?


For a video model, open weights and open source are different claims. Open weights means the checkpoint is downloadable: you can fetch the model and run it. Open source means the license lets you use the weights, modify them, and ship a commercial product on them without a discretionary approval, a territory restriction, or a revenue trigger.


Apache 2.0 clears that bar. A community license with a $10 million revenue threshold or a jurisdiction exclusion does not. By that test, of the leading open-weights families (Wan, Hunyuan, LTX), only Wan 2.2 is open source.


Model


License


Open source by this test


The catch


Wan 2.2


[Apache 2.0](https://github.com/Wan-Video/Wan2.2)


Yes


None; no rights claimed over outputs


LTX-2 / LTX-2.3


[LTX-2 Community License](https://github.com/Lightricks/LTX-2/blob/main/LICENSE)


No


Paid agreement required at $10M annual revenue


HunyuanVideo


[Custom Tencent license](https://github.com/Tencent-Hunyuan/HunyuanVideo/blob/main/LICENSE.txt)


No


Inapplicable in the EU, UK, and South Korea


HunyuanVideo-1.5


[None declared](https://github.com/Tencent-Hunyuan/HunyuanVideo-1.5)


No


Repo license field reads NOASSERTION


Mochi 1


[Apache 2.0](https://github.com/genmoai/mochi)


Yes


None


CogVideoX


[2B Apache 2.0, 5B custom](https://github.com/zai-org/CogVideo)


2B yes, 5B no


5B models sit under the CogVideoX License


Open-Sora 2.0


[Apache 2.0](https://github.com/hpcaitech/Open-Sora)


Yes


None


Wan 2.2's license is the simple one: Apache 2.0, and the README adds that Alibaba claims no rights over generated content.


The LTX-2 Community License requires "Commercial Entities", defined as those with annual revenues of at least $10,000,000, to obtain a paid commercial license from Lightricks, and calls commercial use outside that agreement "strictly prohibited". Lightricks' own FAQ describes LTX-2.3 as "free and open source to use under its license"; the license text above is the binding document.


## What hardware does each open model need?


Three open families matter as of July 2026: Wan 2.2, LTX-2/2.3, and HunyuanVideo-1.5. Mochi 1, CogVideoX, and Open-Sora came before. Vendors rarely state hardware requirements upfront, so here they are per model:


Model


Size


VRAM to run it


Wan 2.2 A14B


27B MoE, 14B active


6-8 GB quantized; 54-65 GB FP16


Wan 2.2 TI2V-5B


5B


24 GB


Wan 2.1 T2V-1.3B


1.3B


8.19 GB


LTX-2.3


22B


A single RTX 5090


HunyuanVideo-1.5


8.3B


14 GB with offloading


Mochi 1


10B


60 GB; under 20 GB via ComfyUI


Open-Sora 2.0


11B


60.3 GB on one H100


Wan 2.2's 14B model is a two-expert mixture-of-experts: 27B parameters total, 14B active per step. It runs in 6 to 8 GB with GGUF quantization and the T5 text encoder offloaded to CPU, needs roughly 54 to 65 GB at FP16, and[the repo's own single-GPU command](https://github.com/Wan-Video/Wan2.2) asks for at least 80 GB. The T5-XXL text encoder, 9.4B parameters on its own, dominates the VRAM budget.


The smaller Wan TI2V-5B generates 720p at 24 fps on a 24 GB RTX 4090, a 5-second clip in under 9 minutes. Wan 2.1's 1.3B model, at 8.19 GB, is still the lowest-VRAM option in the family.


LTX-2.3 is a 22B model that generates synchronized video and audio in one pass, shipped as a full dev checkpoint and an 8-step distilled one, with[just over 2 million Hugging Face downloads last month](https://huggingface.co/Lightricks/LTX-2.3) . The base LTX-2 is a 19B variant with fp8 and nvfp4 checkpoints. Lightricks says it runs locally on a single RTX 5090.


HunyuanVideo-1.5 is the lightest of the three current families: 8.3B parameters, a 14 GB minimum with model offloading enabled, and a 75-second 480p generation on a single RTX 4090 with the step-distilled checkpoint.


Three cautions before you download anything:


- Pages offering "Wan 2.7 open weights" are[SEO fabrications](https://localaimaster.com/blog/local-ai-video-generation) ; official Wan open weights stop at 2.2.
- Comfortable VRAM sits higher than the marketing suggests:["all the various video models really want an 80+ gig vram card, to run comfortably"](https://news.ycombinator.com/item?id=44928997) . The low-VRAM figures come from community projects like[Wan2GP](https://github.com/deepbeepmeep/Wan2GP) , which gets Wan running on as little as 6 GB.
- [AMD owners report ROCm video generation as buggy and OOM-prone](https://news.ycombinator.com/item?id=43944974) , so "runs locally" means NVIDIA in practice.


## What is the easiest way to run LTX-2.3 and Wan 2.2?


Through a unified API.[Magic Hour's video API](https://docs.magichour.ai/api-reference/video-projects/text-to-video) serves both open families and its closed-model catalog (Kling, Veo, Seedance, Sora) through one endpoint, one request shape, and one credit balance, with no GPU on your side. Switching models is one line: the request body's` model` field.


Our two runs above worked exactly this way, differing only in that field. The full model list, from the OpenAPI contract:


` "model": { "default": "default", "type": "string", "enum": \[ "default", "ltx-2", "ltx-2.3", "wan-2.2", "seedance-1.5", "seedance-2.0", "seedance-2.0-mini", "kling-2.5", "kling-3.0", "veo3.1", "veo3.1-lite", "sora-2", "kling-1.6", "seedance", "kling-2.5-audio", "veo3.1-audio" \],`


The[image-to-video](https://magichour.ai/products/image-to-video) endpoint exposes the identical model list, so the same choice carries over when you animate a still image instead of a prompt.


The models differ in three parameters: resolution, duration, and audio.


Model


Weights


Max resolution


Max duration per job


Audio


ltx-2.3


Open


1080p


30s


Yes, no extra credits


wan-2.2


Open


1080p


15s


No


kling-2.5


Closed


1080p


10s


Yes, no extra credits


kling-3.0


Closed


4k


15s


Yes, extra credits


veo3.1


Closed


1080p


56s


Yes, extra credits


veo3.1-lite


Closed


1080p


56s


Yes, extra credits


seedance-1.5


Closed


1080p


12s


Yes, extra credits


seedance-2.0


Closed


720p


15s


Yes, no extra credits


seedance-2.0-mini


Closed


720p


15s


Yes, no extra credits


sora-2


Closed


720p


60s


Yes, no extra credits


Both open models reach 1080p; only kling-3.0 reaches 4k. ltx-2.3 runs jobs up to 30 seconds where wan-2.2 stops at 15, and wan-2.2 is the only model in the list that generates no audio.


Every model runs the same async flow: create a job, poll its status until it reads` complete` , then download the MP4 from a signed URL. Credits are charged only for the frames that render, and a failed generation is refunded automatically.


Left at` default` , the` model` field resolves to kling-3.0 on paid tiers and ltx-2.3 on free tiers. Name the model explicitly if the choice matters to you. The API's own per-model guidance matches this piece's split: "Fastest output. Best for rapid iteration." for ltx-2.3, "Strong physics, camera moves, and motion." for wan-2.2.


## What does the hosted path not get you?


The hosted path trades control for zero hardware. The API serves two open families and stops there: HunyuanVideo, Mochi, CogVideoX, and Open-Sora are not served, and neither is anything past Wan 2.2.


Nothing under the models is exposed either:


- No checkpoints, LoRAs, or custom weights
- No quantization choices, step counts, or seeds
- The models' own limits still apply: wan-2.2 generates no audio through the API either


Three pricing tiers cover the hosted path:


- Free: 400 credits plus 100 per day, capped at 576px with watermarked outputs, enough to check output style but not to produce.
- Credit packs: $3.00 per 1,000 credits, self-serve. The measured $0.72 per clip uses this rate.
- Usage-based: about $0.22 for the same 5-second clip, but it requires setup assistance and is not yet self-serve.


> The code **FIRSTAPI** takes 10% off your first API credit purchase.[Sign up here](https://manicule.link/mh-ltxwan) .


Self-host instead if you need LoRA fine-tuning, one of the unserved models, or parameter-level control. The hardware for that path is in the VRAM table above. For the broader hosted-tool market, see our guide to[AI video generation tools for creators and studios](https://magichour.ai/blog/ai-video-generation-tools-for-creators-and-studios) .


## So which model should you build on?


For building a product, Wan 2.2. For iteration speed, clips past 15 seconds, or built-in audio, LTX-2.3, if you are under its $10 million revenue threshold or willing to sign Lightricks' commercial agreement. For 4k output or the top of cinematic quality, you still need a closed model.


The case for Wan 2.2 is the license and the motion. Apache 2.0 means no territory exclusions, no revenue triggers, no approval clauses, and no claims on your outputs. The 4090 head-to-head scored it highest of the open models on motion stability. And it runs on the widest range of hardware of the three families, from 80 GB datacenter cards down to 6 GB consumer ones.


The case for LTX-2.3 is throughput and trajectory. It returned the test clip roughly six times faster at the same credit cost, runs jobs to 30 seconds where wan-2.2 stops at 15, and adds synchronized audio at no extra credits. And it is the family still getting open releases: Wan's open weights stop at 2.2, so the gap practitioners report on quality is one LTX can still close. Check the license threshold first.


When neither fits, use a closed model: kling-3.0 is the only model in the list with 4k output. For where the closed models are headed, see[The state of AI in video and image generation](https://magichour.ai/blog/the-state-of-ai-in-video-and-image-generation) .


## Frequently asked questions


### Is LTX-2.3 better than Wan 2.2?


Neither dominates. Wan 2.2 produces better motion, anatomy, and face consistency; LTX-2.3 generates roughly six times faster, adds synchronized audio, and runs 30-second clips where Wan stops at 15. Practitioners who run both typically produce on LTX-2.3 and switch to Wan 2.2 when realism or subject consistency decides the job.


### What are the best open source video models?


Wan 2.2 and LTX-2.3, with HunyuanVideo-1.5 behind them. Wan 2.2 is the strongest open model to build a product on: Apache 2.0, with the best measured motion. LTX-2.3 is the fastest: it returned a 5-second 720p test clip in under 45 seconds, roughly six times faster than wan-2.2 at the same credit cost.


### What is the best open source AI video generator?


Wan 2.2, if open source means a license you can ship a commercial product under: it is Apache 2.0, and Alibaba claims no rights over outputs. If open weights is enough, LTX-2.3 generates faster and adds audio, but companies with $10 million or more in annual revenue need a paid agreement with Lightricks.


### Is LTX-2 open source?


No. LTX-2 and LTX-2.3 are open weights under the LTX-2 Community License, which requires companies with at least $10 million in annual revenue to obtain a paid commercial license. Anyone under that threshold can use it commercially at no cost under the license's terms.


### What VRAM does Wan 2.2 need?


From 6 to 8 GB for the quantized 14B model with the text encoder offloaded, 54 to 65 GB at FP16, and at least 80 GB for the repo's official single-GPU command. The 5B version runs 720p at 24 fps on a 24 GB RTX 4090.


### Can I run open video models without a GPU?


Yes, through a hosted API. Magic Hour serves the wan-2.2 and ltx-2.3 model families through the same endpoint as its closed models; the measured cost was 240 credits per 5-second 720p clip on either, $0.72 at credit-pack rates.
