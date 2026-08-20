---
schema_version: "1.0.0"
document_id: "887bb0a10006cb8790caf9b6862dacdb8c63fc761583388b381678a462a73bf9"
company_key: "yc-tenjin"
company: "Tenjin"
source_id: "yc-tenjin-rss-c54d6661109e"
canonical_url: "https://tenjin.com/blog/stop-bad-traffic-fraud-snake-io-kooapps/"
published_at: "2026-06-22T15:04:55+00:00"
first_seen_at: "2026-07-20T23:20:37.018956+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:51ca5504433ceb32ded8b04db61bca041304e812977e3b9ef7c862bb6144cf70"
---

# Stop Bad Traffic: How Snake.io Reduced Fraud Requests by 33%

Learn how one of casual gaming’s biggest studios used Tenjin to[stop fraud](https://tenjin.com/fraud-prevention/) from scaling alongside their UA


Scaling user acquisition (UA) across multiple networks and geos can get complicated. When you’re getting hit with repeat fraud, broken[site-level blocks](https://tenjin.com/glossary/site-id/) , and a growing pile of refund requests, it can make the situation worse.


Just as Kooapps wanted to grow spend across their portfolio, this is exactly where they were. Their situation isn’t so uncommon: they desired to spend more, without scaling fraud alongside it.


In this case study, we explore how Kooapps, one of casual gaming’s most beloved and downloaded studios, leveraged Tenjin’s[Site ID Optimization](https://tenjin.com/blog/introducing-site-id-optimization-sio-tool/) (SIO) to take control over their fraudulent traffic and scale their UA.


**Here’s a snapshot of their impressive results:**


- **About ⅓ reduction in fraud refund requests**
- **Global site-level blocking across all networks from one place**
- **Stabilized business-wide metrics previously distorted by data**


## About Kooapps


Kooapps is one of the most recognized names in casual mobile gaming, with hundreds of millions of downloads across titles like **Snake.io** and **Piano Tiles** . Both games have been fixtures on the App Store and Google Play top charts for years, building loyal player bases that keep coming back.


Marc Wofford, Director of Data and Analytics, leads the team responsible for analytics pipelines, stakeholder reporting, and optimization across UA, ad monetization, and in-game data. So, when fraud gets into the system, it not only wastes resources, it ultimately undermines every decision the team makes downstream.


## **Challenges**


Before using Site ID Optimization, dealing with fraudulent and underperforming traffic was a fragmented and time consuming process. It was difficult to keep track and keep up as their[app portfolio](https://tenjin.com/blog/games-to-app-investment-mobile-app-portfolio-in-2026/) grew.


### Individual Blocking Did Not Stick


Site ID blocks applied on individual network dashboards frequently failed to act properly due to bugs, incomplete rollouts, or new campaigns launched after the fact.


> *“Each \[ad\] network had their own solution and weird UI/UX,” Marc explained. “Blocks hadn’t applied to all games, or didn’t apply to new campaigns, or the network just was super new and hadn’t built site optimization yet.”*


Each network has their own quirks and keeping up with them wasn’t sustainable.


### Fraud was Generated at Scale


Without a unified way to enforce blocks, fraud became an inevitable byproduct of growth.


> *“It was becoming impossible to scale our spend across all networks and geos because of how guaranteed we were to see fraud,” said Marc.*


Scaling UA meant scaling the problem right along with it.


### Data was Untrustworthy


Fraudulent traffic does more than waste budget. It quietly corrupts every metric it touches. Retention curves, conversion signals, monetization data, all of it becomes harder to read and act upon when bad traffic is weaved in. Running controlled experiments of making big decisions gets harder the longer fraud goes unchecked.


### Time-Consuming Refund Requests


Even with attribution tools in place, gaps in early[fraud detection](https://tenjin.com/glossary/fraud-detection/) meant the team was often catching issues only after damage was done, triggering refund requests and hours of manual investigation instead of focusing on growth.


## **Solution**


Tenjin’s SIO gave Kooapps one place to manage everything, cutting through the fragmentation, so the analytics team had more control and oversight.


### One Control Layer for Every Network


> *“Having a one-stop-shop to control the traffic we’re allowing was very appealing,”* Marc said. *“This lets us work on blocking ASAP regardless of the state of each ad network’s dashboards, so we can instantly stop wasting money paying for garbage. Less spent means fewer refund requests and more time on our hands.”*


No more chasing down data on different dashboards, or worrying about whether a block actually applied.


### Global Blocking That Removes Repeat Offenders for Good


The team defaults to global-level blocking, whenever possible:


> *“Fraudsters typically will become repeat offenders if given the chance, so we like to 100% remove them from play,” Marc noted.*


For networks that report site IDs in unique formats, like hashes instead of human-readable bundle IDs, network-level blocks are applied as needed. The goal is always the same, remove the problem completely, rather than just slowing it down.


### Catch Fraud in Evolution


Kooapps’ analytics team developed a layered approach to flagging suspicious site IDs, tracking signals like abnormal or artificially consistent retention curves, D7/D1 and D30/D1 retention ratios, tracked vs. reported install discrepancies, click-through timing anomalies, carrier-geo mismatches, language-geo mismatches, push notification status distributions, OS version and device model misalignment, and cumulative 30 to 90 day spend patterns to catch low-volume fraud that deliberately stays under the radar.


> *“Fraud is a game of cat and mouse,” Marc said. “You should always be looking for new signals that can help inform when fraudsters find a new approach to skirt around the signals you already have.”*


### A Team Process


Each business analyst owns their respective networks, with the team cross-checking one another when suspicious patterns surface across sources, keeping oversight sharp without creating bottlenecks.


### Fraud Refund Requests Cut by a Third


With faster detection and cleaner blocking, Kooapps reduced the size of their fraud refund requests by approximately **33%** freeing up budget, time, and mental bandwidth across the team.


## **Conclusion**


Fraud does not just drain your budget, but it dilutes trust in everything. When bad traffic runs unchecked, it distorts retention data, skews monetization signals, and introduces noise into every experiment you run. As Marc puts it:


> *“Fraud can also lead to confusion on many other metrics. Knocking out major sources globally helps stabilize many other metrics business-wide.”*


For a studio operating at the scale of Kooapps, that kind of instability is more than an inconvenience, because it places a cap on growth. Tenjin’s SIO gave the team the chance to act fast, attack multiple networks from one place,block thoroughly, and stay ahead of evolving fraud patterns, without depending on each individual network to get it right.


Their results? Fraud refund requests reduced by roughly a third, business-wide metrics stabilized, and a team that spends its time building, instead of chasing down bad traffic.


As Marc summed it up:


> *“It’s been great to have our optimization power increase as we scale. Don’t let yourself be drowned by someone else’s bad algo.”*


If your analytics team is still managing fraud one network at a time, Kooapps’ take is straightforward:


> *“This is a must use. Easy wins.”*
