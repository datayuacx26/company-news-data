---
schema_version: "1.0.0"
document_id: "ed79d65eaa3dcd79eb8101b1dc385b2b9d2b8c83e9fe5ab3f9320ba640916032"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/batch-inference-serverless-gpu"
published_at: "2026-06-29T14:22:48+00:00"
first_seen_at: "2026-07-22T21:28:59.137691+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:a608c940fa71bed59f23e0bab7b3801fa3a2b08d8f7c7c10ead907caaff83ac2"
---

# Batch Inference on Serverless GPU

[← All posts](https://www.beam.cloud/blog) Tutorials


# Batch Inference on Serverless GPU


Tim Huynh


June 29, 2026


8 min read


To run batch inference on serverless GPU, wrap your model in a Beam` @function` with a GPU and call` .map()` over your inputs. Beam runs the shards across many containers at once, scales to zero when the queue drains, and bills by the second, so a million-row job costs the same whether you run it weekly or hourly.


```text


```


Run it with` python batch.py` . The` global _pipe` cache loads the model once per container and reuses it across every shard that lands there, so you pay the load cost a handful of times, not once per row.


## What this involves


Batch inference is the offline cousin of a live endpoint. No user waits on a response, so per-request latency does not matter. Total wall-clock time and total cost do. You have a pile of inputs (embeddings to compute, documents to classify, images to caption, a dataset to score) and you want to push it all through a model and write the results somewhere.


The hard part is rarely the model call. It is the plumbing around it: splitting the work into shards, getting a GPU for each shard, running them at the same time without a Kubernetes cluster, retrying the shards that crash, and turning the whole fleet off the second the last shard finishes so you are not paying for idle accelerators overnight.


A standing GPU box solves none of that and bills you 24/7. A[serverless GPU](https://www.beam.cloud/blog/serverless-gpu) setup gives you the accelerators only while the job runs, then releases them.


## Batch inference vs real-time inference


Batch inference processes a fixed pile of inputs offline for the lowest cost per item; a real-time endpoint answers one request at a time for the lowest latency. Batch wins on throughput and price because it packs large effective batch sizes and keeps the GPU busy. Real-time wins when a user is waiting and freshness matters. Pick batch whenever nothing is blocking on the result.


The cost gap is the reason this distinction matters. An always-on endpoint bills for every hour the GPU is allocated, request or no request; running it at 25% average utilization means paying for four GPUs to do the work of one. A batch job provisions the GPU only during active compute, so for offline workloads like classification, embedding, and summarization it typically runs several times cheaper per item than the same model behind a live endpoint.


Dimension Batch inference Real-time endpoint


Optimizes for Throughput and cost per item Latency per request


GPU utilization High, large effective batches Lower, sized for peak


Idle billing None between runs Pays while idle


Best for Offline scoring, embeddings, summarization User-facing requests


Beam primitive .map() fan-out and task_queue A deployed endpoint


## How to run batch inference on a serverless GPU


The code snippet above is the whole fan-out path.` classify.map(shards)` sends each shard to its own container, runs them concurrently, and yields results back as they finish. Beam handles the queue and the[autoscaling](https://www.beam.cloud/blog/serverless-autoscaling) for you, so the same code runs four shards or four thousand. Here are the pieces worth expanding.


### Load the model once with` on_start`


The global-variable cache works, but Beam has a cleaner hook. Pass an` on_start` loader and read its return value from` context` :


```text


```


The loader runs exactly once when a container boots, so model weights land in VRAM before the first shard arrives.


### Queue work instead of mapping it


When inputs arrive over time rather than as one list, use a` task_queue` instead of` .map()` . You enqueue tasks with` .put()` and Beam autoscales by queue depth, adding a container for roughly every N tasks waiting:


```text


```


` retries=3` reruns a shard that hits an out-of-memory error or throws before it gives up, which matters when you are processing millions of rows and a few are malformed.


### Cap runaway shards with a timeout


A single bad input can hang a worker. Set` timeout` (in seconds) so a stuck shard is killed and retried rather than billing forever:


```text


```


### Run the job on a schedule


For a recurring batch (re-embed new rows every night, re-score yesterday's events), deploy the same function behind` @schedule` with a cron expression:


```text


```


Deploy it with` beam deploy batch.py:nightly` and Beam fires it on the cron, spins up the GPUs, and shuts them off when the run ends.


### Move data with Volumes


Large input and output files live on a Beam Volume, a distributed storage mount shared across every container in the job. Read shards from it and write results back without standing up your own object store.


## Optimize throughput so the GPU stays busy


The single biggest lever on a batch job is batch size: push as many items through each forward pass as VRAM allows, because a GPU at 30% utilization costs the same per second as one at 95%. Bigger per-shard batches mean fewer kernel launches and more work per dollar, right up to the memory ceiling. This is the section most provider docs skip, and it is where batch jobs actually save money.


Two practical knobs:


- **Per-shard batch size** — group inputs into chunks (256 in the hero snippet) and feed each chunk to the model in one call. Raise it until you near an out-of-memory error, then back off one step.
- **Container count** — more containers finish the job faster at higher peak spend; fewer keep peak spend low and the wall clock longer. Because Beam scales to zero, total cost is roughly flat across the two, so tune for the deadline you need.


For large language models, an inference server such as[vLLM](https://www.beam.cloud/blog/vllm) adds continuous batching and paged attention on top of this, which keeps the GPU saturated even when sequence lengths vary across a shard.


## Choosing a serverless GPU for batch inference


Batch inference is the workload serverless GPU was made for: huge, bursty, then silent. Four things decide the bill and the effort, and this is where Beam fits the job: cheap GPUs, scale to zero, one-call fan-out, and your own container.


GPU price is the dominant line. Batch jobs are GPU-bound by definition, so the per-hour rate is most of the cost. An H100 on Beam is $1.74/hr versus $4.18/hr on RunPod serverless and $3.95/hr on Modal (list rates checked June 2026). On a job that burns hundreds of GPU-hours, that gap is the difference between a $200 run and a $480 one.


Scale to zero with per-second billing. The fleet costs nothing between runs and starts on the next` .map()` call or scheduled trigger. You pay for the seconds your shards actually compute, and Beam does not bill for container spin-up.


Fan-out is one method call.` .map()` turns a list into a parallel job across containers without you writing a scheduler, a queue, or autoscaling rules. The same code runs 4 shards or 4,000.


You run your own model and container. Unlike a hosted batch API, you are not limited to a provider's model menu. Any weights, any Python, any custom preprocessing goes in the image and runs unchanged.


## Batch inference options compared


Beam, RunPod, and Modal all scale to zero and bill per second; the differences are GPU price, how the batch primitive works, and whether you bring your own model or use a hosted one. Together's Batch API is a different shape: you send a JSONL file of requests against its hosted models and get results back asynchronously, priced per token rather than per GPU-hour. All prices below are list rates checked June 2026.


Option H100 $/hr Scales to zero Batch primitive Bring your own model


Beam $1.74 Yes .map() fan-out and task_queue Yes, any container


RunPod Serverless $4.18 Yes Job queue plus a custom worker you build Yes, custom worker


Modal $3.95 Yes .map() and .spawn() Yes, any container


Together Batch API Token-priced n/a Upload a JSONL batch, poll for results No, hosted models only


Cloud Run / GCP Batch Varies Partial Job array you wire to GPUs yourself Yes, with more ops


Together's Batch API runs at up to 50% off its real-time rates and handles up to 50,000 requests per batch, so it is the cheaper choice when one of its hosted models fits your task and you do not need a custom container. If you are weighing per-GPU platforms instead, the[top serverless GPU providers](https://www.beam.cloud/blog/top-serverless-gpu-providers) cover the wider field, and the[Modal pricing breakdown](https://www.beam.cloud/blog/modal-pricing-explained) digs into one of the rows above. For hosted-model batch alternatives, the[best Replicate alternatives](https://www.beam.cloud/blog/best-replicate-alternatives) compares the API-style options.


## FAQ


### How is batch inference different from a real-time endpoint on Beam?


An endpoint is synchronous and built for requests that finish in 180 seconds or less. Batch work is asynchronous: you fan out with` .map()` or enqueue with a` task_queue` , the call returns a task ID or an iterator, and Beam works through the shards in the background while autoscaling the container count.


### How do I control how many containers run at once?


` .map()` spawns work per input and Beam autoscales the container pool to match. For queue-based jobs you tune how many tasks each replica handles before another replica is added, which sets your ceiling on parallel GPUs. More containers finish the job faster; fewer keep peak spend lower.


### What happens if one shard fails?


Task queues retry a failed task three times by default before marking it failed, and you can set` retries` explicitly. Pair it with a` timeout` so a hung shard is killed and retried instead of billing indefinitely. The rest of the job keeps running.


### How do I run a batch job on a schedule?


Wrap the entry function in` @schedule(when="0 2 * * *")` with a cron expression and deploy it with` beam deploy` . Beam triggers the run, brings up the GPUs, and shuts them off when it finishes, so a nightly job costs nothing the other 23 hours.


### Which GPU should I pick for batch inference?


It depends on the model size and your batch size. Smaller classifiers and embedding models run well on an RTX 4090 or A100 80GB; large LLMs benefit from an H100. You change one argument (` gpu="H100"` ) to switch, so it is cheap to benchmark a shard on two GPUs and pick the better cost-per-row.


### How do I get inputs in and results out?


Mount a Beam Volume for large files shared across all the containers in the job, or read and write your own object storage from inside the function. Results yielded by` .map()` come back to the calling process, so for smaller jobs you can collect them directly and write once at the end.


### Is batch inference cheaper than real-time serving?


Usually, yes, for offline work. A live endpoint pays for the GPU whether or not a request is in flight, and at typical utilization that is several times the cost per item of a batch job that scales to zero between runs. The savings grow with how bursty and how large the workload is.


## Get started


Run your next batch job on GPUs that turn off when it finishes.[Get started free on Beam](https://www.beam.cloud/?utm_source=usecase&utm_campaign=run-batch-inference-on-serverless-gpu) — new accounts include $30 in credit refreshed monthly.


Tim Huynh


Published June 29, 2026


Keep Reading


## More from the Beam blog


[Tutorials Serverless GPU for Reinforcement Learning Fan out thousands of RL rollouts across serverless GPUs with one .map() call. Snapshot environments, scale to zero between updates, run on your own cloud. Tim Huynh](https://www.beam.cloud/blog/serverless-gpu-reinforcement-learning)[Tutorials How to Deploy ComfyUI as an API Turn a ComfyUI workflow into a callable HTTP API on serverless GPUs. Nathanael Chiang](https://www.beam.cloud/blog/comfyui-api)


$30 free credit


refreshed monthly


## Start shipping on infra
you won’t outgrow.


Run sandboxes and GPU workloads on your cloud, and scale out to ours when you need to. No infra to manage.


[Start Building](https://platform.beam.cloud/)[Read the docs](https://docs.beam.cloud/)
