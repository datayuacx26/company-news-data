---
schema_version: "1.0.0"
document_id: "a688155761d271c2366f6a809be9bb236e379c2664f01ec6a1ef6f2f916daa3a"
company_key: "yc-integuru"
company: "Integuru"
source_id: "yc-integuru-news-import-ab81679661d6"
canonical_url: "https://www.integuru.com/blog/customer-penciled"
published_at: "2026-01-23T00:00:00+00:00"
first_seen_at: "2026-07-22T00:13:16.768883+00:00"
fetched_at: "2026-07-28T22:23:13.149633+00:00"
content_hash: "sha256:a1983c7266d55d9703a69011c15b7d95aa4493be58084e94824eab2564ef8b38"
---

# Customer Spotlight: Scaling the Penciled Agent with Real-Time EHR Integrations

I’m Richard, co-founder of Integuru. We generate the most robust APIs with platforms that lack official or accessible APIs.


For the most successful AI products that people rely on, latency, reliability, and throughput aren’t just nice-to-haves but necessities. These requirements are especially true in healthcare, where a 30-second delay or a missed appointment can break patient trust, disrupt operations, and lead to lost revenue.


[Penciled](https://penciled.com/) faced these latency and reliability problems. They’re an Initialized-backed healthcare AI startup building a messaging agent for physical therapy (PT) clinics. Their product helps clinics handle patient messages automatically, schedule appointments, manage cancellations, and perform patient intake. However, as their user base grew, their integrations with electronic health record (EHR) systems couldn’t keep up.


They solved that by using Integuru to move from fragile scripts to fast and reliable integrations. The result was more confident patients, stronger infrastructure, and direct revenue growth.


### **The Integration Problem: Frontend Scripts and Latency Walls**


When EHRs lack official or adequate APIs, Penciled’s initial integrations were built using **browser automation** . After patients contacted the clinic, the Penciled agent triggered a headless browser, loaded the clinic’s EHR system, and interacted with the frontend to complete actions like booking appointments or fetching availability.


This approach worked, but only for a while. Two problems stood out in particular:


**1. Breakage from UI Changes**


EHR systems change their frontends often. A slight layout shift or class name update would break the automation scripts. Engineers had to go in and fix things manually every time this happened. That led to frequent errors, downtime, and operational stress.


**2. Latency from Page Loads & Data Caching**


Even when it worked, browser automation was slow. Each action required the page to fully load before the next step could begin. For messaging interactions, this meant wait times of 30–40 seconds per task, which is far too long for a real-time conversation with a patient.


To reduce this, Penciled cached appointment data every ~30 minutes. But caching introduced a different problem: what if two patients tried to book the same slot before the cache refreshed? That meant real-world scheduling issues and a bad experience for everyone involved.


At this point, Penciled knew they needed something better. That’s when they came to Integuru.


### **The Fix: Direct HTTP Requests**


Instead of working through the frontend, Integuru helped Penciled move to **internal API connections** . This new approach means skipping the browser and sending requests directly to the backend systems powering each EHR platform.


Every modern web app has internal APIs. The frontend uses these hidden endpoints to communicate with the backend. Penciled could achieve fast, direct access to the needed scheduling and patient data by reverse-engineering these APIs.


At Integuru, we support this process end-to-end. We analyze network requests, decode authentication flows, and generate integration code for internal APIs. We make the final integration as easy and reliable to use as a public API.


### **The Result: Sub-Day Setup, Real-Time Usage**


Once the integrations were ready, Penciled’s team implemented them in production in **less than one day per integration** . The difference was immediate:


-


**99.99%+ Uptime** : The internal APIs proved far more stable than the frontend, reducing breakages to near-zero.


-


**Real-Time Responses** : On average, actions that used to take 30+ seconds now complete in 3 seconds.


-


**Live Data Syncing:** Penciled no longer need to cache data, which means that they always have the most up-to-date data from EHRs.


Most importantly, customers are happier. Clinics can trust the messaging agent to get things done. That trust translated into new customers and more revenue for Penciled.


Beyond the numbers, the Penciled team now has confidence that their infrastructure won’t break during patient interaction. They never wake up worrying about breakages or bottlenecks. They could focus on growing the product instead of putting out fires.


### **The Bigger Picture**


Today, many of Integuru’s customers are agentic products like Penciled. These products depend on integrations working reliably across third-party platforms.


When latency, reliability, or throughput becomes a bottleneck, Integuru is the path forward. And our goal is to make that path fast, affordable, and simple, without sacrificing capability.


### **Contact Us**


Let's talk if you’re working on a product that needs to connect to external platforms.


[Book a call with us](https://calendly.com/d/cqb8-d9x-nbf/integuru) to get started.
