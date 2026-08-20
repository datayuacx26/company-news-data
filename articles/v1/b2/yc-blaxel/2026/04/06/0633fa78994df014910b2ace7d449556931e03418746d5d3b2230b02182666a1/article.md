---
schema_version: "1.0.0"
document_id: "0633fa78994df014910b2ace7d449556931e03418746d5d3b2230b02182666a1"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/native-support-for-blaxel-sandboxes-in-the-new-open-ai-agents-sdk"
published_at: "2026-04-15T08:33:49+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:52:37.138540+00:00"
content_hash: "sha256:d1ab65d47c3261bd811a80306e7b151f65f0e74ba51adbcebbc696b3c3b884dd"
---

# Native support for Blaxel sandboxes in the new OpenAI Agents SDK

We're proud and excited to be a first-class sandbox provider in the new[OpenAI Agents SDK](https://developers.openai.com/api/docs/guides/agents-sdk) !


With this SDK, agents can directly create and deploy Blaxel[sandboxes](https://docs.blaxel.ai/Sandboxes/Overview) on demand to run commands, create files, and write and test arbitrary code. These sandboxes run on Blaxel’s high-performance, AI-native infrastructure platform, with automatic scale-to-zero and world-class boot/resume times.


This integration is perfect for building the next generation of coding agents or data analyst agents:


- Each agent gets an isolated[Blaxel sandbox](https://docs.blaxel.ai/Sandboxes/Overview) per task and can freely execute arbitrary code without any risk to the host.
- Blaxel sandboxes resume from standby in ~25ms with full memory state, producing unbeatable performance for almost all sandbox operations. You can choose to[auto-delete](https://docs.blaxel.ai/Sandboxes/Expiration) them once the agent finishes, or keep them running for longer durations.
- Agents can render[live application previews](https://docs.blaxel.ai/Sandboxes/Preview-url) as they work, so human users can build, step away, and inspect or provide further instructions when ready. Both public and private previews are supported.
- The system scales horizontally without any manual coordination. Agents can work in a single sandbox or spin up multiple sandboxes per task.


Once you've built and tested your agents, you can also[deploy them to Blaxel](https://docs.blaxel.ai/Tutorials/OpenAI-Agents-SDK-Deployment) with minimal code editing and zero configuration, enabling you to benefit from near-instant response times by colocating them close to the sandboxes the agents are already using.


Get started with our integration guides here:[https://docs.blaxel.ai/Tutorials/OpenAI-Agents-SDK-Index](https://docs.blaxel.ai/Tutorials/OpenAI-Agents-SDK-Index)
