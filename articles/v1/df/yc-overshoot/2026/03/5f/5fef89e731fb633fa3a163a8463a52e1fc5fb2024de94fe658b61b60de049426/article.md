---
schema_version: "1.0.0"
document_id: "5fef89e731fb633fa3a163a8463a52e1fc5fb2024de94fe658b61b60de049426"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/deploy-qwen3-vl-30b-a3b"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:55a22e49091c05a3cbb114e0777527b8465c12090ebe80e0df1973f608a823c4"
---

# How to Deploy Qwen3-VL-30B-A3B with vLLM

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


# Install vLLM
pip install vllm


```


### 2. Launch Deployment


```text
# Make sure you're in your working directory and activate virtual environment
source   venv/bin/activate


# Recommended public-facing configuration
vllm serve Qwen/Qwen3-VL-30B-A3B-Instruct \
--dtype bfloat16 \
--max-model-len 8192 \
--max-num-batched-tokens 8192 \
--gpu-memory-utilization 0.75 \
--port 8000


```


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
"model": "Qwen/Qwen3-VL-30B-A3B-Instruct",
"prompt": "Hello, how are you?",
"max_tokens": 50
}'


```


**3.3. Test video inference:**


```text
VIDEO_BASE64=$( base64   -w 0 /path/to/video.mp4)


curl http://localhost:8000/v1/chat/completions \
-H  "Content-Type: application/json"   \
-d  "{
\"model\": \"Qwen/Qwen3-VL-30B-A3B-Instruct\",
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
\"text\": \"Describe this video in detail.\"
}
]
}],
\"max_tokens\": 150
}"


```


**Verified Results** :


- Deployment succeeded with the 8K / 75% GPU configuration.
- Text inference returned in about 0.28 seconds for a short prompt.
- A short video inference request completed in about 7.1 seconds and produced a detailed description.


### 4. Troubleshooting


**Issue 1** : Not enough free GPU memory


- **Solution** : Lower` --gpu-memory-utilization` or reduce` --max-model-len` .


**Issue 2** : Slow first load


- **Solution** : This is expected. The first load includes checkpoint loading, torch compilation, and CUDA graph capture.


**Issue 3** : Out of memory during inference


- **Solution** : Use shorter videos, reduce context length, or run fewer concurrent requests.


### 5. Notes


- This is the MoE Qwen3-VL variant: around 31B total parameters with about 3B active per token.
- The tested deployment used about 109 GB at the recommended 8K setting.
- Native context is much larger, but 8K was the clean verified deployment configuration here.
- This model supports text, image, and video inputs through vLLM.


### 6. References


- Model Card:[https://huggingface.co/Qwen/Qwen3-VL-30B-A3B-Instruct](https://huggingface.co/Qwen/Qwen3-VL-30B-A3B-Instruct)
- GitHub:[https://github.com/QwenLM/Qwen3-VL](https://github.com/QwenLM/Qwen3-VL)
- vLLM Documentation:[https://docs.vllm.ai/projects/recipes/en/latest/Qwen/Qwen3-VL.html](https://docs.vllm.ai/projects/recipes/en/latest/Qwen/Qwen3-VL.html)
