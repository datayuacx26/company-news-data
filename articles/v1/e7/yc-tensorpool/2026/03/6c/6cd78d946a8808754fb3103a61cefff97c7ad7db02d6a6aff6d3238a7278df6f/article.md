---
schema_version: "1.0.0"
document_id: "6cd78d946a8808754fb3103a61cefff97c7ad7db02d6a6aff6d3238a7278df6f"
company_key: "yc-tensorpool"
company: "TensorPool"
source_id: "yc-tensorpool-news-import-ce508c9f2d8e"
canonical_url: "https://tensorpool.dev/blog/introducing-nb-dev"
published_at: "2026-03-12T00:00:00+00:00"
first_seen_at: "2026-07-24T13:32:36.763199+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:6ea82b19a8005b774ecaac00e79ad167a5ecfee5b9224fc8ac9933c0c2766604"
---

# Introducing nb.dev: A Cracked Jupyter Notebook for ML

Jupyter notebooks have been an integral part of engineering workflows since their inception. But for ML, they're broken. Reproducibility issues, flaky kernels, and no ability to scale experiments — every ML engineer knows the pain. We built[nb.dev](https://nb.dev/) to fix this.


## What is nb.dev?


nb.dev is an ML-first notebook that introduces three capabilities that change how you experiment:


1. **GPU Attach** — Instantly attach a GPU to any notebook
2. **Machine Snapshots** — Capture your *entire* machine state: everything in GPU memory, RAM, model weights, optimizer state, and data streams
3. **One-Click Branching** — Branch your notebook onto as many new machines as you want, each resuming from the exact snapshot


Unlike traditional checkpoint tools, our snapshots are async and faster, capturing the full machine state so you never lose context mid-experiment. And because branching is so easy, iteration is dramatically faster.


## The Problem: Sequential Experimentation


Consider a typical ML workflow. You're finetuning a model and hit a loss plateau. You have three hypotheses for how to push past it — maybe adjust the learning rate, try a different schedule, or add some regularization.


Traditionally, you'd have to test these **one at a time** . Restart from a checkpoint, wait for training to converge (or not), then try the next idea. Each attempt takes the full training time, and you're blocked while it runs.


This sequential bottleneck is one of the biggest time sinks in ML research.


## The Solution: Branch and Run in Parallel


With nb.dev, when you hit that inflection point, you snapshot the entire machine state and branch your notebook onto multiple machines — each one picking up exactly where you left off.


Here's what that looks like in practice with a Qwen3 finetune:


### 1. Train to the Inflection Point


Set up your training configuration and run until you see the loss flatten. In our demo, we trained with an initial learning rate of 0.02 and checked in at step 500, where the loss plateaued at around 7.


### 2. Snapshot and Branch


Instead of guessing which hypothesis to try first, we snapshot the full machine state and branch the notebook twice — giving us three identical copies of the experiment, each with the model weights, optimizer state, and data stream fully intact.


### 3. Test Hypotheses in Parallel


Each branch gets a different experiment:


- **Branch A** : Keep the learning rate the same
- **Branch B** : Decrease the learning rate significantly
- **Branch C** : Add cosine decay scheduling


All three run simultaneously on separate machines. No restarting from checkpoints. No re-running the first 500 steps. Each branch resumes instantly from the exact state where you left off.


### 4. Compare Results


When all branches finish, you compare the training curves side by side and pick the winner. What would have taken 3x the time sequentially now takes 1x. And you have full confidence that each experiment started from an identical state.


## Why This Matters


This isn't just a faster notebook. It's a paradigm shift in how people experiment with ML.


- **Faster iteration** : Launch many experiments in parallel with minimal overhead
- **Perfect reproducibility** : Every branch starts from the exact same machine state — no checkpoint loading bugs, no environment drift
- **Team collaboration** : Find a version of your notebook you like? Share it with someone on your team instantly
- **Agent-ready** : We're engineering this to make it easy for autonomous research agents to branch and quickly make discoveries


## Get Started


Try it today at[nb.dev](https://nb.dev/) .
