---
schema_version: "1.0.0"
document_id: "d4ddde716b76f513b7303dabb76ae36d62656a69b50618c7e7c29c4c7b1225a4"
company_key: "yc-metoro"
company: "Metoro"
source_id: "yc-metoro-news-import-a8c8033caa18"
canonical_url: "https://metoro.io/blog/openclaw-kubernetes"
published_at: "2026-02-03T00:00:00+00:00"
first_seen_at: "2026-07-22T04:11:54.672278+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:fa5e8523d94f1a5ea4d9af158a968d45ca694ca94c59579bd6793b9f1718b9b0"
---

# Running OpenClaw on Kubernetes

Like the rest of the world, I've been experimenting with[OpenClaw](https://github.com/openclaw/openclaw) over the weekend. Initially I had a fresh linux install on an old laptop with OpenClaw installed but as my workflows and integrations grew, things started to become a little unwieldy.


I wanted to manage OpenClaw declaratively, take snapshots of state periodically and monitor exactly what it was doing. My home lab Kubernetes cluster seemed like a good fit for this.


If you're comparing agent deployments, the[Codex on Kubernetes guide](https://metoro.io/blog/openai-codex-kubernetes) shows the same operational pattern with a different agent stack.


I couldn't see any other helm charts so I decided to build my own at[chrisbattarbee/openclaw-helm](https://github.com/chrisbattarbee/openclaw-helm) .


## Installation


The Helm chart makes deployment straightforward. Three commands get you running:


```text
helm repo  add   openclaw https://chrisbattarbee.github.io/openclaw-helm
helm repo update
helm  install   openclaw openclaw/openclaw  --set    credentials.anthropicApiKey  =  sk-ant-xxx


```


You can set various configurations declaratively by overriding the values in[values.yaml](https://github.com/Chrisbattarbee/openclaw-helm/blob/main/charts/openclaw/values.yaml#L22) .


Once helm has deployed OpenClaw, it will give you a command to shell into the pod and run the onboarding workflow which will look something like this:


```text
kubectl  exec    -it    --namespace   default deploy/openclaw  -c   openclaw --  node   dist/index.js onboard


```


After you run through onboarding, you're off to the races.


My personal workflow is to add integrations, skills etc as I normally would, then inspect the` openclaw.json` file and extract out the config and set it declaratively in the helm chart. This way I can recreate the exact same environment in the future without losing the flexibility of installing integrations and skills on the fly as I go.


## Monitoring


My main requirement with monitoring OpenClaw is to understand what APIs and endpoints it's calling and how often. OpenClaw is essentially allowed to do pretty much anything it wants so seeing exactly what it did is important to me to ensure I can catch rogue actions.


One problem we have is that OpenClaw itself doesn't have native telemetry capabilities right now and even if it did, it's able to call other programs and APIs that may not be instrumented.


I'll be using[Metoro](https://metoro.io/features/ai-agent-monitoring) to monitor OpenClaw as the instrumentation is done at the kernel level with eBPF. This means that we can see exactly what OpenClaw is doing without needing to do any instrumentation of OpenClaw or the programs it calls.


I have OpenClaw do a number of different tasks including:


- Checking emails
- Getting todos from notion
- Checking the weather


We can see the network calls in Metoro even though OpenClaw is calling them via running a` curl` command.


Service map showing OpenClaw's external API calls


The service map shows OpenClaw reaching out to various external services. We can drill down into individual requests to see exactly what's happening.


Trace search showing requests to the weather API


Clicking into a specific trace shows the full span attributes including container ID, namespace, and the exact URL being called.


Trace detail with full request attributes


Based on the traces I then set up an alert to tell me if OpenClaw ever makes a request to something that is not in this list of expected APIs.


## What's Next


Next on my list is setting up VolumeSnapshots to periodically backup OpenClaw's state. Scheduled snapshots would let me restore to a known good state if something goes wrong.


I'm also planning to experiment with giving OpenClaw access to other services in the cluster. Perhaps a local LLM for less sensitive tasks or a vector database for longer-term memory. Running it on Kubernetes makes this kind of integration much easier since everything is already on the same network.
