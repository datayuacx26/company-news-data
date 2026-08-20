---
schema_version: "1.0.0"
document_id: "b1f137d5764a5c777348ffdcd5c7f53f99575825d136b3429fe3182d8d66d1c8"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/control-who-sees-customer-feedback"
published_at: null
first_seen_at: "2026-07-31T23:56:58.141334+00:00"
fetched_at: "2026-07-31T23:56:59.512489+00:00"
content_hash: "sha256:c98e08aa768dad47ee1636a8dee9f134071b5ab5404389928c772dceb6cf6073"
---

# How Do You Control Who Can See Which Customer Feedback in an Analytics Platform?

Customer feedback is one of the few enterprise datasets that almost everyone wants and almost nobody wants to hand out freely. Support wants ticket verbatims. Product wants[app reviews](https://getthematic.com/insights/review-analysis) . A regional director wants to know what their own team said in the engagement survey. Meanwhile the security reviewer is asking why a marketing contractor can open a dataset full of names and account numbers. Both instincts are right. A platform that can't serve them at once ends up serving neither.


You control who sees which customer feedback with three separate mechanisms, not one. Thematic separates authentication, authorization, and provisioning. Authentication proves who a user is, through single sign-on (SSO). Authorization decides what that user is allowed to do, through roles and granular permissions scoped to individual datasets and dashboards. Provisioning creates and removes accounts automatically from your identity provider. Conflating those three is the most common reason access control in a feedback platform quietly fails.


Below is what each layer does, what enterprise reviewers require, how Thematic handles each piece, and the limits worth planning around.


## What "access control" actually means in a feedback platform


Access control is the set of technical controls that decide which users can reach which feedback data and what they can do with it. It gets treated as a single feature, which is why evaluations go wrong. It has five dimensions, and a platform can be strong on some and absent on others:


- **Authentication.** Proving a user is who they claim to be, usually by federating to your existing identity provider.
- **Authorization.** Deciding what an authenticated user may see and do, usually through roles and permissions.
- **Data scoping.** Restricting permissions to specific datasets, dashboards, or views rather than the whole account.
- **Provisioning and deprovisioning.** Creating accounts when people join and removing access when they leave, ideally without a human ticket.
- **Auditability.** Producing a record of who accessed what, and of who granted whom access.


Buyers most often skip data scoping. It's the one that decides whether feedback can be shared widely at all.


## What enterprise feedback access control typically requires


**Least privilege as a documented control.** Enterprise security reviewers are usually mapping vendors against a framework. In the National Institute of Standards and Technology (NIST) publication SP 800-53 Revision 5, the relevant control is[AC-6, Least Privilege](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final) . It requires allowing only the authorized accesses users need to accomplish their assigned tasks, and it's required at both the MODERATE and HIGH baselines. In practice that means granular permissions tied to documented job functions, enforced technically rather than by policy.


**Federated authentication.** Security Assertion Markup Language (SAML) 2.0,[the OASIS standard](https://www.oasis-open.org/standard/saml/) , is the protocol most enterprise identity providers speak. Federating authentication means credentials live in your identity provider, not in the vendor's user table. Revoking someone in Okta or Microsoft Entra ID revokes them everywhere.


**Automated lifecycle management.** System for Cross-domain Identity Management (SCIM) was standardized by the Internet Engineering Task Force as[RFC 7644](https://www.rfc-editor.org/rfc/rfc7644) in September 2015. Its stated intent is to reduce the cost and complexity of user management operations. It matters because manual deprovisioning is the step organizations skip.


**Confidentiality obligations when feedback contains personal data.** Open-ended feedback routinely contains names, contact details, and account references.[Article 32](https://gdpr-info.eu/art-32-gdpr/) of the General Data Protection Regulation (GDPR) requires technical and organizational measures ensuring the ongoing confidentiality of processing systems. Article 32(4) requires that anyone acting under the authority of a controller or processor who has access to personal data processes it only on the controller's instructions. Access scoping is how that obligation is met in practice.


**An audit trail.** Reviewers ask not just who can see the data, but who granted them the ability to.


## Where most feedback tools fall short


The gap is rarely authentication. Most platforms support SSO. The gaps show up further down:


- Permissions apply to the whole account rather than to individual datasets, so granting one dashboard grants everything.
- New datasets are silently inherited by everyone who already had broad access, widening exposure with no decision made.
- SSO is offered but provisioning is not, so accounts are still created and deleted by hand.
- Roles are fixed at a handful of preset tiers with no way to grant one narrow extra permission.
- Individually granted permissions are invisible in the interface, so nobody can audit who has what.


One question separates a real implementation from a checkbox: ask the vendor to show you, live in the product, what happens to an existing user's access when you connect a new dataset. If the answer is "they see it automatically" and there's no way to choose otherwise, the platform can't support least privilege.


## How Thematic controls who sees which feedback


**Roles plus custom permissions.** Thematic grants access two ways. Permissions can be bundled into reusable roles assigned to many users, or granted directly to one user as custom permissions. A user's effective access is the aggregate of all their roles plus any custom permissions, and assigning multiple roles grants the union of those permissions. Thematic ships with four default roles: Dataset Admin, User Admin, Analyst, and Viewer. Editing roles is available on enterprise plans. Custom permissions can be layered on top of a role, for example the Analyst role plus "Manage themes" on a single dataset. A "show custom permissions" toggle surfaces them so they can be audited.


**Permissions scoped to individual datasets and dashboards.** Thematic separates three permission families:


Permission family Permissions available Scoping


Dataset Analysis, Answers, Download, Manage, Manage themes, Upload data Across all datasets and views, or per dataset


Dashboard View, Manage, Manage digests Across all dashboards, or per dashboard


Admin Manage users, Manage roles, Manage integrations, Create datasets, Create dashboards, Access usage metrics Organization-wide


The inheritance rule is the detail worth writing down during an evaluation. Granting a permission across all datasets or dashboards means future ones are inherited automatically. Granting it per object means future ones get no access at all. That single choice is what lets a Thematic administrator open feedback to hundreds of people without every new data source silently widening what those people can reach.


**Single sign-on that doesn't pretend to be authorization.** Thematic supports SAML 2.0 and publishes an app in the Okta app store. SSO establishes who a user is, not what they can do, so permissions stay a separate decision. Administrators configure it themselves under Manage users, then Manage SSO, entering the service-provider Entity ID, the SSO and SLO URLs, and the x509 signing certificate, with a metadata URL available to shorten setup. Because credentials live in your identity provider, invalidating them there revokes Thematic access immediately.


**Just-in-time provisioning (JIT) for first login.** With JIT enabled, a user who authenticates through SAML but has no Thematic account is created on first login and assigned the configured default JIT role. Without it, users without accounts see an error and have to contact an administrator.


**SCIM v2 for the full account lifecycle.** Separately from SSO, Thematic supports the SCIM v2 API using a long-lived bearer token issued by your customer success representative, with region-based and organization-based connector URLs. This is the layer that handles deprovisioning, which JIT doesn't.


**Immutable access logging.** All access to Thematic's analytic tools runs through a single client portal, which is also the only channel for uploading and downloading data. Access logs are aggregated centrally, kept immutable for at least 90 days, and capture database access and login activity. One entry point makes the log complete.


**Three limits worth planning around.** JIT provisioning fires only on a user's first login, so changing the default JIT role later doesn't alter anyone already created. The "Manage users" permission effectively grants full data access, because a user who can manage users can grant themselves anything. Treat it as the most privileged permission in the product. And deleting a user revokes their access to that organization but doesn't unsubscribe them from email digests, so digests need checking separately during offboarding.


## What this looks like when the goal is more access, not less


Access control is usually framed as restriction. In[feedback analytics](https://getthematic.com/insights/customer-feedback-analysis-with-text-analytics) the problem is often the opposite: the data is locked in a way nobody chose, and the people closest to the customer can't see it.


[Community Health System](https://getthematic.com/insights/community-health-systems-actionable-insights) , a not-for-profit healthcare network serving California's central San Joaquin Valley, ran an annual employee engagement survey collecting open-ended feedback from staff across 250 departments. Access to those comments was largely limited to VPs. Directors and middle managers had to book time with the experience team and walk through verbatims on a call. At roughly an hour of preparation per department, that came to over 250 hours and approximately $15,000 in staff time per survey cycle. Working with Thematic, the Organizational Development team delivered standardized one-page reports to all 250 departments in a single three-day sprint.


"The only reason we built these single-page reports is because we had Thematic," said Alexandra Clifton, Organizational Development Partner at Community Health System. "Before, we were doing everything in an Excel spreadsheet and just generating information and having conversations with people in real time."


[Watercare](https://getthematic.com/insights/watercare-customer-excellence-with-thematic) , New Zealand's largest utility, serves 1.7 million people in Auckland. It distributes Thematic reporting by role rather than by request. Team leaders, business area managers, and the chief customer officer each receive regular reports covering overall results and their own focus areas. Each strand of its[Voice of the Customer program](https://getthematic.com/insights/voice-of-customer-strategy) has its own dashboard, and field staff were given direct access to customer responses.


Neither outcome is possible without scoping. You can only open feedback up this far if you can also control what each person sees.


## A buyer's checklist for feedback access control


1. Can permissions be granted per dataset and per dashboard, not just account-wide?
2. When a new dataset is connected, do existing users gain access automatically, and can that be turned off?
3. Which SSO protocol is supported, and is there a listing in your identity provider's app catalog?
4. Is SCIM supported for automated deprovisioning, or is SSO the only integration?
5. Can one extra permission be granted to a single user without creating a new role?
6. Can an administrator see which permissions were granted individually rather than through a role?
7. How long are access logs retained, and are they immutable?


## The short answer


You control who sees which customer feedback by keeping authentication, authorization, and provisioning separate, then scoping permissions to individual datasets and dashboards rather than the whole account. Thematic does this with four default roles plus granular custom permissions, per-object scoping, SAML 2.0 single sign-on with an Okta app-store listing, JIT provisioning, and the SCIM v2 API. The fastest test of any vendor: ask what happens to existing users' access when you connect a new dataset, and insist on seeing it live.
