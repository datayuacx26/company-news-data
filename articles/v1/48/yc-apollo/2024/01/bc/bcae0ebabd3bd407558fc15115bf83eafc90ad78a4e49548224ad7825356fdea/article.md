---
schema_version: "1.0.0"
document_id: "bcae0ebabd3bd407558fc15115bf83eafc90ad78a4e49548224ad7825356fdea"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/new-graphos-platform-apis-enable-blue-green-and-canary-deployments"
published_at: "2024-01-25T12:06:29+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:17.179631+00:00"
content_hash: "sha256:e736d10ecc8af1c89b7908fc6a955fdcbb6272e9d3a13d7fa5acc581c58e81cf"
---

# New GraphOS Platform APIs Enable Blue-Green and Canary Deployments

As platforms, teams, and products evolve, we sometimes need new or different ways to deploy our APIs. Strong Continuous Integration / Continuous Deployment (CI/CD) practices and recovery strategies are necessary components of any[Recovery Time Objective (RTO) and Recovery Point Objective (RPO)](https://aws.amazon.com/blogs/mt/establishing-rpo-and-rto-targets-for-cloud-applications/) .


The recommended GraphOS[continuous deployment](https://www.apollographql.com/tutorials/workshop-summit-graphos-basics/08-changes-process) process uses[Apollo Uplink](https://www.apollographql.com/docs/federation/managed-federation/uplink/) to update the Router fleet in place with new supergraph schemas that pass checks in the GraphOS build pipeline along with automated testing in lower environments such as dev or test. However, some organizations have specific deployment needs that rely on an immutable schema for blue-green deploys or canary releases.


Today, Apollo is excited to release enhanced GraphOS Platform APIs that streamline integration with immutable blue-green and canary deployments workflows beyond the in-place continuous GraphOS updates that most customers use today:


1. [PublishSubgraphsMutation](https://studio.apollographql.com/graph/apollo-platform/variant/main/schema/reference/objects/GraphMutation?_gl=1*1yu3yv*_ga*NDMzNzY1NzQ4LjE3MDI5MTM0MDk.*_ga_0BGG5V2W2K*MTcwNTEwMzIwNC4zMi4xLjE3MDUxMDM1NzEuMC4wLjA.#publishSubgraphs) will publish all of the releases’ subgraphs as a single unit and return the associated launch and downstream Contract launches.
2. [PollLaunchStatusQuery](https://studio.apollographql.com/graph/apollo-platform/variant/main/schema/reference/objects/Launch#status) will poll for the status of any individual launch by ID.
3. [FetchSupergraphSDLQuery](https://studio.apollographql.com/graph/apollo-platform/variant/main/schema/reference/scalars/GraphQLDocument) will fetch the supergraph schema of the launch.


These GraphOS Platform API updates are versatile enough to support many deployment strategies.


[Blue-green deployment](https://www.apollographql.com/docs/federation/managed-federation/deployment/#example-blue-green-deployment) involves maintaining two identical production environments: the “Blue” and the “Green.” Only one of these environments is live at any given time. When it’s time to deploy an update to a supergraph, the team releases it to the inactive environment. For instance, the new version is deployed to “Green” if “Blue” is live. Once the new “Green” version is thoroughly tested and deemed stable, the team switches traffic from “Blue” to “Green.” With these updates to the GraphOS Platform API,[metrics and insights](https://www.apollographql.com/docs/graphos/metrics/) for both stages, the “Blue” and the “Green,” are available in a consolidated place under a single Production variant, which has a single launch history to track schema changes over time.


[Canary deployment](https://www.apollographql.com/docs/federation/managed-federation/deployment/#example-canary-deployment) , on the other hand, is a more gradual process. It involves first rolling out the new supergraph updates to a small subset of users rather than switching simultaneously. This subset, or “canary group,” receives the update and is closely monitored by the platform team for any issues. If the deployment is successful, the team rolls out the update to a more significant portion of the user base and eventually to all users.


While we recommend rolling forward to recover in most cases,[rollbacks](https://www.apollographql.com/docs/federation/managed-federation/deployment/#rolling-back-a-deployment) can be helpful in some scenarios. For example, consider a blue-green deployment where a team deploys the Router and all subgraphs as a single versioned release and houses **immutable schemas** within each Docker image. Then, if the need arises for a rollback, they can simply re-deploy the old Docker images, and the correct supergraph schema will be used. This helps ensure compatibility with existing blue-green workflows that assume an immutable infra mindset.


For platform teams looking to enhance their GraphQL platform stance, stay tuned for additional GraphOS Platform API updates and learn more about[deployment and rollback examples in the GraphOS documentation](https://www.apollographql.com/docs/federation/managed-federation/deployment/) .


Written by


Andrew I. Carlson


[Read more by Andrew I. Carlson](https://www.apollographql.com/blog/author/andrewcarlson)
