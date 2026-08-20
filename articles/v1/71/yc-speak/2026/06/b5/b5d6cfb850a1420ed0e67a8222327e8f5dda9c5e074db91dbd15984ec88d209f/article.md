---
schema_version: "1.0.0"
document_id: "b5d6cfb850a1420ed0e67a8222327e8f5dda9c5e074db91dbd15984ec88d209f"
company_key: "yc-speak"
company: "Speak"
source_id: "yc-speak-news-import-4988501dda0c"
canonical_url: "https://www.speak.com/blog/notes-from-a-builder-pm"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-22T14:26:08.696382+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:590f5c64bed60b6fdf46573c1b5f872d282111e0fc2d3da8aa8be00d7c255cea"
---

# Notes from a builder PM

Speak was my first real job. I came in in 2024 as a design systems intern with almost no experience, and I was handed something that usually takes a whole team: building Speak's first design system. Over a few months I designed components across iOS, Android, and web, sat in with the mobile and web engineers, and spent about as much time getting people to actually use the system as I did building it. It taught me to think in systems. It also taught me you can't design one from far away. You have to be close to the pieces, in the room when someone tells you what's broken.


I kept going after the internship. Through senior year I stayed on part-time and started writing iOS code, now for features and not just the components I'd designed, shipping into a codebase I used to only sketch against. By the time a full-time offer came, I'd stopped fitting cleanly into design or engineering. I was doing both, wherever the problem needed it.


Product team in San Francisco


### **Learning product management**


The full-time role was in growth, and it was my first time as a PM. I'd never owned an acquisition funnel, run a lifecycle program, or managed a paid channel. I'd never really done growth or marketing at all. That gap was the part I was most excited about. Product touches every function, and you can't do it well without actually understanding how each one works.


My one worry was that being a PM would pull me off the hands-on building I loved and leave me coordinating instead of doing. But this was late 2025. The models were getting better by the week, and I found I could get into these functions instead of managing them from outside. I read the marketing stack and pulled the data myself. I got into the code. A year later, that's held.


### What the work actually looks like


Our pod is small and we own a huge surface: paid acquisition in every market, lifecycle, partnerships, organic, regional campaigns, the web funnels. So there's a constant stream of requests, almost no two the same. A campaign that needs a story-submission site and coupon codes wired up. A tracking pixel here, a pricing code there. In my first year we cleared more than 75 of them, most in under a week, where the same kind of request used to sit for one to two weeks whenever engineering was busy.


The deeper work sits underneath. Attribution that breaks when tracking parameters drop somewhere between the website and the app, quietly skewing every market's numbers. Experiment infrastructure that has to exist before anything on my roadmap can run. The internal tooling the whole growth org leans on: the dashboards, the request pipeline, the integrations that wire our marketing stack into everything else. These are systems problems, the kind I came up doing, and they take most of my attention.


AI is what lets me work at both levels at once. By the time I open a request, the agent has already pulled the data, traced the code path, found the last time we tried the same thing, and checked what marketing is running on that audience. My hours go to deciding and building instead of gathering. I can hold a dozen threads without losing the detail on any of them, which a year ago I couldn't have.


It also surfaces what a topline would bury. We ran experiments on the sign-in flow that looked flat overall, but cutting the data by device and provider showed where it was actually failing. The fixes that came out of that work cut our web auth error rate by about a third. A year ago I wouldn't have thought to make that cut. Now it's an afternoon.


### The pod protocol we wrote


Doing all this alone is just a personal productivity trick. Making it work for the pod was the real challenge, and we learned that one the hard way.


Early on I shipped an experiment almost entirely agentically. It worked, it went out, but the code was ugly. Worse, my engineers had been handed a big change with no real way to review the decisions buried in it. They could read the diff. They couldn't see the architecture I'd implicitly chosen. That costs nothing today and a lot in six months.


So we wrote a workflow. Now I start every change with a plan PR, an engineering design doc laying out the *how* , next to the approved PRD covering the *what* , before any code gets written. The engineers review the plan first. If the architecture is wrong, we catch it on one page instead of in a long diff. Once the plan's approved, the agent implements against it, and the final review can focus on the parts that are actually risky.


We've added more since. QA plans written up front so the agent checks its own work. A shared library of skills across the pod. A triage agent that asks the clarifying questions I used to ask and files the ticket itself. No single tool is the point. The pod agreed on how to work, so the leverage compounds instead of sitting in my head.


## What's still hard


The thing we're still figuring out is context. The demos all assume a tidy world: current docs, clear owners, nothing contradicting anything else. A real company is the opposite. Years of Slack. A dozen tools. Notion pages that disagree with each other. Decisions made and quietly reversed. Code from last week next to code from two years ago. Getting an agent to know what's actually true right now is real work, and it doesn't scale just because one person on the pod happens to be good at it. Speak called this context engineering in a post earlier this year, and I think it's the most underrated team skill of the next few years. A pod moves at the speed of its shared context.


## Looking back


I came in having never run an experiment or shipped a campaign. I learned both the way that actually sticks, by doing them, with an agent next to me the whole way. Parts of the business that used to be black boxes to me are now places I can build.


The work I'm proudest of isn't in any repo, even though I spent plenty of the year in one, writing code across web, mobile, and backend wherever the pod needed it. The team grew from two people to eight, and I built a way of working where it doesn't wait on me. Engineers who start before I've finished scoping. An analyst who doesn't wait for the question to be perfectly framed. A request pile that handles its own first mile. We ship faster than a team our size should, because my judgment goes where it counts and the systems I trust carry the rest.


That's the kind of PM I'm still becoming, and Speak is where I get to grow into it.


‍
