---
schema_version: "1.0.0"
document_id: "15c21b7852a8e8d42be58589188fcdd1f7a60fc6a87db2bdd94c97dd739d51de"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/escape-research-pii-disclosure-keycloak-cve-2026-17059/"
published_at: "2026-07-31T08:39:08+00:00"
first_seen_at: "2026-07-31T20:25:37.638956+00:00"
fetched_at: "2026-07-31T21:48:33.066422+00:00"
content_hash: "sha256:e1cc5e08fb2874c18af758ae99a53927f3802153331062f0c2af7e6ba96ef34e"
---

# Escape Research team found a PII disclosure in Keycloak. It's now CVE-2026-17059.

## Executive summary


While Keycloak got the common case right and filters its main users list down to what a restricted admin is allowed to see, Escape research team discovered an active vulnerability in Keycloak, a widely deployed open-source identity and access management server (the upstream of the Red Hat build of Keycloak), that lets a deliberately restricted admin read the personal data of users it should never be able to see:


- The main users endpoint is guarded: a support account holding only` query-users` +` view-realm` gets an empty list, exactly as intended. A sibling endpoint that lists a role's members skips the same guard and returns full user records to that same account.
- The exposed data is the full profile, username, email, first and last name, enabled and email-verified state, for every member of any role the caller can see. In a multi-team realm, an account scoped to one team can enumerate everyone else's personal data.
- No exploit chain, no injection, no crypto or memory-safety trick. Two requests from one restricted token reproduce it, and the empty result from the front door is what proves it's a real access-control gap.
- Escape reported it to the Keycloak team on July 18th, 2026. Red Hat published CVE-2026-17059 on July 24th, crediting Escape's researcher, and Keycloak remediated the issue on July 28th.
- Realms running fine-grained admin permissions v2 are not affected: the member query is filtered at the store layer. The default admin permission model (` adminPermissionsEnabled = false` ) is where the leak occurs.


## Responsible disclosure timeline


**July 18th, 2026** - Escape Research detects and reports the vulnerability to Keyloack team


**July 18th, 2026** - Keyloack acknowledges receipt


