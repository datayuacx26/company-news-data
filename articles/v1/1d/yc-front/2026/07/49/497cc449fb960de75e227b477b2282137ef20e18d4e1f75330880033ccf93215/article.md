---
schema_version: "1.0.0"
document_id: "497cc449fb960de75e227b477b2282137ef20e18d4e1f75330880033ccf93215"
company_key: "yc-front"
company: "Front"
source_id: "yc-front-rss-d12a60119b76"
canonical_url: "https://front.com/blog/untangled-ops-episode-2"
published_at: "2026-07-14T04:06:00+00:00"
first_seen_at: "2026-07-21T14:36:25.160509+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:700c37ff3ed95641667c23b5ec16837209e80c0c016299847701925ba05bd63b"
---

# Untangled Ops episode 2: FreightPlus’s Ben Graeff on Bezosisms

Freightplus COO Ben Graeff on why good intentions never fix a broken handoff, and how mechanisms — not effort — closed the gap. A conversation on order management, unstructured data, and the coordination tax hiding in your operations.


FreightPlus operates in the messy middle market — companies that have outgrown their old logistics setup but haven’t built the new one yet. That gap is where trust breaks: handoffs get dropped, SLAs slip, and 80 to 90 percent of the data running the business is unstructured — paper BOLs, PDF invoices, a buried email asking for a load to move today.


