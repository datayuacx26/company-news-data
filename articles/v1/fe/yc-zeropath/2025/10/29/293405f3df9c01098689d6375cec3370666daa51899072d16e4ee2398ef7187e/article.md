---
schema_version: "1.0.0"
document_id: "293405f3df9c01098689d6375cec3370666daa51899072d16e4ee2398ef7187e"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/breaking-authentication-unauthenticated-api-key-creation-in-better-auth-cve-2025-61928"
published_at: "2025-10-19T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:7a529d81355dbcaf1233c09dced2211c4104d30f8125f1b93124b1cdb4f2e76d"
---

# Critical Account Takeover via Unauthenticated API Key Creation in better-auth (CVE-2025-61928)

## Introduction


ZeroPath has uncovered a critical account takeover vulnerability in better-auth's API keys plugin that allows attackers to mint privileged credentials for arbitrary users, affecting a library with[~300,000 weekly npm downloads](https://www.npmjs.com/package/better-auth) . Better-auth now powers teams ranging from[fast-moving startups to enterprises like Equinor](https://github.com/better-auth/better-auth/discussions/2581) . This post details the vulnerability, exploitation methodology, and our discovery process.


The ZeroPath scanner found this bug while documenting dependency intake workflows for large organizations and building practical guidance for auditing third-party packages. In general, authentication libraries handle one of the most security-critical parts of an application, and issues in those libraries often cascade through every service that depends on them. At large enterprises, packages like these often go through long review processes before inclusion, which made it an ideal candidate for our tests.


## Better Auth and the API Keys Plugin


