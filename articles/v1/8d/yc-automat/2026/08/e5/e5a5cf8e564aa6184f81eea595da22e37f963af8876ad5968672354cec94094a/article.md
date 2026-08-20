---
schema_version: "1.0.0"
document_id: "e5a5cf8e564aa6184f81eea595da22e37f963af8876ad5968672354cec94094a"
company_key: "yc-automat"
company: "Automat"
source_id: "yc-automat-news-import-800367705320"
canonical_url: "https://runautomat.com/blog/the-biggest-rpa-company-wants-to-invest-in-whats-replacing-it"
published_at: "2026-08-11T21:51:48.979+00:00"
first_seen_at: "2026-08-07T03:24:31.231391+00:00"
fetched_at: "2026-08-07T03:24:33.196635+00:00"
content_hash: "sha256:689cfe311e1ccc6a467a9864bfebc77fc6ad2b828c2ebb3ccd89301282e9c96a"
---

# One of the largest public RPA companies tried to invest in us | Automat

*This is the second edition in a series on where enterprise automation is headed. The first made the case for why manual work persists. This one gets specific: the root cause, the places it surfaces, and what fixing it takes.*


‍


## Why do our competitors want in?


‍


If your competitor is paying attention to the problem you're solving, closely enough to try to invest in you, what does it tell you?


‍


This isn't hypothetical for us. One of the top public RPA companies reached out early on to try to invest in automat and have a stake in our company. We take that as a signal we're working on something that matters. The companies that built the last generation of automation understand its limits better than anyone, and when they look for exposure to what comes next, they're agreeing with the diagnosis.


‍


So this edition is about that diagnosis. Where the failure actually lives, what it costs, and what it takes to fix it.


‍


## The root cause: your business is built for a purpose, but your automations weren’t


‍


