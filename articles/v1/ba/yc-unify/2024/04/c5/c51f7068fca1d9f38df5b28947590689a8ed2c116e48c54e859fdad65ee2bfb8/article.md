---
schema_version: "1.0.0"
document_id: "c51f7068fca1d9f38df5b28947590689a8ed2c116e48c54e859fdad65ee2bfb8"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-22"
published_at: "2024-04-25T16:30:28+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:f6ed77ce37b77f157766aa893f93e8a3bf1d30ad49c86dedf2b194a927dfb16c"
---

# The Deep Dive Issue #22

# The Deep Dive Issue #22


### Torchtune, optimal configurations, and AI for science


[Nassim](https://substack.com/@nas07)


Apr 25, 2024


It may not be long until we start seeing perfectly life-like avatars generated on the fly. Microsoft's latest


[VASA-1](https://www.microsoft.com/en-us/research/project/vasa-1/) research is giving us a glimpse of that future by generating video clips from just single images and audio, with natural (well, if you put aside the constant head wobbling) and modular face expressions and head movements! The tech itself isn't super new and it's still not flawless, but they really took this to the next level with some clips you could easily mistake with real footage.


While this won't be available to the general public until they are "certain the technology will be used responsibly" (not sure how you can guarantee that 🤷), it's still pretty cool to think of all the practical use-cases. If anything, making interactions with AI a bit less mechanical. About time we move away from chat interfaces.


### **Tuning beacon**


Streamlined LLM fine-tuning has found its way into PyTorch, the (arguably, I see you TensorFlow folks) most popular machine learning framework. Torchtune lets you easily tune, tweak and test custom LLMs in a breeze.


**Why would you care?** - If you've been training LLMs, chances are you've had to rely on third party tools for fine-tuning.'With torchtune you can do that all in native PyTorch, plus it's nicely integrated with HuggingFace and other model zoos.


**How does it work?** - Fine-tuning with torchtune revolves around


*recipes* , which are training pipelines you define ahead of time that lets you specify the model to train, the training method, and potential extra steps in the process. Recipes include:


-


**Configurable parameters** : That you can specify in a YAML file or via the CLI,


-


**A recipe script** : That handles the workflow for setting up the environment, passing parameters, etc., and


-


**A recipe class** : That contains all the rules and instructions dictating how the model should be fine-tuned.


A typical workflow with torchtune would then consist of:


1.


Downloading a pretrained model, through the HuggingFace Hub for e.g


2.


Selecting (and potentially tweaking) an existing recipe, depending on your memory, device, and performance requirements


3.


Modifying the config file to specify the values for the training parameters as per your needs.


4.


Training the model with a simple CLI command


5.


(Optional) Going out for a short walk as training finishes


Practically, this looks like:


```text
# Downloading a model
>>> tune download \
mistralai/Mistral-7B-v0.1 \                         # Or any other model
--output-dir <checkpoint_dir> \
--hf-token <ACCESS TOKEN>


# Tuning with any recipe from https://github.com/pytorch/torchtune/tree/main/recipes
>>> tune run lora_finetune_single_device \
--config llama2/7B_lora_single_device \             # Or other configs
checkpointer.checkpoint_dir=<checkpoint_dir> \
tokenizer.path=<checkpoint_dir>/tokenizer.model \
checkpointer.output_dir=<checkpoint_dir>
```


Check out the


[repository](https://github.com/pytorch/torchtune) to get started.


### **The Lab**


#### **Path of least consumption**


LLM fine-tuning is computationally intensive and tuning methods have different compute requirements.


To facilitate the selection of fine-tuning methods,


[LLMem](https://arxiv.org/pdf/2404.10933.pdf) proposes to estimate the memory consumption across various distributed fine-tuning methods, such as data parallelism and tensor parallelism. LLMem analyzes the fundamental structure of transformer-based decoder models and the memory usage patterns of different methods to provide accurate estimations.


It takes into account factors like memory chunk management for parameter sharing, memory allocation differences between the transformer and language modeling head, and temporary buffer usage during computation. By considering these aspects, LLMem can effectively predict the peak GPU memory usage for LLM fine-tuning on both single and multiple GPUs.


LLMem demonstrates high accuracy in estimating peak GPU memory usage, with error rates as low as 1.6% on single GPUs and an average error rate of 3.0% for LLMs with over a billion parameters on multi-GPU configurations.


#### **At-home touristing**


Personalizing text-to-image models while preserving the original model's ability to generate diverse and creative content, especially with multiple subjects, remains a significant challenge.


Inspired by the Mixture-of-Experts mechanism in LLMs,


[Mixture-of-Attention (MoA)](https://arxiv.org/pdf/2404.11565.pdf) is a novel architecture that attempts to address this complexity. MoA works by splitting the image generation process into two separate pathways: a "prior" branch and a "personalized" branch. The prior branch utilizes the original, unmodified model and focuses on generating the background and context of the image. Meanwhile, the personalized branch is trained to learn and embed specific subjects into the image based on input images.


A "router" mechanism then intelligently combines the outputs of both branches, ensuring that the personalized elements are seamlessly integrated into the generated scene. This approach allows MoA to personalize images with multiple subjects while preserving the original model's ability to generate diverse and creative content. MoA is able to handle occlusions, generate images with close interactions between subjects, and maintain consistency in multi-subject compositions.


MoA successfully personalizes text-to-image models with multiple subjects while preserving the original model's capacity for diverse and creative content generation, offering disentangled control over subjects and context within a single image generation process.


#### **Sweet-spot**


Prompt engineering has gotten large attention as a means to optimize the output of LLMs, but it remains an inefficient trial-and-error approach given the sensitivity of models to small changes.


Instead,


[position engineering](https://arxiv.org/pdf/2404.11216.pdf) focuses solely on manipulating the positional information of tokens within the prompts. This manipulation is achieved through the introduction of "placeholder tokens," which occupy specific positions within the sequence but do not contribute to the computation of attention scores.


By strategically placing these placeholder tokens, the relative positions of other tokens are altered, influencing the attention weights assigned between different segments of the prompt. The optimal configuration of placeholder tokens is determined by evaluating performance on a training set and then applied to a test set to assess the effectiveness of position engineering in improving LLM performance.


Position engineering demonstrated significant improvements in LLM performance, achieving up to a 15.4% accuracy increase for retrieval-augmented generation (RAG) tasks and a 3.6% increase for in-context learning (ICL) tasks, demonstrating its potential for further enhancing the capabilities of language models.


### **The Pulse**


**Late Language Model** - Despite Apple's silence in the generative AI race, the company is now actively developing its own multimodal model called


[MM1](https://machinelearning.apple.com/research/mm1-methods-analysis-insights) . This new AI model may power the next generation of Siri, offering a significant upgrade in performance and efficiency. MM1 utilizes innovative techniques like synthetic data training and a mixture-of-experts model, allowing it to achieve impressive results with fewer parameters than its counterparts.


**Compute, compute everywhere -** Intel is developing


[Hala Point](https://www.intel.com/content/www/us/en/newsroom/news/intel-builds-worlds-largest-neuromorphic-system.html#gs.7x3nax) , the world's largest neuromorphic system that aims to tackle efficiency and sustainability challenges faced by current AI models. Hala Point can execute 20 quadrillion operations per second with remarkable energy efficiency, exceeding the capabilities of traditional GPU and CPU architectures. The researchers plan to use Hala Point for advanced brain-scale computing research in various scientific fields.


**GeneAI** - Profluent, a California-based AI company, has open sourced an


[AI model](https://github.com/Profluent-AI/OpenCRISPR) capable of designing CRISPR-like proteins not found in nature. This breakthrough has the potential to revolutionize gene editing by providing scientists with a vast array of synthetic proteins for targeted therapies. The company's AI model, trained on extensive biological data, can generate millions of diverse CRISPR-like proteins, opening doors to cures for previously incurable diseases.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify Dev Team.
