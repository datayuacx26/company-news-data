---
schema_version: "1.0.0"
document_id: "339740babc5fa5c8c23d2e1050461063cc154270984b0c6476e7bba9a93c8031"
company_key: "yc-embrace"
company: "Embrace"
source_id: "yc-embrace-rss-35f82ba9b7dd"
canonical_url: "https://embrace.io/blog/reliability-user-experience/"
published_at: "2026-06-09T23:22:04+00:00"
first_seen_at: "2026-07-20T23:20:15.781670+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:28d41270bebd3db82d9e7ff4646a6a60b81c132b5905dc5686daa18c31fb6e42"
---

# Putting users first: What does “reliability” mean today?

Ask an SRE what "reliability" means and you'll get a pretty consistent answer. It means uptime, latency SLOs, error budgets, on-call rotations, and someone's pager going off during a dinner party. (I know, no one carries pagers any more. I’m just enjoying an anachronistic moment.) None of that is wrong. But it's incomplete, and that incompleteness may be hurting your users and your business.


Here’s a scenario. Your infrastructure is humming along beautifully. Uptime is 99.9%. Your SLOs are green across the board. Your error budget is intact and no one has been paged in two weeks. You are, by every traditional measure of reliability, crushing it.


And yet.. users are abandoning your app after two seconds on the loading screen. Scrolling feels choppy and weird. Taps on buttons feel laggy. The app crashes, not frequently, but just frequently enough that a certain percentage of your users – the ones who’ve experienced it – have uninstalled and moved on.


You are, simultaneously, completely reliable and completely failing your users.


## What "reliable" actually means to a user


When a person opens your app, they’re not running a mental checklist of your infrastructure health. They’re a normal human being just trying to do a thing. In those first few seconds, they’re making a series of unconscious assessments, such as…


- Is this fast?
- Does this feel smooth?
- When I tap something, does anything happen?


These aren’t soft, subjective impressions. They’re measurable, and they map to the business metrics your stakeholders actually care about. There are four of them, and together they describe what “reliable” means from the user’s perspective:


### Screen Load: How fast the first screen of your app renders after launch


This is the moment that determines whether users stick around or bail. A slow screen load isn’t just a performance problem, it’s a first impression problem. And you know what they say about first impressions. (They’ve been proven to form in 50 milliseconds on the web, by the way. Do you think mobile is any different?)


### Smoothness: How fluid your animations and transitions feel


Frames drop. Scrolling feels janky. Individual users can’t always articulate what’s wrong, but they just know the app feels off, and off makes people trust things less. It’s like a restaurant where the service is technically fine but nothing quite flows right and you can’t completely relax and enjoy your meal. You can’t put your finger on it, but you don’t make a reservation there again.


### Responsiveness: How quickly the app reacts when a user taps something


Mobile users are impatient in a way that desktop users aren’t, because tapping feels more immediate than clicking. A delayed response feels less like latency and more like the app is ignoring you. Nobody likes to be ignored.


### Stability: Whether or not the app crashes


This one sounds like SRE territory, and it is, but it’s worth sitting with the fact that a crash from a user’s perspective is very different from a crash in your error monitoring dashboard. The user doesn’t know or care whether it was a null pointer or a third-party SDK. They know the app died while they were trying to pay for something, or log a workout, or send a message. And a certain percentage of those users won’t come back.


Interesting side note: A user’s definition of a crash can be wildly different than a dev’s. I’ve heard people refer to something as a crash when:


- The app hung (still on screen, but not responding)
- They clicked a button and nothing happened
- It genuinely crashed


So it seems like for some percentage of the user population, the term “crash” really means something more like “the app didn’t let me do what I wanted to do”.


Getting back to the main topic…


Together, these four pillars describe what it actually feels like to use your app reliably. Your infrastructure can be perfectly healthy while each of these UX indicators is, frankly, terrible.


### "Isn't this a product problem?"


Maybe you’re thinking: this sounds like product’s job, not mine.


I’m going to push back hard on that. And I’m pushing back not just because I think you’re wrong, but because ceding this ground is bad for you.


The SRE conversation with the rest of the business has always had a bit of a visibility problem. Reliability work is mostly invisible when it’s going well. You prevent bad things from happening, and then the bad things don’t happen, and nobody notices. This makes it hard to articulate your team’s value to non-technical stakeholders, to justify investment and get headcount.


User experience metrics change this. Screen load times, smoothness, responsiveness, and stability connect directly to numbers that do get noticed:


- Slow screen loads correlate with session abandonment.
- Choppy animations correlate with lower session depth.
- Crashes correlate with uninstalls.


These aren’t hand-wavey vibes. They’re measurable.


