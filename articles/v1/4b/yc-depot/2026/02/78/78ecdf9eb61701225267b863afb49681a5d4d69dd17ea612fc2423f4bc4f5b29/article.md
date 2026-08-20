---
schema_version: "1.0.0"
document_id: "78ecdf9eb61701225267b863afb49681a5d4d69dd17ea612fc2423f4bc4f5b29"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/my-intuition-doesnt-work-anymore"
published_at: "2026-02-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:57:41.767482+00:00"
content_hash: "sha256:48f9fff82021210138c01172f5c64c4e7c6baf4bec09bc45791572d7e6af2060"
---

# My intuition doesn't work anymore

## John Henry's hammer


John Henry is an American folk legend. He's a steel-driving man. Driving rail spikes through plains and mountain tunnels alike. He raced a steam-powered drill to prove a human could outwork a machine. He won. Then he dropped dead, hammer still in his hand.


I keep thinking about what that must have felt like. Standing there, watching the machine drive spikes into the earth with speed, consistency, and precision. Wondering if the skills you've spent your life getting good at were about to stop mattering.


Because so many of my intuitions as an engineer—intuitions I've honed over a decade about readability, maintainability, and "doing it right"—feel wrong now. Part of how I judged myself as an engineer was based on the empathy I showed in my code. How much easier I made it for the next engineer to read and maintain. But I'm barreling into a world where that "next engineer" is just a bundle of statistics finding the next token.


I don't know what to do with that. Let me show you what I mean.


## The moment I noticed


I recently found myself staring at a 1500-line file in our codebase. It was a dumping ground. It had no structure, terrible naming conventions, and logic that meandered like me when I'm with my wife at Target: seemingly aimless, just hoping I could get the heck out of there.


My engineer instincts kicked in. I felt that familiar itch, the urge to refactor. I fired up an LLM and told it to clean the file up. It did a beautiful job. It organized the logic, extracted duplications into helper functions, and turned that dumping ground into something I could more easily parse with my eyeballs.


But then I stopped and looked at the pristine new file. It was better. It was cleaner. But did it actually matter?


We write clean code so that other humans can read it. It's empathy and insurance for the next human that has to touch it. Over the following months, most every change to that file was made by an agent. Not once did I need to open it and squint at the logic. The agent never cared that I'd organized it into neat little sections. It would have been just as happy chewing through the dumping ground. I was refactoring for me, and I used "empathy for my fellow engineers" as justification.


## 40 years of empathy for the human brain


Look at these two loops.


Example A:


```text
MOV CX, 10
L1:
ADD AX, 1
DEC CX
JNZ L1
```


Example B:


```text
for   i   in   range  (  10  ):
count   +=   1
```


Which one is easier to understand? For 99.999% of us it's B. If you picked A, congratulations, I award you one thousand nerd points.


Why do we prefer B? It's not because Python is faster. It's way slower. We prefer it because it's faster for our brains to parse what the heck is going on.


For the last 40 years, many of the advances in software have been about empathy for the human brain. We built abstractions, invented high-level languages, and wrote books like "Clean Code" because human cognitive capacity is limited. Descriptive variable names and small functions are useful because, unlike computers, I forget what I read a few lines ago.


Code became optimized for the reader, not the compiler.


But what happens when the reader is no longer human?


## Code as artifact


Here's the thing I can't unthink: the code is not the point. It's an instructional artifact to produce something useful for a user. No user downloads an app and says "Man, I hope they preferred composition over inheritance."


I tried to talk myself out of this. "Sure, the AI can write it, but can you fix it when it breaks at 3 AM?"


Being on-call used to mean a customer messaged me with some obscure error, and I'd spend the next thirty minutes manually traversing Cloudwatch and Axiom logs. Hoping to find a needle in a haystack. Now, I copy-paste the error, hit the Cloudwatch and Axiom MCPs, and boom: 9 out of 10 times I find the issue. What used to be a log safari is now ninety seconds and a few tool calls. Multiply that across a team, across a week, and I can't really tell myself that clean code is what's saving time.


Then I told myself it was about structure. Fine, variable names don't matter, but structure matters. And maybe it does. But when I'm honest with myself, I can't point to evidence that an LLM writes worse code in a messy repo. And that bothers me. I've watched agents make correct changes in files I'd be afraid to touch. The machine doesn't need the guardrails that I need.


The last thing I clung to was depth. AI sucks at fixing deep architectural rot that rapidly fills the context window. And that's true.


For how much longer?


## Where the tunnel needs to go


It was never about the hammer though. It's knowing where the tunnel needed to go. The judgement, the systems thinking, the ability to communicate and tease the *real* product requirements. That's the real work. It always was.


And this is where John Henry's story diverges from ours. Or so I like to tell myself. He was pure strength and execution. We're the ones who decide where the tunnel goes and why it's being built at all. The machine could match his hammer but it can't match our judgement. Right?


Because what really worries me is that the same pattern that is making clean code less relevant is climbing the ladder we're all standing on. Each rung AI climbs, I tell myself *this* is the one that requires human judgement. And each time, the horizon gets a little closer.


So, what happens when the machine catches up? When it knows where the tunnel should go?


I don't have an answer. But I think the honest version of this essay isn't "relax, the important stuff is safe." It's that the boundary between where humans excel and machines excel is blurrier than I'd like to admit. And it's still moving.


And you know what? I still pick up the hammer some days. I miss how it feels in my hands.


## Related posts


- [Using AI as my engineering copilot (not autopilot)](https://depot.dev/blog/using-ai-as-my-engineering-copilot-not-autopilot)
- [The bottleneck has shifted from writing code to integrating it](https://depot.dev/blog/the-bottleneck-has-shifted)
- [We hire people who care about their work](https://depot.dev/blog/we-hire-people-who-care-about-their-work)


Andrew "Watts" Watkins


Engineering Manager at Depot
