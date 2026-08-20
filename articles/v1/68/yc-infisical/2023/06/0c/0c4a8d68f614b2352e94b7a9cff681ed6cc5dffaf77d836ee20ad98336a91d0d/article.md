---
schema_version: "1.0.0"
document_id: "0c4a8d68f614b2352e94b7a9cff681ed6cc5dffaf77d836ee20ad98336a91d0d"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/integration-infisical-checkly"
published_at: "2023-06-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:c3ce592748934986c5e85dce5749114cb596bf3e263f9b5901d9819fcf4c8ad9"
---

# Managing Environment Variables in Checkly with Infisical

### What is Checkly?


Checkly is the API & E2E monitoring platform for the modern stack: programmable, flexible and loving JavaScript. It unites E2E testing and monitoring in one **monitoring as code** (MaC) workflow, enabled by the Checkly CLI. Monitoring as code is a methodology for managing your monitoring infrastructure using code. It consists of three key steps:


1. **Code** — Define your monitoring setup as code, so you can version, automate and scale them with ease.
2. **Test** — Run your monitors as E2E tests against production and staging from your CI pipeline.
3. **Deploy** — Deploy and lifecycle your monitoring alongside your application deployments.


With MaC, your synthetic monitoring is now programmable, testable, reviewable and works with your dev pipeline. From your IDE, via PR to CI.


### What is Infisical?


Infisical is the #1 open source secrets management platform. It helps you organize your secrets in one end-to-end encrypted storage. From there on, you are able to distribute secrets wherever you need them:


1. Locally as a replacement for .env files (automatically pull the latest version of secrets without any manual syncing needed).
2. Into your CI/CD and production-level 3rd-party tools using Infisical's automatic integrations (e.g., Checkly, Vercel, AWS).
3. Into the infrastructure tools that you might already be using (e.g., Docker, Kubernetes, Terraform).


On top of that, Infisical provides automatic code scanning capabilities to prevent hardcoding secrets and leaking those to git.


### How does Infisical help?


Using Infisical, your team can now manage environment variables in Checkly directly from a single source of all your secrets.


For example, you are able to create a project (or a folder inside a project) that would automatically sync to Checkly's environment variables.


This is useful because, most likely, you would also be using Checkly's environment variable in other destinations like Vercel or GitHub Actions. With Infisical, you would just need to update them once inside your dashboard for them to be propagated everywhere.


On top of that, you get automatic secret versioning so that you can always rollback if you made an error, and you are able to track which services/users are acessing which secrets.


You can find more information in[the docs](https://infisical.com/docs/integrations/cloud/checkly) .


### Conclusion


Using Insicial and Checkly together will enable you to code, test, and deploy synthetic monitoring at scale while keeping your security posture with relation to developer secrets.


To learn more about how to get started with Infisical, schedule a custom demo[here](https://infisical.com/scheduledemo) or[sign up](https://app.infisical.com/signup) directly for a free account.


To learn more about Checkly, sign up[here](https://www.checklyhq.com/) .
