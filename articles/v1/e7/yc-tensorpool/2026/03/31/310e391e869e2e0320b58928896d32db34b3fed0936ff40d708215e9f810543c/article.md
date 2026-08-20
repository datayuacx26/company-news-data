---
schema_version: "1.0.0"
document_id: "310e391e869e2e0320b58928896d32db34b3fed0936ff40d708215e9f810543c"
company_key: "yc-tensorpool"
company: "TensorPool"
source_id: "yc-tensorpool-news-import-ce508c9f2d8e"
canonical_url: "https://tensorpool.dev/blog/object-storage"
published_at: "2026-03-31T00:00:00+00:00"
first_seen_at: "2026-07-24T13:32:36.763199+00:00"
fetched_at: "2026-07-28T21:56:50.434513+00:00"
content_hash: "sha256:3ff37fcbd9228dc59b2432515b29e3493f0c8964190c1b8968522052b4d9775d"
---

# Introducing TensorPool Object Storage

Training across regions shouldn't mean waiting on data. Today we're launching TensorPool Object Storage: S3-compatible, globally distributed storage that caches your data in the same region as your GPUs.


## The problem


If you've ever had your data in one region and your GPUs in another, you already know. Your dataset lives in us-east-1, but the only available B200s are in the EU. Your training loop stalls on cross-Atlantic latency, and your $6/hr GPUs are twiddling their thumbs waiting for data.


The workarounds are all bad -- manually syncing data across regions, maintaining duplicate buckets, pre-fetching data. You end up managing infrastructure instead of training models.


## How it works


TensorPool Object Storage, built with in collaboration with[Accelerated Cloud Storage](https://www.acceleratedcloudstorage.com/) , **puts your data where your GPUs are** . Behind the scenes, our system eagerly distributes your data across all TensorPool cluster regions with eventual consistency.


This means that:


- Guaranteed base case latency, since data always comes from the same region as your cluster with strong consistency within a region.
- 99.99% of objects are globally available within 15 minutes. Small files propagate in milliseconds.
- All with no ingress/egress fees and unlimited storage capacity!


Wherever you launch a cluster, your data is already nearby.


## Benchmarks: TensorPool Object Storage vs. Cloudflare R2


We benchmarked against Cloudflare R2 from a TensorPool H100 cluster in us-east, against an R2 bucket in the same region (Eastern North America, ENAM) both and cross-region (Asia-Pacific, APAC)


### Small objects (1KB-100KB)


Metric TensorPool Object Storage R2 (Eastern North America, ENAM) R2 (Asia-Pacific, APAC)


Upload latency (P50) 44-64ms 126-450ms 850-1,012ms


Download latency (P50) 52-67ms 58-124ms 552-586ms


TensorPool uploads are 2.9-7x faster than R2 in the same region. Downloads are 1.1-2.2x faster. In cross-region scenarios, the gap widens to 19x faster uploads and 11x faster downloads.


When you're loading thousands of small files per epoch, this latency adds up fast. We've seen the biggest wins from teams working with biological, geospatial, and graph datasets where that's the norm.


### Large files (5-10GB)


Upload throughput 433-451 MB/s 129-200 MB/s 106-117 MB/s


Download throughput 801-873 MB/s 323-440 MB/s 283-306 MB/s


Uploads run at 2.2-3.5x the throughput of same-region R2. Downloads are 2.0-2.5x faster.


A 10GB model checkpoint download that takes ~23 seconds from R2 in the same region finishes in ~12 seconds on TensorPool Object Storage.


### Consistency


Throughput is one thing. Consistency is what actually matters for distributed training -- unpredictable I/O means unpredictable training times and wasted GPU cycles.


Upload variance (σ) 1.1-1.2s 9.1-18.0s 4.3-4.5s


Download variance (σ) 0.006-0.03s 1.3-3.6s 0.4-0.9s


TensorPool uploads are 8-15x more consistent than same-region R2. Downloads are 43-600x more consistent. Predictable I/O means predictable training times and GPUs that aren't sitting around waiting on storage.


## Getting started


All TensorPool Organizations can get started today!


```text
# Enable object storage for your organization
tp object-storage enable


# Create a bucket
tp object-storage bucket create dataset


# Get your S3-compatible credentials
tp object-storage credentials


```


Then use any S3-compatible tools to upload data:


```text
import boto3


s3 = boto3.client(
"s3",
endpoint_url="<your-endpoint>",
aws_access_key_id="<your-access-key>",
aws_secret_access_key="<your-secret-key>",
region_name="global"
)


# Upload your dataset
s3.upload_file("dataset.tar", "dataset", "dataset.tar")


```


For the full usage guide and check out our[Object Storage documentation](https://docs.tensorpool.dev/features/object-storage) .


When you spin up a TensorPool Cluster in any region, your data is already cached nearby. No extra steps.


Stop choosing GPUs based on where your data lives.
