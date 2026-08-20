---
schema_version: "1.0.0"
document_id: "16aed55b8af56d2d15fda98aeab94c32fd1b3b3fc2a6c4c5a8bc932ddea31094"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-24"
published_at: "2024-05-09T16:04:21+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:89578628b340542a271a84bb04e01d803afa7edae75eab5d6d6bd4593a7889b4"
---

# The Deep Dive Issue #24

# The Deep Dive Issue #24


### Unsloth, mixture of techniques, and back to fundamentals.


[Nassim](https://substack.com/@nas07)


May 09, 2024


Lots of interesting news these days, as is custom in the AI space. With all the stuff to digest and growing echoes about the next GPT (takes a smirk from Altman to fire up Twitter now), one breakthrough is probably making less noise than it should. Turns out Med-Gemini, a model from the family of (you guessed it) Gemini models,


[outperforms human experts](https://arxiv.org/pdf/2404.18416) on a bunch of medical tasks.


I mean, we know the song, benchmarks will be somewhat flawed and docs won't be replaced anytime soon. But the prospect of seeing AI save (or help save) millions of lives is uplifting, especially with all the backlash AI is getting around deepfakes. The question remains though, would you entrust your life to an AI if you knew it was better than a doctor?


## **Faster than a sloth**


There's loads of libraries and techniques for finetuning LLMs, but very few provide both high performance and a straightforward interface. Unsloth is one of those and delivers exceptional finetuning speed-ups through a simple API.


**Why would you care?** - We're talking about 2-5x speed-ups minimum with full compatibility across hardwares (including consumer-grade), training in a couple hours and with a fraction of memory cost. But really, who doesn't like a friendly sloth? A minimal example looks like:


```text
from unsloth import FastLanguageModel
import torch
from trl import SFTTrainer
from transformers import TrainingArguments
from datasets import load_dataset


# Get a dataset
url = "https://huggingface.co/datasets/laion/OIG/resolve/main/unified_chip2.jsonl"
dataset = load_dataset("json", data_files = {"train" : url}, split = "train")


# Get a model and tokenizer
model, tokenizer = FastLanguageModel.from_pretrained(
model_name = "unsloth/llama-3-8b-bnb-4bit",
max_seq_length = max_seq_length,
dtype = None,
load_in_4bit = True,
)


# Add LoRA adapters to finetune a subset of params
model = FastLanguageModel.get_peft_model(
model,
r = 16,
target_modules = ["q_proj", "k_proj", "v_proj", "o_proj",
"gate_proj", "up_proj", "down_proj",],
lora_alpha = 16,
max_seq_length = 2048 ,
)


trainer = SFTTrainer(
model = model,
train_dataset = dataset,
dataset_text_field = "text",
max_seq_length = max_seq_length,
tokenizer = tokenizer,
),
)
trainer.train()
```


**How does it work?** - Unsloth uses several optimization techniques to achieve speed and efficiency gains, including:


1.


**Manual Autograd and Chained Matrix Multiplication** : PyTorch uses AutoGrad to automatically calculate gradients. While efficient, Unsloth opts for manual differentiation instead to apply fine-grained optimizations on gradient calculations. Similarly, it tweaks the chained matrix multiplication process by strategically introducing brackets to minimize the number of operations required.


2.


**Rewriting OpenAI Triton Kernels** : Unsloth rewrites all computational kernels using OpenAI's Triton language. Triton is a domain-specific language designed for high-performance deep learning, offering efficient memory management and optimized code generation. This rewrite allows Unsloth to leverage Triton's strengths for further speed gains.


3.


**Using Flash Attention** : Unsloth incorporates Flash Attention, a technique that uses efficient implementations of the attention mechanism. These implementations reduce the memory footprint and computational cost of attention calculations, contributing to the overall speedup achieved.


Check out the


[repository](https://github.com/unslothai/unsloth) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-24?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## **The Lab**


### **Democratic benchmarking**


Evaluating the quality of LLMs is tricky as unsuitable data and faulty assessments can significantly bias the results. Many evaluations now use LLMs as judges, but this can be expensive and introduce LLM-specific bias to the process.


To mitigate these biases, a


[Panel of LLM evaluators (PoLL)](https://arxiv.org/abs/2404.18796) composed of multiple smaller models from different families can be used to generate thorough assessments. In a PoLL, each model in the panel independently evaluates the generated output from the test LLM. This could involve scoring the quality of the output directly, comparing it to a reference answer, or performing pairwise comparisons with another model's output. The individual scores from each evaluator are then aggregated using a voting function (e.g., average, max) to determine the final PoLL score.


The PoLL consistently performs well across different tasks and datasets, exhibiting less bias and achieving a higher correlation with human judgement compared to a single LLM judge, while also being significantly less expensive to run.


### **Consistent lies**


Hallucinations are the bane of LLM users. False or misleading information from LLM outputs make it challenging to establish trust and reliability in the usage of LLMs.


[Harmonic Robustness](https://arxiv.org/pdf/2404.19708) assesses the stability and explainability of an LLM's responses by measuring their deviation from harmonicity. In plain English, harmonicity implies that the LLM's output at any point is equivalent to the average of its outputs for similar inputs around that point.


To measure this, the researchers perturb the input text by adding random, non-semantic characters (like ASCII) that are unlikely to appear in the LLM's training data. The original and perturbed inputs are then processed by the LLM, and their outputs are converted into embedding vectors using a semantic embedding model. The deviation of the perturbed output from the average is then calculated and used as a measure of the LLM's hallucinative tendencies.


The trustworthiness of LLM responses, as confirmed by human evaluation, increases as the harmonicity approaches zero. This indicates that harmonicity may be a relevant proxy to control LLM hallucinations.


### **Chain of experts**


Ensuring LLMs operate in a manner that aligns with human values is a critical challenge, as these models can generate harmful responses due to the vast amount of unfiltered text data they are trained on.


[Mixture of insighTful Experts (MoTE)](https://arxiv.org/abs/2405.00557) proposes to use chain-of-thought (CoT) to prompt the LLM to analyze the question for potential harm, formulate a strategy for crafting a safe answer, and then generate the final response. To enhance this process, MoTE employs a mixture-of-experts (MoE) architecture, where different parts of the model specialize in different steps of the CoT process.


Additionally, a shared expert facilitates knowledge exchange between the different stages, further enhancing the model's ability to generate safe and helpful responses. Further, MoTE can bypass certain steps in the thinking process when appropriate to avoid following incorrect reasoning for too long.


MoTE demonstrates superior performance compared to existing alignment techniques, achieving higher scores in both helpfulness and harmlessness, even when directly responding to prompts without explicit step-by-step reasoning.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## **The Pulse**


**Incoming sassy code reviews** - OpenAI and Stack Overflow


[have partnered](https://stackoverflow.co/company/press/archive/openai-partnership?utm_content=) to enhance AI capabilities in coding tasks. OpenAI will leverage Stack Overflow's extensive coding knowledge base to improve its models. In turn, Stack Overflow, will use OpenAI's models to develop new AI features for its platform, such as OverflowAI.


**Counting in Moore** - Apple's


[announced the M4 chip](https://www.apple.com/newsroom/2024/05/apple-introduces-m4-chip/) , delivering up to 4x faster performance compared to the M2 chip. Additionally, the chip includes a powerful


*Neural Engine* , capable of performing 38 trillion operations per second, making it ideal for AI workloads.


**Another joins the pack** - Microsoft is making a major push into AI with a new LLM.


[MAI-1](https://techstartups.com/2024/05/06/microsoft-to-unveil-mai-1-a-large-language-model-to-challenge-google-and-openai/) is estimated to have 500 billion parameters, placing it in competition with major players like OpenAI and Google. With Phi-3 released a couple weeks ago, it looks like Microsoft is in for serious competition.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify Dev Team.
