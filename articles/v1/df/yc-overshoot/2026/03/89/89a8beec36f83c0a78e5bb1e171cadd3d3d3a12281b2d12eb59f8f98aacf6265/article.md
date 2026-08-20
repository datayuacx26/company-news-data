---
schema_version: "1.0.0"
document_id: "89a8beec36f83c0a78e5bb1e171cadd3d3d3a12281b2d12eb59f8f98aacf6265"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/deploy-qwen3.5-35b-a3b"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:9acde1c108703a4c775ee5ee1ab654728c8fbadf00a6e88d41ba1ac630705ee2"
---

# How to Deploy Qwen3.5-35B-A3B with vLLM

[← Back to all posts](https://www.overshoot.ai/blogs)


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
# Option A: Using uv (faster)
pip install uv
uv pip install vllm --torch-backend=auto --extra-index-url https://wheels.vllm.ai/nightly


# Option B: Using pip directly
pip install vllm --extra-index-url https://wheels.vllm.ai/nightly


# Install video support
pip install  'vllm[video]'


```


### 2. Launch Deployment


```text
# Make sure you're in your working directory and activate virtual environment
source   venv/bin/activate


# Launch vLLM serve
vllm serve Qwen/Qwen3.5-35B-A3B \
--dtype bfloat16 \
--max-model-len 16384 \
--max-num-batched-tokens 16384 \
--gpu-memory-utilization 0.90 \
--port 8000


```


**Note** : First model download takes ~5-10 minutes. First inference request triggers CUDA graph compilation which takes ~4-5 minutes. Subsequent requests are fast (~0.1s for text).


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
"model": "Qwen/Qwen3.5-35B-A3B",
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
\"model\": \"Qwen/Qwen3.5-35B-A3B\",
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


**Verified Results** :


- Text inference: Working. Response in ~0.14s (warm).
- Video inference: Working. video.mp4 (6s clip) processed in ~9.7s, 1024 tokens generated.
- Video output correctly identified soccer player (Wayne Rooney), actions (clapping, shouting), setting (green field), and temporal sequence.


### 4. Troubleshooting


**Issue 1** : First request takes several minutes


- **Solution** : This is expected. CUDA graph compilation happens on the first request. Subsequent requests are fast.


**Issue 2** : Requires vLLM nightly


- **Solution** : Qwen3.5 architecture (` qwen3_5_moe` ) is only supported in vLLM nightly builds. Install from` https://wheels.vllm.ai/nightly` .


### 5. Notes


- This is a MoE model: 35B total parameters but only 3B active per token. Despite the large total parameter count, inference is efficient.
- GPU memory is high (~131GB) because all 256 expert weights must be loaded, even though only 8+1 are active per token.
- The model uses a novel hybrid attention mechanism (Gated DeltaNet + Gated Attention) which provides faster inference on long contexts compared to standard Transformer attention.
- The model naturally produces` <think>` reasoning traces in completions. This is expected behavior.
- Chunked prefill is automatically enabled with max_num_batched_tokens=16384.
