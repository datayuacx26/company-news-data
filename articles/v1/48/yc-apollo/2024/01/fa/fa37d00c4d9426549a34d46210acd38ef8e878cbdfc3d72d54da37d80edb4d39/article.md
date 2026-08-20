---
schema_version: "1.0.0"
document_id: "fa37d00c4d9426549a34d46210acd38ef8e878cbdfc3d72d54da37d80edb4d39"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-intuit-handled-their-busiest-time-of-year-with-apollo-router"
published_at: "2024-01-29T07:30:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:d7ff30c5e957fadbb0958670ef03cfbf01bbd28d71adf4046a840592226af3c2"
---

# How Intuit Handled their Busiest Time of Year with Apollo Router

Millions of small to midsize businesses turn to Intuit’s Quickbooks to manage their accounting, payroll, tax, and more. It is overwhelmingly[the most popular accounting software](https://www.businessdit.com/how-many-companies-use-quickbooks/#why-quickbooks-a-popular-choice-for-businesses) on the market today. Intuit serves 100 million individuals and businesses through its leading products TurboTax, Credit Karma, QuickBooks and Mailchimp.


In the demanding landscape of last year’s tax season, Intuit decided it was time to migrate from several fragmented graphs to a single supergraph powered by[Apollo GraphOS](https://www.apollographql.com/graphos) . This transition to the Apollo platform, though still ongoing, has already begun empowering self-service, ensuring compliance with industry standards, optimizing performance under high traffic, and streamlining supergraph management. Rama Palaniappan, principal engineer from Intuit’s API platform team, discussed this move at the[GraphQL Summit 2023 conference](https://www.youtube.com/watch?v=GB-THCI0Mw0&list=PLpi1lPB6opQzUOqG3QroLLN06FF-Q_uhX&index=37) . Rama outlines the transformational impact of[Apollo Router](https://www.apollographql.com/docs/router/) on the company’s performance metrics and its pivotal role in achieving operational excellence during critical periods.


*“Apollo Router \[…\] checked all the boxes for us, so we decided we’ll go with Apollo Router as a GraphQL orchestrator.”*


*Rama Palaniappan, Principal Engineer, API platform team at Intuit*


## How GraphQL Adoption Evolved at Intuit


Intuit’s API platform team aimed to provide a scalable, resilient infrastructure along with the right tools and processes to accelerate API development.


*Figure 1: Metrics for Intuit’s GraphQL Ecosystem*


Prior to this effort by the platform team, Intuit’s flagship product team switched to GraphQL to better accommodate their microservices architecture. As GraphQL’s popularity grew internally, various teams developed specialized GraphQL gateways to address their specific requirements. These gateways proved scalable and resilient, handling Intuit’s seasonal traffic spikes. However, they sometimes failed to meet industry standards and lacked documentation or accessible knowledge for onboarding or troubleshooting.


*“Since these were internal gateways \[…\] it lacked keeping up to the specs or industry standards, and the knowledge of these gateways was very limited to the team that created it. So if someone wanted to onboard to it \[gateway\], they had to \[…\] talk to the team there. It was not self-serve, and they couldn’t search anything on the internet about how to get things done.”*


Rama Palaniappan, Principal Engineer, API platform team at Intuit


Facing challenges with multiple internal gateways, a team of architects at Intuit embarked on a mission to merge subgraph schemas using a unified gateway. They chose[Apollo Federation 2](https://www.apollographql.com/docs/federation/) as the specification to merge these subgraph schemas.


## Streamlining Supergraph Management with Apollo GraphOS Contracts


By unifying the gateway, Intuit’s team aimed to ensure that clients interacting with their systems would connect to a single endpoint rather than multiple endpoints, aligning with GraphQL’s fundamental principles. This strategic move prompted the team to consider various factors when creating new[supergraphs](https://www.apollographql.com/docs/federation/#next-steps) .


By consolidating to a common GraphQL platform, Intuit could speed up application delivery by promoting better composability and reuse across teams. Rama’s team wanted to make sure teams could share common entities while still avoiding noisy neighbor problems across products during peak usage periods. Additionally, the team grappled with aligning the supergraph structure with functional groups or business units, a vital factor for better internal alignment.


*Figure 2: Implementation of Apollo’s contract feature*


The introduction of[Apollo GraphOS’s contract feature](https://www.apollographql.com/docs/graphos/delivery/contracts/) provided a solution (see figure 2). These contracts enabled them to deliver different subsets of their supergraph to different consumers. The subgraph schemas were tagged during design, creating a complete supergraph schema. Filtering this schema based on tags resulted in multiple contract supergraph schemas that could merge the subgraphs.


This approach enabled effortless modifications in the supergraph structure without requiring extensive changes in publishing or deployment. Moreover, contracts enabled the Intuit team to seamlessly make parts of a single supergraph accessible across multiple contract supergraphs, fostering versatility and adaptability within the system.


## Choosing Apollo Router for Scale and Performance


Intuit’s tax season presented a unique challenge, demanding peak scalability and minimal response time variance. While[Apollo Gateway](https://www.apollographql.com/docs/apollo-server/using-federation/apollo-gateway-setup/) was Intuit’s initial choice for its Apollo Federation 2 support and familiarity within internal teams, load testing revealed response time variability under high traffic. After excellent scalability results from benchmarking[Apollo Router](https://www.apollographql.com/docs/router/) , Rama and his team decided to use the Apollo Router as the GraphQL orchestration layer.


Figure 3: Intuit’s performance benchmarking


Throughout this journey, Intuit’s API team learned crucial lessons, emphasizing the importance of selecting a router that aligns with the surrounding technology stack, ensuring customizability, and prioritizing observability for early detection and recovery from issues.


The story of Intuit’s transition to Apollo GraphQL exemplifies the significance of choosing the right tools, adapting to evolving needs, and continuously refining strategies to maintain efficiency and scalability in handling complex API ecosystems. Check out Rama Palaniappan’s insightful[session at the GraphQL Summit 2023](https://www.youtube.com/watch?v=GB-THCI0Mw0) to learn more.


Explore other inspiring[customer stories](https://www.apollographql.com/customers) and discover how Apollo GraphQL can help future-proof your API strategy. Learn how your organization can modernize your API platform with GraphQL by[consulting the experts at Apollo](https://www.apollographql.com/contact-sales?referrer=nav-contact-sales) .


Written by


Ishwari Lokare


[Read more by Ishwari Lokare](https://www.apollographql.com/blog/author/ishwari)
