---
schema_version: "1.0.0"
document_id: "2fcd78992c558517df3b6fa8529c936d5a84fc789c6b0653efac6c4d33133307"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/how-i-make-support-suck-less"
published_at: "2026-03-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:53:30.609843+00:00"
content_hash: "sha256:b2ec9014b7387a8dfb756810c1db8309f0ea89a65053acbd6cc7842f342f3215"
---

# How I make support suck less

I get an alert, open the ticket, decode what the customer actually means, then start a sacred pilgrimage through logs, traces, metrics and five different repos where the problem might be hiding.


If I'm lucky, I find the issue. If I'm unlucky, I find three unrelated errors, a stale trace, and a new reason to question the life decisions which have brought me to this point at 7pm on a Friday.


Then comes part two: writing a response that is clear, accurate, diplomatic, and does not accidentally promise a feature that will never exist. Fun.


Here's how I removed some of the drudgery, and how you can too!


## Before: Tab olympics and log archaeology


My old support loop looked like this:


Customer writes in -> Discord alert -> open Plain -> interpret message -> jump across observability tools -> grep repos -> walk code paths -> craft careful response.


It's slow and mentally expensive. Forget about the difficulty of solving the actual problem. Just the context switching between 12 different user interfaces is brutal. By the time I have enough signal to answer, I'm already cognitively cooked.


The worst tickets generally have a similar vibe: "this build was working and now it isn't, here's a link." Vague, serious, and with a hint of "and this is probably your trash system that is causing this, not me".


Then came AI…


## Now: One window, agent front-loaded everywhere


While I can't help the quality of the support messages coming in, I can *DRAMATICALLY* improve everything after that.


Now I run support mostly inside Cursor with agents and tool integrations.


I have a support agent that starts by reading my` support-memory.md` . That file is a runbook plus memory bank: where tools live, auth shortcuts, known failure patterns, investigation notes, and resolved cases I can reuse later. It also includes communication constraints, because I refuse to send customers AI slop that sounds like a motivational poster wrote it.


From there, I use the agent at every stage:


1. Pull and summarize the customer thread context.
2. Query logs and traces.
3. Correlate likely error path in code.
4. Draft a response I can edit and send.
5. Log the resolved case back into support memory.


All in one place. No more tab switching between innumerable UIs. All of it. Right there.


I also prefer CLI-based workflows when tools support them. Three reasons:


- I control the output shape (hello` jq` ) – this can help with token consumption and blowing away context.
- I avoid MCP tool limits.
- Way less overhead – no MCP server lifecycle, protocol layer, or connection issues.


And, generally I find agents play better with CLIs than MCPs. Agents find them, read` –-help` , and go. With MCPs I find agents need a little reminder and handholding that they're even there.


The Result: less "tab pinball," more throughput, less brain melt.


I still verify before replying, but diagnosis goes dramatically faster now. Not 10 percent faster. Multiples.


## A ticket that Looks Simple and Is Not


One of my favorite examples is the "slow builds even though autoscaling is enabled" class of ticket.


Customer symptom: builds are taking forever during bursts, everything *looks* configured correctly.


At first glance I assume something generic. Maybe cache weirdness. Maybe cloud capacity. Maybe they're underprovisioned.


The real issue can be weirder: no snapshot exists yet for the volume, so horizontal clones cannot spin up in parallel. In plain English, autoscaling is enabled in config, but the system still needs the right snapshot lifecycle state to actually fan out.


That is not obvious from one dashboard.


The debugging path usually requires stitching together state across systems:


- project autoscaling settings
- volume/snapshot presence
- build timing patterns
- machine lifecycle behavior


This is exactly where agents shine. They can gather all of that context quickly and surface the likely root cause without me manually hand-rolling twenty commands and five queries across disconnected tools.


Once we have the right diagnosis, the response changes from vague hand-waving to specific action: what happened, why it happened, and what to do next.


That's the difference between "support theater" and support that truly helps.


## AI is great at investigation, terrible at vibes


Investigation quality has gone up. Draft-writing quality is mixed.


Sometimes the model gives me useful language. Sometimes it gives me sloppy nonsense like:


> This incident is an opportunity for resilience acceleration.


Ummm, no.


A customer with a broken build does not need or want TED Talk English. They need a solution.


So I use AI drafts as scaffolding. If the tone is off, I just tweak it. The value is not "AI writes better than me." The value is "AI got me to the right technical answer faster, so I can spend my energy on clarity and trust."


It's easier for me to start from one than from zero. It's easier for me to edit something "bad" than nothing at all.


## Logging the win and moving on


After an issue is resolved, I append it to support memory: symptoms, root cause, diagnostics, evidence pattern, fix, and communication notes. That turns one solved case into future leverage. About 10% of the tickets I get now are answered almost immediately from previous cases (from months ago) that I had completely forgotten about.


Then I run` /daily-support-status` .


That command gathers the day's support work and generates a clean update with customer issues, infra events, escalations, and pending follow-ups. The thing that used to take me 20 minutes now takes about 30 seconds.


For a distributed team with very few meetings, that matters. A lot. Everyone gets visibility without the calendar bloat.


## Support still sucks, just (much) less


I wouldn't say I *enjoy* support now. But I don't dread it the same way.


The soul-crushing parts are mostly outsourced to software that is better at repetitive investigation than I am. The human part that remains is the part that should remain (for now): judgment, verification, and communication.


So yeah, support still sucks. It just sucks much, much less.


And, once that ticket is marked as "Done", I have my support bot log it and I take a sip of water to hydrate through the existential dread of how much longer I'll be needed to stitch this all together.


## FAQ


How do you help your AI agent remember solved problems?


I created a file, it's a runbook plus memory bank, that lives alongside my support agent. It includes where tools live, auth shortcuts, known failure patterns, investigation notes, and resolved cases I can reuse later. About 10% of tickets now get answered almost immediately from previous cases I had completely forgotten about.


Can you trust AI drafts to send directly to customers?


Not without editing. The investigation quality is good; the draft-writing quality is mixed. Sometimes it gives useful language, sometimes it gives something like "this incident is an opportunity for resilience acceleration." I use AI drafts as scaffolding, not finished product. The value isn't that AI writes better than me. It's that it got me to the right technical answer faster, so I can spend my energy on clarity and trust.


Does this workflow work with different observability stacks?


The agent approach works well anywhere you can query things from the CLI. I prefer CLI-based tool integrations specifically because I control the output shape (hello` jq` ), avoid MCP tool limits, and skip MCP server lifecycle overhead. If your observability tools have CLIs, agents generally find them, read` --help` , and figure it out. The` support-memory.md` file is what ties it all together across different environments and tool configs.


## Related posts


- [Collaborating with Claude on docs](https://depot.dev/blog/collaborate-with-claude-on-docs)
- [We hire people who care about their work](https://depot.dev/blog/we-hire-people-who-care-about-their-work)
- [Faster Claude Code agents in GitHub Actions](https://depot.dev/blog/claude-code-in-github-actions)


Andrew "Watts" Watkins


Engineering Manager at Depot
