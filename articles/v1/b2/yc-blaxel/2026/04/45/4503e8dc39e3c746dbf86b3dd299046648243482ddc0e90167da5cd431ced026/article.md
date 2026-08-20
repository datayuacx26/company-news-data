---
schema_version: "1.0.0"
document_id: "4503e8dc39e3c746dbf86b3dd299046648243482ddc0e90167da5cd431ced026"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/hands-on-ai-sandbox-workshop-at-open-source-day-2026"
published_at: "2026-04-21T15:01:56+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:52:23.948620+00:00"
content_hash: "sha256:260acc94b2563c8aa6b4d360d7c3aed9e3260f67ffb74e5775d74fc6972a7a3e"
---

# Hands-on AI sandbox workshop at Open Source Day 2026

I was recently invited to speak at[Open Source Day 2026](https://osd.opensourceweekend.org/) , one of India's largest community-driven conferences focusing on open source software. Aimed at developers and maintainers, it was designed as a full day event in Gandhinagar with technical talks, workshops, and plenty of time for networking.


My focus this year was a hands-on workshop for AI builders titled "Give Your Coding Agent a Perpetual Sandbox for Isolated Execution". As the name suggests, sandboxes and agents were the main themes.


I began with a quick overview of the problem space: the risks of arbitrary agent-generated code and how containers, serverless functions, and sandboxes compare in terms of the properties needed for agentic workflows - isolation, speed, reproducibility.


Next was a hands-on coding session. I walked attendees through creating a sandbox in code with Blaxel's open source[Python SDK](https://docs.blaxel.ai/sdk-reference/sdk-python) , integrating an agent using the[sandbox’s built-in MCP server](https://docs.blaxel.ai/Sandboxes/MCP) and the (also open source)[Strands SDK](https://strandsagents.com/) , and building a sandboxed coding agent that created a Next.js website for us, complete with[live preview](https://docs.blaxel.ai/Sandboxes/Preview-url) .


For some of the attendees, sandboxes were a new concept - it was nice to see that they "got it" and understood the value immediately. On the other hand, some attendees were already actively using sandboxes in their products, which led to a number of interesting hallway conversations after the session ended. We talked about sandbox architecture, designing for scalability (a sandbox per agent? per customer?), data retention, and cost management.


All in all, this was an event with great conversations, a fantastic crowd, and a solid opportunity to showcase Blaxel's sandbox capabilities. Looking forward to the next one!
