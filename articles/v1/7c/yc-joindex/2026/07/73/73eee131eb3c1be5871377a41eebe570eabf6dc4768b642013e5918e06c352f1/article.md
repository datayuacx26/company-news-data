---
schema_version: "1.0.0"
document_id: "73eee131eb3c1be5871377a41eebe570eabf6dc4768b642013e5918e06c352f1"
company_key: "yc-joindex"
company: "Dex"
source_id: "yc-joindex-news-import-5787fcdf0962"
canonical_url: "https://www.joindex.com/blog/openclaw"
published_at: null
first_seen_at: "2026-07-21T16:11:25.745021+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:5498e9015e4621053be3fad96d091d930d7edd61ab8bf2dc9a959bf9b8a27701"
---

# OpenClaw’s Wave on Agentic AI

Recently, there's been an incredible craze surrounding the advent of[OpenClaw](https://github.com/openclaw/openclaw) , a simple, open-source application that is allegedly able to execute almost any task on your computer through a unified Gateway interface that allows you to chat with it through a majority of existing chat platforms (including WhatsApp, Telegram, Discord, Slack, Signal, etc.). Anthropic has also recently thrown fuel onto the fire by[locking down their subscription](https://x.com/trq212/status/2009689809875591565) while GPT has expanded support for open-source agents through its[Codex CLI](https://developers.openai.com/codex/cli/) .


More importantly, as a result of all of this, the debate surrounding agents and personal assistants, how capable they really are, and how much they should be allowed to access for any given user has risen to a fever pitch. This post aims to provide an opinionated but thorough overview surrounding the current landscape of personal assistants in various domains, and provide a few specific claims regarding the foundations of AI assistants through the lens of the current debate surrounding OpenClaw.


## Understanding agents and assistants


Today's world of AI is now dominated by two major classes of software: **agents** and **assistants** . **Agents, firstly, are generally meant to be autonomous.** Specifically, most agents have the capability of executing long-horizon tasks in relatively more open action spaces with minimal human supervision, and as a result are usually run as "set-it-and-forget-it" applications where the user tells the agent to do some long-form task that it then sees through to completion on its own.


In comparison, **assistants tend more toward keeping humans in the loop** . Most assistant-style software today is often found in as a standalone application or application addon (e.g. browser extension or a plugin) that augments the user experience with AI generations, explanations, and short executions wherever possible. In this vein, assistants often have less capabilities and are more focussed on bringing in the relevant, streamlining information where necessary (especially for knowledge workers) such that they're able to catalyse human productivity instead of replacing humans on their tasks.


We can see a couple examples as follows.


### Software engineering


Most of today's existing coding assistants are built either as a CLI (e.g.[Claude Code](https://code.claude.com/docs/en/overview) and[Codex](https://openai.com/codex/) ) or as a standalone application ([Cursor](https://cursor.com/) ,[Windsurf](https://windsurf.com/) , etc.). These coding assistants are focussed on human-first code and maximising the efficiency of the human software engineering process by introducing AI where possible while still allowing humans to remain in the loop and maintain primary control over what's being produced. In particular, companies like Cursor have focussed primarily on minimising the context switch between using an LLM or chat-based assistant and coding, allowing strong developers to level up their productivity through features like more efficient tab autocomplete, better suggestions, and faster, more streamlined edit models.


