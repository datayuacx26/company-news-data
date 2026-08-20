---
schema_version: "1.0.0"
document_id: "9c97e14f7d7d9f28058af49b1a0187bfee2c2a4b6f55b4beb438d6ec0f12deaf"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/fireside-chat-with-arjun-iyer-signadot/"
published_at: "2022-03-30T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:f4c0ec1cdfa439409b4b1613c56aab9c236a40797736a374e193a8e67183b4d3"
---

# How to Reduce Engineering Burnout - A fireside chat with Arjun Iyer at Signadot

*\[This interview transcript was originally published at:*[Allison Pickens’ Newsletter](https://allisonpickens.substack.com/p/how-to-reduce-engineering-burn-out) *\]*


As I’ve written about extensively, the competition for talent is the fiercest it’s ever been. Hiring engineers can be particularly challenging. Retaining them becomes a corporate imperative. I tackle this topic in my discussion with[Arjun Iyer](https://www.linkedin.com/in/arjun-iyer-916332/) , the founder of[Signadot](https://www.signadot.com/) , which recently announced its[seed round](https://techcrunch.com/2022/02/23/signadot-promises-developers-faster-feedback-loops/) led by Redpoint (with participation from yours truly!). We cover some primary reasons for engineer attrition, and how a more thoughtful way of enabling engineers to collaborate can reduce frustration and burn-out.


### What are the top reasons why engineers quit their jobs?


[Edmond Lau](http://www.effectiveengineer.com/) , author of “Effective Engineer”, said it well: “Bureaucratic barriers to deploying code and launching features are some of the most common and frustrating reasons that engineers cite during interviews for why they’re leaving their current companies.”


By contrast, there’s nothing more empowering to an engineer than having the autonomy to move forward without having to get approval or wait on other teams. Ideally, engineers should be able to deliver changes to production on a daily basis.


I remember, back when I was at AppDynamics, moving to a frequent release cadence became a top engineering goal. We would spend a significant portion of our time testing closer to the release, and it was still not enough. Code quality suffered, and the debugging process was very challenging. This was a classic situation in which engineers didn’t feel empowered to fix problems independently, because the changes from multiple engineers were batched together.


### I know you and other engineering leaders have thought a lot about how to avoid the “batching” problem. What steps do leaders typically take to give more freedom and independence to individual engineers?


If code is developed as a single unit (often called a monolith), a change to any component requires testing of the entire application. As this becomes impractical to do with a large engineering team, many changes are batched up into a “release” and tested together. In such a scenario, engineers lose the freedom to release smaller, incremental changes to production independently.


The “microservices” architectural pattern was introduced to address these problems with monolithic applications. The idea is to decompose the application into “services” that are developed, tested, and released to production independently. Developers limit testing to the services that have changed. Such a decentralized architecture gives developers flexibility and autonomy to ship changes to production independently.


### I know microservices are not a silver bullet. How do you perceive the challenges with those?


Splitting up an application into services means managing many independently moving pieces of code, which can make validating and testing code challenging. Only relying on unit tests or testing in a mocked environment is not sufficient, as engineers don’t get a high-quality testing signal with these methods when services have many dependencies. Without an optimal testing platform, engineers end up spending a lot of time debugging issues in an integration environment where changes from various services get co-mingled.


Wanting to scratch this “itch”, Anirudh (my co-founder) and I founded Signadot in 2020 to enable “shift left” testing with the mission to empower developers to innovate in building cloud native applications.


### How does Signadot address this issue?


Signadot is a Kubernetes based platform that gives engineers control over testing changes much earlier in the development lifecycle. Engineers create ephemeral test environments called “Sandboxes” to test changes in a “live” environment and discover most issues before code lands in a shared environment. Sandboxes spin up in a matter of seconds, smartly share resources, and scale well as engineering teams grow.


Signadot helps fulfill the promise of microservices by enabling independent, high-quality testing of every code change. Now engineers can develop, test, and release changes independently with confidence.


### I suspect you will be tracking the impact on engineering happiness and retention! Thanks Arjun for having this conversation with me.


‍
