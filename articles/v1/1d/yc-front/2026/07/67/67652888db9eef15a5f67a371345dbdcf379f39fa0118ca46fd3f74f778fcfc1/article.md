---
schema_version: "1.0.0"
document_id: "67652888db9eef15a5f67a371345dbdcf379f39fa0118ca46fd3f74f778fcfc1"
company_key: "yc-front"
company: "Front"
source_id: "yc-front-rss-d12a60119b76"
canonical_url: "https://front.com/blog/untangled-ops-episode1"
published_at: "2026-07-03T05:49:00+00:00"
first_seen_at: "2026-07-20T23:23:54.035202+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:a57a06415580f54705f8161f4c5eaafc9782fcbb2ccd2b6392d88ca4942692a0"
---

# Untangled Ops episode 1: Manychat’s Oleg Krasnov on running an AI-first support ops

Manychat cut 80% of its OKRs to experiment with AI. Watch what its Head of CX built next in episode 1 of Untangled Ops.


Every support ticket carries[a coordination tax](https://front.com/resources/coordination-tax-report) . Whether that tax is big or small indicates how well a support operation is running. We’re taking a closer look on[Untangled Ops](https://front.com/untangled-ops) , Front’s new interview series, pulling back the curtain on how real customer experience and operations leaders keep coordination overhead from eating their teams alive — while playing a retro game of Snake. Because sometimes the metaphor writes itself.


In episode one, host Alex Peattie, Senior Director of Engineering at Front, sits down with[Oleg Krasnov](https://www.linkedin.com/in/phlorid/) , Head of CX at Manychat, the messaging automation platform powering over 1M creators and brands. Oleg previously ran support through Miro’s hypergrowth years and also runs Robusta Consulting, advising other startups on CX and ops strategy. Under his watch, Manychat’s support org grew from 20 people handling 3,000 tickets a month to 90+ people handling 26,000 monthly conversations, with AI now resolving 70% of them.


## Key takeaways


-


**Fix the org chart before the AI roadmap.** Oleg built the headcount model and split ops into platform and self-service teams before automating anything.


-


**Average handling time is still the master metric.** Segment by volume and length first — that’s where coordination costs hide.


-


**The real AI win is zero-ticket resolution.** Catching a problem before a ticket exists beats resolving it faster.


-


**AI fluency is table stakes now.** Oleg used Claude Code to kill his own weekly reporting busywork.


-


**Ownership decides who pays the coordination tax.** On someone else’s API, the cost never disappears — it just moves to whoever’s stuck in the middle.


-


**AI removes the ticket, not the accountability.** Humans stay wherever a decision gets made or an outcome needs an owner.


-


**CSAT alone hides a fragile operation.** Pair it with leading indicators like SLA and resolution time, and measure efficiency, not just sentiment.


## Full transcript


*Transcript has been edited for brevity and clarity.*


**Alex Peattie (host):** Hey everyone, and welcome to Untangled Ops, where we’re gathering customer experience leaders who have turned operational snafus into successes. I’m Alex Peattie, Senior Director of Engineering here at Front, and I’m your host for this episode.


In this series, we’re going to be taking a closer look at the work between the work — which is dragging down teams as they try and help customers: clunky handoffs, lost context, this coordination tax that’s often paid but not tracked. And while we’re chatting about these complex workflows, our guest is going to be playing a slightly less complex game of Snake.


And we chose Snake because, as well as being a certified ’90s classic, it’s a great metaphor for what operations can look like when coordination goes unmanaged — this thing that just keeps growing until it can’t anymore.


So I’m thrilled to be joined today by Oleg Krasnov, who is the Head of CX at Manychat. Manychat is a platform serving over a million creators and brands who want to monetize conversations on their social media channels. Oleg is also an advisor at Robusta Consulting, where he helps startups with their CX and operational challenges, and he was also Head of Support at Miro during its hypergrowth stage.


So Oleg, welcome to the show. Are you ready to jump in and start playing some Snake with us?


**Oleg Krasnov (guest):** Yes, yes. Thank you for having me. So, let’s get it started.


**Alex:** All right, I’m going to put your multitasking to the test and hit you with our first question. At Manychat, you inherited a 20-person team dealing with about 3,000 tickets monthly. That’s now grown to more than 90 people, 26,000 monthly conversations, with AI resolving 70% of that. I definitely want to ask you about AI — but before we get there, even pre-AI, what were the building blocks you had to put in place, the foundational elements, just to prevent coordination overhead from eating your whole operation alive?


**Oleg:** Yeah, that’s a very good question. My first call was just to do the basics. Support wasn’t in the best shape when I arrived at Manychat. At first, I was doing a lot of stuff on my own — one of the first things I did was reworking the hiring process, compensation model, and career ladders. Closer to the end of the year, I built the headcount model. I pulled all the needed data, and based on the business’s projections, I said, okay, we’re going to double the function.


That meant I needed operations built as a function with multiple people, not just a manager. That was my first call, and it worked out well. Overall, we split into a platform ops team and a self-service team that handles the experience before a ticket is even created. All in all, that team is now roughly 20 people. So you can imagine that this was a big call for us to make sure that we can also take care of 90 people now and in the future. I’m a really big believer in leading with program-management ops generalists rather than just throwing bodies at the problem with agent capacity.


**Alex:** Super interesting. We did research recently on coordination tax, and found it’s very normal in B2B for every hour of actual problem-solving to come with about three hours of coordination time. That’s obviously a huge problem for businesses. What’s your advice for anyone dealing with this? Where does coordination get stuck, and how has your team gone about alleviating those bottlenecks?


**Oleg:** You need to start with metrics — usually the main metric here is average handling time. How much time does it take to handle a specific issue? Breaking that down into categories helps a lot. For smaller questions, the return on investment for optimizing usually isn’t that big. I’d rather look for the cases that are massive in volume, or the ones with the largest average handling time.


Once you have that split, first, you can be precise about headcount modeling and justifying the resources your team needs. Second, you understand what’s actually happening on every front when handling those tickets — whether it’s a technical issue or a coordination issue, like talking to engineering, legal, or whoever else. So you roll up your sleeves, dig in, understand the lifecycle of these tickets and how you usually handle them. But overall, I’d say: start with the data, understand average handling time and the cost — COGS, cost-per-ticket, all the financial metrics. That’s table stakes. Then figure out what you can pull to make the process lighter and smoother.


**Alex:** Super interesting, and I love how actionable your advice is. One thing you touched on is that not all tickets are created equal — some are smaller, more repetitive. I think one of the things you’re building at Manychat right now is a zero-ticket workflow, where AI can proactively detect a customer problem before it’s even flagged, and push the fix to engineering — so there’s basically no ticket or handoff, and things get resolved in minutes rather than hours or days. But I imagine you couldn’t fast-forward straight to that — there were operational pieces you had to put in place first. Tell me about that.


**Oleg:** It was more the realization of what’s happened with the AI space — everyone who’s played with Claude Code eventually has the same "aha" moment about what’s possible right now. Previously, I operated as a head of function. Now I feel like I’m CEO running a small services company, because previously, if you were a business owner needing an application or a website built, you’d need to find engineers, find designers, figure out what it should look like — and that would take months. Nowadays, you can do that in a couple of weekends.


That experience is exactly what led me to understand what’s possible from a customer experience perspective too. If you think about why every ticket gets created in the first place — on one hand you have the customer, on the other the product codebase. There are layers between them, for good reason — you don’t want to expose the codebase, there are security concerns, things can go sideways. So the traditional customer journey usually starts with confusion in the product, then the customer tries self-service — docs, chatbot, whatever — and eventually needs to reach a person: support, maybe an escalation tier, maybe QA, engineers, and so forth.


There are so many layers for the customer, and all of that is friction. AI can remove that friction because it solves exactly this operational overhead. Once I started playing with it myself and looking at how other functions were using AI, that became my end point for what’s possible. When I brought this to engineering, they were fully on board, and we’re just starting to build it.


**Alex:** Would you advise all CX leaders to lean into AI right away, or are there hygiene checks they should do on their operation first, before automating?


**Oleg:** Just being proficient with AI, the same way people were expected to be proficient with Microsoft Word back in the day. For example, one of my own use cases as a leader was automating redundant work — I used to do weekly reports for the execs, which took about 30 minutes copying and pasting numbers from Zendesk and other systems into a spreadsheet. One of the first things I built was automating exactly that with Claude Code.


Imagine how much time that saved over a year because of this one small change — and that’s just one example. Every leader has their own version of this: performance reviews, one-on-ones, meeting notes and context. There’s a lot you can optimize for yourself by default, regardless of team size or where you are in your product’s life. It’s becoming table stakes for every role, basically.


**Alex:** Staying on the AI theme — ownership comes up a lot in these conversations. You made an interesting point earlier that AI leverage works best when you actually own the system, because if a problem sits on a third-party API, the coordination cost sticks around. How should support ops leaders think about ownership and where they draw the lines?


**Oleg:** It depends a lot on the product. The reason I bring this up is that Manychat is a third-party-dependent product — we operate on top of APIs like Meta and TikTok, and whatever else becomes available. So we do our best to influence Meta in our favor, but realistically, trying to get a company that size to prioritize your issue among all their partners isn’t very achievable. Anyone operating on third-party infrastructure will run into this.


For example, sometimes people reach out to us because they didn’t get help from Meta, and we can help in certain situations — they’d rather talk to us than a huge company, even though technically it’s outside our scope. If it’s a technical issue, like a Metabans that’s fairly common, there’s often nothing we can do. Those cases will always exist as long as we run on third-party APIs.


Miro is the reverse example — they have third-party apps built on top of them. Back then, our capability to help customers there was very limited too, because we didn’t own that code and had no visibility into how it worked. We’d advise people to contact the app owner directly, or sometimes act as a go-between if it looked like a connectivity issue.


Anyone charging for software will hit some version of this — think banking limitations, where a bank blocks a payment and there’s nothing you can do about it directly. You do have a chance to optimize around it — AI workflows that ask the right questions, or handle refunds and cancellations automatically — but you’ll always have some edge cases.


The last piece I’d flag is prioritization. The reality of support is: you run your voice-of-the-customer session, your homework is solid, you’ve got the data, the financials, everything — and the head of product says, great work, we’ll prioritize this in March 2033. That’s the reality for basically every support leader. It’s maybe a bit easier for customer success and sales, because it’s easier to frame a blocked deal or potential churn of one million ARR than something that plays out long-term. You have to be okay with the fact that not everything gets prioritized by engineering or product, because no company has unlimited resources.


**Alex:** I’m sure every support leader can relate to that conversation at some point in their career. Speaking of common conversations — in this AI era, a lot of it is about headcount: can AI replace headcount, is it making us faster, are expectations rising as fast as efficiency? What’s your advice to support ops leaders trying to make the case for AI investment internally? How do they have an honest version of the AI ROI conversation?


**Oleg:** I need to give a bit of context on the company I work for first. Manychat is fairly unique in that we’re going all-in on AI as a company — that came from leadership, not from me pitching it. My manager, who’s an engineer by background, played with Claude Code and asked me: if I doubled your budget, but it could only be used for innovation, what would you do? That was a fairly luxurious position for me to be in. The product org was bold enough to cut 80% of Q2’s OKRs, along with a lot of meetings and rituals, just to give people space to experiment with AI and figure out what would work for their roles and teams.


That’s an important disclaimer — I wasn’t the one selling the idea internally, I was handed the opportunity. But zooming out to what AI actually does: humans are needed in two circumstances. First, when a decision needs to be made. Second, when accountability needs to sit with a person, because if something goes wrong, you can’t hold the AI accountable — that responsibility is still yours. So the starting point is figuring out where a human needs to stay in the loop under that model, and what that means for handling time or headcount.


The idea that AI frees up hands and can reduce headcount is correct, but it depends heavily on your situation. In a mature company that isn’t growing much, that might mean layoffs, or transitioning people into other roles — you can do more with less. In a growing company, it means you don’t need to hire as many people as you would have without AI, which is also a real benefit.


There are limitations too. With this new generation of agentic products, if it flags an error and proposes a fix, sometimes my reaction is "why me, just go fix it yourself" — and often it can. But users will test the limits of these systems, so you need to understand how the underlying model works, be able to debug and troubleshoot. Agentic experiences will also be highly customizable, which means sometimes you’re wearing a coaching hat — asking, what do you want, how do you measure success, how will you know it’s working. Those are much more psychological questions than support leaders have traditionally had to ask.


There’s also a more values-based question — imagine your product works exactly as intended, but the customer still isn’t satisfied. That’s a clash of perspectives: you believe you did your job, the customer disagrees. Those situations won’t get automated by AI, because they’re about relationship-building, not resolution.


The size of the business matters too. Manychat today is 100% product-led growth — we don’t have an enterprise motion yet. If we moved upmarket, those customers tend to be more conservative and traditional, and if you try to propose something built with AI, the answer is often closer to "let’s talk about this over dinner first" It’s very different, and it’s genuinely hard to generalize across all these scenarios — I’ve already given you several that are pretty distinct from each other. But if we go back to the core idea — there’s customer demand, and there’s the time needed to meet that demand — once you understand that math, that’s the way to think about it.


**Alex:** Yeah, that makes a lot of sense. And it leads nicely into my last question — how do you, as a support and ops leader, know when your operation is actually performing versus just surviving at a faster pace?


**Oleg:** Let me give you one practical example. When I joined Miro, the team was in a pretty immature stage. I wasn’t especially operationally skilled at the time either, but just by putting the right KPIs in place, hiring the right people, and building the right culture, we moved CSAT from 86% to 98% in six months — with none of the complexity of AI systems or elaborate process. Just culture, because I firmly believe happy support means happy customers.


At first glance, a 98% CSAT looks like "these guys are doing amazing." But when COVID hit and demand exploded — we went from 2,000 tickets a month to 2,000 tickets a week — that exposed operational flaws that was difficult to comprehend. I was basically covering up this mess for about half a year just to keep things running, and eventually CSAT dropped, along with other metrics, because we weren’t set up for that level of demand — not the processes, the tools, the operating model, or the data.


Some numbers will tell you how a specific ops leader is doing. But there are also softer signals: does this person know their numbers? Do they think ahead about the business? Do they use the company’s financial forecast? It’s an ongoing process, not something that ever really stops. It comes down to the metrics you’ve agreed on with leadership and how you measure them — not just lagging indicators like CSAT, but leading indicators like SLA, first response time, resolution time. And beyond that, the quality of the work itself, your own observations, the quality of your taxonomy, your relationships with other leaders.


It’s honestly a topic that deserves its own podcast — how do you measure the success of an operations function. But ultimately, I think it comes down to efficiency over time. Imagine two companies, roughly the same size, same stage of growth, same market — all else equal — and two heads of ops. One gets great results with a team of 30. The other gets the same results with a team of 15. That twofold difference says a lot. If the culture is healthy and people aren’t overwhelmed at 15, that’s a strong signal of how efficiently that leader is operating. Again — complex topic, hard to unpack in a couple of minutes, but if we’re scratching the surface, that would be my answer.


**Alex:** Well, I think you did more than scratch the surface — tons of genuinely useful insight there, and I really appreciate it. I think we can call that a wrap for this round of Untangled Ops. Thank you so much for kicking off the series and leaving us with a lot to think about on how to use AI in support ops. And the good news is you’ve shot straight to the top of the leaderboard with a very respectable score — 1,155 points. I was pretty impressed, you were playing that like a champion while giving us a ton of great insights.


**Oleg:** Thank you.


**Alex:** Definitely impressed by your multitasking skills. Thanks for watching — for more conversations on wrangling unwieldy customer ops, subscribe to Front’s YouTube channel @FrontHQ. And that’s it from us.


###### research


:


The Coordination Tax


The Coordination Tax quantifies the hidden cost of cross-team customer work. The report shows why teams spend nearly 3 hours coordinating for every hour solving customer problems — and what the top performers do differently.


[Download](https://front.com/resources/coordination-tax-report)
