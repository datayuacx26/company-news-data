---
schema_version: "1.0.0"
document_id: "6c7771b9035b4755c8cb40af59610279d6a92d3ad832ea54fd1305f9f9ec6bd0"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/what-is-a-container-really"
published_at: "2026-07-07T22:08:09+00:00"
first_seen_at: "2026-07-24T09:37:22.766203+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:748e468fb5313c90cdb85486beb52ae55c2c51366b67a794772121939f07808f"
---

# What Is a Container, Really? Five Years of GPU Infrastructure

We've been doing this for a while. We started the company with the idea that it should be dead simple to deploy models – an AWS Lambda for GPUs. Since then, we've faced almost every infrastructure problem there is. Let me start at the beginning.


## Starting naively on ECS


When we started building, we were building a cloud IDE, sort of like Replit for ML. For the first year or so, we were focused on making it easy to fork and launch machine learning models from your browser, so we invested heavily in the front end. The backend left a lot to be desired. I'd used ECS before for data pipelines and was familiar with it, so we started there.


Our ECS architecture was naive. We'd generate a Dockerfile for each user, bundle their code into it, and spawn an ECS container that embedded a small webserver alongside their code. It was simple, and it kind of worked. This is the demo we raised our pre-seed round with.


As soon as we started running real models, it stopped working. We kept ECS workers running, so it wasn't truly serverless. We just dynamically loaded and unloaded user code on demand to fake it. As we began onboarding our first users into production, we came across the core problem: cold start. That was the thing we'd be fighting (in one form or another) for years.


## Knative scaling woes


The cold-start problem pushed us toward a second version of the system, and toward Kubernetes. I tend to reach for the easiest available thing, so I went looking for something off-the-shelf, truly scale-to-zero, and Kubernetes-native. Knative fit, so we built V2 on top of it: a custom operator, CRDs for our deployment resources, and internally roughly the same server as before: dynamically loaded user code, and a separate image tag per deployment version.


Knative plus Kubernetes was a real improvement. It let us lean on the K8s ecosystem for things like proactive image caching, just warming the disk on nodes with a tool called kube-fledged. And around this time we did something we should have done much earlier: we instrumented the system end to end. We'd been so focused on getting something that worked that we knew when things were slow but not why. So we started shipping metrics.


The data confirmed our suspicions and ranked them. Three things were slow: starting GPU nodes, loading large images stuffed with CUDA dependencies, and loading model weights into RAM and then into VRAM. By far the biggest was starting the GPU node itself. We kept an overprovisioned warm pool to hide it, but that was expensive, so we spent real effort optimizing node bootstrap. That turned out to be wasted work. We were optimizing at the wrong layer. (In today's market it makes even less sense to optimize there: GPUs are scarce enough that you cling to whatever capacity you can get. But that's another story.)


I remember the moment when this became a hair-on-fire problem for us. One of our first production users was running a large inference workload, and at that stage we didn't even have a queuing system for inference. The way we "scaled" their workload was to add containers via Knative, with load balancing at the Kubernetes service level, in front of what was (at that point) just a FastAPI server. It was trash, and it failed in the most visible way possible. We'd set up a shared Slack channel with the customer, and they'd wired a hook to post a message every single time their pipeline threw an error. So we sat there getting bombarded with error messages, over and over.


We tuned every Knative setting there was. We forked it. We changed the autoscaling parameters, rewrote the proxy behavior, tried a month of small tweaks. None of it worked, and watching exactly where it broke is what produced the diagnosis the whole rest of this story follows from.


The problem was twofold.


First, Knative's autoscaling and request buffering were built for web traffic – they can't handle APIs where each request carries this much latency. You cannot scale an inference pipeline the way you scale a CRUD service; you're operating under false pretenses, and it will not work.


Second, even with perfect request buffering, container start and model load time were high enough that it wouldn't matter — you can't buffer your way out of not being able to start the container fast enough.


So there were two problems, not one. How fast can you start a container, and how do you autoscale and proxy requests for workloads that don't behave like web requests. We had to solve both.


