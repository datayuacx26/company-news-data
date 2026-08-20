---
schema_version: "1.0.0"
document_id: "f4beba8808cd50b6a3d0cb3c1bf482f83b5415ba62f3775a7eebb3e146bd142e"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/how-we-cut-gpu-cold-starts-from-minutes-to-seconds-with-memory-checkpointing"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:115f0d76a5a039e102ecae9dd020e01bb47aad59c6277dba5f788ebf50acd800"
---

# How we cut inference cold start to seconds with NVIDIA Dynamo

***Photoroom cut GPU inference cold starts from ~220s to 35–45s using NVIDIA Dynamo Snapshot. The approach snapshots a fully-loaded process with NVIDIA's cuda-checkpoint and CRIU, then restores it on any GPU in a Kubernetes fleet. Here's how we wired it into LitServe and ArgoCD.***


At Photoroom, we run dozens of computer vision and generative AI models in production. Every background removal, studio scene, retouch, or expand is powered by its own model, each with its own latency budget and traffic pattern.


Some models run under continuous load; others are bursty or rarely touched. For the quiet ones, we scale to zero, because there's no sense holding an expensive GPU idle for a model that gets a handful of requests an hour.


Scale-to-zero has one well-known downside: the cold start. The first request after a scale-up pays the full cost of bringing a replica online, and for a large diffusion or transformer model that can mean several minutes. This post covers how we cut that time to seconds using GPU memory checkpointing.


## What actually happens when a container serving with LitServe starts


