---
schema_version: "1.0.0"
document_id: "8a0af50c79ab2df47abeffdc600a0db391e3d6bee85c9d67a4cd6a8fc4fa3e0a"
company_key: "yc-glasskube"
company: "Glasskube"
source_id: "yc-glasskube-news-import-f40b83a58804"
canonical_url: "https://distr.sh/blog/distr-logs-metrics/"
published_at: "2026-03-09T00:00:00+00:00"
first_seen_at: "2026-07-21T21:44:16.826534+00:00"
fetched_at: "2026-07-28T21:58:49.530734+00:00"
content_hash: "sha256:3ee73e15ebc128228820336de32a395e6b3473b6842b7c2f541b460096f2a586"
---

# Container Logs and Metrics From Every Customer Deployment

## When your customer is your only monitoring


“Can you run` docker logs` on the container and send me the output?”


That sentence has probably cost your support team more hours than any real bug. The customer doesn’t know which container. They paste the wrong thing. They truncate it. They redact something that would’ve told you exactly what happened.


You built the software. You know every service, every config option, every failure mode. But the moment it’s deployed in a customer environment, you’re getting all your information secondhand, filtered through someone who doesn’t know what you need to see.


Meanwhile, for your own infrastructure, you have dashboards. Log aggregation. Resource metrics. You’d never run production blind. But for every customer deployment running on their hardware? Nothing. No` docker logs` , no memory graphs, no idea if the thing is struggling or fine. Just a support ticket when something eventually breaks.


## Container logs and system metrics, directly in your vendor portal


We’ve added container log collection and system resource metrics to Distr. Every connected customer deployment now ships both to your vendor portal automatically, with no extra setup, no additional agents, and no new permissions to ask for.


From the vendor portal you can:


- View container logs per deployment, filtered by service/resource name and time range
- See CPU and memory utilization for each deployment target
- Export logs as plain text for deeper analysis or support tickets


Two log streams are available. Deployment logs are the actual stdout/stderr from your running containers, the same data you’d get from` docker logs` or` kubectl logs` if you had direct access. Agent logs capture what the Distr agent itself is doing, which is useful when you’re debugging connectivity or update issues at the infrastructure level.


Logs are batched every 30 seconds. Secrets you’ve configured in Distr are automatically redacted before anything is stored or displayed, so deployment credentials won’t surface in a log line.


## How Distr collects Docker and Kubernetes logs without direct access


The agent is already running inside the customer environment managing deployments. Logs and metrics collect on top of that, with no new footprint, no firewall rules, and nothing extra for your customer to configure.


For Docker and Docker Swarm, the agent pulls per-service logs via the Docker API and collects host CPU and memory using OpenTelemetry’s host metrics receiver. For Kubernetes, it reads pod logs through the Kubernetes API and aggregates node-level resource data across the cluster via the metrics-server.


Data flows from the agent directly to your vendor portal. You don’t need network access to their environment. Your customer doesn’t need to expose any ports or install anything additional.


## What it looks like to debug a deployment you can’t SSH into


Memory at 96%. OOMKilled events on the main container, repeating every 3 minutes. First occurrence 40 minutes ago.


That’s what you see when you open a struggling deployment in the vendor portal now.


Before this, getting to that same information meant: email the customer, wait for a response, get on a call, walk them through running the right commands, hope they don’t paste the wrong output. That process takes a day on a good day.


We’ve seen it compress to a two-minute conversation. Customer reports an issue, you open Distr, you already know what’s wrong before you reply.


## Getting started with logs and metrics in Distr


If you’re already running the Distr agent in customer environments, this is live. Open any deployment in your vendor portal, and the logs tab and resource metrics are there now.


The details (resource labeling, time-range filtering, log export format, Kubernetes metrics-server requirements) are all in the[agents documentation](https://distr.sh/docs/agents/) .


If you’re not using Distr yet and you ship software to[on-prem](https://distr.sh/glossary/on-premises-definition/) or[self-hosted](https://distr.sh/glossary/self-hosted-software/) environments, this is a good place to start.[Try it here](https://distr.sh/onboarding) .


## Join the Conversation


We’d love to hear how you’re using this, and what would make it more useful.


- [Join the discussion](https://github.com/distr-sh/distr/discussions)
- [Book a demo call](https://cal.glasskube.com/team/gk/demo?duration=30)
