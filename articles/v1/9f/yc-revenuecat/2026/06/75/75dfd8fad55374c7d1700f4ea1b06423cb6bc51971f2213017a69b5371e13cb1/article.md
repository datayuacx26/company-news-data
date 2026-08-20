---
schema_version: "1.0.0"
document_id: "75dfd8fad55374c7d1700f4ea1b06423cb6bc51971f2213017a69b5371e13cb1"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/growth/austin-blake-stuff-launched-podcast-2026"
published_at: "2026-06-03T12:47:46+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:8679496525fa81e564fe4f4d2a6fa23b3589a08803afa126c8a9083215e39195"
---

# Why you should build another to-do list app (with a little help from Claude)

The conventional wisdom in the indie developer community is clear: do not build a to-do list app. It is the ultimate crowded market. It is the “hello world” of app development, a space dominated by Apple’s native Reminders, venture-backed giants, and a million abandoned weekend projects.


But when Austin Blake decided to go all-in on his indie career, he dropped five other projects to focus exclusively on Stuff, his aggressively delightful task management app.


“People will tell you that you can’t compete in a space,” Austin says. “And I’ve gotten tons of emails and feedback, some of which are pretty harsh. Like, ‘We don’t need another to-do list app, why are you wasting your time on this?’ But there’s always room for additional competitors. You just have to commit to it and spend time on it.”


In a conversation with Launched host Charlie Chapman, Austin shares how he built a native Mac app using AI agents, why he cut his free trial by 75%, and the brutal realities of launching on macOS.


## The “vibe coding” workflow: Letting AI agents peer-review each other


Austin had never built a Mac app before. He started the traditional way—buying Paul Hudson’s Hacking with Swift: macOS Edition—but quickly pivoted to a highly modern, AI-driven workflow. He didn’t just ask Claude to write code; he built a multi-agent system to write and review his development plans.


“I make plans for everything, and then pass them between the agents and have them review each other’s plans,” Austin explains. “And then I review all of the code before it goes in.”


By letting one AI agent generate a technical plan, another critique it, and a third write the code, Austin bypassed the steep learning curve of AppKit and macOS-specific APIs. This collaborative loop saved him hundreds of hours of manual troubleshooting.


“Just the amount of things that I didn’t have to spend hours learning how to do on the Mac, and just letting that give me a first-pass prototype, saved me a ton of time and honestly let the app come out sooner than it would have.”


## The hidden design differences between iPad and Mac


Porting an iOS or iPadOS app to macOS is rarely as simple as checking a box in Xcode. Even though modern iPads support larger screens, the user expectations on desktop are entirely different.


One major realization was the concept of selection states. On an iPhone, you tap a task and it immediately opens. On a Mac, users expect to navigate using arrow keys, highlight a task without opening it, and press Enter to expand the details.


“There’s this intermediate state that I had to add,” Austin says. “And then there are things like undo. Check a task complete and being able to undo it with Command-Z—that’s something you just expect to work. On iPhone, people don’t shake their phone violently to undo a task, but on Mac, it has to be a thing.”


Furthermore, while mobile design focuses on making full-screen experiences look good, desktop design requires optimizing for the incredibly small. Desktop users rarely full-screen their notes or reminders; they want to shrink them into tiny pockets of information pinned to the corner of their screen.


“You can make Stuff windows pretty dang small to the point where you could just have your monthly goals sitting up in a tiny box in the corner of the screen. There are a lot of affordances like that on Mac that make productivity apps in particular very fun to design for.”


## The 7-day trial experiment: Why less time means more conversions


Like many subscription-based apps, Stuff initially launched with a one-month free trial. It felt generous, giving users plenty of time to experience the app. But Austin noticed a massive drop-off. With 30 days on the clock, users lacked the urgency to build a daily habit, and eventually forgot about the app entirely.


Without complex A/B testing tools, Austin trusted his gut and slashed the trial period down to seven days.


“I am a very bad tester, I’ll be honest. Everything I do is basically through gut,” Austin admits. “I saw a lot of drop-offs with trials, and I said, ‘One month is probably too long, I’ll drop it down to one week.’ And I did see a rise in conversions.”


Slashing the trial length kept the app top-of-mind. Users had to actively decide whether Stuff fit into their workflow while their initial excitement was still high, leading to a direct bump in paid subscriptions.


## The App Store review trap: Budget three weeks, not three days


When Austin prepared to launch the Mac version of Stuff, he gave himself what he thought was a generous buffer: he submitted the app for review a full week before the scheduled launch date.


It wasn’t enough. The app was rejected multiple times—not for new Mac features, but for existing iOS features that had been approved in the App Store for years.


“I was rejected for a lot of different things over the Mac app, which are part of the reason it took so long,” Austin says. “But all of those things were already present on the iOS app. It was just me saying, ‘Hey, this is already approved. We’ve already talked about this.’ It was a lot of back and forth.”


Austin ended up launching three days later than planned, disrupting his coordinated marketing push. His advice for anyone launching a new desktop app? “Next time, I would give it honestly, probably three weeks. Just to give you plenty of time.”


In[the full episode](https://www.youtube.com/watch?v=cohtMYKXKYE) , Austin also talks about his path from a BYU film major to working as a contractor at Apple, how Stuff was featured in the Apple Newsroom for its on-device foundation models, and his transition to joining the RevenueCat team as a Developer Advocate.


## Guest links:


- [Austin Blake on X/Twitter](https://x.com/austinstuff)
- [Stuff on the App Store](https://apps.apple.com/us/app/stuff-task-management/id1663249463)
