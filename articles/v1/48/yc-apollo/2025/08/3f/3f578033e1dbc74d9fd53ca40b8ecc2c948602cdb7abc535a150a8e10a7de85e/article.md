---
schema_version: "1.0.0"
document_id: "3f578033e1dbc74d9fd53ca40b8ecc2c948602cdb7abc535a150a8e10a7de85e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-the-apollo-graphos-operator-in-public-preview"
published_at: "2025-08-05T06:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:a5d1fa17fad7cce6834afd198edf82c7247d79ab928a6f7e4415551e63028752"
---

# Announcing the Apollo GraphOS Operator in Public Preview

### *Simplifying GraphQL API Management on Kubernetes*


As[GraphQL adoption accelerates](https://www.apollographql.com/resources/gartner-when-to-use-graphql-to-accelerate-api-delivery) within organizations, so too does the need for a scalable, reliable API platform that empowers your developers and accelerates innovation. Today, we’re excited to introduce in public preview the[Apollo GraphOS Operator](https://apollographql.com/docs/apollo-operator) : the best way to manage GraphQL deployments within Kubernetes.


## Why we built the Apollo GraphOS Operator


Setting up a best-practices GraphQL development workflow often requires a significant upfront investment to create custom automation for coordinating the deployment process to production, setting up ephemeral development environments, implementing progressive deployments, rollbacks, and more.


The Apollo GraphOS Operator is designed to dramatically reduce the initial and on-going effort by delivering out-of-the-box best-practice GraphQL management for Kubernetes. Over time, it will allow platform and infrastructure teams to:


- **Increase development velocity by empowering developers to easily deploy new GraphQL environments.** Using the operator, individual development teams can easily deploy new environments aligned with the company and platform team requirements. Ephemeral development environments can be utilized to validate changes early in the development cycle. New graphs can be easily deployed and managed, supporting new workloads without federation.
- **Safely deploy to production with progressive rollouts and quick rollbacks.** Validate supergraph changes on a portion of the overall GraphOS Runtime fleet, affecting only a small percentage of traffic at first. In the event any issues are discovered, quickly rollback to the prior schema without having to recompose or waiting for checks to run.
- **Monitor status and health using Kubernetes events and resource statuses** , which integrates directly with many observability solutions and deployment management tools like Datadog, Argo CD, and more.
- **Manage GraphQL environments declaratively or via Studio** . Aligns with development team preferences by supporting both declarative as well as UI-based management. Teams who prefer a declarative approach can use Kubernetes resources to manage their GraphQL deployments, aligning with common Kubernetes and GitOps practices. Teams who prefer a UI-based approach can continue to use[GraphOS Studio](https://www.apollographql.com/graphos/studio) like they do today.


## What you can do with the Public Preview


With the public preview of the Apollo GraphOS Operator, teams can now:


- Declaratively manage fleets with Kubernetes resources, enabling teams to manage their GraphQL supergraphs with the same declarative, GitOps-friendly approach they use for the rest of their Kubernetes workloads. Additional details on declarative management are available in our[documentation](https://apollographql.com/docs/apollo-operator/resources/supergraph) and our[technical blog post](https://www.apollographql.com/blog/getting-started-with-apollo-graphos-operator/) .
- Continue to support GraphOS Studio managed router fleets, while providing the foundation for upcoming features like canary deployments.
- Begin testing the operator so that they can more easily and quickly adopt upcoming features once they are available.


## Who should consider trying the operator


Customers who are familiar with Kubernetes and are either actively or interested in deploying their graph infrastructure on it, should consider evaluating the operator. Cloud and on-premise Kubernetes deployments are supported, including common implementations like GKE and EKS. The minimum required Kubernetes version is 1.30.


Customers who are interested in utilizing some of the upcoming features may also be interested in starting testing now, so that your feedback can be incorporated and upcoming features can be utilized more rapidly.


## The road ahead: A simpler future for GraphQL operations


Our long-term vision for the Apollo GraphOS Operator is to make it the easiest, most robust way to run GraphQL in Kubernetes environments. Future releases after GA will further streamline the experience, adding capabilities that previously required extensive custom development:


- **Canary deployments** for safe supergraph schema changes
- **Fast rollbacks** when issues arise
- **Ephemeral environments** for development and testing
- **Progressive router upgrades** with minimal downtime
- **Self-healing router instances** to improve resiliency
- **Support for managing the Apollo MCP** **Server** to seamlessly connect AI agents to the runtime environments powering your graph


These enhancements will empower teams to focus on building exceptional GraphQL experiences while letting the operator handle the deployment complexity.


## How to get started


The Apollo GraphOS Operator is available now as a public preview. If you’re building GraphQL services on Kubernetes, this is the fastest path to scalable, best-practices supergraph infrastructure.


- Check out our[documentation](https://apollographql.com/docs/apollo-operator) and[technical blog post](https://www.apollographql.com/blog/getting-started-with-apollo-graphos-operator/) for detailed information
- Join our[community forum](https://community.apollographql.com/t/announcing-the-apollo-graphos-operator-in-public-preview/9339) to share your experiences
- Let us know what features you need next via[Apollo Support](https://support.apollographql.com/)


Written by


Josh Lambert


[Read more by Josh Lambert](https://www.apollographql.com/blog/author/jlambert)
