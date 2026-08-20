---
schema_version: "1.0.0"
document_id: "2038ca652979ff46f058c7be93b1e38380cbe549d9195697b65d6827a172ecf5"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/infisical-update-august-2023"
published_at: "2023-09-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:827b6dd6463857157f6d0982c14fe086cc79772d480878fd323d2effd843b82e"
---

# Infisical Update August 2023

In August, we focused a lot on improving the quality of the product. That said, we did ship some new great features that we’re excited to discuss in this update!


### Audit Logs 2.0


In the past, you may have used Audit Logs 1.0, a feature documenting any queries and mutations applied to secrets. Although these audit logs provided users some observability over the status of their secrets, their capabilities were too limited to be useful in practice. With Audit Logs 2.0, we’re proud to say that you can now:


- Track 25+ different events from secret to project-level operations.
- Filter audit logs by event, actor, source, date, or any combination of these filters.
- View extensive metadata for each event.


Re-built with practicality in mind, Audit Logs 2.0 got your security team covered the next time any security incident or review is needed.


### Integrations


With the help of the community we’ve shipped two new integrations:


- [TeamCity](https://infisical.com/docs/integrations/cloud/teamcity)
- [GCP Secret Manager](https://infisical.com/docs/integrations/cloud/gcp-secret-manager)


To date, members of our community have shipped 12 integrations, enabling users to sync secrets from Infisical to other platforms like GitLab, GitHub, Vercel, and more. If you’re looking to contribute to Infisical, you should note that integrations are some of the easiest features to build and get started with the codebase.


Sounds interesting? Join our community[Slack](https://infisical.com/slack) to engage and learn more about how you can get started contributing to Infisical.


### GitHub SSO


To date, we’ve supported email-password based authentication, Google SSO, and SAML SSO (Okta, Azure, JumpCloud). With this update, we’ve now released GitHub SSO, enabling new users to sign up with GitHub and existing users to enable GitHub as an authentication method for their account.


Going above and beyond, we’ve added the ability for users to opt in for multiple authentication methods, linking multiple authentication providers under one account.


### Improved Password Requirements


Previously, Infisical employed strong password requirements for users including enforcing minimum length requirements, variation criteria, and cross-checking passwords against a list of common passwords. With the help of our community, we’ve further strengthened the platform’s password policy to cross-check passwords against a database of ~700M breached passwords and enforce rules to prevent low entropy values containing PII data amongst other common information.


### Small Things:


- You are now able to change the order of environments in your projects.
- Checkly integration now supports custom suffixes which can help you differentiate between environments.
- Improved integration stability with a more reliable quieuing system.
- Tags got a redesign – colors are now consistent and you can choose any color you want.


### Interesting Reads


We also wanted to feature some of the best articles that our community members have written about Infisical. If you write something about Infisical, please let us know[on Slack](https://infisical.com/slack) so that we can amplify it across our social media.


- [Set up Infisical in a development cluster](https://iamunnip.hashnode.dev/infisical-open-source-secretops-kubernetes-setup) by Unni P
- [Set up Infisical in AKS using ArgoCD + Helm and integrate with an application using kustomize](https://mrdevops.medium.com/infisical-open-source-secretops-apply-it-using-gitops-approach-245f57fcd67e) by Hassene Fliss


### Community Calls


Lastly, In August, we started doing **weekly community calls** on Wednesdays at 8am PT. Would love to see you there. Find out more about these in our[community Slack](https://infisical.com/slack) .


As always, we’ve many exciting updates coming up in the pipeline and can’t wait to announce them next month!