**July 24th, 2026** - CVE-2026-17059 published by[Red Hat acknowledging contribution from Escape's researcher (Orionexe)](https://access.redhat.com/security/cve/cve-2026-17059#cve-ack)


**July 28th, 2026** - Keycloak remediates the issue


**July 31st, 2026** - Disclosure from the Escape Research team


## TL;DR: Why it matters


**User-PII disclosure via` GET /roles/{role}/users` .** The main users endpoint routes every result through a helper that filters per user with` .filter(canView)` , so a restricted admin holding only` query-users` +` view-realm` gets an empty list. The role-members endpoint (` RoleContainerResource.getUsersInRole` ) maps its members straight to a representation, skipping that helper and its filter, so the same restricted admin, the one that gets` \[\]` from the front door, reads the full record (username, email, first/last name, enabled/verified state) of every user assigned to any role it can see. A helpdesk-scoped account becomes a directory of everyone's personal data.


It was confirmed end to end against a live Keycloak` 999.0.0-SNAPSHOT` on` main` at commit` 33695405ea` . A self-contained Node proof of concept lives next to this report` pocs/` . It doesn't touch the crypto or memory-safety core, which held up well. It is a Broken Access Control boundary failure. Get your own copy of the proof of concept on[Github](https://github.com/0ri0nexe/poc-keycloack) .


## What is Keycloak


Keycloak is the de-facto open-source identity and access management server, the engine behind "log in with your company account" for a vast number of self-hosted and enterprise deployments, and the upstream of the Red Hat build of Keycloak. It mints the tokens that dozens of downstream applications trust, which makes its own boundaries unusually load-bearing: whoever crosses one of them doesn't just get into Keycloak, they get into everything sitting behind it. It is also worth mentioning that Keycloak is part of the Linux Foundation, which implies a consequent place in the open-source community.


Keycloak is open source on GitHub, which is what lets us trace issues down to the line.


## CVE-2026-17059 at a glance


Field Detail


CVE ID CVE-2026-17059


Reported by Orionexe (Enzo Mongin), Escape


Affected component Keycloak admin REST API,` RoleContainerResource.getUsersInRole`


Vulnerable endpoint` GET /admin/realms/{realm}/roles/{role-name}/users`


Type Broken access control, broken object-level authorization (CWE-639)


Severity Medium


CVSS` CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N` (6.5)


Data exposed Username, email, first name, last name, enabled state, email-verified state


Preconditions An admin account holding` query-users` +` view-realm` and not` view-users` , on a realm using the default admin permission model (` adminPermissionsEnabled = false` )


Not affected Realms with fine-grained admin permissions v2 enabled, where the member query is filtered at the store layer


Confirmed on Keycloak` 999.0.0-SNAPSHOT` ,` main` at commit` 33695405ea`


Fixed in Keycloak 26.7.0


Disclosure date July 24th, 2026 (CVE published)


## One boundary, tested from the side


We reviewed the obvious surfaces first,` redirect_uri` validation, PKCE, request-object signatures, SAML, token exchange, the brute-force protector, and none of them yielded an unconditional high-severity bug: that code is deliberately hardened. So the question shifted from "is there a hole in the wall" to "does each boundary still hold when it's approached from a direction the normal flow never produces".


The direction that mattered here is sibling coverage. When the same invariant, here "a caller who may not view a user must never receive that user's data", has to be re-applied across every endpoint that returns users, the classic failure is that one of them forgets. So the test is to line the user-returning endpoints up side by side and ask each the same question from a deliberately restricted account.


The boundary has a guard in place. The guard doesn't cover the case the test walks in through.


We also tested an additional boundary and identified a critical vulnerability. In accordance with responsible disclosure practices, we will wait until the issue has been resolved before sharing further details.


## A deep-dive: The users list that filters, next to the role list that doesn't


This boundary is about **visibility** : a restricted admin should only ever receive the users it is allowed to see. Keycloak supports exactly this kind of scoped-down administrator, a helpdesk or support account granted` query-users` (so an internal tool can search/autocomplete) and` view-realm` (so it can see its realm's configuration), and *not*` view-users` . Such an account is meant to be near-blind to the user directory.


And on the front door it is. The main users endpoint routes every result through a shared helper,` UsersResource.toRepresentation` , which applies the per-user visibility filter:


```text
private Stream<UserRepresentation> toRepresentation(RealmModel realm, UserPermissionEvaluator usersEvaluator,   // UsersResource.java:567
Boolean briefRepresentation, Stream<UserModel> userModels) {
...
if (!AdminPermissionsSchema.SCHEMA.isAdminPermissionsEnabled(realm)) {
usersEvaluator.grantIfNoPermission(...);
userModels = userModels.filter(usersEvaluator::canView);   // <-- the per-user visibility filter
...
}
return userModels.map(user -> ModelToRepresentation.toRepresentation(session, user, briefRep) ...);
}


```


So a` query-users` -only admin calling` GET /admin/realms/{realm}/users` gets` \[\]` :` canView` returns false for every user it has no permission on. The guard is real and it works.


Now the sibling. Listing the members of a role goes through a *different* method,` RoleContainerResource.getUsersInRole` (` GET /admin/realms/{realm}/roles/{role-name}/users` ), and it never touches that filtering helper:


```text
public Stream<UserRepresentation> getUsersInRole(...) {           // RoleContainerResource.java:583
auth.roles().requireView(roleContainer);   // role-level check — for a realm role this is just view-realm
auth.users().requireQuery();               // generic "may query users" — not scoped to any user


RoleModel role = roleContainer.getRole(roleName);
...
return session.users().getRoleMembersStream(realm, role, firstResult, maxResults)
.map(u -> ModelToRepresentation.toRepresentation(session, u, briefRep));
// ^ maps straight to a full representation — no .filter(auth.users()::canView)
}


```


It checks two coarse things, that you may view the role (` view-realm` satisfies it for a realm role) and that you may run user queries (` query-users` ), and then maps each member **directly** to a representation with` ModelToRepresentation.toRepresentation` , skipping the` UsersResource.toRepresentation` helper and the` canView` filter that lives inside it. Two endpoints, the same data, the same invariant, and one of them forgot to apply it. So the restricted admin that gets` \[\]` from the front door reads full records through the side one:


```text
[0] restricted admin (query-users + view-realm)  → GET /admin/realms/{realm}/users?search=Secret  → 200 []
[1] same admin, same token                       → GET /admin/realms/{realm}/roles/employee/users → 200
[ {"username":"alice","email":"alice@corp.example","firstName":"Alice","lastName":"Secret","enabled":true},
{"username":"bob",  "email":"bob@corp.example",  "firstName":"Bob",  "lastName":"Secret","enabled":true} ]


```


The account that is blind to the user list at step 0 reads everyone's email and name at step 1, one role at a time.


### Files & code involved


Layer File What


Endpoint` admin/UserResource.java` (` impersonate` 385)` POST /admin/realms/{realm}/users/{id}/impersonation`


Target check (the gap)` admin/UserResource.java` (393-394) blocks **service accounts** only; nothing on admin targets


Caller check` admin/fgap/UserPermissions.java` (` canImpersonate` 410) the` impersonation` role alone returns` true`


Target policy` admin/fgap/UserPermissions.java` (` isImpersonatable` 368) **allow-all by default** unless a` user-impersonated` policy is configured


Session issued impersonation →` KEYCLOAK_IDENTITY` SSO cookie a real login session for the target


Token minting` protocol/oidc` auth + token endpoints ordinary OIDC/PKCE flow rides the SSO session into a signed token


### Is this really impactful?


It is a cross-boundary PII disclosure. The leaked representation is the full user record, username, email, first/last name, enabled and email-verified state, for every member of any role the caller can see, returned to an account that the product itself treats as unable to view users (it gets` \[\]` from the main endpoint). In a multi-team or multi-tenant realm, a support account scoped to one group can enumerate the personal data of users across every other team, exactly the segregation` view-users` is supposed to enforce. Severity is Medium` CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N` (6.5). The precondition is a restricted admin holding` query-users` +` view-realm` (a common, deliberately-minimal delegation) on a realm using the default admin permission model (` adminPermissionsEnabled = false` ; with fine-grained admin permissions v2 enabled, the member query is filtered at the store layer and this path doesn't leak).


We isolated it with two requests from one restricted token; the front-door` \[\]` is what proves this is a real access-control gap and not just "an admin listing role members":


Request (restricted admin: query-users + view-realm) Expected if the boundary holds Observed


` GET /users?search=Secret` filtered — the account may not view users` \[\]`


` GET /roles/employee/users` filtered the same way **full PII of alice + bob**


### Reproduction


The self-contained PoC is` pocs/fgap-getusersinrole-bypass/fgap_getusersinrole_poc.mjs` ; it creates a realm (default admin model), a role` employee` with two PII-bearing users, and a restricted admin holding only` query-users` +` view-realm` , then shows the` \[\]` from` /users` and the full leak from` /roles/employee/users` . Manually: grant an account just those two` realm-management` roles, confirm` GET /admin/realms/{realm}/users` returns` \[\]` , then` GET /admin/realms/{realm}/roles/{role}/users` and read the members' emails.


**The fix.** Add the per-user filter the users list already applies, exactly as the fixed sibling` getRoleGroupMembers` (commit` dc0f68bd23` ) did:


```text
.filter(auth.users()::canView)   // <-- the one missing line
.map(u -> ModelToRepresentation.toRepresentation(session, u, briefRep));


```


## What did Escape discover?


We didn't find it on our own - we got help from Escape's platform underlying algorithm.


This is the kind of gap that's invisible in a code review, one endpoint of several remembers the filter, one forgets, and exactly the kind automation is built to catch, because a scanner doesn't reason about the endpoints that are right; it asks every endpoint the same question and compares the answers.


The platform authenticates as a deliberately restricted admin, enumerates the routes that return users from the observed API surface, and asserts an object-level authorization invariant: an identity the canonical users listing returns nothing to must not receive user records from any sibling route. When the role-members endpoint hands that identity full profiles, email, name, status, the invariant breaks and the finding is surfaced automatically, classified as broken object-level authorization, with the request/response pair as evidence.


The same call scoped to a user the account is allowed to see returns nothing and raises nothing, so the check catches the real leak without firing on legitimate traffic; and because it runs on every user-returning endpoint the scan reaches, the "one endpoint forgot the filter" failure mode is caught wherever it appears, and stays caught in CI as the API grows.


---


## Conclusion


This isn't an exotic exploit. It's a missing per-user filter next to an endpoint that has it. A Broken Access Control issue, reproducible with a short script and an ordinary position, no memory-safety trick, no crypto break.


What makes it worth writing up is the failure mode. Keycloak built the boundary and got the common case right, filter the users list down to what a restricted admin may see, and the boundary silently doesn't extend to a sibling path an attacker can choose: the same data reached through the role's member list instead of the users list. Boundaries like these don't announce where they stop holding; you find the edge by walking up to it from the side the happy path never does. Line up the guard that fires against the case it lets through, the users list filtered but the role's member list not, and the wall that looked solid turns out to have a door in it.


If you want to test for this type of boundaries, you can also[book a walkthrough with Escape's product expert.](https://escape.tech/book-a-demo)


---
