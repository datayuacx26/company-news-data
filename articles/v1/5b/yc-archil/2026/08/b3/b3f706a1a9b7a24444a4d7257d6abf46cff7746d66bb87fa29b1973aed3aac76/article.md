---
schema_version: "1.0.0"
document_id: "b3f706a1a9b7a24444a4d7257d6abf46cff7746d66bb87fa29b1973aed3aac76"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-fbbb60f1501e"
canonical_url: "https://archil.com/post/archil-file-system-to-data-company"
published_at: null
first_seen_at: "2026-08-09T19:14:48.524951+00:00"
fetched_at: "2026-08-09T19:14:50.194785+00:00"
content_hash: "sha256:71ca2b8404ee374e10ddb926d837f09f9b7cd826fc75f6efbe20cf419aa2fb90"
---

# Archil: from a file system to a data company

Archil (formerly Regatta Storage) has raised $6.7M to replace legacy cloud storage like EBS with infinite, shareable volumes that instantly connect to S3. The round was led by Felicis and Y Combinator, with participation from other investors.


## The Core Problem


Cloud providers still require developers to specify storage capacity before launching instances—a paradigm unchanged since EC2's inception nearly two decades ago. This approach creates friction: guessing capacity needs, learning unfamiliar concepts like IOPS, paying for unused resources, and managing manual data transfers.


Container orchestration systems like Kubernetes compound this challenge. "How do you deploy a 200 GiB model to a Kubernetes pod?" The tension between startup speed and data availability is real.


## The Archil Solution


Rather than forcing users through complex configuration, Archil asks only three questions: volume name, cloud region, and data source location. The command to mount is straightforward:


```text
sudo   archil   mount   $VOLUME_NAME
```


This delivers a POSIX-compatible, multi-instance mountable filesystem with instant S3 bucket integration. We employ custom data protocols and NVME SSDs for performance—90% lower costs than EBS and 30x lower latencies than S3 directly.


## Beyond Storage: A Data Company


Archil is fundamentally a data company, not merely storage infrastructure. Future capabilities include:


- **Connectivity:** Direct links to diverse data sources (Hugging Face, GitHub, data warehouses, SFTP servers)
- **Compute:** Serverless, low-level data transformations at the storage layer
- **Intelligence:** Built-in versioning, locality awareness, and access controls


## AI and Agentic Applications


Recent developments show autonomous agents prefer cloud-agnostic, scalable-to-zero infrastructure. While databases dominated this space previously, agents increasingly require POSIX filesystems. Archil addresses this gap.


We're excited to build the future of cloud storage. If you're interested in trying Archil,get in touch .
