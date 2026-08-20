---
schema_version: "1.0.0"
document_id: "2f68c09ba6725bb358aaeef57433699bf751e442184ab87c056c304bdcfc77e1"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2020-08-19-howworkingasaproductownerhelpedmebeabett/"
published_at: "2020-08-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:26:40.232877+00:00"
content_hash: "sha256:ca876c90aa4d5ed3bcc66d481fdba8fa831c5f533321a4a49af42cc6ba3e9df7"
---

# How Working as a Product Owner Helped Me Be a Better Engineer (and vice versa)

Over the past few months, I was given the opportunity to try out the life of a Product Owner (PO), alongside retaining my responsibilities as an engineer. The life of a PO has always intrigued me since I joined trivago 2 years ago, and I always found myself unofficially taking on roles that were traditionally done by them. Things like reaching out to stakeholders for collaboration, thinking about KPIs and impact, and general “aligning”. Perhaps it’s because I simply love the sound of my own voice, but I’ve always felt a particularly high level of gratification from contributing in meetings. “Aligning” is an overused word in the workplace, but it is the best to describe where I derive my professional gratification from, outside of building things with code.


So like with most things, trivago was the perfect place to try out something I’d never done before. I made my intentions clear to my responsibility chain and sure enough, I was eventually given the opportunity to run an initiative as both the PO and sole engineer of the project. I was given a simple vision statement, to find “A method of conveying to the user that there is a cheaper deal within a few days from their selected date.” After a quarter of working with design, building the data pipeline, and thinking about every relevant KPI, we ended up with a price graph in our overview section. We tested it with over 2 million people, saw the potential for a positive impact and therefore decided to continue iterating on it this quarter!