(For what it's worth, the Knative fork was a short blip, about a month. Nobody really pushed back on the decision to leave it, mostly because nobody outside the company knew what we were doing internally.)


## What is a container, really?


The second problem (request routing, queuing, proxying) we solved by building our own control plane and leaving the Knative world entirely. But the first problem sent us somewhere stranger. We started asking a genuinely basic question: what is a container, really? And why are we pulling an entire image when we only ever touch a fraction of the files inside it?


A container, it turns out, is not much. It's a process group and a set of namespaces in Linux with a different root filesystem, and that root filesystem is just stacked layers composed through an overlay filesystem. Once you actually internalize that, you realize that if the container only reads a subset of the files, you don't need the whole image present to start it. You can lazy-load the contents on demand.


To do that you need to sit underneath the filesystem, which means FUSE. So we built two things. First, our own container runtime in Go that could spin up containers directly from a root filesystem. Essentially what Docker does under the hood. Second, a custom image format that let us lazy-load contents through that filesystem. This was our first real step toward mature infrastructure, and we bundled it, along with a new control plane that owned the full container lifecycle, into what we called Beam V1.


We also built a scheduler that sat above Kubernetes. We still relied on the Kubernetes scheduler for the workers that actually spawn containers, but our scheduler let us write custom logic around the attributes a given container needed to run. By this point the company itself had shifted: we'd moved off the browser IDE and become a Python SDK. Besides our developer experience, there was nothing flashy in the product anymore. It was entirely the system beneath the surface that mattered. Performance was the product.


## Why FUSE?


FUSE looks like a strange place to spend your engineering budget until you see what it buys you. The key fact is that loading a model image touches a very uneven slice of the files. Some are enormous. A CUDA library can be gigabytes. But a huge amount of what gets read is tiny: the long tail of __init__ files, .pyc files, the small Python files that make up an inference script. And a lot of the image never gets read at all, even though you're still decompressing and mounting it.


FUSE gave us three things.


**It made the access pattern visible.** Because FUSE intercepts every read, open, and lookup, mounting a root filesystem through it lets you see exactly which files a container touches as it runs. We could add hooks to record precisely what was required to load a given image.


**It made caching programmatic.** That same per-lookup hook meant we could proactively cache exactly the content that got accessed, however we wanted: on local disk, or (where we eventually landed) in a distributed networked cache.


**It decoupled storage from compute.** FUSE is just an interface; what it does underneath is arbitrary. The image contents don't have to live on the machine running the container. You could serve a layer from another node entirely. If you're spinning up fifty containers across a cluster, one node can serve the image instead of duplicating that storage on every server.


The result was subsecond container starts without the image ever living on the box running the container. And because the cache was content-addressed (keyed on the hash of the content) it produced a network effect. The more users we had, the better it got for everyone: two users pulling the same CUDA library or the same base image hit a cache the other had already warmed.


## V1 to V2: the long tail


Our first filesystem compressed everything down into a single layer. You could lazy-load individual files out of it and never store the layers themselves in the cache. It was a good enough implementation for about a year. But it had a structural flaw: by collapsing everything into one layer, you lose the cross-user network effect at build time, because you can no longer match on shared layer hashes, and most people are building on the same handful of base images.


So we built V2: a system that indexes within individual layers. Given an arbitrary file path, we can determine which layer it lives in based on how the layers stack, and (because of the index) the exact byte offset of that content in the decompressed layer. That means you don't build the overlay filesystem at all. You don't download every layer and assemble it. You go straight to the decompressed layer and pull the file out of the cache.


It's truly the best of both worlds: per-file seeking and cross-user caching. It also fixed builds. Compress everything to one layer and you rebuild the whole thing on every change; index individual layers and you get Docker's normal behavior back, where only layers above the lowest change rebuild, so builds get faster and multiple users share cached contents. That's why it's V2.


The interesting thing about going from V1-to-V2 is what it did and didn't improve. p50 latency stayed about the same. p90 probably did too. The entire gain was in p95 and p99. More cache hits means more requests that skip the slow path. We'd already gotten a taste of how fast the best case could be once V1 worked; V2 was about making that speed the common case, for more users, most of the time.


## The trustless binary


We run across a lot of different cloud providers, chasing cheaper and more available hardware, and the system that got us this far was annoying to deploy. We had Kubernetes installed on every GPU node, with daemonsets orchestrating the caching layer, credential management, and the rest, all rolled out with tools like Flux. It worked, but every new pool of hardware made it more painful. We wanted something we could stand up with one dumb command.


As we went through SOC 2, the auditors' questions forced us to address questions we hadn't really asked ourselves: what does a worker actually need to know to run a workload, and where are the boundaries of the system? Once we looked, the gaps were everywhere. The worker had a direct connection to Redis, which meant it needed Redis credentials. It had direct database access to update things in Postgres. Every time we changed anything, the service boundary turned out to be in the wrong place. You really don't want a remote machine (possibly on someone else's cloud) storing your Redis credentials.


We closed the gaps, one by one. Today, a node comes up with a temporary token that authenticates it to the control plane, spins up its own copy of the distributed cache and its own worker, and mounts the image filesystems to run containers. There are no daemonsets on remote nodes. No Kubernetes on remote nodes. No saved credentials. If a remote node is compromised, it can't touch other users or other nodes.


There are fewer dependencies, fewer services to see, fewer credentials to store, and fewer networking decisions to make.


We didn't set out to build something you could run anywhere. We set out to make our own lives less annoying and our security boundary defensible. But the benefit of shrinking our footprint this small is that people mostly interact with our system as a binary you can drop onto any machine and run. Which, if you go back to the very beginning, is actually quite close to what we wanted to build in the first place. Something dead simple, for GPUs, that runs wherever and whenever you need it.


> If you're interested in seeing the infra in action, you can get started for free[right away](https://platform.beam.cloud/) . And if the sorts of problems in this article excite you,[we're hiring](https://www.ycombinator.com/companies/beam/jobs) .
