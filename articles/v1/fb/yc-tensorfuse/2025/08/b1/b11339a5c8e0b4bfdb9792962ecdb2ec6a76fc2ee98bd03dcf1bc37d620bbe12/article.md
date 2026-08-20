---
schema_version: "1.0.0"
document_id: "b11339a5c8e0b4bfdb9792962ecdb2ec6a76fc2ee98bd03dcf1bc37d620bbe12"
company_key: "yc-tensorfuse"
company: "Tensorfuse"
source_id: "yc-tensorfuse-news-import-97ab2b67de47"
canonical_url: "https://tensorfuse.io/docs/blogs/inference_on_k8s_1"
published_at: "2025-08-24T00:00:00+00:00"
first_seen_at: "2026-07-24T04:04:56.878556+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:eb3c7d8c793c7a7f311643bfe8ba7b366b4c3fb075225cb655c6feaed2dccb32"
---

# How Tensorfuse Launches AI Inference Containers in Milliseconds on Kubernetes

Aug 24, 2025


Samagra Sharma


Founder


The standard workflow for starting a container on a Kubernetes node involves the node’s container runtime, typically` containerd` , pulling the complete container image from a registry. This process is a significant bottleneck for AI workloads, whose images often exceed 20 GB due to large model weights and dependencies like CUDA and PyTorch.


The typical startup sequence consists of three time-consuming, sequential steps:


- **Download** : Transferring all image layers from the remote registry to the node. This is network-bound.


- **Decompress** : Unpacking each gzipped layer. This is CPU-bound and often single-threaded.


- **Write & Mount** : Writing the decompressed files to the node’s local disk and constructing a union filesystem using a snapshotter like` overlayfs` . This is I/O-bound.


This entire process must complete before the container’s` ENTRYPOINT` can execute. For a 20 GB image, this sequence can take over 10 minutes. However,[typically only a small fraction of the image data is required](https://www.usenix.org/conference/fast16/technical-sessions/presentation/harter) for the application to initialize. This inefficiency leads to long cold start times, forcing teams to overprovision expensive GPU resources to keep “warm” instances available.


## ​


Tensorfuse Architecture: On-Demand File Access


Tensorfuse solves this problem by implementing a` containerd` remote snapshotter. It replaces the default download-and-unpack model with an on-demand, lazy-loading mechanism. This is achieved through two core components: a build-time image indexer and a runtime FUSE-based daemon.


### ​


1. Build-Time: Creating a Seekable Image Index


The primary obstacle to lazy-loading is the[OCI image format](https://github.com/opencontainers/image-spec/blob/main/spec.md) , which uses gzipped tarballs (` tar.gz` ) for its layers. This format is a compressed stream, making random access to individual files impossible without decompressing the entire stream up to the desired file.


Tensorfuse addresses this with a build tool that converts standard OCI images into a highly optimized and seekable format based on the Registry Accelerated File System design, while remaining compatible with OCI registries. This conversion process fundamentally restructures the image by separating filesystem metadata from file data. The metadata is stored in a compact “bootstrap” file, which acts as a comprehensive **Table of Contents (TOC)** .


The file data itself is broken down into content-addressable chunks, or “blobs”. This architecture makes the entire filesystem instantly seekable, enabling the runtime to fetch only the required data chunks for a specific file. This bypasses the need to download or decompress the entire multi-gigabyte layer just to start the container.


### ​


2. Runtime: FUSE and Lazy-Loading


The Tensorfuse snapshotter runs as a daemon on each Kubernetes node. When` containerd` is instructed to create a container, the following occurs:


- Instead of pulling layers, the Tensorfuse daemon instantly mounts a[FUSE (Filesystem in Userspace)](https://en.wikipedia.org/wiki/Filesystem_in_Userspace) filesystem. To the container, this virtual filesystem appears as if the entire image is present on local disk.


- When a process inside the container attempts to read a file (e.g., Python’s` import torch` ), the Linux kernel intercepts the` read()` syscall and forwards it to the Tensorfuse` daemon` .


- The` daemon` consults the pre-generated Table Of Contents (the RAFS bootstrap) to locate the file’s data within the compressed layer in the remote registry.


- It performs an` HTTP` Range Request to the registry, fetching only the small chunk of compressed data containing the file and its preceding decompression checkpoint.


- The daemon uses the checkpoint to initialize the decompressor and unpacks the small data segment in memory.


- The file’s contents are returned to the kernel, which satisfies the application’s` read()` call.


This entire process is transparent to the containerized application. The registry is effectively treated as a **high-performance, random-access network block device.**


### ​


Integration with containerd


Tensorfuse integrates non-intrusively using` containerd's` stable remote snapshotter` gRPC` API. The key interaction occurs during the image pull process.


- For each image layer,` containerd` calls the` Prepare` method on the Tensorfuse gRPC service.
- The Tensorfuse daemon, which only needs to mount the FUSE filesystem, immediately returns an` ErrAlreadyExists` error.
- This specific error code signals to` containerd` that the snapshotter can provide the layer’s contents without needing` containerd` to download and unpack it.` containerd` trusts this signal and skips the download for that layer.


This design requires no modification to` containerd's` core code, preserving the stability and security of the standard container runtime.


## ​


Performance and Impact on vLLM


The architectural changes result in a dramatic reduction in startup time. The multi-minute download and decompression phases are eliminated entirely.


Stage Standard overlayfs Tensorfuse Snapshotter Improvement


Image Data & Unpack ~12 minutes Eliminated (On-Demand) -


Time to ENTRYPOINT ~12 min, 5 sec ~2 seconds > 360x


vLLM Server Ready ~12 min, 30 sec ~20 seconds > 37x
