---
schema_version: "1.0.0"
document_id: "9f838336f039cafdc296df81785c2ef08be8a464743dcd89b5580ab1b26dd423"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/deploy-qwen3.5-4b"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:ca70697180884c03c9cc0d08fde48c4264356a537414b8f46d0635c2cd85699c"
---

# How to Deploy Qwen3.5-4B with vLLM

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
vllm serve Qwen/Qwen3.5-4B \
--dtype bfloat16 \
--max-model-len 32768 \
--max-num-batched-tokens 32768 \
--gpu-memory-utilization 0.90 \
--reasoning-parser qwen3 \
--default-chat-template-kwargs  '{"enable_thinking": false}'   \
--port 8000


```


**Note** : First model download takes ~3-5 minutes. First inference request triggers CUDA graph compilation which may take several minutes. Subsequent requests are fast.


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
"model": "Qwen/Qwen3.5-4B",
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
\"model\": \"Qwen/Qwen3.5-4B\",
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
"model": "Qwen/Qwen3.5-4B",
"messages": [{"role": "user", "content": "What is 27 * 13?"}],
"max_tokens": 512,
"chat_template_kwargs": {"enable_thinking": true}
}'


```


**Verified Results** :


- Text inference: Working. Response instant (warm).
- Video inference: Working. video.mp4 (6s clip) processed in ~4.6s.
- Video output correctly identified man in red shirt on soccer field, yelling, clapping hands, gesturing — sequential actions accurately described.
- Thinking disabled by default, can be toggled per-request.


### 4. Troubleshooting


**Issue 1** : First request takes several minutes


- **Solution** : This is expected. CUDA graph compilation happens on the first request. Subsequent requests are fast.


**Issue 2** : Requires vLLM nightly


- **Solution** : Qwen3.5 architecture (` qwen3_5` ) is only supported in vLLM nightly builds. Install from` https://wheels.vllm.ai/nightly` . Note that` pip install vllm` may install stable 0.16.0 which does NOT include Qwen3.5 support — use` --pre 'vllm>0.16.0'` to force nightly.


**Issue 3** : Transformers` KeyError: 'qwen3_5'`


- **Solution** : vLLM nightly bundles a compatible transformers version. If you see this error, ensure you're using the vLLM nightly venv, not a system Python.


### 5. Notes


- This is one of the smaller **dense** models in the Qwen3.5 family — all 4.66B parameters are active per token.
- GPU memory for weights (~9.3GB) is low; the vast majority of the 90% GPU allocation goes to KV cache for the 32K context.
- On smaller GPUs (16-24GB), reduce` --max-model-len` to 8192-16384 and` --gpu-memory-utilization` accordingly.
- The model uses a novel hybrid attention mechanism (Gated DeltaNet + Gated Attention) with a 3:1 linear-to-full attention ratio, providing efficient inference on long contexts.
- Vision encoder is slightly smaller than the 9B/27B variants (24 layers, hidden 1024 vs 27 layers, hidden 1152).
- Chunked prefill is automatically enabled with max_num_batched_tokens=32768.