We’ve been making this argument over in the web performance world for years. The data on the web side is pretty damning:[page slowdowns can have twice the revenue impact of full outages](https://www.speedcurve.com/blog/downtime-vs-slowtime/) , largely because they happen ten times more frequently. Outages make headlines. Slowdowns hurt your metrics for months, until your retention numbers tell a story nobody wants to hear.


The same dynamic is playing out in mobile apps, just without the standardized measurement framework that grabs people’s attention. That’s what[Core Mobile Vitals](https://embrace.io/blog/core-mobile-vitals/) – Screen Load, Smoothness, Responsiveness, Stability – are designed to fix.


## "So what?"


If you want to drive investment in your reliability work – to get actual resources from the people who control the budgets – you need to be able to answer one big question: so what?


Here’s how that conversation can go, once you have the data:


*“Our screen load times are degrading. We’ve tracked a direct correlation between screen load time and session length: users who wait more than three seconds at launch have 40% shorter sessions on average. We’re losing engagement. Here’s what it costs us, in rough numbers, and here’s what it would take to fix it.”*


That’s a different conversation than *“Our p99 latency is trending in the wrong direction.”*


The first conversation prompts an awesome discussion about getting more resources for your observability team. The other conversation triggers a long, tedious side quest where you need to explain what “p99 latency” means, and you hope the person you’re explaining it to doesn’t make an excuse to bail on the conversation before you get back to your main point.


The key move is connecting user experience metrics to business metrics. **Even a rough correlation is compelling when the alternative is “we have no idea”.** After you’ve made that connection, you’ve done something that makes you considerably more effective regardless of your role: you’ve given the people who fund your work a reason to care.


## How to start making the shift


If you’re an SRE or engineering leader who wants to expand your organization’s definition of reliability to include user experience, the path forward isn’t complicated. But it does require doing three things deliberately.


### First, instrument for the user experience, not just the infrastructure


You almost certainly already have infrastructure monitoring. You may have crash reporting. But do you have visibility into screen load times, smoothness, responsiveness, and stability across your real user population? If not, that’s the gap. (This is not a small gap. You can’t improve what you can’t see, and you definitely can’t make a business case for improving what you can’t measure.)


### Second, find the correlation


Take whatever user experience data you have and map it to a business metric: session length, seven-day retention, conversion rate, whatever is most compelling to your organization. Even directional data is useful. To be absolutely clear, you’re not trying to prove causation in a randomized controlled trial. You’re trying to show enough relationship between reliability metrics and your business that investment decisions become easier to discuss.


### Third, make user experience part of your SLO story


SLOs are a great tool that are underused when it comes to user-facing metrics. There’s no reason your error budget framework can’t include a screen load budget or a responsiveness target. These don’t replace your existing uptime and latency SLOs. They extend them, and in doing so, they extend the conversation you’re able to have about why your work matters.


## TL;DR


SREs care about building systems that work. Users care about apps that feel good to use. For a long time, those two things have been measured separately, optimized separately, and reported separately.


They don’t have to be. In my experience, the organizations that connect reliability with user experience in a meaningful way are both better at retaining users and better at making the case internally for why reliability work deserves investment.


Also: if making your app more reliable doesn’t help your business, it’s worth asking why you’re doing it. That question is uncomfortable, especially if you’ve been doing things the same way for a long time, but it’s a useful one. With the right metrics, there’s an answer, and you get to be the person who discovers it.


## Keep reading


- [When business metrics drop but engineering can’t explain why](https://embrace.io/blog/when-business-metrics-drop-but-engineering-cant-explain-why/)
- [5 tips for designing highly effective mobile SLOs](https://embrace.io/blog/5-tips-for-designing-highly-effective-mobile-slos/)
- [Why engineering orgs should be thinking about mobile SLOs: A RedMonk conversation with Embrace president Andrew Tunall](https://embrace.io/blog/redmonk-embrace-mobile-slos/)


Deliver incredible mobile experiences with Embrace.


Get started today with 1 million free user sessions.


[Get started free](https://dash.embrace.io/signup/)


Author


[Tammy Everts](https://embrace.io/author/tammy-evertsembrace-io/) Tammy is a UX researcher, writer, and speaker who has spent more than two decades studying how people use the web. Her focus is the intersection of performance, user experience, and business outcomes. She's helped some of the biggest companies in the world – from Amazon to Zillow – deliver fast, joyous experiences to their users. Tammy is the author of 'Time Is Money: The Business Value of Web Performance' (O’Reilly) and co-chair of the annual performance.now() conference.


Related Content


App Performance


7 March 2026


• 6 min read


### [Best practices for optimizing your mobile applications](https://embrace.io/blog/mobile-app-optimization-best-practices/)


Use these 6 best practices to optimize your mobile app for the best performance and experience.


App Performance


9 February 2026


• 6 min read


### [What is a bug bash?](https://embrace.io/blog/what-is-bug-bash/)


Discover what a bug bash is, its benefits, and how to conduct one from start to finish.


App Performance


6 February 2026


• 13 min read


### [Top React Native performance monitoring tools for 2026](https://embrace.io/blog/top-react-native-performance-monitoring-tools/)


After successfully launching a React Native app, you need a quality performance monitoring tool to keep your app pristine. Our guide will help you choose the right tool.
