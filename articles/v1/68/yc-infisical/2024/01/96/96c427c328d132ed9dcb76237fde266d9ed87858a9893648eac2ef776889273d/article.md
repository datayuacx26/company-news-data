---
schema_version: "1.0.0"
document_id: "96c427c328d132ed9dcb76237fde266d9ed87858a9893648eac2ef776889273d"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/infisical-update-december-2023"
published_at: "2024-01-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:27fbae3f4241cde55348a27518969ea12daa03cd6964d0c039e62b036f81a299"
---

# Infisical Update December 2023

Our team in known to be lightning-fast! Still, even for our standards, in the last 3 months of 2023, we released an **incredible** amount of new features, bug fixes, performance improvements, and UX updates. Want to find out more? Read this blog till the end.


### Machine Identities + Universal Auth


With[Machine Identities](https://infisical.com/docs/documentation/platform/identities/overview) , you are now able to create custom entities to represent a workload or an application that requires access to your Infisical instance – think of it as an IAM user in AWS or service account in Google Cloud Platform (GCP). Such identities can authenticate into your Infisical instance using[Universal Auth](https://infisical.com/docs/documentation/platform/identities/universal-auth) .


For a long time, Infisical offered a possibility of creating[custom roles](https://infisical.com/docs/documentation/platform/role-based-access-controls) for users within organizations and projects – these roles provided a very straightforward and highly granular way to restrict actions and resources that are available to users. With Universal Auth, it is now possible to assign the same roles to Machine Identities to programmatically interact with Infisical.


In addition, Universal Auth supports adding restrictions for frequency, amount, IP-range, and timeline over which a certain token can be used.


### SDKs for Python, Node, and Java


In December, we announced our cross-language SDKs – redevelopped from scratch. It featured a completely new[Java SDK](https://infisical.com/docs/sdks/languages/java) . As well as significant improvements to[Python](https://infisical.com/docs/sdks/languages/python) and[Node](https://infisical.com/docs/sdks/languages/node) SDKs. You are now able to authenticate into all of these SDKs using[Machine Identities](https://infisical.com/docs/documentation/platform/identities/overview) .


For the curious, the base SDK is written in Rust, and you can find the repository for it[here](https://github.com/Infisical/sdk) .


### Infisical Agent


[Infisical Agent](https://infisical.com/docs/infisical-agent/overview) is a client daemon that simplifies the adoption of Infisical by providing a more scalable and user-friendly approach for applications to interact with Infisical. It eliminates the need to modify application logic by enabling clients to decide how they want their secrets rendered through the use of templates.


[Read how it works](https://infisical.com/docs/infisical-agent/overview)


### No-code Rotation for PostgreSQL/CockroachDB, MySQL/MariaDB, and SendGrid


Infisical now offers automatic rotaion for[SendGrid](https://infisical.com/docs/documentation/platform/secret-rotation/sendgrid) ,[PostgreSQL/CockroachDB](https://infisical.com/docs/documentation/platform/secret-rotation/postgres) , and[MySQL/MariaDB](https://infisical.com/docs/documentation/platform/secret-rotation/mysql) that you can set up in the Infisical Web UI with just a few clicks. After that, your API keys and access tokens will automatically rotate on a pre-specified schedule.


[Check it out](https://infisical.com/docs/documentation/platform/secret-rotation/overview)


### Better self-hosting experience


Self-hosting experience has been made significantly better and easier. In particular, we have reduced required JWT configuration from 6 secrets to just 1, added[docs](https://infisical.com/docs/self-hosting/overview) with 5+ more ways of self-hosting Infisical, and more!


[Find documentation here](https://infisical.com/docs/self-hosting/overview)


### Secret Reminders


Every now and then, our users need to remember to either manually rotate a secret or process it in a certain way. You can now do so with recurring secret reminders.


### Ansible Plugin


We now have a[native Ansible plugin](https://infisical.com/docs/integrations/platforms/ansible) for interacting with Infisical (e.g., fetching secrets from your Infisical Vault into Ansible Playbooks). Our Terraform Provider has also gotten better!


### GitLab SSO in FOSS


This one is very simple – you can now use authentication with GitLab in both Infisical Cloud and when Infisical On-prem – available completely free to all of our users!


[Learn hot to set it up](https://infisical.com/docs/documentation/platform/sso/gitlab)


### Updated API and CLI


Last but not least, our API and CLI have gotten even better! You can now use[Machine Identities with Infisical API](https://infisical.com/docs/api-reference/overview/authentication) (CLI coming soon!), and perform more actions with the CLI (e.g., creating folders).


### We are hiring


- [Full Stack Software Engineer – San Francisco](https://www.ycombinator.com/companies/infisical/jobs/VrU8pDw-full-stack-engineer)
- [Full Stack Software Engineer – Remote](https://www.ycombinator.com/companies/infisical/jobs/VrU8pDw-full-stack-engineer)
- [General Application](https://infisical.com/careers)


## 👋


That's it. If you have any questions, you can always ask those on[Slack](https://infisical.com/slack) . If you have any features requests, you're welcome to[open a GitHub issues](https://github.com/Infisical/infisical/issues) .


Stay tuned for more awesome updates next month!
