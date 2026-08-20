---
schema_version: "1.0.0"
document_id: "a312cb1b72f6abdf066d2083730813b42904294a94e9846d7da3709d3c164f9a"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/terraform-secrets-management"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:81a629f02eb2598b230368611026699be0ceceea56db9a6612cc0a2207c9772e"
---

# A Guide to Terraform Secrets Management: Keeping Secrets Secure in Infrastructure as Code

Terraform is an excellent tool. Instead of clicking through consoles and running one-off setup scripts, you describe your whole infrastructure in code: three app servers, a Postgres database, a bucket for uploads. Terraform reads that, checks what already exists, and creates, updates, or deletes whatever it takes to make reality match.


To do that, it needs access to your infrastructure. So you hand it credentials: a cloud key to create the servers and the bucket, and an admin password to set up that Postgres database.


And then it puts those secrets in plaintext on disk, in a Terraform state file.


That's a problem twice over. State files travel: they get copied to laptops, shared through remote backends, and stored as CI artifacts, and every copy carries the secret. And when a secret has to change, every file holding a copy has to change with it.


Keeping secrets safe with Terraform means solving one specific problem: giving it the access it needs without any secret being stored in plaintext along the way. The way to do that is to keep the real values in a dedicated[secrets management](https://infisical.com/blog/secrets-management-complete-guide) platform, hand Terraform references instead of payloads, and lean on the newer mechanisms that let a value pass through a run without ever being written to state.


There's setup involved, but it pays for itself: once the secrets manager is wired in, adding a secret is one write, rotating it is one change, and nothing needs to be scrubbed out of Git or state afterwards.


## What is the problem with Terraform secrets?


Any secret Terraform handles, whether it's a credential you pass in or a value Terraform generates, gets written into its state file in plaintext. Anyone who can read that file can read the secret.


Let’s see this happen. Here's a configuration that doesn't touch a cloud provider, a database, or any real secret at all. It just generates a random password and marks the output sensitive:


```text
terraform {
required_version = ">= 1.10"
required_providers {
random = {
source  = "hashicorp/random"
version = "~> 3.6"
}
}
}


resource "random_password" "db" {
length  = 20
special = true
}


output "db_password" {
value     = random_password.db.result
sensitive = true
}


```


Apply it, and Terraform does exactly what you'd hope. The value is redacted everywhere it would normally print:


```text
$ terraform apply -auto-approve
random_password.db: Creating...
random_password.db: Creation complete after 0s [id=none]


Apply complete! Resources: 1 added, 0 changed, 0 destroyed.


Outputs:


db_password = <sensitive>


```


So far, so safe.` <sensitive>` in the plan,` <sensitive>` in the outputs. But if we` grep` the file Terraform just wrote to the current directory, we’ll see something worrying:


```text
$ grep -n '"result":' terraform.tfstate
35:            "result": "YBG$nq2(UQjIBVSyekYB",


```


There it is. The password Terraform was so careful not to print is sitting in` terraform.tfstate` in plaintext. And not just once:


```text
$ grep -n -A1 '"db_password"' terraform.tfstate
7:    "db_password": {
8-      "value": "YBG$nq2(UQjIBVSyekYB",


```


The resource attribute is stored, the output is stored, and the state carries a bcrypt hash of the same password for good measure. The` sensitive = true` flag controlled how the value was displayed, but it did nothing about how it was stored.


This is the whole problem in miniature. State is plaintext JSON by default. Every attribute of every resource goes into it, and Terraform needs it that way: state is how it knows what already exists so that it can compute the next diff. The leak is a side effect of Terraform working correctly.


### How do secrets end up in state files?


There are a few ways a secret gets into state:


- **Values you pass in** . A password argument on a resource, or a provider access_key.
- **Values Terraform generates** . The` random_password` above is the clearest case: Terraform created the secret, so of course it stores it.
- **Values you read** . Data sources fetch information and record it in state so future plans are stable. If that information is a secret, it's now in state too.
- **Outputs** . Outputs are stored in state as well, which is why an output derived from a secret is itself a secret at rest.


Notice that` sensitive = true` doesn't appear on that list, because it doesn't change any of it.


Is this all a big deal? Yes. State files get around. They land in a shared remote backend, get copied to a laptop for a quick` terraform state` command, show up as a CI artifact, or get committed by someone who didn't realize what was in them. Every one of those is a copy of your secret, which is how[secret sprawl](https://infisical.com/blog/what-is-secret-sprawl) starts.


So, what do you need to do? Keep real secret values out of state. And that means both kinds of secret Terraform touches: the ones it manages, like the database passwords and API keys it sets on your infrastructure, and the credentials it uses, the keys that let Terraform itself call your cloud and your secrets manager.


## Pulling secrets in at plan/apply time


We used` random_password` in the first example because it kept the demo self-contained: Terraform generated the secret, so nothing real was needed. Real projects usually work the other way round. The database password or API key already exists, and it gets written straight into the configuration:


```text
locals {
db_user     = "app_user"
db_password = "correct-horse-battery-staple"   # in every clone, and in Git history
}


```


This is the pattern to kill. The password sits in your repository, so everyone with read access to the repo has it, and it survives in Git history even after you delete the line. Rotating it means a code change.


The fix is to move the value into Infisical, an open-source secrets manager, and have Terraform fetch it when it runs. Infisical stores secrets as key-value pairs, organized by project, environment (dev, staging, prod), and folder path, and everything that reads them, whether a person or a machine, authenticates with its own identity and permissions. For this run, that means the two values above are saved in the Infisical project as secrets named` DB_USER` and` DB_PASSWORD` in a folder called` /database` and are deleted from the code.


You authenticate the[Infisical provider](https://registry.terraform.io/providers/Infisical/infisical/latest) with a[machine identity](https://infisical.com/docs/documentation/platform/identities/machine-identities) , then read secrets with the` infisical_secrets`[data source](https://registry.terraform.io/providers/Infisical/infisical/latest/docs/data-sources/secrets) :


```text
terraform {
required_providers {
infisical = {
source  = "infisical/infisical"
version = "~> 0.16"
}
}
}


provider "infisical" {
# host defaults to https://app.infisical.com; set it for self-hosted.
auth = {
universal = {
# Both variables default to null, in which case the provider falls back
# to the INFISICAL_UNIVERSAL_AUTH_* environment variables shown below.
client_id     = var.infisical_client_id
client_secret = var.infisical_client_secret
}
}
}


data "infisical_secrets" "backend" {
env_slug     = "dev"
workspace_id = var.infisical_workspace_id
folder_path  = "/database"
}


locals {
db_user     = data.infisical_secrets.backend.secrets["DB_USER"].value
db_password = data.infisical_secrets.backend.secrets["DB_PASSWORD"].value
}


# An output derived from a secret must itself be sensitive, or the apply errors.
output "db_user" {
value     = local.db_user
sensitive = true
}


```


You have to set up[universal auth](https://infisical.com/docs/documentation/platform/identities/universal-auth) . With that in place, the run is uneventful in the right way:


```text
$ terraform apply -auto-approve -var infisical_workspace_id=<project-id>
data.infisical_secrets.backend: Reading...
data.infisical_secrets.backend: Read complete after 0s


Changes to Outputs:
+ db_user = (sensitive value)


Apply complete! Resources: 0 added, 0 changed, 0 destroyed.


Outputs:


db_user = <sensitive>


```


The` Reading...` and` Read complete` lines are the fetch: Terraform called Infisical's API mid-run and pulled the values down. Nothing was built, hence` 0 added, 0 changed, 0 destroyed` . It is a data source, so it only reads. The same two locals exist as before, and the output prints as` <sensitive>` . What changed is where the values live.


Here's what this step buys, and what it doesn't:


Hardcoded Data source


Where the secret lives Your repository Infisical, behind access control, an audit log, and rotation


What's in Git The value itself Coordinates only: project, environment, folder path, key name


How rotation works Edit code, commit, redeploy Rotate in Infisical, then the next plan picks it up


What's in state The value Still the value


That last row is the catch. A data source records what it read into state, so after this run, the state file holds both values in plaintext, exactly like the first example did. About 2 KB of state, with the passwords under` resources` and the output sitting beside its` "sensitive": true` flag.


You've solved secrets-in-code: the value lives in one place, and changing it means changing it in Infisical, with nothing to edit in your repository. You haven't yet solved secrets-in-state. For that, either harden the backend that stores your state or keep the value entirely out of state with an ephemeral resource.


## Keeping secrets out of state with ephemeral resources


Terraform 1.10 introduced[ephemeral resources](https://infisical.com/blog/terraform-ephemeral-resources) : values that are fetched during a run and deliberately *never* written to state or to the plan file. This is the fix for that last table row.


The setup in Infisical changes shape for this one: a single secret called` DB_CREDENTIALS` , saved at the project root, whose value is a JSON object with` host` ,` username` , and` password` keys. The provider reads it through an` ephemeral` block:


```text
ephemeral "infisical_secret" "db_creds" {
name         = "DB_CREDENTIALS"
env_slug     = "dev"
workspace_id = var.infisical_workspace_id
folder_path  = "/"
}


locals {
credentials = jsondecode(ephemeral.infisical_secret.db_creds.value)
}


provider "postgresql" {
host     = local.credentials["host"]
username = local.credentials["username"]
password = local.credentials["password"]
}


```


Put this next to the data source config, and the working difference is the block type:` ephemeral` where data was, plus the singular` infisical_secret` , which reads one named secret instead of a folder.` jsondecode` then unpacks the JSON value into a local.


The` postgresql` provider is where those credentials land. Nothing in this config creates a database resource, so the apply shows no postgres activity. What it shows instead is the ephemeral lifecycle:


```text
$ terraform apply -var infisical_workspace_id=<project-id>
ephemeral.infisical_secret.db_creds: Opening...
ephemeral.infisical_secret.db_creds: Opening complete after 0s
ephemeral.infisical_secret.db_creds: Closing...
ephemeral.infisical_secret.db_creds: Closing complete after 0s


Apply complete! Resources: 0 added, 0 changed, 0 destroyed.


```


Those` Opening...` and` Closing...` lines are the ephemeral lifecycle: fetched for the run, explicitly discarded at the end of it. Compare the data source run, which said` Reading...` and kept what it read. This fetch has an end.


Then the part that matters. Grep the state file the way we did before:


```text
$ grep -i password terraform.tfstate
$ grep -iE 'credential|db_creds' terraform.tfstate
$


```


Nothing. The entire state file is 181 bytes:


```text
$ cat terraform.tfstate
{
"version": 4,
"terraform_version": "1.10.5",
"serial": 1,
"lineage": "565cc4d1-3241-d9aa-0f87-c89437862f48",
"outputs": {},
"resources": [],
"check_results": null
}


```


Real credentials were fetched from the same project as the data-source run, decoded with` jsondecode` , and made available to configure a provider. State has no record of any of it happening. The data-source version of this left 2 KB of state with both values, but this one leaves nothing behind.


Ephemeral values are intentionally constrained: they can only flow into other ephemeral contexts, like provider configuration, other ephemeral resources, or write-only arguments. That restriction is the point. It's what lets Terraform guarantee the value never lands somewhere persistent. If you try to route an ephemeral value into a normal resource attribute or a regular output, Terraform stops you.


Those write-only arguments also close the second route from the earlier list. The random provider ships an ephemeral variant of random_password, and a growing number of resources accept write-only attributes that consume a value without storing it:


```text
ephemeral "random_password" "db" {
length = 24
}


resource "aws_db_instance" "app" {
# ...
password_wo         = ephemeral.random_password.db.result
password_wo_version = 1
}


```


The password is generated ephemerally, handed to the write-only` password_wo` argument, and never lands in state at either end.


Between the data source and the ephemeral resource, the rule of thumb is simple: if a value only needs to exist for the duration of the run, to configure a provider, or to authenticate, make it ephemeral. Reach for the data source only when you genuinely need the value materialized, and encrypt your state when you do.


## Marking variables and outputs as sensitive


We've been using` sensitive = true` since the first example, but what exactly does it do? It does one thing: redact the value from Terraform's display. It applies to both variables and outputs:


```text
variable "db_admin_password" {
type      = string
sensitive = true
}


# From the data source example
output "db_user" {
value     = local.db_user
sensitive = true
}


```


A sensitive variable renders as` (sensitive value)` wherever it flows in a plan, and a sensitive output prints as` <sensitive>` . That's a real benefit, keeping secrets off screens and out of CI logs. Sometimes it's not optional. An output derived from a sensitive value won't apply unless the output is also marked sensitive, which is why the data source example marks` db_user` .


This has two limits:


- **It doesn't resist a request** . Run` terraform output db_password` against the first example, and it prints the value on demand.
- **It doesn't touch storage** . The grep found the value in` terraform.tfstate` in plaintext, because` sensitive` neither encrypts state nor keeps anything out of it.


Treat it as display hygiene that you always turn on, layered on top of the real protections, never as a substitute for them.


## Handling provider credentials safely


Everything so far has covered the first kind of secret, the ones Terraform manages. Provider credentials are the second kind, and they're the highest-value secrets in the setup, because whoever holds them can usually reach everything the other secrets protect.


The rule from the hardcoded example applies here too. These are the two variables it reads:


```text
export INFISICAL_UNIVERSAL_AUTH_CLIENT_ID=...
export INFISICAL_UNIVERSAL_AUTH_CLIENT_SECRET=...


```


Exported variables are fine on a laptop. In CI, they become a problem again: the client secret has to be stored somewhere for the job to read, and a secret stored in your CI platform is one more standing credential to protect.


The fix is to not have one. GitHub Actions can mint a short-lived OIDC token for each job, and an Infisical machine identity configured for[OIDC auth](https://infisical.com/docs/documentation/platform/identities/oidc-auth/github) accepts that token as proof of who's asking. Infisical's[secrets action](https://github.com/Infisical/secrets-action) handles the exchange, the same pattern covered in[How to Manage Secrets in CI/CD Pipelines](https://infisical.com/blog/secrets-management-cicd) :


```text
name: terraform-plan


on:
pull_request:


permissions:
id-token: write   # required for OIDC
contents: read


jobs:
plan:
runs-on: ubuntu-latest
steps:
- uses: actions/checkout@v4


- name: Fetch secrets from Infisical
uses: Infisical/secrets-action@v1.0.9
with:
method: "oidc"
identity-id: "your-machine-identity-id"  # public identifier, safe to commit
project-slug: "your-project-slug"
env-slug: "prod"


- run: terraform init
- run: terraform plan


```


Read the workflow as a sequence:


1. The job starts, and GitHub mints it a[short-lived OIDC token](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect) , authorized by the` id-token: write scope` .
2. The action presents that token to Infisical.
3. Infisical checks its claims against the identity's binding, a subject like` repo:your-org/your-repo:ref:refs/heads/main` , so only that repository and branch can authenticate.
4. It hands back short-lived access, and the job's allowed secrets are injected as environment variables that disappear when the job ends.


There's no stored secret on either side. The` identity-id` in the workflow is a public identifier, safe to commit, because knowing it grants nothing without a token that matches the binding.


The Terraform tie-in is the` TF_VAR_` convention. Store a secret in Infisical as` TF_VAR_db_admin_password,` and the plan step reads it as` var.db_admin_password` , with nothing passed on the command line.


Terraform usually needs cloud credentials in CI too, and the same token covers it. A job's OIDC token can be exchanged for a scoped AWS role through` aws-actions/configure-aws-credentials` , so there's no long-lived cloud key in GitHub either.


The principle is the same everywhere: the credential should reach Terraform from the environment it runs in, never from the code it runs.


## Hardening the state backend


State records every attribute of every resource, secret or not. Some secret values end up there even when you're doing things right: a data source read you chose because the value needs to persist, or a resource that returns a credential when Terraform creates it, an IAM access key, for example. Ephemeral resources don't cover every case yet.


When a secret has to be in the file, protect the file. That starts with where it lives.


Every run so far wrote state to a local` terraform.tfstate` . That's the file the greps were reading. Teams keep state in a remote backend instead, usually a bucket, and the bucket is what you harden. Four properties matter: encrypted, versioned, locked, and readable only by the identities that run Terraform.


On AWS, that's S3 with a KMS key:


```text
terraform {
backend "s3" {
bucket       = "org-terraform-state"
key          = "network/prod.tfstate"
region       = "us-east-1"
use_lockfile = true   # locking: two applies can't run at once
encrypt      = true   # encrypted at rest
kms_key_id   = "arn:aws:kms:us-east-1:123456789012:key/abcd-1234"  # with your key, not the S3 default
}
}


```


The block handles encryption and locking. The other two live on the bucket: enable[S3 Versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) so an overwritten or corrupted state can be recovered, and scope the bucket's IAM so only the identities that run Terraform can read it.


On other clouds, the GCS backend uses a Cloud KMS key via` kms_encryption_key,` with Object Versioning enabled on the bucket, and Azure Blob authenticates with Entra ID via` use_oidc` .


The provider-credentials rule also applies to the backend with greater force: backend configuration is copied into` .terraform/` and captured in plan files. Backend credentials come from the environment or the CI flow above, never inline and never as` -backend-config` values.


With that in place, the data source trade reads differently. The values still land in state, but state is now ciphertext in a locked, versioned bucket that only your run identities can open.


## Managing secrets inside reusable modules


Modules are how Terraform code gets reused: you write the database setup once, and every team and environment calls that module with their own inputs. The reuse is the point, and it's also what spreads secrets around, because a module that needs a credential has to get it from somewhere. The obvious route is an input variable:


```text
module "service" {
source      = "./modules/service"
db_password = var.db_password  # every caller has to hold and pass the raw value
}


```


This multiplies the handling problem. Every configuration that uses the module has to get hold of the password and pass it in, and the value gets recorded in that configuration's state once the module uses it, so one secret ends up copied into every stack that calls the module.


The fix is to pass coordinates instead and let the module fetch its own value:


```text
module "service" {
source       = "./modules/service"
workspace_id = var.infisical_workspace_id
env_slug     = "prod"
secret_path  = "/services/api"
}


```


Inside the module, the same` infisical_secrets` data source from earlier reads the secret at that path, or an ephemeral block does it without touching state. Callers hand over a location. The value only exists where it's used, and access is controlled in Infisical rather than by who has the variable.


## Beyond static secrets: dynamic secrets and rotation


Everything so far protects a static secret: a single value that remains valid until someone changes it. Protecting it well still leaves it worth stealing for as long as it lives. There are two ways to shrink that window, covered in more depth in[Secrets Automation: Rotated vs. Dynamic Secrets](https://infisical.com/blog/secret-rotation-vs-dynamic-secrets) . Dynamic secrets are created on demand and expire automatically. Rotation keeps a standing credential but replaces it on a schedule.


### Dynamic secrets


A[dynamic secret](https://infisical.com/docs/documentation/platform/dynamic-secrets/overview) is generated on demand and expires according to the schedule you set. Rather than keeping a single database password, Infisical creates a new database user each time one is needed and drops it when its lease is up (Dynamic secrets are an[Enterprise feature](https://infisical.com/pricing) ).


There is one dynamic secret resource per backend (e.g.,` infisical_dynamic_secret_sql_database` for SQL databases), with variants for AWS IAM, Kubernetes, and MongoDB.


For Infisical to create and delete database users on demand, it has to log in to the database itself using an account that can manage users. The data source supplies that login. It reads the admin credentials from the DB_CREDENTIALS secret used earlier, so the password never appears in this file. The resource holds the instructions: the connection details, the SQL to run when creating a user, and the SQL to run when removing one:


```text
data "infisical_secrets" "db" {
env_slug     = "dev"
workspace_id = var.infisical_workspace_id
folder_path  = "/"
}


locals {
creds = jsondecode(data.infisical_secrets.db.secrets["DB_CREDENTIALS"].value)
# Role-management DDL needs the direct endpoint, not a connection pooler.
db_host = replace(local.creds.host, "-pooler", "")
}


resource "infisical_dynamic_secret_sql_database" "db" {
name             = "postgres-dynamic"
project_slug     = var.infisical_project_slug   # slug, not ID
environment_slug = "dev"
path             = "/"
default_ttl      = "1h"
max_ttl          = "4h"


configuration = {
client   = "postgres"
host     = local.db_host
port     = "5432"
database = local.creds.database
username = local.creds.username   # privileged account that creates users
password = local.creds.password


creation_statement = <<-EOT
CREATE USER "{{username}}" WITH ENCRYPTED PASSWORD '{{password}}' VALID UNTIL '{{expiration}}';
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO "{{username}}";
EOT


revocation_statement = <<-EOT
REVOKE ALL PRIVILEGES ON ALL TABLES IN SCHEMA public FROM "{{username}}";
DROP ROLE "{{username}}";
EOT
}
}


```


Applying this stores the instructions in Infisical. It doesn't create a credential. Getting one is a separate step, generating a lease:


1. In the dashboard, find the dynamic secret and click Generate.
2. Set a TTL for the lease, anything up to the max_ttl from the config.


Infisical connects to the database, runs the` creation_statement` , and returns a new username and password with an expiry. When the lease expires or is deleted, the` revocation_statement` runs, and the user is gone. You can see this on the database side: a row appears in` pg_roles` for each lease and is removed upon expiry.


The credentials applications actually use are generated per lease and revoked automatically. A leaked one is worthless within hours, and there's no long-lived password in storage for an attacker to find.


### Secret rotation


Some systems can't work with short-lived users and require a single standing login. For those, the fallback is[rotation](https://infisical.com/docs/documentation/platform/secret-rotation/overview) : the credential stays, but its password gets replaced on a schedule, so a stolen one stops working at the next rotation. Rotation is included in the Pro tier.


Infisical does this with two database users instead of one. At any moment, one of them is the live login. When it's time to rotate, Infisical sets a new password on the idle user and switches the live secrets over to it. Nothing breaks during the swap because the previous login remains valid while consumers migrate.


Two things have to exist before the apply:


- The two database users. Create them with throwaway passwords, and Infisical replaces those the moment rotation starts.
- An[App Connection](https://infisical.com/docs/integrations/app-connections/overview) : Infisical's stored access to your database, the same access the dynamic secret needed, this time saved once under organization settings and passed in as` connection_id` .


The resource names those two users, and the two secret keys Infisical keeps updated with whichever login is currently live:


```text
resource "infisical_secret_rotation_postgres_credentials" "db" {
name          = "postgres-rotation"
project_id    = var.infisical_project_id   # ID, not slug
environment   = "dev"
secret_path   = "/database"
connection_id = var.postgres_connection_id # the App Connection, which must exist first


parameters = {
username1 = "infisical_user_1"   # rotation alternates between these two roles
username2 = "infisical_user_2"
}


secrets_mapping = {
username = "POSTGRES_DB_USERNAME"  # consumers always read these keys
password = "POSTGRES_DB_PASSWORD"
}


auto_rotation_enabled = true
rotation_interval     = 30  # days
}


```


Apply it, passing the project ID and the App Connection ID:


```text
$ terraform apply -auto-approve \
-var infisical_project_id=<project-id> \
-var postgres_connection_id=<app-connection-id>


Plan: 1 to add, 0 to change, 0 to destroy.
infisical_secret_rotation_postgres_credentials.db: Creating...
infisical_secret_rotation_postgres_credentials.db: Creation complete after 1s [id=e19d74b0-0be6-432f-bf7d-65b91a571467]


Apply complete! Resources: 1 added, 0 changed, 0 destroyed.


```


Rotation fired immediately on creation. The mapped secret names appeared in /database alongside the existing ones:


```text
DB_PASSWORD
DB_USER
POSTGRES_DB_PASSWORD    <- new, written by rotation
POSTGRES_DB_USERNAME    <- new, written by rotation


```


To verify that the rotation worked, we read the two new secrets back and used them to log in to the database. The login worked as` infisical_user_1` , with a 48-character password Infisical had generated. The throwaway password we created for the user had already been replaced. From here on, Infisical sets the passwords, and no person needs to know them.


Applications never see the swap. They read POSTGRES_DB_USERNAME and POSTGRES_DB_PASSWORD, get whichever login is live, and nothing redeploys when the password changes.


## Anti-patterns worth catching in review


Most Terraform secret leaks trace back to a few repeated habits, and they're easy to spot in code review once you've named them:


Anti-pattern Why it fails Better pattern


` sensitive = true` as the only control Redacts display, but the value still lands in state and plans Ephemeral resources, or retrieval outside Terraform


Reading secret values through data sources The payload is written into state Ephemeral resources, or a hardened backend when you can't read values


Static, long-lived CI credentials Standing blast radius and painful rotation OIDC and short-lived identity


Unencrypted, unversioned, or unlocked state Plaintext at rest, weak recovery, race conditions KMS encryption, versioning, locking, least-privilege access


## Choosing a Terraform secrets manager


Once real values leave your configuration, they need somewhere to live. The realistic choice is between a secrets service that Terraform talks to at runtime and encrypted files that travel with your repo. Cloud KMS sits underneath both, encrypting state and the stores themselves, but key management alone gives you no access control, audit trail, or rotation for individual secrets.


What to weigh for Terraform specifically:


- **An official provider on the registry** , so the secrets platform itself is managed as code alongside everything else.
- **Ephemeral reads** , so fetching a secret at plan time doesn't write it into state.
- **Dynamic secrets and rotation** , so fewer standing credentials exist in the first place.
- **Hosting and license terms** , if self-hosting or open source is a constraint for you.


Here are some options for tooling for this problem:


Option What it is With Terraform Worth knowing


Infisical Open-source secrets platform, cloud, or self-hosted Official provider. Ephemeral secret reads, dynamic secrets, rotation, and syncs are managed as code Covers static storage, dynamic credentials, rotation, and outward syncs in one platform


HashiCorp Vault The long-standing enterprise incumbent, self-managed or HCP Official provider with ephemeral resources Operationally heavy to run yourself. BUSL-licensed since 2023 and now part of IBM


Cloud secret managers Each cloud's native store ([AWS Secrets Manager](https://infisical.com/blog/aws-secrets-manager-complete-guide) ,[Azure Key Vault](https://infisical.com/blog/azure-key-vault-secrets-management) ,[Google Secret Manager](https://infisical.com/blog/gcp-secret-manager) ) First-party providers with data sources and ephemeral resources Single-cloud by design, covering three clouds means running three of them


SOPS File encryption, not a service Community provider decrypts committed files at plan time No access control, audit log, or rotation of its own


There is capability overlap here. Infisical and Vault both issue dynamic secrets, and both rotate credentials. The differences that end up mattering are hosting, license, and how much operating you want to do; see[Top HashiCorp Vault Alternatives](https://infisical.com/blog/hashicorp-vault-alternatives) for a fuller comparison.


### Where SOPS fits


[SOPS](https://github.com/getsops/sops) comes up in this conversation, but it isn’t a secrets manager, it’s file encryption. There's no access control, audit log, or rotation. You encrypt the values inside a YAML or JSON file with an age, PGP, or cloud KMS key, then commit the ciphertext. The keys stay readable, and the values don't, so the file lives in Git and diffs still show what changed.


SOPS is a complement rather than a replacement. If your GitOps flow needs the encrypted file itself in version control, use SOPS for that file, harden the backend that holds your state, and keep runtime credentials in a system that can rotate them.


## The default to change


None of these techniques is heavy. The shift is mostly one of default:


- Keep real values in Infisical, not in` .tf` files or` .tfvars` .
- Prefer ephemeral resources for anything that only needs to exist during a run, as they never touch state.
- Use the data source when you must materialize a value, and encrypt your backend when you do.
- Treat` sensitive = true` as display hygiene, always on, never load-bearing.
- Keep provider credentials in the environment, and prefer short-lived OIDC in CI.
- Encrypt, version, and lock the backend that holds your state.
- Pass secret references into modules, not secret values.
- Where you can, drop standing secrets entirely with dynamic secrets, and rotate the ones you can't.


Do that, and your state file goes back to being what it should be: a record of what you built, not a list of your credentials.
