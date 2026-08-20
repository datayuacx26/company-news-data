---
schema_version: "1.0.0"
document_id: "9e79f72f0f212db02cff72596b12816ea3c707da7ffffaf85da841c1a84c3d4c"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-bitso-is-scaling-branch-based-development-with-signadot-and-neon/"
published_at: "2026-01-07T19:52:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:24:01.443100+00:00"
content_hash: "sha256:0a7f623c4c0fce081d53636d2b937ad3c306a6197abff03feed803a20ede134b"
---

# How Bitso Is Scaling Branch-Based Development with Signadot and Neon

[Bitso](https://bitso.com/) , a leader in digital financial services in Latin America, has selected Signadot and[Neon](https://neon.com/) to modernize their testing infrastructure. By combining Signadot sandboxes with Neon for database branches, Bitso is building what they call RISE: a Robust Isolated Staging Environment architecture.


This new infrastructural model enables their 250+ engineers to test safely in a production-like environment with full per-developer isolation and zero conflict, accelerating feature delivery and improving developer experience.


## **The Challenge: The Bottleneck of Shared Environments**


Scaling to over 250 engineers, Bitso found their previous development and staging workflows buckling under the weight of their complex architecture.


With hundreds of Java microservices and heavy reliance on asynchronous communications via Kafka, it became impossible for developers to run the full stack locally.


Historically, hundreds of engineers shared a single development environment and a single staging environment. This created severe resource contention where environments were constantly unstable. If one developer deployed a faulty change to staging, it blocked the pipeline and broke the environment for everyone else.


> *“Everyone pushing changes onto a shared development branch… it was constantly broken. And if you break something, you break it for everyone using that environment .”* — Joe Horsnell, Principal Platform Engineer at Bitso


The deployment process also lacked strict artifact promotion. Developers would deploy changes to staging, where they were mingled with other changes, and then deploy a different build to production. This meant that the code running in production was never identical to what had been validated in staging.


## **Enter Signadot: Unlocking Isolation and Safe Promotion**


To solve this, Bitso adopted Signadot to implement an isolated testing workflow. Instead of engineers colliding with each other when deploying changes to a shared environment, Signadot allows their developers to test changes in isolation inside[lightweight sandboxes](https://www.signadot.com/solutions/preview-environments/) , which are provisioned automatically based on the changes in their pull requests.


Signadot ensures that only requests to the modified services are routed to the sandbox, with all other service and infrastructure dependencies in the environment being provided by the baseline.


Crucially, this unlocked a safer deployment pattern: artifact promotion.


> *“When you test, you’re testing your changes in isolation, in your sandbox. When we deploy to production, it is precisely the same artifact that was tested in the sandbox - the container image - that we promote.”* — Joe Horsnell, Principal Platform Engineer at Bitso


## **The Missing Piece: State**


Signadot solved the problem of isolation for stateless services and HTTP/gRPC traffic, but the database remained a shared dependency. For roughly half of their services, sharing a database was acceptable. However, for the long tail of services requiring schema changes or write-heavy isolation, sharing a single staging database prevented true isolation.


This is where Neon comes in.


Bitso chose Neon to bring the same level of agility to their data layer that Signadot brought to their compute layer. Neon’s serverless Postgres offers instant branching, allowing Bitso to spin up copies of their staging data for every Signadot Sandbox.


By integrating Neon, Bitso can now provide:


1. **Data Isolation:** Each Sandbox gets its own database branch, if required. A developer can run destructive migrations or test edge cases without affecting the baseline staging data or other developers.
2. **Production-Like Data:** Instead of testing against stale or empty seed data, developers test against a copy of the actual staging dataset, catching data-specific bugs early in the lifecycle.


## **Conclusion: A Fully Isolated, Production-Like Standard**


The combination of Signadot and Neon enables Bitso to implement their RISE architecture vision. This infrastructure evolution does more than just fix internal bottlenecks. In the highly competitive and fast-moving cryptocurrency market, the engineering velocity RISE will unlock is a critical advantage. By reducing the friction of testing and deployment, Bitso can iterate rapidly on new products and services while maintaining the rigorous stability their customers expect.


Using Signadot for request isolation and Neon for data isolation, Bitso is proving that you do not need to choose between velocity and stability. You can have both, even at the scale of a financial services platform processing millions of transactions. If you’re interested in implementing a similar architecture, you can[create a free Signadot account today](https://www.signadot.com/signup/) , check out our[tutorial for implementing NeonDB with Signadot](https://www.signadot.com/docs/integrations/databases/neon) , or[read the Neon write-up here](http://neon.com/blog/bitso-branching-workflow) .
