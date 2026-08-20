---
schema_version: "1.0.0"
document_id: "f0e14cbfc04188c70ceebb26e6eb0c9544affd4a283dd23cc5714e49618a8c34"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/vlm-survey-2026"
published_at: "2026-03-01T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:6938bfc7ab83d4c9477a70823909b312e8c0681f11db341b89c0789d6ecede38"
---

# Open Source Vision Language Models (2026)

[← Back to all posts](https://www.overshoot.ai/blogs)


How much of your time do you spend with your eyes open? The question seems ridiculous, we do not even think of seeing as an activity. In any given day, we will decide to say many things, and read quite a few, but through it all we are constantly watching the world around us.


This ratio is inverted for an LLM, the vast majority of an LLM’s training data and interactions are text based, with the occasional image or video thrown in. This is not because interacting with the world through text is superior, but because text was the easiest avenue for general AI to enter into the world. Much like the newspaper dominated before the advent of the Television, we at Overshoot believe this is the age of text, to be followed by the age of vision.


At this stage, vision in LLMs is added on after the fact. Images and videos are pushed through vision encoders, converted to tokens that neatly fit alongside text input tokens for an LLM’s ingestion. This approach (primitive though it may be) is generating impressive results for visual understanding and is changing the computer vision landscape for security, home monitoring, blind assistance, robotics, and many more.


Some of the best vision models available are open source models, with the Qwen3.5 and Qwen3-VL families at the forefront of model performance and latency. Since summer 2025, the range of possible applications has expanded to include realtime general vision applications. With response times as low as 200ms, rivaling human reaction times (try it for yourself in[playground.overshoot.ai](https://playground.overshoot.ai/) ).


The following is a list of the most important open source video language models as of March 2026. We aim to encompass all significant vision language models that are published later than December 2024 and which support text, image, and importantly video input.


For step-by-step deployment guides for these models, see our[Inference Field Guide](https://www.overshoot.ai/blogs/deploying-vlms-through-vllm) .


## Qwen3.5 Family[Apache-2.0](https://github.com/QwenLM/Qwen3.5/blob/main/LICENSE)


The Qwen3.5 family is the first major model release where every model is natively multimodal via early fusion training. There is no separate “-VL” variant. Every model handles text, images, and video from a single set of weights. See our[detailed breakdown](https://www.overshoot.ai/blogs/qwen3.5-on-overshoot) for benchmarks and model selection guidance.


Model Params Context Date vLLM SGLang Downloads


[Qwen3.5-397B-A17B](https://huggingface.co/Qwen/Qwen3.5-397B-A17B) 397B (17B active) 256K, up to 1M 2026-02-16 nightly


main


1.3M


[Qwen3.5-122B-A10B](https://huggingface.co/Qwen/Qwen3.5-122B-A10B) 122B (10B active) 256K, up to 1M 2026-02-24 nightly


main


171K


[Qwen3.5-35B-A3B](https://huggingface.co/Qwen/Qwen3.5-35B-A3B) 35B (3B active) 256K, up to 1M 2026-02-24 nightly


main


769K


[Qwen3.5-27B](https://huggingface.co/Qwen/Qwen3.5-27B) 27B 256K, up to 1M 2026-02-24 nightly


main


407K


[Qwen3.5-9B](https://huggingface.co/Qwen/Qwen3.5-9B) 9.65B 256K, up to 1M 2026-03-02 nightly


main


172K


[Qwen3.5-4B](https://huggingface.co/Qwen/Qwen3.5-4B) 4.66B 256K, up to 1M 2026-03-02 nightly


main


99K


[Qwen3.5-2B](https://huggingface.co/Qwen/Qwen3.5-2B) 2.27B 256K, up to 1M 2026-03-02 nightly


main


47K


## Qwen3-VL Family[Apache-2.0](https://github.com/QwenLM/Qwen3-VL/blob/main/LICENSE)


Model Params Context Date vLLM SGLang Downloads


[Qwen3-VL-32B-Instruct](https://huggingface.co/Qwen/Qwen3-VL-32B-Instruct) 33B 262K, up to 1M 2025-10-21 ✓


✓


448K


[Qwen3-VL-30B-A3B-Instruct](https://huggingface.co/Qwen/Qwen3-VL-30B-A3B-Instruct) 31B 262K, up to 1M 2025-10-04 ✓


✓


858K


[Qwen3-VL-8B-Instruct](https://huggingface.co/Qwen/Qwen3-VL-8B-Instruct) 9B 262K, up to 1M 2025-10-15 ✓


✓


2.2M


[Qwen3-VL-4B-Instruct](https://huggingface.co/Qwen/Qwen3-VL-4B-Instruct) 4B 262K, up to 1M 2025-10-15 ✓


✓


643K


[Qwen3-VL-2B-Instruct](https://huggingface.co/Qwen/Qwen3-VL-2B-Instruct) 2B 262K, up to 1M 2025-10-21 ✓


✓


555K


## InternVL3.5 Family[Apache-2.0](https://github.com/OpenGVLab/InternVL/blob/main/LICENSE)


Model Params Context Date vLLM SGLang Downloads


[InternVL3_5-38B](https://huggingface.co/OpenGVLab/InternVL3_5-38B) 38B 32K (SFT) 2025-08-26 ✓


?


80K


[InternVL3_5-30B-A3B](https://huggingface.co/OpenGVLab/InternVL3_5-30B-A3B) 31B 32K (SFT) 2025-08-26 ✓


?


2.8K


[InternVL3_5-GPT-OSS-20B-A4B-Preview](https://huggingface.co/OpenGVLab/InternVL3_5-GPT-OSS-20B-A4B-Preview) ? 32K (SFT) 2025-08-26 ✓


?


34K


[InternVL3_5-14B](https://huggingface.co/OpenGVLab/InternVL3_5-14B) 15B 32K (SFT) 2025-08-26 ✓


?


6.8K


[InternVL3_5-8B](https://huggingface.co/OpenGVLab/InternVL3_5-8B) 9B 32K (SFT) 2025-08-26 ✓


?


34K


[InternVL3_5-4B](https://huggingface.co/OpenGVLab/InternVL3_5-4B) 5B 32K (SFT) 2025-08-26 ✓


?


44K


[InternVL3_5-2B](https://huggingface.co/OpenGVLab/InternVL3_5-2B) 2B 32K (SFT) 2025-08-26 ✓


?


28K


[InternVL3_5-1B](https://huggingface.co/OpenGVLab/InternVL3_5-1B) 1B 32K (SFT) 2025-08-26 ✓


?


41K


## InternVL3.5-Flash Family[Apache-2.0](https://github.com/OpenGVLab/InternVL/blob/main/LICENSE)


Model Params Context Date vLLM SGLang Downloads


[InternVL3_5-38B-Flash](https://huggingface.co/OpenGVLab/InternVL3_5-38B-Flash) 40B 32K (SFT) 2025-08-26 ?


?


254


[InternVL3_5-30B-A3B-Flash](https://huggingface.co/OpenGVLab/InternVL3_5-30B-A3B-Flash) 31B 32K (SFT) 2025-08-26 ?


?


171


[InternVL3_5-14B-Flash](https://huggingface.co/OpenGVLab/InternVL3_5-14B-Flash) 15B 32K (SFT) 2025-08-26 ?


?


5.8K


[InternVL3_5-8B-Flash](https://huggingface.co/OpenGVLab/InternVL3_5-8B-Flash) 9B 32K (SFT) 2025-08-26 ?


?


659


[InternVL3_5-4B-Flash](https://huggingface.co/OpenGVLab/InternVL3_5-4B-Flash) 5B 32K (SFT) 2025-08-26 ?


?


320


[InternVL3_5-2B-Flash](https://huggingface.co/OpenGVLab/InternVL3_5-2B-Flash) 2B 32K (SFT) 2025-08-26 ?


?


271


[InternVL3_5-1B-Flash](https://huggingface.co/OpenGVLab/InternVL3_5-1B-Flash) 1B 32K (SFT) 2025-08-26 ?


?


2.0K


## Qwen2.5-VL Family


Model Params Context Date vLLM SGLang Downloads


[Qwen2.5-VL-72B-Instruct](https://huggingface.co/Qwen/Qwen2.5-VL-72B-Instruct) 73B 32,768 2025-01-28 ✓


✓


125K


[Qwen2.5-VL-32B-Instruct](https://huggingface.co/Qwen/Qwen2.5-VL-32B-Instruct) 33B 32,768 2025-03-25 ✓


✓


2.3M


[Qwen2.5-VL-7B-Instruct](https://huggingface.co/Qwen/Qwen2.5-VL-7B-Instruct) 8B 32,768 2025-01-28 ✓


✓


3.1M


[Qwen2.5-VL-3B-Instruct](https://huggingface.co/Qwen/Qwen2.5-VL-3B-Instruct) 4B 32,768 2025-01-28 ✓


✓


21.6M


## InternVL3 Family


Model Params Context Date vLLM SGLang Downloads


[InternVL3-78B](https://huggingface.co/OpenGVLab/InternVL3-78B) 78B 32,768 2025-04-14 ✓


✓


2.8M


[InternVL3-38B](https://huggingface.co/OpenGVLab/InternVL3-38B) 38B 32,768 2025-04-14 ✓


✓


3.2K


[InternVL3-14B](https://huggingface.co/OpenGVLab/InternVL3-14B) 15B 32,768 2025-04-14 ✓


✓


21K


[InternVL3-9B](https://huggingface.co/OpenGVLab/InternVL3-9B) 9B 32,768 2025-04-14 ✓


✓


2.0K


[InternVL3-8B](https://huggingface.co/OpenGVLab/InternVL3-8B) 8B 32,768 2025-04-14 ✓


✓


140K


[InternVL3-2B](https://huggingface.co/OpenGVLab/InternVL3-2B) 2B 32,768 2025-04-14 ✓


✓


40K


[InternVL3-1B](https://huggingface.co/OpenGVLab/InternVL3-1B) 0.9B 32,768 2025-04-14 ✓


✓


133K


## GLM-4.6V Family[MIT](https://github.com/zai-org/GLM-V/blob/main/LICENSE)


Model Params Context Date vLLM SGLang Downloads


[GLM-4.6V](https://huggingface.co/zai-org/GLM-4.6V) 106B 128K 2025-07-01 ✓


✓


54K


[GLM-4.6V-Flash](https://huggingface.co/zai-org/GLM-4.6V-Flash) 9B 128K 2025-07-01 ✓


✓


70K


## GLM-4.5V Family[MIT](https://github.com/zai-org/GLM-V/blob/main/LICENSE)


Model Params Context Date vLLM SGLang Downloads


[GLM-4.5V](https://huggingface.co/zai-org/GLM-4.5V) 108B (12B active) 64K 2025-07-01 ✓


✓


31K


## Kimi-VL-A3B Family[MIT](https://github.com/MoonshotAI/Kimi-VL/blob/main/LICENSE)


Model Params Context Date vLLM SGLang Downloads Notes


[Kimi-VL-A3B-Thinking](https://huggingface.co/moonshotai/Kimi-VL-A3B-Thinking) 16B (3B active) 128K 2025-04-10 ?


?


70K Fine-tuned for advanced reasoning


[Kimi-VL-A3B-Instruct](https://huggingface.co/moonshotai/Kimi-VL-A3B-Instruct) 16B (3.2B active) 128K 2025-04-10 ?


?


174K


## Molmo2 Family[Apache-2.0](https://github.com/allenai/molmo2/blob/main/LICENSE)


Model Params Context Date vLLM SGLang Downloads Notes


[Molmo2-8B](https://huggingface.co/allenai/Molmo2-8B) 9B 36,864 2026-01-15 ✓


?


77K


[Molmo2-O-7B](https://huggingface.co/allenai/Molmo2-O-7B) 7B 65,536 (YaRN) 2026-01-15 ✓


?


23K


[Molmo2-4B](https://huggingface.co/allenai/Molmo2-4B) 5B 36,864 2026-01-15 ✓


?


33K


[Molmo2-VideoPoint-4B](https://huggingface.co/allenai/Molmo2-VideoPoint-4B) 5B 30,000 2026-01-15 ✓


?


99 Video pointing/counting


## MiniCPM-V Family[Apache-2.0](https://github.com/OpenBMB/MiniCPM-o/blob/main/LICENSE)


Model Params Context Date vLLM SGLang Downloads


[MiniCPM-V-4_5](https://huggingface.co/openbmb/MiniCPM-V-4_5) 8.7B 40,960 2025-09-16 ✓


?


68K


## Keye-VL Family[Apache-2.0](https://huggingface.co/Kwai-Keye/Keye-VL-1_5-8B)


Model Params Context Date vLLM SGLang Downloads


[Keye-VL-1_5-8B](https://huggingface.co/Kwai-Keye/Keye-VL-1_5-8B) 9B 128K 2025-08-28 ✓


?


42K


[Keye-VL-8B-Preview](https://huggingface.co/Kwai-Keye/Keye-VL-8B-Preview) 8B 40,960 (mRoPE) 2025-06-26 ?


?


48K


## Tarsier2 Family[Apache-2.0](https://github.com/bytedance/tarsier/blob/main/LICENSE)


Model Params Context Date vLLM SGLang Downloads Notes


[Tarsier2-Recap-7b](https://huggingface.co/omni-research/Tarsier2-Recap-7b) 8B 32,768 (mRoPE) 2024-12 ?


?


8.8M Video description via distillation


## Legend


**vLLM / SGLang:** *✓* = supported in stable, *nightly* / *main* = requires nightly or main branch, *?* = unknown or not documented.


**Downloads:** Last-month downloads from HuggingFace. **Context:** Token context window size. **Params:** Parameter count (active parameters shown for MoE models).
