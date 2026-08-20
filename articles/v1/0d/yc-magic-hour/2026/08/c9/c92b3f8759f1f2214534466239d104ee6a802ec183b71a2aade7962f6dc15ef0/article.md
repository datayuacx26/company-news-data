---
schema_version: "1.0.0"
document_id: "c92b3f8759f1f2214534466239d104ee6a802ec183b71a2aade7962f6dc15ef0"
company_key: "yc-magic-hour"
company: "Magic Hour"
source_id: "yc-magic-hour-news-import-988efd6b5de7"
canonical_url: "https://magichour.ai/blog/open-source-image-generation-models"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T20:11:27.446895+00:00"
fetched_at: "2026-08-05T20:11:29.420770+00:00"
content_hash: "sha256:655e2757a5f872c7908be0dd327869f628a78d457913f86e5ede55bdc74e993e"
---

# 29 Open-Source Image Generation Model Families to Run Locally in 2026

The best local image model is the one whose capabilities, checkpoint terms, and hardware requirements fit your workflow. Treat Base, Turbo, Edit, Dev, quantized, and revision checkpoints as choices within a family rather than separate models.


This guide compares 29 families with downloadable weights and a publisher-documented local inference path: FLUX.1, FLUX.2, Z-Image, Stable Diffusion, Qwen-Image, HunyuanImage 3.0, GLM-Image, ERNIE-Image, HiDream-O1-Image, DeepGen 1.0, JoyAI-Image, Krea 2, Ideogram 4, Boogu-Image-0.1, Cosmos 3, LongCat-Image, FIBO, Infinity, Lumina-Image 2.0, Sana, OmniGen2, BAGEL, CogView4, Kandinsky 5, F-Lite, Janus-Pro, Kolors, AuraFlow, and PixArt-Σ.


## What should you compare open-weight image models on?


- **Capabilities:** Match the checkpoint to text-to-image generation, image editing, text rendering, or multi-reference personalization.


- **Released checkpoint:** Confirm that the exact weights are downloadable. An announcement or hosted variant cannot stand in for a local release.


