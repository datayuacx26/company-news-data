---
schema_version: "1.0.0"
document_id: "fe863f1be75b8f487dafe14dcc76be28f54eb909e0e55b698c5e80eca76912a6"
company_key: "yc-iomete"
company: "IOMETE"
source_id: "yc-iomete-news-import-000d9716a3eb"
canonical_url: "https://iomete.com/resources/blog/federated-query-explained"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-22T00:31:44.448316+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:015767eb0b4d8684f1875e359e29cac5a490b769ee4119c7dd8735aaeec3d474"
---

# Federated query: how it works and when to use it

Compliance in a self-hosted lakehouse uses the same controls as compliance for any other internal system: IAM integration with the organization's identity provider, audit logging into a customer-owned log store, network policies that restrict egress, and encryption keys held in a customer-managed KMS or HSM.


The audit advantage compared to a managed cloud service is direct evidence. Every component runs inside the regulated environment, so compliance teams can demonstrate processing-infrastructure control to auditors from systems the organization already operates, rather than depending on vendor SOC reports or sub-processor disclosures.


In IOMETE deployments, audit logs, encryption keys, and IAM integration all run inside the customer's Kubernetes namespace, which is how DORA, GDPR Article 32, NIS2, and EU AI Act evidence requirements are typically met.
