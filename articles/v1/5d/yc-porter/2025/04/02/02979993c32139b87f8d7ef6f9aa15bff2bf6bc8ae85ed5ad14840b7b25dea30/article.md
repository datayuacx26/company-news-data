---
schema_version: "1.0.0"
document_id: "02979993c32139b87f8d7ef6f9aa15bff2bf6bc8ae85ed5ad14840b7b25dea30"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-d854f87d935e"
canonical_url: "https://www.porter.run/changelog/aws-cost-optimization-drata-soc-2-controls-and-gpu-time-slicing-for-gcp"
published_at: "2025-04-03T00:00:00+00:00"
first_seen_at: "2026-07-25T19:39:33.596384+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:558bd4ae501b8be6873174a3f41a8db624a229b0540dc9d6f03487a519f4f616"
---

# AWS Cost Optimization, Drata SOC 2 Controls, and GPU Time-Slicing for GCP

## **AWS Cost Optimization**


All new Porter users running on AWS now have Cost Optimization for their application workloads enabled by default. This feature has resulted in users seeing >50% savings on EC2 spend for their app workloads.


‍


Users can toggle the feature on (or off) through the Infrastructure tab. Support for Reserved Instances and Savings Plans will be coming in the next release. More details can be found in our docs[here](https://docs.porter.run/cloud-accounts/node-groups#cost-optimization) .


**Beta users have instantly reduced EC2 spend for application workloads by more than 50% -**[Toma](http://toma.com/) **(pictured above) went from spending over $13k per month to ~$6k per month.**


## **Drata Controls for Compliance**


Porter Compliance now ensures all controls required for SOC 2 audits on Drata pass. Specifically, Test 222 on the Drata dashboard, which a few users reported as failing, has now been accounted for.


## **GPU Time-Slicing for GCP**


Time-sharing and MPS (Multi-Process Service) for GPUs in GKE is now supported. This means multiple workloads can run concurrently on a single GPU or multiple workloads can take turns using the full GPU resources.


## **I/O-Optimized Aurora**


Databases provisioned for preview environments can now be Aurora I/O-Optimized - for applications with high I/Os, this feature offers up to 40% cost savings where I/O charges exceed 25% of the total Aurora database spend. Support for IO-Optimized will be available for all Aurora DBs provisioned through Porter soon.


## **Path-based Routing Across Apps**


Porter.yaml now allows for path-based routing across services and applications. Check out the docs[here](https://docs.porter.run/deploy/configuration-as-code/services/web-service) .
