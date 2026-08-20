---
schema_version: "1.0.0"
document_id: "61b69eabdd0b06862e8d009a1218f5a14098ab2dfa67412916d90d8c65bf6253"
company_key: "yc-ryvn"
company: "Ryvn"
source_id: "yc-ryvn-news-import-a6f556f77bcf"
canonical_url: "https://ryvn.ai/blog/how-to-deploy-clickhouse-on-ryvn"
published_at: "2026-04-20T00:00:00+00:00"
first_seen_at: "2026-07-23T23:41:20.719716+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:17bc2dca9a51b7ecd5ae2ddea14b0ebd4f8b5c1645353ce3b0a83cb4ff89003f"
---

# How to Deploy ClickHouse on Ryvn

Below we deploy ClickHouse on Ryvn using YAML files and Git Sync. We'll set up the operator, build a Helm chart for the database, handle authentication with variable groups, and wire up ingress. There's a marketplace blueprint at the end that does most of this in a single file if you'd rather skip ahead.


## Prerequisites


### Create a Ryvn organization


Go to[control.ryvn.app](https://control.ryvn.app/) and create an account. Connect the Ryvn GitHub App to your organization under **Settings → Integrations** so Ryvn can access your repositories.


### Provision an environment


Open the **Environments** tab, click **Create Environment** , and name it` develop` . Pick your cloud provider and follow the provisioning flow. Ryvn provisions a Kubernetes cluster with ingress and cert-manager in your cloud account, which takes a few minutes.


### Enable Git Sync


Under **Settings → Infra as Code** , click **Configure** , connect your GitHub account, and pick the repository and branch where you'll commit Ryvn resource files. Any YAML you push to that branch from here on gets picked up and applied.


### Install the Ryvn CLI


You can skip this if you prefer the dashboard, but it's faster from the terminal.


## Step 1: Define and install the operator


ClickHouse runs on Kubernetes through an operator — a controller that watches for custom resources and turns them into running database instances. Its chart lives on GHCR, so we start by telling Ryvn where to find it:


The same thing is available from the UI if you'd rather click through it — **Services → Create service → Helm Chart → Published Chart** gives you a form for the chart registry URL and chart name:


Charts from external registries don't get automatic releases on Ryvn. Register the version by hand on the channel you want to deploy from:


With the service and release in place, we can write the installation. I'm turning off cert-manager and webhooks here because the Ryvn environment already has its own, but you might want them on if your setup is different:


Commit and push. Git Sync deploys the operator, and the installation shows up on the service detail page with its current health and deploy status:


## Step 2: Create the ClickHouse Helm chart


With the operator running, the next thing we need is a Helm chart that creates the custom resources the operator watches: a` ClickHouseCluster` for the database itself and a` KeeperCluster` for consensus. This chart lives in your repo, not in an external registry, so Ryvn can build and release it for you.


There are two ways to pass the database password.` defaultUserPassword` tells the chart to create a Kubernetes secret with a literal value you provide. That works for local testing but means the password ends up in your Helm values.` defaultUserPasswordSecretName` points at a secret that already exists, and that's the one we want because on Ryvn the secret comes from a variable group. We'll wire that up in step 4.


Here's where we wire the password in. The operator CRD expects a secret reference under` settings.defaultUserPassword.secret` :


An` authSecretName` helper picks which secret to use:


The helper checks whether you supplied a literal password. If you did, it uses a secret the chart creates itself. If not, it falls back to the external secret name, and that's the path we're taking. The chart also has templates for` KeeperCluster` , an HTTP service on 8123, and ingress, but I'm not going to walk through those here because they're standard Helm and the secret wiring is the only part that's specific to Ryvn.


## Step 3: Register the chart as a service


Because this chart lives in your repo, we can define it as a repo-backed service. The` build.chartPath` field tells Ryvn where the chart source is:


When you sync this file, Ryvn opens a PR on your repository with a GitHub Actions workflow that packages the chart and cuts releases whenever you push changes to the chart directory. Merge that PR and you're set. Unlike the operator in step 1, you won't need to register releases by hand for this chart because the workflow handles it.


The service shows up in your **Services** list once it's synced, with tabs for Installations, Releases, and Settings:


## Step 4: Set up authentication and install the database


The chart needs a Kubernetes secret containing the database password, and you obviously don't want to commit that to Git. On Ryvn you handle secrets through variable groups. They're key-value stores that Ryvn syncs as Kubernetes secrets in each environment:


1. Go to your environment's **Variable Groups** section
2. Create a group called` clickhouse-auth`
3. Add a secret value with key` password` and your chosen password


Once created, the variable group shows up at the environment level and can be referenced by any installation in that environment:


Variable groups exist at the environment level, not at the installation level, so any installation in the environment can reference them. In the installation file below, the` k8sSecretName` template function resolves the variable group name to whatever Kubernetes secret name Ryvn assigned:


Push this file and Git Sync will apply it. The operator sees the new` ClickHouseCluster` CR and provisions the database. Keeper handles consensus. Port 8123 gives you HTTP access. The password gets injected from the variable group at deploy time, so it never appears in your repo.


The same flow is available from the UI under **Install in environment** . The install form auto-surfaces any variable groups in the selected environment, so you can see` clickhouse-auth` is already wired up:


## Step 5: Enable the dashboard (optional)


ClickHouse ships with a query UI called Play that's useful for ad-hoc queries. If you want to expose it over HTTPS, you can add ingress config to the same installation file. Ryvn environments come with NGINX ingress and cert-manager already set up, so you just need to reference them:


` external-nginx` is the ingress class Ryvn provisions for public traffic, and` external-issuer` handles TLS certificates. The template variables (` ryvn.env.state.public_domain.name` and so on) resolve per environment, so this exact config works everywhere without changes.


## Step 6: Verify the deployment


Give Git Sync a minute or two to process, then check the status:


Both installations should show as deployed. The Ryvn dashboard has the same information under the environment's installations page if you prefer a visual view. If something isn't right, the installation status will tell you what went wrong. The most common issues are Keeper failing to form a quorum because of insufficient resources, or the operator not starting because of a version mismatch. Fix the config, push again, and Git Sync redeploys.


## Step 7: Wire up to your application (optional)


If you have a server service that needs to talk to ClickHouse, you can link the same` clickhouse-auth` variable group so the password is available as an environment variable. The host and port are just standard Kubernetes service DNS:


The variable group injects its` password` key as an environment variable, so your application code can read it like any other secret. Port 8123 is HTTP, and most ClickHouse client libraries default to it. Port 9000 is native TCP. It's faster for bulk inserts but not all clients support it.


## The shortcut: marketplace blueprint


Everything above gives you full control over every piece of the deployment, but it is a lot of YAML to maintain. If you don't need to customize the operator config or the CRD layout, the Ryvn marketplace has a[ClickHouse blueprint](https://ryvn.ai/marketplace/43d65823-dda6-4b4c-9658-512cd9c00328) that wraps all of it into a single file:


Reference it from your repo with a` BlueprintInstallation` :


The blueprint handles the operator, the chart, releases, and password generation. Other services can reference its outputs directly instead of using manual env vars:


When we update the operator version in the blueprint, every installation picks up the change on the next deploy. Adding another customer environment is just one more file with different inputs.


---


Honestly, I would start with the blueprint unless you know you need something it doesn't expose. It's less to maintain and you can always break out individual pieces later if your requirements change.
