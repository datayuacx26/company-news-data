---
schema_version: "1.0.0"
document_id: "76f984a2bb7555ed63309541e14fac21294fb5dbf24b6cc53290297be975b88a"
company_key: "yc-turing-college"
company: "Turing College"
source_id: "yc-turing-college-news-import-c9a4b7bb10b2"
canonical_url: "https://www.turingcollege.com/blog/build-ai-coding-agent-ruby"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:08.178356+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:e2d4025107b9b47c40cee782d069383a5078e8eed03acc0b7c84a989a0ae6c56"
---

# Can You Build an AI Coding Agent in Ruby? Kai Did It in 10 Days

### Key takeaways


- [Kward](https://kaiwood.github.io/kward/index.html) is an open-source AI coding agent written entirely in Ruby, installable with` gem install kward` .
- Its creator, Kai Wood, has 20 years in Ruby and no formal computer science degree, and he built Kward after a year away from programming.
- It runs on your own OpenAI, Anthropic, or OpenRouter account, so you pay your provider directly with no markup added. Kward now writes its own new features and has replaced the paid coding agents Kai used before.
- Kai reached a releasable version in around 10 days, starting with no spec and letting the working agent build the rest.


He shipped it at three in the afternoon, sat down on his couch, and opened a podcast he follows. Ten minutes into the episode, one of the hosts insisted that nobody would ever use Ruby for agent work. Kai smiled. He had released a Ruby coding agent that same afternoon, and he had built part of it by pointing his agent at the very TypeScript project the host was talking about.


Kai Wood spent 20 years as a self-taught engineer with no formal computer science degree. His most widely used release before Kward was a small Ruby plugin for Visual Studio Code that has passed 500,000 installs. He quit programming for a year, burned out by configuration layers and constant tool changes that he felt added work without adding capability. When he came back, he came back through[Turing College's AI Engineering program](https://www.turingcollege.com/ai-engineering) , and the first thing he wanted to do was build an agent better than the ones he was watching everyone else use. He chose Ruby on purpose, against the advice of the entire field.


## Can you build an AI coding agent in Ruby?


Yes. Ruby runs a full coding agent. The work comes down to calling a model's API and running commands on your machine, and Ruby handles both cleanly. Kward, built in Ruby and installable with` gem install kward` , is proof that the language was never the obstacle.


The belief that Ruby cannot handle this work is widespread among engineers. Kai ran into it directly. As he put it, "the programmer world thinks Ruby is not capable with these kinds of things." The assumption traces back to the years after Ruby on Rails lost popularity, when the language stopped gaining new libraries at the pace Python did and got a reputation as an aging web framework.


An agent's core is small. It talks to a model over an API, parses a structured response, and acts on it. Kai picked Ruby because he had written it for 20 years, because its syntax reads close to plain English, and because the agent he wanted to build is extendable, and Ruby has always been good at the kind of clean extension points that plugins need. "It reads like English," he said. "Even non-programmers read it."


## Why does everyone build agents in Python or TypeScript?


Python and TypeScript dominate agent building because that is where the early tools and libraries landed, not because they are better suited to the task. Python inherited the position from data science and machine learning. TypeScript inherited it from the web and from the first wave of open-source agent harnesses. The gravity is real, but it is habit, not necessity.


Python won machine learning a decade ago, so when large language models arrived, the model providers shipped their first SDKs and example code in Python. Engineers followed the documentation. TypeScript became the other common choice, because the people building developer tools and terminal interfaces already worked in it.


The agent Kai first built on top of is a good example. Pi, an open-source coding agent by Mario Zechner, the developer behind the libGDX game framework, is written in TypeScript and built as a minimal harness with a tiny set of core tools that you extend yourself. Kai admired the design and used it. Then he reached its limits. He wanted features the harness underneath did not support, and in open source, waiting on a pull request to be reviewed and merged can take weeks. As he described that moment: "you can only build so many things on top of something before it gets stupid because the little engine underneath doesn't support anything."


So he decided to build his own engine. Turing College normally asks learners to build their Sprint 3 agent in Python or TypeScript, the two languages most commonly used by the industry. Kai asked to use Ruby instead and made his case. The reviewer assigned to him,[Oleg](https://www.linkedin.com/in/ianeiko/) , turned out to have 20 years of Ruby behind him as well, and took the project on. The exception got approved because his case was strong.


## What do you actually need to build a coding agent?


A coding agent needs six working parts: a connection to an LLM, a tool-calling loop, file read and write operations with safety guardrails, the ability to run shell commands, web and code search, and session state so it remembers the conversation. Everything else is convenience built on top of those six.


Strip away the branding and every coding agent on the market reduces to the same anatomy. Here is what each part does, with Kward as one example of how to implement it:


- **An LLM connection.** The agent sends prompts to a model and reads structured responses back. Kward supports OpenAI, Anthropic, and OpenRouter, so you bring your own provider and your own subscription rather than paying a markup to a wrapper.
- **A tool-calling loop.** The model proposes an action, the agent runs it, and the result goes back into the next prompt. This loop is the engine. It is also the part people overcomplicate.
- **File operations with guardrails.** The agent reads, writes, and edits files in your project. Kward uses read-before-write guardrails so the model has to see a file before it can overwrite it, which prevents careless deletions.
- **Shell command execution.** A coding agent that cannot run your tests or your build is a chatbot. Kward runs local shell commands from the workspace.
- **Web and code search.** The agent looks things up and inspects repositories instead of guessing from out-of-date training data.
- **Session state and memory.** You need to save, resume, and revisit conversations. Kward saves, resumes, clones, compacts, and exports sessions.


If you can combine those six in a language you know, you have an agent. The language you choose matters far less than understanding the loop.


## How long does it take to build one?


Kai reached a releasable version in around 10 days of work. He did it by starting without a specification, building the agent to solve a problem he had himself, and then using the agent to build itself. The hardest part was the 12 hours he spent on a feature he ended up deleting.


Kai did not write a spec or list the problems first. "I just start," he said. "It sounds so stupid. I have this thing in my mind and then I start, like a little boy that plays with Legos, and then I notice, oh, I need a window here." Engineers who wait to know every obstacle before they begin tend to never begin. He built, ran into problems, walked away from the keyboard, and came back with the fix.


The detail that proves it works: Kward built Kward. Once the agent could edit files and run commands, Kai used it to write its own next features. An agent that is good enough to extend itself is an agent that works, and it has been his only coding tool since, replacing the paid agents he used before.


Not every hour was productive, and he is honest about that. One evening he spent 12 hours building a tabbed terminal interface, the kind of feature that sounds simple and is not. He got it working and beautiful, then remembered why he had abandoned that approach years earlier: the multiplexers underneath get slow, and the price of fixing the speed is losing the ability to select text with the mouse. He threw away all 12 hours. That willingness to delete good work that leads nowhere is most of what separates a shipped project from a stalled one.


## Ruby vs Python vs TypeScript for building an agent


For building an AI coding agent, Python offers the most libraries and example code, TypeScript has the strongest terminal and tooling support, and Ruby gives you clean, readable extension points with less competition. The best choice is the language you already know well, because the agent loop is the same in all three.


The honest comparison is about tradeoffs, not winners. Here is how the three compare for someone building a coding agent today.


Factor Ruby Python TypeScript


LLM library support Thinner. Official SDKs are less common, so you call APIs more directly. Strongest. Most provider SDKs and example code ship here first. Strong. Most open-source agent harnesses live here.


Syntax and extensibility Reads close to English. Clean plugin and extension points. Readable. Huge surface area of patterns. Verbose but explicit. Types help on larger codebases.


Ecosystem maturity for agents Early. Few existing agents to copy, so more original work. Mature. Reference implementations everywhere. Mature. Strong terminal and tooling libraries.


Best suited to An engineer who already knows Ruby and wants a standout project. An engineer optimizing for available libraries and examples. An engineer building terminal-heavy or web-integrated tooling.


Thinner library support has an upside and a downside. Kai had fewer shortcuts, so he wrote more of the core himself, which is exactly why his project stood out in review rather than looking like one more near-identical Python submission.


## How to build your own AI agent


Pick a language you already know well, connect your own LLM provider, build the agent by using it on real work, and release early before it feels finished. The fastest path to a working agent is to make it useful to yourself on day one, then let your daily frustrations tell you what to build next.


If you want to build one yourself, four decisions will save you the most time:


- **Use a language you know well.** Kai chose Ruby because 20 years of it meant he could move at full speed and direct the model precisely. Fighting an unfamiliar language and an unfamiliar problem at once is the slow path.
- **Bring your own LLM.** Connect directly to a provider you already pay for rather than routing through a tool that marks up the tokens. You keep control of cost and model choice.
- **Build it by using it.** The moment your agent can edit a file and run a command, switch to building its next feature with the agent itself. Real use reveals the gaps faster than any plan.
- **Ship before it is polished.** Kai released while the documentation still embarrassed him. A released project that works beats a perfect one that never leaves your machine.


Kward is open source. You can read the code, install it from RubyGems with` gem install kward` , and see the documentation at the project site. It started as one learner's argument that a fading language still had something to prove, and it is now the tool he writes code with every day.


## FAQ


### Is Ruby good for AI agents?


Ruby is well suited to building AI coding agents. The work depends on calling an LLM API, running shell commands, and editing files, all of which Ruby does cleanly. Its readable syntax and strong extension points make it a good fit for agents designed to be extended with plugins. Kward is a working example.


### What do you need to build an AI coding agent?


You need six components: a connection to a large language model, a loop that runs the tools the model requests, file read and write operations with safety guardrails, shell command execution, web and code search, and session memory. Build those in any capable language and you have a functioning agent.


### Can you build an AI agent without Python?


Yes. Python is the most common choice because most LLM SDKs and examples ship in it first, but no part of an agent requires Python. Kward is built entirely in Ruby. Any language that can call an HTTP API and run local commands can run a coding agent.


### How long does it take to build a working coding agent?


A single experienced engineer can build a releasable coding agent in roughly one to two weeks of focused work. Kai Wood reached a usable, releasable version of Kward in around 10 days by starting without a spec and using the agent to build its own features.
