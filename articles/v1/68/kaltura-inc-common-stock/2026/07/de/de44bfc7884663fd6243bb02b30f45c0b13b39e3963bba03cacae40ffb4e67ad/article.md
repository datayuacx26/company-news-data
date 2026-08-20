---
schema_version: "1.0.0"
document_id: "de44bfc7884663fd6243bb02b30f45c0b13b39e3963bba03cacae40ffb4e67ad"
company_key: "kaltura-inc-common-stock"
company: "Kaltura Inc."
source_id: "kaltura-inc-common-stock-news-import-bd99a5f9c66a"
canonical_url: "https://corp.kaltura.com/blog/kaltura-delivers-2026-fifa-world-cup/"
published_at: "2026-07-23T10:18:48+00:00"
first_seen_at: "2026-07-23T13:15:34.599785+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:1ce54cf01b348842d567a598a94ce7293815671e528cf011cedda22066cd5c80"
---

# How Kaltura helped deliver the 2026 FIFA World Cup to millions—with AI, engineering, and zero drama

Behind every unforgettable sporting moment is an invisible technology story. During the 2026 FIFA World Cup, millions of viewers across Europe, Asia, and the Middle East watched through TV services powered by Kaltura. For fans, it was as simple as turning on the TV and selecting a match. Behind that effortless experience were robust architecture, operational discipline, and a new approach to AI-powered monitoring.


The World Cup places extraordinary demands on television platforms. Viewers often log in just before kickoff, creating sudden spikes in authentication, entitlement checks, playback requests, channel changes, and content delivery. At this scale, even a brief bottleneck can affect huge audiences and put intense pressure on operators and engineering teams.


Throughout the tournament, Kaltura supported broadcasters and telecom providers serving approximately 40 million registered devices across multiple production environments in Europe, Asia, and the Middle East. The platform powered live TV, VOD, catch-up, recording, entitlement, content ingestion, household management, and other essential backend services.


But the real success story was not only the scale Kaltura handled. It was how differently the team operated compared with the previous World Cup.


## What the 2022 World Cup taught us


During the 2022 World Cup in Qatar, Kaltura successfully supported customers throughout the tournament, but doing so required significant manual effort. Engineers continuously monitored infrastructure dashboards, joined daily command centers and live bridges, reviewed countless metrics, and manually searched for anomalies during peak viewing hours. It wasn’t uncommon for five or six engineers to be refreshing the same observability dashboards simultaneously, creating a highly reactive operating model that placed tremendous pressure on the team.


The tournament was a success, but it exposed an uncomfortable truth: the platform still depended too heavily on human endurance. That realization shaped the next four years of development. Rather than adding more people or more dashboards, the team set out to build a more resilient platform and an intelligent operating model that could automatically surface the right insights, allowing engineers to focus on solving problems instead of searching for them.


## Preparing months before kickoff


Preparation for the 2026 tournament began months in advance with a pre-mortem exercise. Product managers, customer success managers, integration teams, QA specialists, support teams, DevOps engineers, and developers came together to identify everything that could potentially go wrong—and fix it before kickoff.


That process led to numerous improvements. Production environments were frozen during critical periods, heavy background workloads were moved outside match windows, and teams analyzed customer traffic patterns to anticipate regional viewing spikes and ensure systems could scale accordingly.


The work extended far beyond tournament planning. Over the previous four years, Kaltura had fundamentally modernized its platform by breaking apart a large monolithic backend into independent microservices. This significantly improved resilience and scalability, allowing individual services to scale independently and limiting the impact of failures to isolated components instead of the entire platform. It provided the robust architectural foundation needed to support World Cup-scale traffic with confidence.


## Replacing manual monitoring with AI-powered operations


The next step was transforming how the platform was monitored. During the 2022 tournament, engineers spent hours moving between dashboards, logs, alerts, and observability tools. For 2026, the team built a family of AI agents to automate much of that work.


The


**System Health Agent** analyzed multiple production environments, pulling data from Grafana, Coralogix, and OpsGenie to summarize the previous 24 hours, highlight anomalies, identify scaling issues, and direct engineers to areas needing attention.


A dedicated


