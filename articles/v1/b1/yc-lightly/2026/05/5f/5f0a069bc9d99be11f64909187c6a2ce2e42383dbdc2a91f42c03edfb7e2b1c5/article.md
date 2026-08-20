---
schema_version: "1.0.0"
document_id: "5f0a069bc9d99be11f64909187c6a2ce2e42383dbdc2a91f42c03edfb7e2b1c5"
company_key: "yc-lightly"
company: "Lightly"
source_id: "yc-lightly-news-import-b9754ffe9935"
canonical_url: "https://lightly.ai/blog/vision-transformers-vit"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-22T02:25:05.125057+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:36774c52bb0741af70129fbfcbfff64e0d3f683b9d4140dbfd29ec971c7d47fa"
---

# Introduction to ViT (Vision Transformers): Everything You Need to Know

Transformers, introduced by Vaswani et al. (2017), reshaped natural language processing by replacing recurrent networks with self attention mechanisms that model relationships across an entire sequence in parallel. The transformer model's scalability made it the foundation of modern natural language processing systems like BERT and GPT — where GPT's name directly reflects the transformer architecture at its core.


As transformer model performance in natural language processing continued to climb, researchers began exploring whether the same transformer architecture could match or surpass the performance of convolutional neural networks in computer vision. The answer came from the Google Research team.


The vision transformer ViT, proposed by Dosovitskiy et al. from the Google Research team and published at ICLR 2021, was a landmark contribution to deep learning. By treating an input image as a sequence of fixed size patches — each encoded like a word token — the vision transformer ViT demonstrated that a standard transformer encoder applied to image patches, with sufficient pre-training data, could match or outperform the best convolutional neural networks of the time on ImageNet image recognition.


Vision transformers have since become central to modern computer vision. They power a wide range of foundation models: CLIP learns aligned image-text representations; DINOv2 produces general-purpose visual features for downstream tasks through self-supervised learning; SAM2 uses a vision transformer ViT backbone for promptable image segmentation; and Diffusion Transformers (DiT) have replaced U-Net backbones in leading generative modeling systems.


As of 2026, vision transformers are the default backbone for most large-scale computer vision systems. This article covers the core vision transformer architecture — patch embedding, multi head self attention, and the transformer encoder — compares vision transformers to convolutional neural networks, surveys key model variants and benchmarks, and explains where the technology stands today.


## 2. How Vision Transformers Work


Vision transformers adapt the transformer architecture — originally designed for sequential data in natural language processing — to process images. Rather than sliding convolutional kernels across pixel values, vision transformers divide an input image into fixed size patches, embed each as a vector, and feed the resulting input sequence into a standard transformer encoder.


### 2.1 Step-by-Step Breakdown


**Image Patching:** The input image is divided into non overlapping patches of P×P pixels. For a 224×224 image with P=16, this yields 196 patches. Each patch is flattened into a single vector — these flattened image patches form the basis of the vision transformer ViT input.


**Patch Embedding:** Each flattened patch is projected through a single linear layer to a fixed embedding dimension — the patch embedding step maps flattened image patches to lower dimensional linear embeddings (e.g., D=768 for ViT-Base). Patch embedding plays the same role as word embeddings in natural language processing: transforming raw input data into a representation the transformer encoder can process.


**Positional Encodings:** The transformer encoder has no built-in spatial structure, so positional encodings are added to patch embedding vectors to encode each patch's position across its spatial dimensions into the input sequence.


**Classification Token:** A learnable classification token (CLS token) is prepended to the input sequence. After the transformer encoder, the CLS token aggregates information from all patches as a single vector for image classification.


**Transformer Encoder:** The full input sequence passes through multiple encoder blocks of multi head attention and feed-forward networks to capture long range dependencies across the entire image.


**Classification Head:** The final encoder block's CLS output passes through a single linear layer that directly predicts class labels.


