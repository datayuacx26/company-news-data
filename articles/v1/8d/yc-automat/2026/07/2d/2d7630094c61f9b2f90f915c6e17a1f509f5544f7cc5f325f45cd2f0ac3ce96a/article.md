---
schema_version: "1.0.0"
document_id: "2d7630094c61f9b2f90f915c6e17a1f509f5544f7cc5f325f45cd2f0ac3ce96a"
company_key: "yc-automat"
company: "Automat"
source_id: "yc-automat-news-import-800367705320"
canonical_url: "https://runautomat.com/blog/a-builders-philosophy-why-were-rethinking-a-billion-dollar-question-and-rebuilding-it-from-scratch-now"
published_at: "2026-07-23T19:38:34.201+00:00"
first_seen_at: "2026-07-24T17:51:31.960712+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:a480cd8fda3fd15a38d87310b4ac5f63f33d68ed204b143b3d32baa3869e7f2f"
---

# A Builder's Philosophy: Why We're Rethinking a Billion-Dollar Question, and Rebuilding It From Scratch Now | Automat

*This is the first in a series of conversations about where enterprise automation is headed. We're starting with the foundations: what we're building, why we're building it, and the philosophy behind it. Future editions will go deeper into where legacy RPA falls short and what comes after process automation entirely.*


‍


## Before we get to the technology: what problem are you actually solving?


Manual computer work at enterprise scale.


‍


I've spent a lot of the last few years sitting with operations teams at lenders, insurers, manufacturers, and healthcare providers, watching how their work actually gets done. The scene is remarkably consistent: A document comes in. Someone reads it, retypes what they find into a portal or a legacy system, cross-checks it against another screen, and passes it to the next person in the chain. This isn't written into anyone's job description, but it exists because the systems that run the business were never built to talk to each other, so people became the connective tissue.


‍


For many of the teams I've sat with, this is the single largest controllable cost in the operation: routinely upward of $500K a year on repetitive computer work, and climbing with every new customer, because the only way anyone has found to process more volume is to hire more people.


‍


That's when growth gets expensive. More software, bigger teams, more processes and workflows, all scaling in a straight line with revenue. Speed is capped by how quickly you can recruit, train, and retain people, while error rates and compliance exposure climb with volume.


‍


And guess what? People make mistakes. An error slips through, someone has to find it, fixing it takes time, and the person assigned to fix it is usually the same person who was supposed to be improving the process in the first place. The best people on the floor spend their days firefighting.


‍


Software is supposed to solve problems and make teams faster. But for fifteen years, the way it has been built and maintained has failed that purpose.


‍


## Fifteen years and billions in enterprise spend later, why does it need to be rebuilt now?


Because the technology was brittle from day one, and the industry built a business model around the brittleness instead of fixing it.


‍


When RPA emerged in the early 2010s, it addressed the problem with the best tools available. Enterprises ran on legacy systems without APIs, and the only way to automate them was to script the screen: record an operator's clicks,[anchor each one to an element selector](https://runautomat.com/blog/ai-rpa-vs-traditional-rpa) , and replay the sequence. For 2012, that was a reasonable engineering tradeoff.


‍


But it carried a structural flaw. A bot built on selectors is a fixed script running in a living environment. Applications update, layouts shift, fields move. The bot has no model of what it's doing, so it can't recover. It clicks the button because the script says click the button. When the button moves, it breaks, and a person has to fix it. **Every bot ships with its own future maintenance bill attached.**


‍


Faced with that flaw, the industry had two choices: fix the technology, or monetize the fragility.


For the first decade, fixing it genuinely wasn't possible. Recovering the way a person does requires models that can look at a screen and understand it, and those didn't exist yet. So an economy formed around the second choice. Vendors sold licenses priced per bot, along with developer certifications. Systems integrators billed implementation and maintenance hours. Enterprises stood up internal teams to keep the bots alive. Follow the incentives and the 15 years start to explain themselves: **nobody in that value chain earns more when bots stop breaking.**


‍


