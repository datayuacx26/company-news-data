---
schema_version: "1.0.0"
document_id: "5862f79eb1beb753f21d0f40b21bcef62016514a40649262e2052cc4198091f0"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-d1044ff9c1aa"
canonical_url: "https://www.photoroom.com/blog/prx-pixel-photoroom-s-open-source-pixel-space-image-model"
published_at: null
first_seen_at: "2026-07-23T22:00:08.835718+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:68d9104299e3567d87af5b3f12a3a1746482a6b5533d7150bf279b4d242c8436"
---

# PRX Pixel: Photoroom's open-source, pixel-space image model

Most AI image models edit a compressed copy of your image rather than the image itself. PRX Pixel works on the raw pixels directly. It is Photoroom's latest open-source text-to-image model and the newest chapter of our open PRX family, trained from scratch, released under Apache 2.0, and built to show what high-fidelity generation looks like once you take the compression step out of the loop.


The weights are open, the training recipe is published, and the demo is live. This post walks through the technical choices behind PRX Pixel, how we trained a working model in on roughly $1,500 of compute, and where this line of research is heading.


PRX Pixel prompt to punk musician output


## Why most AI images lose fidelity


If you have used a latent diffusion model, and you almost certainly have, the picture you thought you were editing was never quite the thing the model touched.


State-of-the-art models in the SDXL and Flux families share one basic pipeline. First they compress an image into a small grid of numbers called a latent, a compact encoding in which most of the visual information survives but the result is an interpretation of your image rather than a copy of it. The model runs its denoising process over that compressed representation, and a variational autoencoder (VAE) then decompresses the result back into a full image.


That final step rebuilds the image rather than reversing the compression exactly. The VAE fills in detail from what it has learned to expect during training, and that is where hallucinations creep in, because the model ends up generating what it thinks should be there rather than restoring what was actually there.


For general images, a soft reconstruction rarely matters. Nobody is checking whether a background tree has exactly the right bark texture. Product photos are far less forgiving, though. When the logo, the label copy, the packaging color, or the stitching on a seam shifts during reconstruction, the listing can become unusable, and in markets with strict marketplace image-compliance rules a single hallucinated detail is enough to get that listing rejected.


None of this makes latent diffusion a bad design. It is a sound engineering tradeoff for general image generation: far cheaper to train and run at latent resolution than at pixel resolution, and VAE quality is high enough that most use cases never feel the loss. For product image editing, where a shifted label or logo makes the output unusable, that tradeoff no longer holds up.


## PRX Pixel: working at the pixel level


PRX Pixel takes the VAE out of the pipeline. It denoises raw RGB pixel values directly through x-prediction, with no intermediary compression step, so every operation in training and inference happens on the actual pixels rather than on a compressed stand-in for them.


That changes the fidelity profile. With no reconstruction pass sitting between the model's output and the image you see, there is no decoding step left to introduce error of its own.


A few architectural choices make this practical:


-


**x-prediction in pixel space:** Instead of predicting the noise added to a latent, the model predicts the clean pixel values directly. This is a different signal-to-noise regime from latent diffusion, and it interacts differently with the perceptual losses we added.


-


**Perceptual losses (LPIPS and DINOv2):** Pixel-by-pixel loss alone is not enough to produce coherent images at pixel resolution, so we added LPIPS, a perceptual loss grounded in the VGG network, together with a DINOv2-based loss, pushing the model toward outputs that hold together in both low-level texture and high-level visual structure.


-


**TREAD token routing:** A compute-efficient routing mechanism that sends different tokens through different compute paths inside the transformer, keeping training tractable at pixel resolution by putting compute where the model needs it most.


-


**REPA alignment with DINOv3:** A representation-alignment technique that anchors the model's internal representations to those of a strong pretrained visual model, DINOv3, which helps the model converge faster and stay visually coherent even early in training.


-


**Muon optimizer:** An optimizer well-suited to training large transformers efficiently at scale.


-


**Text encoder:** The model conditions on Qwen3-VL rather than the T5 or Gemma encoders used in the latent PRX variants, which gives it strong multilingual prompting natively.


The tradeoff is real. Pixel-space training is more computationally demanding than latent training, because the working space is larger and noisier. The models are bigger, roughly 7B parameters for the PRXTransformer2DModel against 1.3B for the latent PRX variants, and inference is slower. If your goal is generating images at the lowest possible compute cost per image, latent diffusion will keep beating pixel-space models on that metric for a long while yet.


