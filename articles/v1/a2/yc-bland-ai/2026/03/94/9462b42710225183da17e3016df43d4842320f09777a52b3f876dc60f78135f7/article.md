---
schema_version: "1.0.0"
document_id: "9462b42710225183da17e3016df43d4842320f09777a52b3f876dc60f78135f7"
company_key: "yc-bland-ai"
company: "Bland AI"
source_id: "yc-bland-ai-news-import-c77b84ac3c61"
canonical_url: "https://www.bland.ai/blog/norm-build-voice-agents-from-prompt"
published_at: "2026-03-25T12:55:00+00:00"
first_seen_at: "2026-07-21T10:41:07.173500+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:99e5e105bda1d0c7b65ec3e960c3ca9673a93eb2bc7aa44853ec99647c7d44e2"
---

# Introducing Norm: The first AI assistant that builds voice agents from a prompt

People have been talking to each other for about 100,000 years. We’ve been typing to each other for roughly 150. Speech is still how most important things get said. It carries tone, pace, feeling. You can say more in ten seconds of talking than in a paragraph of text.


ChatGPT proved that a computer could hold a written conversation that actually made sense. It became the fastest-growing product in history. But it was text. Voice stayed hard. Building a voice agent that could handle real conversation, interruptions, pauses, and respond in under 400 milliseconds required a kind of expertise most teams simply didn’t have.


Today, Bland is introducing **Norm** : the first AI assistant that builds production-ready voice agents from a prompt. You describe what you want. Norm builds it. That’s it.


**Why Voice Has Been So Hard**


Prompting a chatbot and prompting a voice agent are not the same job. A voice agent is stateful. It has to track where it is in a conversation, handle someone talking over it, know when to pause, and do all of this in real time. Most of the work isn’t creative. It’s structural: validation logic, extraction rules, branching conditions, API calls at the right moment.


Until now, most Bland customers relied on Forward Deployed Engineers to get this right. Norm codifies what those engineers know. It gives every customer the equivalent of their own.


We’ve spent years learning what makes a voice agent work in production. The timing, the structure, the hundred small things that go wrong when you treat voice like chat. Norm is all of that knowledge in a tool. You tell it what you want. It builds something that actually works on the phone.


**How It Works**


You type an instruction. For example: “Build me a full scheduling agent and integrate with my Cal.com.” Norm generates the prompt, the agent, pathways, validation conditions, and extraction rules. For more complex workflows, you can add an API call mid-conversation and test the request before it goes live.


Every change is created on a safe branch. You can review diffs between the original and updated prompts, and simulate agent-on-agent calls to stress-test before deploying. Nothing touches production until you say so.


**What Norm Does**


- **Natural-Language prompting:** Describe what your voice agent should do. Norm generates the structured prompt, persona, agent, and pathways for you automatically.
- **Diff comparisons:** Compare changes between the original and updated prompts before you deploy.
- **Safe branching:** All updates happen in a protected branch. Review and validate before going live.
- **API Integrations:** Add external data sources or actions mid-conversation. Test requests before activation.
- **Agent-on-agent simulations:** Pit your agent against a simulated caller. Find the edge cases before your customers do.
- **Files & Knowledge Bases:** Upload files or attach your existing Knowledge Bases for Norm to reference when building or modifying your pathways.
- **Self-service execution** : Technical teams can launch or modify agents with Norm available 24/7 as their personal FDE.


Norm is available now within the[Bland platform](https://app.bland.ai/) . We hope you give it a try!


‍
