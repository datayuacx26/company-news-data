---
schema_version: "1.0.0"
document_id: "1babdbc641e9227af3a264926ce46e8ab8dedfebe999c9823e64d46cb7374131"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/aws-secrets-manager-vs-kms"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:7a931acf962c6e6b2e8e78c98ef98de79b576921b4f1b0b751a8128c1555fd88"
---

# AWS Secrets Manager vs. KMS: Understanding the Difference

AWS Secrets Manager and AWS Key Management Service (KMS) sound similar, but are distinct: AWS Secrets Manager secures application secrets while KMS is custom-built for encryption keys.


The confusion arises because both services exist to protect sensitive values, and both are important tools for anyone securing an AWS setup. This only gets worse because API keys and encryption keys are both secrets in the sense that they need to be protected. But they diverge operationally.


An API key needs to be constantly available in a config file so your application can authenticate. The other never leaves its service and is used to scramble and unscramble other data. AWS Secrets Manager and AWS KMS are distinct services for a reason, but they overlap and interconnect.


## The difference between AWS Secrets Manager and KMS


[AWS Secrets Manager](https://infisical.com/blog/aws-secrets-manager-complete-guide) stores the API keys, OAuth tokens, and database credentials workloads use to authenticate to each other. AWS KMS creates and manages the keys that encrypt other data, including the data sitting inside Secrets Manager itself.


The distinction breaks down like this:


AWS Secrets Manager AWS KMS


What it is A managed store for application credentials A managed service for creating and controlling encryption keys


What it holds Secret values: database credentials,[API keys](https://infisical.com/blog/api-key-management) , and tokens Cryptographic keys used to encrypt and decrypt data


Rotation Frequent scheduled or ad-hoc rotations Keys don't rotate the data they protect. AWS can rotate the key material itself on a schedule


Access pattern Applications fetch a secret value at runtime Applications send data to KMS to be encrypted or decrypted, they never fetch the key itself


Typical use case Storing a database password an application logs in with Encrypting an S3 object, an EBS volume, or a Secrets Manager secret at rest


Any[secrets manager](https://infisical.com/blog/secrets-management-complete-guide) encrypts secrets to store them. That encryption is powered by an encryption key secured by a KMS. Cryptographic keys underpin secrets management. If an encryption key is compromised, any secret it protects is also compromised. This is why encryption keys rarely move and KMSs are secured thoroughly.


## Secrets Manager and KMS already work together by default


Every secret in Secrets Manager is encrypted using a KMS key: an AWS managed key by default, or a customer managed key you specify yourself. Under the hood, Secrets Manager uses envelope encryption. KMS generates a unique data key for the secret, that data key encrypts the actual secret value, and only the small, wrapped data key ever passes through a KMS API call.


## When you’d use AWS KMS, and when you’d use AWS Secrets Manager


An engineer wiring a database credential into an application creates it in Secrets Manager and moves on. The KMS layer underneath never demands attention because the AWS managed key handles it automatically. Most engineers never touch a KMS, and for most use cases, they never need to.


For most scenarios and jobs, you can ignore AWS KMS if you use AWS Secrets unless you’re part of the platform/compliance/security/infrastructure team that owns it. You might manually operate a KMS when things become more complex:


- You need to encrypt an object that lives outside Secrets Manager (an S3 bucket, a config blob, an EBS volume)
- Building envelope encryption into your own application code.
- A compliance requirement landing on your desk that says the AWS managed key isn't sufficient and you need a customer managed key with your own policy attached.


They mean the problem moved down a layer, from "store this credential safely" to "control exactly which key encrypts what, and who is allowed to use it."


## When to use each, and when to use both deliberately


You’re always using some version of AWS KMS when you use AWS Secrets Manager, but you probably rarely interact with it. But when you’d work with them specifically differs:


- **Secrets Manager alone** is the default for storing and rotating credentials when you don't need to manage the underlying encryption yourself.


- **KMS alone** fits when you're encrypting your own application data or objects outside Secrets Manager (e.g. a[centralized secrets manager](https://infisical.com/blog/best-secret-management-tools) like Infisical), building envelope encryption into custom code, with no need for credential storage or rotation.


- **Both, deliberately** , is where regulated or multi-account environments end up: customer managed keys per environment or per team, key policies that have to line up with a secret's resource policy for cross-account access, and Lambda-based rotation functions that themselves need permission to use the KMS key.


Usage tends to track org size:


A small team relies on the AWS managed key by default and rarely opens the KMS console at all. As teams grow, customers want to manage their own keys for cleaner audit trails. For large, regulated orgs, key policies and grants become their own governance surface with a dedicated team.


On the secrets side, founding teams usually start by individually storing static secrets in AWS Secrets Manager. As engineering orgs grow, a platform/infrastructure/DevOps team might use the secrets manager and its features more intensely to administrate advanced features and workflows.


Both tools follow the same lifecycle: They start as necessary little utilities users spend little time in and eventually become core tools for a specialized team or engineer.


Both Secrets Manager and KMS are robust tools, but they also have their limitations.


## When AWS-native tooling isn’t enough


Both Secrets Manager and KMS solve the AWS-native version of their respective problems well, but both become suboptimal once you extend beyond AWS or need specific features these cloud-native services don’t offer. A few triggers show up repeatedly:


- Infrastructure that spans more than one cloud or includes on-premises systems — the same AWS-native gap that shows up when comparing[Secrets Manager to Parameter Store](https://infisical.com/blog/aws-secrets-manager-vs-parameter-store) .
- A security or compliance team that wants one governance layer instead of separate tooling per environment.
- Sophisticated security measures like Key Management Interoperability Protocol (KMIP). Once an organization standardizes on KMIP for its key infrastructure, AWS's own tooling can't be the whole answer, since AWS KMS doesn't speak KMIP.


AWS’s native services also can’t offer the feature depth of a dedicated tool. AWS Secrets Manager offers Lambda templates for secret rotations, but they’re not Secrets Manager-native. More advanced features like Dynamic Secrets require building from scratch.


In a tool like Infisical, security isn’t yet another service, but the entire purpose of the product. This means advanced features come out of the box, it can work on any infrastructure and integrate with anything.


## Where Infisical fits


Infisical’s offfering go beyond AWS’s own services and offer better developer experience and more functionality. It not only replaces AWS Secrets Manager/KMS, but can even act on top of it if you don’t want to migrate.


### AWS Secrets Manager Sync


Infisical's[AWS Secrets Manager Sync](https://infisical.com/docs/integrations/secret-syncs/aws-secrets-manager) lets you run Infisical as the source of truth for your secrets and mirror them out to AWS Secrets Manager automatically. Use it when you want one place to manage, audit, and rotate secrets across more than just AWS, while still landing values in Secrets Manager for the services that expect to read from it natively.


### AWS KMS as an external key provider


Infisical’s enterprise plan has its own internal KMS for encrypting the data it stores, but a project can hand that job to[AWS KMS](https://infisical.com/docs/documentation/platform/kms-configuration/aws-kms) instead and authenticate with an assumed role or an access key. This makes every encryption and decryption operation for that project run against a symmetric AWS KMS key.


This is worth doing when an organization has already standardized its key strategy on AWS KMS and wants Infisical governed by the same key policies and grants without creating parallel infrastructure. If there’s no existing AWS KMS, Infisical's own system is simpler and needs no extra configuration. External KMS support, alongside[HSM](https://infisical.com/docs/documentation/platform/kms/hsm-integration) and[KMIP](https://infisical.com/docs/documentation/platform/kms/kmip) , is an Enterprise-plan capability.


## The short version


Default to Secrets Manager for anything your application authenticates with. Reach for KMS the moment you're encrypting something yourself. Treat customer managed keys, HSM backing, and KMIP as the tier you grow into once key custody becomes an explicit requirement, not a hypothetical one.
