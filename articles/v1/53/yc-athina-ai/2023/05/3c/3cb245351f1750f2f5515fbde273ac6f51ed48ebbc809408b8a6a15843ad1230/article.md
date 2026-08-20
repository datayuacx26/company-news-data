---
schema_version: "1.0.0"
document_id: "3cb245351f1750f2f5515fbde273ac6f51ed48ebbc809408b8a6a15843ad1230"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-icm-a-unified-framework-towards-image-coding-for-machines-with-task-driven-prompts"
published_at: "2023-05-04T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:4da9ee4cd7aac1524d368bd8008e2d512be0910d6fad8d2ed337feffe2febbd3"
---

# Prompt-ICM: A Unified Framework towards Image Coding for Machines with Task-driven Prompts

Do not index


Original Paper


[https://arxiv.org/abs/2305.02578](https://arxiv.org/abs/2305.02578)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2305.02578](https://arxiv.org/abs/2305.02578)


**By:**[Ruoyu Feng](https://arxiv.org/search/cs?searchtype=author&query=Feng%2C%20R) ,[Jinming Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20J) ,[Xin Jin](https://arxiv.org/search/cs?searchtype=author&query=Jin%2C%20X) ,[Xiaohan Pan](https://arxiv.org/search/cs?searchtype=author&query=Pan%2C%20X) ,[Heming Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20H) ,[Zhibo Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20Z)


**Abstract:**


> Image coding for machines (ICM) aims to compress images to support downstream AI analysis instead of human perception. For ICM, developing a unified codec to reduce information redundancy while empowering the compressed features to support various vision tasks is very important, which inevitably faces two core challenges: 1) How should the compression strategy be adjusted based on the downstream tasks? 2) How to well adapt the compressed features to different downstream tasks? Inspired by recent advances in transferring large-scale pre-trained models to downstream tasks via prompting, in this work, we explore a new ICM framework, termed Prompt-ICM. To address both challenges by carefully learning task-driven prompts to coordinate well the compression process and downstream analysis. Specifically, our method is composed of two core designs: a) compression prompts, which are implemented as importance maps predicted by an information selector, and used to achieve different content-weighted bit allocations during compression according to different downstream tasks; b) task-adaptive prompts, which are instantiated as a few learnable parameters specifically for tuning compressed features for the specific intelligent task. Extensive experiments demonstrate that with a single feature codec and a few extra parameters, our proposed framework could efficiently support different kinds of intelligent tasks with much higher coding efficiency.


---


###


Summary Notes


##


Enhancing AI Efficiency: The Innovative Prompt-ICM Framework for Image Coding


###


Introduction


Efficiently processing images for AI and machine learning tasks, as opposed to human viewing, poses a significant challenge.


Traditional image compression methods, designed with human viewers in mind, often do not meet the needs of machine-based tasks.


This has led to the development of Image Coding for Machines (ICM), a method focused on optimizing images for machine interpretation and efficiency.


A groundbreaking approach within this field is Prompt-ICM, which uses task-driven prompts to transform how images are compressed and processed for AI tasks.


###


Understanding the Framework


ICM aims to tailor image compression to improve machine task performance, which can be categorized into:


- **Task-specific Image Compression** : Compresses images for particular tasks but lacks versatility.


- **Feature-based ICM** : Compresses specific task features, offering some optimization but not task-specific.


- **General Feature-based ICM** : Uses a general feature extractor for all tasks but isn't optimized for specific tasks.


- **Prompt-ICM (Proposed)** : Enhances general feature-based ICM by employing task-driven prompts to direct the compression and feature adaptation for specific tasks, making it a versatile and efficient approach.


###


Techniques Behind Prompt-ICM


Prompt-ICM introduces two key innovations:


1. **Compression Prompts** : Importance maps predicted by an information selector that guide the compression process, ensuring vital information for a task is prioritized.


1. **Task-adaptive Prompts** : Learnable parameters that fine-tune compressed features for specific tasks, enhancing performance efficiently.


###


Experimental Insights


Prompt-ICM has been rigorously tested and has shown exceptional efficiency in various intelligent tasks like classification and segmentation.


It outperforms both traditional and learned codecs in rate-distortion efficiency, proving its effectiveness in compressing images for machine tasks.


###


Key Contributions


Prompt-ICM stands out for several reasons:


1. **Unified Framework** : It's the first to combine image compression and task analysis in a single ICM framework.


1. **Compression Prompts** : Introduces importance maps for content-weighted compression aligned with task needs.


1. **Task-adaptive Prompt Tuning** : Offers a new way to adjust compressed features for tasks, significantly boosting performance with minimal parameter increase.


###


Background Work


Prompt-ICM builds on extensive research in image compression and ICM, from traditional codecs to those optimized for perceptual quality, and the emerging focus on machine-specific image compression.


It also leverages Parameter Efficient Tuning (PET) concepts, crucial for developing the task-adaptive prompts.


###


Conclusion


Prompt-ICM marks a significant advancement in machine-specific image coding, adeptly meeting the challenges of adapting compression for various AI tasks with high efficiency.


By utilizing task-driven prompts, it enhances both the compression process and the quality of features for downstream tasks. As AI applications become more complex, Prompt-ICM's flexibility and efficiency will be invaluable for AI engineers.


###


Figures and Results


- **Figure 1** : Visual comparison of ICM pipelines, showcasing Prompt-ICM's integration of task-driven prompts.


- **Experimental Results** : Highlight Prompt-ICM's superior performance in optimizing image coding for machine tasks across different tasks and datasets.


Prompt-ICM not only streamlines image processing for AI applications but also fosters task-specific optimization, leading to a new era in machine learning characterized by adaptability and efficiency.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
