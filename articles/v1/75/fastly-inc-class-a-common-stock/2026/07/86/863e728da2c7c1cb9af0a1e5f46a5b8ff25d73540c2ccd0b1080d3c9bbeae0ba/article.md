---
schema_version: "1.0.0"
document_id: "863e728da2c7c1cb9af0a1e5f46a5b8ff25d73540c2ccd0b1080d3c9bbeae0ba"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/delivering-a-resilient-world-cup-final/"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-20T19:40:48.579423+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:f5a8af9f6bd7cbf426503c042e4fd58f148a5d2bac4fd43d5355bd840091eeea"
---

# Delivering A Resilient World Cup Final

The 2026 World Cup has concluded! Congratulations to Spain for a thrilling, extra time victory over Argentina. Beyond the on-field drama and the match itself, delivering a high-quality broadcast to an expected 1.8 billion people worldwide requires an intense focus on[resilience](https://www.fastly.com/blog/fastlys-pillars-of-resilience-building-a-more-robust-internet) , performance, and system flexibility. Across major global live events, Fastly’s Edge Cloud Platform routinely supports broadcast networks, OTT providers, and digital publishers to maintain stream integrity under massive, sudden traffic surges.


But enabling a great live broadcast requires more than the exceptional scale and speed we’re named after. It requires deep intelligence to secure and protect these broadcasts against account takeover attacks,[DDoS attacks](https://www.fastly.com/blog/mitigating-ddos-attacks-faster-and-with-even-more-accuracy) , and piracy. These threats have existed since the internet's invention, but the current age of AI is changing the shape and complexity of these attacks. Fortunately for our customers, we’re also continuously evolving our research and products to detect and mitigate them proactively.


## Mitigating Piracy and Automated Bot Threats at the Edge


During high-volume, live sporting events, we work closely with our customers to navigate live stream piracy at the edge, stopping threats dynamically and in real time throughout the broadcast.


By analyzing multiple signals in every request, from client fingerprints to where the traffic originates, we can distinguish legitimate viewers from pirate viewers, scrapers, and botnets. This includes malicious actors trying to disguise themselves as trusted players or browsers. We also help customers secure the delivery path itself, so internal infrastructure can't be exploited as a backdoor to premium content.


These efforts combine technology, operator expertise, and joint real-time response to build a layered defense-in-depth strategy that raises the cost of piracy without adding friction for real fans.[We’ve written about our work in the past](https://www.fastly.com/press/press-releases/fastly-and-laliga-team-up-on-joint-innovation-to-combat-piracy) , but this practice is a continuous research effort for our engineering and customer teams.


The result, in the case of the World Cup, was an amazing set of quarter-finals, semi-finals, third-place, and final match experiences for customers and an expected 1.8 billion viewers. Let's dive into our findings for these matches across our platform. For those curious, the data through this series has been, unless stated otherwise, a minute sample of traffic across our global platform during each match.


## 2026 World Cup Final: Analyzing Peak Traffic Spikes


As expected, the viewership for the World Cup final match was higher than the third-place match (and earlier broadcasts as well). In the finals, the stakes are at their absolute highest, and teams are the most energized. Fans know this, and log on accordingly. However, the third-place match was still between two elite teams with passionate fan bases that drive meaningful viewership. So, how close were they?


When we saw the data unfold, the difference was substantial: the final match between Spain and Argentina drove nearly double the traffic of the third-place match between England and France, when comparing peak viewership. There’s nothing in our data indicating that viewers who watched the third-place match were “truer fans” than those who watched only the final, so we’ll leave that classification as an exercise for the reader.


Perhaps unsurprisingly, the World Cup final had the highest viewership of the tournament, according to our data. Also, the fact that the final match had nearly twice the traffic of the third-place match was expected.


What we didn’t expect was that as the final went into extra time, viewership growth continued in the same pattern we’ve observed throughout this series. We assumed that the World Cup final would have already reached its maximum viewership by mid-game, but there’s evidence of still unmet demand for the most prestigious sports championship in global football.


### How Knockout Stakes Drive Audience Retention


Even though they didn’t produce the viewership totals or carry the prestige of the final, let's take a look at the quarter-final and semi-final rounds. While there were fewer matches in these two stages than the prior knockout rounds, let alone the number of matches in the largest-ever group stage, there were still some interesting insights in the data.


For the quarter-finals, the standard deviation of peak viewership for the second half of matches was noticeably lower than in the round of 16. Specifically, the standard deviation was roughly 15% for the quarter-finals, compared with 25% for the round of 16. We can conclude that when the stakes are higher, people stay glued to their devices throughout the entire match.


Another data trend to that effect: with each stage of the tournament through the quarter-finals, halftime viewership incrementally increased. Average log-off rates during halftime were: 24.65% in the group stages, 22.78% in the round of 32, 22.55% in the round of 16, and 20.15% in the quarter-finals. We’ve said it before during this series, but the data continue to confirm that high-stakes football drives more, and more consistent, viewership. Even during the halftime breaks.


Zooming out, when comparing the quarter-finals and semi-finals matches side by side, clear trends in viewership emerge. While the semi-finals generated more viewership volume than the previous rounds, one quarter-final match was an anomaly: England v. Norway. In this match, underdog Norway briefly appeared capable of pulling off a major upset, forcing England into extra time before finally securing their victory. This was another example of a high-stakes match tied at the end of regulation resulting in a surge in viewership.


## Scale and Secure Your Next Streaming Event


When the stakes are highest, platforms need to rise to the occasion. The World Cup, perhaps more than any other event, demands excellence. Every frame of every match matters – the world is watching. Successfully supporting our customers throughout the tournament window required a combination of technological innovation, disciplined preparation, and deep partnership to achieve their goals.


Whether you're preparing for major live sports, global product launches, or other live streaming events, Fastly's high-performance edge network is engineered to handle peak traffic without compromising security. If you're responsible for scaling systems and customer experiences where performance matters, every second counts, and security is non-negotiable, we are ready to support you! When millions tune in, every millisecond counts –[speak with a Fastly expert today](https://www.fastly.com/contact-sales) to ensure your network is fast, resilient, and secure before your next major traffic surge.