- **Benchmark evidence:** Compare exact checkpoints under the same protocol.[Artificial Analysis](https://artificialanalysis.ai/image/methodology) derives Quality Elo from user votes on serverless API outputs, while[ImageBench V1](https://imagebench.ai/blog/benchmark-v1-methodology) uses 192 prompts and publishes local runs. Neither score is a substitute for testing your prompts on your hardware.


- **Parameter count:** Use model size as a planning input, then size the real runtime from published VRAM, precision, resolution, offload, and quantization requirements.


- **Speed:** Keep step count and latency attached to the tested configuration. For example, Z-Image Turbo’s[sub-second claim uses H800 GPUs, while its 16GB figure describes consumer-device fit](https://github.com/Tongyi-MAI/Z-Image) .


- **Terms:** Read the license attached to the exact checkpoint. Code, weights, gates, commercial rights, and territory limits can differ even within one family.


- **Local runner:** Prefer a publisher-supported route such as Hugging Face Diffusers, an official PyTorch repository, SGLang, vLLM-Omni, or ComfyUI.


This roster consolidates related checkpoints under their upstream family and excludes fine-tunes, repackages, quantized mirrors, UI layers, and hosted-only systems. License terms can change, so verify the exact checkpoint’s current license before commercial use.


## Quick comparison


Family


Recommended for


Variants


How to run it locally


Recommended Hardware


[FLUX.1](https://github.com/black-forest-labs/flux)


General generation and Kontext editing · hosted in[Magic Hour](https://magichour.ai/products/ai-image-generator)


schnell, dev, Kontext dev


Official PyTorch code, Diffusers, ComfyUI


No publisher minimum found


[FLUX.2](https://github.com/black-forest-labs/flux2)


Generation and editing on consumer GPUs · hosted in[Magic Hour](https://magichour.ai/products/ai-image-generator)


klein 4B and 9B; Base variants; dev


Official PyTorch code and Diffusers


[klein 4B: approximately 13GB VRAM](https://huggingface.co/black-forest-labs/FLUX.2-klein-4B)


[Z-Image](https://github.com/Tongyi-MAI/Z-Image)


Fast local photorealism · hosted in[Magic Hour](https://magichour.ai/products/ai-image-generator)


Base, Turbo


Diffusers and ComfyUI


[Turbo: 16GB consumer VRAM](https://github.com/Tongyi-MAI/Z-Image)


[Stable Diffusion](https://github.com/Stability-AI/sd3.5)


Mature ecosystem and broad style work


SDXL Base/Refiner; SD3.5 Large, Turbo, Medium


Diffusers, official PyTorch repos, ComfyUI


[SD3.5 Medium: 9.9GB excluding text encoders](https://stability.ai/news/introducing-stable-diffusion-3-5) ;[SDXL Base test: 11.47GB–28.09GB](https://huggingface.co/blog/simple_sdxl_optimizations)


[Qwen-Image](https://github.com/QwenLM/Qwen-Image)


Text rendering and precise editing


Qwen-Image, 2512, Edit-2511, Layered


Diffusers and ComfyUI


No publisher minimum found


[HunyuanImage 3.0](https://github.com/Tencent-Hunyuan/HunyuanImage-3.0)


Knowledge-rich, large-scale generation


Base, Instruct, Distil


Official PyTorch repo


[Base: at least 3×80GB; Instruct: at least 8×80GB](https://github.com/Tencent-Hunyuan/HunyuanImage-3.0)


[GLM-Image](https://github.com/zai-org/GLM-Image)


Text-heavy and knowledge-intensive images


GLM-Image


Diffusers or SGLang


[More than 80GB on one GPU or multiple GPUs](https://github.com/zai-org/GLM-Image)


[ERNIE-Image](https://github.com/baidu/ERNIE-Image)


Standard and fast bilingual generation


Standard, Turbo


Diffusers


[24GB consumer GPU](https://github.com/baidu/ERNIE-Image)


[HiDream-O1-Image](https://github.com/HiDream-ai/HiDream-O1-Image)


Generation, editing, and references


Full, Dev, Dev-2604


Official PyTorch repo


[CUDA GPU; no publisher minimum found](https://github.com/HiDream-ai/HiDream-O1-Image)


[DeepGen 1.0](https://github.com/DeepGenTeam/DeepGen)


Compact unified generation and editing


SFT, RL


Diffusers


No publisher minimum found


[JoyAI-Image](https://github.com/jd-opensource/JoyAI-Image)


Spatial and camera-aware editing


Edit, Edit-Plus


Official PyTorch repo and ComfyUI


[CUDA GPU; no publisher minimum found](https://github.com/jd-opensource/JoyAI-Image)


[Krea 2](https://github.com/krea-ai/krea-2)


Illustration and varied styles


Raw, Turbo


Official PyTorch repo and ComfyUI


No publisher minimum found


[Ideogram 4](https://github.com/ideogram-oss/ideogram4)


Typography and structured design


9.3B nf4, fp8


Official PyTorch repo and ComfyUI


No publisher minimum found


[Boogu-Image-0.1](https://github.com/boogu-project/Boogu-Image)


Generation and editing at several memory tiers


Base, Turbo, Edit, Edit-Turbo


Official PyTorch repo and ComfyUI


[12GB–80GB, depending on configuration](https://huggingface.co/Boogu/Boogu-Image-0.1-Base)


[Cosmos 3](https://github.com/NVIDIA/cosmos)


Frontier-scale image and world generation


Super-Text2Image, four-step variant


Diffusers, vLLM-Omni, SGLang


[Multi-GPU data-center class](https://github.com/NVIDIA/cosmos)


[LongCat-Image](https://github.com/meituan-longcat/LongCat-Image)


Bilingual generation, text, and editing


Base, Dev, Edit, Edit-Turbo


Diffusers and ComfyUI


[About 17GB with CPU offload](https://github.com/meituan-longcat/LongCat-Image)


[FIBO](https://huggingface.co/briaai/FIBO)


Structured, repeatable art direction


FIBO generation and editing releases


Diffusers and ComfyUI


No publisher minimum found


[Infinity](https://github.com/FoundationVision/Infinity)


Fast autoregressive image generation


2B, 8B


Official PyTorch repo and Docker


No publisher minimum found


[Lumina-Image 2.0](https://github.com/Alpha-VLLM/Lumina-Image-2.0)


Efficient 1024px generation


2.6B checkpoint


Diffusers, official PyTorch repo, ComfyUI


CPU offload supported; no minimum found


[Sana](https://github.com/NVlabs/Sana)


Efficient high-resolution generation


0.6B, 1.6B, 4.8B, Sprint


Diffusers, SGLang, ComfyUI


[Quantized 4K path within about 8GB](https://github.com/NVlabs/Sana)


[OmniGen2](https://github.com/VectorSpaceLab/OmniGen2)


Generation, editing, and in-context composition


OmniGen2


Official PyTorch repo and ComfyUI


[About 17GB native; offload options below that](https://github.com/VectorSpaceLab/OmniGen2)


[BAGEL](https://github.com/bytedance-seed/BAGEL)


Unified generation, editing, and understanding


7B-active/14B-total MoT


Official PyTorch repo


[NF4 path for 12GB–32GB](https://github.com/bytedance-seed/BAGEL)


[CogView4](https://github.com/zai-org/CogView4)


Chinese/English text and high-resolution generation


CogView4-6B


Diffusers and CogKit


[13GB–35GB at 1024px, depending on offload](https://github.com/zai-org/CogView4)


[Kandinsky 5](https://github.com/kandinskylab/kandinsky-5)


Russian/English generation and editing


T2I Lite, Image Editing


Official PyTorch repo and Diffusers


No publisher minimum found


[F-Lite](https://github.com/fal-ai/f-lite)


Copyright-safe, SFW generation


Standard, Texture, 7B


Diffusers and ComfyUI


[At least 24GB VRAM](https://github.com/fal-ai/f-lite)


[Janus-Pro](https://github.com/deepseek-ai/Janus)


Unified understanding and compact generation


1B, 7B


Official PyTorch repo and ComfyUI


No publisher minimum found


[Kolors](https://github.com/Kwai-Kolors/Kolors)


Chinese/English generation and portraits


Base, IP-Adapter, ControlNet, inpainting


Diffusers and ComfyUI


No publisher minimum found


[AuraFlow](https://huggingface.co/fal/AuraFlow)


Literal prompt following under Apache 2.0


v0.1–v0.3


Diffusers and ComfyUI


No publisher minimum found


[PixArt-Σ](https://github.com/PixArt-alpha/PixArt-sigma)


Efficient high-resolution generation


512px, 1024px, 2K


Diffusers and official PyTorch repo


No publisher minimum found


## [FLUX.1](https://github.com/black-forest-labs/flux)


Black Forest Labs’ FLUX.1 is a 12B family spanning fast generation with schnell, higher-quality generation with dev, and editing through Kontext. It remains a strong local baseline for literal prompt following and complex scenes, but practitioners also describe a[narrow stylistic range](https://news.ycombinator.com/item?id=41740100) , recurring faces, and overly smooth photographic skin; schnell trades more detail for speed.


- **Benchmark:** As checked July 30, 2026, the[Artificial Analysis open-weights arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded FLUX.1 dev at Elo 1,029 and rank 32, and schnell at Elo 1,000 and rank 35. The[KRIS-Bench editing leaderboard](https://yongliang-wu.github.io/kris_bench_project_page/) scores Kontext dev at 49.54 overall.


- **Size and hardware:**[Schnell is a 12B model](https://huggingface.co/black-forest-labs/FLUX.1-schnell) ; the publisher gives no universal minimum VRAM figure.


- **Run locally:** Use the[official PyTorch repository, Hugging Face Diffusers, or ComfyUI](https://github.com/black-forest-labs/flux) .


- **Access and terms:**[Schnell uses Apache 2.0 and permits commercial use; dev uses the FLUX.1 dev Non-Commercial License](https://github.com/black-forest-labs/flux) . Kontext dev uses the same non-commercial family of terms.


Choose schnell when permissive commercial terms and speed matter. Evaluate dev or Kontext when their quality or editing capabilities justify the extra restrictions.


### Use it on MagicHour


To use FLUX.1 without a local GPU, select` flux-schnell` in[Magic Hour’s AI Image Generator](https://magichour.ai/products/ai-image-generator) or pass` model="flux-schnell"` to[POST /v1/ai-image-generator](https://magichour.ai/api-reference/image-projects/ai-image-generator) .


It is available on every tier, including free, from 5 credits per image at 640px, 1K, or 2K, with 1–4 images per job.


**If you're new to MagicHour API, use coupon FIRSTAPI to get 10% off.**[Sign up here](https://manicule.link/mh-os-image-models) **.**


## [FLUX.2](https://github.com/black-forest-labs/flux2)


FLUX.2 combines generation and editing, with klein aimed at local use. Early users praise its speed, positional prompting, style range, and edit mode, while[4B testers report unwanted changes and inconsistent anatomy or text](https://www.reddit.com/r/StableDiffusion/comments/1rkkm04/is_flux_klein_4b_supposed_to_be_this_badly_broken/) .


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded klein 4B at Elo 1,057 and rank 26, and klein Base 4B at Elo 968 and rank 37 on July 30, 2026. These are not local-hardware runs.


- **Size and hardware:**[Klein 4B has 4B parameters and a publisher-stated footprint of approximately 13GB VRAM](https://huggingface.co/black-forest-labs/FLUX.2-klein-4B) .


- **Run locally:** Use[BFL’s official PyTorch code or Diffusers; ComfyUI supports the family](https://github.com/black-forest-labs/flux2) .


- **Access and terms:**[Klein 4B and its Base checkpoint are Apache 2.0](https://github.com/black-forest-labs/flux2) . Check dev-family terms separately.


### Use it on MagicHour


To use FLUX.2 without a local GPU, select` flux-2-klein` in[Magic Hour’s AI Image Generator](https://magichour.ai/products/ai-image-generator) or pass` model="flux-2-klein"` to[POST /v1/ai-image-generator](https://magichour.ai/api-reference/image-projects/ai-image-generator) .


It is available on every tier, including free, from 5 credits per image at 640px, 1K, or 2K, with one image per job. Pro, Flex, and Max are hosted offerings rather than additional local families.


## [Z-Image](https://github.com/Tongyi-MAI/Z-Image)


Tongyi-MAI’s Z-Image is a compact 6B family whose Turbo checkpoint is popular for fast, crisp photorealism on modest hardware. Users praise its quality-to-speed ratio but report[limited seed diversity and weaker handling of multi-person scenes, left-versus-right instructions, and uncommon objects](https://www.reddit.com/r/StableDiffusion/comments/1setpkv/the_z_image_turbo_seems_to_be_perfect/) . Base is slower but often preferred for stylistic exploration.


- **Benchmark:**[ImageBench V1 scored Turbo at 49.2 overall with 18.1-second latency](https://imagebench.ai/imagebench-v1/local--z-image-turbo-6b) and[Base at 47.4 overall with 130.7-second latency](https://imagebench.ai/imagebench-v1/local--z-image-6b) . The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Turbo at Elo 1,102 and rank 18 on July 30, 2026.


- **Size and hardware:**[The family has 6B parameters; Turbo fits within 16GB consumer VRAM, while the sub-second claim uses H800 GPUs](https://github.com/Tongyi-MAI/Z-Image) .


- **Run locally:** Use[Diffusers or ComfyUI](https://github.com/Tongyi-MAI/Z-Image) .


- **Access and terms:**[The official repository code is Apache 2.0](https://github.com/Tongyi-MAI/Z-Image) . Verify the license attached to the exact weight file before commercial deployment.


Choose Turbo for a fast consumer-GPU path and Base when its slower, less-distilled behavior better suits your style.


### Use it on MagicHour


To use Z-Image without a local GPU, select` z-image-turbo` in[Magic Hour’s AI Image Generator](https://magichour.ai/products/ai-image-generator) or pass` model="z-image-turbo"` to[POST /v1/ai-image-generator](https://magichour.ai/api-reference/image-projects/ai-image-generator) .


## [Stable Diffusion](https://github.com/Stability-AI/sd3.5)


Stability AI’s local family includes the mature SDXL ecosystem and the newer SD3.5 Large, Large Turbo, and Medium checkpoints. Practitioners value SD3.5’s style range and creativity but often find FLUX more reliable for people and long prompts; hands, feet, and anatomy remain recurring complaints. SDXL is older, yet[local users still value its fine-tune, ControlNet, and face-workflow ecosystem](https://www.reddit.com/r/StableDiffusion/comments/1rkb9l1/unpopular_opinion_sdxl_still_to_beat/) .


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded SD3.5 Large Turbo at Elo 1,024, Large at 1,023, Medium at 946, and SDXL 1.0 at 876 on July 30, 2026.


- **Released checkpoints:**[Stability AI publishes SD3.5 Large, Large Turbo, and Medium weights](https://github.com/Stability-AI/sd3.5) , while[SDXL provides Base and Refiner weights](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0) .


- **Checkpoint sizes and hardware:**[SD3.5 Large and Large Turbo are 8.1B models; the publisher does not state a minimum VRAM figure for either](https://stability.ai/news/introducing-stable-diffusion-3-5) .[SD3.5 Medium is 2.5B and requires 9.9GB VRAM for the model alone, excluding its text encoders](https://stability.ai/news/introducing-stable-diffusion-3-5) . For SDXL Base,[Hugging Face measured 28.09GB unoptimized, 21.72GB at fp16, and 11.47GB with VAE slicing plus sequential CPU offload](https://huggingface.co/blog/simple_sdxl_optimizations) while generating a batch of four 1024px images on an A100; the separate Refiner adds another pipeline stage, and no universal minimum is published.


- **Run locally:** Use[Hugging Face Diffusers, Stability AI’s official PyTorch repositories, or ComfyUI](https://github.com/Stability-AI/sd3.5) .


- **Access and terms:**[SDXL uses CreativeML Open RAIL++-M](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0) .[SD3.5 checkpoints use Stability AI’s Community License](https://huggingface.co/stabilityai/stable-diffusion-3.5-large) , which should be checked at download time.


Choose this family when ecosystem depth, fine-tuning, and control tooling matter as much as base-model leaderboard position.


## [Qwen-Image](https://github.com/QwenLM/Qwen-Image)


Alibaba’s Qwen team built Qwen-Image around text rendering, prompt understanding, and precise editing. Local users particularly value Edit for preserving faces, poses, and scene structure, while[realistic edits can look plasticky or airbrushed](https://www.reddit.com/r/comfyui/comments/1t0xbcd/can_qwen_image_edit_or_any_similar_image_to_image/) ; testers report that 2512 improves skin, hands, and small details.


- **Benchmark:**[ImageBench V1 scored Qwen-Image-2512 at 58.6 overall, 50.0% capability, 67.1 estimated preference, and 80.2-second average latency](https://imagebench.ai/imagebench-v1/local--qwen-image-2512-20b) . The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded the original Qwen Image at Elo 1,064 and rank 24 on July 30, 2026.


- **Size and hardware:**[The original release is a 20B MMDiT model](https://github.com/QwenLM/Qwen-Image) ; the publisher gives no universal minimum VRAM figure.


- **Run locally:** Use[Hugging Face Diffusers or ComfyUI](https://github.com/QwenLM/Qwen-Image) .


- **Access and terms:**[The family is Apache 2.0](https://github.com/QwenLM/Qwen-Image) .


Choose the generation checkpoint for typography-heavy prompts and the Edit or Layered releases when structural preservation matters.


## [HunyuanImage 3.0](https://github.com/Tencent-Hunyuan/HunyuanImage-3.0)


Tencent’s HunyuanImage 3.0 is an unusually large native multimodal family for knowledge-rich prompts, text rendering, and complex composition. One early practitioner found that expanded prompts could produce striking results and stylized lettering, but[simple prompts were more average and Base felt unfinished](https://www.reddit.com/r/StableDiffusion/comments/1nszzmu/hunyuanimage_30_is_perfect/) . Its footprint keeps it outside ordinary desktop workflows.


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded HunyuanImage 3.0 at Elo 1,125 and Instruct at 1,119 on July 30, 2026. A shared publisher table reports[GenEval 0.72, DPG-Bench 86.10, and WISE 0.57](https://huggingface.co/deepgenteam/DeepGen-1.0) .


- **Size and hardware:**[Base has 80B total and 13B active parameters and needs at least 3×80GB; Instruct needs at least 8×80GB](https://github.com/Tencent-Hunyuan/HunyuanImage-3.0) .


- **Run locally:** Use[Tencent’s official PyTorch repository](https://github.com/Tencent-Hunyuan/HunyuanImage-3.0) .


- **Access and terms:**[Tencent’s community license excludes the EU, UK, and South Korea](https://huggingface.co/tencent/HunyuanImage-3.0/raw/main/LICENSE) .


Choose it only when the model’s semantic and typography strengths justify a multi-GPU deployment and its territory terms fit your use.


## [GLM-Image](https://github.com/zai-org/GLM-Image)


Z.ai’s GLM-Image combines an autoregressive semantic stage with a diffusion decoder, targeting text-heavy and knowledge-intensive images. Early testers found it creative and promising for layout, but[ordinary text-to-image output can feel underbaked and slow beside lighter models](https://www.reddit.com/r/StableDiffusion/comments/1qcnemn/glmimage_t2i_test_and_speed/) .


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Elo 1,049 and rank 28 on July 30, 2026. Z.ai reports[DPG-Bench 84.78, LongText-Bench 0.966 average, and CVTG-2K word accuracy 0.9116](https://github.com/zai-org/GLM-Image) .


- **Size and hardware:**[Current optimization requires one GPU with more than 80GB or multiple GPUs](https://github.com/zai-org/GLM-Image) .


- **Run locally:** Use[Diffusers or SGLang](https://github.com/zai-org/GLM-Image) .


- **Access and terms:**[The checkpoint and repository use Apache 2.0](https://github.com/zai-org/GLM-Image) .


Choose GLM-Image for dense text and semantic layout rather than as the easiest general-purpose local generator.


## [ERNIE-Image](https://github.com/baidu/ERNIE-Image)


Baidu’s ERNIE-Image is an 8B text-to-image family with standard and Turbo checkpoints. Community tests praise clean illustration, coherent backgrounds, and solid text, while reporting[prompt-expander drift, camera-angle misses, diagonal artifacts, and an overprocessed HDR look](https://www.reddit.com/r/StableDiffusion/comments/1slbp8k/ernie_image_released/) .


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded ERNIE Image at Elo 1,166 and Turbo at 1,163 on July 30, 2026. Baidu reports[GenEval 0.8856 without prompt enhancement for standard and 0.8667 for Turbo](https://github.com/baidu/ERNIE-Image) .


- **Size and hardware:**[The family has 8B DiT parameters and runs in a publisher-documented 24GB consumer-GPU configuration](https://github.com/baidu/ERNIE-Image) .


- **Run locally:** Use[Hugging Face Diffusers](https://github.com/baidu/ERNIE-Image) .


- **Access and terms:**[The official code and weights use Apache 2.0](https://github.com/baidu/ERNIE-Image) .


Choose standard for maximum quality or Turbo when its eight-step path matters more than the small benchmark gap.


## [HiDream-O1-Image](https://github.com/HiDream-ai/HiDream-O1-Image)


HiDream’s O1 family is an 8B native multimodal model for generation, editing, and multi-reference personalization. Its broad workflow is attractive, but[early testing is mixed](https://www.reddit.com/r/StableDiffusion/comments/1t7v9fy/hidreamo1image_a_pixel_space_model_no_need_for/) : users report lost detail in distilled Dev outputs and plastic skin or artifact-like texture in some full-model results.


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Dev-2604 at Elo 1,189 and rank 2, Full at 1,116, and Dev at 1,080 on July 30, 2026. The official model card reports[GenEval 0.90, DPG-Bench 89.83, and HPSv3 10.37](https://huggingface.co/HiDream-ai/HiDream-O1-Image/blob/main/README.md) .


- **Size and hardware:**[The family has 8B parameters and requires a CUDA-capable GPU](https://github.com/HiDream-ai/HiDream-O1-Image) .


- **Run locally:** Use[HiDream’s official PyTorch repository](https://github.com/HiDream-ai/HiDream-O1-Image) .


- **Access and terms:**[Code and models use the MIT License](https://github.com/HiDream-ai/HiDream-O1-Image) .


Choose HiDream when one local family must cover generation, editing, and subject-driven personalization.


## [DeepGen 1.0](https://github.com/DeepGenTeam/DeepGen)


DeepGen Team’s DeepGen 1.0 is a compact 5B research model that unifies generation, editing, and reasoning-oriented control. Local-model users like the unified design, but[early discussion flags its fixed 512×512 experiments and immature UI and fine-tuning ecosystem](https://www.reddit.com/r/StableDiffusion/comments/1r3epwp/deepgen_10_a_5b_parameter_lightweight_unified/) .


- **Benchmark:** The publisher reports[DeepGen RL at GenEval 0.87, DPG-Bench 87.90, UniGenBench 75.74, and GEdit-EN 7.17](https://huggingface.co/deepgenteam/DeepGen-1.0) .


- **Size and hardware:**[The model has 5B parameters: a 3B VLM and 2B DiT](https://github.com/DeepGenTeam/DeepGen) .


- **Run locally:** Use[Hugging Face Diffusers](https://huggingface.co/deepgenteam/DeepGen-1.0) .


- **Access and terms:**[The official checkpoint uses Apache 2.0](https://huggingface.co/deepgenteam/DeepGen-1.0) .


Choose DeepGen when compact unified generation and editing matter more than a mature local ecosystem.


## [JoyAI-Image](https://github.com/jd-opensource/JoyAI-Image)


JD’s JoyAI-Image is an edit-first family with Edit and Edit-Plus checkpoints plus text-to-image generation. Early testers found it strong at spatial changes and alternate camera angles, but saw[facial-detail drift and distortion at extreme views](https://www.reddit.com/r/StableDiffusion/comments/1sfkd0l/what_happened_to_joyaiimageedit/) . Adoption has also been slowed by a heavy runtime and rough early ComfyUI workflows.


- **Benchmark:** The independent[KRIS-Bench leaderboard](https://yongliang-wu.github.io/kris_bench_project_page/) scores JoyAI-Image-Edit at 63.44 overall across 1,267 knowledge-reasoning edits.


- **Size and hardware:**[The official runner requires a CUDA-capable GPU; no publisher minimum VRAM figure is given](https://github.com/jd-opensource/JoyAI-Image) .


- **Run locally:** Use[JD’s official PyTorch repository or ComfyUI](https://github.com/jd-opensource/JoyAI-Image) .


- **Access and terms:**[The repository and released weights use Apache 2.0](https://github.com/jd-opensource/JoyAI-Image) .


Choose JoyAI when spatially aware editing and camera changes are more important than lightweight deployment.


## [Krea 2](https://github.com/krea-ai/krea-2)


Krea 2 is built around style diversity, with Raw for exploration and Turbo for speed. The team says it prioritized illustration and varied styles over realism; practitioners praise anatomy, animals, and wide compositions while reporting[weaker photography, occasional prompt misses, VAE grid texture, and aggressive safety behavior](https://www.reddit.com/r/StableDiffusion/comments/1ulxqep/krea_2_simple_gen_workflow_with_good_settings_for/) .


- **Benchmark:**[ImageBench V1 scored Krea 2 Turbo at 56.5 overall, 58% capability, 55.2 estimated preference, and 73.2-second average latency](https://imagebench.ai/imagebench-v1/local--krea-2-turbo) .


- **Run locally:** Use[Krea’s official PyTorch repository or ComfyUI](https://github.com/krea-ai/krea-2) .


- **Access and terms:**[Raw and Turbo are gated and use the Krea 2 Community License](https://huggingface.co/krea/Krea-2-Turbo) .


Choose Raw for style exploration and Turbo when iteration speed matters.


## [Ideogram 4](https://github.com/ideogram-oss/ideogram4)


Ideogram’s first open-weight family is a design-focused 9.3B generator for multilingual typography, structured JSON prompts, bounding boxes, palettes, and native 2K output. Practitioners like its direct layout control, but[criticize missing full-precision weights and restrictive safety behavior](https://www.reddit.com/r/StableDiffusion/comments/1ubj0s5/ideogram_4_they_are_gatekeeping_the_highprecision/) .


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded the Quality checkpoint at Elo 1,175 and rank 4 on July 30, 2026. Ideogram cites a blind typography test in which[10 designers selected Ideogram 4 first 47.9% of the time and rated it 3.55/5 for real client work](https://github.com/ideogram-oss/ideogram4) .


- **Size and hardware:**[The nf4 and fp8 releases contain a 9.3B model](https://github.com/ideogram-oss/ideogram4) .


- **Run locally:** Use[the official PyTorch repository or ComfyUI](https://github.com/ideogram-oss/ideogram4) .


- **Access and terms:**[The gated weights use the Ideogram 4 Non-Commercial License](https://huggingface.co/ideogram-ai/Ideogram-v4) .


Choose Ideogram 4 for local, non-commercial typography and layout work.


## [Boogu-Image-0.1](https://github.com/boogu-project/Boogu-Image)


The Boogu Project’s first family covers Base, Turbo, Edit, and Edit-Turbo. A[192-prompt practitioner test](https://www.reddit.com/r/StableDiffusion/comments/1uk4t27/boogu_image_in_depth_test/) found high detail and good overall fidelity but a strong symmetry bias and weak composition; edit users report failures on complex changes and artifacts in small faces, limbs, eyes, and text.


- **Benchmark:**[ImageBench V1 scored Boogu-Image-0.1-Turbo at 61.9 overall, 57% capability, 66.6 estimated preference, and 9.9-second average latency](https://imagebench.ai/imagebench-v1/local--boogu-image-turbo-vs-fal--bytedance--seedream-v5-pro) .


- **Size and hardware:**[Published configurations span 12GB to 80GB VRAM](https://huggingface.co/Boogu/Boogu-Image-0.1-Base) , depending on resolution, offload, and quantization.


- **Run locally:** Use[the official PyTorch repository or ComfyUI](https://github.com/boogu-project/Boogu-Image) .


- **Access and terms:**[The family uses Apache 2.0](https://github.com/boogu-project/Boogu-Image) .


Choose Boogu when you want generation and editing variants with documented memory tradeoffs.


## [Cosmos 3](https://github.com/NVIDIA/cosmos)


NVIDIA’s Cosmos 3 is a 64B omnimodal world-model family with a specialized text-to-image checkpoint. It leads the current open-weights arena, but local practitioners stress that[its reasoner-plus-generator footprint is impractical on ordinary desktops](https://www.reddit.com/r/StableDiffusion/comments/1twh913/gotta_call_it_cosmos3_super_need_its_anima_moment/) .


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Cosmos3-Super-Text2Image at Elo 1,218 and rank 1, and the four-step version at 1,181 and rank 3 on July 30, 2026.


- **Size and hardware:**[Cosmos3-Super and its text-to-image checkpoint are 64B and use multi-GPU configurations](https://github.com/NVIDIA/cosmos) .


- **Run locally:** Use[NVIDIA’s Cosmos framework, Diffusers, vLLM-Omni, or SGLang](https://github.com/NVIDIA/cosmos) .


- **Access and terms:**[Code and models use OpenMDW 1.1](https://github.com/NVIDIA/cosmos) .


Choose Cosmos 3 only when frontier-scale quality is worth data-center-class infrastructure.


## [LongCat-Image](https://github.com/meituan-longcat/LongCat-Image)


Meituan’s 6B LongCat-Image family covers bilingual generation, typography, and editing. Users like its product and face preservation, but report[slow Base inference, uneven editing quality, and occasional oversaturated or plastic output](https://www.reddit.com/r/StableDiffusion/comments/1pevqxb/meituan_longcat_image_6b_dense_image_generation/) .


- **Benchmark:** Meituan reports[GenEval 0.87, DPG-Bench 86.80, and WISE 0.65](https://github.com/meituan-longcat/LongCat-Image) . The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Elo 1,032 and rank 30 on July 30, 2026.


- **Size and hardware:**[The dense DiT has 6B parameters and CPU-offloaded generation needs about 17GB VRAM](https://github.com/meituan-longcat/LongCat-Image) .


- **Run locally:** Use[Diffusers or ComfyUI](https://github.com/meituan-longcat/LongCat-Image) .


- **Access and terms:**[The family uses Apache 2.0](https://github.com/meituan-longcat/LongCat-Image) .


Choose LongCat for bilingual text rendering or editing when 17GB-class offloaded inference is acceptable.


## [FIBO](https://huggingface.co/briaai/FIBO)


Bria AI’s 8B FIBO is built for structured, repeatable art direction: a VLM expands an idea into a long JSON description that controls lighting, camera, composition, and color. Practitioners praise its prompt adherence and texture, but[report weaker hands and note that the local release lacks broad native ComfyUI support](https://www.reddit.com/r/StableDiffusion/comments/1ojsdji/) .


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Elo 1,068 and rank 22 on July 30, 2026.


- **Size and hardware:**[FIBO has 8B parameters](https://huggingface.co/briaai/FIBO) ; the publisher gives no universal minimum VRAM figure.


- **Run locally:** Use[the official Diffusers pipeline; Bria also supplies ComfyUI integrations](https://huggingface.co/briaai/FIBO) .


- **Access and terms:**[Weights are non-commercial; commercial deployment requires Bria licensing](https://huggingface.co/briaai/FIBO) .


Choose FIBO when reproducible, programmatic visual control matters more than free-form prompting.


## [Infinity](https://github.com/FoundationVision/Infinity)


FoundationVision’s Infinity is a bitwise autoregressive generator rather than a diffusion model.[Its 2B and 8B checkpoints target fast 1024px synthesis and strong composition](https://github.com/FoundationVision/Infinity) . Practitioners like the prompt adherence of autoregressive models, but[some see little improvement in the familiar “AI-generated” look and note that the larger 20B release is still absent](https://www.reddit.com/r/StableDiffusion/comments/1jug2oy/) .


- **Benchmark:** The publisher reports[8B GenEval 0.79 and DPG-Bench 86.6](https://github.com/FoundationVision/Infinity) . The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Infinity 8B at Elo 1,045 and rank 29 on July 30, 2026.


- **Size and hardware:**[Official releases include 2B and 8B checkpoints](https://github.com/FoundationVision/Infinity) ; the publisher gives no universal minimum VRAM figure.


- **Run locally:** Use[the official PyTorch notebooks or Docker workflow](https://github.com/FoundationVision/Infinity) .


- **Access and terms:**[The project uses the MIT License](https://github.com/FoundationVision/Infinity) .


Choose Infinity when you want to evaluate an autoregressive alternative to local diffusion pipelines.


## [Lumina-Image 2.0](https://github.com/Alpha-VLLM/Lumina-Image-2.0)


Alpha-VLLM’s Lumina-Image 2.0 is a compact 2.6B 1024px generator with strong prompt adherence and an open fine-tuning path. Practitioners find it capable for composition, but[describe unstable anatomy, uneven detail, and a demanding setup relative to mature SDXL workflows](https://www.reddit.com/r/StableDiffusion/comments/1igo8th/how_do_you_run_luminaimage_20_locally_can_i_use/) .


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Elo 968 and rank 36 on July 30, 2026.


- **Size and hardware:**[The checkpoint has 2.6B parameters and supports CPU offload](https://github.com/Alpha-VLLM/Lumina-Image-2.0) .


- **Run locally:** Use[Diffusers, the official PyTorch repository, or ComfyUI](https://github.com/Alpha-VLLM/Lumina-Image-2.0) .


- **Access and terms:**[The project uses Apache 2.0; its Gemma 2 text encoder requires Hugging Face access](https://github.com/Alpha-VLLM/Lumina-Image-2.0) .


Choose Lumina when an Apache-licensed, fine-tunable 2.6B base is more important than ecosystem maturity.


## [Sana](https://github.com/NVlabs/Sana)


NVIDIA’s Sana family uses linear attention and aggressive latent compression for efficient high-resolution generation. It is attractive for small models and fast Sprint checkpoints, although practitioners characterize[Sprint as extremely fast but visibly lower quality](https://www.reddit.com/r/StableDiffusion/comments/1knlf5u/fastest_model_for_generating_images/) .


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Sana Sprint 1.6B at Elo 934 and rank 40 on July 30, 2026. NVIDIA reports[Sana 1.5 1.6B at GenEval 0.82 and DPG 84.5](https://github.com/NVlabs/Sana) .


- **Size and hardware:**[Releases span 0.6B, 1.6B, and 4.8B](https://github.com/NVlabs/Sana) . NVIDIA documents[4K inference within about 8GB using tiling, offload, and quantization](https://github.com/NVlabs/Sana) .


- **Run locally:** Use[Diffusers, SGLang, the official repository, or ComfyUI](https://github.com/NVlabs/Sana) .


- **Access and terms:** The code is Apache 2.0;[model weights use NVIDIA’s Open Model License](https://huggingface.co/Efficient-Large-Model/Sana_1600M_1024px) .


Choose Sana when efficiency, resolution, and fine-tuning openness matter more than arena rank.


## [OmniGen2](https://github.com/VectorSpaceLab/OmniGen2)


VectorSpaceLab’s OmniGen2 unifies text-to-image generation, instruction editing, and in-context composition. Users like its ability to combine references, but report[slow local runs, inconsistent edits, oversaturation, and installation friction](https://www.reddit.com/r/StableDiffusion/comments/1li4fui/omnigen_2_is_out/) .


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Elo 905 and rank 43 on July 30, 2026.


- **Size and hardware:**[Native inference needs about 17GB VRAM; CPU offload cuts that roughly in half, and slower sequential offload can run below 3GB](https://github.com/VectorSpaceLab/OmniGen2) .


- **Run locally:** Use[the official PyTorch repository or ComfyUI](https://github.com/VectorSpaceLab/OmniGen2) .


- **Access and terms:**[The project uses Apache 2.0](https://github.com/VectorSpaceLab/OmniGen2) .


Choose OmniGen2 when multi-image composition and unified editing matter more than speed.


## [BAGEL](https://github.com/bytedance-seed/BAGEL)


ByteDance Seed’s BAGEL is a 7B-active, 14B-total Mixture-of-Transformer-Experts model that combines understanding, generation, and editing. Practitioners praise its ambitious unified workflow, but[report blurry outputs, aggressive safety behavior, anatomy failures, and heavy local memory use](https://www.reddit.com/r/StableDiffusion/comments/1krnolw/bytedance_released_multimodal_model_bagel_with/) .


- **Benchmark:** The publisher reports[GenEval 0.82 without rewriting and 0.88 with rewriting, plus WISE 0.52 and 0.70 respectively](https://github.com/bytedance-seed/BAGEL) . The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Elo 900 and rank 45 on July 30, 2026.


- **Size and hardware:**[BAGEL has 7B active and 14B total parameters; official guidance provides NF4 for 12GB–32GB, INT8 for 22GB–32GB, and full precision above 32GB](https://github.com/bytedance-seed/BAGEL) .


- **Run locally:** Use[ByteDance’s official PyTorch repository](https://github.com/bytedance-seed/BAGEL) .


- **Access and terms:**[The family uses Apache 2.0](https://github.com/bytedance-seed/BAGEL) .


Choose BAGEL to experiment with a single multimodal model across understanding, generation, and editing.


## [CogView4](https://github.com/zai-org/CogView4)


Z.ai’s CogView4 is a 6B bilingual generator designed for Chinese and English prompts and native high-resolution output. Community interest centers on its text accuracy and prompt length, while users warn that[its large text encoder and custom stack make it heavier than the 6B headline suggests](https://www.reddit.com/r/StableDiffusion/comments/1j3633u/) .


- **Benchmark:** Z.ai reports[DPG-Bench 85.13 and Chinese text F1 0.6168](https://github.com/zai-org/CogView4) .


- **Size and hardware:**[At 1024px and batch four, the publisher reports 35GB normally, 20GB with CPU offload, and 13GB with offload plus a four-bit text encoder](https://github.com/zai-org/CogView4) .


- **Run locally:** Use[Diffusers or the CogKit toolkit](https://github.com/zai-org/CogView4) .


- **Access and terms:**[The project uses Apache 2.0](https://github.com/zai-org/CogView4) .


Choose CogView4 for bilingual text-heavy work when you can accommodate its text encoder.


## [Kandinsky 5](https://github.com/kandinskylab/kandinsky-5)


[Kandinsky Lab’s current family includes 6B text-to-image and editing checkpoints with strong Russian concepts and typography, plus a related video stack](https://github.com/kandinskylab/kandinsky-5) . One community workflow praises its skin texture and image-to-image results, but[calls the family an underrated underdog and needs offload workarounds to fit it on an 8GB GPU](https://www.reddit.com/r/comfyui/comments/1rbtfqq/) .


- **Performance:** The publisher reports[13-second latency for the 6B T2I Lite checkpoint on an H100 80GB and a number-one open-source LMArena ranking for the related Video Pro model](https://github.com/kandinskylab/kandinsky-5) ; treat the latency as a server-GPU result, not a consumer estimate.


- **Run locally:** Use[Kandinsky’s official PyTorch repository or Diffusers](https://github.com/kandinskylab/kandinsky-5) .


- **Access and terms:**[The project uses the MIT License](https://github.com/kandinskylab/kandinsky-5) .


Choose Kandinsky 5 for Russian-language concepts, typography, or a shared image-and-video research stack.


## [F-Lite](https://github.com/fal-ai/f-lite)


[Freepik and fal built F-Lite from licensed, SFW data, with Standard, Texture, and smaller 7B releases](https://github.com/fal-ai/f-lite) . That provenance is its clearest differentiator; practitioners welcome the licensing story but note[limited text rendering and malformed anatomy in some outputs](https://www.reddit.com/r/StableDiffusion/comments/1kasrgr/) .


- **Size and hardware:**[The primary checkpoints are 10B and the publisher recommends at least 24GB VRAM](https://github.com/fal-ai/f-lite) .


- **Run locally:** Use[Diffusers, the official repository, or ComfyUI](https://github.com/fal-ai/f-lite) .


- **Access and terms:**[The weights use CreativeML Open RAIL-M](https://huggingface.co/Freepik/F-Lite) .


Choose F-Lite when licensed-data provenance and commercial safety review are central to the evaluation.


## [Janus-Pro](https://github.com/deepseek-ai/Janus)


DeepSeek’s Janus-Pro combines visual understanding and image generation in 1B and 7B checkpoints. Practitioners praise[the 1B release’s literal prompt adherence and speed](https://www.reddit.com/r/StableDiffusion/comments/1ieliyz/janus_pro_1b_offers_great_prompt_adherence/) , but its hard-coded 384px output and weaker aesthetic finish make it more of a compact research tool than a production image model.


- **Benchmark:** The[Artificial Analysis arena](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) recorded Janus-Pro at Elo 711 and rank 48 on July 30, 2026. Z.ai’s shared table reports[Janus-Pro-7B at DPG-Bench 84.19](https://github.com/zai-org/CogView4) .


- **Run locally:** Use[DeepSeek’s official PyTorch repository or ComfyUI](https://github.com/deepseek-ai/Janus) .


- **Access and terms:**[Code is MIT; weights use the DeepSeek Model License](https://github.com/deepseek-ai/Janus) .


Choose Janus-Pro for compact multimodal experiments rather than maximum-resolution image work.


## [Kolors](https://github.com/Kwai-Kolors/Kolors)


Kuaishou’s Kolors is a bilingual latent-diffusion family for Chinese and English generation, portraits, IP-Adapter workflows, ControlNet, and inpainting. Practitioners value its Chinese handling and portraits, but[prompt comparisons find weaker complex-scene adherence than newer models](https://www.reddit.com/r/StableDiffusion/comments/1ef4zu6/prompt_adherence_comparison_dallee_sd3_auraflow/) .


- **Benchmark:** Kuaishou reports[MPS 10.3, human overall satisfaction 3.59, visual appeal 3.99, and text faithfulness 4.17](https://github.com/Kwai-Kolors/Kolors) on its KolorsPrompts evaluation.


- **Run locally:** Use[Diffusers, the official repository, or ComfyUI](https://github.com/Kwai-Kolors/Kolors) .


- **Access and terms:**[Code is Apache 2.0; the weights are open for academic research, while commercial use requires registration or separate permission](https://github.com/Kwai-Kolors/Kolors) .


Choose Kolors for bilingual portrait and control workflows when its model terms fit the deployment.


## [AuraFlow](https://huggingface.co/fal/AuraFlow)


Fal’s AuraFlow is a fully Apache-licensed flow-based generator built around literal prompt adherence. Practitioners found v0.2 unusually good at complex composition, but also described[weak aesthetics, anatomy problems, and a clip-art-like look](https://www.reddit.com/r/StableDiffusion/comments/1ej2qbu/flux_or_flow_in_terms_of_prompt_adherence/) ; v0.3 improved cohesion for some prompts while regressing adherence for others.


- **Benchmark:** The publisher’s model collection reports[GenEval above 0.7 for the AuraFlow v0.x series](https://huggingface.co/fal/AuraFlow) .


- **Run locally:** Use[Hugging Face Diffusers or ComfyUI](https://huggingface.co/fal/AuraFlow) .


- **Access and terms:**[The model card and weights use Apache 2.0](https://huggingface.co/fal/AuraFlow) .


Choose AuraFlow when permissive terms and literal composition matter more than polished default aesthetics.


## [PixArt-Σ](https://github.com/PixArt-alpha/PixArt-sigma)


The PixArt team’s PixArt-Σ is a compact diffusion-transformer family for 512px, 1024px, and 2K generation. Practitioners praise its prompt understanding and low training cost, while reporting[weaker default aesthetics, anatomy errors, and a need for refinement](https://www.reddit.com/r/StableDiffusion/comments/1clf240/) .


- **Benchmark:** Z.ai’s shared table reports[PixArt-α at DPG-Bench 71.11](https://github.com/zai-org/CogView4) ; this older sibling score should not be treated as a PixArt-Σ result.


- **Size and hardware:**[The main DiT is approximately 0.6B parameters](https://github.com/PixArt-alpha/PixArt-sigma) .[Community workflows report local use around 6GB VRAM](https://www.reddit.com/r/StableDiffusion/comments/1clf240/) , but hardware needs vary with the T5 text encoder and offload.


- **Run locally:** Use[Diffusers or the official PyTorch repository](https://github.com/PixArt-alpha/PixArt-sigma) .


- **Access and terms:**[The project uses Apache 2.0](https://github.com/PixArt-alpha/PixArt-sigma) .


Choose PixArt-Σ when you want a compact, permissively licensed base for high-resolution generation and fine-tuning.


## How to choose your first local test


1. Pick the capability: generation, typography, editing, or multi-reference composition.


1. Select the exact released checkpoint rather than the family name alone.


1. Confirm the checkpoint’s current access and commercial terms.


1. Match its published GPU configuration to your machine.


1. Compare exact releases under one benchmark protocol.


1. Run your own prompts, resolutions, edit inputs, and seeds before production use.


For consumer hardware, the publisher figures point to FLUX.2 klein 4B at approximately 13GB, Z-Image Turbo at 16GB, ERNIE-Image at 24GB, LongCat-Image around 17GB with offload, and OmniGen2 around 17GB natively. Sana and Boogu publish lower-memory configurations through quantization and offload. HunyuanImage 3.0, GLM-Image, and Cosmos 3 belong in the multi-GPU or greater-than-80GB class.


### Is FLUX.1 better than Stable Diffusion 3.5?


The live arena places FLUX.1 dev at[Elo 1,029](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) , narrowly above SD3.5 Large Turbo at 1,024 and Large at 1,023, but that does not make it a universal winner. FLUX.1 is usually the stronger prompt-following baseline; Stable Diffusion retains a deeper fine-tuning and control ecosystem. Compare their exact checkpoints on your prompts, then include license and hardware fit in the decision.


If local model operations fall outside your workflow,[Magic Hour’s AI Image Generator](https://magichour.ai/blog/products/ai-image-generator) exposes selectable` flux-schnell` ,` flux-2-klein` , and` z-image-turbo` model IDs through its hosted product. The[AI Image Generator API reference](https://magichour.ai/blog/api-reference/image-projects/ai-image-generator) documents model selection and job creation.
