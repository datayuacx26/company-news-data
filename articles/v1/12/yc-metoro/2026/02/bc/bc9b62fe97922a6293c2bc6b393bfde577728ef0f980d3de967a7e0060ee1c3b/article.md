---
schema_version: "1.0.0"
document_id: "bc9b62fe97922a6293c2bc6b393bfde577728ef0f980d3de967a7e0060ee1c3b"
company_key: "yc-metoro"
company: "Metoro"
source_id: "yc-metoro-news-import-a8c8033caa18"
canonical_url: "https://metoro.io/blog/openai-codex-kubernetes"
published_at: "2026-02-09T00:00:00+00:00"
first_seen_at: "2026-07-22T04:11:54.672278+00:00"
fetched_at: "2026-07-28T22:21:23.355424+00:00"
content_hash: "sha256:1d901151fcb11a75861478cbaba5add05336c08d61b3710a8b077cdbb5e44fb2"
---

# Running OpenAI's Codex on Kubernetes

I've been using[OpenAI's Codex](https://openai.com/blog/openai-codex) for some time now, even more so since the GPT-5.3-codex release. Like most people, I've been running it locally on my main coding machine and occasionally running some tasks through the web UI when I'm away from my laptop.


The web UI is super convenient when on the go but it has a bunch of limitations that I kept running into:


- Each task is one prompt then codex will slog it out till completion, no where near as steerable as running it locally.
- The container environment that openai gives you is not persistent, builds aren't cached so the whole thing can take upwards of 30 minutes to run / build everything e2e.


On the other side, running things locally is super convenient but I'm asking codex to do more and more complex actions and relying on third party web calls (looking at logs to allow it to debug). I want to be able to monitor all the calls that codex is making - this is super hard to do when codex is often just running things like` curl` commands.


Running codex on my local home-lab kubernetes cluster seemed like a good fit for solving these problems, I can ssh in from my phone to give it tasks when I'm on the go and I can monitor exactly what it's doing.


If you want the same operational pattern with Anthropic's agent, the[Claude Code on Kubernetes guide](https://metoro.io/blog/claude-code-kubernetes) walks through a parallel setup.


I couldn't find any helm charts on the web for Codex so I decided to build my own at[chrisbattarbee/openai-codex-helm](https://github.com/chrisbattarbee/openai-codex-helm) .


## Installation


Before getting started, we're going to need to enable device code authentication for Codex.


You can head to[ChatGpt Security Settings](https://chatgpt.com/#settings/Security) and enable device code authentication there.


Enable device code authorization in ChatGPT Security settings


We'll use this feature later to authenticate codex running in our Kubernetes cluster.


### Helm Chart Installation


The following will deploy Codex to your Kubernetes cluster:


```text
helm repo  add   openai-codex https://chrisbattarbee.github.io/openai-codex-helm
helm repo update
helm  install   codex openai-codex/openai-codex


```


Now you can access codex via the following command:


```text
kubectl  exec    -it   deploy/codex-openai-codex  -c   codex -- codex


```


You'll be greeted with the Codex CLI prompt. Select the` Device Code` option and follow the instructions to authenticate.


Select the Device Code option to authenticate


Now you have a persistent codex instance running on your Kubernetes cluster.


Codex running in a Kubernetes pod


## Monitoring


When monitoring Codex (or any other agent for that matter), my main goal is to understand what APIs and endpoints it's calling. Codex has pretty broad permissions to call external APIs (and it needs to so that it can do its job) I want to be able to catch rogue actions where they arise and make sure it's not doing anything malicious.


One problem we have is that Codex itself doesn't have native telemetry capabilities for us to hook into and even if it did, it's able to call other programs and APIs that may not be instrumented. For example, it can run` curl` commands to fetch data from my tracking system in Notion.


I'll be using[Metoro](https://metoro.io/features/ai-agent-monitoring) to monitor Codex as the instrumentation is done at the kernel level with eBPF. This means that we can see exactly what Codex is doing without needing to do any instrumentation of Codex or the programs it calls.


Now we can see the network calls in Metoro even though Codex is calling them via running a` curl` command.


Service map showing Codex's external API calls


And we can drill down into individual requests to see exactly what's happening.


Trace detail with full request attributes


I set up an alert in Metoro to notify me if Codex ever makes a request to an unexpected API.


Setting up an alert for unexpected API calls


## Conclusion


All in all, I'm pretty happy with this setup, it's easy for me to give codex tasks, intervene when I need to on the go and monitor exactly what it's doing.
