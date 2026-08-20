---
schema_version: "1.0.0"
document_id: "fe7e3eea6f7f1f8ae6336bcefbfed18ad856f641fcee037190e46e7f5ca05422"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/marketers-hour-of-hell-slow-builds"
published_at: "2025-10-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:55:32.133421+00:00"
content_hash: "sha256:cf831b0a19e59383330225057197c271c23178c5d3f6d486e67df22b9b4a0446"
---

# A marketer's hour of hell: What I finally get about slow builds

As a marketer and self-taught developer, I didn't fully grasp the pain of slow builds and broken AF CI/CD pipelines until this week when AWS US-East-1 went down. In my previous positions, I would ask for a change and the engineers would deal with it. From the outside looking in, if it took forever to deploy or they refused to deploy on Friday, or (insert other stereotype about engineers here), it was obviously just an excuse. Right?


Wrong. And this week, my foot was rightfully shoved in my mouth.


Here's the thing about those "stereotypes", seasoned developers aren't being dramatic when they take forever to deploy or refuse Friday deploys. They're being smart. And I finally get it now.


## Life with fast builds


As the head of developer marketing here at Depot, I'm spoiled. If a change needs to be made to the website:


- I create a branch
- make my changes and test locally
- create the PR
- merge and deploy to preview


I see my changes in preview within 2 minutes, and then deploy to live within another 2 minutes. There's no "don't deploy" engineering culture here at Depot because, well... if something goes wrong, you can fix it within minutes instead of hours.


That was my exact workflow until this week.


## When everything slowed down


When AWS US-East-1 went down, like much of the internet, we felt it too. I went to make a simple adjustment for tracking a link in[PostHog](https://depot.dev/customers/posthog) . Made my changes, created my PR, and bam... lint failure. This happens all the time because I forget to run 'fmt', so I quickly ran it and pushed without stopping to check locally, because the last 99 times I've run 'fmt' it hasn't made a damn bit of difference. (Lesson learned, btw).


Waited for checks, merged, and deployed.


That push broke the docs pages. An essential piece to ANY devtool website.


I panicked. Broke out in a cold sweat. Fixed the very small error and pushed the fix, but this time the build took almost 6x longer than usual to fail. Made one more small adjustment, pushed again, another 6x longer build that finally succeeded. Then a long slog to get the fix out to production.


Overall, the docs page was down for close to an hour. With Depot, in my everyday workflow, with AWS-US-East-1 running on all cylinders, this fix would have taken me 10 minutes tops.


That's 50 minutes our docs weren't serving our customers. 50 minutes of wasted time.


Now imagine if that were your entire product going down for that long.


## The realization


[Depot](https://depot.dev/) makes our builds so fast that even a temporary slowdown felt painful. Our usual 2-minute builds stretched to 12+ minutes, and suddenly I understood the frustration engineering teams face daily when their builds crawl. If this brief taste of slow builds was this excruciating, I can't imagine dealing with it as your baseline.


Those engineers who seem overly cautious about deploys? They're not making excuses. They're managing risk in an environment where a simple fix can take an hour instead of minutes. This week, I finally felt that pain myself and it gave me a whole new appreciation for why speed matters.


## Related posts


- [Why 98.5% of organizations have slow actions/checkout](https://depot.dev/blog/why-organizations-have-slow-actions-checkout)
- [We analyzed 66,821 GitHub Actions runs: 9 hidden gems you're missing](https://depot.dev/blog/we-analyzed-66821-github-actions-runs)
- [The hidden cost of self hosting CI runners](https://depot.dev/blog/hidden-cost-of-self-hosting-ci-runners)
- [Monorepos: Worth the hype?](https://depot.dev/blog/monorepos-worth-the-hype)
- [Guide to faster Rust builds in CI](https://depot.dev/blog/guide-to-faster-rust-builds-in-ci)


Tori Elstrom


Head of Developer Marketing at Depot
