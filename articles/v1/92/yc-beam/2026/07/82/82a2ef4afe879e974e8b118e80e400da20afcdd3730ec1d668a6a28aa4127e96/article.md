---
schema_version: "1.0.0"
document_id: "82a2ef4afe879e974e8b118e80e400da20afcdd3730ec1d668a6a28aa4127e96"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/serverless-gpu-reinforcement-learning"
published_at: "2026-07-02T13:17:27+00:00"
first_seen_at: "2026-07-22T21:28:59.137691+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:f618b16d41839bcb7a035f3e260751ddc5d18e02da886d4986d3f36c23d6fe9a"
---

# Serverless GPU for Reinforcement Learning

To run RL environments on serverless GPUs, wrap the environment in a Beam` @function` with a GPU and fan the rollouts out with` .map()` . Each policy update becomes a batch of parallel rollouts that spin up on their own containers, stream trajectories back, and release the GPUs the moment the batch finishes. You pay for the seconds the rollouts run, not for idle accelerators between updates.


```text


```


Run it with` python train.py` .` rollout.map(range(1024))` sends 1024 environments to 1024 containers, runs them at once, and yields each result back as it lands. The same code runs 16 rollouts or 16,000 — you change the range, not the infrastructure.


## What running RL environments actually involves


An RL loop has two halves that want opposite things from your hardware, and conflating them is why RL infrastructure gets expensive. The learner holds the policy weights in VRAM, consumes a batch of rollouts, and updates. It is long-running and stateful, so it lives on a GPU you keep. The environments are the other half: the same simulator or task run thousands of times per update to generate the experience the learner trains on. That half is bursty. It spikes to thousands of concurrent runs, then goes quiet while the policy updates, then spikes again.


Serverless GPUs are a poor fit for the learner and a perfect fit for the environments. The mistake most teams make is trying to force both onto the same box: either they rent a fixed fleet big enough for peak rollout volume and pay for it around the clock, or they run rollouts on the training box and starve the GPU between updates. The throughput of the whole loop is set by how many environments you can run at once, so under-provisioning the rollout half wastes the expensive training half.


