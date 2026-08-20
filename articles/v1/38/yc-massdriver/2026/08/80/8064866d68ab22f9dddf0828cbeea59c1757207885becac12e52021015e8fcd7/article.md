---
schema_version: "1.0.0"
document_id: "8064866d68ab22f9dddf0828cbeea59c1757207885becac12e52021015e8fcd7"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/the-citizen-developer"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T22:01:04.686635+00:00"
fetched_at: "2026-08-19T22:01:07.216716+00:00"
content_hash: "sha256:9d65e1a6059aeb5d4e20e7471519efe74d2392149446f893a37520ff958e5691"
---

# The Citizen Developer

You may not have heard the term citizen developer yet, but they exist, and they've been here for a long time.


In the nineties, it was the person in accounts payable who bought accounting software off a sales call and, whoops, now there's a Dell server plugged into an ethernet port. Or the early 2000s when the marketing team Dreamweaver'd up some site you have to figure out how to get on IIS 5. Today, it's the person on your sales team downloading Claude Code and spinning up an app that stores PII. Same person, different means.


You might be thinking, this sounds like shadow IT. BOO! It is. Shivers down the spine of CISOs and IT folks everywhere.


When it's discovered, teams tend to treat it as the security incident that it is. Maybe even a slap on the wrist for the person who was... simply using their autonomy to move the business forward?


