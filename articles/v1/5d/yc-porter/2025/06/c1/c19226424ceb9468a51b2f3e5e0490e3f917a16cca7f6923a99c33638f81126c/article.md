---
schema_version: "1.0.0"
document_id: "c19226424ceb9468a51b2f3e5e0490e3f917a16cca7f6923a99c33638f81126c"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-d854f87d935e"
canonical_url: "https://www.porter.run/changelog/tailscale-integration-cost-optimization-granularity-helicone-add-on"
published_at: "2025-06-26T00:00:00+00:00"
first_seen_at: "2026-07-25T19:39:33.596384+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:51590a5b03f253059718f1e5d5f6abe272268a9174a5dc950fef78f228b7ad77"
---

# Tailscale Integration, Cost Optimization Granularity, Helicone Add-on, and Improved App Alerts

## **Tailscale Integration**


We’ve partnered with[Tailscale](https://tailscale.com/) to create an updated, official integration for Porter.


Some benefits are finer-grained permissions through OAuth clients and automatic management of auth keys for your cluster, meaning no need for manual renewal.


‍


‍[Here](https://docs.porter.run/other/tailscale#tailscale-on-porter) are the docs.


## **Cost Optimization with Reserved Instances**


Users with cost-optimized node groups can now restrict the instances that are selected to a subset of instance families.


‍


If a user has reserved instances of a particular instance family, they can still take advantage of the dynamic optimization across the different EC2 sizes.


## **Helicone Add-on**


‍[Helicone](https://www.helicone.ai/) (YC W23) is now available as an add-on on Porter. Helicone is an advanced observability platform, providing a unified view of performance, cost, and user interaction metrics for various LLM providers.


## **Validation for porter.yaml**


Users can run` porter apply validate -f porter.yaml` to check the syntax and structure of` porter.yaml` files prior to deploying.


## **Environment Groups Update**


The Environment Group view now has a Versions tab that allows you to see the ten previous versions of your environment groups.


‍


This will allow for easier recovery if environment group changes need to be reverted.


## **Improved App Alerts**


Previously, users had to click into each individual application to be able to see the notification count.


The apps list view now has notification counts for the most recent revisions of each application.


‍


This allows users to get a high level overview of which apps are experiencing failures, restarts, etc.
