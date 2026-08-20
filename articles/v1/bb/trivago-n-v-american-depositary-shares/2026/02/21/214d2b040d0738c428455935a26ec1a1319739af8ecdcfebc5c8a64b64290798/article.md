---
schema_version: "1.0.0"
document_id: "214d2b040d0738c428455935a26ec1a1319739af8ecdcfebc5c8a64b64290798"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2026-02-02-how-not-to-fight-with-product-managers-as-a-developer/"
published_at: "2026-02-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:22:47.419354+00:00"
content_hash: "sha256:a0ca6f8932236d2de87998b51f6ce3700f1aa02cc02e54706686809880098c4b"
---

# How Not to Fight with Product Managers - as a Developer

It’s a scenario developers relate to a little too well. The product manager always comes with more and more feature requests. They also want to release fast by giving a tight deadline. While you, as a developer, care more about stability, performance, and maintainability of your application.


This tension can easily turn into friction. But what if there was a way to align both sides; using data instead of relying on opinions? We are betting on a practice called *Service Level Objectives* (SLO). Can it stop the fight? 🤔


## **Why does conflict happen? ⚖️**


The root of many developer vs product manager disagreements comes from different perspectives:


- **Product managers** focus on **customer value and delivery speed** . They want features out the door as fast as possible, because they are the main drivers for user adoption and business growth.
- **Developers** focus on **quality and reliability** . They worry that rushing will lead to outages, unhappy users, and mounting technical debt.


Both parties have good intentions towards the business. But without a common language, conversations between product managers and developers can feel like an endless tug-of-war: “ship it fast” vs. “we need more time to make things stable”.


## **Enter SLO… A Shared Language 🎯**


