---
schema_version: "1.0.0"
document_id: "e46e10b9a7155cd7cd15ce7ab843f2972172f2b5b007a4f75ad96a3c3aeda7a3"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/infisical-update-may-2024"
published_at: "2024-06-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:0366890f9bdca7d423425660654d24e7866610da02fb93c3514e3f8ccbaf5d08"
---

# Infisical Update – May 2024

May 2024 has been one of the most exciting months for Infisical yet! Our team released many great features and has a lot of awesome news to share!


## 1: AWS, GCP, Azure, and Kubernetes Native Auth Methods


It is now possible for applications to fetch secrets back from Infisical without needing to explicitly manage an additional secret to authenticate with the platform in the first place.


Here’s how each method works:


-


[Kubernetes Auth](https://infisical.com/docs/documentation/platform/identities/kubernetes-auth) : Pods can use Kubernetes-native service accounts to have a service account credential mounted at a specified path. This credential can be used to authenticate with Infisical.


-


[AWS Auth](https://infisical.com/docs/documentation/platform/identities/aws-auth) : AWS IAM principals for services like EC2 and Lambda can send a signed query containing a computed signature specific to the underlying IAM principal to authenticate with Infisical.


-


[GCP Auth](https://infisical.com/docs/documentation/platform/identities/gcp-auth) : GCP services like Compute Engine, App Engine, Cloud Functions, and Kubernetes Engine can use GCP-native ID tokens to authenticate with Infisical.


-


[Azure Auth](https://infisical.com/docs/documentation/platform/identities/azure-auth) : Azure services like VMs, App Services, Functions, and Kubernetes Service can use Azure-native managed identity access tokens to authenticate with Infisical.


By treating Kubernetes, AWS, GCP, and Azure as trusted identity providers to supply and verify native platform tokens, we achieve a near “secretless” authentication pattern for applications running on these platforms. It’s now as seamless as pointing your application to Infisical to fetch back secrets.


## 2: Secret Sharing Infrastructure


Developers frequently need to share secrets with team members, contractors, or other third parties. This can be risky due to potential leaks or misuse. Infisical now offers a secure solution for[sharing secrets](https://infisical.com/docs/documentation/platform/secret-sharing) over the Internet in a time-bound manner. With its zero-knowledge architecture, secrets shared via Infisical remain unreadable even to Infisical itself.


## 3: Access Requests and JIT Access


Infisical users can now[request access](https://infisical.com/docs/documentation/platform/access-controls/access-requests) to particular environments, folders, and secrets directly from Infisical. Based on the predefined policies, admins are able to approve or deny such requests to automatically provision access to users. Such access can be either permanent or temporary (also known as[Just-in-time Access Provisioning](https://infisical.com/docs/documentation/platform/access-controls/temporary-access) ).


## 4: Secret Referencing Update


[Secret Referencing](https://infisical.com/docs/documentation/platform/secret-reference) is probably one of Infisical's most popular features. This month, our team has worked to expand support of secret references and imports to all Infisical clients (e.g.,[CLI](https://infisical.com/docs/cli/commands/secrets) ,[Kubernetes Operator](https://infisical.com/docs/integrations/platforms/kubernetes) ,[SDKs](https://infisical.com/docs/sdks/overview) , integrations, and more) – no matter in which environments you are operating secrets with Infisical, Secret References will now be supported seamlessly.


On top of that, when creating secret references via UI, secret references are now automatically suggested in a dropdown. Just start typing` ${}` to see a list of possible secret references:


## 5: Infisical Jenkins Plugin


Even though it was already possible to operate secrets in Jenkins with Infisical CLI, you can now use the official[Infisical Jenkins Plugin](https://infisical.com/docs/integrations/cicd/jenkins) to fetch secrets in your pipelines and projects. On top of that, secrets are masked in the build log, so users can’t accidentally print them.


## 6: Statuses and Manual Sync for integrations


The integration experience has been significantly improved. It is now possible to manually trigger integrations. In addition, users can easily track statuses of their integrations as well as corresponding failure reasons in case something goes wrong.


## Much more


- [Audit Log Streaming](https://infisical.com/docs/documentation/platform/audit-log-streams) to any external logging.
- [Dynamic Secrets with AWS IAM](https://infisical.com/docs/documentation/platform/dynamic-secrets/aws-iam) .
- [AWS Secrets Manager](https://infisical.com/docs/integrations/cloud/aws-secret-manager) and[Parameter Store](https://infisical.com/docs/integrations/cloud/aws-parameter-store) Integrations now support syncing for tags and custom KMS keys.
- [SCIM](https://infisical.com/docs/documentation/platform/scim/overview) and[User Groups](https://infisical.com/docs/documentation/platform/groups) now integrate with each other.
- [Infisical is now available on AWS Marketplace](https://infisical.com/blog/infisical-launches-on-aws-marketplace) .


That's it for our May 2024 update! If you have any feature requests or feedback for Infisical, please[join our Slack community](https://infisical.com/slack) and share those in the **#infisical-feedback** channel.


If you have any questions about Infisical Pro or Enterprise, you can contact us atsales@infisical.com or[sign up for a demo](https://infisical.com/schedule-demo) .
