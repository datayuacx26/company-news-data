---
schema_version: "1.0.0"
document_id: "c677cb650fff5b32a965d42c27e9e7870340ab3e1cf16378f935731ba8214129"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/introducing-runner"
published_at: "2023-08-07T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:339a44456d05a800274023f0aebef5eacd5e044de9aa1de16ec83fe93a2ca6fb"
---

# Introducing, DeepSource Runner

Last updated on Aug 7, 2023


We're excited to launch DeepSource Runner, a hybrid-cloud based deployment model to run DeepSource that offers the convenience of SaaS with the security and data ownership of on-premise deployments. This is achieved by offloading all services that don't deal with source code to DeepSource's cloud while keeping the service that accesses and processes source code on-premise. With Runner, setup time and maintenance efforts are significantly reduced as Runner is a single-binary agent, with no source code ever leaving the customer's infrastructure.


## Advantages of on-premise model


When we first launched DeepSource in February 2019, it was exclusively available as Software as a Service (SaaS) product, accessible only through the Cloud. While a SaaS product offers many advantages, such as no need for setup and maintenance, it wasn't the best fit for companies with strict data ownership, compliance and security requirements. To cater to these needs, we launched the Enterprise Server in late 2020. This is a fully self-hosted version of DeepSource that you can operate within your private cloud infrastructure with monthly over-the-air updates.


Some of the key advantages include,


- **Data Ownership and Security** : With Enterprise Server, all your data resides within your private cloud infrastructure. This means you have full control and ownership of your data, which is crucial for businesses, especially large enterprises. By hosting DeepSource on your private cloud, you can enforce your own security protocols and standards.
- **Compliance** : If your company operates in a regulated industry or a region with strict data protection laws, hosting DeepSource on your own servers becomes a requirement to comply with these regulations.


## Limitations of on-premise model


While DeepSource Enterprise Server offers numerous benefits, it also comes with a few potential limitations,


- **Setup and Maintenance** : Unlike the SaaS solution, the self-hosted version requires initial setup and ongoing maintenance. This can involve additional time and resources.
- **Support** : As DeepSource's support team doesn't have direct access to the deployments, debugging requires co-ordination by the customer and our engineering team for troubleshooting.


## Runner, a hybrid approach


DeepSource Runner aims to solve these issues by combining SaaS and on-prem models. Here's how it works. Runner is split into two parts: the Control Plane and the Data Plane. The Control Plane is where we handle all the behind-the-scenes work. It's hosted on our side and takes care of the main functions of the product like integrations with Version Control Providers, team dashboard, user management, etc. The Data Plane (Runner) is where your source code is accessed and analyzed. It is a single-binary[open-source](https://github.com/deepsourcecorp/runner) agent set up on your private cloud infrastructure (AWS, GCP, Bare metal). The two planes communicate securely with whitelisted IPs between the customer's premises and DeepSource Cloud to exchange metadata. With Runner, issues can be addressed faster, even proactively, leading to better SLAs and support. This model effectively addresses the challenges of setup, maintenance, and troubleshooting on-premise deployments while offering SaaS convenience for our customers.


To learn more, refer to the[documentation](https://docs.deepsource.com/docs/runner-introduction) (or)[schedule a product demo](https://deepsource.com/contact/sales) with us.