[LitServe](https://lightning.ai/litserve) is an open-source, PyTorch-native serving framework from[LightningAI](https://lightning.ai/) : you just fill in` setup()` to load the model and` predict()` to run inference, and it handles the HTTP server, batching, and workers. We reach for it whenever we want to get a model served quickly. The trade-off: it loads models sequentially, one` setup()` run start to finish before the server accepts a request, whereas frameworks like Triton load them in parallel. That serial cold start is why we focus on LitServe here, and why a fast restore helps so much.


When a LitServe container starts, it will not serve a single request until the model is fully loaded. It launches a worker process, runs your` setup()` inside it, and only brings up the HTTP server once that worker reports READY.` setup()` is where all the heavy lifting happens, roughly in this order:


1.


Import the inference libraries (PyTorch, TensorRT, diffusers, …).


2.


Download the model weights.


3.


Move the weights from host memory into GPU memory.


4.


*(Optional)* Warm up the model with JIT /` torch.compile` or CUDA graph capture.


Only then does the HTTP server come up and start serving.


Where does the time actually go? On our setup: imports take about **15 s** (the ML libraries are surprisingly heavy to load); downloading a ~55 GB model at ~1 GB/s (~8 Gbps) is about a minute, or zero if the weights ship in the image or on a mounted volume; moving them host-to-GPU is only a second or two, because the link is PCIe Gen5 x16 (~50 GB/s); and warm-up depends entirely on the model, from near-zero for an AOT-compiled engine to ~40 s for our regionally-compiled (` compile_repeated_blocks` ) image-edit pipeline.


Where a cold start's time goes: the setup() phases and their rough durations


For a typical large diffusion or transformer setup, expect tens of seconds of imports, seconds to a few minutes of download depending on the bandwidth to the storage that contains the weights, a second or two to load the weights onto the GPU, and anywhere from typically several minutes of warm-up. Put together, that is a few minutes on every cold start, and most of it (the download and warm-up) is repeated every single time.


### What is GPU memory checkpointing?


What if we could freeze the process *after* all of this is done, and thaw it back on the next cold start? That's memory checkpointing. In short, GPU memory checkpointing freezes a fully-loaded inference process, including its GPU memory, to disk, and restores it on a later cold start instead of reloading and recompiling the model. Serverless platforms have used CPU-memory checkpointing for years, but GPU state was out of reach until NVIDIA shipped its` cuda-checkpoint` utility. It's been around for a little while, but only from driver r580 onward can a checkpoint be restored onto a different physical GPU than the one it was captured on, which is exactly what you need in a dynamic, autoscaled fleet.


## Memory checkpointing in one picture: producer and consumer


## How to add GPU memory checkpointing to a LitServe app


### NVIDIA Dynamo Snapshot


[NVIDIA Dynamo](https://github.com/ai-dynamo/dynamo) is a framework for orchestrating LLMs at scale, with backends for vLLM, SGLang, Tokenspeed and NVIDIA TensorRT LLM, and features like disaggregated serving, prefix-aware routing to maximize KV-cache hits, and efficient cross-node KV-cache transfer.


In June 2026 NVIDIA shipped stable memory checkpointing for Dynamo (v1.2) as a standalone[Helm chart](https://github.com/ai-dynamo/dynamo/tree/main/deploy/helm/charts/snapshot) , after previewing it in March. Photoroom, a member of the[NVIDIA Inception](https://www.nvidia.com/en-us/startups/) program for startups, worked with NVIDIA to integrate this capability into their production infrastructure. It combines the CUDA driver's native checkpointing capability` cuda-checkpoint` (command-line utility that saves and restores a running process's GPU state) with[CRIU](https://criu.org/) (open-source Linux tool that freezes a running process to disk and restores it) to snapshot a fully-loaded process and restore it later, potentially on another node and another GPU. It's also native to Kubernetes.


**Requirements:** a cluster with the[Dynamo Snapshot v1.2.0 chart and agent](https://github.com/ai-dynamo/dynamo/tree/main/deploy/helm/charts/snapshot) installed (driver r580+, a fast shared RWX volume, and the rest of[their prerequisites](https://github.com/ai-dynamo/dynamo/tree/main/deploy/helm/charts/snapshot#prerequisites) ), plus your inference image built on a glibc 2.38+ base, so the CRIU / cuda-checkpoint binaries run.


### What we reuse (and what we don't)


Dynamo exposes checkpointing at three levels:


1.


A low-level` snapshotctl` CLI, meant for debugging and validation.


2.


A` DynamoCheckpoint` CRD driven by the Dynamo **operator,** the primary interface that launches the producer Job and wires restore through a mutating webhook.


3.


The **snapshot-agent** itself, which restores a process based on pod labels.


We reuse only the agent.


What we take from Dynamo Snapshot: only the snapshot-agent


The current Dynamo operator's` DynamoCheckpoint` path assumes it owns the serving workload through its own CRDs (` DynamoGraphDeployment` /` DynamoComponentDeployment` ). Our models, however, are LitServe apps served as ArgoCD Rollouts from our own Helm chart, with specific canary steps, autoscaling and init containers that Dynamo's CRDs do not model, so we can't put the Rollout under the operator. Checkpointing can run outside the operator (using standalone` DynamoCheckpoint` CRs), but its production path is coupled to Dynamo's serving stack.


For our case, we install just the snapshot-agent from the Dynamo Snapshot chart, stamp its restore labels on our Rollout ourselves, and drive the produce step with our own Job (the sync-wave flow below).


### How checkpoint and restore actually works


A per-node **agent** (DaemonSet) drives checkpoint and restore, coordinating with the process through sentinel files on a shared control volume:


1.


Once the process is fully loaded and serving, it writes a` ready-for-checkpoint` file.


2.


The agent runs` cuda-checkpoint` to move GPU memory back to the host, then CRIU dumps the whole process to the shared volume. It writes to a temporary directory and atomically renames it only on success, so a partial dump is never mistaken for a valid checkpoint.


3.


To restore, a placeholder pod starts, the agent CRIU-restores the process into it and toggles` cuda-checkpoint` back. The process resumes exactly where it was frozen, with the model already in VRAM.


The checkpoint/restore handshake: the process and the agent coordinate through sentinel files


The checkpoint/restore binaries (` criu` ,` cuda-checkpoint` , and the` nsrestore` namespace helper) have to be present in the container, so we wrap our inference image in a thin one that copies those binaries in. The runtime identity, and therefore the checkpoint, is still driven by the normal inference image. In practice it is a few lines of Dockerfile, and the binaries come straight from the pinned, public snapshot-agent image, so we never build CRIU or the CUDA plugin ourselves:


```text
ARG BASE_IMAGE
ARG SNAPSHOT_AGENT_IMAGE=nvcr.io/nvidia/ai-dynamo/snapshot-agent:1.2.0


FROM ${SNAPSHOT_AGENT_IMAGE} AS snapshot


FROM ${BASE_IMAGE}                     # must have glibc 2.38+ (see below)
USER root


# CRIU runtime dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
libbsd0 libcap2 libnet1 libnl-3-200 libnl-route-3-200 libprotobuf-c1 \
libgnutls30t64 libnftables1 iproute2 iptables procps uuid-runtime \
&& rm -rf /var/lib/apt/lists/*


# copy the prebuilt binaries + CRIU plugins from the agent image
COPY --from=snapshot /usr/local/sbin/criu            /usr/local/sbin/criu
COPY --from=snapshot /usr/local/sbin/cuda-checkpoint /usr/local/sbin/cuda-checkpoint
COPY --from=snapshot /usr/local/bin/nsrestore        /usr/local/bin/nsrestore
COPY --from=snapshot /usr/local/lib/criu             /usr/local/lib/criu


RUN criu --version   # fail the build early if the base image's glibc is too old


```


The same image runs on both the checkpoint-source pod and the restore pods; only the command differs. The source pod runs the real server, loads the model, and is dumped, while a restore pod runs a` sleep` placeholder that the agent CRIU-restores the checkpointed process into. They cannot use different images: CRIU restores the process's memory, but the restored process still references the executables and shared libraries on disk, so the restore filesystem has to match the source exactly.


### Connecting LitServe to Dynamo Snapshot


Dynamo ships sentinel hooks for vLLM, SGLang and TensorRT LLM. For LitServe we add our own. It comes down to two pieces: a small lifecycle **hook** that speaks the sentinel protocol, and making` setup()` **checkpoint-safe** .


The hook waits until the server is serving, signals` ready-for-checkpoint` , and blocks until the agent is done. Where and how it runs is deliberate: in the main process (PID 1), as a daemon thread, and only *after* the server starts (never inside` setup()` ). PID 1 is the process the agent checkpoints as a whole (and the one it signals to exit on the source pod); the daemon thread keeps it from blocking the server; and the timing guarantees we freeze a fully-loaded process, since LitServe marks a worker READY only after` setup()` returns.


```text
# snapshot_lifecycle.py -- speaks Dynamo's sentinel protocol from LitServe
def start_checkpoint_lifecycle(port: int) -> None:
# only active in a checkpoint pod (control dir set by the agent); no-op otherwise
control_dir = os.environ.get("DYN_SNAPSHOT_CONTROL_DIR")
if not control_dir:
return
threading.Thread(target=_run, args=(control_dir, port), daemon=True).start()


def _run(control_dir, port):
# 1. wait until every worker is READY (/health 200) -- the state we want frozen
while not _health_ok(port):
time.sleep(1)
# 2. tell the agent we are ready to be checkpointed
Path(control_dir, "ready-for-checkpoint").write_text("ready")
# 3. block until the agent writes snapshot-complete or restore-complete
while not (snapshot_complete_exists() or restore_complete_exists()):
time.sleep(0.1)
# 4. which sentinel fired tells us our role
if snapshot_complete_exists():
# source pod: exit 0 so the producer Job completes and frees the GPU
os.kill(1, signal.SIGINT)  # PID 1 is LitServe -> clean exit 0
# restored pod: nothing to do, just keep serving


```


The helpers are deliberately tiny:


-


` _health_ok(port)` does a local` GET <http://127.0.0.1>:{port}/health` and returns` True` on a` 200` . That is how the hook knows every worker has finished` setup()` and the process is in the exact state we want to freeze (anything earlier would snapshot a partially-initialized process).


-


` snapshot_complete_exists()` and` restore_complete_exists()` just check whether the agent has dropped a` snapshot-complete` or` restore-complete` sentinel file into the control directory. The agent writes` snapshot-complete` on the source pod once the dump finishes, and` restore-complete` on a restored pod once it is thawed. The same hook code runs in both roles, so those two files are how it tells which side it is on and reacts accordingly: exit 0 to finish the producer Job, or simply keep serving.


The LitServe app defers every GPU-touching import into` setup()` and starts the hook from` __main__` :


```text
import litserve as ls


class InferencePipeline(ls.LitAPI):
def setup(self, device):
# model load here
# keep GPU-touching imports inside setup() (not at module top level) so only this
# worker process calls cuInit -- that is what makes the process checkpointable
self.model = load_model().to(device)


def predict(self, request):
# inference here
return self.model(request)


if __name__ == "__main__":
server = ls.LitServer(InferencePipeline(), accelerator="auto",
workers_per_device=LS_WORKERS)
start_checkpoint_lifecycle(port)   # after the server starts, from PID 1
server.run(port=port)


```


### Execution order: from checkpoint to serving


This does not require ArgoCD: all you need is a way to run the checkpoint producer to completion before the serving replicas start, so any orchestrator (plain Kubernetes Jobs, Argo Workflows, a CI step) works. We happen to express it with ArgoCD sync-waves, so a deployment builds and consumes its own checkpoint with no manual steps:


1.


**Wave 0, produce.** A checkpoint-producer` Job` starts an ordinary-looking pod, loads the model, and the hook signals` ready-for-checkpoint` . The agent dumps the process to the shared volume and the Job exits 0.


2.


**Wave 1, restore.** The serving` Rollout` comes up only after the checkpoint exists. Each replica is a placeholder the agent restores into (~35s) and then serves. Scale-ups and reschedules restore the same way.


The checkpoint id is a hash of everything that affects the runtime (image tag, driver version, GPU type, model, plugins), so a rebuild mints a new id automatically and a stale checkpoint is never restored by accident. The producer is idempotent: if the checkpoint already exists, wave 0 is a fast no-op.


How a deployment builds and consumes its own checkpoint


## What to keep in mind


1.


**State or static resources baked into the process.** Anything resolved once at startup gets frozen into the checkpoint and is stale after a restore onto another node. The clearest example is Datadog in` hostip` mode:` DD_AGENT_HOST` is read once, so the checkpoint captures the *source* node's IP and every trace is dropped after restore. The fix is to reach the agent over a node-invariant path (a Unix Domain Socket) instead of the host IP, so a restored process still finds it wherever it lands.


2.


` cuInit` **and**` torch.compile` **.** Import anything that calls` cuInit` inside` setup()` , so only the worker process touches the GPU. And` torch.compile` 's inductor backend spawns asynchronous compile-worker subprocesses that create UVM/IPC memory` cuda-checkpoint` cannot dump (CRIU fails with` handle_device_vma` ). Force single-threaded compilation with` TORCHINDUCTOR_COMPILE_THREADS=1` ; it is a one-time cost on the producer pod only, restored replicas never recompile.


3.


**The shared volume has to be fast, or it is not worth it.** Restore streams the entire checkpoint (tens of GB) back from the shared filesystem, so its throughput sets a hard floor on restore time: Photoroom's ~60 GB checkpoints restore in ~35 s, about 1.3 GB/s. On slow storage a restore can take longer than loading the model from scratch, and we saw restore times balloon when the volume was under load, so treat a high-throughput RWX filesystem as a hard requirement.


## Results


We measured two representative models on Photoroom's Nebius GPU cluster, each producing a checkpoint of around 60 GB.


**In Photoroom's fleet, restore time is fast and consistent, around 35 to 55 seconds regardless of model** , because it's just streaming the process image back and toggling GPU state, independent of how expensive the original load + compile was. The without-checkpointing figures line up with the setup() phases from earlier: the ~220 s warm-image case is imports + weight load + host-to-GPU + ~40 s of compile, and the ~430 s cold-node case adds the one-time ~11 GB image pull and a from-scratch weight download.


Where a cold start's time goes: phases and their rough durations


Scenario


Without checkpointing


With checkpointing


Container ready (image already on node)


~220 s (load +` torch.compile` )


**~35 to 45 s** (restore)


Fully cold node (incl. ~11 GB image pull)


~430 s


**~195 s**


Scaling a` torch.compile` Qwen image-edit model from zero: restore completed in **~45 s** versus **~220 s** for a normal load-and-compile on a warm-image node, roughly **4 to 5x faster** , and the win grows with how expensive the model's warm-up is.


Scaling an AOT-compiled model to **4 fresh replicas at once** (all on cold nodes): each was ready in **~195 s** , of which the CRIU **restore was only ~35 s** . The other **~160 s was the 11 GB image pull** . In other words, once checkpointing removes the load-and-compile cost, the dominant remaining term for scale-up on brand-new nodes is simply pulling the image.


## Limitations


Memory checkpointing is powerful but not universal, and the tooling is still young:


-


**Single GPU per replica.**` cuda-checkpoint` cannot dump the IPC/UVM memory that multi-GPU runtimes rely on, and Dynamo Snapshot's current release supports single-GPU vLLM/SGLang-style workloads only; multi-GPU and multi-node (via quiesce/resume hooks for PyTorch, NVIDIA NCCL, NVIDIA NIXL, etc.) is on their roadmap but not shipped yet. So we use it only for models that fit on one GPU.


-


**The checkpoint is pinned to the exact image, driver, and GPU type.** That is by design (the content-addressed id), but it means every rebuild re-produces the checkpoint.


## Next steps


-


**Attack the image pull** , which is now the bottleneck on cold nodes:` eStargz` lazy pulling and better layer/host-cache locality are the options we'd like to evaluate here. This lever is independent of checkpointing and would benefit every deployment.


-


**Explore host-cache locality for the checkpoint itself** , keeping a hot copy on the node rather than always reading the full image from the shared volume, to push restore below the current ~35 s.


## Wrapping up


Nvidia Dynamo Snapshot turned cold starts that used to take minutes into a ~35 to 45 second restore, independent of how heavy the model's load and compilation are. The interesting part wasn't the checkpointing itself but everything around it: keeping node-local state out of the frozen process, making` torch.compile` checkpoint-friendly, and wiring the produce-and-restore flow into our GitOps setup (ArgoCD) so the whole thing stays declarative. With that in place, the remaining frontiers for scale-from-zero are the image pull and the node startup itself, which we will cover in future posts.


For Photoroom this turns scale-to-zero from a cost lever we avoid into one we can actually use: bursty models keep their GPUs idle and unbilled when there is no traffic, yet the first request after a scale-up is served in seconds instead of minutes, which is exactly the promise our product is built on.