Ben Graeff, COO at FreightPlus and ex-Amazon in product management, joins Megan Beatty, Head of Product at Front, on[Untangled Ops](https://front.com/untangled-ops) to talk about the Bezos principle that shapes how he runs operations: good intentions never work, mechanisms do. Ben walks through how FreightPlus turned that principle into practice — rebuilding their order management cycle handoff by handoff, using Front to mechanize email SLAs that used to run on hope, and building what he calls a "customer brain" that gives every team the same context on every account. The conversation covers where coordination tax actually hides, how to tell performance from survival, and why the anecdotes matter as much as the dashboard.


## **Key takeaways**


-


**Visibility starts at the handoff.** Walk your order management cycle end-to-end — every handoff is where trust can break.


-


**Control means mechanisms, not intentions.** A mechanism is a process, a tool, and an audit loop. That’s what turns intent into practice.


-


**Confidence comes from matching data to anecdotes.** When the numbers and your best people’s sentiment disagree, trust the anecdote.


-


**Operational performance isn’t always headcount cuts.** The easy budget fix short-term costs you retention and your best people long-term.


-


**Structuring the unstructured data.** 80-90% of logistics data is scattered in multiple formats in multiple areas. Data management underpins a smoother operation for staff and AI alike.


-


**Onboarding and integration are separate jobs.** Run the workflow manually first, then build the integration once it’s proven.


## **Full transcript**


*Transcript has been edited for brevity and clarity.*


**Megan Beatty (host):** Hi everyone, and welcome to Untangled Ops, where we’ve gathered customer experience leaders who’ve turned operational snafus into success. My name is Megan, and I’m Head of Product here at Front, and I’m your host for this episode. We’re going to take a closer look at the work between the work that’s holding teams back as they help customers — the clunky handoffs and lost context — the coordination tax that’s often paid but rarely tracked. And while we chat about these complex workflows, our guest will be playing a not-so-complex game of Snake. It’s an honest representation of what operations looks like when coordination goes unmanaged: it just keeps growing, until it can’t.


We’re joined today by Ben Graeff, COO at FreightPlus, a managed transportation company purpose-built for mid-market shippers. Before that, Ben spent a decade in product management at Amazon. Ben, welcome to the show. Are you ready to play some Snake?


**Ben Graeff (guest):** Yeah, Megan, great to be here. I’m a little nervous about Snake, but we’ll figure it out.


**Megan:** It’s a tough one to multitask on. I’m excited to see how you do — I have it easier, just asking the questions. But I’m super excited to chat and learn more here. So, FreightPlus serves companies that have outgrown their logistical setup, but maybe aren’t big enough to have dedicated teams running it. Why is that specific moment — when you’ve outgrown the old way but haven’t built the new one yet — so operationally dangerous, from a client perspective?


**Ben:** I think this is multifaceted. One, our hypothesis is that the middle market in general has been left behind by "big" anything — big freight, big tech, big ERP. Our founding hypothesis was that we want to simplify complex logistics together — that’s our mission. And in doing so, we meet customers where they are in their growth pattern. We’re purpose-built for the messy middle market — from a data perspective, a people perspective, a tooling perspective, all of it built to meet these customers where they are.


Because often our customers are multi-generational companies going through a massive transformation — they were just purchased by private equity, or they’re a startup that’s grown 50x over the last few years. That’s really the pivot point where you start to crash into the wall. You’re failing in multiple areas — or maybe not failing, but holding on. That’s really our sweet spot: you’re holding on at the barrier to entry, which is pretty high. You need a transportation management system, you need all the integrations that come with it, you need the subject-matter expert. Just like your financials get more complex and you outsource accounting and taxes to a professional with the right tooling — that’s the break point where we feel our solution, and Front, is a critical part of it.


**Megan:** Yeah. So they’re at that tension point, and they need someone to come in and help untangle the complexity, it sounds like.


**Ben:** Yes. And I’d say — we call ourselves a managed transportation company, but at our ethos we’re a managed data company. A lot of what we do for clients in transportation — 80 to 90 percent, and this is where Front has been unbelievable — 80 to 90 percent of logistics data is actually fully unstructured. It’s a paper BOL, a lumper receipt, a PDF invoice, a "hey, I need a load moved today" email. All of that is unstructured, in a world of AI and data — and a lot of our executive team comes from Amazon, where it becomes really hard to manage a business without good structured data. So we’ve built the entire company around structuring unstructured data, to then simplify logistics.


**Megan:** Yeah, it’s about putting that structure in place and then letting operations flow through it over time. You mentioned a lot of folks at FreightPlus come from Amazon, and you yourself worked there for over a decade in product management. You’ve talked before about a Bezos principle you like: good intentions never work, mechanisms always do. What was the coordination problem at FreightPlus that proved to you this was such a great principle to hold onto, and how did you fix it?


**Ben:** I’ll use a Front example to talk through this. To expand on that Bezos, Amazon adage — basically, when something goes wrong, what you usually do is pull the team together, ask what happened, and then ask the team to try harder. But asking the team to try harder doesn’t work, because the team was already trying hard — they already had the best intentions. What you need instead is a mechanism. A mechanism is a process with a tool that drives adoption, plus an audit mechanism to make sure the process is working.


As an example, pre-Front, I was hoping my operational planners would say "good morning" in their emails — that’s a simple one. Or we were promising an SLA to a client — we will respond within 30 minutes, even if it’s just a receipt, not a resolution. And it was best intentions. Then the client comes and says, "you said 30 minutes, it took you 24 hours" — or two days. Unstructured data, and email particularly, is such an important part of the customer experience in transportation — not having that part of the experience mechanized was a major gap in our ability to fulfill our mission and drive client retention. So we started looking at: email is a critical part of our experience — what’s a tool that lets us mechanize that and actually put the good intentions into practice?


**Megan:** Yeah, totally. People need a framework to exercise their good intentions against. Otherwise it’s so hard to keep track of all the moving pieces and the inherent complexity of any problem — and I imagine especially logistics problems. So that totally makes sense.


**Ben:** The other thing people don’t often talk about is mechanisms being important for culture. When people can see, check, and manage to a number, they respond really well.


**Megan:** Yeah, no, that makes a ton of sense. Even internally, I see it with our own teams — when we give people process around inherently unstructured work, like product, which you’ve done yourself, we move so much faster with so much more rigor, and get better outcomes. So it’s cool to see you doing that for such a complex problem in the logistics space too.


**Megan:** All right, next question. You were talking about these mechanisms, and I know some of them have solved having context travel with a request across the various teams that touch these operations. Now you’re building something that gives a more holistic view of the various areas of customer management. Can you tell us more about what’s going into that, and some of the challenges you’re trying to solve?


**Ben:** We’ve done a ton of work on our overall service model to clients, which is really based on four foundational documents. There’s the MSA, which drives the scope of work, which drives the client SOP across the order management cycle — what that looks like. And then the client SLA is the scorecard: here’s the value we want to deliver across relationship, financials, service, and data — are we meeting that, and can we score each other in the partnership?


As we’ve evolved this — like anything, you do it manually to start, and then ask how to make it non-manual. In the world of AI, where data and context are super critical, we’ve started to look at Front as a critical tool here — for sentiment, CSAT, all of that. How do we build what I’m calling our "customer brain"? That means building knowledge bases on those foundational documents, and transcribing our calls, so actions aren’t missed — and using AI to understand verbal sentiment, the same way Front helps us understand written sentiment. That comes together to build a knowledge base that’s not just a knowledge base but updates those operating documents — making sure that if we committed to a deliverable, we’re delivering on it and have eyes on it.


What does that single pane of glass — the customer brain — look like? It’s the SLA metrics, the sentiment metrics powered by Front, the contract terms, when they’re renewing, the operational performance metrics. The intention — and we’re still in flight building this — is that anyone in the company, at any time, can come see what’s happening with a customer. Prepping for a QBR, an executive sync, a day-to-day ops meeting — everyone comes to a single page with context from a bunch of different sources.


**Megan:** Yeah, what you’re saying resonates so much, because I think now more than ever, having that single pane of glass is almost expected by the customer. They expect you to see everything they’re doing with your company, all the interactions, and have a deeper understanding of who they are — and that’s what helps you provide that high level of service, especially in relationship-driven businesses. So it’s really cool that you’re doing that.


**Ben:** Totally. It’s a work in progress, but a fun one — you see the team react like, "holy moly, this is making my life easier," which in turn makes them better at client service and relationships.


**Megan:** What’s one of your favorite pieces of feedback you’ve heard from the team on this so far?


**Ben:** "Oh my god, this used to take me two days and now I do it in two minutes."


**Megan:** That’s such an obvious time saving — a huge win. Love hearing things like that. You’re obviously a thought leader in this space. If a head of operations came to you and said their team is spending more time coordinating the problem than actually solving it, where would you tell them to look first, and why?


**Ben:** A mentor of mine from Amazon — she was the director of Prime when I was in product there — her name is Anna Collins, and she introduced me to the concept of the order management cycle. It’s from an old Harvard Business Review write-up on basic manufacturing, but I think it applies to any business. Effectively: unless you know, at the ground level, every single piece of coordination and every handoff that’s occurring, you can’t really fix the problem — you have to know the current state.


That’s what I tell every operational leader when they start running their own department: go read this article, go read our content on the order management cycle, and get very familiar with that process. Every time an order is handed off, a customer is handed off. So I’m handing off from sales, then into planning, then into carrier operations, then into AP, then into AR. At every single one of those handoffs, the customer is being handled — and every one is where you can lose customer trust. So to any operational leader — spend the first three to six weeks deeply understanding your order management cycle. And do it at the ground floor, not at ten thousand feet where you just build a flow diagram — it’s "then what happens, then what happens, then what happens." That’s where you start to see the coordination tax, and where you can simplify — which I think is our job as leaders. That’s the advice I’d give anyone trying to dig into a coordination problem.


**Megan:** And as you’ve dug into that problem at FreightPlus, what are the biggest unlocks you’ve seen — where are the sticking points, and what have you done to unstick some of the most friction-heavy points?


**Ben:** We’ve redone our entire OMC — so it’s a hard question to answer. Like you said, development happens so fast now — we’re evolving as a company every three months, but we stay grounded in that ground truth of the order management cycle.


I’d say where that process has been unbelievably effective is with our clients, as part of implementation. We do a full order management cycle from order to cash: what does it look like today, in your environment, before we operate a piece of freight? Then we walk through it — part of our ethos is we meet you where you are, so we’re not going to change anything at first. Megan, you’re emailing orders today? You’ll keep emailing FreightPlus orders. We’ll build trust with you, find the sticking points jointly, before we propose what a fully integrated digital experience looks like. We’ll operate that OMC for a period of time — once we’ve operated it, that’s when we do the integration.


We’re going to onboard and integrate you at the exact same time, which became kind of like waterfall development — you’re spending all your time on integration instead of talking through the workflow, instead of really operating it before you build the future state. Going through that OMC process, and enough bumpy implementations, you start to see — we had the cart before the horse. We’re not going to integrate immediately, even though that’s the end state we want. Agentic AI with Front has been very helpful here, because now I can honestly say: Megan, you’re emailing orders — email them to me, and it functions like an integration. The additional cost our team takes on is pretty negligible, but it means there’s no change management required of the customer to start. It’s a rip-and-replace — we can build that fully customized integration for clients in parallel.


**Megan:** Yeah, that’s awesome. I love so much of what you said there — both about, as a leader, being on the ground level with your team, really understanding the sticking points and where that heavy coordination tax is playing out, and then going through each one to really solve the friction points.


I must say, Ben, I’ve been watching you play Snake this whole time, and it’s quite impressive — the multitasking. I don’t know if I’d be able to hit the same level. Snake technically has no finish line, so maybe I’ll ask in that spirit: how do you know when your operation is actually performing, versus just surviving at a faster pace?


**Ben:** That’s a really interesting question, because as an operational leader you’re always managing to a budget and a P&L, and the easiest thing is to cut headcount, do more with less. But that leads to downstream ramifications — retention, losing great people, folks feeling like the company isn’t investing in them, and so on.


I think about it through a Pareto lens — in a lot of companies, 80 percent of the work is done by 20 percent of the people. Your big rock stars — you can tell from their sentiment, and I do this in a non-structured, non-mechanized way — how are the folks I know are my key players doing? Where’s their head at? That gives me a signal, when I’m not on the ground every day, that says, "okay, this person is stressed, and they’re not usually stressed." They’re usually dialed in, and now they’re not — so let me go interrogate that. That’s more of my approach to understanding whether we’re just hanging on by a thread.


Then the mechanisms — going back to mechanisms — are really important, because if you’re dutifully auditing your business with the right inputs, you start to understand lagging versus leading indicators. Leading indicators show you: we’re having degradation in our Front SLA, in our on-time pickup, in our tracking performance, in our ability to cover loads. Why is that? Let’s dig in. Those are the two things I use — the mechanism, and, another Bezos-ism, I must sound like an Amazon robot — when the data and the anecdotes don’t match, trust the anecdotes. You want to tie those two things together: how am I measuring employee sentiment and customer sentiment, and overlaying that with the KPIs I’m looking at every week, to understand — are we at a break point, do we need to go higher, do we need to reevaluate a step in the OMC because it’s becoming a big bottleneck?


**Megan:** Yeah, that makes so much sense. I love that matching of the mechanized and non-mechanized approach, because — like you said at the beginning — so much of what we’re doing here at Front is about putting structure to unstructured data, to help unstick and coordinate that complexity. But we also have products like CSAT and QA, because you can’t ignore what you’re hearing from people — both your customers and your best employees. You have to match those signals to get the full picture. It’s really cool to see how you’re operationalizing that within your own company too.


Well, that’s a wrap on this round of Untangled Ops. Thank you for joining us today and giving us a behind-the-scenes look at all the infrastructure you’ve built to solve the coordination overhead. So let’s see, Ben, where you’re ranked on the leaderboard... And it looks like you’re coming in at number one. Like I said at the beginning, it’s been so impressive to see you answering my questions and navigating this game at the same time — I can’t say I’d have the same focus. So, appreciate that for sure.


**Ben:** Thank you, Megan, and thank you, Front. You’ve been life-changing for our organization.


**Megan:** Amazing — we love to hear that, and we love having you and FreightPlus as a customer of ours. All right, everyone — for more conversations on wrangling unwieldy customer ops, subscribe to Front’s YouTube channel at FrontHQ. Thanks, everyone.


###### research


:


The Coordination Tax


The Coordination Tax quantifies the hidden cost of cross-team customer work. The report shows why teams spend nearly 3 hours coordinating for every hour solving customer problems — and what the top performers do differently.


[Download](https://front.com/resources/coordination-tax-report)
