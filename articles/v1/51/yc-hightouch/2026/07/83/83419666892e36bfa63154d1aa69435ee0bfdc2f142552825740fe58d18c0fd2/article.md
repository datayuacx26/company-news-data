---
schema_version: "1.0.0"
document_id: "83419666892e36bfa63154d1aa69435ee0bfdc2f142552825740fe58d18c0fd2"
company_key: "yc-hightouch"
company: "Hightouch"
source_id: "yc-hightouch-news-import-8089bfc075a8"
canonical_url: "https://hightouch.com/blog/performance-marketing-levers"
published_at: null
first_seen_at: "2026-07-25T08:06:20.021758+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:dfef9c4a32e3a6bd3a5e670cd3d8e3290af04ee7b9398e64247c52c5a3098919"
---

# The five levers of modern performance marketing

Performance marketing has always rewarded the teams that adapt fastest. But the pace of change has accelerated dramatically. The rise of automated campaign types like Google Performance Max, Meta Advantage+, and their successors have fundamentally shifted what the job looks like. Algorithms increasingly handle targeting and bidding. Platforms reward creative volume that no team could manage manually. And signal loss, privacy regulation, and walled gardens that report what they want to report have upended measurement.


The marketers pulling ahead aren't the ones with the biggest budgets. They're the ones who understand which levers still matter and operate them with precision.


## 1. Signals