What has changed is that the technology constraint expired. Models that can see a screen and reason about it have existed for several years now. The market leaders have been slow to rebuild on them, and it's hard not to notice that[genuinely solving the maintenance problem would cannibalize the license and services revenue](https://runautomat.com/blog/why-companies-are-leaving-uipath-2026) they've spent 15 years building. Our read is that this has more to do with balance sheets than with technology.


‍


## What does that look like inside a company that's bought in?


You end up with software that was supposed to eliminate manual work but instead generates it.


‍


The Center of Excellence is a clear example. Companies stand up these teams to build and maintain their automations, and the team scales linearly with the number of automations. Fifty bots, so many engineers. A hundred bots, twice as many. At scale, the math turns upside down: maintenance consumes the majority of the program's total effort, and for every dollar spent on licensing, another three to four dollars go to keeping the bots running. The headcount you were trying to avoid shows up as automation headcount on a different budget line, where it's harder to see.


‍


A simple test I give operations leaders: count the people whose jobs exist because automation exists. If that number grows every quarter, the program has quietly become a second operations team.


## What does “agentic” actually mean? How does it solve the problem?


‍


An agentic system understands the "why" behind the work and learns as your process evolves, not just the sequence of clicks.


‍


Our agents learn the way you'd train a new hire. They read the standard operating procedure or watch a recording of an operator performing the task. From that, they understand the goal, the steps to get there, and enough context to handle situations the script never anticipated. When the UI changes, the agent looks at the screen and identifies what moved, just as a person would. That's what self-healing actually means.


‍


I'd draw a line here, because[the word "agentic" is on every vendor's homepage right now](https://runautomat.com/blog/what-is-agentic-process-automation) . A lot of it is performative AI: a wrapper around the same old technology, a copilot sitting atop bots that still run on selectors. **True agentic automation means a dedicated agent that performs the task independently and repairs itself when things change.**


‍


If a developer is still maintaining selectors underneath, the honest description is "rebranding," not "agentic automation."


## Every automation vendor claims some version of that now. How is Automat different?


‍


Three things.


‍


**First, you go from demonstration to automation without a dedicated technical resource.** Show the agent the process, and it builds the automation itself, no certified developers, no months-long implementation.


‍


**Second, we balance deterministic and probabilistic behavior.** This is the part most people building in this space get wrong. Pure determinism gives you legacy RPA: accurate until anything changes, then broken. A purely probabilistic agent improvises when you need it to be exact. Enterprise work needs both repeatability and accuracy in the structured parts, and adaptation when the environment throws something novel or unstructured at you. Getting that balance right is most of the engineering.


‍


**Third, we keep a dedicated engineering function on our side of the table.** If an automation fails, we treat that as our problem to fix rather than a ticket in your backlog. We're economically invested in the automation working, while the legacy model gets paid whether your bots run or not.


‍


## Who are you building this for?


Operations leaders. CFOs, COOs, the people who actually understand their company's processes and feel the cost of the manual work. They aren't shopping for a platform to tinker with. They want the most efficient, compliant way to get manual tasks off their team's plate.


‍


The conversation with them often goes something like this: you have scaling goals for the next few quarters. The default path is hiring, and hiring for repetitive computer work is slow, expensive, and hard to reverse. The alternative is agentic teammates at a fixed or variable cost that doesn't blow out your org chart: higher productivity, more output, a leaner team, better unit economics.


## What's your philosophy behind building Automat?


Software should reduce work rather than relocate it. And I believe the way to achieve that is to build systems that take on the whole job, understanding included. When an agent learns the goal behind a process, adapts on its own when the environment changes, and comes with a team accountable for keeping it running, the effort genuinely leaves the customer's side rather than resurfacing as maintenance, tickets, or headcount.


‍


Every design decision we make gets tested against that: does this remove effort from the customer's side, or does it just shift it to a less visible place? Legacy automation failed that test for 15 years. We're building something better to solve the problem at its source.


‍


Thank you for reading this article. If it sparks any thoughts,[we’d love to hear them](https://runautomat.com/contact?utm_source=newsletter&utm_medium=email&utm_campaign=A+builder%27s+philosophy) .


‍


*Coming next:*


*In the next edition, we'll dig into the specifics: the common challenges legacy RPA promised to solve and where it falls short.*
