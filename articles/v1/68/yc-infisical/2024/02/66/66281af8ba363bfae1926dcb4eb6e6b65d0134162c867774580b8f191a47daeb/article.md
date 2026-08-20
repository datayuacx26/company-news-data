---
schema_version: "1.0.0"
document_id: "66281af8ba363bfae1926dcb4eb6e6b65d0134162c867774580b8f191a47daeb"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/introducing-saml-sso"
published_at: "2024-02-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:1c53e27b8994679cafc96416062c75d76f82870115508066b804a5ea8eb0b70e"
---

# Infisical + SAML SSO

While most teams use standard member controls to invite/remove members to their organization, they should know that Infisical supports both[SAML SSO](https://en.wikipedia.org/wiki/SAML_2.0) and[SCIM provisioning](https://en.wikipedia.org/wiki/System_for_Cross-domain_Identity_Management) as part of its enterprise package to streamline user management through an external identity provider (IdP) such as[Okta](https://infisical.com/docs/documentation/platform/sso/okta) ,[Azure](https://infisical.com/docs/documentation/platform/sso/azure) , or[JumpCloud](https://infisical.com/docs/documentation/platform/sso/jumpcloud) .


Once configured, SAML SSO allows an organization's members to log into Infisical via supported IdP with support for both SP-initiated and IdP-initiated workflows. Moreover, SAML SSO can be enforced in an organization's settings to ensure that it remains accessible only to users that authenticate through the designated IdP. Paired with the SCIM provisioning feature for user provisioning/de-provisioning, SAML SSO makes is a powerful feature to consider for optimal user and access control management.


### **What is SAML?**


Security Assertion Markup Language or SAML is a standard for telling a service provider that a principal is who they say they are with the help of an external identity provider. SAML makes single sign-on (SSO) technology possible by providing a way to authenticate a user once and then communicate that authentication to application(s). In our context, the service provider would be Infisical, the principal would be the user trying to access Infisical, and the identity provider would be an external service like Okta or Azure used by an organization to manage its members.


To note, Infisical draws an internal distinction between user-based and organization-based authentication methods. A user-based authentication method is unique to each user and specifies how they can log into an organization without an enforced organization-based authentication method; this includes traditional email/password-based authentication and Google/GitHub/GitLab SSO methods. Meanwhile, an organization-based authentication method, which is typically enforced, specifies that a particular authentication mode must be used to access that organization's resources regardless of what user-based authentication methods are configured at the user-level for the members of that organization.


In short, SAML SSO lets members of an organization to log into Infisical through a compatible external IdP.


### **How can I get started with Infisical + SAML SSO?**


Using SAML SSO with Infisical involves configuring SAML details in both Infisical and an intended external IdP and then toggling the enforce control in Infisical. You can learn more about how to configure SAML SSO across various IdPs with Infisical[here](https://infisical.com/docs/documentation/platform/sso/overview) .


With SAML SSO, you can bolster your organization's security and better streamline your user management workflow.


Onward and upward!


Infisical - The open source secret management platform


[Infisical](https://infisical.com/) helps thousands of teams and organizations store and sync secrets across their team and infrastructure.
