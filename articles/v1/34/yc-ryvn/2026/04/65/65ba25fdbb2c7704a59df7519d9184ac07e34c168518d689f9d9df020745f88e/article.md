---
schema_version: "1.0.0"
document_id: "65ba25fdbb2c7704a59df7519d9184ac07e34c168518d689f9d9df020745f88e"
company_key: "yc-ryvn"
company: "Ryvn"
source_id: "yc-ryvn-news-import-a6f556f77bcf"
canonical_url: "https://ryvn.ai/blog/how-to-deploy-langfuse-on-ryvn"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-23T23:41:20.719716+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:7b4ef034b00f034be2205328bcb2ff49eb8b5ed345a139849fcb51d3c7ac41e9"
---

# How to Deploy Langfuse on Ryvn

Below we deploy self-hosted Langfuse on Ryvn using YAML files and Git Sync. We'll register the chart, generate the secrets it needs, install it with its bundled Postgres/ClickHouse/Redis/MinIO, and expose the dashboard. There's a marketplace blueprint at the end that does most of this in a single file if you'd rather skip ahead.


## Prerequisites


### Create a Ryvn organization


Go to[control.ryvn.app](https://control.ryvn.app/) and create an account. Connect the Ryvn GitHub App to your organization under **Settings → Integrations** so Ryvn can access your repositories.


### Provision an environment


Open the **Environments** tab, click **Create Environment** , and name it` develop` . Pick your cloud provider and follow the provisioning flow. Ryvn provisions a Kubernetes cluster with ingress and cert-manager in your cloud account, which takes a few minutes.


### Enable Git Sync


Under **Settings → Git Sync** , click **Add Git Sync** and point it at the repository and branch where you'll commit Ryvn resource files. Any YAML you push to that branch from here on gets picked up and applied.


### Install the Ryvn CLI


You can skip this if you prefer the dashboard, but it's faster from the terminal.


## Step 1: Register the Langfuse chart as a service


Langfuse publishes its Helm chart at` https://langfuse.github.io/langfuse-k8s` , and the chart bundles Postgres, ClickHouse, Redis (Valkey), and MinIO as sub-charts. That means one chart install gives you the whole stack, which is exactly what we want for a self-hosted deployment. We start by telling Ryvn where the chart lives:


Charts from external registries don't get automatic releases on Ryvn. Register the version you want on the channel you'll deploy from:


Commit and push the service file. The release stays pinned until you register a new one, so upgrades are deliberate.


## Step 2: Generate the secrets Langfuse needs


Langfuse needs a handful of secrets before it will start: a salt for hashing API keys, a 256-bit encryption key for sensitive fields, a NextAuth secret for JWT signing, and passwords for each of the bundled data stores. You don't want any of that in Git, so we put it in a variable group.


1. Go to your environment's **Variable Groups** section
2. Create a group called` langfuse-secrets`
3. Add these keys with generated values (any password manager's random string generator works):


- ` salt` — 44 characters
- ` encryption-key` — 64 hex characters (this one must be 256 bits)
- ` nextauth-secret` — 44 characters
- ` postgres-password` — 32 characters
- ` clickhouse-password` — 32 characters
- ` redis-password` — 32 characters
- ` s3-access-key` — e.g.` langfuse`
- ` s3-secret-key` — 32 characters


Variable groups exist at the environment level, so any installation in the environment can reference them. The` k8sSecretValue` template function reads individual keys out of the synced Kubernetes secret, which we'll use in the next step.


Once the variable group is created you can confirm the keys with` ryvn describe variable-group` . Sensitive values are masked in the output, so it's safe to share screenshots with your team:


## Step 3: Install Langfuse


With the service and the secrets in place, we can write the installation. The Langfuse chart takes a big block of values that configures the web app, the worker, and each sub-chart. The interesting part is the secret wiring — every password comes from the variable group instead of being committed to Git:


Push this and Git Sync applies it. The chart creates a web deployment, a worker deployment, and one pod for each of the four bundled data stores. The` nextauth.url` value uses the internal cluster DNS for now so Langfuse works without public ingress — we'll point it at the real hostname in the next step.


## Step 4: Expose the dashboard (optional)


Langfuse's UI is where you'll spend most of your time: tracing LLM calls, inspecting prompts, checking token costs, running evals. To reach it from outside the cluster you need ingress, which Ryvn environments already have set up with NGINX and cert-manager. Update the` config` block on the same installation file to flip ingress on and point NextAuth at the public URL:


A few things worth flagging:


- ` external-nginx` is the ingress class Ryvn provisions for public traffic, and` external-issuer` handles TLS certificates. The template variables resolve per environment, so this exact config works everywhere.
- ` nextauth.url` has to match the public URL exactly or OAuth callbacks will break. The template keeps them in sync.
- ` signUpDisabled: true` is the right default for shared deployments. Create your admin user first, then flip it on and commit.


## Step 5: Verify the deployment


Give Git Sync a minute or two to process, then check the status:


The installation should show as deployed. The Ryvn dashboard has the same information under the environment's installations page if you prefer a visual view. If something isn't right, the installation status will tell you what went wrong. The most common issues are the encryption key being the wrong length (it has to be 64 hex chars for 256 bits) or the database sub-charts failing to come up because of tight resource limits on small clusters. Fix the config, push again, and Git Sync redeploys.


Once it's up, browse to` https://<installation-name>.<environment-domain>` , create your first user, and generate an API key in the project settings.


## Step 6: Wire up your application


Your application sends traces to Langfuse over HTTP using one of the[official SDKs](https://langfuse.com/docs/sdk) . Inside the same Kubernetes cluster, the host is the standard service DNS, so you don't need to route through public ingress:


Public and secret keys come from the Langfuse dashboard. Put them in a variable group on the app's environment instead of committing them — I left them inline above just to keep the example short.


## The shortcut: marketplace blueprint


Everything above gives you full control over the chart values, but it is a lot of YAML to maintain across environments — and the secret generation step is tedious to do by hand. If you don't need to customize the sub-chart configs, the Ryvn marketplace has a[Langfuse blueprint](https://ryvn.ai/marketplace/langfuse) that wraps all of it into a single file:


The blueprint generates the salt, encryption key, NextAuth secret, and every data-store password for you on first install. Flip` exposeDashboard` to` true` and you get a public HTTPS URL with TLS. Other services can reference its outputs directly instead of hard-coding internal hostnames:


When we update the chart version in the blueprint, every installation picks up the change on the next deploy. Adding another customer environment is just one more file with different inputs.


---


Honestly, I would start with the blueprint unless you know you need to customize the sub-chart configs. It's less to maintain, and the secret generation alone saves enough hassle to make it worth it. You can always break out the individual pieces later if your requirements change.
