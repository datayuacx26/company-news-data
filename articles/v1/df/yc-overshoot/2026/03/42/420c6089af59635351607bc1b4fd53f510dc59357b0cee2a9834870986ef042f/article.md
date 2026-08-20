---
schema_version: "1.0.0"
document_id: "420c6089af59635351607bc1b4fd53f510dc59357b0cee2a9834870986ef042f"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/deploy-qwen3-vl-32b"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:2df9b5bb35d0dd2ad6913921d20fe0366abcf8a1db54570755c0dae87f1e9b3a"
---

# How to Deploy Qwen3-VL-32B with vLLM

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
vllm serve Qwen/Qwen3-VL-32B-Instruct \
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
"model": "Qwen/Qwen3-VL-32B-Instruct",
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
\"model\": \"Qwen/Qwen3-VL-32B-Instruct\",
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
- Text inference returned in about 0.99 seconds for a short prompt.
- A short video inference request completed in about 9.2 seconds and produced a detailed description.


### 4. Troubleshooting


**Issue 1** : Not enough free GPU memory


- **Solution** : Lower` --gpu-memory-utilization` or reduce` --max-model-len` .


**Issue 2** : KV cache is too small for the requested context


- **Solution** : Reduce` --max-model-len` until the requested context fits the available GPU memory.


**Issue 3** : Slow first load


- **Solution** : This is expected. The initial run includes checkpoint loading, torch compilation, and CUDA graph capture.


### 5. Notes


- This is the dense Qwen3-VL 32B model, so all parameters are active on every token.
- The tested deployment used about 109 GB at the recommended 8K setting.
- Native context is much larger, but 8K was the clean verified deployment configuration here.
- This model supports text, image, and video inputs through vLLM.


### 6. References


- Model Card:[https://huggingface.co/Qwen/Qwen3-VL-32B-Instruct](https://huggingface.co/Qwen/Qwen3-VL-32B-Instruct)
- GitHub:[https://github.com/QwenLM/Qwen3-VL](https://github.com/QwenLM/Qwen3-VL)
- vLLM Documentation:[https://docs.vllm.ai/projects/recipes/en/latest/Qwen/Qwen3-VL.html](https://docs.vllm.ai/projects/recipes/en/latest/Qwen/Qwen3-VL.html)
