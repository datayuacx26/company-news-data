---
schema_version: "1.0.0"
document_id: "905fe524e1028d7ab828ce5a440ac72452554910c585cb950ae05e069baf98c9"
company_key: "yc-metorial"
company: "Metorial"
source_id: "yc-metorial-news-import-9c457a3d81d4"
canonical_url: "https://metorial.com/blog/vibe-coding-first-time"
published_at: "2025-10-24T02:52:00+00:00"
first_seen_at: "2026-07-22T04:11:44.298499+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:67d291f0014242285d099dfe18094a139edbcf4d325a1efc0d84efc0c4477f42"
---

# My First Time Vibe Coding: A Skeptic's Journey

It’s October 2025 and it’s my first time vibe coding. I have mixed feelings.


Frankly, when vibe coding tools came out I was very skeptical. Don't get me wrong, I like using LLMs and I've gotten so used to GitHub Copilot just completing my thoughts in VSCode and Vim. But generating entire code bases with LLMs? I'm not so sure about that.


Then from time to time, I looked at code from Lovable, Repl.it, and Cursor that my friends had sent me. I wasn't impressed. It was full of redundancies. It wasn't elegant. It was code that's fine for a beginner, someone making their first strides at building products, not someone I would let touch an actual production code base.


Also for context, here at Metorial we build infrastructure that's capable of running tens of thousands of MCP servers concurrently. We basically invented hibernation for MCP. Everything is completely custom and there is actual production infrastructure that our customers rely on behind it. Not something to play around with. But the other day I had this idea for a better MCP testing playground, called[Starbase](https://github.com/metorial/starbase) . Starbase is an experiment. A toy. Starbase can't take our customers down. Starbase can't expose our customer's data.


People keep telling me that they vibecode so much now and I'm always like "yeah, nah!". This time, I thought I should give it a shot and Starbase is the right opportunity to do so. Not much at stake, but a lot to learn.


This isn't a benchmark or anything even close to scientific. It's just my learnings as an anti-vibecoder vibecoding for the first time.


### Setting the Scene


I used Claude Code. I felt like that's probably the best option for someone like me. I could still use the terminal, don't have to learn too much new, and don't have to switch to a new text editor.


I wanted to give it a fair chance so I actually put some effort into writing the first prompt. Clearly explaining what I want it to build. Giving it technical directions. Very precisely explaining the UI and the functionality.


I used a stack that it would probably have a lot of data for: Next.js + Prisma + styled components. I don't want to hear anything about Tailwind! I write CSS so should Claude Code.


I tried to go step by step. I had it build a very basic version initially. Something that barely functions and lacks the features I really wanted. I then iterated feature by feature asking it to add them.


And you know what? The first thing it gave me was impressive. It had a clear vision of the UI and except for a few minor bugs, it delivered. It looked cool. It worked well enough for a prototype. I was seriously reconsidering my stance on vibecoding.


Then came the advanced features.[Starbase](https://github.com/metorial/starbase) is an MCP playground. Nothing complicated but also more than a todo list. MCP connections aren't super straightforward and I wanted to do the MCP connections client side while of course running the models on the backend (to not leak our API keys). A basic auth system. Some simple tables to store previous server connections. Nothing crazy.


## The back and forth


And then began the back and forth. It made mistakes, lots of them. Every time I got it to fix one, another one would pop up. It was tedious. Some mistakes were simple and it fixed them well. Some were difficult, like the MCP connection handling, and it really struggled with them.


The MCP client-side connection logic was where things got messy. Claude Code would generate code that might look right at first glance, but it kept mixing up where the connection should be established versus where it should be used. I'd point out the issue, it would fix that specific instance, and then introduce the same pattern in a different file.


What frustrated me most was the lack of consistency in error handling. Some functions would throw errors, others would return null, and some would just silently fail. When I asked it to standardize error handling across the codebase, it would update the files I mentioned but leave the rest untouched. It couldn't maintain a holistic view of the project’s architecture (that shouldn’t be a context problem to context is more than big enough to fit the entire code base into it).


## The code quality


The code is far from perfect. It's often very inconsistent. It doesn't adhere to the standards you've given it (or even try to set it’s own standards). This reassured my initial feelings about vibecoding. The code is like someone trying to learn and play around. Not someone who has years of experience and does stuff the right way. It’s not a senior engineer and not even a junior.


The styling was all over the place. Despite my explicit instruction to use styled components consistently, I found inline styles sprinkled throughout and some components using Tailwind. When I called this out, it would fix the issue and then after a while generate some new code with the same issue.


The code structure showed no understanding of separation of concerns. No modularity. No reuse. Business logic lived in components. API calls were duplicated across files. Utility functions that should have been extracted were copied and pasted. When I asked it to refactor, it would clean up some stuff but underlying problems remained.


The comments were insane. Most good code is self documenting. Comments should add extra context; the code explains what (if possible), the comments explain why. Claude commented everything in grave detail. Things that are super obvious had a comment explaining what they do.


## My take on vibe coding


I'm a bit torn. On the one hand I am impressed. What it built works. And I was able to do other stuff while babysitting the model here and there. At the same time, after having read the code, I wouldn't let Claude Code even remotely close to Metorial's primary codebase. There's too much at stake. But that highlights an interesting point. There is code where a lot is at stake. Hell, some people are out there writing code that, if it malfunctions, could kill people (not including the people who intentionally write code for killing people). On the other hand, there are codebases where bugs just don't matter that much. Internal tools, or experiments, like[Starbase](https://github.com/metorial/starbase) .


In addition to that is the fact that I love writing code. And I know many software engineers do too. There's a reason we sometimes talk about recreational programming. There's a reason why many devs work on OSS and side-projects on the weekend. Vibecoding takes that away. Instead of being the maker, you are the supervisor, which isn't fun.


So my conclusion is that I've learned a lot and become a bit more open about vibecoding. While also having my initial fears and biases against it reinforced. It's an interesting tool that, if used correctly, can help many devs (and of course non-technical people) become more productive. But it's no replacement for humans and it takes away the fun. The real value proposition became clear to me afterwards: vibe coding is perfect for the stuff you don't want to write anyway. Boilerplate. Proof of concepts. Internal tools that five people will use. But for anything that matters, anything where code quality affects maintainability, reliability, or user trust, you still need human developers who care about the craft.


Would I use it again? Absolutely. For the right project. But for now, keep it far away from anything that actually matters.


*Building something with AI? Check out *[Metorial](https://metorial.com/)* . We make it dead simple to integrate 600+ tools with your agents. Open source, great SDKs, and actually maintained by people who understand infrastructure.*
