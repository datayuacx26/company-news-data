---
schema_version: "1.0.0"
document_id: "69f124a1b06debbc80561838bd12878b2ad9bb68ebd88c676c9c541a4920abf2"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/building-better-authorization-infrastructure-with-arm"
published_at: "2025-05-14T13:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:6c6e6ba6ee6f8ec7e1ffa5fd97bcdfe1b40944f8172fd87c0c60b2a1f4a03d08"
---

# Building Better Authorization Infrastructure with ARM: Benefits from Laptop to Cloud

How ARM helps AuthZed build and operate authorization infrastructure, from day-to-day productivity gains to cost-effective, performant cloud compute.


## Meeting Modern Development Challenges


Today's cloud-native development environment requires running a growing list of simultaneous services: container orchestration, monitoring, databases, observability tools, and more. For engineering teams, this creates a critical challenge: how to balance performance, cost, and efficiency across both development environments and production deployments.


At AuthZed, we provide flexible, scalable **authorization infrastructure** —the permissions systems that secure access for your applications’ data and functionality—enabling engineering teams to focus on building what matters—their core products. For our customers using AuthZed's dedicated cloud, the balance of performance, cost, and efficiency is also crucial—they expect a reliable, performant, and cost-effective solution.


ARM architecture has become our strategic advantage in meeting these challenges across our entire workflow.


## The ARM Advantage for Development


The availability of ARM-based laptops with customizable configurations and ample RAM has transformed our development environment. Our journey began with ARM processors in early 2022 and expanded to more powerful variants as they became available. The developer community quickly adopted these machines, and tooling and library support rapidly matured, enabling us to fully adopt ARM as our primary architecture in development.


### Developer Productivity in Action


At AuthZed, we work with distributed systems and databases daily, and running the full stack locally can be resource-intensive, often requiring significant CPU and memory. ARM's efficient performance helps utilize machine capacity, while its energy efficiency keeps our laptops cool enough to truly stay on laps—even when running our resource-intensive local environment.


After upgrading to higher-performance ARM-based laptops, notable improvements compared to our previous development environment included:


- 27% decrease in average container image build times
- 40% decrease in parallel build times for our application stack
- Ability to run our entire application stack locally, including supporting monitoring and observability services


The qualitative benefits have been even more significant—true mobility with our laptops due to minimal battery drain and absence of overheating, smoother performance during resource-intensive tasks, and most importantly, tighter feedback loops during debugging and testing.


## CI/CD with ARM


AuthZed has been building and publishing multi-architecture Docker images for our tools and authorization database for over three years (since March 2022), so we recognized the value of multi-architecture support in CI/CD early on.


There's now robust support for third-party ARM-based action runners for GitHub Actions, our CI/CD platform. Combined with toolchain maturity across runner images for popular architectures, migration to ARM for CI/CD has never been easier.


Build and test workflows are unique to each project and evolve as the project develops. Consequently, the benefits and tradeoffs for a CI/CD platform change over time. We've benefited from being able to easily migrate between architectures and runner providers to best meet our engineering needs at different stages.


## Powering AuthZed Dedicated with ARM


Major providers like Google Cloud, AWS, and Azure have all released custom-designed ARM-based CPUs for their cloud compute platforms. The expanding ARM ecosystem bolsters our multi-cloud strategy for AuthZed Dedicated and allows our production workloads to benefit from ARM's design, which prioritizes high core count and power efficiency under load.


AuthZed Dedicated is our dedicated authorization infrastructure deployed adjacent to customer applications in their preferred cloud platform. This allows for the lowest latency between user applications and our permissions systems, and for the most comprehensive region support. With the availability of ARM-based compute options across the major providers, we are able to take advantage of the economic and performance advantages of ARM-based infrastructure in production:


- 20% cheaper compute costs
- 20-25% more efficient CPU usage for our workloads
- 20% higher throughput (based on a load tests at 1 million QPS on AWS Graviton EC2 instances)


## End-to-End ARM Advantage


From developer laptops to cloud infrastructure, ARM delivers consistent advantages throughout our engineering pipeline. For AuthZed, it's now our preferred platform for building and running authorization infrastructure that helps customers secure applications with confidence and scale efficiently.


The combination of developer productivity, cost efficiency, and performance gains enables our growing startup to innovate and compete effectively. As cloud providers continue expanding ARM-based offerings and development tools mature further, we expect these advantages to compound, creating even more opportunities to deliver value through our authorization infrastructure.


By embracing ARM across development and production environments, we've created a seamless experience that benefits both our team and our customers—accelerating development while delivering more performant and cost-effective services.


Curious about the inspiration behind AuthZed’s modern approach to authorization? Explore the[Google Zanzibar research paper](https://authzed.com/z/google-zanzibar-annotated-paper) with our annotations and foreword by Kelsey Hightower to learn how it all began.
[https://authzed.com/z/google-zanzibar-annotated-paper](https://authzed.com/z/google-zanzibar-annotated-paper)


On this page


- Meeting Modern Development Challenges
- The ARM Advantage for Development
- Developer Productivity in Action
- CI/CD with ARM
- Powering AuthZed Dedicated with ARM
- End-to-End ARM Advantage


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