**World Cup Agent** monitored every match before, during, and after play, providing near real-time summaries instead of waiting for daily reports. An


**SRE Agent** investigated logs, request volumes, error patterns, and historical trends across multiple systems, returning concise operational insights on demand.


Named after football legends Pelé, Maradona, Maldini, and Iniesta, the agents did more than add personality. They dramatically reduced repetitive monitoring while making expert operational knowledge instantly available, allowing engineers to focus on solving problems instead of searching for them.


## From five engineers on a bridge to one


The impact on day-to-day operations was dramatic. During the 2022 World Cup, five or more engineers would manually monitor dashboards during key matches. By 2026, a single support engineer could oversee operations while AI agents continuously monitored system health, gathered data from multiple observability tools, and delivered prioritized updates through our operational channels. Reports that once took three or four hours to prepare were generated in about five minutes, allowing engineers to focus on resolving issues instead of searching for them.


The agents surfaced anomalies, summarized findings, and recommended actions such as scaling capacity or adjusting thresholds, but engineers remained in control of every production change. By automating the repetitive work while keeping humans in the loop, Kaltura transformed its operational model without sacrificing reliability.


## Learning from the tournament in real time


One of the most valuable lessons came during an early match, when login traffic exceeded an API rate limit. Although rate limiting is designed to protect platforms from excessive requests, the predefined threshold proved too low for legitimate World Cup traffic. Rather than simply increasing the limit, the team built a new


protection mechanism


during the tournament to continuously monitor API consumption, alert engineers as usage approached critical thresholds, and recommend when limits should be adjusted.


The team deliberately kept humans in control of production changes, since traffic spikes could indicate either genuine demand or a software issue. Instead of making automatic adjustments, the agent provided early warnings and actionable recommendations, enabling engineers to address potential problems before customers noticed them. The result was a platform that was continuously learning and improving.


##


## What the traffic told us about viewers


The tournament also produced fascinating insights into viewer behavior. Traffic graphs mirrored the rhythm of the matches so closely that engineers could often tell what was happening on the field without watching the broadcast. Activity surged before kickoff, dipped during halftime and hydration breaks, climbed again as play resumed, and dropped almost immediately after the final whistle, with extra time creating its own distinct traffic pattern.


Regional viewing habits were equally clear. In France, traffic peaked during French national team matches, while in Singapore, audiences showed particularly strong interest when Argentina or England played. These patterns helped the team anticipate demand and focus attention on the customer environments most likely to experience heavy loads.


The data also reinforced the unique nature of live sports. During the tournament, catch-up and video-on-demand viewing declined by 20% to 30% as audiences shifted to live broadcasts.


## The 2026 World Cup in numbers


The scale and efficiency of the project can be summarized in a few key figures:


- **40 million** registered devices supported across Kaltura-powered TV services


- **30 million** devices monitored daily


- **1.3 billion+** playback sessions during the tournament


- **7 million** peak concurrent sessions


- **Approximately 86% more scale** than during the 2022 World Cup


- **6** production environments monitored across multiple regions


- **1** engineer on the live operations bridge, compared with 5 in 2022


- **0** manual Grafana refreshes, daily war-room calls or major incidents


- **4 hours of analysis reduced to approximately 5 minutes**


- **80% fewer engineers** required on the live bridge


These results were not achieved by simply adding infrastructure or increasing headcount. They came from four years of sustained investment in architecture, microservices, observability, operational planning, and AI-powered workflows.


## Reliability that viewers never had to think about


The best outcome for an engineering team supporting live television is that viewers never notice the technology at all. They do not see the infrastructure scaling behind the scenes, the agents reviewing system health, or the teams preparing for traffic spikes. They only see that the stream starts when they press play and continues through the final whistle.


That is what made the 2026 World Cup such an important success story for Kaltura’s Media & Telecom team. The tournament placed more demand on the platform than ever before, yet it required fewer people, less manual intervention, and far less operational tension.


Kaltura turned one of the most complex and high-pressure events in television into a controlled, predictable operation. After four years of architectural patience and operational innovation, the biggest compliment was also the simplest: behind the scenes, the tournament was almost boring.


And in live television, boring is a remarkable achievement.
