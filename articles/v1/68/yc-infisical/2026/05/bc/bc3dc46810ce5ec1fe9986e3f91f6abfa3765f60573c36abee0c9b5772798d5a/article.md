---
schema_version: "1.0.0"
document_id: "bc3dc46810ce5ec1fe9986e3f91f6abfa3765f60573c36abee0c9b5772798d5a"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/learning-from-the-vercel-breach-a-secrets-security-playbook"
published_at: "2026-05-04T04:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:0592ca5a82852ccd76817db0830e0c823af878bbddce1d510ef38b25ee2bffda"
---

# Learning from the Vercel Breach: A Secrets Security Playbook

# Learning from the Vercel Breach: A Secrets Security Playbook


On April 19, 2026, Vercel[disclosed a security incident](https://vercel.com/kb/bulletin/vercel-april-2026-security-incident) involving unauthorized access to internal systems.


Their response was ideal: a detailed public bulletin, Mandiant engaged, affected customers notified inside the week, and product hardening shipped alongside the post-mortem.


What makes the incident worth studying is not the platform it happened on. It is the attack chain, and the fact that every platform engineering team reading this has the same structural exposure sitting in their stack.


## The attack chain


Per Vercel's bulletin, the compromise moved through four stages:


1.


Initial breach at **Context.ai** , a third-party AI tool with a Google Workspace OAuth app installed by a Vercel employee.


2.


Takeover of the employee's individual Vercel Google Workspace account using access gained through the compromised app.


3.


Access to the employee's Vercel account via the compromised Workspace account.


4.


Lateral movement inside a Vercel environment and enumeration of non-sensitive environment variables (the ones that decrypt to plaintext on read).


The indicator of compromise (IOC) Vercel published is a single OAuth client ID.


Three things stand out about that chain. The initial breach happened at a fourth party. The first pivot was into a SaaS identity provider via an OAuth-connected tool. And the lateral movement happened inside a control plane that legitimately held production credentials in readable form.


None of those failure modes are unique to Vercel. Substitute any SaaS that holds your env vars, any IdP your engineers use, and any AI or productivity tool granted Workspace or Entra ID scopes in the last eighteen months.


The takeaway: your effective secrets perimeter is the union of every system that can read a production credential in cleartext. If keys, DB passwords, and signing secrets live in a hosting provider's env var store, a compromise of that control plane compromises all of them in a single hop.


## Rebuilding the threat model


If this has you auditing your own setup, catalog every location a production secret exists in cleartext or in a form reversible by a platform operator. For most teams the list looks like:


-


Hosting provider env var UIs (Vercel, Netlify, Render, Heroku)


-


CI/CD secret stores (GitHub Actions, GitLab CI, CircleCI contexts)


-


.env files on engineer laptops and in dotfile repos


-


Kubernetes Secrets (base64 is not encryption)


-


Terraform state files, especially unencrypted remote backends


-


Notion pages, Slack DMs, Loom recordings, shared Postman collections


Every item on that list contributes to blast radius. Modern secrets hygiene collapses it toward one: a single vault, with everything else consuming ephemeral references or short-lived tokens issued at runtime.


## How to harden your stack against this attack chain using Infisical


Here is the five-step playbook. None of these steps would have stopped the initial OAuth-app compromise that started the Vercel chain. That is a SaaS perimeter problem that lives outside your secrets stack.


What the playbook does is make sure that when something like this happens to you, the attacker cannot enumerate readable production credentials in a single hop. The chain breaks before it reaches the part that hurts


### 1. Centralize in a secrets platform with RBAC and audit


Pick a[secrets management platform](https://infisical.com/blog/best-secret-management-tools) and make it the source of truth.


[Infisical](https://infisical.com/) runs on Postgres and supports:


-


Project and environment scoping with folder-level RBAC


-


SCIM-provisioned groups


-


Approval policies on secret changes


-


Point-in-time recovery for secrets and folders


-


Deployment as Infisical Cloud, dedicated tenant, or self-hosted on Kubernetes via the[Helm chart](https://github.com/Infisical/infisical/tree/main/helm-charts)


Non-negotiables at this layer:


-


Every read audited


-


Every mutation versioned


-


Access scoped by identity, not shared bearer tokens


-


Transparent encryption story (open source matters for this one)


### 2. Eliminate secret zero with workload identity


The classic failure mode is putting a long-lived` INFISICAL\\_TOKEN` in a .env file. That just moves sprawl one layer down.


The fix is[machine identity authentication](https://infisical.com/docs/documentation/platform/identities/overview) backed by platform-native trust. Infisical supports:


-


AWS Auth: IAM principals like EC2 instances and Lambda functions


-


GCP Auth: GCE, GKE workload identity, and other GCP runtimes


-


Azure Auth: managed identities, Entra ID, OCI


-


Kubernetes Auth: projected service account JWTs


-


OIDC Auth: any federated provider including GitHub Actions, GitLab CI, and CircleCI


The pattern is identical across all of them: the workload presents an identity document the platform already issues, Infisical validates it against a trust policy, and returns a short-lived access token scoped to that identity.


For GitHub Actions, that means zero long-lived credentials in the repo. The OIDC token minted for the workflow run is the bootstrap.


The same pattern applies to agentic workloads. Every MCP server and every coding agent with tool access is a non-human identity with scopes. Issue them machine identities, scope them narrowly, expire their credentials aggressively. Do not let an agent hold an API key a human would not be allowed to hold.


For agents specifically, even narrowly-scoped credentials carry a risk traditional secrets management cannot fully address: prompt injection. We recently released[Agent Vault](https://github.com/Infisical/agent-vault) as a research preview to explore this problem. Agent Vault runs as an HTTPS_PROXY forward proxy that terminates TLS at the edge and injects credentials into the outbound request, so the agent process never reads the secret in any form. It is early, but the design is where we think credential delivery for agents has to head.


### 3. Kill .env files in local development


The .env file is a grep-able plaintext copy of production credentials on every engineer's laptop. It is also the first thing an agentic coding tool indexes when pointed at your repo.


Replace it with runtime injection:


` infisical run --env=dev --path=/backend -- npm run dev`


` infisical run --env=dev -- ./mvnw spring-boot:run`


` infisical run --env=dev -- bin/rails server`


The infisical run CLI authenticates with your user or machine identity, pulls secrets for the environment and path, and injects them into the child process's environment.


What you get:


-


Nothing touches disk


-


Secrets rotate at the vault and propagate on the next process start


-


No .env to commit, Slack to a new hire, or leave behind when someone offboards


### 4. Replace static secrets with dynamic ones


Rotation shortens the exposure window.[Dynamic secrets](https://infisical.com/docs/documentation/platform/secrets-mgmt/concepts/dynamic-secrets) eliminate it.


Your application requests a lease on a credential generated on demand, scoped to the requesting identity, and revoked at TTL expiry. Infisical issues dynamic credentials for:


-


Databases: PostgreSQL, MySQL, SQL Server, MongoDB, Redis, Oracle, Azure SQL


-


Cloud IAM: AWS IAM, GCP IAM, Azure Entra ID


-


And more across the provider landscape


Concrete flow for a service talking to Postgres:


1.


Service authenticates with Infisical via Kubernetes Auth using its projected service account token.


2.


Requests a PostgreSQL lease with a 15-minute TTL.


3.


Infisical provisions a uniquely-generated user and password via the configured` creationStatement` .


4.


Service uses the credential for the duration of the task.


5.


At lease expiry the user is dropped via` revocationStatement` .


A leak inside that window has a blast radius measured in minutes and scoped to one identity, not your entire fleet.


For microservices across dev, staging, and production, this is the single highest-leverage change. "Rotate everything" becomes a configuration change at the vault, not a sprint of coordinated deploys.


For credentials that can't be issued dynamically (third-party API keys, signing keys, OAuth client secrets), automated rotation is the next best thing. Infisical natively rotates credentials, on a configurable schedule, with the new value propagating through the same sync pipelines that deliver the rest of your secrets. The exposure window shrinks from "until someone notices" to whatever interval you set.


### 5. Log, alert, and review


Audit logs are only useful if you read them. Pipe Infisical's audit stream to your SIEM and alert on anomalies:


-


Bulk enumeration against a single identity


-


Reads from unexpected IP ranges


-


Leases requested outside business hours for workloads that only run during business hours


The organizations that caught this incident early were the ones whose detection stack actually fired on the IOC.


## Still deploying to Vercel


None of this implies you should move off Vercel. The platform is excellent and the team shipped stronger sensitive-env-var defaults as part of their response.


The right architecture is to move the source of truth:


-


Keep secrets in Infisical as the source of record


-


Use the[Vercel secret sync](https://infisical.com/docs/integrations/secret-syncs/vercel) to push them into your Vercel project's environment variables


-


For any values that never need to be read back (API keys, signing secrets, tokens), mark them as sensitive on the Vercel side so they cannot be retrieved from the dashboard or API


-


Drive all rotation from the vault, then let the sync propagate


Vercel becomes a delivery target, not a store of record.


## Where to start


If you are looking at a sprawl of services and environments and wondering where to begin, prove the pattern on one service first:


1.


Migrate its secrets into Infisical, scoped by environment and path.


2.


Replace the local .env workflow with` infisical run` .


3.


Wire up OIDC or cloud-native machine identity for CI and production.


4.


Convert one static database credential to a dynamic secret with a short TTL.


5.


Pipe audit logs to your SIEM and add alerts on the obvious anomalies.


6.


Repeat per service. The pattern is the same every time, which is the point.


You can start this playbook on Infisical Cloud at[app.infisical.com/signup](https://app.infisical.com/signup) . The CLI, machine identities, basic RBAC, and integrations are available on the free tier, and dynamic secrets, audit log streaming, approval workflows, and point-in-time recovery sit on paid plans.


If you are operating a larger microservices fleet and want help mapping the migration,[talk to us](https://infisical.com/talk-to-us) .


Security infrastructure does not improve because incidents happen. It improves because teams decide, between incidents, to do the architectural work.