[Service Level Objectives](https://en.wikipedia.org/wiki/Service-level_objective) (SLO) give both sides a clear, data-driven agreement on what is “good enough” for our user experience.


An SLO defines a target level of reliability (e.g., “Our search-engine API will return a successful response 99.9% of the time within 300ms over a 30-day window”). It’s based on user expectations, not gut feelings. When practiced correctly, SLOs shift discussions from opinion-based debates to fact-based decisions.


## **A Real-World Example 📊**


Imagine a critical system component; trivago’s *search-engine* SLO for *availability* is **99.9% uptime** over a 30-day period. This follows:


> Total minutes in this period → 30 days x 24 hours x 60 minutes → 43200 minutes
>
>
> Expected uptime → 43200 minutes x **99.9%** → 43156.8 minutes
>
>
> Budget for downtime → 43200 minutes - 43156.8 minutes → 43.2 minutes


So, setting availability target to **99.9%** means we can only have **43 minutes of downtime in a month** . More availability calculation[here](https://en.wikipedia.org/wiki/High_availability#Percentage_calculation) .


In other words:


- Scenario 1: If the team has only used up **10 minutes** of downtime (error budget), we can confidently push new features.
- Scenario 2: If we’ve already hit **40 minutes** of downtime, everyone can agree that adding new features isn’t wise until service stability is restored.


## **How SLO Brings Peace to Your Organization ✌🏽**


Reliability is the number one feature of any software. And SLO helps achieve that reliability. Instead of arguing about whether to build a new feature or improve reliability, you can look at SLO data. If you’re within your error budget, you have space to innovate. If you’re burning through it, you know it’s time to focus on reliability.


Imagine a product manager asking, “Can we launch this feature next week?”
With SLO data, we can only have two answers:


1. *Yes, error budget is healthy, let’s go!* 🚀
2. *No, we’re breaching SLOs… We need to stabilize before shipping more features.* ⛑️


Many organizations are already following such practices: fixing issues before putting a new feature in the mix. But when reliability discussions are backed by data, developers don’t feel like they’re being pushed back and product managers don’t feel like they’re being blocked. The data speaks for itself. No arguments. Just data driven alignment.


## SLO practices at trivago 🧩


Since the inception of SLO, Site Reliability Engineers around the Tech Industry have mostly advocated this practice. But in reality it’s difficult to get buy-in from other roles. However, adopting SLO practices taught us it’s not just for SREs. Developers and Product Managers also benefit from it. And we should spread SLO knowledge across the organization for greater good.


We are currently utilizing Service Level Objectives (SLOs) in our Search Backend, Routers, Pricing Services, and few services in our Marketing Solutions. The common SLOs we have for our services are “ *availability”* and “ *latency”* indicators. However, we are also capturing critical business metrics in SLO format, such as “ *search result completeness”* and “ *clickout ratio”* .


This approach ensures reliable performance indicators, giving our on-call engineers better understanding over key priorities. For example, if the rate of error is minimal and our error budget is not in a risk, we can probably review it the next day. If the error rate is higher, it will trigger high priority alerts, and we have to jump in quickly to prevent larger outage. Overall, this practice improves our user experience and business outcomes.


When a service’s history shows frequent unavailability, we pause feature development immediately and work on improving the stability until health is restored. Conversely, if the service remains stable for a long time, we have more error budget and can take greater risks with feature development and ambitious experiments.


Huge credit to our SRE, Sunny Redhu, who created a Helm chart that lets us define our own *[good event](https://sre.google/workbook/implementing-slos/#:~:text=good%20events%20divided%20by%20the%20total%20number%20of%20events)* and *[total events](https://sre.google/workbook/implementing-slos/#:~:text=good%20events%20divided%20by%20the%20total%20number%20of%20events)* for a metric. When applied, this configuration will generate alerting rules in the backend automatically with SLI (Service Level Indicator), SLO (Service Level Objective), Burn Rate, and Error Budgets over a sliding window. Additionally, we have a common dashboard that displays historical and real-time performance of each service over a time window.


## Getting Started: Your Adventure Awaits! 🏗️


The[SRE Handbook](https://sre.google/sre-book/service-level-objectives/) by Google is the perfect starting point. This book helps you understand the basic concepts of Service Level Indicators (SLI), Service Level Objectives (SLO), Service Level Agreements (SLA), and Error Budgets.


Even with all the knowledge, it can be difficult to choose a number for the reliability value. You can ask us, how come we end up deciding a number that is 99.99% and not something else? Well, we did not come with such a number magically.


1. We tried out the SLO practices in some areas
2. We observed the performance over a period of time
3. We learned many unknowns about our systems
4. We then set up a value that represents the true nature of our systems
5. We kept repeating from step 1 and tried to achieve higher reliability where we could


To start with, let’s imagine we set the reliability target of a service indicator to 99% ( *a.k.a: two nines* ). That means the error budget for that indicator is ~432 minutes in a 30 day window. If we consistently stay within budget for a few weeks or months, we know that our service is doing well. Then probably it’s time to increase the bar up to 99.9% ( *a.k.a: three nines* ), meaning we can afford ~43 minutes downtime over a 30 day window. And iterate going forward.


One can also ask, “if reliability is the number one feature, why don’t we set all of our SLOs to 100%”? It’s because achieving 100% reliability is much harder and often requires much higher effort that doesn’t pay off. There are many internal and external factors that contribute to the overall service reliability which are simply unavoidable. So, together with developers and product managers we agree on a target that is “good enough” for our end users.


## **Final Thoughts: What Are We Fighting For? 🫱🏼‍🫲🏽**


SLOs are not just a tool for Site Reliability Engineers. They’re a collaboration framework. They give developers and product managers a shared compass, pointing towards what really matters: **great user experience** .


Next time you face a “speed vs. quality” debate, remember you don’t need to fight product managers. Use SLO practices to fight for the users instead.


---


Shoutout to our models who did not just help make this blog complete, but they are also SLO ambassadors at trivago.


- **Sahil Maniar** , our amazing Product Manager who keeps us all aligned and inspired
- **Bart Ramaekers** , a fantastic Backend Software Engineer, creating magic with his keystrokes
- **Ekaterina Falikova** , our Employer Branding Manager and the wizard behind the camera
