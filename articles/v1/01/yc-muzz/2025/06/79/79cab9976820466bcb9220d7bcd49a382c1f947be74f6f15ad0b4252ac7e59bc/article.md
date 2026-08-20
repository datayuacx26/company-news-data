---
schema_version: "1.0.0"
document_id: "79cab9976820466bcb9220d7bcd49a382c1f947be74f6f15ad0b4252ac7e59bc"
company_key: "yc-muzz"
company: "Muzz"
source_id: "yc-muzz-news-import-4a23516b0f87"
canonical_url: "https://engineering.muzz.com/swipe-right-on-go-why-we-ghosted-php"
published_at: "2025-06-30T15:07:39+00:00"
first_seen_at: "2026-07-25T16:24:04.083021+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:39693349afd19e870f9b44c269215fec77b47e4c910289e0c0ff7bfb2e5ca0aa"
---

# Swipe Right on Go: Why We Ghosted PHP

### From legacy code to lightning-fast matchmaking—why Go won our hearts (and our traffic).


#### So – why migrate?


As our platform scaled, our legacy PHP system began to struggle. Response times increased. Latency issues cropped up. In some cases, requests failed outright. What once worked well was starting to hold us back.


Beyond the language migration, a pivotal aspect of our evolution involved a significant architectural shift. Our legacy PHP system, while initially designed with an ingenious approach to mimic microservices by splitting endpoints to even out load, was still fundamentally a monolith. This approach, though clever in its time, presented inherent limitations in terms of true independent scalability and deployment.


As we embraced Go, we concurrently transitioned to a genuine microservice architecture. This move allowed us to break down the monolithic structure into truly independent, smaller services, each with its own specific responsibility. This architectural change complements the performance gains from Go by enabling finer-grained control over scaling, more isolated deployments, and a more robust system overall. This is an ongoing journey, and a crucial component of building a more resilient and future-proof platform.


To support continued growth—and provide a smoother, more reliable experience—we had to make a change.


#### The Limits of PHP


Our traffic and user activity were growing steadily, and PHP’s architecture wasn’t keeping up.


PHP’s architecture presented significant challenges as our platform grew. As the volume of incoming requests increased, especially during peak traffic, the system experienced noticeable slowdowns and performance degradation. This eventually led to rising performance issues and made maintaining the old stack increasingly inefficient, as resources were constantly being diverted to address these bottlenecks rather than on new development.


As our user base and app complexity grew, scaling PHP became a real pain point—both technically and financially. With handling around 2,000 requests per second, every step up in scale meant major investments in hardware and network capacity. PHP gave us flexibility early on, but it didn’t scale horizontally without a lot of extra work and overhead. That led to infrastructure costs rising way faster than user engagement or revenue growth.


To put it in perspective, we’re seeing ten’s of matches created every second, and there are hundred’s of millions of total matches sitting in the database. Keeping up with that kind of load meant constant tuning, upgrading, and firefighting. Over time, more and more of our engineering resources got tied up in just keeping the system running, leaving less room to build new features or explore new markets.


It wasn’t sustainable.


### **Why We Chose Go**


When it came time to choose a new backend language, we evaluated several contenders but Go consistently rose to the top for one reason: it offered the best balance of performance, simplicity, and operational efficiency at scale.


Here’s a closer look at why Go was the right fit for us:


#### **1. Concurrency That Just Works**


Our application handles high volumes of concurrent requests—especially during peak usage windows. In PHP, this kind of load was costly and difficult to scale efficiently. We were running into performance ceilings, and spinning up additional servers was quickly becoming an unsustainable fix.


Go was built from the ground up for concurrency. Its lightweight goroutines and built-in channel mechanisms gave us a simple, elegant model for parallelism that didn’t require external libraries or complex threading models.


Instead of each request spinning up a heavyweight thread (as with many traditional stacks), goroutines let us handle tens of thousands of concurrent operations in a resource-efficient way. This drastically improved throughput and lowered latency, even on commodity hardware.


#### **2. Fast, Predictable Performance**


