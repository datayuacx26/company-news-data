---
schema_version: "1.0.0"
document_id: "e5b9d80ff4830878311b8097a9c36a7d6a04fa42c8ae2f52498ab24d323ff192"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/gpt-oss"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:5ce4aa2905f64b837c024a6c99fdc063b3ca1575eb697bd0e46fbc416bfeab96"
---

# OpenAI gpt-oss

## Welcome OpenAI’s gpt-oss!


Ollama partners with OpenAI to bring its latest state-of-the-art open weight models to Ollama. The two models, **20B** and **120B** , bring a whole new local chat experience, and are designed for powerful reasoning, agentic tasks, and versatile developer use cases.


### Feature highlights


- **Agentic capabilities:** Use the models’ native capabilities for function calling, web browsing (Ollama is providing a built-in web search that can be optionally enabled to augment the model with the latest information), python tool calls, and structured outputs.
- **Full chain-of-thought:** Gain complete access to the model’s reasoning process, facilitating easier debugging and increased trust in outputs.
- **Configurable reasoning effort:** Easily adjust the reasoning effort (low, medium, high) based on your specific use case and latency needs.
- **Fine-tunable:** Fully customize models to your specific use case through parameter fine-tuning.
- **Permissive Apache 2.0 license:** Build freely without copyleft restrictions or patent risk—ideal for experimentation, customization, and commercial deployment.


### Quantization - MXFP4 format


OpenAI utilizes quantization to reduce the memory footprint of the gpt-oss models. The models are post-trained with quantization of the mixture-of-experts (MoE) weights to MXFP4 format, where the weights are quantized to 4.25 bits per parameter. The MoE weights are responsible for 90+% of the total parameter count, and quantizing these to MXFP4 enables the smaller model to run on systems with as little as 16GB memory, and the larger model to fit on a single 80GB GPU.


Ollama is supporting the MXFP4 format natively without additional quantizations or conversions. New kernels are developed for Ollama’s new engine to support the MXFP4 format.


Ollama collaborated with OpenAI to benchmark against their reference implementations to ensure Ollama’s implementations have the same quality.


**20B parameter model**


` gpt-oss-20b` model is designed for lower latency, local, or specialized use-cases.


**120B parameter model**


` gpt-oss-120b` model is designed for production, general purpose, high reasoning use-cases.


### NVIDIA and Ollama collaborate to accelerate gpt-oss on GeForce RTX and RTX PRO GPUs


NVIDIA and Ollama are advancing their partnership to boost model performance on NVIDIA GeForce RTX and RTX PRO GPUs. This collaboration enables users on RTX-powered PCs to accurately leverage the capabilities of OpenAI’s gpt-oss model.


We will continue to collaborate and enhance Ollama. In the future, we will publish an in-depth engineering post on the model.


### Get started


Get started by downloading the[latest Ollama version](https://ollama.com/download)


The model can be downloaded directly in Ollama’s new app or via the terminal:


**ollama run gpt-oss:20b**


**ollama run gpt-oss:120b**


### Reference


[OpenAI launch blog](https://openai.com/index/introducing-gpt-oss)[OpenAI model card](https://openai.com/index/gpt-oss-model-card/)[NVIDIA RTX blog](https://blogs.nvidia.com/blog/rtx-ai-garage-openai-oss)
