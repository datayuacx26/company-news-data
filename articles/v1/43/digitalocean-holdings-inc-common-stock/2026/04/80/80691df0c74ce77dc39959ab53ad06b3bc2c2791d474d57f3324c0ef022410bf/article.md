---
schema_version: "1.0.0"
document_id: "80691df0c74ce77dc39959ab53ad06b3bc2c2791d474d57f3324c0ef022410bf"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/run-advanced-reasoning-with-arcee-ai-trinity"
published_at: "2026-04-01T20:09:27.856+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:16:07.458343+00:00"
content_hash: "sha256:4e7716fe2ad99649c6d624dfa6a85fcf7a9c1de8c7314e594d03a6110d46d75b"
---

# Run Advanced Reasoning on DigitalOcean with Arcee AI's Trinity Large-Thinking

Today, we’re announcing that[Arcee AI](https://www.arcee.ai/) ’s Trinity Large-Thinking is now available in Public Preview on DigitalOcean’s Agentic Inference Cloud, giving developers the ability to run frontier-class reasoning workloads without managing infrastructure or stitching together complex systems.


DigitalOcean is proud to partner with Arcee to bring Trinity Large-Thinking to AI builders, available via Serverless Inference, on day one. Instantly available and queried directly through the DigitalOcean Cloud Console or API alongside the compute, data, and services you already run on DigitalOcean.


## Why this model, why now


Trinity Large-Thinking didn’t emerge in a vacuum. It’s been pressure-tested in exactly the kind of workloads DigitalOcean is built for.


Arcee is a 26-person San Francisco startup that spent nine months building a full open-weight model family from the ground up, with the explicit goal of producing models developers and enterprises could actually own. The result is a family ranging from 4.5B to 400B parameters, and a top-of-stack reasoning model that has earned its place in production.


In its first two months, Trinity served over 3.4 trillion tokens on OpenRouter, becoming the most-used open weight model in the U.S., driven by always-on, agentic workloads running continuously.


Trinity Large-Thinking builds on that foundation with extended reasoning, stronger multi-turn tool use, and more stable long-running behavior. It ranks #2 on PinchBench (Kilo’s benchmark for agentic model capability) at approximately 96% lower price point than the top-ranking model.


Developers shouldn’t have to choose between a model that can reason and one they can afford to run at scale. Thanks to the partnership between DigitalOcean and Arcee, they don’t have to.


## Built for real-world agent workloads on DigitalOcean


Reasoning workloads are long-running, multi-step, and deeply integrated into the rest of your stack. This is crucial for building agents and complex applications that dynamically interpret unstructured data and execute complex, multi-step action sequences.


On DigitalOcean’s Agentic Inference Cloud, Trinity Large-Thinking runs as part of a complete system and not a standalone model endpoint you have to wire up yourself.


With this launch, you get:


-


**Frontier reasoning at usable economics:**[#2 on PinchBench](https://www.arcee.ai/blog/trinity-large-thinking) for agentic tasks at ~$0.90/M output tokens. Capable enough for complex systems, affordable enough to run continuously.


-


**Integrated infrastructure:** Run agents alongside your Kubernetes clusters, databases, and storage. No stitching across vendors.


-


**Instant, serverless access:** No provisioning or scaling. Query immediately via API or console, your infrastructure adapts to your workload.


-


**Full model control:** Apache 2.0 licensed weights available on Hugging Face. Inspect, fine-tune, distill, or self-host as needed.


## A new phase of AI infrastructure


This is what the next phase of AI infrastructure looks like: integrated systems where reasoning, data, and compute run together.


As more workloads shift toward continuous, agent-driven execution, the platform they run on matters just as much as the model itself.


Hear more about Trinity Large-Thinking and the partnership between DigitalOcean and Arcee from CEO Mark McQuade at[Deploy](https://www.digitalocean.com/deploy?internal_ref=blog_acee) on April 28th in San Francisco.[Save your spot](https://www.digitalocean.com/deploy?internal_ref=blog_acee#register) to attend live.


## Get started in seconds


Trinity Large-Thinking is live now in Public Preview on DigitalOcean Serverless Inference. You can start running advanced reasoning workloads immediately, without managing infrastructure, and without compromising on cost.


Get started quickly using the request below:


```text
curl    --location    '[https://inference.do-ai.run/v1/chat/completions](https://inference.do-ai.run/v1/chat/completions)'    \
--header    'Authorization: Bearer $DO_API_TOKEN'    \
--header    'Content-Type: application/json'    \
--data    '{
"model": "trinity-large-thinking",
"messages": [
{
"role": "user",
"content": "What is the capital of France?"
}
],
"temperature": 0.7,
"max_completion_tokens": 256
}'


```
