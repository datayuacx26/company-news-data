---
schema_version: "1.0.0"
document_id: "8f1e7e2fceb0c31f3d911047735f928db902ce4332334270069cabf52149af87"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/deploy-qwen3.5-9b"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:891f5a8ff00aa945a92a187f4039dd022d60aa938c0b53c31c73b1c113784df1"
---

# How to Deploy Qwen3.5-9B with vLLM

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
vllm serve Qwen/Qwen3.5-9B \
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
"model": "Qwen/Qwen3.5-9B",
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
\"model\": \"Qwen/Qwen3.5-9B\",
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
"model": "Qwen/Qwen3.5-9B",
"messages": [{"role": "user", "content": "What is 27 * 13?"}],
"max_tokens": 512,
"chat_template_kwargs": {"enable_thinking": true}
}'


```


**Verified Results** :


- Text inference: Working. Response instant (warm).
- Video inference: Working. video.mp4 (6s clip) processed in ~4.6s.
- Video output correctly identified man in red jersey on field, arm gestures, clenching fists, speaking — sequential actions accurately described.
- Thinking disabled by default, can be toggled per-request.


### 4. Troubleshooting


**Issue 1** : First request takes several minutes


- **Solution** : This is expected. CUDA graph compilation happens on the first request. Subsequent requests are fast.


**Issue 2** : Requires vLLM nightly


- **Solution** : Qwen3.5 architecture (` qwen3_5` ) is only supported in vLLM nightly builds. Install from` https://wheels.vllm.ai/nightly` . Note that` pip install vllm` may install stable 0.16.0 which does NOT include Qwen3.5 support — use` --pre 'vllm>0.16.0'` to force nightly.


**Issue 3** : Transformers` KeyError: 'qwen3_5'`


- **Solution** : vLLM nightly bundles a compatible transformers version. If you see this error, ensure you're using the vLLM nightly venv, not a system Python.


### 5. Notes


- This is the largest **dense** model in the Qwen3.5 small series — all 9.65B parameters are active per token.
- GPU memory for weights (~19.3GB) is moderate; the remaining GPU allocation goes to KV cache for the 32K context.
- On H100 80GB, reduce` --max-model-len` to 16384-24576 and keep` --gpu-memory-utilization 0.90` .
- The model uses a novel hybrid attention mechanism (Gated DeltaNet + Gated Attention) with a 3:1 linear-to-full attention ratio, providing efficient inference on long contexts.
- Vision encoder is larger than the 2B/4B variants (27 layers, hidden 1152 vs 24 layers, hidden 1024), matching the 27B model's ViT.
- Chunked prefill is automatically enabled with max_num_batched_tokens=32768.
