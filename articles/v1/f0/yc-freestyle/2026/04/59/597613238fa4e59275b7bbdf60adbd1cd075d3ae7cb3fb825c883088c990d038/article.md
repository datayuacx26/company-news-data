---
schema_version: "1.0.0"
document_id: "597613238fa4e59275b7bbdf60adbd1cd075d3ae7cb3fb825c883088c990d038"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/opinion/youre-starving-your-agent"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-25T13:26:33.334256+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:37b0cf498bfe30445ae7d62315d242c032c9c27b35fd3f4e53f2ce91dc515ac1"
---

# You're starving your agent

All the way back in... *2025* ... we used to think giving agents more tool calls or MCP servers would deliver us useful agents. But when we all tried Claude Code we saw what a job-threatening agent could do. The only difference between Claude Code and other agents was it had access to your entire authenticated computer through` bash` . So suddenly instead of LLMs having the **ability** to perform tasks, it had the **agency** to do your job.


## We need more than Claude Code


Claude Code is great, but it only works well because you install a binary on your personal machine. We need agents to run in the cloud: we need asynchronous agents, agents on the web, swarms of coding agents, and agents my mom can use.


But creating agents that don't just use our personal computers for everything is hard. Claude Code is lucky that it has a full operating system and access to services that we've spent time authenticating to. Cloud agents usually don't have this luxury.


So the industry has converged on a spectrum. On one end, give the agent a handful of tool calls and nothing else. On the other end, hand it a full computer with an operating system, something like a Linux VM in the cloud. In between, Cloudflare thinks simply[executing arbitrary Typescript code](https://blog.cloudflare.com/dynamic-workers/) is the ideal sandbox, and[Vercel created Just Bash](https://vercel.com/changelog/introducing-bash-tool-for-filesystem-based-context-retrieval) — a simulated Linux environment with` grep` ,` ls` , and` cat` but no real VM underneath.


These middle-ground solutions make sense on paper. VMs are expensive. A V8 worker is cheap. Simulating bash commands like` grep` ​,` cat` ​,` ls` ​ without a real OS underneath is cheap. But the rest of the OS exists for a reason, without the rest of the OS,` just-bash` has the same ceiling a hypothetical SQL tool would. And yea, like SQL,` just-bash` is surprisingly capable for reading data, searching data, and writing data, and most tasks *seem* like they fit in that box.


## We are not smarter than the model


So the natural conclusion is: *give the agent enough computer to perform its predicted tasks.* This is naive. We are not clever enough to know what these SOTA models need from its harness to perform its tasks.


For example, I recently asked an agent I built to calculate the distance from one residential address to all points of interests in the area (think schools, grocery stores, etc.) and generate a flyer based on what it found. Since the agent had access to a linux sandbox, it installed` geopy` , used some government api, and generated the flyer as a pdf with python. When I told it the flyer it made was ugly, it installed` react-to-pdf` and generated a much nicer report using React.


I didn't predict the agent needed` geopy` . I didn't predict it needed that government API. And I definitely didn't predict that "this is ugly" would mean React was needed. To perform any job it's impossible to predict what is needed *beforehand* . If I had the job to prettify a PDF, I wouldn't have known about` react-to-pdf` either — I'd have googled around until I found it.


We already do this with people. The new hire who's only going to edit CSS still gets a $3000 MacBook Pro, because nobody actually knows *exactly* what they'll end up needing while they work there.


## There are tradeoffs


"But the more power we give to agents, the more damage they can do." Fair. We deal with this in the real world too. Interns can be quite dangerous so we restrict their power — they can't deploy, can't touch prod, can't spend money, can't make architectural decisions. And that's great for uptime, but frankly interns aren't very useful. A senior engineer is, in large part, someone with fewer restrictions.


Look, I get it. Giving a model more power is scary. But you can't engineer your way out of the tradeoff. Capable agents need capable harnesses.


So instead of designing harnesses around expected abilities, go in the other direction. Decide what restrictions you want, and give the agent as much agency as fits inside them.
