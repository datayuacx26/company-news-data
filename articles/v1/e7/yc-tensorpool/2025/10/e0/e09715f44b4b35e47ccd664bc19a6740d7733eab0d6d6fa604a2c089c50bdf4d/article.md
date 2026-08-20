---
schema_version: "1.0.0"
document_id: "e09715f44b4b35e47ccd664bc19a6740d7733eab0d6d6fa604a2c089c50bdf4d"
company_key: "yc-tensorpool"
company: "TensorPool"
source_id: "yc-tensorpool-news-import-ce508c9f2d8e"
canonical_url: "https://tensorpool.dev/blog/tensorpool-jobs"
published_at: "2025-10-31T00:00:00+00:00"
first_seen_at: "2026-07-24T13:32:36.763199+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:d87a2aec1f3be40f024adbd6e3e93e91405644f1c3eb2a9d0787afab79d9e771"
---

# Introducing TensorPool Jobs: Git-Style GPU Workflows

Managing GPU training jobs shouldn't feel like juggling SSH sessions and manually moving files around. That's why we built TensorPool Jobs: a git-style interface that lets you push ML workloads to GPUs as easily as you push code to GitHub.


## The Problem with Traditional GPU Workflows


The typical GPU training workflow is tedious:


1. Manually SSH into a GPU instance
2. Set up your environment
3. Start training and keep the terminal open
4. Babysit the process or risk losing output
5. Manually download results when done
6. Remember to shut down the instance (or pay for idle time)


If you're running multiple experiments, multiply this complexity by the number of jobs. It's time-consuming, error-prone, and expensive.


## Jobs: A Better Way to Train


TensorPool Jobs flips the script. Instead of managing long-running SSH sessions, you define your job once, push it, and let TensorPool handle the rest.


Here's what makes it powerful:


### Pay Only for Compute Time


Jobs spin up a cluster, run your training, save your outputs, and automatically shut down. You're billed only for the time your code is actually running—not for idle instances you forgot about.


### Configuration as Code


Define your job in a` tp.config.toml` file:


```text
[job]
name = "llama-finetune"
instance_type = "8xH100"
commands = [
"pip install -r requirements.txt",
"python train.py --epochs 100 --batch-size 32"
]


[output]
files = [
"checkpoints/",
"logs/",
"results.json"
]


```


This configuration is version-controlled, shareable, and reproducible.


### Submit Jobs in Seconds


Once your config is ready, push your job:


```text
tp job push tp.config.toml


```


TensorPool handles:


- Spinning up the specified GPU cluster
- Uploading your code
- Running your commands sequentially
- Capturing all logs
- Saving specified output files
- Shutting down the cluster


### Monitor and Retrieve Results


Once your job is running, you can:


- **Stream logs in real-time** :` tp job listen <job_id>`
- **Download outputs when complete** :` tp job pull <job_id>`
- **List all your jobs** :` tp job list`
- **Cancel a running job** :` tp job cancel <job_id>`


## Real-World Workflows


### Single Experiment


Quick model training with automatic cleanup:


```text
# Initialize config
tp job init


# Edit tp.config.toml with your settings


# Push and forget
tp job push tp.config.toml


```


Your job runs, saves checkpoints, and shuts down automatically.


### Hyperparameter Sweeps


Create multiple configs for different hyperparameters:


```text
tp.config.lr-0.001.toml
tp.config.lr-0.0001.toml
tp.config.lr-0.00001.toml


```


Push them all at once:


```text
tp job push tp.config.lr-0.001.toml
tp job push tp.config.lr-0.0001.toml
tp job push tp.config.lr-0.00001.toml


```


Jobs queue and run automatically. Retrieve the best results when they're done.


## Getting Started


The workflow is straightforward: initialize a config, customize it for your training job, push it to TensorPool, and retrieve your results when done.


### 1. Initialize Your Config


In your project directory:


```text
tp job init


```


This creates a` tp.config.toml` template with sensible defaults.


### 2. Customize Your Job


Edit the config with your training command, GPU requirements, and output files:


```text
[job]
name = "my-training-job"
instance_type = "1xH100"
commands = [
"python train.py"
]


[output]
files = [
"model.pth",
"metrics.json"
]


```


### 3. Push and Monitor


Push your job and get a job ID for tracking:


```text
tp job push tp.config.toml


```


That's it. Your cluster spins up, runs your training, saves outputs, and shuts down automatically. You can monitor progress with the commands described above, and pull your results when complete.


## Under the Hood


Jobs leverage TensorPool's core infrastructure:


- **Fast provisioning** : Clusters spin up in minutes
- **Automatic cleanup** : Resources deallocate when jobs finish
- **Persistent storage** : Optional NFS volumes for large datasets


The result? A seamless experience that feels like running code locally, but with on-demand access to H100s, H200s, and B200s.


## What's Next


Top startups and labs already use TensorPool Jobs to ship faster and train better models. Jobs will continue to get better with your feedback in[our Slack](https://tensorpool.dev/community) :


- **Scheduled jobs** : Run training at specific times
- **Job templates** : Share configs across teams
- **Enhanced monitoring** : Real-time GPU utilization graphs
- **Error Handling** : Agentized job error fixing
- **Multi-node Jobs** : Don't need to explain this one :)


## Try It Today


If you've been putting off GPU-intensive experiments because managing infrastructure is tedious, Jobs removes that friction.


Install the TensorPool CLI:


```text
pip install tensorpool


```


Initialize your first job:


```text
tp job init


```


Push it and see how simple GPU training can be:


```text
tp job push tp.config.toml


```


Ready to train smarter, not harder? Check out the full[Jobs documentation](https://docs.tensorpool.dev/features/jobs) .


---


Questions? Feedback? We'd love to hear from you. Join our community or reach out on[our website](https://tensorpool.dev/community) .
