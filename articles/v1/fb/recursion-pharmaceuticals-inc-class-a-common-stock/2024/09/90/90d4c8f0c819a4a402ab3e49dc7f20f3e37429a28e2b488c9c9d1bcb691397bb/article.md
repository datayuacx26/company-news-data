---
schema_version: "1.0.0"
document_id: "90d4c8f0c819a4a402ab3e49dc7f20f3e37429a28e2b488c9c9d1bcb691397bb"
company_key: "recursion-pharmaceuticals-inc-class-a-common-stock"
company: "Recursion Pharmaceuticals Inc."
source_id: "recursion-pharmaceuticals-inc-class-a-common-stock-news-import-f0ced5d1ab91"
canonical_url: "https://www.recursion.com/news/how-phenomic-foundation-models-are-empowering-the-brightfield-comeback"
published_at: "2024-09-11T00:00:00+00:00"
first_seen_at: "2026-07-22T11:05:14.614366+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:9a4872a3f79b9fb62631f974b816961c55645dd927e79c86853c8204d502aafe"
---

# How Phenomic Foundation Models Are Empowering the Brightfield Comeback

Images produced by brightfield microscopy – a method of detecting cells in samples using light that was first developed in the 17th century – look like low contrast blobs and dots to the human eye. Even widely-used image analysis software like CellProfiler struggle to identify cells in these types of images. Now, Recursion has demonstrated that our recently developed series of foundation models, called Phenom, can extract nearly the same amount of biological insights from brightfield images as their CellPaint counterparts – a breakthrough that has the potential to bring increased speed and efficiency to our AI drug discovery workflow.


### **What is Phenom?**


At the beginning of the year, Recursion released[Phenom-Beta](https://www.recursion.com/news/nothing-short-of-phenomenal-new-deep-learning-model-available-on-nvidias-bionemo-platform) , a phenomics foundation model built on NVIDIA’s BioNeMo platform. We also presented a[highlighted paper](https://cvpr.thecvf.com/virtual/2024/poster/31565) describing the model at the Computer Vision and Pattern Recognition conference in Seattle. This series of Phenom models was trained using the vision transformer (ViT) architecture, which is inspired by the deep learning architectures that have driven groundbreaking progress in large language models. Unlike previous attempts to train deep learning models to featurize microscopy images that rely on supervised transfer learning, we leverage self-supervised learning, specifically masked autoencoders (MAEs). These models are trained by masking out a large portion of pixels in input images, typically 75% or more, and optimizing the model to reconstruct the missing regions. This label-free approach is much less sensitive to the dataset used in previous state-of-the-art supervised models, and we’ve proven that it continues to scale with datasets that contain nearly 100 million microscopy images and with ViTs with over 1.8 billion parameters!


Visualizing MAE ViT reconstructions on random validation images from four datasets – RxRx1, RxRx3, RPI-52M, and RPI-93M. For each dataset column, we show a triplet of the masked input (left), the reconstruction (middle), and the original (right); for this model, we randomly mask 75% of the 1,024 8x8 patches constructed from the 256 x 256 center crop of the full image. Images are taken from wells on the same experimental plate, rows alternate between randomly sampled negative control and perturbation conditions


### **Training Phenom on Brightfield Images**


Recently, we’ve demonstrated that these models can be adapted to brightfield images by fine-tuning them on a larger set of brightfield experiments done at Recursion. This model can capture signals from ~95% of the genetic perturbations captured with CellPaint and identifies ~94% of the relationships between perturbations identified with our best CellPaint ViT. Additionally, we recently fine-tuned a shallow ViT-based decoder on top of a frozen version of our best Phenom encoder to predict CellPaint images from brightfield images using a set of experiments that were imaged with both modalities. In other words, our foundation model can perform in-silico cellular organelle fluorescent staining on brightfield images.


This model can even accurately predict CellPaint images for cell types it wasn’t trained on (see figure 1)! We can also pass these generated CellPaint images through the same frozen MAE encoder (Recursively 😂) (see figure 2) and compare relationships between perturbations in a small CRISPR KO experiment. Here we see that the relationships between KOs are maintained between using the original CellPaint images, or the CellPaint images generated from brightfield inputs (see figure 3).


Together, these results indicate that Phenom models leveraging brightfield images capture nearly all the biological patterns that can be extracted from CellPaint experiments, while unlocking incredible experimental capabilities, like imaging wells over time, or capturing additional read outs, like transcriptomics, from the same wells post imaging.


Authors:[Charles Baker](https://www.linkedin.com/in/charlesjbaker/) ,[Oren Kraus](https://www.linkedin.com/in/oren-kraus-5a681b69/) &[Kian Kenyon-Dean](https://www.linkedin.com/in/kian-kenyon-dean-a7961a171/)


Figure 1: Prediction of CellPaint from Brightfield.


Figure 2: Featurizing predicted CellPaint images with Phenom models.


Figure 3: Relationship between CRISPR KOs for original CellPaint and predicted CellPaint images.


‍ *‍*


*Authors: Charles Baker, Oren Kraus and Kian Kenyon-Dean*
