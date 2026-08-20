---
schema_version: "1.0.0"
document_id: "5564880f10e6ee33d447d1fa6a23fed25617df717bcc78ecfd4ac9f049a05345"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/poblaxel-x-rippletide-ensuring-safety-and-trust-for-enterprise-agentsst"
published_at: "2025-12-11T00:46:36+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:55:05.658267+00:00"
content_hash: "sha256:5c5fa0093195d476748ab15ac149f029ec24c98a04003f4ea847264850175934"
---

# Rippletide and Blaxel partner for secure and trustworthy AI agents

Today, we're excited to announce our partnership with[Rippletide](https://rippletide.com/) , enabling enterprises to run autonomous, trustworthy agents directly onto Blaxel's high-performance sandboxes.


## Building secure, trusted enterprise agents


In the past, companies have often had to choose: build an agent that is enterprise-ready and can answer and act reliably, or build one that is fast and scalable. Today, this is no longer true. With sophisticated agent tooling and high-performance architecture readily available, companies can have both.


Rippletide provides an advanced hypergraph database that combines memory and reasoning to reduce agent hallucinations. At Blaxel, we provide secure and high-performance computing infrastructure for AI agents. Our partnership combines these distinct solutions, providing enterprises with a unified full-stack solution for building, deploying and scaling trusted production-grade AI agents that can run code without hallucinating.


## How it works


Spinning up a secure compute environment (like a container or VM) for every single task is slow, expensive, and adds critical seconds of latency. For real-time use cases like a sales-assist tool or a live customer support bot, even a 800-millisecond delay is a failure.


Our platform is built for speed and security, offering agent-native compute environments with millisecond-boot times. Rippletide-powered agents benefit from our airtight isolation (ideal for arbitrary code execution), state persistence across sessions, and real-time debugging and observability. We've also re-architected our core network proxy and slashed latency by 9x, ensuring that even when the agent needs to fetch data from an external database service, we can meet the demanding response times required for real-time agent interactions.


The result? Every Rippletide agent gets an isolated, secure compute environment that spins up, executes code, and returns results almost instantly.


## Get started


To see this in action, try our[customer support agent template](https://github.com/blaxel-templates/template-rippletide-customer-support/) . This template deploys an intelligent customer support agent powered by Rippletide's hypergraph database on Blaxel's infrastructure.


Follow these steps:


1. Clone the[template repository](https://github.com/blaxel-templates/template-rippletide-customer-support/) (or run` bl new agent` ) and follow the instructions to install dependencies.
2. Add your Rippletide API key to the` .env` file and populate the` knowledge-base` folder with your company's Q&As, workflows, and guardrails.
3. Run the setup script to create an agent and obtain and agent ID:` uv run setup.py`
4. Add the agent ID to the` .env` file
5. Deploy the agent in one command:` bl deploy`


Get started today with Rippletide at rippletide.com and Blaxel at app.blaxel.ai, or visit our[documentation](https://docs.blaxel.ai/) to learn more.
