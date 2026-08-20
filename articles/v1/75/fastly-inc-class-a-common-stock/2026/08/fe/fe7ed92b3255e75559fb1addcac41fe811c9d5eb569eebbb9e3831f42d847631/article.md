---
schema_version: "1.0.0"
document_id: "fe7ed92b3255e75559fb1addcac41fe811c9d5eb569eebbb9e3831f42d847631"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/terraform-your-fastly-config-in-a-few-commands/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T16:00:05.489723+00:00"
fetched_at: "2026-08-12T16:00:07.028375+00:00"
content_hash: "sha256:c089c3fd5863f6bba2b7e7d3c59b3db5bac419d5581c7ff4a4e67cfd2fb9c80a"
---

# Terraform Your Fastly Config in a few Commands

Your Fastly infrastructure exists. You're managing services, security rules, TLS certificates, and logging endpoints every day. But when someone asks, "Is that in version control? Can we replicate it in another environment?"—the honest answer is probably no.


Infrastructure as Code has become table stakes for serious DevOps teams. It's not optional anymore. Version control, auditability, disaster recovery, team collaboration aren't just nice-to-haves. But migrating an existing Fastly setup to Terraform typically means one of two things: abandon console management and rewrite everything from scratch, or manually map each resource to a Terraform definition. Both are painful and take weeks to implement.


Luckily, there's a third way. Meet **fastly-terraformer** .


## The Problem We're Solving


Fastly is powerful infrastructure. You've already invested time building your edge services, configuring backends, setting up[Next-Gen WAF](https://www.fastly.com/products/web-application-api-protection) (NGWAF) security rules, and wiring logging. The problem isn't *what* you've built—it's that it's not in code.


[Terraform](https://www.fastly.com/documentation/guides/integrations/non-fastly-services/developer-guide-terraform/) exists to solve this. But there's friction: you'd have to understand the full Fastly Terraform provider surface, manually inspect each resource in the console, and write import blocks for everything. For a complex setup that includes multiple services, dozens of domains, NGWAF workspaces with alert integrations the work can take a full weekend or even a week to complete. Or - in the worst case - it turns into something that never happens. Fastly-terraformer removes that friction entirely.


## How fastly-terraformer Works: One Tool, Just a few Commands


The idea behind fastly-terraformer is simple. Point the tool at your Fastly account, and it discovers everything you've built, then generates the Terraform import blocks you need. No rewrites. No reinvention. Just your existing infrastructure, now code.


**Here's what it looks like:**


Copied!


```text
export FASTLY_API_KEY="your-fastly-api-token"
export FASTLY_TF_DISPLAY_SENSITIVE_FIELDS="true"
./fastly-terraformer
terraform init
terraform plan -generate-config-out=generated.tf
terraform apply
```


After those commands, your entire Fastly infrastructure is now under Terraform management safely, without changing anything running in production.


**Let's break down what happens under the hood:**


**Step 1** : fastly-terraformer scans your Fastly account via the API and discovers all your resources—services, backends, domains, TLS certificates, NGWAF workspaces, key-value stores, everything.


**Step 2** : It generates an` import.tf` file containing Terraform import blocks for every resource:


Copied!


```text
import {
id = "abc123def456"
to = fastly_service_vcl.my_service
}


import {
id = "workspace-123/list-456"
to = fastly_ngwaf_workspace_list.my_workspace_blocklist
}
```


**Step 3** : You run` terraform plan -generate-config-out=generated.tf` to fetch the actual resource configuration from Fastly, and Terraform writes it to` generated.tf` .


You can learn more about how the` -generate-config-out` flag works in HashiCorp's[Import Configuration Guide](https://developer.hashicorp.com/terraform/language/import/generating-configuration) .


**Step 4** :` terraform apply` imports everything. Your infrastructure is now in state.


Your Fastly config is unchanged. No downtime. No rewrites. You've just moved it from the console into version control.


### What Gets imported


fastly-terraformer covers the full spectrum of Fastly infrastructure:


-


**Compute & Services** : VCL services, Compute (WASM) applications, domains, backends, health checks, snippets, request settings, response objects.


-


**Security & WAF** : NGWAF workspaces, workspace-scoped rules and lists, account-level security signals, alert integrations (Slack, PagerDuty, Datadog, Teams, Opsgenie, Jira, webhooks, mailing lists).


-


**Storage & Logging** : Config stores, KV stores, secret stores, S3 logging, Syslog, Datadog, BigQuery, Splunk, and Papertrail endpoints.


-


**TLS & Access Control** : TLS subscriptions, certificates, configurations, ACLs, and user management.


That's 40+ resource types—essentially everything you can build in Fastly except legacy WAF (which has API limitations; use modern NGWAF instead).


Two import modes give you flexibility: import your entire account at once, or scope to just NGWAF resources if you're moving security config to code first.


## The Benefits of Using fastly-terraformer


Fastly-terraformer offers many benefits, such as:


-


**Speed.** Teams spend weeks manually mapping Fastly resources to Terraform. fastly-terraformer does it in minutes.


-


**Team consistency.** Once your infrastructure is in code, every team member sees the same source of truth. Changes go through pull requests, not the console. You get audit trails, peer review, and rollback capability.


-


**Disaster recovery.** If something breaks, your infrastructure lives in a Git repo. You can redeploy from scratch with confidence.


-


**Environment parity.** Need to spin up staging that matches production? Terraform + fastly-terraformer means you're not reverse-engineering from the console.


-


**Moat against vendor lock-in.** Code is portable. If your team ever needs to migrate off Fastly, your infrastructure definition travels with you.


Most importantly: your team stops maintaining infrastructure in two places (console + memory). One source of truth, one workflow, one set of standards.


## Getting Started


Head to the[fastly-terraformer GitHub repository](https://github.com/fastly/fastly-terraformer) . Installation guidance is in the README.


1.


Grab an API token from the Fastly console


2.


Set the environment variables


3.


Run the commands mentioned previously


The tool handles the rest—including intelligent resource naming, conflict prevention for workspace-scoped resources, and graceful error handling if something fails mid-discovery.


## The Bottom Line


Fastly is enterprise infrastructure. It deserves enterprise tooling. You wouldn't keep AWS or GCP infrastructure in the console—and you shouldn't keep Fastly there either.


fastly-terraformer removes the last barrier to IaC adoption. Your Fastly setup can move to code today, not someday. No rewrites, no downtime, no weeks of manual work.


Your config is already in Fastly. We just made it easy to version control.


**Ready to try it?** Check out[fastly-terraformer on GitHub](https://github.com/fastly/fastly-terraformer) . Have questions or want to contribute? Open a pull request to get started!