Despite the general trend of AI-based IDEs, a number of companies have also spawned more directly **agentic** services, which is where the "vibe coding" craze primarily comes from. Specifically, this includes applications like[Devin (from Cognition)](https://devin.ai/) ,[Jules from DeepMind](https://jules.google.com/) , and[more creative Claude Code setups](https://x.com/bcherny/status/2007179832300581177) that now exist. Many of these treat LLM-based agent harnesses as the ground truth for execution and interface only through natural language nowadays, allowing engineers to run many orders of magnitude more projects and test runs at once because these code agents are able to autonomously build, test, and deploy prototypes wherever and whenever necessary.


### Where knowledge work assistance lies


One important trend to note is that most of the more powerful developments have come in fields like software engineering and mathematics, which often have clearly verifiable, even numerical rewards that make it extremely easy to incrementally improve models based on their performance and mistakes on benchmarks. However, this is most certainly not the case for the general economically viable action space. Indeed, even most simple knowledge work tasks like replying to emails or updating calendars and spreadsheets are often challenging to optimise simply because the optimum will change with each person and each use case. It thus becomes extremely hard to extract simple, generalisable insights from any of these tasks to train into models. After all, without a simple, verifiable goal, how can models and training paradigms sprint toward that goal?


## OpenClaw: a productivity agent?


From both its marketing and the use cases that it presents, OpenClaw purports to sit closer to the agent side of the spectrum because of its strongly alleged autonomy. In fact, OpenClaw maintains a shocking level of autonomy as well as an unprecedented size of action space (since it has local control over many of the applications on a user's computer, including terminal access and the ability to browse the internet). This, coupled with the fact that it executes most of the tasks that it takes on purely by itself, certainly positions it closer to the agent side of the spectrum (the software itself would likely agree, especially given instances like[this hit piece on a developer that closed one of its PRs](https://theshamblog.com/an-ai-agent-published-a-hit-piece-on-me/) ).


However, given our above discussion of non-verifiability, this immediately begs the question: how does OpenClaw aim to solve the idea of personalisation and individually-verifiable tasks? And more importantly, is the system that it's built on a viable indication as to a possible direction to solve the overall problem of personal assistance at all, whether or not it chooses to do so in a more agentic or more assistant-based manner?


### A deep-dive into OpenClaw's architecture choices


OpenClaw runs on a persistent **Gateway** , which is a local server funneling messages from apps like WhatsApp, Telegram, Discord, or Slack into isolated agent sessions. These commands serve as the interface where the user dictates how the AI uses its tools (including terminal, file system, and browser automations), which generally occur in standard, sandboxed ReAct-style loops. In particular, however, perhaps the most notable detail in terms of the personalisation and individually-verifiable reward problem we discussed above is how the system handles its memory.


OpenClaw chooses to adhere to the filesystem paradigm of token-space memory at present day, which most closely mirrors Claude's existing system. More specifically, the system has a number of markdown notepads that are indexed via a standard SQLite plugin-based hybrid vector/semantic search, with the agent itself holding the ability to jot down notes within the different notepads. The user's conversations will also be automatically compacted and maintained within the context window of the agent at large, meaning some manner of "conversation hidden state" will also be maintained long-term. Overall, the system echoes the memory systems of frontier lab products like Claude and ChatGPT.


### Limitations of this memory architecture


There are a couple of notable limitations here with this type of system. First and foremost is that the system, like those of the labs, still relies on the performance of the models themselves as well as an increase in context window. This is a simple conclusion to be drawn from the fact that the labs still rely on retrieval across tokens to be put into the context window of the model, as well as tool calls from the agent itself to determine what is important to remember. Though it is not out of the question to bet on LLM/agent scaling and improvement, having an agent-first approach becomes unreliable and nondeterministic because of the stochasticity and opacity of the agents themselves.


One other major problem to highlight is the idea that the OpenClaw system still only has access to "what its told” i.e. what messages the system receives from the user through the varying chat interfaces. As an example, if the user enters the first half of their preferences in a discussion with an agent then switches to a Google search and Amazon browsing session to fine-tune the rest of their preferences, further conversations with the agent will lose out on the second half of information.


Based on these conditions, we can generalise to understand exactly where the gaps of OpenClaw are: because it has fragmented, incomplete context on each person's individual operations, it fails to effectively learn each person's **individual action policy** , and as a result is **unable to execute each task to its user's preference.** In short, it doesn't understand its user well enough to be able to understand what the user wants.


## Conclusion: living in a world of agents?


Through this exploration, we come to a more clear understanding of how agentic assistance should really look, at least in regards to how much autonomy we might allow our agents at present moment. Specifically, examples like[the crabby-rathbun post](https://crabby-rathbun.github.io/mjrathbun-website/blog/posts/2026-02-11-gatekeeping-in-open-source-the-scott-shambaugh-story.html) discussed above now very clearly define the upper limit of how much freedom we should let agents have. But perhaps the more interesting question, then, is given this upper limit, when would we feel more ready to readjust the limit and accept a greater level of autonomy? That is, at what point are we ready to hand off distinctly human understandings like intent and high-level executive decisions to a general agent, and in particular one that might have highly deleterious capabilities or vulnerabilities that might allow malicious activity from external actors?


At Dex, it is our belief that we need to condition on an understanding of an individual's action policy and preferences in order to ensure that reliability hits a threshold that makes humans comfortable to step out of the execution loop of these agents. Specifically, we believe that there exists some sort of local minimum within nonverifiable domains like knowledge work that ensures that agent error rate and (if there do exist errors) error propagation radius are low enough that there is justification beyond a reasonable doubt to truly remove the human from the loop; in that vein, it becomes a very clear first step to teach agents how to best learn these preferences and maximise the capabilities of capturing preferences, finding the right context, and personalising outputs. More generally, it is this process of building up the systems and training the agent to better proxy human knowledge work outputs that will hopefully bring about the "agentic promotion" from a simple intern to a proper domain-agnostic knowledge worker.
