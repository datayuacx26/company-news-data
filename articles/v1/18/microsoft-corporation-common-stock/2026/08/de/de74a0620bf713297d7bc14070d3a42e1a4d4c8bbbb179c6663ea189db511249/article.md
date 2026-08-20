---
schema_version: "1.0.0"
document_id: "de74a0620bf713297d7bc14070d3a42e1a4d4c8bbbb179c6663ea189db511249"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0524baf4a828"
canonical_url: "https://news.microsoft.com/signal/articles/teaching-ai-to-speak-the-language-of-pathology/"
published_at: "2026-08-04T13:38:55+00:00"
first_seen_at: "2026-08-04T15:11:24.569795+00:00"
fetched_at: "2026-08-04T15:13:53.558824+00:00"
content_hash: "sha256:20f849cd86cc3a5e988ce2e287e68563ce105668a50307e56f99d1c618c93fc6"
---

# Teaching AI to speak the language of pathology

Artificial intelligence is already helping clinicians spot patterns in diagnostic images and detect signs of disease. But many of today’s pathology AI systems are built for a single task and must be rebuilt for each new application.


A study[recently published in Nature Medicine](https://www.nature.com/articles/s41591-026-04521-4) describes a different approach aimed at advancing research in this field. Researchers from Microsoft Research and Paige, now part of Tempus, developed PRISM2, a pathology foundation model trained on both tissue images and language based on real pathology reports. The goal was to help AI learn from the large amount of available text data and create a system that could support future pathology research and development.


In testing, the model matched or exceeded the performance of specialized cancer-detection systems on several benchmark tasks, including prostate cancer, breast cancer and breast lymph node metastasis detection. Importantly, researchers reported these results without creating a separate model for each task.


The full PRISM2 model weights are publicly available for research use on Hugging Face[here](https://huggingface.co/paige-ai/Prism2) and[here](https://huggingface.co/paige-ai/Prism2-survival) .


**Why it matters**


Pathology sits at the center of many cancer diagnoses. Pathologists examine tissue samples and create reports that guide treatment decisions. As healthcare generates larger volumes of data, researchers have been exploring how AI can support clinicians in making these decisions and possibly even deliver new insights.


Most pathology AI systems today are designed for a single purpose, such as detecting a particular type of cancer. PRISM2 was developed as a research effort to support a broader range of tasks through a single model that can respond to prompts and questions. Researchers say that could make it easier to build and adapt future pathology tools without starting from scratch for each new application.


**How it works**


The central idea behind PRISM2 is that pathology is not only a visual discipline. It is also a language-driven one.


To train the model, researchers paired pathology images with information derived from pathology reports, creating millions of question-and-answer examples that helped connect visual findings with diagnostic language. The resulting model can work with either images only or images and text, enabling users to interact with it through prompts rather than relying solely on task-specific software.


This approach differs from many earlier pathology foundation models, which focused primarily on learning visual representations from images. PRISM2 was designed from the outset to link visual patterns in tissue with the language clinicians use when making diagnoses.


*Lead image created with Microsoft Copilot.*
