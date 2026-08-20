---
schema_version: "1.0.0"
document_id: "fe57653f9bc6a942978c71d636be1ff187f0cf30445c2faefc12a40191972ecb"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/testing-microservices-with-rabbitmq-using-signadot-sandboxes/"
published_at: "2025-08-29T21:03:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:d71b1acf261588e2f9086e848c803e2b4b7626b577122167b565308a289a9180"
---

# Testing Microservices with RabbitMQ using Signadot Sandboxes

## Introduction


Asynchronous microservices are hard to test: if two versions of a consumer read from the same queue, they compete for messages. Spinning up a separate broker per branch is slow and pricey.


**Signadot Sandboxes** solve this with **request-level isolation** . Keep a single RabbitMQ, but route messages only to the intended version (sandbox) of your consumer using a **sandbox routing key** . Each sandboxed consumer has its own queue binding; baseline traffic remains untouched while you test safely in parallel.


### What you will accomplish:


- Set up a RabbitMQ-based microservices application
- Use routing keys + selective consumption to isolate sandbox traffic
- Deploy services to Kubernetes
- Create Signadot sandboxes for isolated testing
- Test message routing between baseline and sandbox environments


**Time required:** 45-60 minutes


## Prerequisites


Before starting, ensure you have:


1. **Signadot CLI installed** - Follow the[installation guide](https://www.signadot.com/docs/getting-started/installation)
2. **Docker Desktop** running locally
3. **kubectl** configured to access your Kubernetes cluster
4. **Python 3.8+** and pip
5. **RabbitMQ knowledge** - Basic understanding of exchanges, queues, and routing


## Architecture Overview


- A topic exchange fans out each message to both baseline and sandbox queues.
- OTel propagates the sandbox routing key automatically as W3C baggage.
- Selective consumption in consumers:


- Baseline consumer processes messages with no routing key, and skips messages whose key belongs to an active sandbox of this same service. It still processes messages carrying keys for other services’ sandboxes or inactive/unknown keys.
- Sandbox consumer processes only messages with its own sandbox key.


This gives isolation without multiple brokers: everyone receives the message, but only the right consumer acts on it.


### The idea in one picture


Baseline and sandbox consumers each have their own queue bound to the same exchange. Routing keys in headers determine who should act.


## Project structure


You can scaffold this from your own repo or adapt from[Signadot examples](https://github.com/signadot/examples) .


## Step 1 – Get the project


Clone the example repository from GitHub:


Inside the repo you’ll find:


- **publisher/** — Flask app with` /publish` and` /events` . Publishes to a topic exchange and logs events to Redis.
- **consumer/** — Python consumer that declares its own queue, binds to the exchange, and processes messages.
- **k8s/** — manifests for RabbitMQ, Redis, publisher, consumer, and a namespace.
- **signadot/** — sandbox specs for publisher/consumer and a **RouteGroup** to bind them under a single routing key.


## Step 2 — Build and push images


If you’ll test a dev version in a sandbox, also push a` :dev` tag.


## Step 3 — Deploy the baseline stack


Create namespace and infra:


Deploy services:


Verify pods:


## Step 4 — Connect to Signadot Locally


Use Signadot Local Connect to reach cluster services directly from your local machine:


Send a baseline message (no sandbox key):


Check:


You should see baseline processing.


## Step 5 — Create sandboxes and a RouteGroup


Spin up forked workloads for **publisher-v2** and **consumer-v2** :


Create a **RouteGroup** that ties them together under one routing key / URL (optional):


## Step 6 — Prove isolation


Baseline message (no header → baseline handles it):


Sandbox message (explicit header → sandbox handles it):


Watch logs side-by-side:


- Baseline:


- Sandbox (replace name with your sandbox consumer deployment)


Expected outcome:


- **Baseline** processes **only** messages without a sandbox key or routing key of sandboxes of other services
- **Sandbox** processes **only** messages with its **own** key


## What’s Happening Under the Hood


- Context propagation: When you hit a sandbox URL or pass` sd-routing-key` , the publisher attaches that key to message headers.
- Fan-out delivery: RabbitMQ’s topic exchange routes messages to both baseline and sandbox queues (via their bindings).
- Selective consumption:


- Baseline consumer accepts messages without a sandbox key; it skips messages with a key that belongs to an active sandbox.
- Sandbox consumer processes only messages whose` sd-routing-key` equals its sandbox ID.


- No message loss: Each sandbox has its own queue bound to the exchange. Messages always have a target consumer; baseline only “avoids” messages while a matching sandbox is active.


## Conclusion


You’ve built and deployed a minimal RabbitMQ publisher/consumer stack, forked services into Signadot sandboxes, and confirmed that sandbox routing keys isolate messages in a shared RabbitMQ. This approach scales beyond this demo:


- Works with Kafka, Pub/Sub, or SQS (using analogous header/attribute + selective consumption patterns)
- Supports multiple sandboxes concurrently (e.g., per PR)
- Integrates naturally with CI/CD to spin up ephemeral test envs for each change


For deeper dives, see:


- [Signadot docs](https://www.signadot.com/docs/overview)
- [Example source](https://github.com/signadot/examples/tree/main)


Happy testing!
