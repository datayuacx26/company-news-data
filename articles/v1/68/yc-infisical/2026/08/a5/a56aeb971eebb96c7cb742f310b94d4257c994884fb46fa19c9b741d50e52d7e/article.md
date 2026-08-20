---
schema_version: "1.0.0"
document_id: "a56aeb971eebb96c7cb742f310b94d4257c994884fb46fa19c9b741d50e52d7e"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/hashicorp-vault-secrets-management"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T16:12:12.119592+00:00"
fetched_at: "2026-08-04T17:31:37.256081+00:00"
content_hash: "sha256:73419817ef802f19400ecb5e50de4f3c31242927664b060ee41f8364341c1c36"
---

# HashiCorp Vault for Secrets Management: How It Works and Best Practices

HashiCorp Vault is to secrets managers what Salesforce is to CRMs: it's used by large enterprises, has every feature you could ask for, and is often derided for being complex and slow.


Vault's reputation as a robust legacy tool precedes it, but is rarely backed by an understanding of how it manages secrets. It's worth understanding what Vault is and does in detail whether you're thinking about deploying it or already running it and running into issues.


Understanding Vault means getting its[secrets management](https://infisical.com/blog/secrets-management-complete-guide) model: how it stores static secrets and issues dynamic ones, the operational practices that keep deployments healthy, and the friction points that show up once it's running in production.


The first thing worth untangling, though, is which Vault you're even talking about. "HashiCorp Vault" now describes several products that differ in what they can do and how you run them.


## Which Vault are you even looking at


Many teams evaluating Vault are wondering: Is Vault free? Can it be self-hosted? What separates the free version from the paid one?


There are currently three Vault products HashiCorp offers.


### Vault Community Edition


Vault Community Edition is the free, self-managed version, and it's self-hosted only. It includes the core features for secrets management. For Vault, that means secrets engines, storage, and other features we'll define later on.


It isn't open source anymore. HashiCorp relicensed Vault away from the open-source MPL 2.0 license to the[Business Source License (BSL)](https://infisical.com/blog/hashicorp-new-bsl-license) , which makes it source-available without qualifying as true open-source software.


Vault Community Edition is ideal for anyone already familiar with Vault (or willing to learn a new tool) who doesn't mind the operational overhead of self-hosting or enjoys tinkering with new tools.


### Vault Enterprise


Vault Enterprise is the paid, self-managed tier. On top of the core features, it adds:


- Namespaces (isolated tenants within a single cluster)
- Performance replication and disaster recovery replication across clusters
- HSM (hardware security module) auto-unsealing
- Sentinel policies (a policy-as-code engine)
- FIPS 140-2 seal wrapping (a federal cryptographic standard some regulated industries require)
- [KMIP](https://infisical.com/blog/kmip) (Key Management Interoperability Protocol) support
- Transform secrets engines


That's a lot of jargon, but the basic thing to remember is that if a Vault deployment needs multi-region failover or SAML-based authentication, it needs Enterprise.


Vault Enterprise is great for any company that needs the full suite of features and doesn't mind hiring a dedicated team and doing custom development.


### HCP Vault Dedicated


HCP Vault Dedicated is a hosted, single-tenant version of Vault Enterprise that HashiCorp runs for you. It carries most of Enterprise's capability set without the operational burden of running and maintaining your own Vault instance.


A few Enterprise features like FIPS 140-2 seal wrapping aren't available on the HCP deployment option, but most other advanced features are. It isn't free: a small production cluster on HCP Vault Dedicated's Essentials tier starts in the range of a thousand dollars a month before adding a per-client fee for every authenticated identity that talks to it.


HCP Vault Dedicated is ideal for any organization that doesn't want the burden of self-hosting, but does want to use Vault for its secrets management.


If you're researching Vault, you may also read about two other products:


- **HCP Vault Secrets** was a separate, lighter-weight managed product for storing and generating secrets without running a full Vault cluster, which offered a free tier. After its acquisition by IBM, HashiCorp announced[it's discontinued](https://infisical.com/blog/hashicorp-vault-secrets-shutdown) .
- **Vault Radar** scans source code and git history for secrets that shouldn't be there, which makes it a secrets scanner, not a secrets manager.


Evaluating Vault today is a choice between Community Edition, Enterprise, and HCP Vault Dedicated. These differ mainly in available features and deployment options, but all share the same foundation.


## What Vault actually is


Vault's core job is storing and controlling access to secrets. But Vault isn't a secrets manager you install and configure. It's a framework for building a secrets management system. A few concepts define how it works:


- **Secrets engines.** Vault has multiple ways to store secrets, called engines. Each handles a different kind of secret with different rules. The key/value engine stores static values you set yourself while the database engine, AWS engine, and similar engines generate dynamic credentials on demand.
- **Storage backends.** Vault's data (encrypted secrets, configuration, audit metadata) lives in Raft by default, although it can use other storage backends, including major databases. HashiCorp only offers limited support for non-Raft storage.
- **Seal and unseal.** Vault encrypts data with a root key which is protected by a process called sealing. On startup, Vault is sealed and unusable until it's unsealed with a quorum of unseal keys or an auto-unseal mechanism backed by a cloud KMS (key management service).
- **Policies.** Access control in Vault is written as policy documents that grant or deny paths within Vault's secret hierarchy. Every human and machine identity that talks to Vault authenticates, then operates under whatever policy is attached to that identity.
- **Runtime.** Vault is a binary, and how you run it is up to you: a` systemd` service on a VM, a Docker container, or a cluster of pods deployed with HashiCorp's own Helm chart on Kubernetes.


This means Vault is not a plug-and-play product. That makes it extremely customizable, but it also burdens users with assembling their own Vault first. Whether that's the right tradeoff depends on whether your team wants to assemble the pieces or wants them already assembled.


One of the most important aspects of this is which kinds of secrets you'll be using.


## Static secrets versus dynamic secrets in Vault


Vault handles two kinds of secrets, which create the same end result (a secret delivered where it's needed) but work fundamentally differently.


**Static secrets** live in the key/value engine. A secret exists as its name (key), for example` STRIPE_KEY` , and the actual key (value), for example` sk_live_...` . These are secrets that don't change by themselves. Stripe doesn't issue a new key automatically, so it's stored in Vault until it's changed.


There are some ergonomics to make this easier for end users. The static engine keeps a configurable number of past versions of each secret, ten by default, so a bad value can be rolled back, and deleting a version soft-deletes it first, recoverable until it's explicitly destroyed.


**[Dynamic secrets](https://infisical.com/blog/secret-rotation-vs-dynamic-secrets)** are generated on request and revoked after use. As an example, the database secrets engine lets an administrator configure Vault with database connection details and a role defining the permissions of a generated user. An application then requests access from Vault, and Vault mints database credentials with a short lease. No two applications ever share a credential, and a credential that's compromised is only useful until its (short) lease runs out.


These two types of secrets differ operationally. Generally speaking, Vault is a capable secrets management tool, but it has many nuances and resulting best practices.


## Best practices for running Vault well


A few practices separate a Vault deployment that stays healthy from one that becomes a maintenance burden. Most of them are Vault-specific expressions of broader[secrets management best practices](https://infisical.com/blog/secrets-management-best-practices) .


- **Use dynamic secrets engines wherever the target system supports them.** Reaching for the database, AWS, or equivalent dynamic engine instead of storing a static credential in the key/value engine removes rotation work.
- **Design policies around least privilege from the start.** Every identity, human or machine, should get the paths it needs and nothing more. Loosening a policy later because something didn't work is a common pattern, and it's how a Vault deployment quietly accumulates more standing access than anyone intended.
- **Set lease durations deliberately.** A long lease defeats much of the point of a dynamic secret. A short lease can create renewal churn for long-running processes. This is a per-use-case decision, not a global default.
- **Turn on audit logging and actually look at it.** Vault can log every request to an audit device. Pair it with alerting on the requests that matter, like policy changes or root token usage.


## Where teams hit friction


Vault ships as infrastructure, not a finished workflow. There's no:


- Built-in UI for requesting temporary access
- Approval flow for a teammate to grant another teammate scoped access
- Dashboard a non-infrastructure engineer can use without CLI familiarity


Teams that want those things have to build them. Some companies even build an entirely custom UI on top of Vault to give the rest of the org a usable interface. Others need a team of infrastructure engineers dedicated full-time to their Vault deployment. Many organizations run into both the complexity of Vault and its often extremely high[effective costs](https://infisical.com/vault-tco-calculator) . The reason for this is simple: Vault was designed for automated machine use cases. Developers were an afterthought. Some organizations can live with these downsides and treat them as the cost of keeping secrets safe (or throw so many resources at the problem until it's solved).


Teams with plenty of staff to build and operate the exact workflows they need sometimes appreciate the flexibility.


This is a much harder tradeoff for a team that wants a secrets manager they can use now, without spending months getting it into production and staffing a dedicated team to run it. Unless it's set up correctly, Vault may be safer but not operationally more efficient than managing secrets manually. A new engineer needing access to a database has to wait on whoever owns the policy file, who then edits and re-applies a policy by hand, and the actual access doesn't land until that change is reviewed, approved, and merged. This is just one example of one part of Vault. Combine it with configuring storage, secrets engines, sealing rituals, and the rest, and Vault can soon become more work than it saves.


If you want to avoid these shortcomings, it's worth evaluating Infisical.


## Where Infisical differs


Infisical takes the same core idea that secrets should be managed centrally with dynamic, short-lived credentials wherever possible, and ships it as a ready-to-go product instead of a framework.


A few things differentiate Infisical from Vault:


- Every secret type is available from the start. Where Vault makes you enable and configure a different secrets engine for each use case, Infisical ships them all out of the box. Adding your first dynamic or database secret carries zero extra setup overhead.
- Infisical runs on Postgres rather than a proprietary storage layer, which means the operational skill set your team already has covers it. No Raft cluster expertise or unsealing ceremony required.


Many Infisical customers previously used Vault.[Migrations](https://infisical.com/docs/documentation/platform/external-migrations/vault) became easy because Infisical can directly import secrets from Vault (and many other secrets managers) to replicate your existing setup in minutes.


For teams evaluating whether to build that layer themselves on Vault or adopt it already built,[our full comparison against Vault](https://infisical.com/infisical-vs-hashicorp-vault) walks through the tradeoff in more depth, and[Vault pricing](https://infisical.com/blog/hashicorp-vault-pricing) is worth checking against your own numbers before deciding which path costs less in practice.


[Try Infisical for free](https://app.infisical.com/signup) or[talk to an expert](https://infisical.com/talk-to-us) about migrating a Vault deployment.


### Finn


Technical Content Marketer, Infisical


[linkedin](https://linkedin.com/in/finnlobsien)
