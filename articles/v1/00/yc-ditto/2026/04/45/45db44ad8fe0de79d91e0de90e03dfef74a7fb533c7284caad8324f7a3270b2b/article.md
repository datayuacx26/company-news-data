---
schema_version: "1.0.0"
document_id: "45db44ad8fe0de79d91e0de90e03dfef74a7fb533c7284caad8324f7a3270b2b"
company_key: "yc-ditto"
company: "Ditto"
source_id: "yc-ditto-news-import-8ae80e1b68c3"
canonical_url: "https://www.dittowords.com/blog/ai-agents-101-your-terminology-guide"
published_at: "2026-04-07T00:00:00+00:00"
first_seen_at: "2026-07-23T07:46:02.800033+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:f79a95efa928b2f928188161a3b7a2a9adc64c86a3b3a33be24df715154c24b9"
---

# Ditto Blog | AI & Agents 101: Your Terminology Guide

If you came into content design because you love language — the craft of finding exactly the right word, the satisfaction of a helper copy that actually helps — and now suddenly everyone is talking about pull requests, MCP servers, and agentic workflows, this post is for you.


We're compiling (and continuously adding to) this comprehensive terminology guide for everything you should know (but might not know to ask) about AI and agentic tooling.


### **The AI basics**


**LLM (Large Language Model)**


The technology behind Claude, ChatGPT, and Gemini. Trained on massive amounts of text, it predicts the next word based on patterns (not understanding). Better inputs mean better outputs.


**Model**


A specific version of an LLM. Claude 3.5 Sonnet and Claude 3 Opus are different models — different capabilities, speeds, and costs. "Which model are you using?" means which version.


**Agent / agentic tool**


An AI that can *do things* , not just respond. Regular LLMs tell you how to do something. Agents actually do it — reading files, writing code, running searches. Claude Code and Cursor are agentic tools.


**Context window**


The AI's working memory — everything it can "see" at once. Instructions or conversation history that falls outside the window stops influencing the output. Keep instructions concise and front-loaded.


**System prompt**


Instructions given to an AI before the conversation starts, shaping everything it produces. Users usually don't see it. When you add copy rules to a[CLAUDE.md](http://claude.md/) file, you're contributing to the system prompt for your project.


**Prompt / prompt engineering**


A prompt is what you give the AI. Prompt engineering is crafting it carefully for better results. Specificity is the whole game — and content designers are often naturally good at it. It's brief-writing.


**Hallucination**


When an AI confidently produces something wrong. Plausible-sounding, polished, incorrect. Any factual content — product names, dates, regulatory language, pricing — needs human verification before it ships.


**RAG (Retrieval-Augmented Generation)**


A technique that lets AI pull from a specific document or database at generation time, rather than relying only on training data. It's what makes it possible for an AI tool to generate *your* copy instead of generic copy — by retrieving your actual style guide or approved strings as context.


### **The tools**


**IDE (Integrated Development Environment)**


The software developers use to write code — like Word, but for code. VS Code is the most popular. Cursor is an IDE with AI built in. This is where a lot of product decisions, including copy decisions, are now being made.


**Terminal (also: command line, CLI)**


A text-only interface for controlling a computer — no icons, just typed commands. Claude Code runs here. You don't need to use it, but knowing it exists explains why "just check Figma" is increasingly an incomplete picture of where work happens.


### **How code gets made and reviewed**


**Branch**


A parallel version of the codebase where work happens without touching the live product. Features are built on branches, then reviewed and merged into main.


**Main (or Master) branch**


The authoritative version of the codebase — what's live, or about to be. Merging to main is roughly equivalent to publishing.


**Commit**


A saved snapshot of a specific set of changes, with a message describing what changed and why. The building block of version history — including copy history.


**Push**


Uploading commits to the shared repository so the team can see them. Before a push, changes exist only on one person's machine.


**Pull request (PR) / Merge request (MR)**


The formal request to merge a branch into main — and the primary review checkpoint before code ships. A PR shows exactly what changed, including copy, in a visual diff. Getting eyes on your team's PRs is one of the highest-leverage moves a content designer can make right now.


**Merge**


Combining a branch into main after a PR is approved. Before merge = last chance to review. After merge = significantly harder to change.


**CI/CD (Continuous Integration / Continuous Delivery)**


An automated pipeline that runs checks every time code changes. Tests, linting, rule enforcement — all triggered automatically. The infrastructure that makes automated copy review possible at scale.


**Staging environment**


A working version of the product that's not live to users. If you can get access to staging, you can review copy in the actual product interface — buttons in context, error messages in place — instead of a doc or spreadsheet. Worth asking for.


**Production**


The live product. What real users see. "Shipping to production" means it's out in the world.


### **How AI connects to your work**


**MCP (Model Context Protocol)**


An open standard that lets AI tools connect to external systems and take specific actions within them. A GitHub MCP lets an agent read and write to GitHub. A Ditto MCP lets an agent fetch style guide rules and search approved copy. "We have an MCP for that" means that system can talk directly to your AI tool.


[CLAUDE.md](http://claude.md/) **/ .cursorrules /**[agents.md](http://agents.md/)


A plain text file in your project that Claude Code ([CLAUDE.md](http://claude.md/) ) or Cursor (.cursorrules) reads automatically at the start of every session. It's a standing brief — instructions and rules that apply to every interaction without you repeating them. If there's no copy guidance in your project's[CLAUDE.md](http://claude.md/) , that's a gap you can fill.


**Hardcoded string**


Text written directly into code rather than stored in a content system. Common in fast-moving teams — an engineer writes a button label in the code because it's faster. Hardcoded strings are harder to find, review, translate, and update at scale.


**Token**


The unit an LLM processes — roughly a word fragment. Models have token limits per request, and usage is priced per token. You don't need to count tokens, but it explains why the AI sometimes cuts off or behaves unexpectedly with very long documents.


‍
