---
schema_version: "1.0.0"
document_id: "8c4e397fe9c8e726c58d08ff7e3a152795063f1bbc3c0217b4354bef514faad2"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/gitlab-restricted-access-improvements/"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:07.155656+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:8b820323da7ab85ab26cb18cdda3487b26a6aa86721f22a4cdbd67c9521a3812"
---

# Keep your GitLab seats in check with restricted access

GitLab restricted access for instance admins, group owners, and billing managers enables predictable seat costs with less manual gatekeeping. The feature has been significantly improved and is now more complete for the workflows that commonly affect seat usage. This update closes the gaps around identity provider provisioning, dormant user reactivation, and sign-in flows so organizations can use restricted access with more confidence in real-world environments.


In this article, you'll learn what restricted access does, what changed, and how to turn the feature on.


## What is restricted access?


Restricted access is a seat control feature available on GitLab.com and Self-Managed. When it is enabled and all licensed seats are already in use, GitLab blocks new billable users from being added.


Organizations, therefore, can avoid unexpected seat growth before renewal and keep seat usage aligned more closely to the number of seats they have purchased. Restricted access is designed to prevent new overages going forward, not to undo overages that already exist.


Users who do not need project or group access, such as users who authenticate through GitLab as an OpenID Connect (OIDC) provider, can be assigned the non-billable Minimal Access role. Those users can still authenticate without consuming a paid seat.


## Existing billable members are not retroactively affected


Restricted access is forward-looking. If you enable it on a group or instance that is already over its seat limit, GitLab does not downgrade, remove, or block existing billable members. Current memberships stay as they are. If there is already an overage, administrators still need to bring usage back within the purchased limit by removing billable members or purchasing additional seats.


Once seat usage is back within the subscription limit, restricted access helps prevent additional billable growth beyond that limit.


## Restricted access works better with your identity provider


A major part of the recent feature completion work was improving how restricted access behaves with identity-driven provisioning.


When restricted access is enabled and no seats are available, users provisioned through SAML, SCIM, or LDAP are no longer added directly into billable roles. Instead, GitLab assigns them the non-billable Minimal Access role. Synchronization can continue while avoiding an immediate billable overage.


This behavior is especially helpful for organizations that rely on automated provisioning and want tighter cost controls without giving up centralized identity management.


If you use GitLab as an OIDC provider and some users only need authentication rather than project or group access, assigning Minimal Access at the top-level group remains a useful pattern. Those users do not consume billable seats, and users with only Minimal Access can still be reactivated even when no seats are available.


## Dormant users no longer create silent overages


GitLab can automatically deactivate users who have had no activity for a configurable period, freeing up seats. Previously, when those users signed back in through OIDC or single sign-on (SSO), they could be silently reactivated as billable users, bypassing restricted access and creating license overages.


Now, when restricted access is active and no seats are available, dormant users who sign back in are placed in a pending approval state. Their group and project memberships are preserved, and an administrator can approve them when a seat opens up.


## Warnings, banners, and notifications are clearer


Restricted access is also easier to operate day to day.


Recent improvements added more guidance directly into the product so administrators understand what will happen before and after they hit their seat limit. Depending on the scenario, that includes:


- Contextual warnings when configuring LDAP sync, SAML group links, or SCIM provisioning while restricted access is active
- Separate in-product states for approaching the seat limit and reaching the seat limit
- Email notifications to group owners or instance administrators when users are assigned Minimal Access because no paid seats are available
- Audit visibility for Minimal Access fallback events


The goal is not just to block new billable additions, but to make that behavior easier to understand and manage.


## GitLab Self-Managed's settings cache


On GitLab Self-Managed, application settings are cached for 60 seconds by default for performance reasons.


As a result, if you switch between restricted access and user cap, some UI changes or seat-control behavior might not appear immediately. The cache refreshes automatically, and behavior becomes consistent once it does. If needed, administrators can adjust the cache interval.


See the[application settings cache documentation](https://docs.gitlab.com/administration/application_settings_cache/) .


## The difference between restricted access and user cap


Restricted access and user cap are related, but they solve different problems.


User cap puts new users into a pending approval flow for administrators or group owners to review, regardless of whether seats are still available. Restricted access is tied directly to the number of licensed seats and blocks new billable additions only when no seats remain.


In other words, user cap is an approval control. Restricted access is a seat-limit control.


They also cannot be enabled at the same time. When you enable restricted access, user cap is disabled automatically. On GitLab.com, switching from user cap to restricted access can also affect pending members, so it is worth reviewing the documented behavior before making the change.


## Get started


Restricted access is available on GitLab.com and Self-Managed.


- On GitLab.com, group Owners can enable it at **Settings > General > Permissions and group features > Seat control > Restricted access** .
- On Self-Managed, administrators can enable it at **Admin > Settings > General > New user account restrictions > Seat control > Restricted access** .
- On GitLab.com, restricted access is not available when the top-level group is shared with an external group.


If your team wants tighter control over seat growth, fewer billing surprises, and a clearer operational model for provisioning and reactivation, restricted access is worth a closer look.


## Resources


- [Restricted access documentation](https://docs.gitlab.com/subscriptions/manage_seats/#restricted-access)
- [Manage seats documentation](https://docs.gitlab.com/subscriptions/manage_seats/)
- [Application settings cache documentation](https://docs.gitlab.com/administration/application_settings_cache/)


##


Are you just managing tools or shipping innovation?


[Get your DevOps maturity score](https://about.gitlab.com/assessments/devops-modernization-assessment/)


Quiz will take 5 minutes or less
