---
schema_version: "1.0.0"
document_id: "3be7f67cec9dd6463c180da347aad384b4bccc20b53e23e87d5abc936a37a8bf"
company_key: "yc-fume"
company: "Fume"
source_id: "yc-fume-rss-5fd00bac91aa"
canonical_url: "https://www.fumedev.com/blog/productive-idiots"
published_at: "2025-08-12T12:00:00+00:00"
first_seen_at: "2026-07-20T23:23:55.513253+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:3c2e647b3617197def550cead348a9b840d1c70d8d395b75b58f378a9db34ba8"
---

# Productive Idiots - Becoming Illiterate Thanks To Vibe Coding

A recent conversation made me realize a big, long-term threat of “vibe coding”: becoming illiterate toward the code I wrote.


I recently took on a ticket that involved building two-way streaming for a remote Chrome session using[CDP](https://chromedevtools.github.io/devtools-protocol/) . The final solution turned out to be a little more nuanced than I initially anticipated, but it was nothing I couldn’t handle with some help from[Cursor](https://cursor.com/) . A few days later, someone else asked me a question about its implementation, and I confidently answered. After all, I was the one who built it — or so I thought. Long story short, somewhere in the long debugging sessions, Cursor’s agent changed the implementation significantly, and I blindly accepted the changes, seeing that the bug was resolved. Yes, this was mostly my mistake for not reviewing every change an LLM suggested, but the nature of vibe coding sets up fallible engineers like me to repeat this more often in the future.


## Understanding A Codebase → Old School


In every codebase I was onboarded to, there was always a single approach that reliably made me truly understand it: taking on a ticket that I had no idea how to implement. Contrary to intuition, asking questions or reading docs never helped me and I have a good guess why that is:


I believe every seasoned engineer in a team has a high-level mental model for their product that is a culmination of (probably multiple) codebase(s), the business itself, past technical decisions that were made, code reviews received, past bugs/problems, etc. Documentation or answering a question involves encoding a part of this mental model into natural language. I believe this process is inherently very lossy. Even if there is a way to minimize the loss, it requires a great deal of skill that an engineer is unlikely to have (no one becomes an engineer because they are great communicators 😀)


Additionally, decoding information from natural language is another challenge that is at least as hard as encoding it well. My initial mental model built by reading docs rarely matches the reality I get hit with when I try to actually implement what I’m doing.


The only great solution is brute-forcing your way to build your unique mental model, which means a lot of trial and error. All the command-clicks you do following a call graph are actually a search algorithm you use to collect data points for your model. Once you can comfortably implement anything you want in a codebase, you know you’ve done enough trial and error to have a mental model that is close enough to reality.


Here is the killer: vibe coding blurs the line entirely.


You can implement much more complex changes than your understanding of the codebase. This gives you overconfidence in your abilities ([Dunning-Kruger effect IRL](https://en.wikipedia.org/wiki/Dunning%E2%80%93Kruger_effect) ) and therefore stops you from going through all that painful trial and error, command-clicking, print debugging, etc.


## Productive Idiot


Vibe coding mints a new type of engineer I like to call the “productive idiot.” These are the engineers whose implementation complexity exceeds their depth of understanding. Sadly, I consider myself a member of this group now. Here is a list of questions that you can ask yourself to find out if you are one too:


- Did I type in the code editor or Cursor Composer more today?
- Did I read the diff before accepting the changes?
- Would I be able to complete the same task in the pre-ChatGPT era?


## The Risk


I believe the generation of productive idiots is a more deep-rooted risk than all those bugs and bad practices people say LLMs promote. People who start coding with code-assist tools like Cursor run the risk of never building a good mental model for themselves. In the long term, this might even mean the extinction of the old-school programmer. This might seem like an odd and unlikely scenario, but I bet the programmers in the 60s thought the same about COBOL developers. Yet, the[extinction of senior COBOL devs](https://www.reddit.com/r/programming/comments/1divrxf/the_real_cobol_programmer_shortage_is_510_years/) is a real risk to the banking industry (most financial mainframe computers still use COBOL).


## The Solution (if there is one)


Well, I don’t think there is an easy way. Even if we continue using these tools (which I most certainly will), it’s probably a good idea to read and understand the code diff before clicking on ‘Accept All’.


Additional layers of checks before deployment also help:


- AI code reviews are a good start (IMO everyone should check out Cursor Bugbot)
- Tests are becoming more important (cough cough check out Fume if you want browser tests cough cough)
- Detox ticket: I sometimes try to complete a full ticket in the old-school way on the weekends. This helps me stay grounded with my work and the codebase in general.


## Disclaimers


- I do not believe this is the future we are going towards. I believe smart engineers will find the balance between vibes and hands-on programming. This opinion blog was a result of the discourse I mentioned. The amount of ignorance I had toward the code I shipped gave me chills.
- This might happen and turn out to be a good thing. Very few of us understand the low-level bits and pieces of the software we build on, yet we are doing fine.
- This is a GREAT opportunity for building prototypes fast. I argue the problem arises when you deploy to production and get to a certain scale.
