---
schema_version: "1.0.0"
document_id: "c703769c0d1fdb5ad44da6c73736c3f685aec120de5a7975e574cd0f11ff5ae9"
company_key: "ge-healthcare-technologies-inc-common-stock"
company: "GE HealthCare Technologies Inc."
source_id: "ge-healthcare-technologies-inc-common-stock-rss-f82d3b59c758"
canonical_url: "https://research.gehealthcare.com/patient-care-pathways/decipher-mr-a-vision-language-foundation-model-for-3d-mri-representations/"
published_at: "2026-04-14T23:53:06+00:00"
first_seen_at: "2026-07-20T03:33:19.174351+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:18e5cf1b17878742844a7a4610d040e6812aaeeb4f398f78e35f035d2f5220c0"
---

# Decipher-MR: A Vision-Language Foundation Model for 3D MRI Representations

Magnetic Resonance Imaging is a critical imaging modality in clinical diagnosis and research, yet its complexity and heterogeneity hinder scalable, generalizable machine learning. Although foundation models have revolutionized language and vision tasks, their application to MRI remains constrained by data scarcity and narrow anatomical focus.


We present Decipher-MR, a 3D MRI-specific vision-language foundation model trained on 200,000 MRI series from over 22,000 studies spanning diverse anatomical regions, sequences, and pathologies. Decipher-MR integrates self-supervised vision learning with report-guided text supervision to build robust representations for broad applications. To enable efficient use, Decipher-MR supports a modular design that enables tuning of lightweight, task-specific decoders attached to a frozen pretrained encoder. Following this setting, we evaluate Decipher-MR across disease classification, demographic prediction, anatomical localization, and cross-modal retrieval, demonstrating consistent improvements over existing foundation models and task-specific approaches. These results position Decipher-MR as a versatile foundation for MRI-based AI in clinical and research settings.


[Read the paper.](https://arxiv.org/pdf/2509.21249)
