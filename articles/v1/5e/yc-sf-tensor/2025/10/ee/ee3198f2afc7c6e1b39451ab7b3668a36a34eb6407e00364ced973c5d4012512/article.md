---
schema_version: "1.0.0"
document_id: "ee3198f2afc7c6e1b39451ab7b3668a36a34eb6407e00364ced973c5d4012512"
company_key: "yc-sf-tensor"
company: "SF Tensor"
source_id: "yc-sf-tensor-news-import-88ed66988698"
canonical_url: "https://sf-tensor.com/news/tensor-cloud-launch"
published_at: "2025-10-06T00:00:00+00:00"
first_seen_at: "2026-07-22T13:14:18.119837+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:d698bc0b13d1fe014ee190a2ba6c44e8a29240a39e438bae2eb224f6b4c87183"
---

# Tensor Cloud Launch

Remember when cloud computing was supposed to be simple? Just spin up what you need, when you need it. Turns out that's bullshit if you're doing anything serious with AI.


Try running workloads across different providers and you'll quickly discover that GPU availability is completely random, prices swing wildly every few hours, and managing AWS, GCP, Azure, plus all the weird specialty providers basically requires a degree in cloud computing.


We got sick of it.


## Introducing Tensor Cloud


So we built Tensor Cloud. It's basically one interface for all the cloud providers. You don't need to worry about the caveats of every single provider you try.


What it does:


- Finds cheap GPUs automatically by watching prices across clouds and neoclouds
- Deals with preemptible (spot) instances, by moving your job automatically (saving up to 80% on costs)
- A simple web interface so you never have to touch the provider


[Tensor Cloud demo](https://sf-tensor.com/images/demo_loop.gif)


## How It Works


1. You tell us what and how much you need (H100s, A100s, TPUs, Trainium, whatever)
2. We find the cheapest place to get it
3. Your training runs
4. The results get pushed to your storage bucket, downloaded to your computer, or persisted to the local cloud provider's persistent storage - you choose.


You can use your credits for the chosen cloud provider, or pay through us.


## The Future of Compute


This is obviously just the beginning. Cloud infrastructure should be like electricity — you flip a switch and it works. You shouldn't need to know which power plant your electricity comes from.


Next up, we're building features that calculate which available hardware is best for your training run and automatically optimize your model to speed up training, allowing you to harness the full power of GPUs and TPUs irrespective of the manufacturer.


Compute should be boring. Let's make it boring.
