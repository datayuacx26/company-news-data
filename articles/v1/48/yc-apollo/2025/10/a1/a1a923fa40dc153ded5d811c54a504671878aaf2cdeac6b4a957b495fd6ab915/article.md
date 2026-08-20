---
schema_version: "1.0.0"
document_id: "a1a923fa40dc153ded5d811c54a504671878aaf2cdeac6b4a957b495fd6ab915"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-graph-artifacts"
published_at: "2025-10-07T07:00:02+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:d324e2c19546af6a8fda3edcff03d98c08c65c242ca0210c6d4af9f86cc2709c"
---

# Introducing Graph Artifacts: Versioned, Immutable Schemas for Confident Deployments

***Every DevOps team knows the pain:** a schema breaks production, and rolling back means republishing, recomposing, and crossing your fingers. Graph artifacts change that—giving you versioned, immutable schemas you can deploy, roll back, and audit with confidence.*


DevOps best practices have given engineering teams confidence in how they ship software—except when it comes to supergraph schemas. The[Apollo Router](https://www.apollographql.com/docs/graphos/routing) is often configured to auto-update to the “latest” schema, making it difficult to lock to a known version, roll back safely, or audit what was running in production at any given time. Teams managing production supergraphs need the same controls they have for application code: versioning, instant rollbacks, and clear audit trails.


**Graph artifacts, now in Public Preview** , bring that control to schemas. Every publish automatically produces a versioned, immutable package you can pin, roll back, and audit. Your team gets the stability and traceability you expect from modern DevOps—no surprises, no drift, just the exact schema you intended.


## Why graph artifacts?


Teams running production supergraphs—especially in enterprises and regulated industries—need more control and traceability over schema releases. When routers automatically update to the latest published schema, teams face three significant challenges:


- **No version pinning** : You can’t lock a router to a specific schema version for testing or gradual rollouts
- **Slow rollbacks** : Fixing a bad release requires republishing schemas and waiting for recomposition
- **Missing audit trail** : There’s no way to confirm which exact schema was running at a specific point in time


Graph artifacts solve these problems. Each artifact is identified by a unique SHA-256 digest, creating an immutable reference you can deploy, roll back to, and audit with confidence.


## How they work


Graph artifacts require[Apollo Router v2.7](https://github.com/apollographql/router/releases) or later. The publishing flow remains the same—you publish your subgraphs to GraphOS—but now every publish automatically generates an[OCI](https://opencontainers.org/) -compliant artifact stored in the GraphOS registry with a unique SHA-256 digest. Your router configuration pins to that digest, ensuring it runs only that specific schema version.


Figure 1: Graph artifact lifecycle


## Manually configure your router


Graph artifacts and their SHA digests appear on the **Launches** page in Apollo Studio. Click the copy button to copy an artifact’s reference URI, then configure your router to run that specific schema version.


Figure 2: Graph artifact build


Once you have a graph artifact’s reference URI, point your router to it by setting this environment variable:


```text
APOLLO_GRAPH_ARTIFACT_REFERENCE="artifact.api.apollographql.com/my-graph-id@sha256:6c3c62..."
```


This ensures your router only runs that specific schema version until you change the configuration to a new artifact reference.


## Integrate graph artifacts with CI/CD


Integrate graph artifacts into your CI/CD pipeline by programmatically fetching the latest artifact reference URI using the[GraphOS Platform API](https://www.apollographql.com/docs/graphos/platform/platform-api) :


```text
query GetGraphArtifactHistory($graphId: ID!, $variantName: String!) {
graph(id: $graphId) {
variant(name: $variantName) {
launchHistory {
graphArtifact {
completedAt
status
location {
uri
}
}
}
}
}
}
```


The artifact reference is returned in the` location.uri` field of the graph artifact with the latest` completedAt` value and a` status` value of GRAPH_ARTIFACT_COMPLETED. Write this URI to your router’s configuration during deployment to ensure it runs that specific schema version.


## Deploy with the Apollo GraphOS Operator for Kubernetes


The[Apollo GraphOS Operator](https://www.apollographql.com/graphos/apollo-graphos-operator) —now generally available—supports graph artifacts, enabling schema versioning in Kubernetes-native deployments. The Operator automatically releases new artifacts and handles orchestration through Kubernetes. Learn more in the[Operator documentation](https://www.apollographql.com/docs/apollo-operator) .


## Open standards, flexible tooling


Graph artifacts leverage the[Open Container Initiative (OCI)](https://opencontainers.org/) image format and the GraphOS registry is OCI-compliant. This means you can use industry-standard tools like[ORAS](https://oras.land/docs/) to pull and inspect graph artifacts directly from the registry—giving you flexibility in how you integrate artifacts into your workflows. Learn more in the[graph artifacts documentation](https://www.apollographql.com/docs/graphos/platform/schema-management/delivery/graph-artifacts) .


## What this means in practice


Graph artifacts transform how teams manage schema deployments across four critical workflows:


**Promote from staging to production with zero drift**


Publish a schema change and GraphOS creates artifact` sha256:abc123` . Pin your staging router to that artifact, run your full integration test suite, and validate performance. When you’re ready, pin production to that exact same` sha256:abc123` reference—the identical schema that passed all your tests. No recomposition between environments, no timing windows, no surprises.


**Instantly rollback without republishing**


At 3pm, you deploy artifact` sha256:def456` to production. By 3:15pm, error rates spike. Instead of emergency schema changes and waiting for recomposition, you copy the previous artifact reference (` sha256:abc123` ) from the Launches page, update your router configuration, and redeploy. You’re back to stability in under 2 minutes—versus the several minutes a traditional republish-and-wait approach requires.


**Fleet-wide consistency across distributed deployments**


Lock all your production routers—whether you’re running 3 or 300—onto a single artifact SHA. Every router in every region runs the identical schema version until you explicitly update them. No race conditions, no partial rollouts, no “works in *us-east* but not *eu-west* ” troubles.


**Audit trails for compliance and debugging**


When a customer reports an API issue from last Tuesday at 2pm, check the Launches page to see exactly which artifact was running at that time. Each artifact is immutable and tied to a specific launch, giving you a complete timeline of which schema version was active when. Perfect for post-mortems, compliance requirements, and reproducing production issues.


## What’s next


The Public Preview is just the start. Upcoming improvements include:


- **Enhanced support** for CI/CD integration
- [Rover CLI](https://www.apollographql.com/docs/rover) **support** for artifact history and details
- **Custom tags** to assign human-friendly identifiers to artifacts
- **Hot-reloading** of graph artifacts in the router by automatically following modifications to a specific tag


## Start using graph artifacts today


The graph artifacts Public Preview is available now in GraphOS for all supergraph users running[Apollo Router v2.7](https://github.com/apollographql/router/releases) or later. Get started today:


- Open the **Launches** page in Studio—your recent publishes already have graph artifacts
- Copy an artifact reference and configure your router
- Share your feedback in the[Apollo Community](https://community.apollographql.com/t/graph-artifacts-public-preview/9449)


Read the[full documentation](https://www.apollographql.com/docs/graphos/platform/schema-management/delivery/graph-artifacts) to learn more about CI/CD integration, rollback strategies, and OCI tooling support.


Written by


David Staheli


[Read more by David Staheli](https://www.apollographql.com/blog/author/dstaheli)
