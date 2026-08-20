---
schema_version: "1.0.0"
document_id: "5b2df593824e7661a4992872cb982105d4dd600dcc91d46a10848bd0155003e8"
company_key: "yc-embrace"
company: "Embrace"
source_id: "yc-embrace-rss-35f82ba9b7dd"
canonical_url: "https://embrace.io/blog/opentelemetry-explainer/"
published_at: "2026-06-17T16:27:13+00:00"
first_seen_at: "2026-07-20T23:20:15.781670+00:00"
fetched_at: "2026-07-28T21:10:58.759142+00:00"
content_hash: "sha256:220b87efcd4711995e8ec4d2f143419105396ec7eae33c1089964e65c6419351"
---

# What is OpenTelemetry? A plain-language guide for engineering teams

Ever watched your backend team, mobile team, and web team all investigate the same issue from three different dashboards? Then you already know the problem that OpenTelemetry solves. If you need an easy-to-understand explainer to share with folks who are new to OTel, this is for you.


I’m almost* embarrassed to admit that I’ve been with Embrace for six months, and I only recently started really digging into what the heck OpenTelemetry is, despite the fact that OTel is one of the pillars of our platform.


*I’m not quite actually embarrassed, due to the fact that (1) I’ve been kinda busy with[other exciting things](https://embrace.io/blog/core-mobile-vitals/) , and also (2) I’m kinda shameless.


Now that we’ve announced[our partnership with Honeycomb](https://embrace.io/blog/embrace-honeycomb-partnership/) – which is also built on OpenTelemetry – I realized I needed to get up to speed. Writing helps me learn, so I thought I’d get my thoughts out here. If you’re OTel-curious as well, join me on this journey!


## First, why should you care about OTel?


As I understand it, OpenTelemetry solves three big longstanding problems in the traditional observability space:


1. Frontend and backend teams don’t speak the same language.
2. Your data is held hostage by your vendor relationships.
3. Observability data isn’t connected to user experience and business outcomes. (This is the one I feel especially strongly about.)


Further down in this post we’ll walk through each of those problems and then look at some specific use cases where OTel really shines. Before we get into that, let’s get this question out of the way…


## How does OTel work?


Simply put, OpenTelemetry is an open standard for collecting and describing what I used to call “performance data” and what I’m now learning to call “telemetry”.


The best analogy I’ve heard is to think of OTel as a universal protocol… kind of like HTTP but for observability. When everyone instruments their software using OTel, the telemetry they produce looks the same, regardless of which tool they’re using or what layer of the stack they’re monitoring.


There are three main signal types:


- **Traces** – a record of how a request moved through your system
- **Metrics** – aggregated measurements like error rates or response times
- **Logs** – timestamped event records


(Do not be fooled by my authoritative-looking definitions. I grabbed those from some of[our awesome Embrace docs](https://embrace.io/docs/) . I get what metrics are, of course, but I’m still wrapping my head around traces and logs. For today’s purposes, those handwave-y definitions will suffice.)


The important thing to understand is that **OTel defines how all three of those signals are collected, named, and exported, so a trace from a mobile app and a trace from a backend service can actually talk to each other.** That’s huge!


The success of OTel is built on three principles:


- **Transparent** – no black-box proprietary formats
- **Portable** – you can switch tools without re-instrumenting everything
- **Extensible data collection** – you can add custom signals for your own app’s specific behavior


That sounds dry, so here’s a more exciting way to say it: **OpenTelemetry gives you unprecedented freedom to share data across teams and platforms** .


Now let’s talk about how that freedom helps you solve three common problems…


## Problem 1: Frontend and backend teams don't speak the same language


I don’t love new technology just because it’s new. I’m not that species of nerd. I’m the species that likes to solve human problems, so the best place to start is by understanding the problem that any new technology is trying to solve. Here’s my understanding of the history that led up to the current problem.


As I’m sure you already know (but bear with me), modern software runs on two layers:


- **Server-side** – the databases, APIs, and infrastructure that your backend and SRE teams watch over
- **Client-side** – the mobile apps and browsers where real users actually experience your product, which your frontend performance team typically watches over


Both sides generate data about how things are performing. The trouble is, these are usually entirely separate tools that don’t share a common set of telemetry and don’t connect in any meaningful way. Because of this, engineering teams aren’t able to speak the same language.


The result is a mess. What a surprise.


When something goes wrong – say, a checkout flow starts failing – here’s what different folks see:


- Your backend team sees a **spike in server errors.**
- Your mobile team sees a **crash signal.**
- Your frontend web team sees that **Interaction to Next Paint is hurting.**


Each team is working from different datasets, using different tools and completely different mental models. That’s a huge disconnect!


Everyone is trying to connect dots they realistically can’t be expected to connect, and it’s not fair to expect them to. Site reliability teams are forced to guesstimate user and business impact from service and infrastructure metrics. Developer teams are tasked with magically solving issues that are out of their control. And so on.


One of the widely touted advantages of OpenTelemetry is that it connects your whole stack. Great, I guess?


But TBH “connect your whole stack” is one of those expressions that doesn’t land for me. (Like I said, I’m not that species of nerd.) **The real win is that OTel connects your teams.** In doing that, it helps everyone work together to help your users. And ultimately, helping your users helps your business.


OTel isn’t about tech for the sake of tech. It’s about people. (There’s a Soylent Green joke in there somewhere, I know it.) I care about people, and that’s why I think OTel is important.


## Problem 2: Your data is held hostage by your vendor relationships


Before OTel, every observability vendor had their own agent, their own data format, and their own proprietary schema. Traditional vendors favored closed ecosystems and opaque pricing.


That sounds bad because it is bad, but for a long time no one called it bad because that was just the way the tools worked. (To be clear, I’m not accusing any vendors of unsavory intent. They were just following standard practice.)


If you wanted to switch vendors, you had to rip out your instrumentation and start over. Ugh. Not only was that hideously expensive, but even if you exported your data, there was always the risk of losing or compromising it.


OpenTelemetry means your data is no longer held hostage by your vendor relationships.


Your team doesn’t have to re-instrument when changing vendors. You write your instrumentation once and it works with any OTel-compatible backend – Honeycomb, Grafana, or whatever you choose.


## Problem 3: Your data doesn’t connect to your users and your business


I saved my favorite topic for last. If you care about your users and your business (and please, I hope you do), this is for you.


Traditional observability tools were built to watch servers, not people. They can tell you that a service had increased latency at 2pm, but they can’t tell you that this was the fallout:


- 12,000 users experienced a frozen checkout button
- Android users suffered the most
- Your conversion rate dropped by 6%


That’s because user experiences are messy in ways that server logs aren’t. A backend request takes milliseconds. A user session can take minutes, or even hours, winding through a dozen interactions on a device you’ve never tested on a network you can’t control.


OTel connects your servers, your mobile apps, and your web frontend into a single shared view. When that view also includes user engagement and business metrics, then you get the full, glorious technicolor picture. It’s the difference between monitoring your infrastructure and understanding your users.


## Use cases where OTel makes a difference


I’ve covered these already as problems, so now let’s reframe them as solutions. As you consider these, it becomes pretty clear that these use cases all build on each other: from communication to diagnosis to prioritization based on business impact. It’s a beautiful thing.


### 1. Break down team silos


Frontend and backend teams need to speak the same language and access the same datasets to draw insights from user experiences. When both teams’ tools are built on OTel, a mobile engineer and an SRE can open the same trace, look at the same data, and have a productive conversation about what broke and where.


### 2. Diagnose an issue across the stack


Without OTel, a backend engineer investigating a high error rate has no visibility into what users were actually doing when those errors hit. With OTel-connected frontend data flowing into the same system, they can see that the errors coincided with a new app version rollout, which affected users on a specific device type. The root cause becomes findable.


### 3. Prioritize what to fix based on user impact


We all want to prioritize issues by understanding actual user impact, which is only possible by connecting backend observability data directly to end-user experiences. OTel makes that connection possible. Not every p99 latency spike is worth an all-hands page, but if that spike is hitting your highest-value users during checkout, it absolutely is!


### 4. Switch vendors without starting over


Because OTel is an open standard, your instrumentation isn’t tied to any particular vendor. If you switch tools, your data comes with you, so you’re not starting from scratch.


## OTel isn't glamorous – it's plumbing


Plumbing may not be cool, but we all know what happens when it doesn’t work. In this case, it’s the plumbing that connects what’s happening in your servers to what’s happening for your users. That’s a gap that’s existed for a long time. If you want to be a hero in your organization, grab your wrench and close that gap.


## Keep reading


- [Why Embrace is leading the way in OpenTelemetry for mobile](https://embrace.io/blog/why-embrace-opentelemetry-mobile/)
- [Embrace and Honeycomb: Connecting user experience to backend truth](https://embrace.io/blog/embrace-and-honeycomb/)
- [Send your mobile and web telemetry wherever you need it](https://embrace.io/product/opentelemetry-integration/)


Deliver incredible mobile experiences with Embrace.


Get started today with 1 million free user sessions.


[Get started free](https://dash.embrace.io/signup/)


Author


[Tammy Everts](https://embrace.io/author/tammy-evertsembrace-io/) Tammy is a UX researcher, writer, and speaker who has spent more than two decades studying how people use the web. Her focus is the intersection of performance, user experience, and business outcomes. She's helped some of the biggest companies in the world – from Amazon to Zillow – deliver fast, joyous experiences to their users. Tammy is the author of 'Time Is Money: The Business Value of Web Performance' (O’Reilly) and co-chair of the annual performance.now() conference.


Related Content


OpenTelemetry


8 May 2026


• 6 min read


### [Browser OpenTelemetry without the compromises: How to stay vendor-neutral and still get full RUM](https://embrace.io/blog/browser-opentelemetry-without-compromises/)


A real-world React SPA setup surfaced a question about browser OTel. The answer points to a better way to instrument the web.


OpenTelemetry


30 April 2026


• 3 min read


### [I spent six years thinking I was bad at mobile observability. I wasn’t.](https://embrace.io/blog/i-spent-six-years-thinking-i-was-bad-at-mobile-observability-i-wasnt/)


It took me joining Embrace to figure out I wasn’t totally incompetent at mobile observability. The tools I was using just weren’t making it easy.


OpenTelemetry


4 January 2026


• 6 min read


### [10 Best OpenTelemetry tools in 2026](https://embrace.io/blog/best-opentelemetry-tools/)


Discover the best OpenTelemetry tools of 2026 to unify observability, speed up root cause analysis, and optimize performance across modern apps.
