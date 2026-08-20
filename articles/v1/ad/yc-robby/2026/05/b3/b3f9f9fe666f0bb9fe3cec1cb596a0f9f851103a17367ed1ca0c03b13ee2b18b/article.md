---
schema_version: "1.0.0"
document_id: "b3f9f9fe666f0bb9fe3cec1cb596a0f9f851103a17367ed1ca0c03b13ee2b18b"
company_key: "yc-robby"
company: "Robby"
source_id: "yc-robby-news-import-47ee1caa7dbe"
canonical_url: "https://www.tryrobby.com/blog/blanton-and-sons-ai-automation-adam-grubb-interview"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-23T23:13:24.345752+00:00"
fetched_at: "2026-07-28T22:12:48.521638+00:00"
content_hash: "sha256:9bb0fe6cc8741a43ded9cb93eb82eb3f82138e6d17a982eefdac3e56c4648311"
---

# The Leaf Blower on the Roof: How Blanton & Sons Uses AI to Stay a Family Business

This is the third installment of our Home Services Leaders in AI series. We sit down with business owners and operators who are actually using AI and automation to run their companies — no theory, no hype, just real stories. (Catch up on the[first interview with Sarah at Golden Rule Cleaning](https://www.tryrobby.com/blog/golden-rule-cleaning-ai-automation-sarah-interview) and the[second with Dave at Glass Therapy Clarksville](https://www.tryrobby.com/blog/glass-therapy-clarksville-ai-window-cleaning-dave-interview) .)


[Blanton & Sons](https://blantonandsons.com/) is a family-owned plumbing and HVAC company that's been around since 1996. The founder, Jimmy Blanton, still lives on the property. His son Brandon is co-owner. And the general manager, Adam Grubb, started there as a plumber in 2018, became plumbing manager, then got promoted to GM about a week after that. He runs the company day-to-day now, and the way he runs it is unusual: every back-office process he can automate, he does. Not to cut headcount for the sake of it, but to claw back the time it takes to be the kind of company where the owner climbs on a customer's roof with a leaf blower.


## The Story That Reset the Company


A while back, Adam and Brandon took a call from a long-time customer. She'd been with the company for over twenty years. She told them she still remembered the day Jimmy Blanton himself came out to install her water heater. "It was the funniest thing," she said. "He finished the install, and the next thing I know, I hear this loud noise. I walk outside, and Jimmy is up on my roof with a leaf blower, blowing out my gutters."


This is a plumbing and HVAC company. Nobody had any business with her gutters. Jimmy just noticed they were full while he was up there, and figured he'd handle it. "Let me show these people I care," Adam recounts. "That's all it was."


The story landed hard with Adam and Brandon. By that point Blanton & Sons had grown fast — too fast, by Adam's admission. "We scaled a little bit too big for our bridges," he says. "And sometimes you lose sight of your values." Cracks were showing. The leaf-blower story was a reminder of who they were supposed to be.


ℹ️


Why This Matters for the AI Conversation


Most of the AI conversation in home services is framed around growth: more leads, more bookings, faster dispatch. Adam's framing is different. He uses automation to protect a culture, not to scale past it. Every minute the back office isn't on payroll math or PO orders is a minute someone can be on the phone with a customer or — metaphorically or literally — climbing onto a roof with a leaf blower.


## From Plumber to GM in Two Weeks


Adam's path through the company is its own story. He came on in 2018 as a plumber doing installs. He'd help out on service calls when the install board was empty. The owners — Jimmy and Brandon — would hang out with the team, sit around the campfire, talk shop. Adam's wiring stood out. He thought about the work like an operator, not just a tech.


One day he told them, "My goal is to be your plumbing manager." That Friday, they made him plumbing manager. He immediately said, "I'm not going to stop there. One day I'll be your general manager." A week later, they made him GM.


"I told them I wasn't ready," Adam says. "But we did it." That field-to-management arc shapes how he thinks about technology. He remembers being on the truck and groaning when the office introduced a new process. "I'd be like, oh great, more for me to do every day." Now he's the one introducing the processes, and he designs them with that older version of himself in mind.


## The Stack: ServiceTitan, Atlas, and a Web of Vendors


The platform underneath everything at Blanton & Sons is ServiceTitan. Inside ServiceTitan, the techs lean on Atlas (ServiceTitan's built-in AI assistant) for in-the-moment help, and they fall back to ChatGPT for trickier troubleshooting. "We didn't have any of that when I was in the field," Adam says. "It was just read the manual. Now they can pull up a manual in an instant. They can check warranty status in seconds. It helps them educate the customer better, which drives satisfaction."


On top of ServiceTitan, the company runs a layered set of automations from outside vendors and from Adam's own builds. Two of the outside ones do most of the lifting on the front end of the funnel.


### Buck AI: The Dispatch Board That Builds Itself


Blanton & Sons partners with[Contractors in Charge](https://contractorincharge.com/) (CIC) for call handling. CIC fields the inbound calls and books them. Then a tool called Buck AI takes over the dispatch board. Buck reads each tech's skills, the area they work, and the live state of the schedule, and assigns calls accordingly. The unassigned bucket fills, Buck distributes, the dispatcher reviews.


"It used to be an all-day job," Adam says. "Now you just manage capacity for the week." The downstream effects are concrete: less drive time, less wasted gas, more revenue per truck. "With gas prices being what they are, that alone is a huge deal."


### OnePath: Online Bookings With Zero Hands


[OnePath](https://onepath.ai/) , also under the CIC umbrella, handles a much wider swath of the customer-facing layer than just bookings. It runs an AI help desk on the Blanton & Sons website, a web scheduler that drops jobs straight into the CRM, all of the company's drip campaigns, and inbound from Google LSA. A customer reaches out through Local Services Ads, OnePath replies, qualifies, and books the job. "Zero hands have to touch it," Adam says. "All we do is monitor and classify whether the lead was good."


Adam is matter-of-fact about the category. "This is your normal stuff," he says. "If you walk into any home-services event, a hundred vendors are doing this same kind of thing." But the choice of vendor wasn't generic. Buck AI and OnePath both sit under the Contractors in Charge umbrella, and the whole reason CIC exists — across every product they ship — is to elevate the customer experience. For a company whose entire identity is the leaf blower on the roof, that fit is the point. The interesting work on top, in Adam's view, is the layer he built himself.


## The Automation That Eliminated a Whole Role


The build Adam is most proud of removed an entire position from the org chart. The company used to employ an install coordinator whose job was to take a job a tech had sold in the field, gather all the relevant info, build a new install job in ServiceTitan, and send out the purchase order to vendors. It's exactly the kind of meticulous, paper-shuffling work that's miserable to do at scale and easy to fumble.


Adam worked with an outside AI shop to build a workflow that handles it end to end. When a tech sells equipment they don't have on the truck, they mark the job "perform later" inside ServiceTitan. The automation grabs the form data, the invoice summary, and the sold estimate. It builds the new install job, populates it correctly, and fires off the PO order.


📊


From Coordinator to Customer Advocate


The install coordinator role didn't disappear so much as it changed shape. The person who used to spend her day building jobs and chasing POs now spot-checks the AI's work and spends the rest of her time on the phone with customers — keeping them informed, answering questions, smoothing the install. Same person, same paycheck, much higher leverage.


It's the cleanest example of Adam's philosophy in practice. Strip the manual work out, point the human time at the customer. "These automations are great as a tool," he says. "Not to replace people. Enablement."


## Building His Own With Claude


Adam keeps subscriptions to both Claude and ChatGPT and uses them to build his own internal tools. The first thing he reaches for, more often than not, is a calculator.


Here's the kind of thing he'll spin up: a mobile pricing calculator for techs in the field. The tech is sitting in a customer's home, sizing up an HVAC system. The weather is mild, demand is soft, the customer is shopping. The tech needs to drop margin a little to land the job. Adam's calculator lets him punch in the equipment cost, material cost, target margin, and commission rate, and it spits back a price — plus a tier of acceptable prices above and below. "It's a cheater," Adam says. "He knows where he needs to be to make the money he needs to make and still close the deal."


On the management side he built the inverse. He plugs in the price the tech offered and the calculator tells him what margin and commission that price implies, given current labor and material costs. "Kind of like a job coster," he says. "I can spot-check after the fact."


### The Payroll Workbooks


The other thing Adam built with Claude is a set of payroll workbooks. Blanton & Sons pays techs either hourly or on commission, and South Carolina labor rules require overtime to be calculated on the commission dollar amount, not just on a base rate. That's the kind of math that's easy to get wrong, and getting it wrong has legal exposure. Claude helped him build workbooks that handle the calculation automatically.


Even with the workbooks, payroll still isn't fully automated — it's the next big project. Today, by the time Adam finishes payroll, he's also produced a weekly P&L because he's job-costing every line as he goes. "I know exactly where we're at on margin every week." The trade-off is that payroll still eats real hours of his week.


💡


Adam's Build vs. Outsource Framework


"If it's something simple — this happened, now do that — I'll build it myself. If it sounds fun, I'll make time to learn it. But if it's APIs and webhooks, or anything where I could mess up someone's payroll and get into legal trouble, I'm bringing in help." That's the whole rubric.


## What's Most Surprising About All This


Asked what's surprised him most as he's rolled this stuff out, Adam doesn't pick a tool or an integration. He picks the secondary effects.


> The amount of time you actually get back. We have fewer customers calling asking 'what's going on?' If I have time, I'm not rushed to get off the phone. I can let them know full-fledged — we're going to do this, this, and this. Customer satisfaction goes up. Reviews go up. And reviews are just free phone calls coming back to you.
>
>
> — Adam Grubb, Blanton & Sons


It's a familiar argument by now in this series. Sarah at Golden Rule used the time savings to build a fully remote operation. Dave at Glass Therapy used it to scale from $0 to $650K in two years. Adam is using it to make a 30-year-old family business feel like a family business again. Different goals, same lever.


## Adam's Framework


1. Treat AI as enablement, not headcount reduction. The install coordinator at Blanton & Sons didn't lose her job; she traded paperwork for customer conversations. That's the bar.
2. Outsource the front-end commodity automations. Dispatch and online booking are solved problems. A hundred vendors at any home-services event are doing the same thing. Pick a good one and move on.
3. Build the weird stuff yourself. The pricing calculator, the reverse calculator, the company-specific payroll math — that's where Claude and ChatGPT earn their keep. Internal tools shaped exactly to your operation.
4. Outsource anything with legal exposure. Payroll, taxes, complex APIs and webhooks — get help. Saving a thousand bucks isn't worth getting a labor complaint.
5. Use the time you save to compound. Adam built the install automation, then used the time it saved to build the next thing. The point of automation isn't to do the same job faster — it's to free up the next investment.
6. Anchor it back to a story. The leaf blower on the roof. Whatever your version of that story is, write it down somewhere people can see it. Every automation should pass the test: does this give us more capacity to be that company, or less?


## The Bottom Line


Blanton & Sons isn't trying to be a tech company. They're trying to be a 30-year-old plumbing and HVAC business that still answers the phone like it's 1996 and shows up like it's the founder's first job. The way they're getting there — and the way Adam is keeping the place running while he gets there — is by stripping out every minute of administrative drag he can find.


An entire coordinator role disappeared. A pricing calculator lives in every tech's pocket. Payroll math that used to risk legal trouble now runs through workbooks Claude helped write. And the goal, every time, is the same: more time on the customer, less time on the keyboard. The leaf blower on the roof is the metric.


## Ready to Automate Your Home Services Business?


If Adam's story sounds like the kind of operation you're trying to build — where the back office gets out of your team's way and the customer experience pulls ahead — that's where Robby comes in. We help home-services companies wire up AI-powered documentation, automated job tracking, and the kind of integrations that connect ServiceTitan, HouseCall Pro, and the rest of your stack. No agency retainer, no generic pitch.


Book a free demo and we'll walk through your specific workflow and show you the biggest wins.


ℹ️


Home Services Leaders in AI


This article is part of our video series interviewing home-services business owners and operators who are leading the charge with AI and automation. Follow along on our blog and LinkedIn for new interviews each month. Got a story to share, or know someone we should talk to? We'd love to hear from you.
