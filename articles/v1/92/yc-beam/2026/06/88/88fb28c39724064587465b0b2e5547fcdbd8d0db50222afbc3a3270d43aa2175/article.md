---
schema_version: "1.0.0"
document_id: "88fb28c39724064587465b0b2e5547fcdbd8d0db50222afbc3a3270d43aa2175"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/comfyui-api"
published_at: "2026-06-28T20:02:08+00:00"
first_seen_at: "2026-07-22T21:28:59.137691+00:00"
fetched_at: "2026-07-28T21:42:09.561502+00:00"
content_hash: "sha256:1b0d4518667cae351b23dbc47a1130b60f8b48b38e8abc203a9adedef22aaeea"
---

# How to Deploy ComfyUI as an API

[← All posts](https://www.beam.cloud/blog) Tutorials


# How to Deploy ComfyUI as an API


Nathanael Chiang


June 28, 2026


6 min read


To deploy ComfyUI as an API, wrap it in a Beam Pod that exposes port 8000 and call` create()` . You get a public HTTPS URL backed by an H100 that scales to zero between requests and bills by the second. ComfyUI already serves its own HTTP API, so the Pod hands you a callable` /prompt` endpoint with no extra framework and no GPU box to babysit.


```text


```


## What this involves


ComfyUI is a desktop tool. To call it from an application you need it running on a GPU somewhere, reachable over HTTP, and ideally not burning money while no images are being generated. That is the whole problem: the model itself is the easy part.


The naive version is renting a GPU box, installing ComfyUI, leaving the server up, and paying for it 24/7. That works until traffic is bursty, which image generation almost always is. You want the GPU to appear when a request arrives and disappear when the queue drains.


There are two shapes of "ComfyUI as an API":


- Expose ComfyUI's own HTTP API. ComfyUI already speaks HTTP on the port you pass` --port` . You POST a workflow graph to` /prompt` and read results from` /history` . This is the fastest path and keeps full ComfyUI compatibility.
- Wrap one workflow in your own REST endpoint. You hide the node graph behind a small handler that takes a prompt and returns an image URL. Cleaner contract, more code.


If ComfyUI is new to you, the[install walkthrough](https://www.beam.cloud/blog/how-to-install-comfyui) and the[workflow basics](https://www.beam.cloud/blog/how-to-use-comfyui) cover the desktop side before you put any of it behind an endpoint.


## Export your workflow in API format first


Before any of this works, export your workflow in API format, not the UI format ComfyUI saves by default. The two JSON shapes look similar but the` /prompt` endpoint only accepts the API one, and feeding it the UI export is the most common cause of validation errors. Get this step right and the rest is plumbing.


In ComfyUI, open the settings gear, enable **Dev mode options** , and a **Save (API Format)** button appears in the menu. Click it to write` workflow_api.json` . That file is what you POST to` /prompt` ; the node graph you see on screen is not.


Two more things bite people here:


- Reference only checkpoints and LoRAs that are actually installed on the server. A path that exists on your laptop but not in the container fails validation.
- Custom nodes must be installed in the image. If the workflow calls a node class the server has not loaded, the request errors before generation starts.


## How to deploy ComfyUI on a serverless GPU


Beam runs your container on a GPU and gives it a public URL. The snippet at the top is the whole "expose ComfyUI's HTTP API" path. A few details worth expanding.


Build the image once. The` Image` chain installs` comfy-cli` , runs the ComfyUI installer pinned to a version, and is where you bake in model weights so they are not pulled on every start:


```text


```


Want a single-workflow REST endpoint instead of the whole ComfyUI server? Swap the Pod for an` @asgi` handler. It boots ComfyUI in the background on start, then serves your own FastAPI routes:


```text


```


Deploy it with` beam deploy api.py:handler` . The rendered images come back as signed URLs from` Output` , so you do not have to stand up your own object storage.


## Which ComfyUI endpoints you actually call


Once the Pod is up, you talk to ComfyUI's built-in HTTP server. You queue a workflow on` /prompt` , then poll` /history/{prompt_id}` until the render lands and pull the file from` /view` . Four endpoints cover almost every integration:


Endpoint Method What it does


/prompt POST Queues an API-format workflow, returns a prompt_id


/history/{prompt_id} GET Returns status and output filenames for a run


/view GET Returns a rendered image by filename, subfolder, and type


/upload/image POST Uploads an input image for img2img or controlnet workflows


/ws WebSocket Streams live progress for long-running renders


For quick jobs, POST to` /prompt` and poll` /history` . For multi-minute workflows, open the` /ws` socket so your UI can show progress instead of a spinner.


## What to look for in a ComfyUI API platform


ComfyUI traffic is spiky: a user kicks off a batch of renders, then nothing for an hour. Beam fits that pattern because the endpoint costs nothing while idle, starts on the next request, and bills by the second once it does. Three specifics matter for image generation.


Scale to zero, per-second billing. The endpoint costs nothing while idle and starts on the next request. You pay for render seconds, not for a parked GPU, and Beam does not charge for container spin-up.


GPU price. An H100 on Beam is $1.74/hr versus $4.18/hr on RunPod serverless and $3.95/hr on Modal (all checked June 2026). For GPU-bound image generation that is the dominant line on the bill. The same scale-to-zero economics apply to any[serverless GPU](https://www.beam.cloud/blog/serverless-gpu) workload, not just ComfyUI.


Weights stay on the machine. Baking checkpoints into the image or a Volume means a cold start loads weights into VRAM rather than re-downloading multi-gigabyte safetensors, which is the slow part of starting ComfyUI cold.


You keep ComfyUI itself unchanged. The Pod route exposes the exact same HTTP API you would hit locally, so existing ComfyUI clients and your favorite[ComfyUI workflows](https://www.beam.cloud/blog/top-comfyui-workflows) run without rewrites.


## ComfyUI hosting options compared


Prices are list H100 rates checked June 2026. RunPod and Modal both scale to zero and bill per second; the gap is GPU price and how much glue code you write to expose the workflow. A raw GPU VM is the most flexible and the most work: you own the autoscaling, the URL, and the idle bill.


Option H100 $/hr Scales to zero How you call it Weights


Beam Pod $1.74 Yes ComfyUI's native /prompt API on a public URL Baked into image or Volume


Beam @asgi $1.74 Yes Your own REST route in front of the graph Baked into image or Volume


RunPod Serverless $4.18 Yes Custom worker handler you build Network volume or baked


Modal $3.95 Yes Your own web endpoint you build Modal Volume


Raw GPU VM (EC2/EKS) varies No Whatever you wire up plus ops Your responsibility


## FAQ


### How do I call the ComfyUI API once it is deployed?


` Pod.create()` returns a public HTTPS URL. ComfyUI already serves its own HTTP API on the port you exposed, so you POST an API-format workflow JSON to` /prompt` on that URL and poll` /history` for the result. No extra web framework is needed for the Pod route.


### What is the difference between API-format and UI-format workflow JSON?


The UI format is what ComfyUI saves by default and describes the visual graph. The API format is a flatter structure the` /prompt` endpoint expects. Enable Dev mode in settings and use Save (API Format) to export the right one; posting the UI export is the usual cause of validation errors.


### Does the endpoint scale to zero when no one is generating images?


Yes. Beam scales the container to zero when idle and starts it again on the next request. You are billed by the second for compute time, not for an always-on GPU, and Beam does not bill for container spin-up.


### Which GPU should I pick for ComfyUI?


It depends on the model. SDXL and Flux-schnell run comfortably on an L40S or A100 80GB; large Flux-dev or video workflows benefit from an H100. You change one argument (` gpu="A100-80"` ) to switch.


### How do I avoid re-downloading model weights on every cold start?


Bake the weights into the image with` huggingface-cli` during the build step, or mount a Beam Volume and download once. Both keep the weights on the machine so a cold start only loads them into VRAM instead of pulling them over the network.


### Can I expose a single workflow as a clean REST endpoint instead of the whole ComfyUI server?


Yes. Use the` @asgi` route with a FastAPI handler that accepts your prompt parameters, runs the workflow through a background ComfyUI process, and returns the output image URL. That hides the ComfyUI graph behind your own API contract.


## Get started


Put a ComfyUI workflow behind an HTTPS endpoint without managing a GPU box.[Get started free on Beam](https://www.beam.cloud/?utm_source=usecase&utm_campaign=deploy-comfyui-as-an-api) — new accounts include $30 in credit refreshed monthly.


Nathanael Chiang


Published June 28, 2026


Keep Reading


## More from the Beam blog


[Tutorials Serverless GPU for Reinforcement Learning Fan out thousands of RL rollouts across serverless GPUs with one .map() call. Snapshot environments, scale to zero between updates, run on your own cloud. Tim Huynh](https://www.beam.cloud/blog/serverless-gpu-reinforcement-learning)[Tutorials Batch Inference on Serverless GPU Learn how to fan out batch inference across GPUs with one .map() call. Tim Huynh](https://www.beam.cloud/blog/batch-inference-serverless-gpu)


$30 free credit


refreshed monthly


## Start shipping on infra
you won’t outgrow.


Run sandboxes and GPU workloads on your cloud, and scale out to ours when you need to. No infra to manage.


[Start Building](https://platform.beam.cloud/)[Read the docs](https://docs.beam.cloud/)
