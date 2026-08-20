---
schema_version: "1.0.0"
document_id: "bdb7ea9ad83ea18de485da264c357d6404a1de94d6a86084744f0e6b5e9c372a"
company_key: "yc-propelauth"
company: "PropelAuth"
source_id: "yc-propelauth-news-import-52b3c4ba8ae4"
canonical_url: "https://www.propelauth.com/post/what-is-scim"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-25T20:01:02.470883+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:96bd21df02359771e33d8916c34321c4f3e1132373f796ababd51b2f066607ad"
---

# What is SCIM? A Practical Guide for B2B SaaS Teams

**The TL;DR**


- SCIM (System for Cross-domain Identity Management, pronounced like "skim") is an open standard ([RFC 7643](https://datatracker.ietf.org/doc/html/rfc7643?ref=propelauth.mymidnight.blog) /[7644](https://datatracker.ietf.org/doc/html/rfc7644?ref=propelauth.mymidnight.blog) ) that lets an identity provider like Okta, Microsoft Entra ID, or JumpCloud automatically create, update, and deactivate user accounts in your application.
- Mechanically, it's a REST API with JSON payloads. Your app exposes SCIM endpoints, and your customer's IdP calls them whenever their employee directory changes.
- SAML and OIDC are the login method. SCIM is for syncing employee information. They're used together, not instead of each other.
- The reason enterprise IT teams actually ask for it: **deprovisioning** . When an employee leaves, their access is revoked automatically from every product the Identity Provider manages.


If you've spent any time selling B2B software to larger companies, you've probably had a conversation like this:


> **Prospect's IT team:** "When someone leaves our company, their access needs to be revoked immediately."
>
>
> **You:** "...define immediately?"


What they're asking for is SCIM. This post is about what SCIM actually *is* : the protocol, the mechanics, and the edge cases. Once you understand what's happening on the wire, every other SCIM conversation (with prospects, auditors, or your own team) gets easier.


## The definition #


SCIM stands for **System for Cross-domain Identity Management** . It's the open standard that automates the full lifecycle of user accounts between an identity provider (IdP) and the applications a company uses.


In plain terms: SCIM is how a company's IT department controls who has access to every tool their employees use, without logging into each tool individually.


Let's say a customer of yours sets up your product in their Identity Provider and enables SCIM. When a new employee joins the customer - they appear in your product. Someone changes roles - your product reflects it. Someone leaves - their access is cut, whether or not they ever log in again.


That last case is the whole ballgame, and it's worth understanding *why* . Which brings us to how the protocol works.


## How the SCIM protocol actually works #


In SCIM terminology, your application is the **service provider** and your customer's IdP is the **client** . You expose a set of REST endpoints, the IdP holds an API key you issued, and it calls you whenever something changes in the directory.


The core endpoints you're expected to serve:


Endpoint What it's for


` /Users` Create, read, update, and deactivate user accounts


` /Groups` Sync group membership (often mapped to roles or teams)


` /Schemas` Describe the attributes your app supports


` /ServiceProviderConfig` Advertise which SCIM features you implement


` /ResourceTypes` List the resource types available (Users, Groups)


A provisioning request looks like this:


```text
POST /scim/v2/Users
Authorization  :   Bearer <scim-api-key>


{
"schemas"  :     [  "urn:ietf:params:scim:schemas:core:2.0:User"  ]  ,
"userName"  :     "maria@customer.com"  ,
"name"  :     {
"givenName"  :     "Maria"  ,
"familyName"  :     "Chen"
}  ,
"emails"  :     [  {     "value"  :     "maria@customer.com"  ,     "primary"  :     true     }  ]  ,
"active"  :     true
}
```


Your app creates (or links) the user and responds with the created resource, including an` id` the IdP will use to reference this user forever after. When Maria changes departments, the IdP sends an update. When she leaves the company, it flips` active` to` false` , and your app is expected to lock her out and, ideally, kill her live sessions too.


Three protocol details that surprise people the first time:


**Updates come in two flavors.** Some IdPs send` PUT` requests with the complete user object. Others send` PATCH` requests containing only what changed. And PATCH itself isn't consistent: sometimes you get a field path and a value, sometimes a JSON object with instructions like` replace` or` remove` and no path at all. Your implementation has to handle all of it.


**Reads matter as much as writes.** IdPs regularly call` GET /Users?filter=...` to check who already exists before provisioning, to reconcile state, or to re-fetch a user "just in case." Many of these requests require no action from you, but they all require a spec-compliant response.


**It's not always about users.** IdPs will also query your schemas and resource types, and sync group membership through` /Groups` . Group sync is how "put everyone in the Engineering group into the Admin role" works without anyone touching your app.


## The parts the spec doesn't tell you #


The SCIM spec is detailed in some places and surprisingly vague in others, and every IdP speaks it with a slightly different accent. A few real-world examples:


- **Entra's syncs aren't immediate.** There's often a 30-40 minute delay between an IT admin granting access in Microsoft Entra ID and the SCIM provision request reaching your app. A user can legitimately log in via SSO before their SCIM record exists, so your app needs a policy for that window.
- **Okta's OIDC + SCIM setup uses two separate applications.** If a customer grants a user access to the SSO app but not the SCIM app, that user can log in but will never be provisioned. (SAML + SCIM doesn't have this problem.)
- **Types drift.** Entra has been known to send strings where you'd expect booleans, depending on when the account was created.
- **Custom attributes are the wild west.** The spec defines a core schema and an enterprise extension, but says little about where *your* custom fields should live. Ask five customers to send` favoriteSport` and you'll receive` favoriteSport` ,` fav_sport` ,` FavoriteSport` , and a fully-qualified schema URN.
- **Support for roles varies wildly.** Many B2B applications have a concept of roles or RBAC, and while some IdPs support this concept, the way they send that information is completely custom.


None of this is a reason to fear SCIM. It's a reason to treat "we'll just implement the spec" estimates with suspicion.


## SCIM vs. JIT provisioning #


If your app supports enterprise SSO today, you likely already do **just-in-time (JIT) provisioning** : when a user logs in via SAML or OIDC, you create or update their account with whatever the IdP sent in the login assertion.


JIT is genuinely fine for a while, and the difference from SCIM comes down to one word: *proactive* .


With JIT, you only learn about users when they log in. Suppose an employee gets married, changes their name and email, and doesn't log into your product for a month. You show stale data for a month. Annoying, survivable. Now suppose an employee *leaves the company* and never logs in again. JIT never tells you. Their account sits there, active, holding whatever access and API keys it had. Nobody notices until a security review does.


SCIM closes that gap by pushing changes to you the moment they happen in the directory, login or no login. In practice most products run both: SCIM as the source of truth for who should exist, and JIT as a graceful fallback for timing gaps (like Entra's sync delay above).


## SCIM vs. SAML and OIDC #


The short version: SAML and OIDC are **authentication** protocols. They run at login time and decide whether a user can sign in. SCIM is a **provisioning** protocol. It runs continuously in the background and decides which accounts exist and whether they're active. A SCIM-provisioned user still logs in via whichever of SAML or OIDC their company configured, so you'll almost always deploy SCIM alongside one of them, and most IdPs require SSO to be set up first.


For a deeper walkthrough of how the two layers interact, with examples of what SAML alone misses, see[Understanding SAML and SCIM](https://www.propelauth.com/post/understanding-saml-and-scim?ref=propelauth.mymidnight.blog) .


## Do you need SCIM? #


Short answer: not on day one, and almost certainly yes once you sell to mid-market or enterprise. It tends to arrive late in a sales cycle, phrased as a security-questionnaire checkbox, and it can quietly stall a deal that seemed almost closed. We wrote a full breakdown of the buying triggers (company size, compliance drivers, and how to tell you're at that stage) in[SCIM Provisioning: What It Is and When You Need It](https://www.propelauth.com/post/scim-provisioning-what-it-is-and-when-you-need-it?ref=propelauth.mymidnight.blog) .


## What supporting SCIM involves #


If you build it yourself, the work breaks down into:


1. **The endpoints** -` /Users` ,` /Groups` ,` /Schemas` ,` /ServiceProviderConfig` , each with its own quirks
2. **Security** - these endpoints create and delete users, so you'll issue and rotate per-customer API keys
3. **IdP variance** - Okta, Entra, OneLogin, and JumpCloud all behave differently (see above)
4. **Timing and conflict policies** - SSO-before-SCIM windows, pre-existing accounts, email/ID mismatches
5. **Custom attribute mapping** - deciding where customer-specific fields live, and tolerating customers who don't follow your instructions
6. **Debugging through someone else's IT admin** - the feedback loop for "your IdP sent us something weird" runs through your customer, so every iteration is slow


None of it is impossible. It's a few weeks-to-months of undifferentiated work whose best possible outcome is that nobody ever thinks about it. If you want the implementation-level detail, we've written a complete guide to[adding SCIM to your product](https://byo.propelauth.com/post/adding-scim-to-your-product?ref=propelauth.mymidnight.blog) , and if you'd rather keep your existing auth and bolt SCIM on top, that's exactly what[PropelAuth BYO's SCIM support](https://byo.propelauth.com/post/scim-but-better?ref=propelauth.mymidnight.blog) does with a single catch-all route.


## Frequently asked questions #


**Does SCIM replace SAML?** No. SAML (or OIDC) handles authentication, and SCIM handles the account lifecycle. They're used together, and most setups require enterprise SSO to be configured before SCIM.


**Is SCIM the same as directory sync?** Effectively, yes. "Directory sync" is the friendlier product name, and SCIM is almost always the protocol underneath.


**SCIM 1.1 vs SCIM 2.0: which matters?** SCIM 2.0. It's what every major IdP uses today. You may meet 1.1 in older enterprise environments, but new implementations should target 2.0.


**Which identity providers support SCIM?** All the ones your enterprise customers use: Okta, Microsoft Entra ID (formerly Azure AD), Google Workspace, OneLogin, JumpCloud, Rippling, and most others.


**Is SCIM only for provisioning users, or roles too?** Both. Group membership synced via` /Groups` (or attributes on the user) is commonly mapped to roles in the application, so IT admins can control not just who gets access but what they can do, driven by their IdP group structure.


**How long does SCIM take to implement?** From scratch: typically weeks to months, most of it spent on IdP-specific edge cases and testing loops with customer admins. With a provider handling the protocol: hours to days.


## How SCIM fits into PropelAuth #


PropelAuth is built around organizations as a core concept, and SCIM slots directly into that model: provisioned users are automatically created in the right organization, with properties and roles mapped from the IdP's attributes. Code you've already written against organizations (membership checks, role checks, permissions) works identically whether a user was invited manually, logged in via SSO, or arrived via SCIM. No new code paths when you close your first enterprise customer.


To see how it works, check out the[SCIM docs](https://docs.propelauth.com/overview/authentication/scim?ref=propelauth.mymidnight.blog) , or reach out atsupport@propelauth.com . We think about this stuff so you don't have to.
