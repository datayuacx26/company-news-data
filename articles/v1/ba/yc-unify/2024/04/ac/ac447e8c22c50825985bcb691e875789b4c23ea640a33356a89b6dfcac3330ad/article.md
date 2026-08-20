---
schema_version: "1.0.0"
document_id: "ac447e8c22c50825985bcb691e875789b4c23ea640a33356a89b6dfcac3330ad"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-20"
published_at: "2024-04-11T17:57:54+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:26:01.008185+00:00"
content_hash: "sha256:62d66a9eb71b91deca072b90379ac3d278c530e827eed59f333b89a8c5c8e754"
---

# The Deep Dive Issue #20

# The Deep Dive Issue #20


### PyReFT, steering LLM reasoning, and new models


[Nassim](https://substack.com/@nas07)


Apr 11, 2024


Here's a fun experiment: Put an LLM in a room with a kid for an hour and see what happens. Much like with real parents, turns out that's


[enough to jailbreak](https://www.anthropic.com/research/many-shot-jailbreaking) models with long context windows. No need to rush to your nearest chat interface though, you can expect this to be fixed by now, sorry.


That said, this does raise a question regarding how model upgrades are released. This cat-and-mouse game is not new, but as mentioned in the post, such vulnerabilities could be disastrous with future, potentially harmful models.


Regulation will do its thing, but the community will need to contribute as well. So, if you end up tearing apart the next AGI, try not to kick-start the apocalypse please.


## **Stepping-in at the right time**


Adapting LLMs to new tasks through full-scale fine-tuning is expensive and inefficient. Conversely, Parameter Efficient Fine Tuning (PEFT) techniques mitigate this by adapting only some of the parameters. Unfortunately, this limits the flexibility to adapt the model to diverse tasks.


Low-Rank Linear Subspace Representation Fine Tuning (now that's a mouthful, let's stick to LoReFT) proposes an alternative way of fine tuning models that doesn't add this limitation.


**Why would you care?** - LoReFT demonstrates superior efficiency and performance compared to existing PEFT methods. It achieves state-of-the-art results while using 10 to 50 times fewer parameters.


*(Left) In ReFT, a function Φ is applied to hidden representations at positions P in layer L. (Right) More granular intervention can be applied using LoReFT. Here it is applied at all layers with prefix length p = 2 and suffix length s = 2. Source: [Original Paper](https://arxiv.org/pdf/2404.03592.pdf)*


**How does it work?** - LoReFT modifies a subset of the model's representations rather than its weights. It uses "interventional interpretability" to explore how modifying representations can impact a model behavior. Here, representations are the internal encodings the model generates to capture meaning and relationships within the data.


Starting from a frozen base model, LoReFT learns to apply adjustments using a low-rank projection matrix. The matrix is trained to identify the subspace within the representations where modification can be made. During training, it analyzes the relationships between the model's hidden representations and the target task. Using this matrix helps steer the model's behavior towards a task without altering underlying weights.


LoReFT can be used with existing PEFT techniques but requires fewer parameters. You can apply LoReFT to your models with the \`pyreft\` library as follows:


```text
import torch
import transformers
from pyreft import (get_reft_model, ReftConfig, ConsreftIntervention)


# loading huggingface model
model_name_or_path = "yahma/llama-7b-hf"
model = transformers.AutoModelForCausalLM.from_pretrained(
model_name_or_path, torch_dtype=torch.bfloat16, device_map="cuda")


# wrap the model with rank-1 constant reft
reft_config = ReftConfig(representations={"layer": 15, "component": "block_output",
"intervention": ConsreftIntervention(
embed_dim=model.config.hidden_size, low_rank_dimension=1)})
reft_model = get_reft_model(model, reft_config)
reft_model.print_trainable_parameters()


"trainable intervention params: 4,097 || trainable model params: 0"
"model params: 6,738,415,616 || trainable%: 6.080064266549391e-05"
```


Check out the


[repository](https://github.com/stanfordnlp/pyreft) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-20?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## **The Lab**


#### **Classified**


When using language models, you need to choose between powerful remote models, or private weaker models. Leveraging both to build efficient LLM pipelines is difficult for privacy-conscious use-cases. This involves sharing sensitive information with remote systems, which is often not an option.


Google researchers


propose a


[privacy-preserving pipeline](https://arxiv.org/pdf/2404.01041.pdf) for building cascading LLM applications. Unlike traditional cascade systems, this approach assumes all data held by the local model should not be shared with the remote model. The research also aims to prevent remote models from reconstructing private information from auxiliary data.


The local model is asked to remove personal identifiable information from the data while retaining task-relevant information. Three methods are explored for achieving this goal. The first involves having the local model only generate a high-level description of the problem it is facing. The second makes the local model generate new examples similar to the original data but without private information. The third involves replacing entities within the original examples with alternative values. The remote model uses these sanitized queries to generate in-context learning examples for the local model. This provides improved performance, without compromising privacy.


By introducing data filtering into the process, this method allows small LLMs to securely leverage potent models. These pipelines can then be deployed in privacy-constrained environments with minimal data leaks.


#### **Know your place**


LLMs struggle to identify relevant information when processing large contexts. This is especially true when important information is located in the middle of the sequence.


[Position-Aware Parameter Efficient Fine-Tuning (PAPEFT)](https://arxiv.org/pdf/2404.01430.pdf) attempts to address this "positional bias" exhibited by LLMs. First, it randomly shuffles the order of items within a list. This forces the LLM to consider all possible positions for relevant information. Then, it employs a "location encoding adapter" that informs the LLM of the position of each item within the sequence. This adapter is a small, trainable module that learns to represent the position of each item as a numerical value. The adapter is then combined with the textual content of the item before being processed by the LLM.


By combining data augmentation with positional awareness, PAPEFT reduces positional bias in LLMs. This results in more consistent and accurate output, regardless of where important information is located.


#### **Brains and brawn**


Multi-step reasoning tasks remain challenging for LLMs. Prompting techniques like Chain-of-Thought (CoT) can help but the extended output can be based on weak rationale.


To address this,


[LM-Guided CoT](https://arxiv.org/pdf/2404.03414.pdf) uses a small LM for generating rationales, and a large LM for predicting answers based on those rationales. The small LM is first trained through knowledge distillation using rationales generated by the large LM. This helps the small LM learn reasoning abilities from the larger model. To further refine rationale quality, the small LM is then optimized using reinforcement learning. This involves measuring rationale quality based on factuality or relevance, among others aspects. The small LM is then rewarded for generating rationales that score well, leading to improved reasoning abilities.


LM-Guided CoT outperforms both standard prompting and CoT prompting on multi-hop question answering tasks. This demonstrates its effectiveness as a resource-efficient approach to improving reasoning in LLMs.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## **The Pulse**


**Your Command is served** - Cohere released


[Command R+](https://txt.cohere.com/command-r-plus-microsoft-azure/) , a new cost-effective LLM specifically designed for enterprise RAG workloads. Command R+ excels in retrieving information and automating tasks, and performs competitively with larger models on business-use benchmarks at a lower cost.


**Lossless** - Stability AI has launched


[Stable Audio 2.0](https://stability.ai/news/stable-audio-2-0) , an upgraded music generation model capable of creating high-quality, full-length music tracks up to 3 minutes long from a single prompt. The new model also supports audio-to-audio conversion, and emphasizes sound effects and style transfer. Stable Audio 2.0 was trained on a fully licensed dataset, allowing for commercial use of the model.


**Code and behold** - OpenAI


[has made](https://platform.openai.com/docs/models/gpt-4-turbo-and-gpt-4) its advanced GPT-4 Turbo with Vision model available to the public through its API. You can now use the model's vision capabilities through code, and automate various applications. Several startups, such as Cognition, are already leveraging GPT-4 Turbo with Vision to enhance their AI-powered applications.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify Dev Team.
