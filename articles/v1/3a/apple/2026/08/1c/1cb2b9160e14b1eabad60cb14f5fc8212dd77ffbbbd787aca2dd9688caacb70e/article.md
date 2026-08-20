---
schema_version: "1.0.0"
document_id: "1cb2b9160e14b1eabad60cb14f5fc8212dd77ffbbbd787aca2dd9688caacb70e"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/taming-outlier-tokens"
published_at: null
first_seen_at: "2026-08-06T08:50:57.010371+00:00"
fetched_at: "2026-08-06T08:50:57.924596+00:00"
content_hash: "sha256:297b44f032efaa976e78b8f61fc8a448c331c580039533b9796086ef2220c78f"
---

# Taming Outlier Tokens in Diffusion Transformers

[View publication](https://arxiv.org/abs/2605.05206)


We study outlier tokens in Diffusion Transformers (DiTs) for image generation. Prior work has shown that Vision Transformers (ViTs) can produce a small number of high-norm tokens that attract disproportionate attention while carrying limited local information, but their role in generative models remains underexplored. We show that this phenomenon appears in both the encoder and denoiser of modern Representation Autoencoder (RAE)-DiT pipelines: pretrained ViT encoders can produce outlier representations, and DiTs themselves can develop internal outlier tokens, especially in intermediate layers. Moreover, simply masking high-norm tokens does not improve performance, indicating that the problem is not only caused by a few extreme values, but is more closely related to corrupted local patch semantics. To address this issue, we introduce Dual-Stage Registers (DSR), a register-based intervention for both components: trained registers when available, recursive test-time registers otherwise, and diffusion registers for the denoiser. Across ImageNet and large-scale text-to-image generation, these interventions consistently reduce outlier artifacts and improve generation quality. Our results highlight outlier-token control as an important ingredient in building stronger DiTs.


- † Rice University
- * Equal contribution
