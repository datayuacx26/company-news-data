---
schema_version: "1.0.0"
document_id: "04eec2b86cb54eb19cac957bc903bc7c24494f6bde50aac5ec08a2d376e930ac"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/deploy-qwen3.5-27b"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:6e0d6049bc5a65859b38f026049db952ad1b3ef432dc905d30eb02b7a8580829"
---

# How to Deploy Qwen3.5-27B with vLLM

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
pip install vllm --pre --extra-index-url https://wheels.vllm.ai/nightly


# Option B: If nightly wheels are unavailable for your platform,
# reuse a venv from another Qwen3.5 model that already has nightly installed


# Install video support
pip install  'vllm[video]'


```


### 2. Launch Deployment


```text
# Make sure you're in your working directory and activate virtual environment
source   venv/bin/activate


# Launch vLLM serve
vllm serve Qwen/Qwen3.5-27B \
--dtype bfloat16 \
--max-model-len 32768 \
--max-num-batched-tokens 32768 \
--gpu-memory-utilization 0.90 \
--reasoning-parser qwen3 \
--default-chat-template-kwargs  '{"enable_thinking": false}'   \
--port 8000


```


**Note** : First model download takes ~5-10 minutes. First inference request triggers CUDA graph compilation which may take several minutes. Subsequent requests are fast.


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
"model": "Qwen/Qwen3.5-27B",
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
\"model\": \"Qwen/Qwen3.5-27B\",
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
"model": "Qwen/Qwen3.5-27B",
"messages": [{"role": "user", "content": "What is 27 * 13?"}],
"max_tokens": 512,
"chat_template_kwargs": {"enable_thinking": true}
}'


```


**Verified Results** :


- Text inference: Working. Response in <0.01s (warm).
- Video inference: Working. video.mp4 (6s clip) processed in ~0.9s.
- Video output correctly identified soccer player in red jersey, shouting/gesturing on grassy field.
- Thinking disabled by default, can be toggled per-request.


### 4. Troubleshooting


**Issue 1** : First request takes several minutes


- **Solution** : This is expected. CUDA graph compilation happens on the first request. Subsequent requests are fast.


**Issue 2** : Requires vLLM nightly


- **Solution** : Qwen3.5 architecture (` qwen3_5` ) is only supported in vLLM nightly builds. Install from` https://wheels.vllm.ai/nightly` . If nightly wheels are unavailable for your platform, check for a compatible recent nightly or reuse an existing Qwen3.5 venv.


**Issue 3** : Thinking tokens appear in content


- **Solution** : Ensure` --reasoning-parser qwen3` is set on the server. To disable thinking entirely, add` --default-chat-template-kwargs '{"enable_thinking": false}'` .


### 5. Notes


- This is the only **dense** model in the Qwen3.5 family — all 27B parameters are active per token. All other Qwen3.5 variants use MoE.
- GPU memory for weights (~51GB) is lower than the 35B-A3B (~70GB) despite similar effective quality, because there are no inactive MoE experts to store.
- The model uses a novel hybrid attention mechanism (Gated DeltaNet + Gated Attention) which provides faster inference on long contexts compared to standard Transformer attention.
- The model naturally produces` <think>` reasoning traces when thinking is enabled. Use` --default-chat-template-kwargs '{"enable_thinking": false}'` to disable by default.
- Chunked prefill is automatically enabled with max_num_batched_tokens=32768.
