---
schema_version: "1.0.0"
document_id: "660b36ed6f58da08fc5b96f1c70de7c0e8a66ef1c06c7e1c6a228e3ae7fe6d09"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/aws-secrets-manager-complete-guide"
published_at: "2026-06-02T04:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:d764e26127f2d8811ff2159f5976c895aeb4626a641f6ae71786fe9808479f26"
---

# AWS Secrets Manager: Complete Guide, Best Practices, and Tips

AWS Secrets Manager is Amazon’s native secret management solution. This makes it look like an obvious option for any company building on Amazon’s infrastructure: it directly integrates with AWS IAM and RDS, Redshift, and many other services.


Choosing the right security infrastructure is important because migrating security infrastructure is painful, so it’s worth understanding your secrets manager in depth. If your infrastructure lives on AWS, it’s worth understanding AWS Secrets Manager’s features, architecture, advantages, and shortcomings before moving forward with it.


## What AWS Secrets Manager is


AWS Secrets Manager is a managed service for storing, retrieving, rotating, and auditing secrets: database credentials, API keys, OAuth tokens, and any other secrets. This is a massive upgrade over` .env` files or any other form of manual[secrets management](https://infisical.com/blog/secrets-management-complete-guide) . Instead of hardcoding credentials where they might accidentally leak, your application retrieves them from a central secrets store and injects them at runtime. This eliminates[secret sprawl](https://infisical.com/blog/what-is-secret-sprawl) , thereby increasing security and avoiding downtimes or engineering slowdowns.


AWS Secrets Manager’s core architecture is like any other secrets management tool: It stores secrets in a database, encrypted at rest. Then it serves them to applications at runtime via an authenticated API call.


AWS Secrets Manager is different from two similar-sounding AWS services that often cause confusion.


### AWS Systems Manager Parameter Store vs. AWS Secrets Manager


AWS Systems Manager Parameter Store stores configuration values and can hold secrets. It also has affordances like access control via AWS IAM and native integrations with other parts of the AWS stack. But storing credentials in Parameter Store instead of Secrets Manager is like running Postgres on EC2 instead of RDS.


It’s possible, but you’ll lack core features that make your life easier. For instance, Parameter Store lacks built-in secret rotation (regular changes of a secret’s value to limit the blast radius of breaches). Secrets Manager is purpose-built for secrets management. Using Parameter Store for it means more manual work for a worse result.


### AWS Secrets Manager vs. AWS Key Management Service (KMS)


AWS Key Management Service (KMS) manages encryption keys, which are secrets. But Secrets Manager uses KMS under the hood rather than replacing it.


KMS is not built for secrets management and you’ll probably only interface with it if you’re building custom encryption workflows for your data. You’d only use it directly if you’re building your own secrets manager.


## How AWS Secrets Manager works


Secrets Manager’s architecture consists of three core workflows: encryption, access control, and versioning.


### Encryption with KMS


Every secret value is encrypted at rest using a key from AWS KMS. When you create a secret, you’ll usually use the AWS managed key` aws/secretsmanager` , which is free and the right default for most workloads. Secrets Manager generates a unique data key per secret version and encrypts the value with it. The data key itself is protected by your KMS key. When the secret is retrieved for a build, the value is decrypted, returned over TLS and injected into the build.


You can also use a Customer Managed Key (CMK) for encryption. This is ideal for advanced workflows, e.g. sharing a secret across AWS accounts, applying a custom key policy, or auditing encryption operations independently. In a CMK key policy, you can scope the key to Secrets Manager alone by setting the` kms:ViaService` condition to` secretsmanager.<region>.amazonaws.com` , so the key can't be used for anything else.


### Access control with AWS IAM


Access to secrets is governed by two layers: The first are identity-based IAM policies attached to users and roles. An example of this is that a` staging backend service` role gets read access to secrets under a specific path.


Another type of policy is resource-based. These are secret policies attached directly to a secret, meaning a database credential can have its own policy as to which identities get what access.


The most important practice with regards to access control is the principle of least privilege. It means that every identity should have the least necessary privileges to limit the fallout if anything goes wrong. This is a contrast to previous secrets management practices like` .env` files, which tended to give access based on what secrets are convenient to bundle.


An application role should be able to call` secretsmanager:GetSecretValue` on the specific secrets it needs, and nothing more. AWS realizes this via assigning each secret an Amazon Resource Number (ARN), a unique identifier


A tightly scoped identity policy looks like this:


```text
{
"Version": "2012-10-17",
"Statement": [
{
"Effect": "Allow",
"Action": "secretsmanager:GetSecretValue",
"Resource": "arn:aws:secretsmanager:us-east-1:111122223333:secret:prod/db/credentials-??????"
}
]
}


```


The trailing question marks match the random six-character suffix AWS appends to every secret ARN, so the policy keeps working across secret recreation without granting access to other secrets that share a name prefix.


### Versioning and staging labels


A secret in Secrets Manager isn't a single value, it's a series of versions, each tracked with staging labels. The label` AWSCURRENT` points at the version your applications retrieve by default. During rotation, a new version is created and labeled` AWSPENDING` , tested, and only then promoted to` AWSCURRENT` .


The previous version is relabeled` AWSPREVIOUS` , which gives you a one-step rollback path if a rotation goes wrong. Understanding these labels is the key to understanding rotation, which comes later in this guide.


## Getting started in the console


The fastest way to create your first secret is the console. Open Secrets Manager, choose **Store a new secret** , and pick a secret type. For Amazon RDS, Aurora, Redshift, or DocumentDB credentials, the dedicated types wire up the database reference for you. For everything else, choose **Other type of secret** and enter key-value pairs or raw JSON.


Choose your encryption key (` aws/secretsmanager` unless you have a reason not to), give the secret a name that reflects a clear hierarchy such as` prod/payments/db-credentials` , and store it. Naming convention matters more than it looks: secret names are unique within an account and region, and a consistent path-style scheme makes IAM policies, search, and auditing dramatically easier down the line.


The console is fine for a first secret and for occasional inspection. For anything repeatable, move to the CLI or Terraform.


## Working with the AWS CLI


The CLI is where day-to-day secret management happens. Creating a secret takes one command:


```text
aws secretsmanager create-secret \
--name prod/payments/db-credentials \
--description "Payments service database credentials" \
--secret-string '{"username":"payments_app","password":"REPLACE_ME"}'


```


Retrieving it is equally direct:


```text
aws secretsmanager get-secret-value \
--secret-id prod/payments/db-credentials \
--query SecretString \
--output text


```


Updating a secret value creates a new version and moves` AWSCURRENT` to it automatically:


```text
aws secretsmanager put-secret-value \
--secret-id prod/payments/db-credentials \
--secret-string '{"username":"payments_app","password":"NEW_VALUE"}'


```


One caution worth internalizing: any secret value passed on the command line can land in your shell history and, in some configurations, in process listings or CloudTrail. For sensitive values, read from a file with` --secret-string file://creds.json` rather than typing the value inline, and clear the file afterward. AWS calls this out directly as a CLI risk to mitigate.


Listing and inspecting secrets rounds out the basics:


```text
aws secretsmanager list-secrets --query "SecretList[].Name"
aws secretsmanager describe-secret --secret-id prod/payments/db-credentials


```


` describe-secret` returns metadata, rotation configuration, and version labels without ever exposing the secret value, which makes it safe to use in automation and dashboards.


## Managing secrets with Terraform in AWS Secrets Manager


[Terraform secrets management](http://infisical.com/blog/how-to-manage-secrets-on-terraform-using-infisical) is a bit different. But if you run infrastructure as code, Secrets Manager fits cleanly into Terraform. The` aws_secretsmanager_secret` resource defines the secret container, and` aws_secretsmanager_secret_version` holds the value:


```text
resource "aws_secretsmanager_secret" "db_credentials" {
name        = "prod/payments/db-credentials"
description = "Payments service database credentials"
kms_key_id  = aws_kms_key.secrets.id
}


resource "aws_secretsmanager_secret_version" "db_credentials" {
secret_id = aws_secretsmanager_secret.db_credentials.id
secret_string = jsonencode({
username = "payments_app"
password = var.db_password
})
}


```


There's a real caveat here that every team hits eventually: anything Terraform manages goes into Terraform state, and state stores values in plaintext.


If you put a literal password in your configuration, it lives in state and in any plan output. The common patterns to avoid that are to source the value from a variable marked` sensitive = true` , to let the database resource generate the password and feed it in by reference, or to create the empty secret with Terraform and write the actual value through a separate process so it never enters state at all. Treat your state backend (typically an encrypted S3 bucket with locking) as a secret store in its own right.


To consume an existing secret elsewhere in your configuration, the` aws_secretsmanager_secret_version` data source reads the current value at plan time:


```text
data "aws_secretsmanager_secret_version" "db" {
secret_id = "prod/payments/db-credentials"
}


```


The same plaintext-in-state consideration applies to data sources, so reference them deliberately.


## Rotating secrets


Rotation is the feature that distinguishes Secrets Manager from a simple encrypted store, and it's the one most worth understanding properly. Secret rotations are automatic, scheduled changes of each secret. This is a best practice done to invalidate any credentials that may inadvertently have leaked or may leak in the future.


Secrets Manager rotation runs on an AWS Lambda function. When a rotation triggers, Secrets Manager invokes that function four times, once for each step:


```text
createSecret   → generate a new credential, store it as AWSPENDING
setSecret      → apply the new credential to the database or service
testSecret     → verify the AWSPENDING credential actually works
finishSecret   → move the AWSCURRENT label to the new version


```


Only after` finishSecret` completes do applications start receiving the new value, because they retrieve` AWSCURRENT` by default. If any step fails, the old credential keeps serving traffic and you can investigate without an outage.


There are two rotation strategies, and the choice has real operational consequences:


**Single-user rotation** updates the password for one database user in place. It's simpler to set up, but it creates a brief window during rotation where in-flight connections using the old password can fail. It suits workloads that can tolerate a momentary blip or that reconnect cleanly.


**Alternating-users rotation** maintains two users and switches between them. While one user's credentials are live and serving traffic, the other is rotated in the background, then the` AWSCURRENT` label flips to it. This buys effectively zero-downtime rotation and is the better choice for high-availability services. It requires a second admin (superuser) secret with permission to manage the rotating users.


For Amazon RDS, Aurora, Redshift, and DocumentDB, AWS provides ready-made rotation function templates, so you don't write the Lambda yourself. For other databases or custom services, AWS publishes a[generic rotation template](https://github.com/aws-samples/aws-secrets-manager-rotation-lambdas) you customize to your system. Secrets Manager supports rotation as often as every four hours at no extra charge beyond the underlying Lambda invocations.


Turning on rotation from the CLI looks like this:


```text
aws secretsmanager rotate-secret \
--secret-id prod/payments/db-credentials \
--rotation-lambda-arn arn:aws:lambda:us-east-1:111122223333:function:rotate-payments-db \
--rotation-rules '{"ScheduleExpression":"rate(30 days)"}'


```


All of this functionality ties into other AWS resources and services via native integrations. That’s exactly why many teams on AWS evaluate it first. But like every other tool, AWS Secrets Manager has upsides and downsides and isn’t ideal for every use case (even if you heavily rely on AWS).


## AWS Secrets Manager pros and cons


### Pros


- Integrates directly with IAM. No separate authentication layer needed, the same roles and policies your infrastructure already uses govern secret access.
- Rotation is built in for RDS, Aurora, Redshift, and DocumentDB, covering the credentials that matter most for most teams.
- Fully managed: no servers to run, no storage to provision, no key rotation to orchestrate manually.
- For teams already inside the AWS ecosystem (and without plans to branch out), it can be convenient to stay within that ecosystem.


### Downsides


- No native answer for workloads outside AWS. Multi-cloud teams end up managing parallel stores or building glue between them.
- The per-secret-per-region pricing model that's negligible at small scale becomes meaningful once secrets multiply across environments and regions.
- No built-in approval flows for accessing production credentials, no change management, and no unified audit view across teams.
- Rotation covers AWS-managed databases well but requires custom Lambda functions for anything else.


## Cost and how to control it


Secrets Manager pricing is simple, but easy to underestimate. The list price is $0.40 per secret per month, plus $0.05 per 10,000 API calls. That pricing is identical across all 36 AWS regions, which is a rare exception among AWS services and means you can pick a region for latency and compliance rather than cost.


For a small team with a few dozen secrets, the bill is often a few dollars a month, which makes it a negligible line item. But as organizations grow, teams often miscalculate in a few ways, which causes large AWS Secrets Manager bills:


**Environment and region multiplication** One hundred production secrets become four hundred once you replicate them across production, staging, QA, and development. Now multiply that by each region and any scaling company’s Secrets Manager costs increase rapidly.


**API call volume** Many teams underestimate how often secrets are fetched. If a database credential is fetched on every request across an auto-scaling fleet of containers, it can generate millions of calls.


One of the issues with AWS Secrets Manager’s pricing policy is that it disincentivizes good security posture. Best practices like dynamic secrets (short-lived credentials created just-in-time) and frequent rotations are more important than long-lived, static credentials.


## AWS Secrets Manager best practices


Most of what makes Secrets Manager safe and maintainable comes down to a handful of disciplines:


**Centralize secrets in Secrets Manager.** For[secrets management](https://infisical.com/blog/secrets-management-complete-guide) , it’s important that everything is in one place. All secrets should be managed in Secrets Manager to give DevOps or platform teams certainty and full auditability. It makes incident response easier and limits the blast radius of any potential breach.


**Use IAM roles for AWS-to-AWS authentication** so there's no bootstrap secret to manage in the first place. To catch the mistakes that slip through, pair Secrets Manager with[secrets scanning](https://infisical.com/docs/documentation/platform/secret-scanning/overview) across your repositories so a leaked credential is detected before it becomes an incident.


**Scope access tightly with IAM.** Grant` GetSecretValue` on specific secret ARNs, not wildcards. Use resource-based policies to add a second layer of control, and block broad access with` BlockPublicPolicy` on` PutResourcePolicy` calls.


**Turn on rotation wherever the downstream system supports it.** A rotated credential is a time-bound credential. Choose alternating-users rotation for anything that can't tolerate a connection blip.


**Audit everything with CloudTrail.** Every Secrets Manager API call is logged. CloudTrail gives you the record of who retrieved which secret and when, which is both an operational tool and a compliance requirement under frameworks like SOC 2 and PCI DSS.


**Cache reads and right-size what lives in Secrets Manager.** Caching protects both your latency and your bill. Keeping static config out of Secrets Manager keeps the service focused on actual secrets.


**Use a clear naming hierarchy.** A scheme like` <env>/<service>/<purpose>` makes policies, search, and reasoning about access scale with your secret count instead of fighting it.


## When AWS Secrets Manager isn't enough


AWS Secrets Manager is an excellent default when your world is a single AWS account. It's deeply integrated, managed, and works well for AWS-native workloads. Plenty of teams simply use it and move on.


But AWS Secrets Manager quickly runs into constraints. As an AWS-exclusive product, it will not support multi-cloud and hybrid infrastructure. The moment you're also running workloads on Azure, GCP, on-premises, and across SaaS platforms, you need to use separate solutions. This negates the benefits of centralization in each environment and requires your organization to build brittle workarounds to bridge the gaps between parallel secrets infrastructure. Many teams start to evaluate[alternatives to AWS Secrets Manager](https://infisical.com/blog/aws-secrets-manager-alternatives) once they run workloads beyond AWS.


The second is workflow depth. Secrets Manager stores and rotates secrets well, but it offers little around the human processes: approval workflows for accessing credentials, granular change management, a unified audit view across teams, or dynamic short-lived secrets generated on demand. Teams often end up building these themselves, which creates dependencies and becomes an internal tool to maintain.


The third is cost behavior at scale. What seems like a trivial cost when you’re small becomes a line item worth questioning once secrets multiply into the thousands and your infrastructure requires millions of API calls.


### Why some teams use Infisical over AWS Secrets Manager


This is the point where a dedicated identity and secrets platform earns its place, not as a replacement for AWS so much as a layer that unifies it with everything else.[Infisical](https://infisical.com/) offers a secrets management platform that works with any kind of infrastructure and offers many of the workflows and DevEx upgrades AWS Secrets Manager lacks. It's built on Postgres rather than proprietary infrastructure, runs in the cloud or fully self-hosted, and gives developers a usable dashboard, approval workflows, and dynamic secrets out of the box.


Adopting it doesn't mean a disruptive migration. Infisical's[AWS Secrets Manager sync](https://infisical.com/docs/integrations/secret-syncs/aws-secrets-manager) connects directly to your existing setup: you can import the secrets you already have, keep them synced, and adopt centralized workflows without ripping anything out. This is exactly[what Infisical customer Teamworks did](https://infisical.com/customers/teamworks) : AWS Secrets Manager remains a core part of their workflow, but Infisical became the layer that governs secrets consistently after they outgrew a single-cloud infrastructure. For a wider survey of the options, our roundup of the[best secrets management tools](https://infisical.com/blog/best-secret-management-tools) puts the landscape side by side.
