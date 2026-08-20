---
schema_version: "1.0.0"
document_id: "a59d520d80ae3d00457ea933cca8401a37737824dd77bc1b1042ba814969e2d8"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/authzed-cloud-is-now-available"
published_at: "2025-08-20T16:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:5da5cf84c34209ccc96dd2ba690b88a2f9d13fbcff79522b1a8dce70198a70af"
---

# AuthZed Cloud is Now Available!

Today marks a special milestone for AuthZed: we're celebrating our 5th anniversary! There are honestly too many thoughts and reflections swirling through my mind to fit into a single blog post. The reality is that most startups don't make it to 5 years, and I'm extremely proud of what we've built together as a team and community.


If you want to hear me reflect on the journey of the past 5 years, I'm giving a talk today about exactly that, and we'll post a link to the recording here when it's ready. But today isn't just about looking back, it's also about looking forward, and I’ve personally been looking forward to launching our next iteration of authorization infrastructure:[AuthZed Cloud](https://authzed.com/cloud) .


In this blog post, I'll cover what we've built and why, but if you don't need that context and just want to dive in, feel free to bail on this post and[sign up right now](https://authzed.com/cloud/signup) !


## AuthZed Dedicated is the Perfect Product


To understand why we built AuthZed Cloud, I need to first talk about[AuthZed Dedicated](https://authzed.com/products/authzed-dedicated) , because in many ways, Dedicated represents our vision of the perfect authorization infrastructure product.


AuthZed Dedicated is nearly infinitely scalable: capable of handling millions of queries per second when you need it. It's co-located with your workloads, which means there's no internet or cross-cloud latency penalty for your authorization decisions, which are often in the critical path for user interactions. It can run nearly anywhere on earth, with support for all three major cloud providers, giving you the flexibility to deploy where your business needs demand.


Perhaps most importantly, Dedicated provides total isolation for each customer across datastore, network, and compute layers. It marries the best permissions database in the world ([SpiceDB](https://authzed.com/spicedb) ) with the best infrastructure design ([Kubernetes](https://kubernetes.io/) + operators) to create what we believe is the best authorization infrastructure in the world.


So how did we improve on this formula? We made it more accessible!


AuthZed Dedicated's biggest challenge isn't technical: it's the enterprise procurement cycle that comes with it. The question we kept asking ourselves was: how can we bring these powerful concepts to more companies, especially those who need enterprise-grade authorization but can't navigate lengthy procurement processes?


## Introducing AuthZed Cloud


AuthZed Cloud takes the most powerful concepts from AuthZed Dedicated and makes them available in a self-service product that you can start using today.


We've also made several key improvements over what’s available in Dedicated today:


**Self-service registration and deployment** : No more waiting weeks for procurement approvals or implementation calls. Sign up, configure your permissions system, and start building. Scale when you need to!


**Roles** : We've added granular access controls that let you limit who can access and change things within your AuthZed organizations. This was a frequent request from teams who needed to federate access to our platform in different ways. You’ll be happy to know that this feature is, of course, also powered by SpiceDB.


**Usage-based billing** : Instead of committing to fixed infrastructure costs upfront, you can spin up resources on-demand and pay for what you actually use.


The best part? **These improvements will also be landing in Dedicated soon, so all our customers benefit!**


Delivering on this vision does require some compromises. AuthZed Cloud uses a shared control plane and operates in pre-selected regions (though please let us know if you need a region we don't support today!). But honestly, that's about it for compromises.


## For Whom?


AuthZed Cloud is designed for companies of all sizes. Despite the shared infrastructure approach, we've maintained high isolation standards. Your SpiceDB runs as separate Kubernetes deployments, and datastores are dedicated per permissions system. You still get the same scalable technology from Dedicated that allows you to scale up to millions of queries per second when needed, and the same enterprise-grade reliability.


What makes Cloud special is how attainable it is. The base price is a fraction of our base Dedicated deployment price, opening up AuthZed's capabilities to a much broader range of companies.


That said, some organizations should still consider Dedicated. You might consider dedicated if you need higher isolation requirements like an isolated control plane or private networking, or if you need higher flexibility around custom legal terms or deployment in cloud provider regions that AuthZed Cloud doesn't yet support.


## Early Access Reception


The response during our early access period has been incredible. There was clearly pent-up demand for a product like this! We've had several long-time AuthZed customers already making the move to Cloud.


Lita Cho, CTO at moment.dev, had this to say:


> “We love Authzed—it makes evolving our permissions model effortless, with a powerful schema language, makes rapid prototyping possible along with rock-solid production performance, all without heavy maintenance. **Authzed Cloud delivers the power and reliability of Dedicated at a startup-friendly price, without the hassle of running SpiceDB. That lets me focus on building our modern docs platform, confident our authorization is secure, fast, and future-proof.”**


## Get Started Today


The best part about AuthZed Cloud is that you can sign up immediately and get started building. We've also set up a program where you can[apply for credits](https://authzed.com/cloud/starter-program) to help with your initial implementation and testing.


As we celebrate five years of AuthZed, I'm more excited than ever about the problems we're solving and the direction we're heading. Authorization remains one of the most critical and complex challenges in modern software development, and we're committed to making it accessible to every team that needs it.


Here's to the next five years of building the future of authorization together.


On this page


- AuthZed Dedicated is the Perfect Product
- Introducing AuthZed Cloud
- For Whom?
- Early Access Reception
- Get Started Today


## Related


[Product Composable Schemas is GA Composable schemas are now generally available in SpiceDB 1.51.1. Split monolithic schemas into well-defined, reusable components so teams can move faster, make changes, and scale without everything becoming entangled. Apr 15, 2026 · 3 min](https://authzed.com/blog/composable-schemas-ga-and-zed-v1)[Product Composable Schemas is GA Composable schemas are now generally available in SpiceDB 1.51.1. Split monolithic schemas into well-defined, reusable components so teams can move faster, make changes, and scale without everything becoming entangled. Maria Inés Parnisari · Apr 15, 2026 · 3 min](https://authzed.com/blog/composable-schemas-ga-and-zed-v1)


[Product Zed v0.30.2 Release Zed CLI provides seamless interaction with SpiceDB clusters, allowing you to manage schemas, relationships, and permissions checks. Our v0.30.2 release adds composable schema support, automatic retries, backup functionality, and upcoming Windows package integration via Chocolatey. May 1, 2025 · 2 min](https://authzed.com/blog/zed-v0-30-2-release)[Product Zed v0.30.2 Release Zed CLI provides seamless interaction with SpiceDB clusters, allowing you to manage schemas, relationships, and permissions checks. Our v0.30.2 release adds composable schema support, automatic retries, backup functionality, and upcoming Windows package integration via Chocolatey. Maria Inés Parnisari · May 1, 2025 · 2 min](https://authzed.com/blog/zed-v0-30-2-release)


[Engineering SpiceDB v1.39 Released The SpiceDB v1.39 release delivers enhanced monitoring through native histograms, smarter health checks, and transaction metadata improvement. Dec 20, 2024 · 3 min](https://authzed.com/blog/spicedb-v1-39-released)[Engineering SpiceDB v1.39 Released The SpiceDB v1.39 release delivers enhanced monitoring through native histograms, smarter health checks, and transaction metadata improvement. Sam Kim · Dec 20, 2024 · 3 min](https://authzed.com/blog/spicedb-v1-39-released)
