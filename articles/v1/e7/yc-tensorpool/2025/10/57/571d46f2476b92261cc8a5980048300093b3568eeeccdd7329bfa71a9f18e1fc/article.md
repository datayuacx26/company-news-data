---
schema_version: "1.0.0"
document_id: "571d46f2476b92261cc8a5980048300093b3568eeeccdd7329bfa71a9f18e1fc"
company_key: "yc-tensorpool"
company: "TensorPool"
source_id: "yc-tensorpool-news-import-ce508c9f2d8e"
canonical_url: "https://tensorpool.dev/blog/fast-nfs-storage-ml-training"
published_at: "2025-10-09T00:00:00+00:00"
first_seen_at: "2026-07-24T13:32:36.763199+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:0d1084600ec3ca989006386715a7de14912d0ca6fd1003dd282465ae14c602a9"
---

# Why Fast NFS Storage Matters for ML Training

When scaling ML workloads across multiple nodes, storage performance often becomes the hidden bottleneck. You've invested in expensive H100s or B200s, but your GPUs sit idle waiting for data.


## The Problem


Traditional cloud storage struggles with distributed ML training. AWS EBS and Google Persistent Disk typically deliver ~250 MB/s reads with limited IOPS. When you're paying $3/hour per GPU, having your GPUs wait on storage is an expensive inefficiency.


## TensorPool's NFS: Built for Speed


We've engineered our NFS infrastructure specifically for the demands of ML workloads. Here's what sets it apart:


### Performance That Scales


For a **100TB volume** , you get:


- **43 GB/s reads** - Load data faster than your GPUs can consume it
- **11 GB/s writes** - Checkpoint without slowing down training
- **150k read IOPS** - Handle millions of small files efficiently
- **75k write IOPS** - Support intensive logging and metrics


Need even more? Our **1000TB+ volumes** deliver:


- **322 GB/s reads** - Saturate even the largest multi-node clusters
- **161 GB/s writes** - Checkpoint massive models in seconds
- **1.5M read IOPS** - Support hundreds of concurrent workers
- **750k write IOPS** - Handle extreme write-intensive workloads


### Why This Matters


Let's put these numbers in context:


**Loading a 100GB dataset:**


- AWS EBS (~250 MB/s): **~7 minutes**
- TensorPool NFS (43 GB/s): **~2.3 seconds**


**Checkpointing a 50GB model:**


- Traditional cloud storage (~250 MB/s): **~3.5 minutes**
- TensorPool NFS (11 GB/s): **~4.5 seconds**


When you checkpoint every epoch, these seconds add up to hours saved over a training run.


## On-Demand Multi-Node: The TensorPool Advantage


Here's what sets TensorPool apart: **true on-demand access to multi-node clusters** .


Other providers force you to choose between long-term reservations or single-node instances. Want a 4-node H100 cluster? You'll need to commit to weeks or months upfront, or you simply can't get it.


TensorPool's architecture changes this. Because our high-performance NFS storage is independent from compute, we can offer genuinely on-demand multi-node clusters:


- Spin up 2 nodes for a quick experiment
- Scale to 4 nodes when you're ready to train
- Jump to 8 nodes for your production run
- Scale back down to 0 when you're done— **storage persists**


No long-term commitments. No data migration between cluster sizes. Your datasets and checkpoints stay on blazing-fast shared NFS while you elastically scale compute to match your workload.


This is what on-demand multi-node training should look like: **instant access to clusters of any size, backed by persistent high-performance storage** .


## Getting Started


Setting up high-performance NFS with TensorPool takes just a few commands:


```text
# Create a 100TB NFS volume
tp nfs create -s 100000 --name ml-data


# Create a multi-node cluster
tp cluster create -i ~/.ssh/id_ed25519.pub -t 8xH100 -n 4 --name training


# Attach NFS to your cluster
tp nfs attach <storage_id> <cluster_ids>


```


Your NFS volume will be mounted at` /mnt/nfs-<storage_id>` on all nodes, ready for immediate use.


## Conclusion


Don't let slow storage bottleneck your GPU investment. TensorPool's high-performance NFS delivers speeds up to 322 GB/s and 1.5M IOPS, combined with the flexibility to scale your compute on-demand without data migration.


Ready to stop waiting on I/O?[Get started with TensorPool](https://tensorpool.dev/) .