**Figure:**[ViT Architecture.](https://github.com/google-research/vision_transformer)


### 2.2 Vision Transformers vs. Convolutional Neural Networks


Vision transformers and convolutional neural networks differ fundamentally in how they extract features from an image.


Feature Vision Transformer (ViT) CNN


Feature extraction Attention (global) Convolutions (local)


Inductive biases Minimal Translation equivariance, locality


Data requirements High (large scale pre training) Lower (works from scratch)


Scalability Strong More limited


Parallelism Fully parallel across patches Sequential through depth


Traditional convolutional neural networks have a strong inductive bias towards locality and translation invariance, making them effective for small datasets. Vision transformers exhibit minimal inductive biases, making them more data-hungry — but they scale more effectively. When trained on 300 million images, vision transformer ViT models outperform traditional convolutional neural networks like ResNets by a significant margin.


**Figure: Side-by-side comparison of CNN local receptive field vs. vision transformer global attention across an image**


## 3. Key Components of Vision Transformers


The vision transformer architecture consists of several key components: image patching and patch embedding, positional encoding, a learnable classification token, and a multi-layer transformer encoder. Understanding each component explains why vision transformers work for so many computer vision tasks and why the vision transformer ViT model represents a fundamental shift in deep learning for image analysis.


### 3.1 Image Patches and Patch Embedding


For a patch size P, an image H×W×C yields N = (H/P)×(W/P) patches. Each is flattened to length P²C (flattened image patches) and projected to embedding dimension D via patch embedding — mapping flattened image patches to lower dimensional linear embeddings encoding both content and spatial dimensions. The full input sequence fed to the transformer encoder is:


z₀ = \[x_cls; x₁E; x₂E; ...; xₙE\] + E_pos


**Figure:**[Pass the embeddings through the Linear layer and add pos encodings.](https://gaussian37.github.io/dl-concept-vit/)


### 3.2 Self Attention Mechanism


The self attention mechanism is the core operation that enables vision transformers to model relationships across all image patches simultaneously. For each self attention layer, the input data — patch embedding vectors — are projected to query (Q), key (K), and value (V) matrices. The attention weights are computed as:


Attention(Q, K, V) = softmax(QKᵀ / √d_k) · V


Scaling by √d_k prevents attention weights from growing large and saturating the softmax. The self attention mechanism uses these attention weights to produce a weighted sum of value vectors, where higher attention weights indicate stronger relationships between patches.


Multi-head attention runs h parallel attention heads with separate projection matrices, concatenates their output tokens, and projects back to the model dimension. Multiple heads allow the self attention mechanism to capture different types of relationships simultaneously — one self attention layer may focus on local texture, another on global layout, another on object-background relationships.


Unlike convolutional neural networks, which capture long range dependencies by stacking multiple layers with expanding receptive fields, vision transformers use attention to capture long range dependencies directly in every encoder block. This is a primary reason why vision transformers outperform CNNs on many computer vision tasks at scale.


### 3.3 Transformer Encoder Block


Each encoder block in the vision transformer ViT contains:


- **Pre-norm Layer Normalization:** Applied before the self attention layer and feed forward layer — standard in modern vision transformer models.
- **Multi-Head attention:** Computes attention weights and output tokens across all image patches.
- **Residual connections:** Wrap both the self attention layer and the feed-forward network, allowing gradients to flow through multiple layers without vanishing.
- **Feed Forward Layer:** Two fully connected layers with a Gaussian Error Linear Unit (GELU) activation applied to each token. These fully connected layers expand the model dimension (typically 4×) then project it back down.


Layer normalization and residual connections are essential to stable training; these residual connections ensure gradients flow cleanly through multiple encoder blocks. A ViT-Base vision transformer model has 12 encoder blocks; ViT-Large has 24; ViT-Huge has 32.


### 3.4 Output Tokens and Multi-Modal Extensions


For image classification tasks, the output tokens from the CLS position after the final encoder block pass through a linear layer to produce class predictions. Output tokens from all patch positions serve as feature maps for dense prediction tasks — image segmentation, object detection — where they feed into task-specific decoder heads.


Vision transformer models extend to multi-model tasks: CLIP trains a vision transformer alongside a text encoder for zero-shot image classification tasks, while large vision transformer encoders now power visual question answering, visual grounding, and visual reasoning systems.


### 3.5 Attention Maps and Interpretability


The attention weights in each self attention layer can be visualized as attention maps showing which image patches each token attends to. In well-trained vision transformers, these maps highlight semantically meaningful regions — foreground objects, faces, structural boundaries — without explicit supervision. DINOv2's self attention maps produce high-quality unsupervised image segmentation masks, demonstrating that vision transformers learn spatially structured representations from image analysis alone — a key advantage over CNNs.


ViTs can visualize attention through attention maps, enhancing interpretability compared to CNNs. However, attention weights do not always fully explain model predictions, and interpretability research for vision transformer models remains an active area of machine learning.


**Figure: attention map visualizing how a vision transformer highlights foreground objects**


## 4. Performance Benchmarks and Key Models


### 4.1 Original Vision Transformer ViT Results


When the vision transformer ViT was published (ICLR 2021), its central finding was that a standard transformer encoder applied to image patches — with no convolutional components — could achieve state of the art image recognition results with sufficient pre-training data. A ViT-L/16 pre trained on JFT-300M (300 million images) achieved 88.5% top-1 accuracy on ImageNet-1K, comparable to EfficientNet-L2, while requiring roughly 4× less compute to train.


The performance of vision transformers improves significantly when trained on larger datasets such as 300 million images, where ViT models outperform traditional CNNs like ResNets by a significant margin. On smaller datasets, however, ViT models initially underperformed CNNs — a practical barrier that subsequent research directly addressed.


### 4.2 Key Vision Transformer Variants


**DeiT (2021)** — Vision transformer ViT models trained on ImageNet-1K from scratch reach ~85% top-1 accuracy with RandAugment, Mixup, CutMix, stochastic depth, and CNN teacher distillation, making vision transformers accessible without large proprietary datasets.


**Swin Transformer (ICCV 2021)** — Hierarchical vision transformer ViT processing image patches in shifted local windows, producing multi-resolution feature maps for various vision tasks. Achieved 87.3% on ImageNet-1K and strong COCO object detection results.


**Swin Transformer V2 (CVPR 2022)** — Scaled to 3 billion parameters with post-norm, scaled cosine attention, and log-spaced positional bias. Set state of the art on ImageNet-V2 (84.0%), COCO object detection (63.1 box mAP), ADE20K image segmentation (59.9 mIoU), and Kinetics-400 (86.8%).


**DINOv2 (2023)** — Meta AI trained a vision transformer ViT on 142 million curated images without labels. DINOv2 features transfer directly to image classification tasks, image segmentation, and depth estimation without fine-tuning, and its feature maps have become a standard backbone for downstream computer vision applications.


**SAM2 (2024)** — Uses a vision transformer ViT backbone for promptable image segmentation and video object tracking. Extends promptable segmentation to video data; SAM 2.1 received an ICLR 2025 Outstanding Paper Award.


**DiT (ICCV 2023)** — Replaced the U-Net backbone in latent diffusion models with a patch-based vision transformer, yielding better generative modeling scaling. DiT now underpins leading text-to-image and text-to-video systems.


### 4.3 Current State (2025-2026)


Vision transformer ViT models have continued to scale across many vision tasks. InternViT-6B is a 6-billion parameter vision encoder used as the visual backbone in multimodal language models. Recent architectural studies have benchmarked modern vision transformer design choices including Rotary Positional Embeddings (RoPE), QK normalization, and register tokens — additional output tokens that absorb attention noise in large vision transformer models.


At the efficiency end, a 2025 survey found that compressed ViT models can reduce edge inference energy by up to 53% with minimal accuracy loss, enabling vision processing tasks on resource-constrained hardware. Pre-trained ViT models in timm and Hugging Face Transformers make it straightforward to apply leading vision transformers to new computer vision applications.


**Figure: Timeline of major vision transformer milestones from 2021 to 2025: original ViT, DeiT, Swin, MAE, DINOv2, SAM2, and DiT**


## 5. Pre-Training and Fine-Tuning Vision Transformers


### 5.1 Why Pre-Training Matters


Vision transformers require more data than CNNs (CNNs) due to their lower inductive bias — they must learn spatial structure from input data rather than having it built into the model architecture. This makes them more data-hungry for effective training, but it also means they scale more efficiently: given enough input data, vision transformer models absorb more information per parameter than traditional CNNs.


Training vision transformers effectively often involves pre-training on large datasets followed by fine-tuning on smaller, task-specific datasets. ViTs demonstrate higher resilience to image distortions and occlusions after pre-training, particularly in specialized computer vision tasks. Vision transformers generally provide high-quality representations that are beneficial for downstream tasks, including self-supervised learning applications.


### 5.2 Supervised Pre-Training


Large-scale supervised pre-training on labeled datasets (ImageNet-21K, or proprietary datasets of hundreds of millions of images) followed by fine-tuning on target computer vision tasks is the standard approach. Positional encodings in the embedding can be bicubically interpolated to handle higher resolution images during fine-tuning. Pre-trained ViT models from timm and Hugging Face Transformers provide a strong starting point for most computer vision applications.


### 5.3 Self-Supervised Pre-Training


When labeled input data is scarce, self-supervised methods learn representations from unlabeled images for various vision tasks.


**MAE (Masked Autoencoders)** — Masks 75% of patches in the input data and trains the vision transformer encoder to reconstruct the masked regions. Only unmasked patches pass through the full transformer encoder, making pre-training computationally efficient.


**DINO / DINOv2** — A student vision transformer is trained to match the output tokens of a momentum teacher with no labels. DINOv2 features match or exceed supervised representations across many computer vision tasks without fine-tuning, demonstrating that the self attention mechanism can learn semantically rich spatial structure purely from image data processing.


**CLIP** — Paired image-text input data trains a vision transformer alongside a text transformer model using contrastive learning. The resulting vision transformer model supports zero-shot image classification tasks, image-text retrieval, and transfer to new computer vision applications through natural language prompting.


### 5.4 Fine-Tuning Best Practices


Apply layer-wise learning rate decay, AdamW with cosine scheduling and warmup, and data augmentation (RandAugment, Mixup, CutMix). Gradient checkpointing and mixed-precision (BF16) reduce memory demands. FlashAttention rewrites the self attention layer kernel to minimize memory reads/writes, substantially reducing runtime for vision transformer model training. Start from pre trained ViT models from timm or Hugging Face Transformers whenever possible.


## 6. Applications of Vision Transformers


Vision transformers are applied across many vision tasks, from single-image analysis to video processing and multi-model tasks combining visual and textual input data.


### Image Classification


Vision transformers excel in image classification tasks by categorizing images into predefined classes through their self attention mechanisms, which learn intricate patterns and relationships within the images. ViT models now achieve over 90% top-1 accuracy on ImageNet-1K at large scale, and their robustness to distribution shift consistently exceeds CNNs of similar size across real-world computer vision applications.


### Object Detection


In object detection, vision transformers are effective at not only classifying objects within images but also localizing their positions, suitable for autonomous driving and surveillance. Key architectures include DETR (transformer encoder-decoder for end-to-end object detection), ViTDet (plain vision transformer backbone with simple feature pyramids), and Swin-based detectors widely used on COCO for detection and instance segmentation across many vision tasks.


### segmentation


Vision transformers are applied in segmentation to accurately delineate object boundaries — particularly valuable in medical imaging for diagnosing diseases. SAM2 uses a vision transformer encoder with a memory-based decoder for promptable segmentation and video object tracking. SETR and Segmenter replaced CNN encoders with vision transformer backbones for semantic segmentation across many vision tasks.


### Video Processing


Vision transformers adapted for video data include TimeSformer (space-time attention applied to frame patches), ViViT (factorized spatial-temporal transformer), and VideoMAE (self-supervised pre-training on video clips). These models achieve strong results on action recognition — classifying human actions in videos and capturing temporal long range dependencies — with video processing applications spanning surveillance and human-computer interaction.


### Generative Modeling


Diffusion Transformers (DiT) replaced convolutional U-Nets in latent diffusion models with patch-based vision transformers, yielding better generative modeling scaling. DiT underpins leading text-to-image and text-to-video generation systems.


### Multimodal Vision Transformers


Vision transformers excel in multi-model tasks combining visual and textual information — visual grounding, visual question answering, and visual reasoning. CLIP-trained vision transformers enable zero-shot image classification tasks across novel categories. Large Vision-Language Models (LVLMs) including LLaVA, InternVL, and Qwen-VL use vision transformer encoders for visual question answering, visual grounding, and visual reasoning.


### Medical Imaging


ViTs are increasingly utilized in medical imaging to detect anomalies and assist in diagnosis by analyzing CT and MRI scans. The global self attention mechanism captures dependencies in anatomical structure that CNNs miss. Vision transformers are applied in segmentation of organ and tissue boundaries with strong results in radiology, histopathology, and retinal imaging.


## 7. Challenges of Vision Transformers


**Data requirements:** Vision transformers require more data than CNNs due to their lower inductive bias, making them more data-hungry for effective training from scratch. Self-supervised pre-training (MAE, DINOv2) and knowledge distillation (DeiT) significantly reduce this barrier, but pre-training on large datasets remains standard for most computer vision tasks.


**Quadratic attention complexity:** The self attention mechanism scales as O(N²) with the number of input tokens, limiting applicability to higher resolution images and long video data. Solutions include windowed attention (Swin), FlashAttention (IO-efficient self attention layer computation), and token pruning approaches that drop low-importance output tokens in deeper encoder blocks to reduce data processing overhead.


**Edge deployment:** Full vision transformer models are memory- and compute-intensive. Model compression — token pruning, knowledge distillation to smaller ViT models, quantization — has made vision processing tasks feasible on edge hardware. A 2025 survey reports compressed ViT models can reduce edge energy by up to 53% with minimal accuracy degradation across many vision tasks.


**Lack of inductive biases:** Vision transformers do not assume locality or translation equivariance. This is a strength for large-scale computer vision tasks and a limitation for data-limited regimes. Hybrid architectures that add a convolutional stem to the embedding step recover some efficiency in low-data settings.


**Interpretability:** While attention maps from each self attention layer provide qualitative insight, attention weights are not always reliable explanations of what drives predictions. Research into vision transformer interpretability remains an active area of machine learning.


**Figure: Comparison of interpretability methods between architectures. The CNN (left) uses Grad-CAM to produce a coarse, gradient-based heatmap of general interest areas. In contrast, the ViT (right) leverages Self-Attention to provide sharp, patch-aligned visualizations that pinpoint specific semantic features like the eyes and nose.**


## 8. Getting Started with Vision Transformers: Code Example


```text
# Installation
# pip install transformers pillow requests


from   transformers   import   ViTForImageClassification, ViTImageProcessor
from   PIL   import   Image
import   requests


model_name =   "google/vit-base-patch16-224"


processor = ViTImageProcessor.from_pretrained(model_name)
model = ViTForImageClassification.from_pretrained(model_name)


url =   "https://upload.wikimedia.org/wikipedia/commons/4/4f/Felis_silvestris_catus_lying_on_rice_straw.jpg"
image = Image.open(requests.get(url, stream=True).raw)


inputs = processor(images=image, return_tensors=  "pt"  )
outputs = model(**inputs)


predicted_class = outputs.logits.argmax(-  1  ).item()
print(model.config.id2label[predicted_class])
```


The \`google/vit-base-patch16-224\` checkpoint is a vision transformer ViT Base model with 16×16 patch size, pre trained on ImageNet-21K and fine-tuned on 1,000 image classification classes from ImageNet-1K.


**Further Resources**


- **timm (PyTorch Image Models)** — \`pip install timm\` — hundreds of pre trained ViT models, Swin Transformers, and DeiT variants. \`timm.create_model('vit_base_patch16_224', pretrained=True)\` is the fastest path to a fine-tunable feature extractor for vision tasks.
- **Hugging Face Transformers** — Standardized APIs, training utilities, and model cards for vision transformer ViT models and variants.
- For self-supervised pre-training on domain-specific unlabeled input data, see the Lightly section below.


## 9. Conclusion


Vision transformers have moved from a research proposal to the foundational architecture of modern computer vision. Starting from the original vision transformer ViT (ICLR 2021), the field has produced hierarchical variants like Swin Transformer for dense prediction vision tasks, self-supervised methods like DINOv2 that eliminate the need for labels, and foundation models like SAM2, CLIP, and DiT that deliver strong performance across many vision tasks.


The core strengths of vision transformers — global attention, strong scaling behavior, and architectural flexibility across multi-model tasks — make them well-suited to the direction machine learning and computer vision are heading: larger pre trained models, broader task coverage, and less task-specific engineering. The main practical challenges — data requirements, quadratic attention complexity, and edge deployment cost — each have active solutions.


For practitioners, the standard path starts with pre trained ViT architectures from timm or Hugging Face, fine-tuned on domain-specific data for target tasks. When labeled data is limited, self-supervised pre-training on your own unlabeled images using LightlyTrain can meaningfully improve performance across many vision tasks with fewer annotations.


## See Lightly in Action


Assembling the right training data for vision transformers is often the real bottleneck — not model architecture.


**LightlyStudio** is a unified data curation and labeling platform for image and video workflows. It uses embedding-based selection to surface the most informative training samples, supports active learning loops, near-duplicate detection, annotation and QA, and multi-dataset management with on-prem deployment. The open-source core is free to use locally.


**LightlyTrain** is a framework for self-supervised pretraining, fine-tuning, distillation, and autolabeling on domain-specific visual data without labels. It supports vision transformer ViT architectures, YOLO (v8, v11), RT-DETR, DINOv3, ResNet, and other architectures for various vision tasks. The vision transformer model learns visual representations from raw images in the pretraining phase, then fine-tunes to image classification tasks, detection, or segmentation with fewer labeled examples.


Get started today.
