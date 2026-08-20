---
schema_version: "1.0.0"
document_id: "810b5a97a178289f1e967d67bc20940205c351b6ccb133ee58d79d6b3624b13d"
company_key: "yc-pirros"
company: "Pirros"
source_id: "yc-pirros-news-import-d36ebf5364b2"
canonical_url: "https://www.pirros.com/blog/we-took-three-days-off-the-roadmap"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-07-24T09:19:06.597310+00:00"
fetched_at: "2026-07-28T22:15:58.980681+00:00"
content_hash: "sha256:32baf77b92ae216bbeb98936bee8919183a1da6290c2a191258b710cc346ae7a"
---

# We Took Three Days Off the Roadmap. It Forever Changed How We Build.

##### We weren't in trouble. There was no crisis.


But there's a specific kind of exhaustion that comes with being in a fast-paced startup. The kind where your best engineers are executing so hard, so consistently, that they stop inventing. They stop asking "what if we built this completely differently." They just build what's on the roadmap. And they do it well.


Something was missing. People were building quickly, but they weren’t always exploring.


It wasn’t a problem yet, but it felt like the kind of thing that could become one if we ignored it. So instead of trying to tweak the process, I decided that we should step outside of it entirely.


We took three days off the roadmap and gave them back to the engineers.


‍


### **Why We Did This**


At Pirros, our developers are good. Really good. But in the daily rhythm of sprints and deadlines, they're executing someone else's vision. The product roadmap, the spec, the Jira ticket. There's almost no space for them to be the inventor.


I wanted to give them three days to be product owners. Not engineers implementing a feature, but people who identify a problem, design a solution, and ship it. Full ownership, start to finish.


The culture shift was the point.


‍


### **The Setup**


Ten people. A rented house. Tuesday evening to Friday afternoon. No assigned teams, no mandatory theme. One rule: your project must create real value for the business. Everything else (linting, tests, CI/CD, polish) was optional.


The only non-negotiable was demo stability. If it doesn't work on Friday, it doesn't count.


At the beginning, that level of freedom felt a little strange. Without a clear scope or a set of requirements, the question wasn't "how do we build this?" but "what's actually worth building?" There was a brief period where people were circling ideas, testing them out, and letting go of the ones that didn't quite hold up.


But that uncertainty didn't last long. By the next morning, things started to take shape. Small groups formed naturally around ideas that felt promising. Conversations got more focused. People began sketching, debating, and refining in real time. Once that happened, the pace picked up quickly.


‍


‍


### **What Got Built in 72 Hours**


Twelve projects made it to the final demo on Day 3. All of them working. Some already in production.


Three stood out:


- **Agent Lavash:** An AI agent that reads a Jira ticket and, if there’s enough information, opens a pull request automatically. Bug fixes, feature scaffolding, routine tasks. The implications for developer velocity are serious.
- **Revit WebView Bridge:** A web/plugin bridge that creates seamless interaction between our web app and Revit. The impact: 5x faster product development, and every engineer can now contribute to every part of the codebase. Going into production.
- **AutoCAD Integration:** A new plugin that opens Pirros to an entirely different CAD ecosystem. A new market, prototyped in three days. That’s the kind of thing that doesn’t happen inside a sprint.


The rest of the team shipped just as hard — some developers built more than two projects each.


‍


### **The Moment That Stuck With Me**


On day two, Isaac had a realization mid-hack.


What if we reviewed specs instead of code? The spec is the feature written in plain English. If the spec is right, the code almost writes itself.


He was experimenting with spec-driven development using Claude, writing a complete specification of a feature in natural language, letting the AI reason about it, and only then moving to implementation. Code review becomes spec review. The source of truth lives in English, not in a pull request.


Watching that unfold shifted how many of us thought about the process. It made the work feel less like translating requirements into code and more like designing something coherent from the start. The idea that the spec itself could be the primary artifact, rather than something that sits upstream of the “real” work, stuck with us.


It’s become one piece of how we’re evolving our process, and it's something we’re actively trying to apply in development now.


‍


### **What Surprised Me Most**


I expected good work. I didn’t expect complete products.


Almost every team brought something to Friday’s demo that could have shipped that day. Not prototypes. Not proof-of-concepts. Working software.


And I think I understand why. When a developer has full ownership — no handoffs, no approvals, no “let me check with product” — something unlocks. The decision-making is faster. The motivation is different. They’re not implementing someone else’s idea. They’re building their own.


That shift in ownership changed the pace, but it also changed the tone. People were more invested, more decisive, and more willing to take responsibility for the outcome.


Those three days produced more than some of our sprints.


‍


‍


### **How It Changed the Way We Build**


This is the part I didn’t fully anticipate.


After the hackathon, we didn’t go back to normal. We started asking why some of this couldn’t just be how we work.


We restructured how product and engineering collaborate. Now, instead of product handing a spec to an engineer, they build it together in the same document, in the code itself. The engineer makes the implementation calls. Product stops being the bottleneck. We move faster.


One team started prototyping directly in the codebase instead of Figma. PM and engineer in the same session, building the real thing instead of a mockup.


We also adopted get-shit-done (gsd) — a development framework that enforces the mindset we discovered during the hackathon. Fast cycles, clear ownership, no over-engineering.


And the entire team is now on Claude. Not because anyone told them to switch, but because they used it under real pressure for three days and felt the difference themselves. That’s the only adoption that sticks.


‍


### **To the Engineering Leader Who Says “We Don’t Have Time”**


I know. We didn’t have time either.


But here’s what I’ve come to believe: three days borrowed from the roadmap can change your entire development culture. Not just your tooling — your culture.


If your whole team understands how to build product , not just how to implement tickets, your velocity with AI becomes 3x. The tools amplify the thinking. But first, the thinking has to be there.


The hackathon gave our engineers the space to act on what they already felt. Ownership over the outcome, not just the code. They always cared about the product. Now they had the room to prove it.


You'll get projects in production. You'll get tool adoption. But the thing I didn't expect, the thing you can't put on a slide, is that your team comes back different.


More autonomous. More inventive.


We're doing it again.
