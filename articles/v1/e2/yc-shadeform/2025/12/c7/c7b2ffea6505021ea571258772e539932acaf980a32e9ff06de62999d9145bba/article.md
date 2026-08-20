---
schema_version: "1.0.0"
document_id: "c7b2ffea6505021ea571258772e539932acaf980a32e9ff06de62999d9145bba"
company_key: "yc-shadeform"
company: "Shadeform"
source_id: "yc-shadeform-news-import-b903e9afa9f0"
canonical_url: "https://shadeform.com/resources/tutorials/model-guide-deepseek-v32"
published_at: "2025-12-05T12:00:00+00:00"
first_seen_at: "2026-07-24T00:27:10.014959+00:00"
fetched_at: "2026-07-28T21:27:04.798558+00:00"
content_hash: "sha256:8f410d9310acd62b361e3d1c476d39b2c94888d1f31067a2ccfbd6bba552c21d"
---

# Model Guide: Deepseek V3.2

In this model guide, we explore two new model releases from DeepSeek, how they compare to similar models, which GPUs make the most sense to run them on, and how to deploy them yourself.


## Introducing DeepSeek V3.2


The team at DeepSeek have released a new model architecture, DeepSeek V3.2, that performs on par with both OpenAI’s GPT 5 and Google’s Gemini 3.0 Pro.


The DeepSeek V3.2 family encompasses two new SOTA models with distinct use cases:


- **DeepSeek V3.2 Thinking:** DeepSeek’s flagship general purpose reasoning model, now with enhanced agentic reasoning and tool use capabilities.
- **DeepSeek V3.2 Speciale:** an experimental model that trades tool-use functionality and output formatting for maximum reasoning performance.


Both models leverage a MoE architecture with 685B total parameters, 37B active parameters, and a context length of 128K tokens.


Additionally, unique to DeepSeek’s V3.2 architecture, these models share a new feature called the DeepSeek Sparse Attention (DSA) mechanism. DSA changes the computational complexity for attention from quadratic to near-linear, making long-context processing much faster and cheaper.


## DeepSeek V3.2 Thinking


DeepSeek designed V3.2-Thinking to bring reasoning to agentic tool-use. To achieve this, the team developed a novel synthesis pipeline that systematically generates tool-use specific training data at scale. This training data was then used in a lengthy post-training process that yielded substantial improvements in generalization and instruction-following within complex interactive environments.


As a result, DeepSeek V3.2 Thinking scores on par and in some cases higher than leading closed models in tool-use specific benchmarks like T2 and Tool-Decathlon.


## DeepSeek V3.2 Speciale


Where DeepSeek V3.2 Thinking was post-trained to incorporate agentic capabilities, V3.2-Speciale was post-trained entirely on reasoning data, with further optimizations made to enhance mathematical performance. Because of this, V3.2-Speciale forgoes any agentic capabilities, as well as losing the ability to control output formatting (raw text only).


DeepSeek V3.2 Speciale can be thought of as an experimental high-performance variant designed to test the limits of LLM reasoning capabilities.


The tradeoffs appear to have paid off, as V3.2-Speciale outperforms several closed models, even Google’s Gemini 3.0 Pro, in reasoning specific benchmarks like AIME 2025 and IMOAnswerBench.


## Which model should you use?


Choosing between DeepSeek V3.2 Thinking and DeepSeek V3.2 Speciale depends entirely on your specific use case. The two models are optimized for fundamentally different objectives: one for production workflows and the other for raw, deep reasoning.


DeepSeek V3.2 Thinking is your model of choice for integration into real-world applications, agentic systems, and automation pipelines. It sacrifices a small fraction of peak reasoning performance for the critical features required for production use:


- **Agent Orchestration & Tool-Use:** Designed to seamlessly integrate with external tools, APIs, and retrieval-augmented generation (RAG) systems.
- **Structured Output:** Capable of producing reliable, schema-compliant outputs (e.g., JSON, YAML) essential for downstream software consumption.
- **Production-Ready Consistency:** Prioritizes predictable formatting and reasoning consistency, making it stable for deployment.


It’s ideal for: Agent workflows, code generation, RAG pipelines, data extraction with strict schemas, and general automation where external interaction is needed.


