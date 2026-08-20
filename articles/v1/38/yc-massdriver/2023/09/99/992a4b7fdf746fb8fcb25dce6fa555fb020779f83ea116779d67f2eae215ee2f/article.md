---
schema_version: "1.0.0"
document_id: "992a4b7fdf746fb8fcb25dce6fa555fb020779f83ea116779d67f2eae215ee2f"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/massdriver-remains-unaffected-by-hashicorps-terraform-licensing-changes"
published_at: "2023-09-25T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:8c49a4d27db07482fcacf6418b379eaa6f113a17f8825737c179929797c66023"
---

# Massdriver Remains Unaffected by HashiCorp's Terraform Licensing Changes

HashiCorp recently announced a significant change in the licensing for some of their core products, like Terraform and Vault. They are shifting from an open-source MPL 2.0 license to a more restrictive Business Source License (BUSL) model.


At Massdriver, we want to assure our users and community that this licensing shift does NOT impact our platform or customers.


Here’s why:


- We compete with HashiCorp’s Waypoint product, but many of our official marketplace modules are written using Terraform. From the[license FAQ](https://www.hashicorp.com/license-faq#competitive-product-bsl-coverage) : “What the BSL license would not allow is hosting or embedding Terraform in order to compete with Terraform, or hosting or embedding Vault to compete with Vault.”
- Our customers always have a choice in the provisioning engine. We allow customization and support tools beyond just Terraform. Teams aren’t locked into any single orchestration tool.
- Our Terraform provisioner continues to run on older versions of Terraform under the original MPL 2.0 license. These versions will receive critical security backports through the[end of 2023.](https://www.hashicorp.com/license-faq#security-patch-backporting)
- We are founding members of the[OpenTofu](https://opentofu.org/) project, the community-driven fork of Terraform. We are already working to enable OpenTofu support in our platform for customers who want an alternative to HashiCorp-led Terraform.


At Massdriver, open-source infrastructure software is critical for customer choice and preventing vendor lock-in. We are committed contributors to projects like OpenTofu that advance open ecosystems.


As HashiCorp drifts from its open-source roots, Massdriver remains dedicated to empowering engineers through open collaboration and simplifying cloud operations. We’ll keep listening to our customers and community and provide the flexible tools you need without restrictions.


Try Massdriver today to simplify cloud management on your terms, not any single vendor’s. The future remains open!


‍
