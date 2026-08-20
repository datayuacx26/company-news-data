---
schema_version: "1.0.0"
document_id: "e98287223024f81ac66ea8c8d85b4e67fe59bd50ae851054fa8e521a9d5f3486"
company_key: "yc-taktile"
company: "Taktile"
source_id: "yc-taktile-news-import-01317932feb4"
canonical_url: "https://engineering.taktile.com/blog/it-has-never-been-more-exiting-to-be-a-builder/"
published_at: "2026-02-19T00:00:00+00:00"
first_seen_at: "2026-07-24T03:38:54.472601+00:00"
fetched_at: "2026-07-28T21:26:29.837264+00:00"
content_hash: "sha256:784d272eff379d3991a0122d81a7c089a6b27a194893a2bf7917daa58eb6a072"
---

# It's never been more exciting to be a Builder

Feb 19, 2026 ·


4 min read


# It's never been more exciting to be a Builder


---


*Note: this piece was entirely written by[hu](https://www.linkedin.com/in/adrianbuzgar/) ma[ns](https://www.linkedin.com/in/robin-raymond/) .*


Software Engineering is going through major upheaval right now. Everyone is scrambling to join the agentic revolution and employ AI to get those sweet 10x productivity buffs. If this still sounds overly hype, you missed what’s been happening since ~fall 2025.


Between the launch of[stronger models](https://www.anthropic.com/news/claude-opus-4-5) , the increase to[1mil token context size](https://www.anthropic.com/news/claude-opus-4-6) , and everyone going wild on Gas Town and OpenClaw, we think it’s fair to say there’s been a Cambrian explosion of what is now achievable with AI and how far the slider has moved from handholding to agentic.


We are writing this on the tail end of joining the[Pragmatic Summit](https://www.pragmaticsummit.com/) last week, so this article will be a mix of where Taktile Engineering is in our agentic journey versus the general consensus we could gather from keynotes from[Gergely Orosz](https://www.linkedin.com/posts/gergelyorosz_iconic-moment-at-the-pragmatic-summit-this-share-7428342901933309952-QKtm?utm_source=share&utm_medium=member_desktop&rcm=ACoAAAH1p_ABFGFW-UujmmElGj_-mWEEccDsXJ0) , OpenAI, Uber, Ramp, DX and others. Along with some future gazing.


### Status Quo


Take every hairy organizational problem you were already dealing with, be it CI flakes, the sprawl of long-running E2E tests, or your production alert fatigue. Now plug those into an inner loop that spins 10x faster and you instantly get it: agentic AI massively accelerates everything, including how painful your bottlenecks are.


AI does not solve for observing production system health or for handling incidents as well as it one-shots simple coding tasks. A dysfunctional organization that staples AI on top of major problems looking for raw throughput and speed at any cost will quickly run into a brick wall. One of the polarizing stats shown by[Laura Tacho](https://www.linkedin.com/in/lauratacho/) (DX) at the Pragmatic Summit was some orgs see 2x production incidents post AI adoption. Given the types of sensitive data we process at scale, for us that’s clearly not an acceptable outcome. Especially with how many security questions Agentic AI raises.


Taktile Engineering saw a strong shift in AI adoption from Cursor AI to Claude Code in H2 2025, as we are sure many other Eng orgs did. Adoption is unanimous, but engagement with Agentic tools is not uniform. We have power-users who burn through token quotas and there are folks who never run out. We’re running multiple internal experiments around leveraging Claude across the development lifecycle, from coding, to reviewing, to deduplicating and investigating alerts, and hopefully anything in between. We’re experimenting quite freely with what’s possible while staying open to the notion that core practices are still getting defined and adopted. Our internal AI chapter and our AI dev exp Slack channel have been our most exciting hubs for Engineering learning and development in recent months. And of course, like many others, we’ve named our internal agent. For Taktile Engineering, it’s called Claudius.


A lot of our upcoming blog content will focus on our Agentic journey - both from a pure dev/builder perspective, but also about how this ties into our Agentic Product vision. The part we will be excited to share is around velocity metrics. We are already seeing a divergence in speed between teams that really shows bottlenecks: the internal delta on PR cycle time for a green-field team can be 4x faster versus a team that runs on a slower more mature CI.


### Future Gazing


Our running hypothesis right now is that small teams that leverage Agentic AI to cut back on entropy and chore will see the most benefits from refocusing on what we love most: building great product. A highly functional team that hyper leverages itself will probably run into other bottlenecks soon, including whether the old two pizza team Eng:Product ratio still works in the Agentic world. Product Managers and Designers can now unleash creative potential, and will soon probably be expected to build at similar speed and depth as any Product Engineer.


While it’s never been a better time to be a Builder, it’s never been a more challenging time for the management career ladder. The hard truth is there will probably be less of a need for management roles in an Engineering org that’s fully leveraged with Agentic AI. If anything,[the great flattening](https://www.forbes.com/sites/bryanrobinson/2025/01/24/the-great-flattening-trend-is-picking-up-steam-in-2025/) will continue, as it now makes less sense than ever to introduce artificial layers of management.


Two things are for certain, and witnessing this in person at the Pragmatic Summit was reassuring:


1. Everyone’s scrambling to go from AI adoption to Agentic 10x. There are no clear solutions or winners yet. Exciting times!
2. This is unlike any previous revolution in Software Engineering. It is the fastest one by far. Whether English will in fact replace all programming languages is still to be decided, but we are very confident that companies either go all-in or will be left behind. Which is great for us at Taktile, because both our[Product vision](https://taktile.com/ai-agent-manager) , as well as our Engineering strategy, position us on the leading edge of the Agentic AI revolution.


If you want to be part of our journey,[we’re hiring](https://jobs.ashbyhq.com/taktile) !
