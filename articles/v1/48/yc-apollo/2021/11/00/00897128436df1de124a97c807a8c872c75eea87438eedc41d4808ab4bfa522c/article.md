---
schema_version: "1.0.0"
document_id: "00897128436df1de124a97c807a8c872c75eea87438eedc41d4808ab4bfa522c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/moving-apollo-federation-2-to-the-elastic-license-v2"
published_at: "2021-11-03T09:14:04+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:dc4f3ac54735c5a5a160ad48f9b61c58da1a8421b47401b46c60fe3e14009b7f"
---

# Moving Apollo Federation 2 to the Elastic License v2

Today, we are introducing[Apollo Federation 2](https://www.apollographql.com/blog/announcement/backend/announcing-federation-2) , a major update to our graph composition technology that lets developers build and evolve a unified graph in a modular, principled way. Federation 2 is a new implementation of our query planner and execution gateway that introduces a more flexible model for how types and fields compose across subgraphs. As part of this release, we’re changing the license for the new core federation libraries to the[Elastic License v2](https://www.apollographql.com/docs/resources/elastic-license-v2-faq) (ELv2).


## **Why the new license?**


The Apollo developer community is at the heart of everything we do. As stewards of our community, we have a responsibility to prevent harm from anyone who intends to exploit our work without contributing back. We want to continue serving you by funding the development of important open-source graph technology, like our MIT-licensed projects Apollo Client, Apollo Server, and` apollo-rs` , for years to come. To honor that commitment, we’re moving Apollo Federation 2 to the Elastic License v2 (ELv2).


Open-source licensing is evolving with the cloud. Many successful companies built on open-source technology (such as[Elastic](https://www.elastic.co/blog/elastic-license-v2) ,[MongoDB](https://www.mongodb.com/blog/post/mongodb-now-released-under-the-server-side-public-license) , and[Confluent](https://www.confluent.io/blog/license-changes-confluent-platform/) ) have followed the path we’re taking to protect their communities and combine open, collaborative development with the benefits of cloud services that are easy to adopt and manage. In that spirit, we worked with[Heather Meeker](https://heathermeeker.com/about-me/) , a lawyer specializing in open-source software who has worked with Mozilla and Elastic, to steer us toward the best open license for Apollo Federation.


## **Elastic License v2**


Instead of creating our own license, we chose ELv2 because of its simplicity and permissiveness. You shouldn’t have to involve your company’s legal department to adopt Apollo Federation, which is why we chose a license that’s already familiar if you’re using Elasticsearch. Although this change shouldn’t affect the vast majority of our community and customers, we want your migration to Apollo Federation 2 to be friction-free. Read[the FAQ](https://www.apollographql.com/docs/resources/elastic-license-v2-faq) for more guidance.


The[Elastic License v2 (ELv2)](https://www.elastic.co/licensing/elastic-license) is simple and permissive. You can use, copy, modify, and distribute the software as long as you stay within three limitations. You can’t:


1. Provide federation tooling as a managed service
2. Circumvent the license key functionality or remove/obscure features protected by license keys
3. Remove or obscure any licensing, copyright, or other notices


There is no impact to most of our community and customers. Just as before, running a federated graph in production (both public and private) is permitted, as is modifying or extending any part of the federation code. To address your questions, we’ve included some examples in[our FAQ](https://www.apollographql.com/docs/resources/elastic-license-v2-faq) that explain the limitation on “managed services.”


### **Libraries under the Elastic License v2**


We’re licensing the following libraries under ELv2:


- Apollo Federation 2 gateway and libraries, starting with the v2.0.0-alpha.* release
- Rover composition add-on (` rover-fed2` )
- Apollo Workbench


The license applies to the source code, as well as the binaries and packages distributed on npm and the VS Code Marketplace.


### **Libraries that remain MIT-licensed**


Excluding our Federation 2 libraries, nearly all of our 100+ public repositories on[GitHub](https://github.com/apollographql) are MIT-licensed. The open-source libraries you know and love, such as Rover, Apollo Client, and Apollo Server, will stay MIT-licensed.


Apollo Federation 0.x libraries will remain MIT-licensed. We want you to migrate at your own pace, so we will actively maintain the Apollo Federation 0.x packages by merging compatibility updates, bug fixes, security patches, and select feature enhancements through the end of 2022.


We’re grateful for the subgraph maintainers who have built a healthy ecosystem around Apollo Federation. Over 12 GraphQL servers,[in languages ranging from Kotlin to Ruby](https://www.apollographql.com/docs/federation/other-servers/) , all support Apollo Federation for composing subgraphs. All subgraph libraries under` @apollo/subgraph` will remain MIT-licensed.


We’re committed to developing and maintaining open-source software for the graph ecosystem. It’s why we’ve released software we use to build Apollo Federation, such as[apollo-rs](https://www.apollographql.com/blog/announcement/tooling/apollo-rs-graphql-tools-in-rust/) (a GraphQL parser for Rust), under the MIT license.


## **A call for a standard open license**


When[the Elastic License v2 was released](https://www.elastic.co/blog/elastic-license-v2) , Elastic CEO Shay Banon called upon open-source companies facing a similar decision to “coalesce around a smaller number of licenses.” We’re excited to be part of this coalition of modern infrastructure companies building businesses that empower developers. Adopting software shouldn’t require a legal degree to decipher complex licenses and restrictions, which is why we are championing a single, standard open license with the simplest language and least amount of limitations necessary.


Not only do you deserve a friction-free development experience, you also deserve choice. We’re seeing a rapid shift toward unbundling the cloud where you can compose the best tools and services for your needs by mixing and matching them seamlessly. You should pick the stack that’s best for you, instead of picking a service because it comes from the same vendor providing other parts of the stack.


Apollo has always been about helping developers build amazing applications. Moving the Apollo Federation libraries and gateway to ELv2 helps us focus on our mission: empowering all of you.


Written by


Matt DeBergalis


[Read more by Matt DeBergalis](https://www.apollographql.com/blog/author/matt)
