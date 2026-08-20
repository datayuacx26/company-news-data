---
schema_version: "1.0.0"
document_id: "d07ba5ecc364b9950e6c0e891fcc4e95e1fef5747ec6f459e9354171bd0ebcfa"
company_key: "criteo-s-a-american-depositary-shares"
company: "Criteo S.A."
source_id: "criteo-s-a-american-depositary-shares-rss-02db2411825d"
canonical_url: "https://medium.com/criteo-engineering/adtech-culture-diversity-and-ai-notes-from-our-latest-women-in-tech-meetup-103f4d82257a"
published_at: "2026-04-08T06:56:01+00:00"
first_seen_at: "2026-07-20T23:17:33.645392+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:a48afb079865b259fdd545f80c8a1a5d609713b46629dab41d1fe449c929f30b"
---

# AdTech, culture, diversity, and AI: Notes from our latest Women In Tech Meetup

Adtech


Women In Tech


AI


Diversity


Culture


# AdTech, culture, diversity, and AI: Notes from our latest Women In Tech Meetup


## Author:[Andreea Dragomirescu](https://www.linkedin.com/in/andreea-dragomirescu-36a632135/)


[Criteo Tech](https://medium.com/@criteotech?source=post_page---byline--103f4d82257a---------------------------------------)


11 min read


·


Apr 8, 2026


--


On a weeknight in Bucharest, Criteo opened the doors of its new tech hub for a Women in Tech meetup under a simple but clear theme: **Driving Change in Tech** . The agenda moved from a deep‑dive keynote on ad tech at scale to a panel about culture, diversity, and AI, and finally, networking.


Press enter or click to view image in full size


The result felt less like a formal presentation and more like a series of honest conversations about how a global ad tech company actually works — and what kind of culture you need behind the infrastructure.


## Keynote: How to Handle 1 Trillion HTTP Requests/Day


The central technical talk of the evening, *“How to handle over 1 trillion HTTP requests/day: Behind the scenes of the ad tech industry”* , by Diana, one of our devleads in Bucharest.


Press enter or click to view image in full size


### The TV that follows you everywhere


Diana anchored the theory in a simple real‑world flow: you browse a retailer’s site for TVs, click one, decide not to buy, then later see that same TV advertised on a news site. From your point of view, it’s either spooky or convenient. From the system’s point of view, there’s a long chain of events involving the retailer, Criteo, publishers, and several ad tech platforms.


When you click the product on the retailer’s site, an event is sent to Criteo’s platform with information about what you looked at. Later, when you open the news site, the publisher’s **Supply‑Side Platform (SSP)** sees an available ad slot and sends a **bid request** to an **ad exchange** . The ad exchange then broadcasts that request to multiple **Demand‑Side Platforms (DSPs)** — Criteo and others.


Press enter or click to view image in full size


Each DSP has only a few tens of milliseconds to decide whether this specific impression is worth bidding on and at what price. If Criteo deems it valuable and returns a bid, that bid competes in a **Real‑Time Bidding (RTB)** auction with all the others. The winning DSP’s creative is returned to the browser and rendered in the slot. If Criteo wins, it still has to choose which product to show out of the retailer’s catalog — and often that catalog contains **billions** of items.


## Who Does What: SSPs, DSPs, and the Exchange


Diana split the ecosystem into three big blocks:


- **SSP (Supply‑Side Platform)** — runs on the publisher side, managing the inventory of ad slots: positions on the page, sizes, media types, and floor prices. Its job is to maximize revenue for the publisher without compromising brand safety. Mechanisms like` ads.txt` and global blocklists help ensure only authorized sellers and safe creatives can appear on those sites.
- **DSP (Demand‑Side Platform)** — runs on the advertiser side, deciding which impressions to bid on, how much to bid, and which creatives or products to serve. It handles targeting and audience definitions, campaign management (including budgets, pacing and frequency capping), creative management, and a lot of machine learning that estimates the value of each impression.
- **Ad Exchange** — the marketplace in the middle. It receives bid requests from SSPs, fans them out to DSPs, collects bids, runs the auction in ~100ms and returns the winning creative to the browser.


Beyond simply “serving an ad”, the system also has to decide whether an ad was **viewable** . Industry standards say an ad is viewable if at least 50% of its pixels are on screen for more than one second, so the system measures and logs that as well.


Press enter or click to view image in full size


### Feedback Loops: Hot, Warm, Cold


To keep improving performance, ad systems are constantly learning from what just happened. Diana described three feedback loops:


- The **hot loop** (real‑time) tracks signals like clicks, conversions, viewability and time on screen, and uses them for immediate tactical adjustments.
- The **warm loop** (minutes to hours) monitors budget consumption and fraud signals — such as abnormal volumes from specific apps or IP ranges — and adjusts pacing, spend and filtering rules accordingly.
- The **cold loop** (hours to days) aggregates all that data into training sets for machine learning models that will drive future bidding decisions, recommendations and fraud detection.


Press enter or click to view image in full size


### Latency, Scale, and Privacy: Constraints That Define the System


The talk then pivoted to constraints.


**Latency** is unforgiving. Network delays often outweigh compute time; the RTB window is tiny; any timeout is lost revenue. To cope, Criteo keeps as much data as possible in memory, as close as possible to users geographically. Geo‑distributed data centers keep round-trip times short, asynchronous communication minimizes blocking, and the team pays attention to efficient protocols, payload sizes, and progressive loading.


**Scalability** is shaped by very uneven traffic. During Cyber Week or other big retail events, the volume of bid requests and clicks can spike sharply. The architecture leans heavily on horizontal scaling: more instances rather than larger ones, **stateless** business‑critical services, **Kafka** for streaming and partitioning, **Kubernetes** for autoscaling, and explicit **backpressure** and **graceful degradation** strategies so the system can drop non‑essential features before collapsing entirely.


**Privacy** has been a moving target since the so‑called “cookie apocalypse”, with third‑party cookies being phased out in Chrome and other browsers. Instead of relying on stable user IDs, Criteo and the wider industry increasingly rely on probabilistic models, contextual targeting (using the page’s content and context rather than personal history), on‑device computation, and aggregated/anonymized data. Pipelines are designed for shorter data retention and models are trained to cope with weaker signals and partial histories.


### Infrastructure: Data Centers, Clusters, and Microservices


Criteo runs **11 data centers** across the globe on its own bare‑metal infrastructure rather than fully managed cloud, partly for latency reasons and partly for cost efficiency at this scale. Across those centers, they operate thousands of servers, more than **1 million CPU cores** , **1.5 petabytes of RAM** and around **250 petabytes of storage** .


Press enter or click to view image in full size


Diana split the high‑level architecture into **online** and **offline** parts:


- Online, Linux containers on **Kubernetes** clusters serve real‑time applications: ad serving, bidding, campaign management APIs, etc., mainly implemented in **C#** on **.NET Core** , backed by **Aerospike** and **Kafka** .
- Offline, a large **Hadoop/Spark** cluster, with about **4,300 nodes** , runs batch jobs and machine learning training pipelines, mostly written in **Scala + Apache Spark** and **Python** , orchestrated by in‑house schedulers. **SQL Server** appears both online and offline, where relational queries make sense.


Over time, Criteo migrated from a more monolithic design to a **microservices** architecture. Now there are many small services, each with tight responsibilities. Only a limited number are exposed externally; inside, services can be deployed frequently — sometimes daily — guarded by extensive automated tests so teams can ship changes without constantly fearing regressions.


To make sure this all holds during peak periods like Black Friday, Criteo puts in place a **deployment freeze** for about a month before Cyber Week, scales services up in advance, and sets clear on‑call rotations. The “behind the scenes” tooling slide listed the following: SDKs for logs, **Elasticsearch/Kibana** for log search, **Prometheus** for metrics, **Grafana** dashboards, **Sentry** for front‑end errors, **Slack** for alerts and collaboration, and **PagerDuty** for paging engineers when things go wrong.


### Where AI Fits


A key part of the keynote was showing that AI at Criteo is not a buzzword but infrastructure. The **Criteo AI Lab** , founded in 2018, feeds into many parts of the stack:


- **Audience management/targeting** — deciding who should see which ad.
- **Predictive bidding** — estimating how valuable a given impression is likely to be (click, conversion probability, expected revenue).
- **Controllers** — monitoring and steering campaign performance based on observed outcomes.
- **Supply bidding** — deciding how much Criteo is willing to pay for inventory on specific sites or apps.
- **Recommendations** — choosing which products to show a user at a given moment.
- **Creative optimization** — adjusting banners and layouts, and sometimes generating variants on the fly.


Diana closed by looking forward: **commerce intelligence** , where more of the “user actions” in commerce are taken by AI agents rather than humans, will push even more strongly towards **API‑first** ecosystems and protocols like MCP.


## Panel Discussion: AI Is in Our DNA, But Is Culture the Secret Ingredient?


On stage were **Anca** (Engineering Program Manager), **Alice** (Software Development Engineer), **Cecile** (Senior Lead Talent Acquisition), **Cristina** (Tech Talent Sourcer), and **Andreea** (Staff Dev Lead). Together, they dug into how diversity, psychological safety, and AI interact in real engineering teams.


Press enter or click to view image in full size


### What an EPM Does at Criteo


Anca explained the **Engineering Program Manager (EPM)** role as driving complex engineering programs and projects end‑to‑end. That includes structuring and planning work, aligning product and engineering teams, identifying and managing dependencies, and making sure that execution actually matches the strategy.


There’s an element of **leading with authority** , on the way work is prioritized and run, and **leading without authority** , where you influence product and technical decisions among people who don’t report to you directly. A short summary she offered was: helping teams **“do things right”** together.


### What Criteo Looks For in Engineers (Beyond Skills)


Cecile then talked about hiring from a culture perspective. Building on what she’d shared earlier and on the R&D values slide, she highlighted three things she consistently looks for:


- An **engineering mindset** : people who enjoy creating things from scratch, tackling complex systems and stepping back from their own solutions to critique and improve them.
- Comfort with **complexity** : the scale and architecture at Criteo mean you rarely work on isolated, trivial problems.
- **Collaboration and communication** : someone you’d genuinely want to work with. In a hybrid setup where your manager and teammates may be in another country, being able to communicate blockers, ask for help and share context is critical, not optional.


### Psychological Safety: “You Can’t Fix a Secret”


Alice moved the discussion onto **psychological safety** , using Amy Edmondson’s classic definition: it’s the belief that you won’t be punished for speaking up. She tied this to a motto Criteo likes: *“you can’t fix a secret”* . If people feel they can’t say “I don’t understand this requirement” or “I think this design is risky”, those issues don’t disappear. They just become silent broken features.


Press enter or click to view image in full size


She shared that in her own career, she’s been lucky to land in environments where she was genuinely given time to ramp up on new tech, encouraged to ask questions and included in team decisions. That continuity of psychological safety made big changes (like stack switches or job moves) feel manageable rather than threatening.


Cristina, coming from a non‑engineering background, echoed that. In teams where psychological safety is present, she could bring her own expertise into conversations with engineers, share both fears and accomplishments, and know that she had a seat at the table even if she wasn’t writing the code.


The takeaway was simple: without psychological safety, diversity stays quiet and potential issues stay invisible.


### AI as an Amplifier, Not a Fix


Andreea and Anca then connected these cultural points back to AI, referring to the **DORA “**[State of AI‑assisted Software Development](https://cloud.google.com/resources/content/2025-dora-ai-assisted-software-development-report?gclsrc=aw.ds&gad_source=1&gad_campaignid=20645498047&gclid=Cj0KCQjw9-PNBhDfARIsABHN6-34nbzoybgoIHGTop7M5F1B_FHt90Yc3i4TaP08_IJJn3TeHEnzavIaAiyKEALw_wcB) **”** report. The research found that AI’s primary role is that of an **amplifier** : it magnifies the strengths of high‑performing organizations and the dysfunctions of struggling ones.


In practice, that means you can’t sprinkle AI on top of a broken process and expect miracles. AI will happily accelerate whatever patterns it finds: good or bad. If your teams have diversity, psychological safety and a healthy approach to change, AI can boost performance and quality. If they don’t, AI just helps you ship biased, brittle or misaligned decisions faster.


Anca offered a helpful way to think about the system as four interconnected conditions:


- **Diversity** — brings more perspectives, better error‑correction, more creative solutions.
- **Psychological safety** — lets that diversity actually show up in decisions, instead of staying silent.
- **AI** — accelerates what’s already present in the system.
- **Change capability** — the “muscle” that lets teams adapt continuously rather than treating change as a one‑time event from A to B.


Press enter or click to view image in full size


In a world where decision timelines are compressed and more work is being automated, she argued that B is often not a fixed destination; it keeps moving. So the only sustainable approach is to build that change muscle and keep using it.


### Women in Tech: Myths, Facts, and Data


The panel wrapped up with a quiz: **“Women in Tech: Myths and Facts”** . Attendees answered questions live via Mentimeter about promotion rates, impostor syndrome, remote work, allyship, history, and bias.


Some highlights:


- For every **100 men** promoted to their first engineering lead or manager role, only **93 women** are promoted. This is the “broken rung” on the corporate ladder and it happens at the very first management step.
- **Impostor syndrome** affects all genders, but women are **22% more likely** to report it, often due to a lack of visible senior role models in engineering.
- Around **90% of women in tech** rank remote and hybrid work as their most valued benefit for avoiding burnout and maintaining work–life balance.
- **77% of male tech executives** consider themselves active allies to women in engineering, but only **45% of women engineers** agree with that assessment — a clear “perception gap”.
- Computer science hasn’t always been male‑dominated: in **1984** , women earned **37%** of all CS degrees in the US, and earlier decades often viewed software as “women’s work” before status and pay shifted the culture.
- AI hiring systems are not automatically neutral. Amazon famously found that an internal hiring model trained on male‑heavy historical data had learned to penalize CVs containing gendered markers like “Women’s” (e.g., “Women’s Volleyball Team”).
- While there is some skew towards front‑end roles, women make up a **significant and growing share** of backend, cloud and cybersecurity engineering — roughly **24%** according to cited stats.
- The first widely recognized computer programmer was indeed a woman: **Ada Lovelace** , for her work on the Analytical Engine in **1843** .
- When a woman’s gender is identifiable on GitHub, her pull request acceptance rate drops by about **10%** , highlighting reviewer bias rather than code quality as the underlying issue.
- At the current rate of progress, the **global gender gap** in tech will not be closed by 2040; the projection is more like **123 years** .


The formal program ended there, and people moved to the cafeteria for food, drinks, and conversations. That’s where the real value of meetups tends to show up: lots of chats that you never see on slides, but that ultimately decide whether “driving change in tech” is just a tagline or something people actually feel part of.


If you want to be part of this amazing team in our Bucharest office, check our open positions. We are hiring! 😃


[Your next adventure awaits! Explore new opportunities at Criteo. Browse our list of open jobs and see if you can find one matching your skillset… careers.criteo.com](https://careers.criteo.com/en/jobs/?keyword=&country=Romania&team=Engineering&pagesize=10&source=post_page-----103f4d82257a---------------------------------------#results)
