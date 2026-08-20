---
schema_version: "1.0.0"
document_id: "ba4d5346be1df1da6f04caf7dda226113ca25860d75a0d01d369920bc4addf14"
company_key: "yc-asteroid"
company: "Asteroid"
source_id: "yc-asteroid-news-import-1b74f0bf6416"
canonical_url: "https://asteroid.ai/blog/when-will-browser-agents-do-real-work/"
published_at: "2025-11-13T00:00:00+00:00"
first_seen_at: "2026-07-21T08:06:12.451370+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:9e04b4ead0b751f2d607b52320f0460318863259152d1fce550b00cbfa527f31"
---

# When will browser agents do real work?

> **📖 This post summarizes an article published on[InfoWorld](https://www.infoworld.com/article/4081396/when-will-browser-agents-do-real-work.html) .** Read the full article for complete insights from our founding engineers.


In January 2025, OpenAI released[Operator](https://openai.com/index/introducing-operator/) —the first large-scale agent powered by a computer-use model to control its own browser. The demo was impressive: an AI moving the mouse, clicking buttons, and performing actions like a human would. But just eight months later, OpenAI quietly discontinued Operator and rolled it into ChatGPT’s new Agent Mode.


The shift reflected a hard-earned truth: **computer-use models don’t yet work reliably enough in production.**


## Vision-Based vs DOM-Based Agents


The article explores two fundamental approaches to browser automation:


**Vision-based agents** treat the browser as a visual canvas. They analyze screenshots, interpret them using multimodal models, and output low-level actions like “click (210,260)”. This mimics how humans use computers, but comes with precision and performance tradeoffs—visual models are slower, require scrolling through entire pages, and struggle with subtle state changes.


**DOM-based agents** , by contrast, operate directly on the Document Object Model—the structured tree that defines every webpage. Instead of interpreting pixels, they reason over textual representations: element tags, attributes, ARIA roles, and labels. Modern preprocessing techniques like accessibility snapshots (popularized by Microsoft’s Playwright MCP server) transform the live DOM into structured, readable text that language models can understand better than pure HTML.


DOM-based control is faster and more deterministic—both crucial for enterprise workflows running thousands of browser sessions daily.


## The Hybrid Future


In practice, both methods have strengths. Vision models handle dynamic, canvas-based UIs (like dashboards or image-heavy apps). DOM-based models excel at text-rich sites like forms or portals. **The best systems today combine both** : using DOM actions by default and falling back to vision when necessary.


OpenAI’s decision to deprecate Operator led directly to the creation of the new ChatGPT Agent, which embodies this hybrid approach. Under the hood, it can use either a text browser or a visual browser, choosing the most effective one per step.


## Learning by Doing: The Next Frontier


Hybrid systems solve reliability for today, but the next challenge is adaptability. How can a browser agent not just complete a task once, but actually learn from experience and improve over time?


A promising strategy is to let agents explore workflows visually, then encode those paths into structured representations like DOM selectors or code:


1. **Exploration phase** : The agent uses computer-use or vision models to discover the structure of a new web page and record successful navigation paths.
2. **Execution phase** : The agent compiles that knowledge into deterministic scripts (Playwright, Selenium, or CDP commands) to repeat the process with high reliability.


With new large language models excelling at writing and editing code, these agents can self-generate and improve their own scripts, creating a cycle of self-optimization.


## The Bottom Line


While computer-use models are still too slow and unreliable, browser agents are already becoming production-ready—even in critical sectors such as healthcare and insurance. The future of browser agents lies not in vision or structure alone, but in orchestrating both intelligently.


---


**Read the full article:**[When will browser agents do real work?](https://www.infoworld.com/article/4081396/when-will-browser-agents-do-real-work.html) on InfoWorld


Ready to build production-ready browser agents?[Get started with Asteroid](https://platform.asteroid.ai/) or[book an onboarding demo](https://calendly.com/founders-asteroid-hhaf/new-meeting) .
