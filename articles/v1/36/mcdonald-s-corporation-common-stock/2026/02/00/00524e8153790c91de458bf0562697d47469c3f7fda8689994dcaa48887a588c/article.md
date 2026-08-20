---
schema_version: "1.0.0"
document_id: "00524e8153790c91de458bf0562697d47469c3f7fda8689994dcaa48887a588c"
company_key: "mcdonald-s-corporation-common-stock"
company: "McDonald's Corporation"
source_id: "mcdonald-s-corporation-common-stock-rss-e3f7e88d5cc9"
canonical_url: "https://medium.com/mcdonalds-technical-blog/delivering-virtual-burgers-one-instance-at-a-time-with-multi-tenancy-8ee6eee7c7ec"
published_at: "2026-02-24T14:06:40+00:00"
first_seen_at: "2026-07-22T17:27:13.648832+00:00"
fetched_at: "2026-08-20T02:26:39.473331+00:00"
content_hash: "sha256:6bfc2f62e203196655f7fbee9b819b1bfe15464b812fb7bb788d4b5f98bd7c2e"
---

# Delivering Virtual Burgers, One Instance at a Time with Multi-Tenancy

One microservice, many markets — McDonald’s tech adapts to local needs at global scale.


*by: Fabian Vergara, Software Engineer III — McDelivery Lead Software Engineer*


Quick Bytes:


- Scaling microservices across 100+ markets means balancing global consistency with local flexibility — no easy feat for a brand as big as McDonald’s
- Multi-tenancy lets one service power many markets, each with its own logic and data — unlocking agility without sacrificing control
- Faster launches, lower costs, and a consistent global experience — so every market can deliver its own flavor, instantly


**Scaling flavor with architecture** At McDonald’s, delivering consistent digital experiences to millions of people across diverse global markets requires a robust and scalable architecture. The system must be flexible but also rigidly contained. Even microservice-based architectures face challenges at our scale.


Do you have a microservice serving a single market? How about a single geographic region or continent? The strategies vary, and there are solutions for every flavor.


One of the key strategies we employ is **multi-tenancy —** a design pattern that allows us to build **shared microservices** that serve multiple markets (tenants), each with its own configuration and, if required, business flows.


**What is multi-tenancy, really?** Multi-tenancy enables a single instance of a microservice to serve multiple tenants. In our context, imagine a microservice that serves many **markets** (e.g., US, Canada, France). Each market may be a tenant with:


- Unique configurations
- Market-specific business logic
- Independent data segregation


This approach allows us to **reuse core functionality** while maintaining **flexibility and isolation** . Expanding our operations and entering new markets can be efficiently managed by increasing the number of pods in our deployment scripts and introducing a configurable tenant. With appropriate auto-scaling policies in place, we are well-equipped to accommodate the demands of additional markets.


**Why it works for a global giant like McDonald’s** Operating in over 100 countries, McDonald’s faces a wide range of operational nuances. Multi-tenancy helps us:


- **Reduce duplication** of services across markets
- **Accelerate development** and **reduce associated costs** by reusing shared components
- **Ensure consistency** in global features — so your McDonald’s order reaches your doorstep, wherever you are
- **Enable customization** for local needs, as each market poses different nuances to the business
- **Support isolated testing** while using **shared environments**


**Behind the scenes: How we architect for multi-tenancy** An architecture that relies on multi-tenancy is built on a foundation of:


- **Tenant-aware microservices** : Services identify the tenant via headers, tokens, or routing logic
- **Configuration-driven behavior** : Tenant-specific settings and configuration are injected at runtime
- **Isolated data layers** : Each tenant has its own data store or schema to ensure privacy and compliance
- **Feature toggles** : Enable or disable features per tenant without redeploying code


**The trade-offs: It’s not always fries and sunshine** While multi-tenancy offers many advantages, it also comes with challenges. For example, now you have to manage tenant complexity, where your configuration and tenant registry must be centralized, introducing a potential single point of failure that must be accounted for.


Another common concern is the interaction between tenants. If tenants need to harvest and store customer data, and some markets have stricter regulations, you may need to implement strict access controls to ensure only the appropriate tenants can have access and can manipulate that data.


**What we’ve gained** Despite the complexities and trade-offs, the advantages of multi-tenancy have far outweighed the challenges for McDonald’s. Here’s what we’ve gained by adopting this approach:


- **Faster time-to-market for new features:** Shared services empower multiple markets, allowing us to roll out new capabilities simultaneously across regions. This minimizes duplication, speeds up iterations, and helps us quickly adapt to changing customer needs.
- **Lower development, operational, and testing costs:** By reusing core components across tenants, we reduce the overhead of building and maintaining separate services. Shared environments streamline testing, eliminating the need for dedicated infrastructure for each market.
- **Improved scalability:** As we expand to new markets or scale existing ones, multi-tenancy ensures that we can onboard tenants into our system due to the extensibility of our system.
- **Consistent global brand experience:** Our platform delivers localized experiences while maintaining brand consistency. Core components are reused across tenants, ensuring that every market benefits from the same high standards and reliability.


**One platform, many markets** Multi-tenancy has been a game-changer in how we build and scale microservices at McDonald’s. It allows us to strike the perfect balance between **global consistency** and **local flexibility** , empowering markets to innovate while staying aligned with the brand.


---


[Delivering Virtual Burgers, One Instance at a Time with Multi-Tenancy](https://medium.com/mcdonalds-technical-blog/delivering-virtual-burgers-one-instance-at-a-time-with-multi-tenancy-8ee6eee7c7ec) was originally published in[McDonald’s Technical Blog](https://medium.com/mcdonalds-technical-blog) on Medium, where people are continuing the conversation by highlighting and responding to this story.
