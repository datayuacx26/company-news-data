---
schema_version: "1.0.0"
document_id: "8f6c8c74d375944b53604f2ac97f924755b2b6fafd2775ed83de69bfd5df0210"
company_key: "yc-embrace"
company: "Embrace"
source_id: "yc-embrace-rss-35f82ba9b7dd"
canonical_url: "https://embrace.io/blog/devs-owned-pre-prod-sres-owned-production-nobody-owned-the-gap/"
published_at: "2026-05-15T16:00:01+00:00"
first_seen_at: "2026-07-20T23:20:15.781670+00:00"
fetched_at: "2026-07-28T21:15:16.761066+00:00"
content_hash: "sha256:15256d8b8acb6528b2c881b5b0721d94b210623d1b68b383f2035c34f790a366"
---

# Devs owned pre-prod. SREs owned production. Nobody owned the gap.

I want to tell you about a phrase I started using years ago that I'm not proud of: "mean time to innocence."


It described something I was seeing constantly. A performance regression would ship to production. SREs would catch it. And the first thing developers would say — genuinely, not defensively — was: “It looked fine in pre-prod.” They weren’t wrong. It *had* looked fine in pre-prod. The synthetic tests passed. The problem only showed up when real users on real devices in real network conditions hit it.


So you’d get this standoff. Developers pointing at clean staging results. SREs pointing at production signals that weren’t translated into anything a frontend engineer could act on. Everyone waiting to be absolved. Nobody owning the gap between the two.


I watched it happen across the industry. And I think, if I’m honest, we sort of did it to ourselves.


## How the camps formed


Go back to the early 2010s. The performance community was at Velocity Conference, the event where web ops and frontend engineering briefly shared the same stage. Steve Souders on one side, John Allspaw on the other. “Faster and stronger.” Two camps, one room.


But the tooling was already drifting apart. Developers got Lighthouse and Chrome DevTools — free, powerful, and completely disconnected from production. SREs got the observability stacks and the budget to run them. Frontend visibility became, in my experience, “a bit of a checkbox” — you got the basics of RUM, enough to say you had a signal, and the rest of the investment went into backend observability.


The result: devs owned pre-production performance through synthetic monitoring, SREs owned production reliability through observability tooling that wasn’t designed for frontend engineers. Two workflows, two datasets, one wall between them.


And when something passed the synthetic test but broke in production — which happened more times than I can count — you got the mean time to innocence problem. Not because anyone was being dishonest. Because the tools literally couldn’t give them a shared picture.


## What RUM couldn’t do — and what changed


Part of what kept the camps apart was a real limitation: for most of its history, RUM was a reporting tool. It could tell you something was slow. It couldn’t reliably tell you why.


If INP was bad, you knew INP was bad. You didn’t know which element on the page was causing it, which script was blocking the main thread, or whether it was a first-party bundle or a third-party tag. That diagnostic gap meant developers couldn’t act on RUM data effectively, even when they had access to it. So they stuck to synthetic, where the waterfall showed you everything.


What’s changed is attribution. LCP now has subparts — resource load delay, resource load duration, render delay — that tell you *why* it’s slow, not just that it is. INP has subparts too: input delay, processing duration, presentation delay. And long animation frames (LoAF) now give you script-level attribution — you can see exactly which scripts were blocking the main thread when your INP spiked, right down to whether it was your own bundle or a cookie consent tag somebody added without thinking.


This is what I mean when I say RUM has become genuinely diagnostic. A developer can now look at RUM data and have something to act on. That’s new. It’s only been true for the last couple of years.


Server timing headers are also worth mentioning — I recently pulled HTTP Archive data showing adoption crossed 50% in 2024–2025, driven largely by CDNs and platforms like Shopify emitting timing data automatically. Cache hit rates, time to origin, database processing time arriving in your frontend RUM data. Backend context, no custom instrumentation required.


## The OTel part I didn’t expect to care about


I’ll be straight with you: when I first came to Embrace, I barely knew what OpenTelemetry was. I understood RUM. I understood performance. OTel felt like a backend concern.


I was wrong.


