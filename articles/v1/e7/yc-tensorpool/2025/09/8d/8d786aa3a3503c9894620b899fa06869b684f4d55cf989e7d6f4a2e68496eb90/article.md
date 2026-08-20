---
schema_version: "1.0.0"
document_id: "8d786aa3a3503c9894620b899fa06869b684f4d55cf989e7d6f4a2e68496eb90"
company_key: "yc-tensorpool"
company: "TensorPool"
source_id: "yc-tensorpool-news-import-ce508c9f2d8e"
canonical_url: "https://tensorpool.dev/blog/tensorpool-cli-guide"
published_at: "2025-09-28T00:00:00+00:00"
first_seen_at: "2026-07-24T13:32:36.763199+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:e54d47b551e0c3615cb9ff0c355d97dc38ed7a0585515c476a44293ba0aae2cd"
---

# Getting Started with the TensorPool CLI

The TensorPool CLI is your command-line interface to powerful GPU infrastructure. Whether you're spinning up a single H100 for model training or orchestrating a multi-node cluster for distributed workloads, the CLI makes it fast and simple.


## Video Tutorial


Watch our comprehensive video guide to get started with the TensorPool CLI:


## Prerequisites


Before getting started, make sure you have the following:


- A TensorPool account (sign up at[tensorpool.dev](https://tensorpool.dev/) )
- Python 3.8 or later installed
- SSH keys generated (we'll show you how)


---


## Step-by-Step Guide


### Step 1: Install the TensorPool CLI


Install the CLI using pip:


```text
pip install tensorpool


```


Verify the installation:


```text
tp --version


```


### Step 2: Set Up Your API Key


Get your API key from the TensorPool Dashboard and set it as an environment variable:


```text
export TENSORPOOL_KEY="your_api_key_here"


```


**TIP:** Add this to your` ~/.bashrc` or` ~/.zshrc` to make it persistent.


### Step 3: Generate SSH Keys


If you don't have SSH keys yet, generate them:


```text
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519


```


Press Enter to accept the defaults when prompted.


### Step 4: Create Your First Cluster


Let's create a single-node cluster with one H100 GPU:


```text
tp cluster create \
-i ~/.ssh/id_rsa.pub \
-t 1xH100 \
--name my-first-cluster


```


For a multi-node cluster with 8 B200s per node:


```text
tp cluster create \
-i ~/.ssh/id_rsa.pub \
-t 8xB200 \
-n 4 \
--name distributed-training


```


Available instance types:` 1xH100` ,` 2xH100` ,` 4xH100` ,` 8xH100` ,` 1xH200` ,` 8xB200` , and more.


### Step 5: List and Connect to Your Cluster


View all your clusters:


```text
tp cluster list


```


This shows cluster ID, instance type, SSH username, IP addresses, ports, and hourly pricing. SSH into your cluster using the provided details:


```text
ssh tensorpool@192.168.1.42


```


### Step 6: Create and Attach NFS Storage


Create a 500GB NFS volume for persistent storage:


```text
tp nfs create \
--s 500 \
--name nfs-test


```


List all NFS volumes:


```text
tp nfs list


```


Attach the NFS volume to your cluster:


> ⚠️ **Note:** NFS volumes can only be attached to multi-node clusters. Single-node clusters do not support NFS attachment.


```text
tp nfs attach <storage_id> <cluster_ids>


```


### Step 7: Tear Down Resources


When you're done, clean up resources to stop billing:


```text
# Detach NFS from cluster
tp nfs detach <storage_id> <cluster_ids>


# Destroy NFS volume
tp nfs destroy <storage_id>


# Destroy cluster
tp cluster destroy <cluster_id>


```


**WARNING:** Destroying a cluster or NFS volume is permanent. Make sure to back up any data before running destroy commands.


---


## Common Use Cases


### 💻 Single-GPU Development


Quick prototyping and model experimentation:


```text
tp cluster create -i ~/.ssh/id_rsa.pub -t 1xH100 --name dev


```


### 🚀 Multinode Training


Large-scale model training across multiple nodes:


```text
tp cluster create \
-i ~/.ssh/id_rsa.pub \
-t 8xB200 \
-n 2 \
--name llm-training


```


### 📊 Data Processing Pipeline


Attach shared NFS storage for data pipelines:


```text
# Create cluster
tp cluster create -i ~/.ssh/id_rsa.pub -t 4xH100 --name pipeline


# Create and attach 1TB NFS
tp nfs create --size 1000 --name data-store
tp nfs attach --cluster-id <id> --nfs-id <nfs_id>


```


## Next Steps


- Check out the[full CLI documentation](https://github.com/tensorpool/tensorpool) on GitHub
- Join the TensorPool Slack to ask questions and share your projects
- Read our blog post on optimizing multi-node GPU clusters


---


Ready to get started? Sign up for TensorPool and get your first cluster running in minutes.
