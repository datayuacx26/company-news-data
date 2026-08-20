---
schema_version: "1.0.0"
document_id: "fe04202ba211095d3ae893bad7c33f9a4c252e6fffb4ea950e4874c8e9c896ce"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/infisical-update-september-2023"
published_at: "2023-10-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:d942f94081ed9e288489f5af2070322978d38682fc847ce791d1a28ea97b5ad4"
---

# Infisical Update September 2023

In September, our team addressed support requests, feature requests, and issues whilst keeping inline with the product roadmap. Most notably, we shipped a big rehaul to Infisical’s permission system: RBAC.


Let’s dive into this exciting update!


### Role-based Access Control (RBAC)


In the past, you may have used Infisical’s project-level permission system. Although the system worked, its capabilities were limited to managing project member` read/write` access to secrets across various environments in a project. With a full-fledged RBAC system now in place, we’re proud to say that you can now:


- Define 20+ permission types from organization to project-level controls. Amongst many new controls, you can now define which team members should be able to create, read, update and delete (CRUD) secrets, environments, integrations, service tokens, roles, webhooks, and so much more.
- Group permission sets into roles and assign them to users. Infisical provides a few default roles for you to get started such as the` admin` ,` member` , and` viewer` roles for projects.


Re-built with extensibility in mind, RBAC got your access control requirements covered.


### Dashboard Update


If you’ve been following Infisical, then you know that we continuously optimize the dashboard page for performance, security, and developer experience. Notably, in this update we:


- Deprecated the batch CRUD secrets endpoint that previously served all dashboard secret operations.
- Instead, we now perform CRUD operations either one secret at a time or in batches where each batch consists of the same operation. Now, for example, users are encouraged to create one secret at a time unless uploading a` .env` file wherein the batch endpoint for creating multiple secrets is used.
- Improved the UI for the rollback screen so you can now see what changed from one snapshot to the next.


### Integrations


In this update, we shipped one new integration with[Qovery](https://www.qovery.com/) .


We also added a few new sync behavior options for a couple integrations to let users prevent Infisical from overriding existing variables in the target integration platform:


- [GCP Secret Manager](https://infisical.com/docs/integrations/cloud/gcp-secret-manager) : You can now prefix/suffix secrets that get synced to GCP Secret Manager; existing secrets in GCP Secret Manager without the prefix/suffix are left untouched. You can also label them anything like` managed-by:infisical` (recommended).
- [GitLab](https://infisical.com/docs/integrations/cicd/gitlab) : Similar to GCP Secret Manager, you can now also prefix/suffix secrets that get synced to GitLab.


As always, if you’re looking to contribute to Infisical, you should note that integrations are some of the easiest features to build and get started with the codebase.


Join our community[Slack](https://infisical.com/slack) to engage and learn more about how you can get started contributing to Infisical.


### Service Token Generation via CLI


One common feature request we received was the capability to manage service tokens via CLI. With this update, you can now do so with the` infisical service-token` command.


For example, to create a service token via CLI, you can use:


```text
infisical service-token create --scope=dev:/global --scope=dev:/backend --access-level=read --access-level=write


```


## 👋


That's it. If you have any questions, you can always ask those on[Slack](https://infisical.com/slack) . If you have any features requests, you're welcome to[open a GitHub issues](https://github.com/Infisical/infisical/issues) .


Stay tuned for more awesome updates next month!
