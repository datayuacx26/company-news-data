---
schema_version: "1.0.0"
document_id: "ded7ac15ed5529b532cbdb8182f7c692d96480f221933c6e032e7904ad8bb7c9"
company_key: "yc-speak"
company: "Speak"
source_id: "yc-speak-news-import-4988501dda0c"
canonical_url: "https://www.speak.com/blog/what-i-learned-going-from-marketer-to-builder"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-22T14:26:08.696382+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:cf2588403323a9de721def27e032f5aa4b0c6c8e673e769abcd895e0d6b4863d"
---

# What I Learned Going From Marketer to Builder

As an influencer marketing manager, I used to have a massive headache managing our entire process end-to-end across Google Sheets, Google Forms, and email. Our days were consumed by reading emails to update sheets, sending follow-ups when influencers missed deadlines, and chasing down deliverables.


Instead of focusing on strategy, most of my daily resources were spent just keeping the process afloat.


That all changed when we built[Musubime](https://www.speak.com/blog/using-agentic-coding-to-solve-influencer-ops) , an influencer management dashboard for my team, using agentic coding.


As soon as we implemented Musubime, we saw a 70% drop in communication volume, saving us roughly an hour per creator. That freed us up to spend more time reviewing drafts and giving meaningful feedback, which improved both content quality and performance.


The success of Musubime changed my career — I went from a marketer who manages a process to one who builds the systems that run it.


Speak backed this shift immediately. They gave me the opportunity to shadow the core engineering team in the San Francisco office, and I later attended Anthropic's "Code w/ Claude" event in Tokyo.


This post is for marketers who are in the exact position I was in six months ago — drowning in process management instead of building strategy. You can build tools yourself to automate your day-to-day work so you can focus on what actually matters. Here are three lessons I've learned over the past six months.


#### Lesson 1: Understand First, Build Second


LLMs write code that works. They're excellent at executing instructions, but they don't know your underlying intentions. Even if your product seems fine at first, if you don't understand how the system works at a high level, you'll find the gaps after it's already in production.


That's why you need to thoroughly understand what you're building before you build it, even if you're not the one writing the code. This doesn't require deep coding skills — you just need to be able to explain what the system is doing in plain language.


When I first started coding with AI, my workflow was flawed. I'd throw a messy problem at a model and expect it to hand back a clean system architecture. One of my biggest mistakes building Musubime was letting the LLM design the database schema and its rules without fully understanding them myself. I had to rebuild the entire schema later because of it.


While shadowing our engineering team in San Francisco, I noticed how much time they spend on planning, not just writing code. Models are improving fast, but you still need absolute clarity on your proposed solution before you begin. At Code w/ Claude in Tokyo, an Anthropic engineer told me the same thing: they spend most of their time planning, not typing.


With Boris Cherny (Head of Claude Code) @ Code with Claude in Tokyo


#### Lesson 2: Knowing the Problem Mattered More Than Knowing the Tools


Building tools with AI is addictive. It's easy to fall down rabbit holes mastering technique. But the lesson I keep coming back to is that your skill with Codex or Claude Code matters far less than knowing exactly what to build and why.


Before agentic coding, writing software required specialized programmers. Now that AI closes that execution gap, understanding the problem and architecting the solution is the real differentiator. Coding is a means to an end.


That's why first-principles thinking is the most critical skill for building with AI — not how many "How to use Claude Code" tutorials you've watched.


If you're the one living and breathing a specific marketing problem, you're often the best person to build the solution — not the most senior engineer at your company, and not an outside agency. You understand the problem intimately. That's your edge.


We built Musubime because we hit these pain points every day, which let us tailor the solution to our exact process. Handing it to a traditional engineering team would have gotten us a worse solution than the one we built ourselves.


An Anthropic engineer at the Tokyo event confirmed this: their best engineers aren't the ones with the deepest technical backgrounds. They're the ones who reason well from first principles.


Marketers can be great builders — and often better builders of marketing tools than product engineers or software companies who are two steps removed from the actual problem.


#### Lesson 3: Context Was the Real Unlock


Ask any developer what matters most for AI-assisted development, and the answer is almost always the same: context.


Without context, an LLM guesses. And it doesn't guess timidly — it proceeds with confidence, fills in the gaps itself, and hands you a system that technically runs but no longer matches how things actually work.


One thing the engineers at Speak told me stuck with me: you have to give an agent "eyes." A coding agent that can only see the current file is half blind. It needs the repo structure, the operational docs, the task list, the database shape, the logs, the screenshots, sometimes the LLM traces from the product it's debugging.


That changed how I set up my own workspace. I restructured my local directory so it could act as a real agent environment. My operational markdown files, active project goals, task lists, reusable agent skills, and the Musubime codebase now live in one place.


If I ask it to handle an ambiguous creator reply, it can read the creator-email rules, the campaign workflow, and the Musubime database context, then figure out whether the message is a price negotiation, a scheduling issue, a contract concern, or a normal follow-up. It drafts the right response and flags anything that needs my approval.


As that directory gets richer, it becomes the agent's world. It stops needing a long explanation before every task — it reads the environment, understands the operation, and handles heavier work on its own: debugging sync jobs, drafting safe replies, checking data quality, updating internal tools, keeping documentation aligned with how the system actually works.


#### Join Us


Speak gave me the room to redefine what a marketer does. Agentic coding might not feel like your job as a marketer. It is now, and it's the fastest way to get your time back from process work. Speak gives marketers access to frontier AI models and tools, not just engineers.


If you want to be a builder and a marketer, we're hiring. Check out our open roles:[https://www.speak.com/careers](https://www.speak.com/careers)


‍
