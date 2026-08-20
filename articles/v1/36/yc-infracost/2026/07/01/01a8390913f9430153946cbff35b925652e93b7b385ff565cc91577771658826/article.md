---
schema_version: "1.0.0"
document_id: "01a8390913f9430153946cbff35b925652e93b7b385ff565cc91577771658826"
company_key: "yc-infracost"
company: "Infracost"
source_id: "yc-infracost-news-import-5e947f59e679"
canonical_url: "https://www.infracost.io/resources/blog/terraform-plan-output-support-cli-v2"
published_at: "2026-07-02T00:10:00+00:00"
first_seen_at: "2026-07-23T11:56:51.837513+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:a4ae2b1fdbea657b77d108f989bcdcdf2745814a4d1fabbce1b8a51551ee5e35"
---

# Cost your Terraform plan JSON directly

# Cost your Terraform plan JSON directly


*As of CLI v2.4.2,*` *infracost scan*` *reads the output of*` *terraform show -json*` *and turns it into a cost breakdown.*


If a Terraform plan is already part of your workflow, you can now turn it straight into a cost estimate.


Point` infracost scan` at a plan JSON file (the output of` terraform show -json` ) and it prices what's actually in the plan: the fully-resolved resources, with the values Terraform worked out at plan time. Data-source lookups, computed counts, the sizes that only settle once it runs, all of it.


## Point Infracost at the plan


As of v2.4.2:


```text
terraform plan  -out   tfplan.binary
terraform show  -json


```


```text
terraform plan  -out   tfplan.binary
terraform show  -json


```


```text
terraform plan  -out   tfplan.binary
terraform show  -json


```


That's the whole setup. Nothing new runs in your pipeline, and Infracost never touches the Terraform binary: the plan is already resolved, so the file stands on its own. It won't download modules or read your variables to price it.


Infracost recognizes the file as a plan by looking inside it, not by the filename, checking for the` format_version` and` planned_values` keys that Terraform writes. So you can call the file whatever you like.


From there, it reads the resolved resources and prices them, then applies your usual FinOps and tagging policies. Drill into any line with` infracost inspect` . OpenTofu plans work the same way.


## When this fits


The plan JSON path is built for teams whose pipeline already produces a plan, or whose costs depend on values that only resolve at plan time:


-


Your costs depend on plan-time values, like data sources or dynamic lookups. The plan has them resolved, so Infracost prices the real figures.


-


You're on Bitbucket and already generate a plan in CI you can reuse.


-


You're on HCP Terraform or Terraform Enterprise, which produces the plan JSON for you.


If you own platform, this slots Infracost into the artifact your pipeline already builds. The cost estimate a developer sees on a pull request reflects the fully-resolved plan, including the figures that depend on plan-time data, and there's no extra plan step to add, because you're reusing the one you already run.


## Try it


Upgrade to v2.4.2 or later, run those two commands, and let Infracost price what your plan is about to build. It's free to start.


**→**[Read the Terraform Plan docs](https://www.infracost.io/docs/features/terraform_plan_json/)