The best talent is the one that has the full context of every single aspect of the company, from Creative Production all the way to Accounting. Of course, that talent would also need to work about 72 hours per day to get close to that level of experience, but it’s no surprise that some of our most skilled talents at trivago have worked in so many[different parts of the company](https://life.trivago.com/humans/7-things-i-learned-when-i-stepped-away-from-a-leadership-position.html) . Understanding how things work outside the scope of your job description is always a beneficial tool to keep on your belt. So what better role to start learning in depth than one of your closest co-worker?


## How working as a PO helped me be a better engineer


When you throw the lengthy job description of a Product Owner into a blender and then strain it through a coffee filter, what comes out is one idea – understanding the user problem. All other responsibilities of a PO are auxiliary to this primary objective. This was something I struggled with in the very beginning when I began trying to practice my PO abilities.


As an engineer, we base our whole career on solutions, solutions and solutions. We are evaluated on how well designed our solutions are, we comment on each other’s Pull Requests to advise each other on how to improve our solutions, and we safely ignore the wider context and implications of what we are helping build. This isn’t inherently bad, but understanding the context definitely makes you better.


When I sat through design sprints, I always hated the part in the beginning where we would spend 30 minutes defining the problem. Why waste all this time pedantically defining the problem when we could be working towards the solution? What I didn’t quite grasp at the time was that if you don’t figure out the exact problem, you will waste time solving the wrong thing. Truly understanding the problem, will make every solution obvious. Because you become the user. And the user knows what they want.


I have had the great fortune of working with some fantastic POs since joining trivago. One thing that always impressed me was when I approached them with a question about how the feature should behave in some obscure scenario, they always seemed to know the answer after only a second of thinking. But what I didn’t realise was when you live and breathe the user’s problem day in and day out, it’s not hard to think of the exact right behaviour for a given scenario. It’s a skill, just like any skill, that is built up over time and experience.


I didn’t even realise I myself had begun building this intuition until after my first three month stint as a PO was over and I was working as an engineer on a logging task for another PO. The task required me to log a user’s click on a new feature once per session. The QA found this strange and asked if it was a mistake in the requirements and whether it should be logged once per search instead. My answer came quick and without hesitation: “The once per session will be useful for Penetration metrics. We don’t necessarily care how many times it is clicked, or if they click more after changing search. We just want to see how many people who see the feature, end up clicking on it at any point.” This was supported by the PO once they had read the message.


In that answer, I had put aside my engineer brain, which was just repeating “I’m just following the spec” over and over again, and flexed my PO muscle that looked at the user problem and saw why we only needed a single logging per session. And just like any muscle, it can only grow when trained slowly and consistently over a period of time.


With only three months of experience, I have managed to save plenty of time avoiding conversations that normally would’ve happened for the sake of “alignment”. Now, the answers were obvious. By spending time better understanding the problem, I don’t need to ask as many questions anymore. The benefits don’t stop there however, in the next part, I’m going to describe…


## How working as an engineer helped me be a better PO


Coming into the PO role as an engineer felt like I walked into an exam with a cheat sheet of secret engineering hand signals that we use to communicate. Like I could speak a language that many other POs can’t. And to an extent, it isn’t far from the truth. The engineering lingo is heavy, and just understanding the words isn’t always enough, understanding how they connect is an equally large part of the puzzle. I can’t count the number of times that a PO I’ve worked with has had to just nod and trust me when I explain why we “need to pipe the data through X instead of Y, and Z doesn’t even connect to X in that way and… it’s hard to explain.”


It’s no surprise that understanding the details of how the system works helps on a daily, if not hourly basis. The best example of this would be when I was trying to request a new data stream from our data team for the feature I was building. The thing about trivago is, we have a lot of hotels. A lot. And when experimenting with building features, it isn’t necessary to test the feature on every single hotel because then we’d need data for every single hotel. And all that data needs to be stored somewhere. This is how the question arose from the data team, “how many hotels do you want us to include?”


What would’ve normally been a few alignment meetings between the PO, data team and engineer, was reduced to just a small Slack conversation. I knew just how I wanted to structure and store each key, I estimated the size each key would take, and subsequently found the exact amount of keys (and therefore hotels) we can store within our size restriction. All this happening within an hour of being asked the question.


This was an extreme example of how my engineering experience saved us from a lot of conversations and alignment meetings that would normally be necessary. But this knowledge still applies all across the spectrum. It is far easier to create a shipment plan when I can personally guesstimate the work load and complexity of a given feature. What could look easy may actually be very hard simply because of where it sits in the codebase. Likewise, what may look like a difficult feature could actually be quite trivial. Just having a surface level understanding of the code base’s structure, how the data is stored, and where it comes from, can already give you heaps of context off of which you can incorporate into the PO cycle.


Another thing that being an engineer has given me insight into is something I’ll call the “engineer grump”. If you’ve ever seen an engineer completely apathetic to something you’re saying, you may have been a victim of the Engineer Grump™. This phenomenon occurs when an engineer goes through multiple quarters of half-finished features that never get a second iteration. The PO shows up with a three quarter long plan about how they are going to change everything, the engineer gets enthusiastic and motivated, only to switch to a completely different project next quarter becomes “priorities changed”. Working as a PO, the diminishing returns of a feature are obvious. The more you work on it, the less value you’ll be able to extract from it. However, this was not something that came to me intuitively when I started as a PO. Engineers work with edge cases all the time. From thinking about every line of code and its consequences, to working with QA on the most obscure bugs. Fixing everything and making everything as close to perfect as possible is the only point where code becomes acceptable.


Whereas for a PO, the whole point is to make high impact change, after high impact change. Fixing niche use cases is not a valuable use of time when there are a hundred other high impact features that address a significantly larger audience’s problems. The engineer doesn’t have that context by default. All they see is a really cool feature, that never really got finished. All they see are the bugs they missed. And the feature will sit there, as a testament to the engineer’s shortcomings. Never getting a chance to fix it.


Coming to the world of being a PO as an engineer has really shone a light on what was previously really confusing behaviour. It explained why a PO could be so hopeful for a feature one quarter, only to change to a completely different tune the next. And coming with engineering knowledge has definitely been a major boon to my capabilities.


---


With all that being said, I am not advocating that one becomes a “jack of all trades”. Mostly because I still strongly believe in the second part of the phrase, “jack of all trades, master of none.” I am militantly against putting my brain power towards anything design related. The best example of this was when I was in high school and I designed a website with a purple background and blue text and thought it looked good. Clearly I have a long way to go before I become even halfway decent with design. I prefer to leave the cognitive load of thinking about the design to the people who know it best, the designers. I only have so much brain power and I would personally prefer to use it to deep dive into a couple of topics. What I do advocate is truly spending some time and effort on understanding your closest stakeholder’s role. The value from that effort comes quicker than you’d think.


The experiences I have gained from just a few months have been invaluable to say the least. What I once found frustrating about certain PO behaviour, I now understand and agree with. What I once found confusing about feature behavior, I now have a more intuitive knowledge for. I have been able to apply the things I’ve learnt over the past few months on a daily basis. And at the end of all this, the next time you scroll through trivago, I hope you take a second to look at the little feature that I poured my heart and soul into, before scrolling on.


Thank you for reading :)