It is a security incident, sure. But it's also a signal that people have shit to do and they don't have time to wait on the engineering organization. And it's not slowing down. It didn't start with AI either. Gartner found in 2021, before ChatGPT shipped, that[41% of employees](https://www.gartner.com/en/newsroom/press-releases/2022-03-13-gartner-survey-finds-rise-in-business-technologists-is-driving-funding-for-tech-purchases-outside-of-it) were creating technology or analytics capabilities from outside the IT department. Two in five, back when it was still hard. And oh boy, it's sooooo easy now. Half your company is already routing around you. They just aren't telling you.


## Who are these people!?


The SDR who asked for a lead management tool, watched it die in backlog grooming, and built a sloppy version himself in an afternoon. His company employs plenty of engineers. None of them work for him, and his ticket was never going to beat a roadmap item in a prioritization fight.


The operations manager at a regional insurance carrier that has never employed a software team in its history. Twenty years of institutional knowledge lives in her spreadsheets, and for the first time there's a way to turn it into something more without hiring anyone. She's not violating the IT policy on this. *There isn't one.* Nobody ever imagined she'd need it.


And us. The frontend developer dropped into the backend. The backend developer poking at Terraform. I'm not a Node expert, so when I'm in Node, *I'm* a citizen. My co-founder is a citizen in Elixir. Step outside your lane and you're one of them.


When I first started learning about this role, I thought it was important to disambiguate a "citizen" developer from a "software" developer, until I realized we're all the citizen when we're outside our comfort zone, trying to do something important, something that pushes the business forward, just outside our means. It happens all the time. Nobody in this industry is licensed, and everyone's expertise covers a sliver of an enormous surface. That describes the average software developer more often than we'd like to admit.


The more I've sat with this role from the DevOps/Platform point of view, the more the two look the same from that seat too. Ops has systems that people need to self-serve, and the governance around them (security, compliance, cost) doesn't care who's asking. From where ops sits, the citizen and the professional developer are the same. They are both introducing changes to a production system that we must keep stable. *Kinda sounds like we're doing a devop.*


## When did you become a developer?


How much software must you write or understand before you're granted the title? There's no license to pass. Am I a software developer because I applied for a job and got it? Because I finished one book? One course? A compsci degree? A bootcamp grad with a badge is a developer on day one, and a fifteen-year hobbyist without a job title isn't. That tells you what the title actually measures, *nothing* . You become a software developer the day a company agrees to call you one.


Maybe you're a seasoned developer. You wrote every line by hand, developer. You wrote them with a language server, still a developer. You wrote them with autocomplete, still a developer. Copilot wrote half, sure, still counts. Claude wrote all of it, but you specified every behavior and caught its bugs. Where in that sequence did you stop being a developer? Nobody can answer, because the question was never about the *code* .


So what do the SDR and the senior developer actually share? They're both looking at some value the business needs, deciding it should be automatable and interactable through some interface, and willing it into existence. That's the whole job. One of them has more practice. There's a gradient between the expert and the citizen. We drew the boundary because it paid better.


## A means to an end


Nobody starts a business thinking, I can't wait to hire forty people who really like to argue about code formatting and blow up my OpEx on cloud spend and send nerds to Vegas to get free shirts. They start a business because they found a problem they want to solve. Software is a means to that end. Developers are a means to that end. This has always been true, and our industry has spent twenty years politely not saying it out loud.


Look at how software actually gets made inside a company. The business has a problem. The problem waits on a PM. The PM waits on engineers. The engineers wait on ops. There are roadblocks all the way down the layer cake, and every layer is understaffed and behind. Three years ago, the person at the top of that stack just waited. Today they don't wait. They open up Claude or whatever and think, "it can't be this hard." And lo and behold, it's not. For them, anyway. For you, later, when it lands in your lap? Tough shit.


And the business is fine with that. Put yourself in the CEO's chair. Two people can produce the thing you need. One can do it right now, and it might be sloppy, and sloppy can be fixed. The other wants to talk about craft and has a six month backlog. That decision takes about four seconds. They're okay with slop.


If that offends you, I understand. It offended a lot of people when it was outsourcing, and when it was no-code. The business has never cared how the value gets made. We were just the only ones who could make it, and **we mistook that monopoly for respect.** We were always a means to an end. The end belongs to the business. The means now belong to everyone.


## We've done this before


"But the slop is dangerous." Yes. It is. Last October, researchers[scanned 5,600 vibe-coded apps](https://escape.tech/blog/methodology-how-we-discovered-vulnerabilities-apps-built-with-vibe-coding/) running in production and found more than 2,000 vulnerabilities, 400 leaked secrets, and 175 instances of exposed personal data, including medical records and bank account numbers. Moltbook[leaked 1.5 million API tokens](https://www.wiz.io/blog/exposed-moltbook-database-reveals-millions-of-api-keys) three days after launch through a Supabase key sitting in client-side JavaScript. Slop compounds, and anyone who has carried a pager knows a four-second decision is how you end up on an incident call two years later. The incident calls have started.


This is the same problem that created DevOps.


Developers wanted to ship faster than operations could safely absorb, and the industry's first answer was to block. Tickets, change advisory boards, walls. It didn't work. Developers did an end run around ops the same way citizens are routing around everyone now. DevOps was the eventual admission that the answer to a faster class of builder is a paved road, not a bigger gate. Build it so the fast path and the safe path are the same path. Plenty of organizations still struggle with that today, and now a new kind of builder is coming at them, faster than developers ever were, shipping straight to users without ever touching a server ops knows about. And the vendors are paving the on-ramp. Microsoft is shipping[open source skill files](https://learn.microsoft.com/en-us/fabric/fundamentals/skills-for-fabric-overview) that turn Claude Code and Copilot into Fabric-aware agents, so an analyst can stand up and query enterprise data workloads in plain English. The largest software company on earth wants more of these builders, not fewer.


So no, operations is not obsolete. I'd argue the opposite as loudly as I can. For thirty years, the thing that actually kept companies safe wasn't governance. It was scarcity. Only a handful of people could create software, so the blast radius stayed small enough to manage by hand. That scarcity is gone. When everyone in the org can ship an app, the guardrails are the only control left. DevOps just became the most important job in the building, and most ops teams don't know it yet. Neither do the frameworks. The[CSA points out](https://labs.cloudsecurityalliance.org/research/csa-research-note-vibe-coding-ai-governance-gap-20260602-csa/) that none of the major AI security frameworks (NIST AI RMF, the OWASP LLM Top 10, CSA's own) offer dedicated guidance for citizen developers shipping without professional security oversight. The people writing the standards haven't caught up to the people writing the software.


You can't stop the citizens, and your CEO doesn't want you to. The job is to be the steward of the non-negotiables (security, cost, compliance) while the whole company builds around you at light speed. Get out of the way and stay in control at the same time. The new work is harder and more valuable than anything we did before.


## They're not going back


You might laugh at these people. You might think the AI bubble is going to pop. It may. But the person in marketing has shipped an app now. The analyst who lived in spreadsheets has a working tool with her name on it. People do not hand back that kind of power once they've felt it, bubble or no bubble.


The citizen developer was always there. The tools just scaled them up to where nobody can miss them.


In 2009, John Allspaw and Paul Hammond stood on stage at Velocity and told a room full of engineers that Flickr was deploying more than ten times a day. Half the room heard recklessness. The industry's answer was to make deploying safe enough that the number stopped mattering: small blast radius, fast rollback, tooling everyone shared. Seventeen years later the number is coming back, except now it's ten deploys a day from sales, from claims, from that analyst. Ops is the only team in the building that can make that number boring.
