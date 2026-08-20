---
schema_version: "1.0.0"
document_id: "83ab0aaecf5ffb6854c3bfdd7539254b5736bdf15319796f9c03b4253cc03f4d"
company_key: "yc-ditto"
company: "Ditto"
source_id: "yc-ditto-news-import-8ae80e1b68c3"
canonical_url: "https://www.dittowords.com/blog/ai-and-agents-101-the-basics-for-content-designers"
published_at: "2026-03-24T00:00:00+00:00"
first_seen_at: "2026-07-23T07:46:02.800033+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:9c0200bfa9e015ae9bdc15ceedbd9327e7fc489692a772fa0de5f756c27fa89e"
---

# Ditto Blog | AI and Agents 101: The Basics for Content Designers

If you came into content design because you love language — the craft of finding exactly the right word, the satisfaction of a helper copy that actually helps — and now suddenly everyone is talking about pull requests, MCP servers, and agentic workflows, this post is for you.


Your role hasn't just evolved. It's transforming faster than almost any job title in the industry. And the gap between "I've heard of Claude" and "I understand what's happening to my workflow" can feel enormous.


This post is our attempt to close that gap. No hype. No vague promises. Just a clear-eyed explanation of what these tools actually are, how they're changing the way your team works, and where you fit into it — whether you use Ditto or not.


## **First: What is an agentic tool, actually?**


You've probably used ChatGPT or[Claude.ai](http://claude.ai/) . You type something, it responds. That's a **large language model (LLM)** — an AI system trained on enormous amounts of text that predicts, word by word, what the most useful response to your input would be.


An **agentic tool** is something different. It takes that same underlying AI and gives it the ability to *do things* — not just describe what it would do, but actually take actions on a computer. Open files. Write code. Run tests. Search the web. Send a message.


The critical thing to understand: agentic tools don't "decide" what to write. Technically, the underlying model is predicting the next most likely word based on everything it's been trained on, plus whatever context and instructions it's been given in the moment. That's why what you *give* it matters. The better the inputs — the more context, the more specific the instructions — the better the output.


This is the thing that matters most for content designers: **the AI doesn't have opinions about your brand voice. It has whatever you've told it.**


## **The tools you'll hear about (and what makes them different)**