Go is statically compiled to native machine code, and it shows. Our services now start quickly, use less memory, and execute faster. Combined with garbage collection optimized for low-latency workloads, Go gave us the kind of performance characteristics we simply couldn’t achieve with PHP—especially under heavy load.


The result: fewer slow endpoints, faster p99s, and more consistent performance across the board.


#### **3. Simplicity Without Sacrifice**


Go’s philosophy of simplicity and minimalism aligned closely with our own engineering values. It enforces clear code structure, avoids over-engineered abstractions, and offers just enough language features to get the job done—without inviting unnecessary complexity.


This made it easier to:


- Write predictable, readable code.
- Onboard new engineers quickly.
- Perform reliable code reviews and refactors.


We didn’t need a sprawling framework or deep runtime magic to be productive. Go let us build fast, stay focused, and ship confidently.


#### **4. Strong Ecosystem and Tooling**


Go’s tooling out of the box is exceptional. From fast, reliable builds (go build) to dependency management (go mod) and race condition detection (go test -race), the developer experience is streamlined and efficient.


We also found a rich ecosystem of well-maintained libraries for everything from HTTP servers to observability, logging, and database access. That meant we could move quickly without constantly reinventing the wheel—or compromising on stability.


#### **5. Easier Hiring and Talent Retention**


Surprisingly, Go also gave us a hiring edge.


While Go has a smaller pool of engineers than some older languages, the ones we found were experienced, pragmatic, and drawn to solving real engineering challenges. The language’s reputation for simplicity and performance attracts the kind of developers we want on our team.


Go also tends to reduce onboarding time. With a consistent style and limited language surface area, new engineers can contribute to production code in days, not weeks.


#### **6. Operational Efficiency at Scale**


Go’s memory footprint is small, and its binaries are statically linked and easy to deploy. That made our CI/CD and infrastructure much simpler. We went from bloated PHP deployments and web server configs to clean, lightweight services that are easy to containerize and run in Kubernetes.


As a result, our DevOps team spends less time managing infrastructure, and more time supporting meaningful platform improvements.


Together, these advantages made Go a clear winner—not just as a new language, but as a foundation for the next generation of our architecture.


We weren’t just looking to replace PHP. We were investing in a system that could scale with our growth, support a modern engineering culture, and deliver a better experience for our users.


#### The Migration: Challenges and Tradeoffs


One of the toughest parts of the migration was keeping both systems running in parallel. We needed to avoid service disruptions, which meant maintaining the PHP system while building out Go in production.


This dual setup added complexity—especially around keeping both platforms in sync and shipping new features at the same time.


Like any large-scale migration, it took longer than expected. But we treated it as a long-term investment, and prioritised careful testing and cross-team coordination to keep things moving.


#### What We Gained


Our first migrated service—“Discover”, the swipe interface users interact with—showed just how much we stood to gain:


- **P50 response time:** ↓ 57%
- **P90:** ↓ 39%
- **P95:** ↓ 13%
- **P99:** ↓ 22%


But the benefits went beyond raw performance. Our development process got faster. The codebase became easier to work with. Maintenance demands dropped. And as traffic continues to grow, we’re better positioned to scale without bottlenecks.


#### What We’d Do Differently


One lesson stood out: better upfront coordination across teams is essential.


In a few cases, features were built on the PHP system even though it was on its way out. That led to duplicated work and unnecessary maintenance. With clearer communication and more deliberate planning, we could have avoided that friction.


Next time, we’d sync earlier and allocate resources with the new architecture in mind from the start.


#### Where We Found Support


We leaned heavily on[Go’s official documentation](https://golang.org/doc/?ref=engineering.muzz.com) and the wider developer community on[Stack Overflow](https://stackoverflow.com/?ref=engineering.muzz.com) . When issues came up, there was almost always someone who had tackled it before.


#### Final Thoughts


This migration was a significant technical and organisational effort—but it was the right move.


We’ve reduced latency, improved performance, and built a system that’s far more scalable and maintainable. While there were tradeoffs along the way, the end result is a faster, more resilient platform—and a better experience for our users.
