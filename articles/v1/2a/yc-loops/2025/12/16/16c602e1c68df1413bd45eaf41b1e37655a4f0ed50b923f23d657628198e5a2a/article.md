---
schema_version: "1.0.0"
document_id: "16c602e1c68df1413bd45eaf41b1e37655a4f0ed50b923f23d657628198e5a2a"
company_key: "yc-loops"
company: "Loops"
source_id: "yc-loops-news-import-7f2c6ba5f1f0"
canonical_url: "https://loops.so/engineering/our-first-code-freeze"
published_at: "2025-12-09T00:00:00+00:00"
first_seen_at: "2026-07-24T09:57:10.497635+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:22f25f16388950f44f08b825bee21d67ba1794e6c090d08a8d5d029b84202b0a"
---

# Our first Code Freeze

Normally we ship code to production constantly throughout the week (yes, even on Fridays). Developers are responsible for getting their PRs tested, approved, and deployed. This year, as Black Friday approached, we made the unusual (for us) decision to put a code freeze in place. Our goal in doing this was to prioritize stability given the needs of our users during this busy email season.


## The Typical Reason


Many companies halt production code deploys during the holidays while limited staff are available. Engineers, product managers, and ops teams are taking vacation, leaving fewer hands available to respond to incidents. Most service interruptions are the result of an intentional change, whether it’s a bug that shipped or a well-intentioned database script that went sideways.


## Black Friday Email Volume


The week surrounding Black Friday is one of the highest-risk days of the year from an operational standpoint for email companies. Prior to leaving for their Thanksgiving vacations, many of our users schedule their biggest campaigns of the year to send during this period. Our users trust us to “take it from here” and deliver these emails on time with good inbox placement.


Our intuition (backed by last year’s experience) ended up being validated as we saw a sharp uptick in email sending on November 28th, the day after Thanksgiving.


## Balancing Stability and Velocity


A code freeze didn’t mean we stopped moving forward. We approached it strategically:


1.


**Soft boundaries, not a full shutdown**


Critical fixes **were still allowed,** but everything else was paused. We didn’t do anything to block or otherwise actually prevent folks from merging pull requests or deploying changes. It was just an informal agreement. In the event that we needed to do something, we wanted to keep it easy to do so; being a small team helped in this regard.


2.


**Pre-Black Friday preparation**


We reserved extra quotas where needed, shored up monitoring and alerting, and reviewed operational procedures. Our goal was to enter the weekend fully prepared.


3.


**Focus on internal improvements**


During the freeze, the team could focus their time on our (very human) support.


4.


**Clear communication**


We settled on a plan a few weeks in advance, wrote everything down, and shared the details in a notion doc as an RFC.


## Why now?


Choosing to freeze code was a reflection of the scale and maturity that Loops had grown to. Our customers put their full trust in us to deliver email for them when they expected it.


-


Reliability can matter more than shipping features.


-


We value our users’ experience and want to make sure campaigns go out smoothly, especially when the stakes are highest.


## Looking Ahead


We’ll review system performance from this year and use it to inform how we handle this next year. The freeze reminded us that sometimes the smartest engineering decision isn’t about moving faster. If we can keep reliability high while still making steady progress, we consider that a win.
