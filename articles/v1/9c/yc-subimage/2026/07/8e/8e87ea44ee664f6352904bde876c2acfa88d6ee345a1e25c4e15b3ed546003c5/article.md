---
schema_version: "1.0.0"
document_id: "8e87ea44ee664f6352904bde876c2acfa88d6ee345a1e25c4e15b3ed546003c5"
company_key: "yc-subimage"
company: "SubImage"
source_id: "yc-subimage-news-import-96b948f674dd"
canonical_url: "https://www.subimage.io/blog/screwing-up-vuln-mgmt-at-scale/"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-26T02:26:23.667341+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:28547f8324eaf4360ab92feb6b236953ff5ca2d829f94f294fccdbf1e2904571"
---

# Screwing up vulnerability management at scale, and building a startup to fix it

Last week we shipped[Issues](https://docs.subimage.io/docs/issues) , a feature that turns vulnerabilities, misconfigurations, and attack paths into prioritized work that updates (and closes!) itself. This post is the story of how we built exactly what we wish we had at previous jobs.


## We got more tickets for you (jk)


Most CSPMs and security tools focus on finding problems. They scan your cloud, produce long lists of findings, attach severity scores, and leave the rest to you.


We do that too (across cloud, on-prem, identity, code, SaaS, and everything else we can pull into the graph), but finding problems is increasingly the easy part.


The hard part is deciding which ones to work on first, understanding what actually needs to change, getting that work to the right person, and knowing whether the fix really shipped. Issues is us handling that entire loop.


## Inspired by our time screwing up vuln management at scale


At previous jobs on security teams, we helped bootstrap huge vulnerability management programs:[I wrote about doing this at Lyft a few years back](https://medium.com/lyft-engineering/vulnerability-management-at-lyft-enforcing-the-cascade-part-1-234d1561b994) . I initially fell into the trap of treating tickets as the end of the story. My system produced hundreds of thousands of findings and blasted them at engineering teams with a “Jira ticket cannon”. Teams were overwhelmed by the volume and often confused about what they were actually being asked to change.


Meanwhile, it was really tedious to handle the messy lifecycle logic ourselves: who owns this, was it fixed within SLA, did the rebuild pick up the patched package, did anyone merge the PR, can we close the ticket, will it come back on the next scan, has someone accepted the risk, do we have a defensible reason for calling this urgent?


Or, say an auditor demands justification for why we've missed the SLA on fixing a critical vuln that affects 32-bit systems... and we do not own any 32-bit systems.


We underestimated the effort required to handle this and it burned out our team. Worse still, if left unchecked it could have burned the relationship between security and the rest of the company. Throwing tickets over the fence is a great way to get ignored as the boy who cried wolf. I eventually figured out what worked to get us out of that hole (and shared it as a talk at[BSides SF 2023](https://www.youtube.com/watch?v=F4EFHK21Et0) ).


When we started SubImage, we knew we wanted to build something that followed those principles.


## I used to abuse Jira as a database


We designed Issues to work differently. Instead of treating every scanner finding as its own problem, SubImage groups dozens of CVEs into single action items: e.g. update a base image from one SHA to another.


After all, nobody wants 150 tickets describing 150 symptoms of the same root cause. In my old job, due to time pressure I did exactly this and ended up treating Jira as a database. It *can* work, but is far from ergonomic.


## Not all CVEs treated equally


With Issues, we incorporate attack path context to prioritize work. When I was on a security team, this was the obvious next thing to build, but by then we had reached the limit of what an internal security program could justify investing in. I have since heard similar things from much larger enterprises.


The priority of an Issue comes from environment context rather than severity scores like CVSS alone. We factor in whether the workload is reachable from the internet, its exploitation probability, whether a fix exists, and what the blast radius looks like if the vulnerable thing gets popped.


A CVE on an isolated internal service should be treated differently from the same CVE sitting on an internet-facing attack path with access to customer data. Attackers use this lens to decide what to attack first, and it's useful for an overwhelmed security team to decide what to defend first.


I wrote about this[before](https://www.linkedin.com/feed/update/urn:li:activity:7475935632045801472/) , but in vuln management, there seem to be two schools of thought:


1. **Patch everything super fast** so that prioritization barely matters. Like this:


1. **Use environmental context** to focus on vulnerabilities that expose sensitive data or sit on meaningful attack paths.


I see the merit in both, but as vulnerability discovery accelerates, there is too much data to treat every finding as equally urgent. Yes, I am advocating for the midwit position in my own meme.


The U.S. Government seems to agree too:[CISA BOD 26-04](https://www.cisa.gov/news-events/directives/bod-26-04-prioritizing-security-updates-based-risk) will require federal agencies to prioritize updates based on risk (about time! I was sick of blindly chasing "make number go down" every month in my old role).


## Base image intelligence


The fixes we recommend are also specific. Container images are built in layers, and in most organizations the base image belongs to an infrastructure team while the application layers belong to a product team. Issues separates those concerns. It might tell the infrastructure team that bumping the base image from one SHA to another resolves one set of CVEs, while telling the application team that updating five pins in a lock file resolves another.


We can link directly to the repository and Dockerfile where the change needs to happen, because the graph connects the running workload back to the image, the image back to the repository, and the repository back to the people who work on it.


The instructions are precise enough that you can copy the prompt into Claude Code/GPT/your-agent-of-choice, let it make the change, and review the resulting PR (you can also use our own agent to make the change for you, but more on that in a future blog :)).


## The TODO list that closes itself


Now I get to talk about what I'm most proud of: our work items close themselves.


When the fixed image gets built and deployed, the vulnerabilities disappear. SubImage sees the change, and the Issue closes automatically. If the underlying problem comes back, so does the Issue.


To make a nerdy comparison, it's like you're playing a role-playing game, you slay the dragon, and the quest log updates itself.


Issues can send the work to Linear, Jira, Slack, or webhooks. Each ticket includes the signals that drove the priority, the attack path if there is one, the fix, the owner, and the history. When the Issue closes in SubImage, the external ticket closes too.


At prior roles, we did this by manually taking the outputs of one script and piping it to another to close the tickets. In the spirit of get-shit-done, sure. But man, this was painful.


Internally, we've been calling this a self-updating, over-engineered, actually-good TODO list for security teams, which undersells it but captures the spirit. The serious point is that the same graph we use to find attack paths already knows which repository to apply a fix to, who owns that code, and whether the new build actually reached production.


Starting with open source[Cartography](https://cartography.dev/) , I've spent over seven years (whew) working on graphs that show how attackers move through an environment. Issues uses that same graph to help security teams clean up after them.


## New Game+


Here's where I think this fits in a security team's stack. Fundamentally, teams need a system that organizes the work, prioritizes it using cross-provider context, tells you how to fix it (or even fixes it itself), routes it to the right place, and verifies that it actually got done.


Transparently, we're still figuring out how to best market this. Did we just teach our CSPM how to SOAR? (I groaned too). For better or worse, we built what we needed for ourselves and mostly ignored the established product categories, and we hope you find it useful too. I'm not big on the standard security tooling categories (and I suspect most security teams feel the same way).


Anyway, Issues is live in SubImage today. Our full[walkthrough on our YouTube channel](https://www.youtube.com/watch?v=bMAiyr1dPXM) shows the whole arc from attack path to closed Issue. The demo tells the whole story, but it's our staged environment. If you'd rather see how it manages yours,[reach out](https://cal.com/team/subimage/chat-with-subimage) and we'll set you up.
