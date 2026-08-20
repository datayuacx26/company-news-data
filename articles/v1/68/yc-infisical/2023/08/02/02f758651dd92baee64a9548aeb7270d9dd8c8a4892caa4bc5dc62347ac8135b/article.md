---
schema_version: "1.0.0"
document_id: "02f758651dd92baee64a9548aeb7270d9dd8c8a4892caa4bc5dc62347ac8135b"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/infisical-update-july-2023"
published_at: "2023-08-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:9683120f1be12bbb435d9baf456904f7c0f7f8c271c8e52d6227c2bc5ae16c5a"
---

# Infisical Update July 2023

In July’s update, we have lots of great features, new integrations with products we love, and some exciting company news in the end 👀


## Secret Imports and Referencing


[Both of these features](https://infisical.com/docs/documentation/platform/secret-reference) let you establish a true source of truth for you secrets. In particular:


- With imports, you can now inherit secrets from other environments and folders. For example, you can create ‘root’ environments that serve as a starting point for every other environment.
- With references, you can reference secret values across folders and environments. Every time you change the main secret’s values, it will be propagated to every reference.


## Integrations


On the way to become a truly universal secret management solution, we have added 8 more integrations with services like:


- [Bitbucket](https://infisical.com/docs/integrations/cicd/bitbucket)
- [Cloud66](https://infisical.com/docs/integrations/cloud/cloud-66)
- [Codefresh](https://infisical.com/docs/integrations/cicd/codefresh)
- [DigitalOcean App Platform](https://infisical.com/docs/integrations/cloud/digital-ocean-app-platform)
- [Laravel Forge](https://infisical.com/docs/integrations/cloud/laravel-forge)
- [Northflank](https://infisical.com/docs/integrations/cloud/northflank)
- [Terraform Cloud](https://infisical.com/docs/integrations/cloud/terraform-cloud)
- [Windmill](https://infisical.com/docs/integrations/cloud/windmill)


Huge shoutout to our amazing community members who’ve been helping us a lot with these integrations. You can[join us in our community Slack](https://infisical.com/slack) .


## New authentication methods + SAML SSO


For the past couple of months, we’ve been testing various authentication methods with a closed list of customers. Finally, we are excited to announce that we have:


- Added support for Google SSO.
- Added support for[Okta](https://infisical.com/docs/documentation/platform/sso/okta) ,[Azure AD](https://infisical.com/docs/documentation/platform/sso/azure) , and[JumpCloud SAML](https://infisical.com/docs/documentation/platform/sso/jumpcloud) authentication.


Spoiler: GitHub SSO was also added in the last couple of days in August.


## Much More


- Redesigned the project/organization experience.
- Updated the secrets overview page; users are now able to edit secrets directly from it.
- Released[webhooks](https://infisical.com/docs/documentation/platform/webhooks) – more materials on this will follow soon!
- Lots of performance to the main dashboard.


## Team Update


This month, we're proud to welcome Akhil Mohan to our team as a founding engineer. Excited to work together with him to build the future of secret management and more!


There's more coming all the time, and we're building features to solve your problems.
