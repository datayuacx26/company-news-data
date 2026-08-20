---
schema_version: "1.0.0"
document_id: "df6d7c7b9705006640910d9428b35a5c24331c91092c2f9f096cbf5a89b0ca75"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/deploy-qwen3.5-2b"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:c5c6e924bd8db438faaddc4fb694378c2b3e0145e4c2b7984511cbc91bf0250a"
---

# How to Deploy Qwen3.5-2B with vLLM

## Detailed Deployment Instructions


### 1. Environment Setup


```text
# Create a working directory (choose your preferred location)


# Create virtual environment with Python 3.12
python3.12 -m venv venv


# Activate virtual environment
source   venv/bin/activate


# Verify Python version (should be 3.12.x)
python --version


# Upgrade pip
pip install --upgrade pip


# Install vLLM nightly (REQUIRED — Qwen3.5 is not yet in stable vLLM)
# Option A: Using pip with nightly index
pip install --pre  'vllm>0.16.0'   --extra-index-url https://wheels.vllm.ai/nightly


# Option B: If option A installs stable instead of nightly, force it:
# pip install vllm --extra-index-url https://wheels.vllm.ai/nightly


# Install video support
pip install  'vllm[video]'


```


### 2. Launch Deployment


```text
# Make sure you're in your working directory and activate virtual environment
source   venv/bin/activate


# Launch vLLM serve
vllm serve Qwen/Qwen3.5-2B \
--dtype bfloat16 \
--max-model-len 32768 \
--max-num-batched-tokens 32768 \
--gpu-memory-utilization 0.90 \
--reasoning-parser qwen3 \
--default-chat-template-kwargs  '{"enable_thinking": false}'   \
--port 8000


```


**Note** : First model download takes ~2-3 minutes (small model). First inference request triggers CUDA graph compilation which may take several minutes. Subsequent requests are fast.


### 3. Verification


**3.1. Check server is running:**


```text
curl http://localhost:8000/v1/models


```


**3.2. Test text inference:**


```text
curl http://localhost:8000/v1/completions \
-H  "Content-Type: application/json"   \
-d  '{
"model": "Qwen/Qwen3.5-2B",
"prompt": "Hello, how are you?",
"max_tokens": 50
}'


```


**3.3. Test video inference:**


```text
VIDEO_BASE64=$( base64   -w 0 /path/to/your/video.mp4)


time   curl http://localhost:8000/v1/chat/completions \
-H  "Content-Type: application/json"   \
-d  "{
\"model\": \"Qwen/Qwen3.5-2B\",
\"messages\": [{
\"role\": \"user\",
\"content\": [
{
\"type\": \"video_url\",
\"video_url\": {
\"url\": \"data:video/mp4;base64,\$VIDEO_BASE64\"
}
},
{
\"type\": \"text\",
\"text\": \"Describe the user actions in this video in a sequential list.\"
}
]
}],
\"max_tokens\": 1024
}"


```


**3.4. Test with thinking enabled (per-request):**


```text
curl http://localhost:8000/v1/chat/completions \
-H  "Content-Type: application/json"   \
-d  '{
"model": "Qwen/Qwen3.5-2B",
"messages": [{"role": "user", "content": "What is 27 * 13?"}],
"max_tokens": 512,
"chat_template_kwargs": {"enable_thinking": true}
}'


```


**Verified Results** :


- Text inference: Working. Response instant (warm).
- Video inference: Working. video.mp4 (6s clip) processed in ~4.3s.
- Video output: "A person stands up and claps." — correctly identified the action in the clip.
- Thinking disabled by default, can be toggled per-request.


### 4. Troubleshooting


**Issue 1** : First request takes several minutes


- **Solution** : This is expected. CUDA graph compilation happens on the first request. Subsequent requests are fast.


**Issue 2** : Requires vLLM nightly


- **Solution** : Qwen3.5 architecture (` qwen3_5` ) is only supported in vLLM nightly builds. Install from` https://wheels.vllm.ai/nightly` . Note that` pip install vllm` may install stable 0.16.0 which does NOT include Qwen3.5 support — use` --pre 'vllm>0.16.0'` to force nightly.


**Issue 3** : Transformers` KeyError: 'qwen3_5'`


- **Solution** : vLLM nightly bundles a compatible transformers version. If you see this error, ensure you're using the vLLM nightly venv, not a system Python.


**Issue 4** : Thinking loops


- **Solution** : The 2B model is prone to thinking loops when thinking mode is enabled. Use` --default-chat-template-kwargs '{"enable_thinking": false}'` to disable by default, and only enable per-request when needed.


### 5. Notes


- This is one of the smallest **dense** models in the Qwen3.5 family — all 2.27B parameters are active per token.
- GPU memory for weights (~4.5GB) is very low; the vast majority of the 90% GPU allocation goes to KV cache for the 32K context.
- On smaller GPUs (8-16GB), reduce` --max-model-len` to 4096-8192 and` --gpu-memory-utilization` accordingly.
- The model uses a novel hybrid attention mechanism (Gated DeltaNet + Gated Attention) with a 3:1 linear-to-full attention ratio, providing efficient inference on long contexts.
- Chunked prefill is automatically enabled with max_num_batched_tokens=32768.