DeepSeek V3.2 Speciale is a specialized, experimental variant built for maximum cognitive accuracy. It is designed to push the boundaries of LLM reasoning, but this comes at the cost of production compatibility.


- **No Tool-Use/Agentic Capabilities:** It cannot interact with tools or external environments.
- **Unstructured Output:** It loses the ability to control output formatting, providing only raw text responses.
- **Maximum Reasoning Accuracy:** By removing the constraints of format compliance and tool interaction, the model allocates its full capacity to complex, long-form deduction.


It’s ideal for: Research benchmarking, mathematical problem-solving, complex logical proofs, and any task where maximum raw intelligence is required, but without the need for structured output or tool integration.


## GPU Benchmarking - What you should run on


Due to the DeepSeek V3.2 family’s 685B parameter shared architecture, the only available GPUs with enough memory to fit the models are the NVIDIA B200 and H200.


To evaluate which of the two GPU types performed best, we spun up a vLLM server for each model on a single node (8 GPUs) of both B200s and H200s.


Using the vllm-bench testing suite and ShareGPT dataset, we tested each GPU type and model to measure average token throughput at various levels of concurrency. Additionally, we calculated token cost for each GPU type and model using the lowest hourly pricing on the[Shadeform marketplace](https://platform.shadeform.ai/?utm_source=model_guide_blog) .


Starting with DeepSeek V3.2 Thinking, the following table illustrates average token throughput (tokens/s) at concurrency levels of 8, 16, 32, 64 and 128.


### DeepSeek V3.2 Thinking


Concurrency


B200 (tokens/s)


H200 (tokens/s)


8


294.37


305.81


16


503.61


502.56


32


857.31


860.43


64


1412.64


1435.91


128


2411.26


1896.80


The next table illustrates average cost per 1K tokens, calculated using the previous throughput data and the lowest hourly pricing on the[Shadeform marketplace](https://platform.shadeform.ai/?utm_source=model_guide_blog) .


### DeepSeek V3.2 Thinking


Concurrency


B200 (USD/1k tokens)


H200 (USD/1k tokens)


8


USD 0.0316


USD 0.0164


16


USD 0.0185


USD 0.0099


32


USD 0.0108


USD 0.0058


64


USD 0.0066


USD 0.0035


128


USD 0.0039


USD 0.0026


At most concurrency levels, H200 GPUs will have roughly the same performance as B200 GPUs but at lower cost. Unless you expect consistently high concurrency exceeding 128 requests, we recommend deploying DeepSeek V3.2 Thinking on H200 GPUs.


Moving to DeepSeek V3.2 Speciale, the following table illustrates average token throughput (tokens/s) at concurrency levels of 8, 16, 32, 64 and 128.


### DeepSeek V3.2 Speciale


Concurrency


B200 (tokens/s)


H200 (tokens/s)


8


294.77


307.01


16


504.16


504.37


32


858.53


865.83


64


1411.14


1440.30


128


2414.72


1946.76


### DeepSeek V3.2 Speciale


Concurrency


B200 (USD/1k tokens)


H200 (USD/1k tokens)


8


USD 0.0316


USD 0.0163


16


USD 0.0185


USD 0.0099


32


USD 0.0108


USD 0.0058


64


USD 0.0066


USD 0.0035


128


USD 0.0039


USD 0.0026


For the same reasons as before, we also recommend deploying DeepSeek V3.2 Speciale on H200 GPUs.


## Quickstart


To deploy DeepSeek V3.2 models, we recommend using the popular inference framework vLLM.


To set up a vLLM server with either model, you can use our[V3.2-Thinking template](https://platform.shadeform.ai/templates/fe723020-0400-4fc8-bd45-d2858ec5937e?utm_source=model_guide_blog) ,[V3.2-Speciale template](https://platform.shadeform.ai/templates/118496a6-93ce-4fab-97fa-32c84f565da8?utm_source=model_guide_blog) , or follow the steps below.


The following example is for DeepSeek V3.2 Thinking. To modify for the V3.2-Speciale variant, simply replace the Hugging Face model id with` deepseek-ai/DeepSeek-V3.2-Speciale` .


**Step 1:** Set up your environment and install dependencies


shell


```text
# Set up a clean virtualenv   python3 -m venv ~/deepseek-vllm && source ~/deepseek-vllm/bin/activate
# Install vLLM (CUDA 12.1 wheels) and tools   pip install --upgrade pip   pip install wheel   pip install vllm --extra-index-url https://wheels.vllm.ai/nightly   pip install git+https://github.com/deepseek-ai/DeepGEMM.git@v2.1.1.post3 --no-build-isolation   pip install huggingface_hub  # CLI login to pull weights
```


**Step 2:** Authenticate with HF


Provide your hf access token when prompted


shell


```text
hf auth login
```


**Step 3:** Create Jinja chat template for DeepSeek V3.2


At the time of writing, DeepSeek does not provide a standard Jinja chat template for V3.2 models.


shell


```text
cat > deepseek_v3_2_chat_template.jinja << 'EOF'   {% set thinking_start = '<think>' %}   {% set thinking_end = '</think>' %}
{{ bos_token }}   {% for message in messages %}       {% if message.role == 'system' %}           <｜System｜>{{ message.content }}       {% elif message.role == 'user' %}           <｜User｜>{{ message.content }}       {% elif message.role == 'assistant' %}           <｜Assistant｜>{{ thinking_end }}{{ message.content }}{{ eos_token }}       {% endif %}   {% endfor %}
{% if messages[-1].role == 'user' %}       <｜Assistant｜>{{ thinking_start }}   {% endif %}   EOF
```


**Step 4:** Start vLLM server


Set` --tensor-parallel-size` to the number of GPUs in the instance. Set` --max-num-seqs` to the number of concurrent requests you want to run. Set` --max-num-batched-tokens` according to the number of concurrent requests. This is the total number of tokens for the entire batch.` --max-model-len` is the permitted context length. Shorter context lengths allow for more parallel requests.


shell


```text
vllm serve deepseek-ai/DeepSeek-V3.2 \     --host 0.0.0.0 --port 8000 \     --tensor-parallel-size 8 \     --gpu-memory-utilization 0.92 \     --max-num-seqs 128 \     --max-num-batched-tokens 65536 \     --max-model-len 8192 \     --chat-template deepseek_v3_2_chat_template.jinja \     --trust-remote-code
```


**Step 5:** Query the server


Make sure to replace` <instance-ip>` with the ip address of your GPU instance.


shell


```text
curl -X POST "http://<instance-ip>:8000/v1/chat/completions" \        -H "Content-Type: application/json" \        -d '{            "model": "deepseek-ai/DeepSeek-V3.2",            "messages": [                {"role": "user", "content": "Your prompt here"}            ],            "temperature": 0.7,            "max_tokens": 512        }'
```


## Recap


DeepSeek V3.2 is positioning itself as a serious challenger to frontier closed models, delivering performance on par with GPT 5 and Gemini 3.0 Pro.


**V3.2 Thinking** provides a reliable, production-ready foundation for agentic systems, automation pipelines, and structured-output workflows.


**V3.2 Speciale** pushes the boundaries of reasoning performance, trading format control and tool-use for uncompromising reasoning depth that has placed it at the top of several notable benchmarks.


For teams looking to self-host these models, **H200 GPUs** consistently offer the best cost-to-performance profile across typical concurrency levels.


Organizations evaluating next-generation LLM capabilities should consider both DeepSeek V3.2 variants as strong candidates within their respective domains.


## Deploy DeepSeek V3.2 today


The[Shadeform marketplace](https://platform.shadeform.ai/?gputype=H200&utm_source=model_guide_blog) offers on-demand H200 GPUs as low as $2.25/hr.


To get started, deploy DeepSeek V3.2 using our[V3.2-Thinking template](https://platform.shadeform.ai/templates/fe723020-0400-4fc8-bd45-d2858ec5937e?utm_source=model_guide_blog) or[V3.2-Speciale template](https://platform.shadeform.ai/templates/118496a6-93ce-4fab-97fa-32c84f565da8?utm_source=model_guide_blog) on any H200 offering on our marketplace.
