---
schema_version: "1.0.0"
document_id: "b810f9a665f352baf65f44030704931a13f658327c99be21819c855b3d284b48"
company_key: "blend-labs-inc-class-a-common-stock"
company: "Blend Labs Inc."
source_id: "blend-labs-inc-class-a-common-stock-rss-4631133ca4a9"
canonical_url: "https://full-stack.blend.com/migrating-the-kubernetes-network-overlay-with-zero-downtime.html"
published_at: "2020-02-03T23:46:57+00:00"
first_seen_at: "2026-07-20T23:18:43.300114+00:00"
fetched_at: "2026-08-20T00:34:47.711311+00:00"
content_hash: "sha256:6de5ed5c0f8b9369fe5614b9cc53c6313fc0d1192e0f9cb0bea707d2427a58cd"
---

# Migrating the Kubernetes Network Overlay With Zero Downtime

At Blend, we make extensive use of Kubernetes on AWS to power our infrastructure. Kubernetes has many moving parts, and most of these components are swappable, allowing us to customize clusters to our needs. An important component of any cluster is the Container Network Interface (CNI), which handles the networking for all pods running on the cluster. Choosing the right CNI for each use case is critically important and making changes, once serving production traffic, can be painful.
