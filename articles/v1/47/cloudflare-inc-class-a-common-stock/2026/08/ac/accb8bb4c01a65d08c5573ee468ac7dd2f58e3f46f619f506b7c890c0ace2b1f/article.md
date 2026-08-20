---
schema_version: "1.0.0"
document_id: "accb8bb4c01a65d08c5573ee468ac7dd2f58e3f46f619f506b7c890c0ace2b1f"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/total-eclipse-internet-traffic-iceland-spain-portugal/"
published_at: "2026-08-13T19:58:01+00:00"
first_seen_at: "2026-08-13T22:32:30.348733+00:00"
fetched_at: "2026-08-13T22:32:32.211750+00:00"
content_hash: "sha256:909858084d9a4b8cbab24c49e43f1ff6151b7828ccc252e9b922edd83bc0a292"
---

# Total eclipse of the Internet: traffic impacts in Iceland, Spain, and Portugal

At a time when looking down at our devices is a ritual in daily life, a natural phenomenon that demands our attention communally upward is a welcome event. On Wednesday, August 12, a total solar eclipse swept from the North Atlantic across Europe, moving over Iceland and northern Spain and Portugal, with a deep partial eclipse over the rest of Western Europe, all near local sunset. This was the first total solar eclipse to cross mainland Europe in twenty years, and it drew millions outdoors to witness the moon pass between Earth and the sun.


As we saw[during the 2026 World Cup](https://blog.cloudflare.com/2026-world-cup-internet-traffic/) and[the last total eclipse in 2024](https://blog.cloudflare.com/total-eclipse-internet-traffic-impacts-mexico-us-canada/) , online behavior is noticeably affected when an event at this scale takes place. In this blog post, we’ll use data from[Cloudflare Radar](https://radar.cloudflare.com/) to examine how Internet traffic shifted alongside the moon and the sun.


## Internet traffic dips align precisely with maximum obscuration


In the figure above, we measured HTTP request volume in five-minute buckets across the affected countries on eclipse day, and compared it to a normal-day baseline. Each row is a country (except for Alaska) and each column is a five-minute slice of August 12, 2025, the day of the eclipse. The countries appear above in the order in which they saw the eclipse.


The black diamonds mark the moment of maximum eclipse and the color shows the percent change in HTTP traffic versus the baseline (red = below normal, blue = above). We can see very clearly that the black diamonds overlay the darkest red, almost perfectly, signaling that **as the eclipse deepened, Internet traffic decreased.** The decrease was most significant along the path of totality and in countries that saw the deepest partial eclipse (Iceland, Ireland, the UK, France, Spain and Portugal) and all but absent where the sun was barely obscured (Sweden, Denmark, Poland, Switzerland).


Where the eclipse was deep, the red color and black diamonds mirror each other: traffic falls into a trough that sits directly beneath the peak of obscuration and rebounds as the sun reappears, typically within minutes of maximum coverage as people return to their screens.


The scatter plot above suggests that these decreases are not due to random chance. Each point represents the peak solar obscuration of an individual country ( *x* -axis), against the region's traffic dip ( *y* -axis). The traffic dip is measured as the average percentage change versus baseline in the 15-minute window surrounding maximum eclipse. The downward trend of the dotted line following the dots demonstrates that **regions along the path of totality saw traffic fall by roughly 15% to 30%, whereas areas experiencing only a shallow partial eclipse dipped far less or not at all.** While local variables like population density, time of day, and cloud cover account for scatter at any given coverage level, the overall direction remains consistent. Paired with the precise timing of the drops, the trend of the data demonstrates that the eclipse itself was the primary driver of the decline.


## Iceland, Spain and Portugal saw the biggest decreases in traffic


The figure above shows trend lines for each individual country. The gray triangles represent the progression of the eclipse obscuration, while the red line tracks the amount of traffic changed from its usual baseline. In almost all the countries and regions impacted in the course of the eclipse, traffic made noteworthy changes ranging from 9.3 to -46.7%.


The right-hand numbers on the “ *y2* ”-axis are the obscuration, calculated using precise sun and moon positions. For each location we found the apparent angular sizes of the sun and moon and how far apart they are in the sky, then calculated the fraction of the sun's disk covered by the moon every 5 minutes via the geometric overlap of two circles. That gave each place both its peak obscuration (how deep the eclipse got, 0–100%) and its moment of maximum eclipse. We then summed all regions in each country for the traffic total, and took the average obscuration across a country's regions to place its national max-eclipse time.


To differentiate the eclipse from a typical Wednesday evening, we compared eclipse day against the same weekday: the median of the three previous Wednesdays, matched slot-by-slot on time-of-day. Using the median keeps one odd week from skewing the comparison. Every number is then reported as percent change vs. baseline. When looking at the percentage on the left-hand y-axis, 0% means "totally normal" and negative means "less active than usual."


We can see the changes in traffic beginning in Alaska around 15:35 UTC, where the eclipse began its pathway. Iceland, Spain, and Portugal experienced the most dramatic drops in traffic, whereas Poland and Denmark quickly returned to pre-eclipse levels. Norway and Sweden actually saw slight traffic increases above baseline, while Denmark recorded the least overall change.


Ultimately, these findings reveal a clear correlation between the path of the eclipse and human behavior online. While the severity and duration of traffic drops varied by region, Radar’s HTTP traffic data demonstrates how a shared physical event can temporarily reshape digital activity across an entire continent.


## Track the impact of world events on Cloudflare Radar


Major events in the physical world remind us that digital traffic is, at its core, a direct reflection of human attention. When the moon obscured the sun across Europe, the Internet slowed down not because of network failures, but because people paused their online activity to watch. As network patterns quickly normalized post-eclipse, the data left behind offers a fascinating snapshot of how a cosmic event can momentarily realign our online world.


To explore more interactive traffic insights and track how major worldwide events shape internet activity every day, visit[Cloudflare Radar](https://radar.cloudflare.com/) or follow us on social media at[@CloudflareRadar](https://twitter.com/CloudflareRadar) (X),[https://noc.social/@cloudflareradar](https://noc.social/@cloudflareradar) (Mastodon), and[@](https://bsky.app/profile/radar.cloudflare.com)[radar.cloudflare.com](http://radar.cloudflare.com/) (Bluesky).