What OTel does for web RUM is bring it into the same instrumentation standard that backend teams have been using for years. End-to-end traces. W3C trace headers propagating from the browser to the backend. Spans that route to whatever observability system your organisation already uses — so you’re not locked into a vendor’s proprietary format and you’re not starting from scratch if your stack changes.


More importantly: it means frontend and backend telemetry can live in the same place, in the same format. A frontend engineer and a backend SRE looking at the same data. That’s what closes the gap I was describing. Not goodwill, not org restructuring — shared instrumentation.


OTel also fills in browser API gaps. Chrome leads on adopting new APIs, Firefox follows, WebKit lags. CLS still isn’t supported in WebKit. Rather than waiting for parity that may be years away, you can instrument what you care about with OTel spans and collect that data across every browser your users are actually on.


## How the tools work together now


Tammy has a framing I keep coming back to: RUM reveals, synthetic validates.


RUM tells you where problems are and who’s affected. Synthetic tells you what’s causing them and confirms when you’ve fixed them.


In practice: your RUM data shows a spike in INP — jumps to something like nine seconds on a specific page. Attribution data tells you the element, the URL, which scripts were involved. You run a targeted synthetic test, get a full waterfall, and see the long animation frames showing up in red that weren’t there before. You added something to your bundle that’s now blocking the main thread. Two tools, a few minutes, a clear answer.


Or the reverse: you use RUM to tell you where to test in the first place. You’re seeing 5% of traffic from India and you haven’t set up synthetic tests from there. RUM tells you which URLs are highest-traffic, which journeys have the most friction, which device profiles actually represent your user base. That’s what should be configuring your synthetic setup — not a guess about which five URLs probably matter.


The other direction works too. You push new code. You run synthetic tests against staging before anything reaches production. You set performance budgets and let the CI pipeline catch regressions before your users do. Then you watch your RUM data after the deploy for anything that made it through anyway.


Both tools, all the way through the cycle. That’s the thing that was structurally impossible when the camps were separated. It’s not impossible anymore.


## Why I think this matters beyond the tooling


Core Web Vitals helped start closing the gap because they gave both camps shared language. A metric that a developer tests for in pre-production and an SRE monitors in production is a metric that creates a reason for both to be in the same conversation.


OTel is doing the same thing at the infrastructure level — literally a shared standard for frontend and backend telemetry.


But the thing that’s actually changed, the thing I didn’t fully appreciate until recently, is that the RUM data is now diagnostic enough to act on. Developers have something they can do with it. That’s what breaks the “mean time to innocence” cycle — not culture change, not better meetings, but tools that close the loop between seeing a problem and fixing it.


Shared ownership of performance across the full stack isn’t a vision statement anymore. It’s what the tools make possible today. The main thing holding most teams back is the habit of treating pre-production and production as separate problems.


They never were. We just built tools that made them feel that way.


Deliver incredible mobile experiences with Embrace.


Get started today with 1 million free user sessions.


[Get started free](https://dash.embrace.io/signup/)


Author


[Cliff Crocker](https://embrace.io/author/cliff-crocker/) Cliff is a long-time web performance advocate who has spent the majority of his career building tools and products that drive performance culture. Cliff is obsessively focused on improving the user experience by providing visibility, insight, and value for the largest sites on the web. Previously at Akamai.


Related Content


OpenTelemetry


8 May 2026


• 6 min read


### [Browser OpenTelemetry without the compromises: How to stay vendor-neutral and still get full RUM](https://embrace.io/blog/browser-opentelemetry-without-compromises/)


A real-world React SPA setup surfaced a question about browser OTel. The answer points to a better way to instrument the web.


Web


3 May 2026


• 6 min read


### [This is where web performance was always meant to live](https://embrace.io/blog/web-performance-week/)


Let’s kick off Web Performance Week at Embrace


business


29 April 2026


• 5 min read


### [Make the business case for web performance with just three visuals](https://embrace.io/blog/web-performance-business/)


If you want to get the attention of non-technical stakeholders, these are the three most powerful charts to immediately grab folks' interest and get them to care about site speed and user experience.
