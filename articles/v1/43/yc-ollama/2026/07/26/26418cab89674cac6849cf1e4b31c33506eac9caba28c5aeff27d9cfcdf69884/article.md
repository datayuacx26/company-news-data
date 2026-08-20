---
schema_version: "1.0.0"
document_id: "26418cab89674cac6849cf1e4b31c33506eac9caba28c5aeff27d9cfcdf69884"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/improved-performance-and-model-support-with-gguf"
published_at: null
first_seen_at: "2026-07-24T07:14:45.991039+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:1a68effc0a035a798a16f552a06204639d902b5fbbf800c0c79f843e54d24b1a"
---

# Improved performance and model support with GGUF

[Ollama 0.30](https://ollama.com/download) is now available with improved performance and GGUF model compatibility through[llama.cpp](https://github.com/ggml-org/llama.cpp) . This augments Ollama’s[MLX engine](https://ollama.com/blog/mlx) on Apple silicon, bringing support to more models on a wider range of hardware.


## Performance across more GPUs


### Faster throughput on NVIDIA hardware


With[Ollama 0.30](https://ollama.com/download) , performance on NVIDIA hardware is now up to 20% faster, leveraging optimizations contributed by the NVIDIA and llama.cpp teams.


*Tested with the Gemma 4 26B model running on an NVIDIA RTX 5090 using the Q4_K_M quantization.*


### Wider hardware support with Vulkan


Vulkan is now enabled by default, extending Ollama’s GPU acceleration to a wider range of hardware, including AMD and Intel devices. More users can now run models on the GPU out of the box, without installing vendor-specific libraries.


## Support for more models


[Ollama 0.30](https://ollama.com/download) expands compatibility with the GGUF ecosystem, so more models run out of the box—including model families such as[LFM](https://huggingface.co/LiquidAI/LFM2-8B-A1B-GGUF) and[Prism](https://huggingface.co/prism-ml/Bonsai-8B-gguf) , as well as fine-tuned models published by[Unsloth](https://unsloth.ai/docs/get-started/unsloth-model-catalog) .


### Run GGUF models from Hugging Face


To use a model, first download the GGUF file or a directory containing GGUF files. Next, create a` Modelfile` with the` FROM` command pointing to the path of the GGUF file (or directory):


```text
FROM ./my-model.Q4_K_M.gguf


```


Then create and run the model:


```text
ollama create -f Modelfile my-model
ollama run my-model


```


### Coding agents and assistants


If a model supports tool calling, that capability carries over to Ollama. You can use these models with your favorite coding agents and personal assistants in a single command.


**Claude Code**


```text
ollama launch claude --model my-model


```


**Hermes Agent**


```text
ollama launch hermes --model my-model


```


**OpenClaw**


```text
ollama launch openclaw --model my-model


```


To verify that a GGUF file supports tool calling, look for the` tools` capability with` ollama show` :


```text
ollama show my-model


```


## Acknowledgements


We’d like to acknowledge the work done by Georgi Gerganov and the llama.cpp maintainer teams, as well as hardware partners including NVIDIA, AMD, Qualcomm, and Intel, who have worked hard to optimize performance with the GGML ecosystem on their respective platforms.


If you have any feedback, join[Ollama’s Discord](https://discord.gg/ollama) or reach out at hello@ollama.com.
