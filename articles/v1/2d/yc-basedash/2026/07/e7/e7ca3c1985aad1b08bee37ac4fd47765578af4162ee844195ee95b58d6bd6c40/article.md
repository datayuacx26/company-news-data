---
schema_version: "1.0.0"
document_id: "e7ca3c1985aad1b08bee37ac4fd47765578af4162ee844195ee95b58d6bd6c40"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/introducing-basedash-scim/"
published_at: "2026-07-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:86d9b29e6a0b20890a85072729a1930cb9ff1b4ed48201f04ccdbdc2aa5c76f2"
---

# Introducing Basedash SCIM

Today we’re launching **SCIM provisioning for Basedash** , a new enterprise control that keeps the user and group lifecycle aligned with the identity provider your organization already trusts.


For Heads of BI and VPs of Data, SCIM is about more than provisioning. It is evidence that Basedash can roll out across a changing organization without turning the data team into the manual access-management layer.


Your org changes. Access keeps up.


## Why identity becomes a BI risk as adoption grows


Analytics rollouts rarely stay small. More teams need answers, more leaders need dashboards, and more people need governed access to data. Every new user makes identity ownership more important.


Without a directory-driven lifecycle, the BI tool can drift from the organization around it. People change teams while old group memberships remain. Departing employees can stay active longer than intended. IT, security, or the data team becomes responsible for repeating the same updates inside another application.


That creates three enterprise rollout risks at once: stale organization access, an administrative bottleneck, and more friction when security reviewers ask who controls the user lifecycle.


*Enterprise identity readiness: directory-aligned users, groups, and organization membership.*


## Grow without losing control


SCIM lets Basedash follow supported provisioning changes from Okta, Microsoft Entra ID, and other compatible SCIM 2.0 providers. As the organization changes, the identity lifecycle can change with it:


- **Join:** create a user and synchronize their group memberships when the identity provider assigns Basedash
- **Move:** update supported user details, groups, and memberships as teams change
- **Leave:** deactivate the person’s Basedash organization membership when the provider sends a deactivation
- **Return:** reactivate the same organization membership when the provider restores it


The result is a rollout model that does not require data leaders to become the app-by-app identity administrator. Analytics can reach more people while the directory remains the operating source of truth for who belongs.


## One source of truth. Fewer access gaps


SCIM synchronizes users, groups, and group memberships. That makes ownership clear: the identity provider controls who belongs to the organization and its synced groups.


Resource permissions remain a separate Basedash decision. A Basedash admin chooses which data sources, dashboards, and other resources each group can access. SCIM does not provision administrator roles or directly assign those permissions.


This separation gives enterprise teams both automation and control:


1. The identity provider manages people and group membership
2. SCIM carries those supported lifecycle changes into Basedash
3. Basedash admins govern access to analytics resources


*One source of truth for identity, with resource permissions controlled in Basedash.*


## Enterprise controls work together


SCIM is one layer in Basedash’s enterprise control stack:


- **Single sign-on (SSO)** authenticates people through the company identity provider
- **SCIM** manages the supported user, group, and membership lifecycle
- **Role-based access control (RBAC)** governs workspace, group, and resource access
- **Row-level security** controls which rows users can see


Each layer has a distinct job. SCIM strengthens identity lifecycle management without replacing the authentication, authorization, and data controls around it or assigning resource permissions.


Together, these controls give data and security leaders a clearer model for enterprise rollout: central identity, explicit authorization, and governed data access.


## How it works


The high-level flow is simple:


**Directory → SCIM → Basedash**


Your organization manages users and groups in Okta, Microsoft Entra ID, or another compatible identity provider. SCIM sends supported user, group, membership, and active-status changes to Basedash. Basedash reflects those identity changes while admins continue to manage resource permissions separately.


*SCIM joins SSO, RBAC, and row-level security in the Basedash enterprise control stack.*


## Getting started


SCIM is available on the Basedash Enterprise plan.[Talk to the Basedash team](https://www.basedash.com/book/book-a-demo) to plan your rollout, or follow the[SCIM setup guide](https://www.basedash.com/docs/features/scim) when your administrators are ready to configure a compatible identity provider.


You can also explore Basedash’s broader[enterprise controls](https://www.basedash.com/enterprise) and[security model](https://www.basedash.com/security) .


**Grow analytics across your organization without losing control of who belongs.**
