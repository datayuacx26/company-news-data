---
schema_version: "1.0.0"
document_id: "3fb5171c27907ac8b6c370d0b93151cf0fac8e2ae9def964269b1acf3f32cede"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/deploy-qwen3-vl-8b"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:0d049afa3eaf58aeab1c6ece782793ed215721342420d272d020f964766a57d0"
---

# How to Deploy Qwen3-VL-8B with vLLM

[← Back to all posts](https://www.overshoot.ai/blogs)


## Detailed Deployment Instructions


### 1. Environment Setup


```text
# Create a working directory (choose your preferred location)


# Create virtual environment with Python 3.12 (3.14 not supported by numba)
# Make sure you have Python 3.12 installed: sudo apt install python3.12 python3.12-venv
python3.12 -m venv venv


# Activate virtual environment
source   venv/bin/activate


# Verify Python version (should be 3.12.x)
python --version


# Upgrade pip
pip install --upgrade pip


# Install vLLM (this takes ~5-10 minutes)
pip install vllm


```


### 2. Launch Deployment


```text
# Make sure you're in your working directory and activate virtual environment
source   venv/bin/activate


# Launch vLLM serve (maximum context for solo deployment)
vllm serve Qwen/Qwen3-VL-8B-Instruct \
--dtype bfloat16 \
--max-model-len 65536 \
--max-num-batched-tokens 65536 \
--gpu-memory-utilization 0.9 \
--port 8000


```


### 3. Verification


**Check server is running:**


```text
curl http://localhost:8000/v1/models


```


**Expected output:**


```text
{
"object"  :    "list"  ,
"data"  :    [  {
"id"  :    "Qwen/Qwen3-VL-8B-Instruct"  ,
"object"  :    "model"  ,
"created"  :    1769818884  ,
"owned_by"  :    "vllm"  ,
"root"  :    "Qwen/Qwen3-VL-8B-Instruct"  ,
"parent"  :    null   ,
"max_model_len"  :    65536
}  ]
}


```


**Test text inference:**


```text
curl http://localhost:8000/v1/completions \
-H  "Content-Type: application/json"   \
-d  '{
"model": "Qwen/Qwen3-VL-8B-Instruct",
"prompt": "Hello, how are you?",
"max_tokens": 50
}'


```


**Test with image (multimodal):**


```text
curl http://localhost:8000/v1/chat/completions \
-H  "Content-Type: application/json"   \
-d  '{
"model": "Qwen/Qwen3-VL-8B-Instruct",
"messages": [{
"role": "user",
"content": [
{"type": "text", "text": "What is in this image?"},
{"type": "image_url", "image_url": {"url": "https://example.com/image.jpg"}}
]
}],
"max_tokens": 100
}'


```


**Test with video (base64 encoded):**


```text
VIDEO_BASE64=$( base64   -w 0 /path/to/video.mp4)
curl http://localhost:8000/v1/chat/completions \
-H  "Content-Type: application/json"   \
-d  "{
\"model\": \"Qwen/Qwen3-VL-8B-Instruct\",
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


**Example output:**


```text
1. A soccer player in a red jersey is standing on a grass field.
2. He is gesturing with his hands, appearing to be communicating or arguing with someone off-camera.
3. He continues to gesture and speak, moving slightly as he does so.
4. The camera focuses on him as he keeps talking and gesturing.


```


### 4. Troubleshooting


**Issue 1** : Python 3.14 not supported by numba


- **Solution** : Use Python 3.12 or 3.13 instead. Recreate venv with` python3.12 -m venv venv`


**Issue 2** : Slow compilation on first run


- **Symptom** : Server takes 2-3 minutes to fully start
- **Solution** : This is normal. Torch compilation caches graphs for future use. Subsequent startups will be faster.


### 5. Notes


- **Context length** : Model natively supports 256K tokens (up to 1M). Solo deployment uses 65K context.
- **Multimodal support** : Supports both image and video inputs
- **Flash Attention** : Automatically enabled for vision encoder (AttentionBackendEnum.FLASH_ATTN)
- **Torch Compilation** : Enabled for backbone with Inductor backend for better performance
- **Chunked prefill** : Enabled with max_num_batched_tokens=16384
- **Asynchronous scheduling** : Enabled for better throughput
- **Prefix caching** : Enabled by default


### 6. References


- vLLM Qwen3-VL Documentation:[https://docs.vllm.ai/projects/recipes/en/latest/Qwen/Qwen3-VL.html](https://docs.vllm.ai/projects/recipes/en/latest/Qwen/Qwen3-VL.html)
- Model Card:[https://huggingface.co/Qwen/Qwen3-VL-8B-Instruct](https://huggingface.co/Qwen/Qwen3-VL-8B-Instruct)
- GitHub Repo:[https://github.com/QwenLM/Qwen3-VL](https://github.com/QwenLM/Qwen3-VL)
- Minimum vLLM version: 0.11.0 (deployed with 0.15.0)
