---
schema_version: "1.0.0"
document_id: "409b2e19f0202f8ef470b7c47c82ad79cdbe61b49eed86dda2968013a2e7b9a2"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0d567709f64e"
canonical_url: "https://www.microsoft.com/en-us/power-platform/blog/power-pages/native-dataverse-authorization-public-preview-for-stronger-security-in-power-pages/"
published_at: "2026-07-07T15:06:13+00:00"
first_seen_at: "2026-07-20T04:34:28.280378+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:1c4d92a45d1ca08be98f9deb27beea9e647fa2adda957de9d8de2b269754e1c0"
---

# Native Dataverse Authorization (Public Preview) for Stronger Security in Power Pages

We’re excited to introduce the public preview of **Native Dataverse Authorization** **(Enhanced Authorization)** in Power Pages.


This release brings a significant advancement in how external user access is secured and managed. Authorization is now enforced directly within Dataverse, providing stronger security, improved visibility, and more precise control while keeping the maker experience unchanged. This feature currently needs to be enabled at a website level.


### What’s new in this release


#### Authorization enforced directly in Dataverse


Access decisions are evaluated where the data resides, ensuring consistent enforcement of security policies across all access scenarios and strengthening overall data protection. This helps reduce security gaps and ensures a more reliable access control framework.


#### Improved audit visibility of external users


Audit logs now reflect the actual external user performing actions. This enhances traceability, simplifies compliance reporting, and provides clearer operational insights. It also enables teams to investigate issues faster and maintain stronger accountability.


#### Column-level security support


Protect sensitive data with greater precision. Column security profiles allow you to control not only which records users can access, but also which specific fields are visible or editable. This ensures sensitive information is exposed only to the right users when needed.


Learn more:[Column security profile (preview)](https://learn.microsoft.com/power-pages/security/column-security-profile)


#### Custom Scope for granular access control


Introducing Custom Scope, enabling fine-grained, row-level access control based on business conditions. This allows you to define exactly which records each user can access, beyond traditional access models. It gives you greater flexibility to model real-world access scenarios with precision.


Learn more:[Custom access type (Preview)](https://learn.microsoft.com/power-pages/security/table-permissions?#custom-access-type-preview)


#### Seamless experience for makers


There’s no need to redesign your applications. Makers can continue using familiar Power Pages constructs while benefiting from stronger, platform-level enforcement behind the scenes. This allows teams to adopt enhanced security without impacting productivity or development workflows.


### How it works (at a glance)


Native Dataverse Authorization brings Power Pages security closer to the Dataverse model through a simple alignment of concepts.


At a high level:


• **Contacts are represented as Dataverse users** : Each external user is represented within Dataverse, enabling access to be evaluated in a true user context.


• **Web roles align with security roles** : Roles defined in Power Pages align with Dataverse security roles, ensuring consistent role-based access control.


• **Operations run in user context** : All operations execute in the context of the signed-in user rather than a shared application identity, ensuring access is consistently enforced based on user permissions.


**What to be aware of**
If you have existing plugins or custom logic registered on Dataverse tables, they will now execute using the calling user’s permissions. In some cases, this may surface access-related errors where actions previously succeeded with elevated privileges. We recommend reviewing and validating such customizations to ensure they align with user-level access.


### Getting started


Enable Native Dataverse Authorization in your Power Pages site and start exploring these enhancements:


•[Enable enhanced authorization (preview)](https://learn.microsoft.com/power-pages/security/unify-pages-security-with-dataverse)
•[Column security profile (preview)](http://learn.microsoft.com/power-pages/security/column-security-profile)
•[Custom access type (Preview)](https://learn.microsoft.com/power-pages/security/table-permissions#custom-access-type-preview)
•[Audit logging](https://learn.microsoft.com/power-pages/configure/implement-privacy)


### **We are looking forward to** your **feedback**


Your feedback is crucial in shaping the future of this feature. We want to[hear from you](https://ideas.powerpages.microsoft.com/d365community/forum/1edba0ec-30cf-ec11-a7b5-000d3a545c96) !
