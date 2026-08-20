---
schema_version: "1.0.0"
document_id: "e11591949661ed3c95cc19d0058b10ac8f49d9eceb9745dc2eca98e5d9ff3394"
company_key: "yc-propelauth"
company: "PropelAuth"
source_id: "yc-propelauth-news-import-52b3c4ba8ae4"
canonical_url: "https://www.propelauth.com/post/developer-and-customer-management-roles"
published_at: "2026-05-27T00:00:00+00:00"
first_seen_at: "2026-07-25T20:01:02.470883+00:00"
fetched_at: "2026-07-28T22:07:11.290939+00:00"
content_hash: "sha256:b4c7bb25d17b415d8fcfa16fbca8bc8095b2dcb7595ec8a9157d54172e3d34b6"
---

# Introducing Developer and Customer Management Roles

We've added two new roles to the PropelAuth dashboard: **Developer** and **Customer Management** . The idea behind both is the same: give people access to everything they need for their job, without access to anything extra.


In practice, the person setting up your login methods and the person helping a customer fix their SSO connection are doing pretty different things. Before today, you'd have had to give both of them broader access than either actually needed. These roles fix that.


## Developer #


The **Developer** role is for engineers who need to configure how the project works. That covers updating signup and login settings, managing organization settings, configuring API keys, setting up integrations, updating roles and permissions, configuring MCP and OAuth, setting custom domains, and generating backend API keys.


Developers can view users and organizations, but can't create, modify, or delete them. It's the right fit for someone who needs full access to the configuration side but shouldn't be touching customer data.


## Customer Management #


The **Customer Management** role is for support and ops teams who work with users and organizations directly. That includes creating and deleting accounts, blocking and unblocking users, managing Enterprise SSO and SCIM connections, changing role mappings, managing org membership, and archiving API keys.


It doesn't include any access to project configuration, so your support team can do their job without being able to accidentally change your login settings.


## How these fit with the existing roles #


Nothing changes for existing roles. Owner, Admin, Member, and ReadOnly all work the same way they did before, and you now have six to choose from:


- **Owner** : Full access
- **Admin** : Full access except billing, impersonation settings, and org membership settings
- **Developer** : Project configuration only; no user or org management *(new)*
- **Customer Management** : User and org management only; no project configuration *(new)*
- **Member** : Moderate access across both areas
- **ReadOnly** : View-only


Neither new role can manage teammates or update billing. That stays with Owner and Admin.


## Full permission breakdown #


Permission Developer Customer Management


Users


View users ✓ ✓


Create / modify users — ✓


Block / unblock users — ✓


Delete users — ✓


Organizations


View organizations ✓ ✓


Create / manage orgs — ✓


Enable Enterprise SSO / SCIM — ✓


Delete organizations — ✓


Project configuration


Update signup / login settings ✓ —


Manage API key settings ✓ —


Configure MCP / OAuth / integrations ✓ —


Update roles & permissions ✓ —


Team & billing


Invite / manage teammates — —


Update billing — —


For the full matrix across all six roles, check out[the docs](https://docs.propelauth.com/overview/basics/dashboard?ref=propelauth.mymidnight.blog#managing-teammate-roles) . The roles are live for all accounts now. To assign them, head to your organization settings in the dashboard.
