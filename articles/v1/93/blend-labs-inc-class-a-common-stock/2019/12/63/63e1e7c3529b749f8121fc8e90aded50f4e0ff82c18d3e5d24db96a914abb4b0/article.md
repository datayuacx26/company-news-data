---
schema_version: "1.0.0"
document_id: "63e1e7c3529b749f8121fc8e90aded50f4e0ff82c18d3e5d24db96a914abb4b0"
company_key: "blend-labs-inc-class-a-common-stock"
company: "Blend Labs Inc."
source_id: "blend-labs-inc-class-a-common-stock-rss-4631133ca4a9"
canonical_url: "https://full-stack.blend.com/moving-a-business-critical-monolith-to-kubernetes.html"
published_at: "2019-12-19T08:00:00+00:00"
first_seen_at: "2026-07-20T23:18:43.300114+00:00"
fetched_at: "2026-08-20T00:34:47.711311+00:00"
content_hash: "sha256:0735a018def020135025c821b516d1579e6062f4500f2c7f9e8b273b6d46a78a"
---

# Moving a Business-Critical Monolith to Kubernetes

At Blend we have been pushing for Kubernetes adoption across all services for the last two years. Migrating our monolith from AWS ECS to a self-hosted Kubernetes cluster marked a major milestone. Moving business-critical applications in general requires deliberate planning and in many cases major updates to deployment pipelines, system monitoring, testing, and infrastructure. This post will explore the migration strategies and lessons learned as we got the monolith up and running across deployments with zero downtime.
