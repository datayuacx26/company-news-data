---
schema_version: "1.0.0"
document_id: "d0d073daa0007daaed866dec80cd1a24a839efbda41b8e7d369fc053c5a6a422"
company_key: "yc-metoro"
company: "Metoro"
source_id: "yc-metoro-news-import-a8c8033caa18"
canonical_url: "https://metoro.io/blog/claude-code-kubernetes"
published_at: "2026-02-10T00:00:00+00:00"
first_seen_at: "2026-07-22T04:11:54.672278+00:00"
fetched_at: "2026-07-28T22:21:12.159869+00:00"
content_hash: "sha256:88a53eeff9cc22a2bd227dc320e4a9b71f1d8fd7861d2cd5a5cf3d730eed7a51"
---

# Running Claude Code on Kubernetes

[Claude Code](https://docs.anthropic.com/en/docs/claude-code) has become a core part of how I write software. Like many developers, I've been running it on my local machine, but recently I've been wanting to level up my setup.


Running locally is convenient, but I kept hitting a few walls:


- Accessing it remotely is a pain - I'd love to spin up tasks from my phone when I'm away from my desk.
- Tracking what external calls Claude Code makes is tricky - especially when it shells out to tools like` curl` - this is a security concern as I offload more and more tasks to it.


Deploying Claude Code to my home-lab Kubernetes cluster felt like the right solution. I can shell into the pod from anywhere to assign tasks and keep tabs on everything it does.


If you're comparing agent runtimes, the[Codex on Kubernetes guide](https://metoro.io/blog/openai-codex-kubernetes) covers the same pattern with OpenAI's tooling.


There weren't any existing helm charts out there, so I put one together at[chrisbattarbee/claude-code-helm](https://github.com/chrisbattarbee/claude-code-helm) .


## Installation


To get Claude Code running on your cluster:


```text
helm repo  add   claude-code https://chrisbattarbee.github.io/claude-code-helm
helm repo update
helm  install   claude-code claude-code/claude-code


```


Then connect to it with:


```text
kubectl  exec    -it   deploy/claude-code  -c   claude-code -- claude


```


## Authentication


You'll see the login screen with several authentication options:


Choose your preferred authentication method


- **Claude subscription** - Sign in with your Claude Pro or Teams account
- **Anthropic API key** - Authenticate using a key from the Anthropic Console
- **Third-party API** - Use a compatible third-party provider


After logging in, you've got a persistent Claude Code instance running in your cluster.


Claude Code running in a Kubernetes pod


## Monitoring


My primary concern when running Claude Code (or any AI agent) is visibility into what APIs and services it's reaching out to. Claude Code needs broad permissions to call external APIs to be useful, but I want to spot any unexpected behavior and ensure nothing sketchy is going on.


The challenge is that Claude Code doesn't expose telemetry natively. And even if it did, it spawns subprocesses and runs tools that wouldn't be instrumented anyway. For instance, it might execute` curl` to pull data from my task tracker in Notion.


I'm using[Metoro](https://metoro.io/features/ai-agent-monitoring) for monitoring since it instruments at the kernel level via eBPF. This lets me observe exactly what Claude Code is doing without adding any instrumentation to Claude Code itself or the programs it invokes.


With this setup, I can see network requests in Metoro even when Claude Code makes them through a` curl` subprocess.


Service map showing Claude Code's external API calls


I can also drill into specific requests to inspect the details.


Trace detail with full request attributes


## Conclusion


Overall, I'm really pleased with how this turned out. I can hand off tasks to Claude Code from anywhere, step in when needed, and have full visibility into its behavior through Metoro's eBPF-powered observability.
