---
schema_version: "1.0.0"
document_id: "4398ed6db1561c0c74eb22b8773fb39f41e13e94d764bcc945ca1e007fac4e6d"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/the-invisible-stadium/"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T18:48:00.530258+00:00"
fetched_at: "2026-08-13T18:48:01.242324+00:00"
content_hash: "sha256:4da8d2963ab0322a9c2e952df6ef447b9bdaa52e0b5a752f65089abd4731b20e"
---

# The Invisible Stadium

### Inside the edge infrastructure, humans, and AI behind the World Cup


When an estimated 1.8 billion people tune in to a single match, as they did in the last game of the World Cup, the entire internet reshapes itself around them.


Traffic on our network surged during the final. What stood out was how traffic across unrelated categories inverted in real time.


Gaming traffic dropped 18 - 20% below baseline as millions of people put down their controllers. Meanwhile, social traffic surged ~25%, driving sharp micro-spikes that lined up second-for-second with every major moment, from Enzo Fernández’s red card to Ferran Torres’s goal. Then, within 45 minutes of the final whistle, the global network baseline snapped right back to normal.


*Note: Data has been indexed for visualization purposes. Category traffic is displayed relative to scale.*


Looking at the telemetry, the green curve shows World Cup streaming traffic ramping up ahead of kickoff and sustaining a high plateau throughout the match. At the exact same time, gaming traffic (yellow) drops steadily in direct proportion. But the most volatile signal is social traffic (blue), spiking sharply at the most important moments of the match.


## From Single-Match Spikes to Tournament Scale


Across 39 days and 104 matches, the tournament evolved from steady 12-hour daily plateaus into hyper-concentrated global spikes that reached a huge aggregate peak during the final.


Delivering uninterrupted, zero-outage performance on the biggest stage didn't happen by accident, it was engineered. It required **predictive capacity planning, autonomous edge routing with inline security, and dedicated human engineering effort on live bridges.**


### Predictive Telemetry and Capacity Planning


Preparing for an event of this scale begins months before kickoff. When streaming, media, and platform customers experience simultaneous traffic surges, capacity allocation becomes critical. To build a global infrastructure roadmap, our teams work directly with customers months ahead to map out bandwidth projections and regional infrastructure requirements.


We use machine learning models to help us make capacity planning decisions. By ingesting and synthesizing massive streams of real-time and historical network data, our AI-driven modeling tools give engineering teams faster, more precise visibility into regional traffic demand while keeping experienced operators firmly in control of architectural decisions.


During the group stage, for example, our telemetry models flagged an unexpected surge in viewership. The tooling projected the necessary regional topology adjustments instantly, allowing our infrastructure teams to accelerate localized buildouts. We moved from initial telemetry alerting to live traffic distribution in a matter of days.


### Autonomous Edge Routing and Security


While predictive models handle capacity planning, live stream delivery requires sub-second execution at the edge layer.


During the tournament, our underlying network made millions of autonomous, instant routing decisions per second. Systems like[Autopilot and Precision Path](https://www.fastly.com/blog/preventing-outages-with-resilient-architectures) continuously evaluated transit provider health and backbone performance, automatically executing sub-millisecond reroutes around upstream bottlenecks.


Simultaneously, live sporting events attract high volumes of malicious activity, including automated stream piracy, credential stuffing, account takeover attempts, and volumetric DDoS attacks. Driven by the rise of AI tools, these threats are becoming faster and more sophisticated. Because performance and security are inseparable at this scale, Fastly acts as the first line of defense, bringing extensive expertise in combating fraud and piracy directly to the edge.


Midway through the tournament, our teams deployed anti-piracy mitigations directly across live distribution paths. Powered by our[Adaptive Threat Engine](https://www.fastly.com/blog/mitigating-ddos-attacks-faster-and-with-even-more-accuracy) and tools like[ContentGuard](https://www.fastly.com/blog/adapting-in-the-era-of-ai) , our platform actively filtered out unauthorized distribution streams inline at the edge without adding latency to legitimate viewers or overloading customer origin servers.


### Dedicated Humans on Live Bridges


No matter how advanced the automated routing or tooling, high-stakes infrastructure requires expertise on the front lines.


A core value at Fastly is serving as a direct extension of our customers’ teams and technology. Throughout all 104 matches, our engineering teams maintained dedicated, shared live bridges with customer technical leads. This was backed by months of upfront preparation, developing joint runbooks, building shared observability dashboards, and executing parallel failure scenario dry runs. Fastly and customer teams operated off identical, real-time telemetry, monitoring shared health metrics during gameplay to catch and address potential bottlenecks before they impacted end users.


Managing the tournament wasn't a single event, but it was executing hundreds of micro-events, backed by experienced teams wielding powerful tools.


### Building Resilience for Peak Traffic


The World Cup only happens every four years, but handling real-time traffic surges, security threats, and sub-second routing isn't just for the big games. It’s what modern infrastructure needs to do every day.


When technology is built to handle that kind of global scale without dropping packets or experiencing customer outages, those resilience patterns become the baseline standard for the entire platform. The ultimate goal is to keep the infrastructure invisible so the experience is all viewers remember.


Planning for your next peak traffic event?[Reach out to the Fastly team](https://www.fastly.com/contact-sales) to learn more about how we can help.