Automation is only as good as the data feeding it.[Signal engineering](https://hightouch.com/blog/signal-engineering-new-frontier-ad-performance) is the practice of capturing, modeling, and transmitting high-quality conversion signals back to ad platforms — so the algorithms optimize for outcomes that actually matter to your business, not just the conversions that are easiest to track.


The shift from manual campaign management to AI-driven automation has made signal quality the most underleveraged lever in performance marketing.


### What great looks like


Great signal engineering means sending conversion events that reflect real business outcomes, not just the ones that are easiest to capture. The best teams enrich every event with customer identifiers (hashed email, phone, address) to maximize match rates and assign value to each conversion based on contribution margin or predicted lifetime value. They also use predictive models to estimate the likelihood of high-value outcomes within attribution windows, even when the actual result — a renewal, a second purchase, a closed deal — won't be confirmed for weeks.


XP Inc., one of Latin America's leading financial platforms, faced a common problem: their ads were optimizing for account sign-ups, but their real goal was first investments, which took most users up to 14 days. By building predictive models to identify high-value investors early and syncing those signals to ad platforms via Conversion APIs, they replaced a platform-defined event with a precision signal built on their own data. The result:[$66M in incremental revenue and a 62.5% improvement in lead qualification](https://hightouch.com/customers/xpinc) .


### How to evaluate your readiness


- Are you sending conversion events server-side via Conversion APIs?
- Are you enriching events with customer identifiers (email, phone, address) to maximize match rates?
- Are your conversion values aligned with actual business outcomes (e.g., contribution margin, predicted LTV) rather than raw revenue?


## 2. Targeting


First-party data is now paramount in targeting. As third-party cookies erode and platform targeting democratizes, the marketers with an edge are those activating their own customer data: building lookalike audiences, suppressing existing customers, and retargeting high-intent segments directly from their data warehouse.


### What great looks like


Great targeting starts with clean, comprehensive first-party data activated directly from[the source of truth — your data warehouse](https://hightouch.com/blog/data-warehouse-101-for-marketers) . The best teams aren't just retargeting website visitors; they're building dynamic suppression lists to protect margin, seeding lookalike audiences with their highest-LTV customers, and syncing CRM segments in real time so platform audiences always reflect current customer state.


### How to evaluate your readiness


- Is your first-party audience data clean, comprehensive, and syncing to ad platforms in real time?
- Are you going beyond retargeting to build lookalikes from high-LTV segments? Are you running dynamic suppression lists?
- Are your audience refreshed daily or weekly, or are they stale?


## 3. Creative


For years, creative was the last thing performance marketers optimized. Targeting and bidding took priority, whereas creative was something you handed off and hoped for the best. That's changed. As platform algorithms have absorbed targeting and bidding decisions, creative has become the primary remaining lever that humans control.


The shift is structural. Platforms like Meta and Google now explicitly reward variety, volume, and relevance — and[they're transparent about it](https://www.facebook.com/business/news/demystifying-creative-diversification) . More unique creative means more signals for the algorithm to learn from, more surface area to find what resonates, and more opportunities to stay fresh before fatigue sets in. The constraint has shifted from campaign strategy to production capacity.


### What great looks like


Great creative programs treat creative as a system, not a series of one-off campaigns. The best teams connect performance data directly to creative production. They understand which elements are driving results, iterate on winners, and generate new concepts before fatigue sets in — rather than scrambling after performance drops.


Volume and variety matter, but so does quality. The high-performing teams[move from insight to launch in days rather than weeks](https://hightouch.com/blog/introducing-ad-studio) , testing across offers, formats, hooks, and messages at a scale that was previously difficult to reach without sacrificing brand integrity.


The result is a compounding advantage: more ideas in market, faster feedback loops, and algorithms with richer creative signals to optimize against.


Otrium, a European fashion marketplace, was constrained by the same bottleneck most creative teams face: production timelines that made it impossible to test at the volume platforms reward. By building a workflow that connected performance insights directly to creative production, they saw[incredible results](https://hightouch.com/customers/otrium) : they cut campaign execution time by 70% (from four weeks to one) and saw a 13% increase in click-through rate, 15% more conversions, and a 10% improvement in ROAS.


### How to evaluate your readiness


- Do you have a structured process for testing creative across offers, formats, hooks, and messages?
- Are you iterating on winners before fatigue sets in, or reacting after performance drops?
- Can you move from insight to launch in days rather than weeks?


## 4. Bidding & Budget Allocation


Bidding and budget allocation used to mean manual bid adjustments, dayparting, and device modifiers. Platform algorithms have absorbed most of that work.


The lever today is different: it's about how you structure campaigns to give algorithms the right constraints, how you set value-based bidding targets that reflect actual unit economics, and how you allocate budget across channels and funnel stages based on marginal returns rather than historical inertia. As platforms consolidate spend into fewer, larger campaign types, the decisions you make at the structural level matter more than ever.


### What great looks like


The best teams have largely handed bid-level decisions to algorithms, but they're deliberate about the constraints they set. They ground target ROAS and CPA values in actual unit economics, not platform-suggested defaults, and review them regularly as costs and margins shift. They design campaign architecture to avoid cannibalization — giving algorithms enough data to learn without splitting signals across too many campaigns.


At the budget level, great teams move away from channel-based budgeting driven by historical spend patterns. Instead, they allocate based on marginal returns — informed by Media Mix Modeling (MMM) and incrementality tests — and rebalance continuously rather than once a quarter. The mindset shift is from "how much did we spend here last year" to "where does the next dollar work hardest."


### How to evaluate your readiness


- Are your target ROAS and CPA values grounded in actual unit economics? Or are you just optimizing for defaults?
- Are you allocating budget based on incrementality data, or primarily on platform-reported ROAS?
- Is your campaign architecture giving algorithms room to optimize without cannibalizing each other?


## 5. Measurement


If you can't trust your measurement, you can't make good decisions. Measurement closes the loop on performance, telling you what channels are working, what creative is driving outcomes, where to shift budget, and whether your strategy is delivering real business impact.


### What great looks like


Most teams are still over-relying on platform-reported attribution, which systematically overstates performance and underpins flawed budget decisions. The best teams have moved beyond multi-touch attribution (MTA) as a standalone source of truth, recognizing its limitations across cross-device journeys, walled gardens, and signal loss.


Instead, they triangulate. They use MTA for directional day-to-day optimization, MMM to understand channel contribution at a macro level and guide long-term budget allocation, and incrementality studies (geo-lift tests and holdout experiments) to validate whether spend is actually driving outcomes or just correlating with them. The goal isn't to pick one method. It's to build a measurement framework where each approach provides a check against the others, and where budget decisions are grounded in incremental lift instead of attribution credit.


### How to evaluate your readiness


- Are you running incrementality experiments to validate what's actually driving outcomes?
- Do you have MMM in place to guide long-term budget allocation?
- Are you triangulating platform attribution with MMM or incrementality data before making budget decisions?


## Where Hightouch fits


Hightouch is built for the three levers where data, AI, and speed compound into a durable performance advantage: signals, targeting, and creative.


### High-quality signals


Most signal quality problems are data problems. Conversion events are incomplete, conversion values don't reflect actual business outcomes, and predictive signals never get built because the data science and marketing teams are working in separate systems.


Hightouch’s Composable CDP sits on top of your warehouse (where your best data already lives) and activates it directly to ad platforms via native Conversion API integrations. The result is cleaner signals, higher match rates, and algorithms that learn to optimize for the outcomes that matter.


### Fast and easy targeting


First-party data is only an advantage if marketers can activate it faster than the competition. Hightouch’s Customer Studio provides marketers with a no-code UI to build and sync audience segments, including suppression lists, lookalikes, and high-LTV retargeting pools, directly from your warehouse to ad platforms in real time. Your platform audiences always reflect your current customer state, not last week's export.


### Fast and effective creative with AI agents


Producing the volume and variety of creative that platform algorithms reward has historically been a bottleneck.[Hightouch Ad Studio](https://hightouch.com/platform/ad-studio) connects performance insights from your data stack to creative production, working from a brand context layer that knows your products, your guidelines, and your approved assets. The result: on-brand concepts generated at the volume platforms reward — across formats, languages, and platform specs — and exported directly to ad platforms. The constraint shifts from production capacity to creative strategy.


### Measurement that you own


While Hightouch doesn't replace your measurement stack, the warehouse-native architecture makes it significantly easier to build one you can trust. In-platform attribution flows naturally from the signals you send via Conversion APIs. Incrementality tests on first-party audiences can be run directly through Customer Studio.


And while geo-lift and MMM don't run through Hightouch, they're most effective when centralized in the same data warehouse alongside your customer and outcome data — which is exactly where Hightouch operates.


## The compounding advantage


None of these levers are new. What separates the teams pulling ahead is the precision, data, and speed they bring to each one — and that advantage only compounds.
