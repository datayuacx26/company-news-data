---
schema_version: "1.0.0"
document_id: "2f44bebfd5adc7a6544b4f3cafbbe353ea9ed9946cd625f1e89be1be9504ae95"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/gcp-secret-manager"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:ab64a9cc3ff03fee590f96cd715a837c169136362f931f31ba8c10b58d1aebc0"
---

# Google Cloud Secret Manager: Setup, Best Practices & GCP Guide

Google Cloud Secret Manager is GCP's native service for storing secrets: database passwords, API keys, OAuth tokens, and any other sensitive value. For teams exclusively building on Google Cloud, it's the obvious starting point. It's purpose-built for GCP and integrates with a variety of services. It also authenticates through the same identity and access management (IAM) you already use everywhere else in GCP.


Security infrastructure is a long-term commitment, and migrating it later is painful, so it's worth understanding Secret Manager's versioning model, its replication behavior, how rotation actually works, and when it stops being the right secrets management tooling.


## What Google Cloud Secret Manager is


Google Secret Manager is a managed service for storing, accessing, and versioning secrets, with access governed by IAM and every access logged in Cloud Audit Logs.


The premise is the same as with any[secrets management tool](https://infisical.com/blog/best-secret-management-tools) . Instead of baking credentials into` .env` files or config where they can leak, your application authenticates to Secret Manager and pulls credentials at runtime. This eliminates[secret sprawl](https://infisical.com/blog/what-is-secret-sprawl) and gives you one audited, encrypted place where access is controlled and logged. For the broader fundamentals, see our[secrets management guide](https://infisical.com/blog/secrets-management-complete-guide) .


### How Parameter Manager and Cloud KMS differ from Secret Manager


Two adjacent GCP services can look similar to Secrets Manager, so it's worth being explicit.


- **Cloud KMS** manages encryption keys. Secret Manager *uses* KMS under the hood to encrypt your secrets, so it isn't an either/or choice. You'd only interact with KMS directly if you were building custom encryption workflows.
- **Parameter Manager** is Secret Manager’s closest relative. It’s an extension of Secret Manager built for *configuration data* , both sensitive and non-sensitive, with JSON and YAML validation. The pattern mirrors[AWS's Parameter Store vs. Secrets Manager](https://infisical.com/blog/aws-secrets-manager-complete-guide) distinction: put credentials in Secret Manager, put application configuration in Parameter Manager.


## How Secret Manager works


Three things define how Secret Manager behaves and where it surprises people: its versioning model, its replication policy, and IAM-based access.


### Versioning and version states


A secret in Secret Manager is a *container* . The actual value lives in a numbered version to ensure auditability and enable rotations. Applications usually reference the` latest` alias or pin to a specific number.


Each version is in one of three states:


- **ENABLED:** The current secret that should be used for workloads.
- **DISABLED:** This secret can’t be used anymore, but the data is still there.
- **DESTROYED:** A destroyed secret is gone and the value can no longer be found.


Secrets should typically be disabled before destroying them. Disabling is reversible, so you can avoid outages by not destroying secrets before you’ve verified it’s no longer in use anywhere.


### Replication policy: the GCP-specific decision


Replication policy is a Google-specific feature that has cost and compliance consequences. When you create a secret, you pick a replication policy you can never change.


- **Automatic replication** lets Google manage where the secret is stored for availability and latency. You're billed one location regardless of how many regions Google uses. This is the right default for most teams.
- **User-managed replication** lets you decide the regions a secret is stored in, which is necessary for data-residency and compliance requirements. You’re billed per region: a single version replicated to three regions costs three times as much. Only companies with specific requirements should use this.
- **Regional secrets** are a modern feature that let you create secrets pinned to a single region with a regional API endpoint, which some compliance regimes require.


You’d have to recreate a secret to change its secret's replication policy. This choice should be deliberate.


### Encryption


By default, Secret Manager encrypts everything at rest with Google-managed keys, and you don't have to think about it. For more control, Google allows a **customer-managed encryption key (CMEK)** from Cloud KMS.


This is an advanced feature for organizations who need to own their own encryption or are importing complex workflows onto Google Cloud Platform.


### Access control with IAM


Access is governed by IAM, using the same model as the rest of GCP. The key role is` roles/secretmanager.secretAccessor` , which grants permission to read a secret's value.


You can grant` secretAccessor` at the project level, but you usually shouldn't, because that hands a service account every secret in the project. Secret Manager lets you bind IAM **directly on an individual secret resource** , which is how you actually implement least privilege (the practice of provisioning the lowest levels of access and permissions needed):


```text
gcloud secrets add-iam-policy-binding prod-payments-db-password \
--member="serviceAccount:payments-app@my-project.iam.gserviceaccount.com" \
--role="roles/secretmanager.secretAccessor"


```


That binding gives the` payments-app` service account read access to one secret and nothing else. If the role was attached to the entire project, the service account could access many other credentials, which would increase the blast radius if this account were compromised. Always bind at the secret level for production credentials rather than at the project level.


## Getting started in the console


The fastest path to your first secret is the Google Cloud console. Open Secret Manager, choose **Create secret** , give it a name, paste a value (this becomes version 1), and pick a replication policy (Automatic unless you have a residency requirement).


Naming matters because secret names are unique within a project. A descriptive path-style scheme such as` prod-payments-db-password` makes IAM bindings, search, and auditing scale better than disparate secrets simply titled` Postgres` or` Stripe` . Once workflows become repeatable or more complex, most teams graduate from using the console and move to` gcloud` or Terraform.


## Working with the gcloud CLI


Day-to-day work lives in` gcloud secrets` . Creating a secret and its first version takes a single command that reads the value from a file, which keeps it out of your shell history:


```text
gcloud secrets create prod-payments-db-password \
--replication-policy="automatic" \
--data-file=./password.txt


```


Adding a new version later is just as direct, and it's how you "update" a secret:


```text
gcloud secrets versions add prod-payments-db-password \
--data-file=./new-password.txt


```


And accessing the current value:


```text
gcloud secrets versions access latest \
--secret=prod-payments-db-password


```


It’s usually worth using` --data-file` over passing a secret value inline, since anything typed on the command line can land in shell history or process listings, creating a potential vulnerability. Read from a file and delete it afterward.


## Accessing secrets from GKE and outside GCP


For Kubernetes workloads, the integration that matters is Workload Identity. It binds a Kubernetes ServiceAccount to a Google IAM service account, so your pods call the Secret Manager API authenticated by identity*.* This means you don’t have to inject a static token. Combined with per-secret IAM bindings, a pod can read exactly the secrets it needs and nothing more, with no credential in the manifest.


Google Secret Manager offers an add-on for Google Kubernetes Engine (GKE), a managed Container Storage Interface (CSI) component, along with a Secret Store CSI driver provider. Both mount secrets into pods as files, which is useful when you'd rather not call the API from application code.


For workloads *outside* GCP, such as another cloud, on-prem, or CI runners, Workload Identity Federation lets an external identity exchange its token for GCP access without a long-lived service-account key. This is the right pattern whenever you'd otherwise be tempted to export and store a service-account JSON key, which is itself a credential you then have to protect and rotate.


This is also where a GCP-native solution like Secret Manager runs into its limits. Integrating external workloads is always a workaround and doesn’t offer deep, native integrations. Organizations running multi-cloud setups won’t use Google Secret Manager as their source of truth.


## Managing secrets with Terraform


If you run infrastructure as code, Secret Manager maps cleanly to two Terraform resources:` google_secret_manager_secret` for the container and` google_secret_manager_secret_version` for the value:


```text
resource "google_secret_manager_secret" "db_password" {
secret_id = "prod-payments-db-password"
replication {
auto {}
}
}


resource "google_secret_manager_secret_version" "db_password" {
secret      = google_secret_manager_secret.db_password.id
secret_data = var.db_password
}


```


The same caveat that applies to every cloud secrets manager applies here: anything Terraform manages goes into Terraform state, and state stores values in plaintext. A literal password in your configuration lives in state and in any plan output. Source the value from a variable marked` sensitive = true` , or create the empty secret with Terraform and write the actual version through a separate process so the value never enters state. Treat your state backend as a secret store in its own right. ([More on Terraform secrets.](https://infisical.com/blog/how-to-manage-secrets-on-terraform-using-infisical) )


## Rotating secrets


Secret rotations are scheduled or on-demand changes of secret values. It’s best practice to rotate API keys, auth tokens, and other credentials regularly. Google Secret Manager is somewhat finicky when it comes to rotation. It does notification-based rotation, not rotation itself:


1. You configure a rotation period and a **Pub/Sub topic** on a secret.
2. At the scheduled interval, Secret Manager publishes a` SECRET_ROTATE` message to that topic.
3. *You* run a Cloud Function (or Cloud Run service) subscribed to the topic that does the actual work: generate a new credential, update the downstream database or service, and store the new value as a new secret version.


The logic here is a bit complex and can get brittle if you’re rotating a lot of secrets across different services.


```text
gcloud secrets update prod-payments-db-password \
--next-rotation-time="2026-07-01T00:00:00Z" \
--rotation-period="2592000s" \
--topics="projects/my-project/topics/secret-rotation"


```


The credential change itself is not in the command. This is the opposite of AWS Secrets Manager, which ships ready-made Lambda rotation for RDS and friends. GCP gives you the scheduling and the trigger but leaves building the rotation logic to you. For a Cloud SQL password, that means writing and maintaining the Cloud Function yourself. It's flexible, but it's work, which means secrets rotation isn’t turnkey.


## Google Secret Manager pros and cons


Like any tool, Google Secret Manager has both upsides and downsides. Most of them are related to being GCP-native, but some are purely about features


### Pros


- Purpose-built and simple: one focused service, IAM-native, with a clean versioning model.
- Per-secret IAM bindings make real least-privilege access straightforward.
- Workload Identity removes static service-account keys for GKE and federated external workloads.
- Deep integrations with the Google Cloud ecosystem
- Replication policy gives explicit control over data residency when you need it.


### Cons


- No native answer for workloads outside GCP, so workloads on other clouds, self-hosted infrastructure, or third-party SaaS apps end up with parallel stores, negating the benefits of centralized secrets management.
- Rotation is notification-only, so you build and maintain the actual rotation logic yourself.
- User-managed replication and accumulating versions can quietly multiply the bill.
- No built-in approval workflows, change management, or unified cross-team audit view.
- Secrets only, so keys and certificates live in separate services and a "single pane" spans three products.


## Cost and how to control it


Secret Manager's pricing is cheap per unit but easy to underestimate. There are three components:


- **$0.06 per active secret version per location per month** ,
- **$0.03 per 10,000 access operations**
- **$0.05 per rotation notification** .


The free tier covers six active versions and 10,000 access operations a month. Management operations, including creating secrets, changing version state, and destroying versions, are free.


Two things inflate the bill in ways teams don't anticipate:


**Replication multiplication.** A version under automatic replication bills as one location. The same version under user-managed replication across three regions bills as three, so $0.18/month instead of $0.06. Multiply that across hundreds of secrets and several environments and it adds up.


**Version accumulation.** Because every update adds a version, and old versions stay *active* (and billable) until you disable or destroy them, a secret that's rotated frequently can quietly accumulate dozens of billable versions. Controlling Google Secret Manager pricing requires some housekeeping of disabling and destroying versions you no longer need.


## Secret Manager best practices


**Bind IAM at the secret level.** Grant` secretAccessor` on the specific secret a workload needs, not at the project level. This is the single most important habit for limiting blast radius.


**Use Workload Identity, not service-account keys.** For GKE and federated external workloads, authenticate by identity so there's no static key to store, leak, or rotate. Pair this with[secret scanning](https://infisical.com/docs/documentation/platform/secret-scanning/overview) across your repositories to catch the credentials that slip in by hand.


**Disable before you destroy.** Putting a version in the DISABLED state first gives you a reversible "looks deleted" state and a safe rollback path. Destroying is permanent.


**Choose replication deliberately.** Default to automatic, and use user-managed replication or regional secrets only when data residency requires it, and remember the per-region cost and the fact that the policy is immutable.


**Use a clear naming hierarchy.** A scheme like` <env>-<service>-<purpose>` makes IAM, search, and reasoning about access scale with your secret count.


**Audit with Cloud Audit Logs.** Every access is logged. That record of who read which secret and when is both an operational tool and a compliance requirement under frameworks like SOC 2 and PCI DSS.


**Keep version sprawl in check.** Disable and destroy stale versions, and cache reads so access-operation volume and version count both stay under control.


## When GCP Secret Manager isn't enough


Secret Manager is good when you run simple secrets workflows exclusively using Google Cloud and aren’t planning to branch out. It's simple, IAM-native, cheap to start, and works well for GCP workloads. But many teams realize its limitations and move beyond GCP Secret Manager.


The first is multi-cloud. Secret Manager is GCP-exclusive, so the moment you also run on AWS, Azure, on-premises, or across SaaS platforms, you're maintaining separate secret stores with no shared access model and no unified audit view, plus brittle workarounds between them. Centralization is the first step of secrets management best practices. Teams that grow beyond single-cloud setups often evaluate secrets management tools like Infisical that can provide an auditable, centralized layer on top of a variety of secrets managers.


The second is workflow depth. Secret Manager stores and versions secrets well, but leaves customers to build approval workflows for accessing production credentials, granular change management, a unified cross-team audit view, or dynamic short-lived secrets generated on demand. Teams end up building these in-house and get stuck maintaining these solutions.


The third is rotation, where GCP gives you the trigger and leaves you to build and maintain the engine.


### Why some teams add Infisical on top of GCP Secret Manager


This is where a dedicated platform earns its place, not as a replacement for GCP but as the layer that unifies it with everything else.[Infisical](https://infisical.com/) is an open-source identity security platform whose secrets management works across any infrastructure and provides the workflows and developer experience Secret Manager lacks:


- a usable dashboard
- approval workflows
- dynamic secrets
- a single audit view across every environment.


Infisical is built on Postgres rather than proprietary infrastructure and runs in the cloud or fully self-hosted.


Adopting it isn't a disruptive migration. Infisical's[GCP Secret Manager sync](https://infisical.com/docs/integrations/secret-syncs/gcp-secret-manager) connects to your existing project directly, so you can import the secrets you already have, keep them synchronized, and adopt centralized workflows without ripping anything out. Secret Manager keeps doing what it's good at, and Infisical becomes the consistent governance layer once you've outgrown a single cloud or don’t want to manually handle secret rotations or third-party integrations.


If you want a direct feature-by-feature view, see[Infisical vs. GCP Secret Manager](https://infisical.com/infisical-vs-gcp-secret-manager) . And if you're surveying the field more broadly, our rundown of[GCP Secret Manager alternatives](https://infisical.com/blog/gcp-secret-manager-alternatives) and our roundup of the[best secrets management tools](https://infisical.com/blog/best-secret-management-tools) put the landscape side by side.


Try[Infisical for free today](https://app.infisical.com/signup) or[schedule a demo](https://infisical.com/talk-to-us) to see it in action.
