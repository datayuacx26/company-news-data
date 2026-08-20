---
schema_version: "1.0.0"
document_id: "480469763c1622d34b542477d6a57df2abc4448525c2249d04b1f911410b9fbb"
company_key: "yc-hatchet-run"
company: "Hatchet"
source_id: "yc-hatchet-run-rss-3f16f06bd764"
canonical_url: "https://hatchet.run/blog/andon-cord"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-17T17:14:54.836419+00:00"
fetched_at: "2026-08-17T17:14:57.450085+00:00"
content_hash: "sha256:b85e45a1ba9500f1a7dde3e208ca1c3130ef56aed0e709ea0638ede735ec8cf0"
---

# Your team needs an andon cord

Alexander Belanger


Co-Founder


Hatchet


One of the most important engineering decisions we made this year was sending the following emoji in our company Slack channel.


If you’d told me that 2 years ago, I probably would’ve quit on the spot; assuming, naturally, that the shitcoiners trying to infiltrate our Discord had done so successfully and I was an NFT shill.


But this is not that. The green toad is called Bufo, and the yellow rope-looking thing is an andon cord. Stay with me.


#### What’s an andon cord?


It’s an actual, physical rope, originally used in Japan’s manufacturing industry, most notably Toyota factories, and popularized in Western culture through Mike Rother’s *Toyota Kata* . In the event of a defect, bottleneck, or other known issue in a factory, an employee would pull the andon cord and all factory production would stop.


Please excuse the AI-generated image; I couldn’t find a fair-use picture of an andon cord on the internet.


There’s no point in continuing production if a bottleneck exists in the assembly line, because the productivity losses caused by the bottleneck (backpressure on upstream components, underutilization of downstream components) would exceed those caused by fully stopping production to fix the issue.


Productivity gains aside, the andon cord has a significant cultural utility: it reinforces continuous improvement and discourages the normalization of production issues.


#### What does this have to do with software?


In Hatchet’s early days, we spoke often of the kind of culture we’d like to build. One of our founding team members mentioned the andon cord as a cultural norm they’d appreciated in a previous job. It immediately resonated with us.


Every software startup is a factory. The raw materials are computers, human brains, and your other tooling. The output is software delivered to end users.


Our assembly line involves a lot of steps. Writing code is a small part of the process: gathering user feedback, getting feedback internally, reviewing PRs, staging deployments, load tests, and production rollouts comprise the bulk of the work.


Anything that impacts our ability to ship and deploy a stable, reliable platform impacts the performance of our factory, and therefore, our bottom line. We decided that slowdowns or bottlenecks in our factory would be the overriding priority.


And thus,` :bufo-pulls-the-andon-cord:` was born.


#### When everything’s highest priority, nothing is


Note the emphasis on *overriding* priority.


We’re an ambitious team, so we tend to bite off a lot of work at once. When a customer asks for something, and we think it’s a great feature request, we usually make it a priority. When we notice a small spike in latency on some critical-path service, we also make it a priority. When we postmortem an outage, it’s the priority. And so on, until the priority tickets pile up.


In a world of hundreds of high-priority tickets, it becomes impossible to see through the noise. You’re constantly shipping, fighting fires, and feeling like you’re falling behind.


The andon cord makes the ultimate priority very clear: there is nothing more important than our ability to ship reliable software. A clear and resolvable problem in our software factory is more important than anything else (yes, even new features and launches). It’s worth stopping all work to resolve it.


It’s slightly counterintuitive: you would think that throwing a signal interrupt every few weeks would lead to more noise and overhead. And in the short term, this is true. But after a few months of resolving the most critical bottlenecks, you suddenly start to be able to ship with fewer fires and less unplanned work.


#### Our andon cord-worthy problems


I won’t bore you with the deep internals of Hatchet’s factory, but it’s worth listing the cases where we’ve pulled the andon cord in the past 6 months.


For some background on our infrastructure setup, Hatchet runs on over 25 shards in 3 regions. Our busiest shards are running tens of thousands of transactions per second. This volume naturally makes certain things difficult.


- We didn’t have a sensible rollout strategy, leading to releases piling up in our backlog, and multiple big risky changes being shipped simultaneously. Before continuing deployments, we rewrote our entire deployment process.
- We didn’t have good enough observability, so customers alerted us about unexpected latency before we were aware of it. We started very closely tracking latency metrics and instrumented all critical-path endpoints.
- We had a ton of observability data, but we were over-alerting and over-paging. We stopped all work to move towards zero error logs in production.
- Our staging environment wasn’t catching enough issues during the load testing process. We rolled out a new environment called` staging-chonky` and rewrote our load testing harness.


These days, we still have andon cord moments, but they’re resolved very quickly, and usually by one or two team members, instead of the entire team. We’ve ramped up our shipping velocity significantly in the past few months; this past week, we shipped 3 new core engine features, and a ton of smaller improvements and fixes.


#### The burden of unplanned work


*Can you tell we’ve been reading The Phoenix Project?*


At startups, there’s a continuous pressure to ship all of the time. Dropping everything to invest in our tooling and process is rare among startups that I talk to; most teams tend to normalize and adopt the mentality that “things are on fire because we’re a startup.”


The issue is that there is nothing more burdensome than unplanned work, and it’s usually invisible to teams when it becomes normalized. This unplanned work regularly produces heroic, monumental efforts—like pulling an all-nighter to resolve some critical incident—which just leads to more pressure to meet tight deadlines, which leads to more unplanned work, and so on.


I used to think this was just part of startup culture. But adopting and implementing the andon cord principle has changed my view significantly. There’s short-term pain, yes; but by honoring the andon cord, we’re shipping rapidly and more effectively now.


---


#### PostScript: Bufo has PMF


*An aside to the aside: Bufo is a genus of true toads in the amphibian family Bufonidae.*


This post is not about Bufo, but at this point you might be wondering who he is.


We provide a lot of customer support through shared Slack channels. These are Slack channels which are connected from one workspace to another. This also means that we get to see which emojis other companies are using.


Earlier this year, we started seeing this little toad emoji reacting to various messages in our Slack channel. I only know of 2 named toads: Toad from *Frog & Toad* , and Pepe.


Pepe was born out of 4chan, the birthplace of incel culture, and in my mind has a fairly negative connotation. Thankfully, a quick google search revealed that this is a different green toad called Bufo.


Within a week of seeing Bufo, he appeared in our internal emoji set, much to the chagrin of our overly curmudgeonly head of growth, who attempted her own andon cord moment of sorts:


It didn’t work. And within a few months, Bufo had successfully infiltrated at least five other shared Slack channels.


I’ve never seen clearer signs of product-market fit.
