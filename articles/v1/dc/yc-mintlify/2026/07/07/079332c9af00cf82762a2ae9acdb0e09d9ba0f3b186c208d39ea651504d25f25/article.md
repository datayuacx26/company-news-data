---
schema_version: "1.0.0"
document_id: "079332c9af00cf82762a2ae9acdb0e09d9ba0f3b186c208d39ea651504d25f25"
company_key: "yc-mintlify"
company: "Mintlify"
source_id: "yc-mintlify-news-import-4dae4ee3e362"
canonical_url: "https://www.mintlify.com/blog/llms-txt-agent-benchmark"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-22T04:29:43.596583+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:30af5c695405b1d0dda7adb35002441740d221ebde205ca663fe7a7e8bc8c02c"
---

# Docs URL Benchmark: Markdown & llms.txt > HTML

Docs used to be written for humans. Now, the most demanding readers you have are agents, and ours were failing. They'd fetch a page, guess at the links, and 404 on URLs that weren't real.


Back in November of last year, Paul, the CEO of[Browserbase](https://www.browserbase.com/) , told us that Claude Code kept 404ing on Mintlify-hosted docs, and that he could reproduce it every time. The worst part was that it was happening on our own docs too.


The cause was almost comical. To make our pages readable for agents, we served a stripped-down markdown, but in doing so we stripped out the navigation too. The agents were left exploring blind, with no map of what pages existed, so they guessed, and fetched paths that were never real.


The fix was to give them that map. Every Mintlify site already publishes an[llms.txt](https://www.mintlify.com/blog/what-is-llms-txt) , a single file that lists every page with its title and description that basically serves as a table of contents built for agents. So at the bottom of every markdown page, we added a link to it, and the 404s stopped.


But it raised a question: was that actually the best way to let an agent navigate a site? Did we want to link the llms.txt and let the agent fetch it? Should we inline the whole thing into every page? Or could we skip the map entirely and just serve rich HTML?


So we stopped arguing and benchmarked it.


## How we ran the benchmark


The setup was simple. We took four ways to serve the same docs: HTML, plain markdown, markdown with a link to llms.txt, and markdown with the entire llms.txt inlined. Each was gated behind a local proxy so every fetch an agent made could be observed. Then we pointed Claude Code and Codex, running on Sonnet 5 and GPT-5.5, at 20 Mintlify docs sites, 5 questions per site, run 3 times across the 4 formats. All together, 2,400 runs in total.


## What the benchmark found


On raw HTML, the agents thrashed. They kept reaching for` .md` and` /llms.txt` versions of pages, and 404'd every time the site had nothing to hand back. Once we added a single link to the llms.txt, that mostly disappeared: about 90% fewer dead URLs, far fewer wasted fetches, and a big cut in token usage.


And it cost nothing more to get there. Accuracy stayed in the mid-to-high 90s across every format, because agents are good at eventually finding the right page no matter how you serve it. The map didn't make them more right, it just got them there faster and cheaper. The win was efficiency, not correctness.


## It's the map that matters


The most surprising find was that markdown itself wasn't the fix, the map was. Plain markdown was arguably the worst of the four formats. Without a map to point to, agents started building` .md` paths on their own and guessed more wrong URLs than they had on HTML. Hand them the llms.txt and that stops.


And between the two ways of handing over that map, linking won. Inlining the whole file into every page killed the same 404s but cost more tokens every time, so the link got the same benefit at a fraction of the cost.


And this wasn't a one off. We replicated it across three more models, Opus 4.8, Fable 5, and GPT-5.6, over the same 20 sites, and 404s fell to near zero on every one: Opus 4.8 went from 2.11 to 0.08 per task, Fable 5 from 1.13 to 0.00, and GPT-5.6 from 6.79 to 0.02. The drop held on 19 or 20 out of 20 sites for every model tested.


## What this means for your docs


The takeaway is simple: if you serve content to agents, give them a map and link to it. One llms.txt pointer does the work, and it pays off with every agent we tried.


We've[open-sourced the whole benchmark](https://github.com/mintlify/url-discovery-bench) , so you don't have to take our word for it. You can regenerate every number from the committed data, or point it at your own docs and watch the same gap show up in minutes.


Docs used to be judged by whether a person could read them. That's still true, but they're increasingly judged by whether an agent can navigate them too. Agents are quickly becoming the most important readers your docs have, and every Mintlify site already does the best possible thing for them, automatically.
