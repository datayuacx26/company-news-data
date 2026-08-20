---
schema_version: "1.0.0"
document_id: "f432cd24927e6c5b5ea743f4347b6583ae26f281babe227fdc32f0b521d6b038"
company_key: "yc-osmosis"
company: "Osmosis"
source_id: "yc-osmosis-news-import-de01a8838bba"
canonical_url: "https://osmosis.ai/blogs/applying-rl-open-source-slm-trained-for-mcp"
published_at: "2025-05-08T00:00:00+00:00"
first_seen_at: "2026-07-25T18:22:44.798253+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:71c96fe0f003e152d97eb7a8dc2368b145f227b3a03eb768599ffdd06b855b70"
---

# Applying RL: Open Source SLM Trained for MCP

MCP is quickly becoming the open standard for AI agents, and for good reason! It’s well designed and easy to use — however, MCP implementations are limited since:


1. The best models are large and closed-source (3.7 Sonnet, Gemini 2.5 Pro)
2. It introduces tool sprawl (numerous MCP clients, MCP servers to integrate)


We wanted to fix this!


Using reinforcement learning, we trained a 4B model that can hook into any MCP client to work with every MCP server. And it’s open source! Download the model[here](https://huggingface.co/osmosis-ai/osmosis-mcp-4b) , and check out a quickstart example[here](https://github.com/Osmosis-AI/Osmosis-MCP-4B-demo) .


In our MCP exploration, we found that most models besides the aforementioned 3.7 Sonnet and Gemini 2.5 were fairly inconsistent when it came to MCP tool usage. And the reason why is simple: MCP is a great toolbox, but most ‘brains’ (i.e. LLM) don’t know how to use the toolbox well.


That leads us towards training a model (via Dr. GRPO) to become an expert at using MCP. We went with Qwen3-4B as the model of choice given existing function calling ability, and used VeRL’s ability to conduct multi-turn tool-call training with SGLang. We added in scenarios (e.g. using weather data to determine the right clothes to wear) that required multiple instances of tool-use and chain-of-thought.


The results are strong — we tested Osmosis-MCP-4B on GSM8K and we were able to reach performance parity with leading foundation models:


And the best part is that since the model is open source, you can use SFT or RL to specialize the model even further for any particular use case. We’re excited to see how developers use Osmosis-MCP-4B — please share any projects or demos with us on X!


Finally, if you are interested in a continuously improving custom version of Osmosis-MCP-4B (or any other open source models), reach out to[us](https://osmosis.ai/cdn-cgi/l/email-protection#167d7765736f5679657b79657f6538777f) !
