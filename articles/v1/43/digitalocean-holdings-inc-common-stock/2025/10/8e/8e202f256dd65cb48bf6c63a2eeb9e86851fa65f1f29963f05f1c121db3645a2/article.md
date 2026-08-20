---
schema_version: "1.0.0"
document_id: "8e202f256dd65cb48bf6c63a2eeb9e86851fa65f1f29963f05f1c121db3645a2"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/fal-ai-image-models-gradient-ai-platform"
published_at: "2025-10-23T12:30:00.022+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:55:32.133421+00:00"
content_hash: "sha256:38d98661aa0f3f94a4d9508f775b3a63f3f88a1421516fcee234844952d4d88c"
---

# Image and audio models from fal now available on DigitalOcean

We’re excited to announce the launch of four multimodal AI models from[fal](http://fal.ai/) on the[DigitalOcean Gradient™ AI Platform](https://www.digitalocean.com/products/gradient/platform) , now available in public preview through[Serverless Inference](https://docs.digitalocean.com/products/gradient-ai-platform/how-to/use-serverless-inference/) . These models allow you to generate images and audio directly via API, without worrying about infrastructure, scaling, or vendor management. With this release, building AI-powered applications that include visual and audio content is easier than ever.


## Explore the new models


The fal models, now in public preview, cover a variety of modalities, enabling you to experiment, prototype, and deploy multimodal AI features quickly:


**Image generation:**


-


**Stable Diffusion XL fast** (` fal-ai/fast-sdxl` ) – High-resolution image generation


-


**FLUX.1 (schnell)** (` fal-ai/flux/schnell` ) – Fast image generation for quick prototyping


**Audio generation:**


-


**Stable Audio** (` fal-ai/stable-audio-25/text-to-audio` ) – Convert text into natural-sounding audio


-


**ElevenLabs TTS Multilingual v2 9** (` fal-ai/elevenlabs/tts/multilingual-v2` ) – Multilingual text-to-speech


These models are available via Serverless Inference, letting you generate images and audio through the same simple API-driven workflow you already use on Gradient AI Platform.


## Try it out


You can start using these models through the[Serverless Inference API](https://docs.digitalocean.com/products/gradient-ai-platform/how-to/use-serverless-inference/) (` https://inference.do-ai.run` ) after[opting in to the public preview](https://cloud.digitalocean.com/account/feature-preview?feature=fal-models) in the DigitalOcean console. Here’s a quick look at how to interact with them:


First, **[opt in to the public preview](https://cloud.digitalocean.com/account/feature-preview?feature=fal-models)** to access the fal models on the Gradient AI Platform. Once opting in, it should take about 10 to 15 minutes for your access to be granted.


**Example: Generate an Image**


```text
export MODEL_ACCESS_KEY="YOUR_KEY"
curl -sS -X POST 'https://inference.do-ai.run/v1/async-invoke' \
-H "Authorization: Bearer $MODEL_ACCESS_KEY" \
-H "Content-Type: application/json" \
-d ' {
"model_id": "fal-ai/flux/schnell",
"input":  {   "prompt": "A high-quality photo of a futuristic city at sunset"  }
}  '


```


**Example: Generate an Image with Customized Parameters**


```text
export MODEL_ACCESS_KEY="YOUR_KEY"
curl -sS -X POST 'https://inference.do-ai.run/v1/async-invoke' \
-H "Authorization: Bearer $MODEL_ACCESS_KEY" \
-H "Content-Type: application/json" \
-d ' {
"model_id": "fal-ai/fast-sdxl",
"input":  {
"prompt": "A high-quality photo of a futuristic city at sunset",
"output_format": "landscape_4_3",
"num_inference_steps": 4,
"guidance_scale": 3.5,
"num_images": 1,
"enable_safety_checker": true
}  ,
"tags": [
{   "key": "type", "value": "test"  }
]
}  '


```


**Example: Generate Sound**


```text
export MODEL_ACCESS_KEY="YOUR_KEY"
curl -sS -X POST 'https://inference.do-ai.run/v1/async-invoke' \
-H "Authorization: Bearer $MODEL_ACCESS_KEY" \
-H "Content-Type: application/json" \
-d ' {
"model_id": "fal-ai/stable-audio-25/text-to-audio",
"input":  {
"prompt": "Futuristic epic song",
"seconds_total": 60
}  ,
"tags": [
{   "key": "type", "value": "test"  }
]
}  '


```


**Example: Text to Speech (TTS)**


```text
export MODEL_ACCESS_KEY="YOUR_KEY"
curl -sS -X POST 'https://inference.do-ai.run/v1/async-invoke' \
-H "Authorization: Bearer $MODEL_ACCESS_KEY" \
-H "Content-Type: application/json" \
-d ' {
"model_id": "fal-ai/elevenlabs/tts/multilingual-v2",
"input":  {
"text": "Hello, this is a text to speech example using Digital Ocean multilingual voice."
}  ,
"tags": [
{   "key": "type", "value": "test"  }
]
}  '


```


**Check the request status**


These requests start the job and return a` request_id` , which you can use to check when your image is ready. Because Serverless Inference uses an asynchronous API, you’ll need to poll the request until it completes.


The /status endpoint is lightweight, so you can query it frequently to check progress. Once the job shows` COMPLETE` , use the` /async-invoke/{request_id}` endpoint to fetch the full generated result.


```text
curl -sS -X GET "https://inference.do-ai.run/v1/async-invoke/ {  request_id }  /status" \
-H "Authorization: Bearer $MODEL_ACCESS_KEY"


```


Keep polling this endpoint until the response shows:


```text
{   "status": "COMPLETE"  }


```


**Retrieve the final result**


Once the job is complete, you can get the full response (which includes your generated image) using:


```text
curl -sS -X GET "https://inference.do-ai.run/v1/async-invoke/ {  request_id }  " \
-H "Authorization: Bearer $MODEL_ACCESS_KEY"


```


The returned JSON includes a URL to the generated audio file, which you can download or play directly in your browser or app.


```text
curl -O " {  url }  "


```


## Bring your ideas to life with fal on DigitalOcean


With these four new multimodal models in public preview, you can now build richer AI-powered experiences, generating images and audio without managing infrastructure.


Get started today by exploring the[Serverless Inference API](https://docs.digitalocean.com/products/gradient-ai-platform/how-to/use-serverless-inference/#serverless-inference-api-endpoints) and integrating these powerful fal models into your applications. For more resources, check out our[Gradient™ AI SDK](https://gradientai-sdk.digitalocean.com/) or watch our new tutorial below!


This launch marks an expansion of DigitalOcean’s partnership with fal, bringing high-performance image and voice generation models to developers through the Gradient AI Platform. Learn more about the collaboration in the official[press release](https://investors.digitalocean.com/news/news-details/2025/DigitalOcean-and-fal-Expand-Collaboration-to-Advance-Multimodal-AI-Innovation/default.aspx) .
