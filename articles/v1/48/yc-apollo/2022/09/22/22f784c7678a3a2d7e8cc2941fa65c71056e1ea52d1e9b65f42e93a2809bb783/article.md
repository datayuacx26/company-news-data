---
schema_version: "1.0.0"
document_id: "22f784c7678a3a2d7e8cc2941fa65c71056e1ea52d1e9b65f42e93a2809bb783"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/prevent-breaking-changes-in-downstream-variants-with-contract-checks"
published_at: "2022-09-15T08:53:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:23508b83603b5d1f4ae05107ba519fca236b92e06afb2e77914dedf86ad929d5"
---

# Prevent breaking changes in downstream variants with contract checks

For federated graph variants, Apollo offers schema checks to ensure that proposed changes to subgraphs won’t break supergraph composition (build checks) or your active clients (operation checks).


Today, we’re excited to extend this capability to[contracts](https://www.apollographql.com/blog/announcement/platform/contracts-are-now-generally-available/) !


## What are contract checks?


Contract checks ensure that changes made to a source variant’s subgraphs or to the contract filtering configuration won’t cause breaking changes to any downstream contract variant’s schema or active clients. Each contract check runs two types of tasks to do this:


1. **Build tasks** : Build tasks verify that downstream contract variants build successfully and that filtering rules continue to work as expected.
2. **Operation tasks:** Operation tasks check proposed schema changes against historical operations to the contract variant (and any other selected variant) to verify whether the changes will break any of the contract variant’s active clients.


## How are contract checks triggered?


Contract checks are triggered in two scenarios:


**1. Changes are made to the source variant’s subgraphs and source variants checks are run**


When a source variant has downstream contracts associated with it, Apollo will run an additional task in the checks workflow called a **downstream task** .


When a change is made to any of the source variant’s subgraphs, the checks workflow begins, and the downstream task runs contract checks for all of the downstream contract variants.


**2. Contract filter configuration is edited in Studio**


When you edit the filtering configuration for a contract in Studio, contract checks will run to make sure that the new filter rules can apply without build errors and without causing breaking changes that affect operations from the contract variant’s active clients.


If any checks fail, you’ll see a warning when reviewing the new contract schema, but you can still choose to update the contract even if there are warning or if checks haven’t finished running.


## Blocking downstream contract variants


When checks for a source variant are triggered, checks for all associated contract variants will also run. You can decide which of those contract variants you’d like to mark as` BLOCKING` .


If a contract variant is marked as` BLOCKING` and a check for that contract *fails* , then the source variant’s check status would also be evaluated as *failed* .


## Get started!


Contracts checks are available today on Apollo’s Enterprise plan! Head over to checks configuration in Studio to select which contract variants should be blocking your source variant checks to fail. If you aren’t on Apollo Enterprise and would like to try it out,[talk to us](https://www.apollographql.com/contact-sales/) about an Enterprise trial.


ℹ️ If you are using the Rover CLI to run checks in your CI/CD pipelines, you must upgrade to at least Rover v0.8.2 to get the best and full functionality of contract checks. If you are using Rover v0.8.0 or v0.8.1, your source variants that have associated contract variants *will* start triggering contract checks as described above, however, if a contract check fails, it won’t cause` rover subgraph check` to fail, affecting the accuracy of your CI/CD pipeline.


⚠️ **Note:** Please make sure you are on the latest version of the Studio UI. Click on the bell icon at the top right corner of Studio and if you see *“Update available, click to relaunch”* at the bottom left of the modal then click that to get the latest version.


Written by


Parul Schroff


[Read more by Parul Schroff](https://www.apollographql.com/blog/author/parul)