[better-auth](https://github.com/better-auth/better-auth) is a modern authentication framework for TypeScript applications. It uses a plugin architecture to extend functionality, including an API keys plugin that allows applications to generate and manage API keys for authentication. The plugin exposes several endpoints:


- ` /api/auth/api-key/create` to mint API keys
- ` /api/auth/api-key/update` to modify existing keys
- ` /api/auth/api-key/list` to list a user's keys


These endpoints are integrated into the authentication flow, but a subtle interaction in their authorization logic introduced an unintended security flaw.


## The Vulnerability


Inside` createApiKey` , the handler derives the user context from the session and request body:


```text
const   authRequired   =     (  ctx  .  request     ||   ctx  .  headers  )     &&     !  ctx  .  body  .  userId  ;
const   user   =   session  ?.  user   ??     (  authRequired   ?     null     :     {   id  :   ctx  .  body  .  userId     }  )  ;
```


When a request lacks a session but supplies` ctx.body.userId` ,` authRequired` becomes` false` . The handler then constructs a user object directly from attacker-controlled input and skips the "server-only" validation branch that rejects privileged fields. The execution path becomes:


1. An unauthenticated request includes a chosen` userId` in the JSON body.
2. ` authRequired` evaluates to` false` , so the handler fabricates a user object using the supplied identifier.
3. Validation that normally blocks` refillAmount` ,` rateLimitMax` ,` remaining` , and` permissions` never executes.
4. The database layer receives attacker-controlled values and proceeds with create or update operations for the victim's keys.


## Proof of Concept


The behavior is reproducible with a single unauthenticated POST to the create endpoint:


```text
curl   -X POST http://localhost:3000/api/auth/api-key/create   \
-H   'Content-Type: application/json'     \
-d   '{
"userId": "victim-user-id",
"name": "zeropath"
}'
```


The response yields a valid API key bound to the specified user. Attackers can repeat the sequence for any known or guessable account identifier.


## Impact


API keys generally outlive browser sessions and often unlock elevated automation privileges. With CVE-2025-61928, an unauthenticated adversary can mint keys for target users, bypass MFA, and script account takeovers. The issue only affects deployments that enable the API keys plugin, but those installations inherit high-risk exposure.


## Mitigation and Detection


Organizations should upgrade to better-auth version 1.3.26 or later, which remediates the flawed authorization check. After patching, rotate any API keys created through the plugin during the exposure window and invalidate unused credentials. Review application and reverse-proxy logs for calls to` /api/auth/api-key/create` or` /api/auth/api-key/update` lacking authenticated session cookies, especially where the request body sets` userId` ,` rateLimitMax` , or` permissions` values inconsistent with provisioning workflows. If logs surface ambiguous activity, reissue credentials for the affected accounts and monitor subsequent API usage for sign-ins originating from unfamiliar IPs or service tokens.


## How ZeroPath Found It


The ZeroPath team scanned better-auth's canary branch on October 1 with the ZeroPath SAST, while building dependency assessment playbooks. Our tooling ingests repositories into tree-sitter ASTs and enriches them into graphs linking call sites, control flow, request schemas, and database interactions.


For this scan, the ZeroPath tool performed its standard repository-wide analysis, including application identification and preliminary threat assessment. During analysis, it identified the API key endpoints and handlers in` packages/better-auth/src/plugins/api-key/routes/create-api-key.ts` . Dataflow analysis tracked` ctx.body.userId` through the authentication branch, noted the fabricated user object, and observed that server-only field guards did not run.


ZeroPath's analysis showed how that input flowed into the record that the handler creates:


```text
const   data  :     Omit  <  ApiKey  ,     "id"  >     =     {
/* ... */
userId  :   user  .  id  ,
rateLimitMax  :   rateLimitMax   ??   opts  .  rateLimit  .  maxRequests     ??     null  ,
remaining  :   remaining   ===     null     ?   remaining   :     (  remaining   ??   refillAmount   ??     null  )  ,
refillAmount  :   refillAmount   ??     null  ,
permissions  :   permissionsToApply  ,
}  ;


const   apiKey   =     await   ctx  .  context  .  adapter  .  create  <  Omit  <  ApiKey  ,     "id"  >  ,     ApiKey  >  (  {
model  :     API_KEY_TABLE_NAME  ,
data  ,
}  )  ;


return   ctx  .  json  (  {
...  (  apiKey   as     ApiKey  )  ,
key  ,
metadata  :   metadata   ??     null  ,
permissions  :   apiKey  .  permissions     ?     safeJSONParse  (  apiKey  .  permissions  )     :     null  ,
}  )  ;
```


The` data` object inherits the attacker-supplied` userId` and server-only fields before they're written to the DB and finally returned. The ZeroPath scanner produced similar reasoning for` updateApiKey` , since it reuses the same` authRequired` logic.


The vulnerable control flow has been present since the API keys plugin first shipped in[better-auth/better-auth#1515](https://github.com/better-auth/better-auth/pull/1515) , so every release containing the plugin has been susceptible up to version 1.3.26.


## Dependency Assessment


This vulnerability emerged while documenting a pragmatic approach to third-party dependency intake. Traditional, scalable vetting processes (checking reputation, GitHub stars, and existing CVEs) and off-the-shelf scanners can't cover business logic issues and implementation-level traps like these. Internal requirements often force companies to fall back to requiring manual review, which quickly becomes impossible to do well at scale.


ZeroPath's opt-in beta workflow extends this capability to third-party dependencies by adding automatic scans during approval and upgrade cycles. Teams that want early access can reach out for onboarding, and the same analysis that surfaced CVE-2025-61928 can enforce custom rules in those dependencies, like ensuring no PII reaches logs.


Better-auth's maintainers responded quickly and shipped a fix shortly after disclosure. We will publish a separate guide on operationalizing these kinds of assessments using ZeroPath.


## Timeline


- **October 1, 2025** – ZeroPath scanned better-auth 1.3.25 and identified the vulnerability.
- **October 2, 2025** – Issue disclosed to the maintainers.
- **October 3, 2025** – Patch released in version 1.3.26.
- **October 8, 2025** – Advisory published as[GHSA-99h5-pjcv-gr6v](https://github.com/better-auth/better-auth/security/advisories/GHSA-99h5-pjcv-gr6v) .
- **October 9, 2025** – CVE-2025-61928 assigned.


## Disclosure


CVE-2025-61928 is now public via[GitHub Security Advisory GHSA-99h5-pjcv-gr6v](https://github.com/better-auth/better-auth/security/advisories/GHSA-99h5-pjcv-gr6v) . ZeroPath coordinated disclosure with the better-auth team and verified the fix. Organizations relying on better-auth's API keys plugin should update to at least version 1.3.26.
