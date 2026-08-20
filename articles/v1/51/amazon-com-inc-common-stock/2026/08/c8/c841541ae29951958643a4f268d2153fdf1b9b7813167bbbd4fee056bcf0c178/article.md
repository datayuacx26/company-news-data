---
schema_version: "1.0.0"
document_id: "c841541ae29951958643a4f268d2153fdf1b9b7813167bbbd4fee056bcf0c178"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/iam-policy-autopilot-now-supports-terraform-plan-files"
published_at: "2026-08-18T17:19:00+00:00"
first_seen_at: "2026-08-18T22:19:11.708664+00:00"
fetched_at: "2026-08-18T22:19:13.543416+00:00"
content_hash: "sha256:eda48758b001a848129e28db4012aea16050c67f11510854daa9ee4001b566b5"
---

# IAM Policy Autopilot now supports Terraform plan files

IAM Policy Autopilot can now generate baseline IAM policies directly from a Terraform plan file. IAM Policy Autopilot is an open source tool, launched at re:Invent 2025, that analyzes your code to deterministically create scoped-down IAM policies you can refine as your application evolves, reducing the time you spend writing IAM policies and troubleshooting access issues. Until now the tool analyzed application source code, but it was not possible to generate policies for deploying AWS infrastructure defined via Infrastructure as Code.


Now you can pass a Terraform plan file as input, and IAM Policy Autopilot applies a deterministic analysis to produce a policy scoped to the CRUD functions of the resources in that plan. The generated policies reference specific resource ARNs rather than wildcards, when possible. Supporting policy generation for deploying AWS infrastructure defined via Terraform has been the most requested capability since IAM Policy Autopilot launched, and it complements the existing Terraform-aware analysis, which cross-references Terraform resource definitions with SDK calls in your application code to resolve ARNs.


IAM Policy Autopilot is available at no additional cost and runs on your own machine. To get started, visit the[IAM Policy Autopilot GitHub repository](https://github.com/awslabs/iam-policy-autopilot) .