In the first edition, I shared[my philosophy](https://runautomat.com/blog/a-builders-philosophy-why-were-rethinking-a-billion-dollar-question-and-rebuilding-it-from-scratch-now) on building automations: that every system doing work on a computer has to answer two questions. What am I looking at? And what is the reason I’m doing this work?


‍


Traditional automation answers neither. RPA binds to the presentation: clicks, keystrokes, screen positions. It knows where to click based on visual similarity and never what it's doing or why. The layer that holds the goal, the meaning of what's on screen, the judgment about what to do when something unexpected appears, was never built. I call it the context layer. Its absence is why traditional automation was never built to survive a change in the business.


‍


The omission was understandable. RPA emerged as a workaround. Companies had legacy systems without APIs and wanted automation without deep IT projects, so the tools imitated a human operating the screen. That gave quick wins and a low barrier to adoption. It also locked in a design that is fragile, inflexible to rule change, and blind to meaning. **The tradeoff that made RPA easy to adopt is the same one that makes it fail at scale: it replays recorded actions**[instead of understanding intent](https://runautomat.com/blog/ai-rpa-vs-traditional-rpa) **.**


‍


You can see the missing layer in how the industry itself talks about failure. The standard advice for a struggling automation program has always been strategic: pick simpler processes, standardize before you automate, keep exceptions below 20 percent. Meanwhile, EY[reported](https://www.forbes.com/sites/cognitiveworld/2018/12/02/the-big-rpa-bubble/) in 2016 that 30 to 50 percent of initial RPA projects fail, and Deloitte's 2018 survey of more than 400 global companies found that 63 percent said implementation took longer than they had expected. The usual diagnosis is that companies failed to align automation with their business goals. Follow the constraint, and it explains itself: the tool could only reach a narrow, stable, exception-free slice of the business, so the goals were scoped down to fit it. Advice like "clean up your process first" was a quiet confession about what the technology couldn't handle.


‍


## What happens when the system can't understand the goal


‍


Once you see that root cause - the traditional automation is built without the context - these four most common failures start to line up:


‍


**Brittleness.** A bot with no model of its goal can't adapt to changes. A UI update, a moved field, a login flow that adds a step, and the recording is invalid. It bites hardest in work that touches systems you don't control: web portals, third-party vendor sites,[legacy ERPs feeding newer web apps](https://runautomat.com/blog/automate-sap-citrix-without-apis) . Every vendor release becomes a potential outage for your automation. This surprises buyers with striking consistency: in a survey by Pegasystems, itself an automation vendor that competes with RPA, 40 percent of adopters said their bots took more time and resources to maintain than expected.


‍


**Variation.** Without context, everything must be scripted in advance. Real work is full of exceptions and messy inputs: emails, contracts, scans, invoices that every vendor formats differently. Anything requiring reading or judgment falls outside the decision tree, so the bot errors out or routes to a human. The industry's patch, bolted-on OCR and ever more rules, mostly relocates the cliff edge.


‍


**Maintenance.** A system that can't adapt must be constantly repaired, and[the upkeep often approaches or exceeds the labor the bots replaced](https://runautomat.com/blog/why-companies-are-leaving-uipath-2026) . In the programs I've sat with, maintenance takes a third to half of total program cost and team time, which is capacity spent standing still rather than automating anything new. Tight coupling means another team's routine change becomes your outage, and every new bot adds one more thing to be managed manually. Eventually, companies end up hiring more people to manage the systems that were supposed to help them work faster.


‍


**Scale.** Automating a single task is easy. Orchestrating the full process it sits inside is a different problem entirely: work that spans systems, carries decisions, and absorbs exceptions along the way. A system with no context has no way to hold a whole workflow together, so the end-to-end version usually never gets attempted. And stitching bots together doesn't get you there; every seam adds fragility.


‍


Lastly, **process drift.** Businesses don't hold still. Policies change, vendors churn, systems get consolidated after an acquisition, teams find better ways to work. “ **A recorded script encodes a snapshot of the business on the day it was built, and every day after that, the business and the script drift apart.”** The technical debt literature on automation names this directly: automation layers fall out of sync with the business logic they were meant to serve.


‍


A system with a context layer evolves with the process. A system without one has to be rebuilt every time the process moves.


‍


## Companies know all this. So why is it so hard to solve?


‍


They do. What holds them back is reliability and accountability.


‍


Start with an uncomfortable fact of **timing** : the AI that makes a real context layer viable arrived roughly a decade after most enterprises committed to RPA. By the time the better answer existed, the old one had compounded into something much harder to evolve from. Teams hired around it, Centers of Excellence built to sustain it, hundreds of bots woven into daily operations. That's the real barrier: the cost of unwinding a commitment that was rational when it was made. Replacing a working if brittle deployment is expensive, disruptive, and political. Those bots run payroll today, and "better in principle" competes with "works well enough, and I know how it fails."


‍


**The main concern is determinism.** In[banking](https://runautomat.com/industry/banking-and-financial-services) ,[insurance](https://runautomat.com/industry/insurance-ops) , and[healthcare](https://runautomat.com/industry/healthcare) , rigidity is a feature. A rule-based bot does the same thing every time and leaves a clean audit trail. A reasoning system can also reason incorrectly in novel ways; nondeterminism is hard to certify and defend, and ownership of an agent's bad judgment can get murky.


‍


Add the earned skepticism of a category that overpromised autonomy before.[Gartner now warns about "agent washing,"](https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027) vendors rebranding RPA and chatbots as agents without substantial agentic capability, and predicts that over 40 percent of agentic AI projects will be canceled by the end of 2027. Discounting the new pitch is rational, even when the technology has genuinely changed.


‍


## What solving the context layer focuses on


‍


The work is building automation around understanding, then proving that understanding can be trusted.


‍


**It starts with binding to intent rather than coordinates:** the agent learns the goal, perceives the page the way a person does, locates elements by meaning, and finds the new path when a layout shifts. This means changing the unit of automation from a fixed decision tree to a model that generalizes: reading arbitrary documents without templates, making judgment calls on unscripted cases, escalating only the truly ambiguous. It means designing for the whole process from the start, carrying state across systems and handling exceptions inline. And because the system adapts on its own, the largest maintenance cost collapses. A vendor's UI change no longer breaks anything, and process drift stops being a rebuild event.


‍


Then the part that matters most for the skeptic: **adaptability and auditability hold together.** A system can reason about context and still show its work, log every action, operate inside bounded and reviewable limits, and keep hard determinism exactly where compliance requires it. Where that proof is still being built, we say so plainly. In a market full of overpromises, honesty about the edge of the evidence is itself a differentiator.


‍


## The tradeoff was just lifted


‍


For fifteen years, automation stayed brittle because trust required rigidity. That was a reasonable trade when software couldn’t understand software (models couldn’t understand a screen). It stopped being reasonable when something could.


‍


The market seems to agree. Gartner's own data shows RPA's growth slowing: the market grew 14.5 percent to $3.6 billion in 2024, and Gartner attributes the deceleration to generative AI, computer use tools, and agentic automation. The companies that built the old generation appear to see it too. That's what their interest tells us, and it's why we're building the context layer first.


‍


If it sparks any thoughts,[we'd love to hear them](https://docs.google.com/forms/d/e/1FAIpQLSfKdYDbW21Zee2guKu2W2CiiH-5Qqw2ve7JOUqXtLeOxe6kPw/viewform?usp=header) .


‍


*Coming next:*


*In the next edition, we'll look past process automation entirely: when the structured work is handled, what happens to everything in between?*
