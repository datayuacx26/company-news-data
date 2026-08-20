---
schema_version: "1.0.0"
document_id: "25ff9578230909b7274f8f13c0729f13a11f655eadd5b03dd1ef976d578ffdb4"
company_key: "yc-thunder-compute"
company: "Thunder Compute"
source_id: "yc-thunder-compute-news-import-c1dcc13dd65c"
canonical_url: "https://www.thundercompute.com/blog/how-thunder-compute-works-gpu-over-tcp"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-22T16:40:14.192629+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:6a0fa8c91e8bd9bf816dfe441b98ce95b7ac8ee9447cf1e03e9660844d3b4635"
---

# How Thunder Compute works (GPU-over-TCP)

## 1. Why make GPUs more efficient?


GPUs are expensive and often sit idle while developers read logs or tweak hyper-parameters. With Thunder Compute, instead of your GPU sitting there doing nothing, it detaches from your server. When you need a GPU again, your workload transparently claims a GPU, on the order of double-digit milliseconds. This is different from a scheduler like slurm; everything happens behind the scenes, in real time, without waiting.


## 2. How does Thunder Compute work?


- **Network-attached** : The GPU sits across a high-speed network instead of a PCIe slot. Each virtual machine communicates with its GPU over TCP via the data center's network fabric.
- **Invisible to the workload** : This virtualization sits at the CUDA layer, below the inference, training, or other GPU workload. This means that each program stays the same while behind the scenes our virtualization layer translates CUDA calls into network messages.
- **Sole tenancy** : When a process on an instance uses a GPU, that GPU is entirely dedicated to that instance. The instance has access to the full VRAM and compute of the card for the duration of the process. When the process exits (or sits idle), the GPU is assigned to another workload.


## 3. Does this affect latency?


Thunder Compute has a negligible effect on latency in the conventional sense. Establishing an initial connection with a GPU takes ~10-20 milliseconds (blinking is ~200 milliseconds), latency which is only incurred once on initial program startup. The main potential impact is not to latency but rather runtime, as slight network delays add up across thousands of CUDA calls making the overall program take longer to run. Fortunately, this is something we are able to heavily optimize at the systems level. By strategically tuning the way your program runs behind the scenes, we can prevent network latency from affecting your GPU computation. For common workloads this impact is negligible; for less common edge cases you can see a slowdown of ~2x from native. Even in the worst cases we find that the slowdown is dramatically outweighed by efficiency improvements across the cluster.


## 4. How much efficiency does this add?


Within our cloud we can serve ~1.8x more users on our GPU fleet than we would be able to without virtualization. This means we get nearly double the revenue from the same GPUs. In many cases the benefit is much larger, particularly for I/O-bound agentic workloads such as GPU sandboxes, or fleets with long-term reservations that leave instances running for months. Because our instances are entirely on demand, fewer sit idle, so 1.8x oversubscription is on the lower end of what we see in other GPU fleets.


## 5. Is Thunder Compute secure?


As a group of systems nerds we care a lot about security. When a job ends, we wipe GPU memory and reset the card so no data leaks to the next user. Single tenancy provides strong guarantees here.


## 6. Learn more


If you're interested in learning more, or if you're interested in deploying Thunder Compute's virtualization to increase revenue in your fleet of GPUs,[contact us](https://www.thundercompute.com/contact) !