This is what a[serverless GPU](https://www.beam.cloud/blog/serverless-gpu) setup fixes for the environment half. You keep your learner where it is and fan the rollouts out to GPUs that exist only while a batch runs. When the batch drains, the containers scale to zero and the bill stops. The next` .map()` call brings them back.


## How to run RL rollouts on a serverless GPU


The hero snippet is the whole fan-out path:` rollout.map(seeds)` turns a list of seeds into a parallel job across containers, and Beam handles the queue and the[autoscaling](https://www.beam.cloud/blog/serverless-autoscaling) so you never write a scheduler. These are the pieces worth expanding for a real training loop.


### Load the policy once with` on_start`


Re-loading policy weights on every rollout would dominate the runtime. Pass an` on_start` loader so the weights land in VRAM once per container and every rollout that hits that container reuses them:


```text


```


The loader runs when the container boots, not when the rollout starts, so you pay the load cost a handful of times per batch instead of once per episode.


### Snapshot a prepared environment and clone it


When an environment needs heavy setup — a game build, a compiled simulator, a checked-out repo — you do not want to redo that work on every rollout. Snapshot the prepared sandbox once and restore the snapshot into each run so every rollout starts from an identical, ready state:


```text


```


Restore that snapshot with` Image.from_id(image_id)` for a filesystem clone, or use` snapshot_memory()` and` create_from_memory_snapshot()` when you want the process state captured too, so a warmed simulator restores in seconds rather than rebooting.


### Put a GPU inside the environment


Many modern RL setups run a model inside the environment itself: a reward model scoring outputs, a tool-using agent doing inference, or a GPU-accelerated simulator. Because the` gpu` argument is on the environment function, that inference runs on the same GPU as the rollout instead of round-tripping to a separate service on every step. For a reward model served with something like[vLLM](https://www.beam.cloud/blog/vllm) , point the environment at a local endpoint and keep the whole step on one accelerator.


### Stream async rollouts with a task queue


When rollouts arrive over time — an actor generating episodes continuously rather than in fixed batches — use a` task_queue` instead of` .map()` . You enqueue with` .put()` and Beam autoscales by queue depth:


```text


```


` retries=3` reruns a rollout that crashes on a bad environment state before giving up, which matters when a sweep launches millions of episodes and a few hit edge cases.


### Run rollouts on your own cloud


RL reward functions and training data are often proprietary. Because Beam's runtime,` beta9` , is open source and AGPL-licensed, you can run the same rollout API on your own AWS, GCP, or Azure account — using GPU credits you already have — or connect your own servers and source cheaper hardware, then burst to Beam's cloud when a sweep needs more capacity than you own. The environments stay inside your infrastructure, and there is no hard session cap, so a long training run is not cut off at a fixed timeout the way it is on session-limited sandboxes.


## What to look for in a serverless GPU for RL


Whatever platform you pick, the rollout half of the loop lives or dies on the same handful of properties. Evaluate against these rather than a headline GPU price.


- **Rollout parallelism.** How many environments run at once? This caps rollout throughput, which caps how fast the policy improves. Beam fans a single function out to thousands of concurrent containers from one` .map()` call.
- **GPU inside the environment.** Can a rollout touch a GPU for a reward model or an accelerated simulator, and which GPUs? Without it, every in-environment inference step round-trips elsewhere.
- **Churn cost.** Rollouts start and stop constantly. Cold-start latency, and whether you are billed for it, compounds across thousands of episodes per sweep. Beam does not bill for container spin-up or image-pull time.
- **Deployment control.** Can rollouts run in your own cloud account or on your own servers, so sensitive reward data and training sets stay put? This is where managed-only platforms force a trade-off.
- **Session and billing shape.** Per-second billing that scales to zero between updates beats a fixed fleet you pay for around the clock, and an indefinite session beats a hard timeout for long runs.


For a full ranking of sandbox platforms against these RL criteria, the[best sandbox providers for reinforcement learning](https://www.beam.cloud/blog/best-sandbox-providers-reinforcement-learning-2026) guide compares the field in depth.


## Serverless RL platforms compared


The platforms split into two shapes. Beam, Modal, RunPod, and Northflank give you a serverless GPU primitive you run your own environments and trainer on. CoreWeave's Serverless RL is a fully managed RL service: you bring a Weights & Biases account and an API key, and it runs the rollout-and-update loop for you, billed for the tokens it generates rather than per GPU-hour. Which one fits depends on whether you want to own the training loop or hand it off.


Platform GPU in the environment Rollout fan-out Run on your own cloud Session cap Shape


Beam Yes .map() and task queue Yes, open-source runtime None Serverless GPU primitive


CoreWeave Serverless RL Yes Managed by the service No, managed only Managed Fully managed RL loop


Modal Yes .map() and .spawn() No, managed only Managed Serverless GPU primitive


RunPod Serverless Yes Job queue plus a custom worker Customer-managed via sales Managed Serverless GPU primitive


Northflank Yes High-concurrency microVMs Yes, self-serve BYOC None Full platform


On raw GPU rate, an H100 on Beam is $1.74/hr versus $3.95/hr on Modal and $4.18/hr on RunPod serverless (list rates checked July 2026), and an A100 80GB is $1.30/hr on Beam versus $2.50/hr on Modal. Because RL rollouts are GPU-bound and run in the thousands, that per-hour gap is most of the bill on a large sweep. CoreWeave's per-token model is a different unit entirely, which makes it simple to start but hard to compare directly until you know your token volume. The wider field is covered in the[top serverless GPU providers](https://www.beam.cloud/blog/top-serverless-gpu-providers) roundup, and the[Modal pricing breakdown](https://www.beam.cloud/blog/modal-pricing-explained) digs into one of the rows above.


## FAQ


### What is a reinforcement learning environment?


It is the system the agent acts inside: it takes an action, advances its state, and returns an observation and a reward. In RL training you run that environment thousands of times per policy update to collect the experience the learner trains on, so the environment is the part of the loop you need to run at high volume and low cost.


### Do RL rollouts actually need a GPU?


Often, yes. Many setups put a model inside the environment — a reward model scoring outputs, an agent running inference, or a GPU-accelerated physics simulator like Isaac Gym. When the environment holds a model, the rollout needs a GPU or every step round-trips to separate infrastructure and adds latency. Classic tabular or lightweight-simulator environments can stay on CPU.


### Is serverless a good fit for RL training?


For the environments, yes; for the learner, not really. The policy update is long-running and holds weights in VRAM, so it belongs on a GPU you keep. The rollouts are bursty and massively parallel, which is exactly what serverless GPUs are built for. The practical pattern is to keep the learner on a fixed GPU and fan the rollouts out to serverless containers that scale to zero between updates.


### How is per-token RL billing different from per-GPU?


A managed RL service like CoreWeave's Serverless RL charges for the tokens the training loop generates, so you never see a GPU-hour. A serverless GPU primitive like Beam or Modal bills for the GPU seconds your rollouts run. Per-token is simpler to start and abstracts the hardware; per-GPU is cheaper and more predictable when your rollouts are heavy and you want to control the model and the environment yourself.


### Can I run RL rollouts on my own cloud?


With some platforms. Beam's runtime is open source, so you can run the same rollout API on your own AWS, GCP, or Azure account or connect your own servers, then burst to Beam's cloud for extra capacity. Northflank offers self-serve bring-your-own-cloud; RunPod handles customer-managed compute through sales; Modal and CoreWeave's Serverless RL are managed only.


### How many environments can I run in parallel?


It depends on the platform. Beam fans a single function out to thousands of concurrent containers from one` .map()` call, and you raise the ceiling by widening the input list rather than provisioning machines. Match the number to your rollout batch size per policy update — the bigger the batch you can run at once, the faster the policy improves.


## Get started


Run your next RL sweep on GPUs that turn off between policy updates.[Get started free on Beam](https://www.beam.cloud/?utm_source=usecase&utm_campaign=serverless-gpu-reinforcement-learning) — new accounts include $30 in credit refreshed monthly.