[Claude.ai](http://claude.ai/)


Anthropic's web and desktop AI tool. You interact with it through a chat interface. It can connect to third-party tools like Slack, Notion, and GitHub, but it doesn't have direct access to the files on your computer by default. Good for: drafting, reviewing, researching, brainstorming.


**Claude Code**


Also from Anthropic, but completely different in how it works. Claude Code runs in your terminal (the text-based command interface on your computer) and has direct access to your codebase — the files that make up your product. It can read, write, and edit code. It can run tests. It can look at your whole project and make changes across multiple files at once. Engineers and increasingly designers and PMs are using it to build features, prototype, and iterate — fast. This is an *agentic* tool.


**Cursor**


A code editor — the software developers use to write code — with agentic AI built directly into it. It's based on VS Code, which is one of the most popular code editors in the world, so if you've ever peeked over a developer's shoulder and seen a dark screen full of colored text, there's a good chance they were using VS Code or something like it. Cursor adds AI that can understand your whole codebase and help write, edit, and explain code inline. Engineers use it constantly. It's increasingly where non-engineers are getting their first taste of coding workflows.


**Figma Make**


Figma's own agentic tool, built into their design environment. You describe what you want — "create a settings page for a mobile banking app" — and it generates mockups directly in Figma. It's the most accessible entry point for people who live in Figma already.


## **How this is changing how your team works**


Because AI can now do things beyond any one person's expertise, it lets people work outside their lane by describing the outcome they want rather than knowing the technical steps. It removes the bottleneck of technical skill for a lot of tasks.


But when it comes to product copy, **nobody is stopping to think carefully about words.** When an engineer builds a new onboarding flow in Claude Code on a Friday afternoon, the placeholder copy becomes real copy faster than it used to. The gap between "first draft" and "in production" has compressed dramatically.


Copy decisions are being made — they're just being made by AI, based on whatever context it happens to have.


## **What happened to the handoff?**


The traditional product workflow had checkpoints: design review, content review, engineering review. Each one was an opportunity to catch problems before they shipped.


Those checkpoints still exist — but they're compressed, sometimes skipped, and happening closer to production than before.


The one checkpoint that matters most now, and that content designers need to understand, is the **pull request** (or merge request, depending on your platform).


‍–––––


## **Your AI terminology guide**


A comprehensive, regularly updated terminology guide for everything you should know (but might not know to ask) about AI and agentic tooling.


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


‍––––––


## **Where copy decisions actually happen now**


Honestly? They're happening constantly, and they're mostly invisible.


Every time an agent generates a screen, writes a button label, or drafts an error message, it's making a copy decision. Not deliberately — it's predicting what the next word should be based on its training data and whatever context it's been given. If it hasn't been given your brand guidelines, your approved terminology, or your compliance requirements, it's working off of generic patterns from the internet.


That's not a crisis — it's a design problem. And like most design problems, it has a systems solution.


The teams getting this right are the ones who've stopped treating copy governance as a review step and started treating it as an input. They're building their style guide rules, approved terminology, and content standards into the tools their team uses to build — so the AI generates from the right system, not from scratch.


## **How to plug yourself in**


You don't need to become an engineer. But you do need to understand enough about how your team's development workflow works to know where your voice belongs.


**Start by learning your team's workflow end to end.** Ask an engineer (or your manager, or a friendly PM) to walk you through a feature's lifecycle: How does a ticket get created? Who picks it up? What does the review process look like? Is there a staging environment you can access? Where does code go before it goes live? This conversation will tell you more than any article.


**Identify where you want to be involved.** You don't have to be everywhere. Think specifically about: Do you want to review before something goes to production? After? Both? What types of copy would you want a human to review every time (compliance language, legal copy, anything user-facing in a critical flow)? What would you trust an AI to catch if it had the right rules?


**Think about what you would automate if you could.** Are there repetitive review tasks — checking that button labels follow capitalization rules, flagging strings that don't match your approved terminology — that a well-configured AI could handle? What would need to be true for you to trust that? Writing down your own mental checklist for copy review is a good first step toward building the automated version of it.


**Find out what tools your team uses.** If your engineering team is using Claude Code, get it set up and spend an afternoon playing with it — not to write code, but to understand how it works and what it produces. If they're using Cursor, ask someone to show you a workflow. You don't need to use it daily. You need to understand what it can and can't do.


**Get curious, not qualified.** The people who understand these tools best right now aren't certified in anything — they got there by experimenting, asking questions, and comparing notes with colleagues. The best thing you can do is find one person at your company or in your network who's genuinely into this stuff and start having regular conversations. The field is moving too fast for any course or certification to keep up.


## **What all of this means for how you think about your role**


The most useful reframe for content designers in this moment is this: **your job is shifting from writing to architecting.**


Not away from writing — but above it. You're increasingly the person who builds the system that makes writing consistent, compliant, and scalable across every tool your team uses. That means:


- Your style guide isn't a document — it's a rule set that can be enforced automatically
- Your approved copy isn't a spreadsheet — it's a library that agents can search and reuse
- Your review process isn't a handoff step — it's a workflow integrated into how code gets shipped


That can feel like a loss if you loved the craft of individual copy decisions. But it can also feel like a profound expansion of your influence — because a system you design well makes every copy decision across your whole product better, whether you're in the room or not.


The tools are going to keep changing. The specific terminology will evolve. What won't change is the core skill: understanding your team's workflow well enough to know where your expertise makes things better.


That's always been the job. The context just got a lot more interesting.


*Want to go deeper on building a content system that works in agentic workflows?*[See how Ditto connects your style guides and approved copy to Claude Code and Cursor →](https://ai.dittowords.com/)


‍