What pixel space buys you is fidelity. When every detail in an image has to stay true to the source, as it does in product photography, medical imaging, or document processing, working directly on pixels sets a higher fidelity floor.


## What we released


**Model type**


Text-to-image, pixel-space foundation model


**Parameters**


~7B (PRXTransformer2DModel)


**Text encoder**


Qwen3-VL


**Training method**


x-prediction, no VAE, trained from scratch


**License**


Apache 2.0


**Weights**


[Photoroom/prxpixel-t2i on Hugging Face](https://huggingface.co/Photoroom/prxpixel-t2i)


**Demo**


[Photoroom/PRX-Pixel on Hugging Face Spaces](https://huggingface.co/spaces/Photoroom/PRX-Pixel)


**Code**


[github.com/Photoroom/PRX](http://github.com/Photoroom/PRX) (Apache 2.0)


**Diffusers**


Available on the main branch of Diffusers


**Languages**


Multilingual prompting via Qwen3-VL


PRX Pixel is the latest model in the[PRX collection on Hugging Face](https://huggingface.co/collections/Photoroom/prx) . That collection also holds the latent diffusion PRX variants, from 256px through 1024px, built on flow matching; they are a separate architectural lineage from PRX Pixel. The previous chapter of this story is our[September 2025 announcement](https://www.photoroom.com/inside-photoroom/open-source-t2i-announcement) , where we introduced the first PRX checkpoint and the Mirage latent experiments. PRX Pixel is the next step in it, with the model now working on pixels instead of a compressed interpretation of them.


## How we trained and tested it


To pressure-test the pre-training recipe, we ran a 24-hour experiment on 32 NVIDIA H200 GPUs, at a total compute cost of roughly $1,500. This was a recipe-validation run, not the training behind the released weights – the PRX Pixel model we published was trained separately.


The speedrun is not a claim that 24 hours is the right training run for a production model. What it shows is that a single day is enough to check whether a recipe holds together, whether pixel-space training, the perceptual losses, TREAD, REPA, and the Muon optimizer actually converge to something coherent. A fast, cheap proof of recipe lets you iterate on architecture decisions before running up a large compute bill on a direction that might not pay off.


The full recipe, configs, and training code are published in[Part 3 of our blog series](https://huggingface.co/blog/Photoroom/prx-part3) on Hugging Face, while the recently released[Part 4](https://huggingface.co/blog/Photoroom/prx-part4-data) covers our data strategy. Future posts will dig into fine-tuning, preference alignment, and the curation tooling behind them. Parts 1 and 2 cover the architecture experiments and the early PRX variants, so reading Parts 1 through 4 in order gives you the full context for how we arrived at pixel space and what drove the shift. All of it is reproducible: fork the[GitHub repository](https://github.com/Photoroom/PRX) to run it yourself, and open an issue if something in the recipe looks wrong.


## Why Photoroom builds its own models


Most companies in AI pick a foundation model from a third party and build a product layer on top of it, which is a perfectly reasonable choice given how expensive the best foundation models are to train.


Photoroom took a different path from the start. Our CEO Matthieu Rouif has framed the choice plainly: "The question isn't how to make beautiful images, it's how to make images that convert without breaking product fidelity." That distinction carries more weight than it first appears.


A general-purpose foundation model is trained to produce images that look compelling across a huge range of subjects and styles. The tradeoffs it makes, such as softening fine detail for aesthetic coherence or blending textures at boundaries, stay invisible in most outputs and are broadly correct for most uses. In product photography those same tradeoffs break the result, because a label that has been reconstructed even slightly is a label that has been changed.


Training our own models means we control the architecture, the training data, the loss functions, and the fidelity objectives, so we can make the fidelity-over-aesthetics call at the training level rather than patching it in afterward. It also means that when an output goes wrong, we have the whole stack to debug instead of waiting on an external provider to ship a change.


Publishing the recipe openly comes from the same philosophy. Photoroom is a French AI company, and we believe that building capable AI in the open, with weights, methods, and code under a permissive license, is both better science and the right way to earn trust with the community that will use and scrutinize the results.


## Where this is heading


PRX Pixel is a text-to-image model, and that is the whole of its current scope. It is research, and it is not in Photoroom's product today.


What pulls us toward pixel space is less about text-to-image and more about image editing. When you edit a real product photo, replacing a background, changing the lighting, or removing an object, you want the model working on the actual pixels of your original rather than on a compressed version of it. That is exactly where a VAE's reconstructive errors cost the most, and where the pixel-space approach offers the clearest practical payoff.


Getting text-to-image right at pixel resolution is the step before getting image editing right at pixel resolution. The recipe, the losses, and the architecture choices all have to work for generation before they can power editing, and that is the direction this work points.


## Try it, build on it, contribute


-


**Run the demo:**[huggingface.co/spaces/Photoroom/PRX-Pixel](http://huggingface.co/spaces/Photoroom/PRX-Pixel)


-


**Download the weights:**[huggingface.co/Photoroom/prxpixel-t2i](http://huggingface.co/Photoroom/prxpixel-t2i) (Apache 2.0, free to use commercially)


-


**Read the training blog series:**[PRX collection on Hugging Face](https://huggingface.co/collections/Photoroom/prx) (Parts 1 to 4 cover the full journey)


-


**Fork the code:**[github.com/Photoroom/PRX](http://github.com/Photoroom/PRX)


-


**Contribute or report issues:** open an issue on GitHub or comment on the Hugging Face model page


-


**Join the team:** we are hiring researchers and engineers who want to work on this problem; see open roles on Photoroom's careers page


---


## The team and acknowledgments


PRX Pixel is the work of Photoroom's research team. We thank NVIDIA for the H200 GPU access that made the speedrun possible; PRX was trained on NVIDIA Hopper architecture, and the inference stack is optimized with NVIDIA TensorRT. We also thank the open-source research community whose published work on perceptual losses, token routing, and representation alignment this model builds on, along with the Hugging Face community for hosting the weights, the demo, and the conversation around this work.


---


## References


-


*PRX Pixel model card:[https://huggingface.co/Photoroom/prxpixel-t2i](https://huggingface.co/Photoroom/prxpixel-t2i)*


-


*PRX Pixel demo (Hugging Face Space):[https://huggingface.co/spaces/Photoroom/PRX-Pixel](https://huggingface.co/spaces/Photoroom/PRX-Pixel)*


-


*PRX GitHub repository:[https://github.com/Photoroom/PRX](https://github.com/Photoroom/PRX)*


-


*PRX collection on Hugging Face:[https://huggingface.co/collections/Photoroom/prx](https://huggingface.co/collections/Photoroom/prx)*


-


*PRX Part 1 — Text-to-Image Architectural Experiments:[https://huggingface.co/blog/Photoroom/prx-part1-architectures](https://huggingface.co/blog/Photoroom/prx-part1-architectures)*


-


*PRX Part 2 — Training Design for Text-to-Image Models: Lessons from Ablations:[https://huggingface.co/blog/Photoroom/prx-part2](https://huggingface.co/blog/Photoroom/prx-part2)*


-


*PRX Part 3 — Training a Text-to-Image Model in 24h:[https://huggingface.co/blog/Photoroom/prx-part3](https://huggingface.co/blog/Photoroom/prx-part3)*


-


*PRX Part 4 — Our Data Strategy:[https://huggingface.co/blog/Photoroom/prx-part4-data](https://huggingface.co/blog/Photoroom/prx-part4-data)*


-


*Diffusers PRX pipeline documentation:[https://huggingface.co/docs/diffusers/main/en/api/pipelines/prx](https://huggingface.co/docs/diffusers/main/en/api/pipelines/prx)*


-


*Photoroom open-source T2I announcement (September 2025):[https://www.photoroom.com/inside-photoroom/open-source-t2i-announcement](https://www.photoroom.com/inside-photoroom/open-source-t2i-announcement)*


-


*Photoroom Foundation Diffusion Model:[https://www.photoroom.com/inside-photoroom/photoroom-foundation-diffusion-model](https://www.photoroom.com/inside-photoroom/photoroom-foundation-diffusion-model)*


-


*Photoroom open-sources PRX at NVIDIA GTC (March 2026):[https://itbusinessnet.com/2026/03/launched-at-nvidia-gtc-photoroom-open-sources-prx-a-1024px-text-to-image-model-trained-on-nvidia-hopper-gpus/](https://itbusinessnet.com/2026/03/launched-at-nvidia-gtc-photoroom-open-sources-prx-a-1024px-text-to-image-model-trained-on-nvidia-hopper-gpus/)*
