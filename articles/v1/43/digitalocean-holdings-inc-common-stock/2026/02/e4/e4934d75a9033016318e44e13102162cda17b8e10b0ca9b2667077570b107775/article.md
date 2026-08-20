---
schema_version: "1.0.0"
document_id: "e4934d75a9033016318e44e13102162cda17b8e10b0ca9b2667077570b107775"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/claude-opus-4-6-gradient-ai-platform"
published_at: "2026-02-06T19:38:29.255+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:21:24.537254+00:00"
content_hash: "sha256:4956fae7d81f9de924d1c44836ced529faa59efc1884a8bb070ab894520aba4e"
---

# Now Available: Anthropic Claude Opus 4.6 on DigitalOcean’s Agentic Inference Cloud

Claude Opus 4.6 is now available on the DigitalOcean Gradient™ AI Platform via Serverless Inference—giving teams access to Anthropic’s most capable model on a platform built to run inference reliably at scale.


Start using the new model now, via the[API](https://docs.digitalocean.com/products/gradient-ai-platform/how-to/use-serverless-inference/) or in the[DigitalOcean Cloud Console](https://cloud.digitalocean.com/registrations/new?activation_redirect=%2Fgen-ai&redirect_url=%2Fgen-ai) .


With massive 1M-token context, adaptive reasoning, and advanced agentic coding, Claude Opus 4.6 enables teams to analyze huge datasets, refactor entire codebases, and generate high-quality outputs in a single pass. It’s also optimized for everyday knowledge work, including reports, spreadsheets, and presentations.


## What Opus 4.6 unlocks


- **Agentic coding & software development:** Plan, debug, and iterate across large codebases; perform root cause analysis; handle multilingual coding and cybersecurity tasks.
- **Knowledge work & research:** Analyze financial data, run research, and manage multi-step tasks in documents, spreadsheets, and presentations.
- **Agentic automation:** Coordinate multiple AI agents for parallel, read-heavy, or long-running tasks; summarize large contexts and make adaptive reasoning decisions.
- **Information retrieval & long-context reasoning:** Retrieve hard-to-find details across vast datasets and reason over hundreds of thousands of tokens.
- **Office productivity:** Generate structured reports, spreadsheets, and presentation decks; ingest unstructured data and produce polished outputs in one pass.


## Why Run Opus 4.6 on DigitalOcean


Claude Opus 4.6 runs natively inside your existing DigitalOcean environment—alongside your applications, data, networking, and storage—so inference becomes part of your stack, not another system to integrate or operate.


There are no separate model contracts, vendor accounts, or billing surfaces to manage. Usage is billed predictably alongside your other DigitalOcean services, with inference managed by default so you can start running Opus 4.6 quickly without provisioning or tuning infrastructure.


Safe defaults are built in from the start. Opus 4.6 runs within your DigitalOcean project with security-hardened defaults, reducing exposure and operational risk as workloads scale.


The result: you can build, deploy, and scale AI applications with Opus 4.6 using App Platform, Kubernetes, Managed Databases, and storage—all in the same environment, with fewer moving parts and less overhead.


## Get started


Opus 4.6 is available on DigitalOcean Serverless Inference, so there’s no infrastructure to provision or manage. Authenticate with your[model access key](https://cloud.digitalocean.com/gen-ai/model-access-keys?i=b59231) and see a response immediately using the curl request below.


```text
curl https://inference.do-ai.run/v1/chat/completions \
-H "Authorization: Bearer YOUR_MODEL_ACCESS_KEY" \
-H "Content-Type: application/json" \
-d ' {
"model": "anthropic-claude-opus-4.6",
"messages": [
{
"role": "user",
"content": "What is the capital of France?"
}
],
"temperature": 0.7,
"max_tokens": 1000
}  '


```


You can also test the new model in the[DigitalOcean Model Playground](https://cloud.digitalocean.com/gen-ai/model-playground) or compare it to other existing models.


**🚀 Access Opus 4.6 today via the DigitalOcean[API](https://docs.digitalocean.com/products/gradient-ai-platform/how-to/use-serverless-inference/) or in the[Cloud Console](https://cloud.digitalocean.com/registrations/new?activation_redirect=%2Fgen-ai&redirect_url=%2Fgen-ai) .**


Want to learn more about Opus 4.6 and accessing it via DigitalOcean?[Check out our article here](https://www.digitalocean.com/community/tutorials/claude-opus) .
