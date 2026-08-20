---
schema_version: "1.0.0"
document_id: "bdf4177edcdd4970f06363a6643216ae5da05dc6f2fb3d97157a561bee53a31e"
company_key: "yc-gordian-software"
company: "Gordian Software"
source_id: "yc-gordian-software-news-import-71ddcd1d5e14"
canonical_url: "https://www.gordiansoftware.com/blogs/the-firewatcher"
published_at: null
first_seen_at: "2026-07-24T10:51:45.719052+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:65a695c28f0a63c01687f39bc8eca444aa6ab3dde3f2ee67cb37d29496582be5"
---

# The Firewatcher

From the lookout station, the Firewatcher scans the horizon. In the distance, they see something so subtle that most would miss - a small, rising plume of smoke against the clouds. With their compass, map and alidade, they quickly calculate the smoke’s location and relay it to fire suppression teams.


‍


At Gordian, we also have a Firewatcher. They’re on constant lookout for potential problems among the hundreds of services we’ve integrated. Instead of maps and alidades, they use comprehensive monitoring and alerting tools that track every part of our system. At any given second, our systems are processing critical transactions. From purchasing bags to securing the last seat on an urgent flight home, our partners depend on these systems to serve their customers.


‍


The Firewatcher is our first-responder for issues that may impact our partners. If there’s an incident, they coordinate the response. Every engineer at Gordian is trained to be on Firewatch, and each of us takes on week-long shifts.


‍


Most of the time, Firewatch is quiet. Our systems hum along while handling tens-of-thousands of transactions. But every now and then, an airline we’ve integrated has an unexpected outage. When this happens, here’s how we respond.


‍


##### Smoke on the Horizon


Like smoke against clouds, a few bad requests among thousands of successful ones could be easy to miss. **** However, **** our alerting systems are so sensitive that we often detect an airline outage before the airline themselves have even noticed the outage. Taking the role of Firewatcher, I will immediately begin pulling transaction traces using our custom-built logging and tracing tools. Analyzing these traces will allow me to quickly confirm the outage and determine its impact on our systems.


‍


##### Fire Suppression


The next step would be mitigation. I will call in an additional engineer to assist with communications while asking others to remain on standby. The supporting engineer will contact the airline’s technical team directly and assist them in diagnosing the issue. While this is happening, I will notify our partners about the outage. While an airline outage is a critical issue, our systems are built to handle this and automatic failovers would already be taking place. This could include using alternate services or temporarily halting transactions with the problematic airline. We will quickly relay updates from the airline to our partners while keeping a close-eye on our systems. As the airline’s services come back up, we will see our alerts close and metrics gravitate to nominal levels. We then inform our partners that all services are back online.


‍


##### Reflection


Once the incident has closed, we do what we call a “Reflection” every Friday. Here, I would create a detailed writeup, including a timeline and analysis of the incident. Our team would evaluate how we handled it and determine how we could further optimize our response. We may even throw-in hypothetical curveballs - what if there was a simultaneous outage in another service? - to test our ability to respond. We all document our ideas, which could include tuning alerts or building additional failovers, then assign them out to engineers to implement. The process of Reflection keeps us sharp and prepares us for situations regardless of whether we’ve seen them before.


‍


##### Impact


Behind our systems and the hundreds of services we’ve integrated is the Firewatcher keeping a close and careful eye. They’re the first to spot a potential problem, the first to coordinate a response, and often the first point-of-contact for our partners. As a result, their actions have immediate impacts in ensuring our systems run smoothly.


‍


The Firewatcher is one of the most important roles at Gordian. Equipped with our monitoring and alerting tools, they’re ready to call-in a team and mobilize a response at even the most subtle sign of smoke.
