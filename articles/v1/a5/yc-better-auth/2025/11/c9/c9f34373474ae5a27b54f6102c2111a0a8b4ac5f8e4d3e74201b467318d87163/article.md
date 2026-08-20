---
schema_version: "1.0.0"
document_id: "c9f34373474ae5a27b54f6102c2111a0a8b4ac5f8e4d3e74201b467318d87163"
company_key: "yc-better-auth"
company: "Better Auth"
source_id: "yc-better-auth-rss-ab6aa45cffa8"
canonical_url: "https://better-auth.com/blog/1-4"
published_at: "2025-11-21T00:00:00+00:00"
first_seen_at: "2026-07-24T18:52:17.785032+00:00"
fetched_at: "2026-07-28T20:55:16.082008+00:00"
content_hash: "sha256:1a8a95b2bf3eee91b47e97832b8ed8799724d45e1238104eb7214ff370b9af3e"
---

# Better Auth 1.4

## Better Auth 1.4 Release


We're excited to announce the release of Better Auth 1.4. This release includes several new features, performance and security improvements.


To upgrade, run:


```text
npm   install   better-auth@1.4
```


---


## 🚀 Highlights


### Stateless Auth


You can now enable stateless session management without any database by omitting the` database` option in your auth config.


[👉 Read more about stateless auth](https://better-auth.com/docs/concepts/session-management#stateless-session-management)


```text
import   { betterAuth }   from   "better-auth"  ;


export   const   auth   =   betterAuth  ({
// No database configuration
socialProviders: {
google: {
clientId: process.env.  GOOGLE_CLIENT_ID  ,
clientSecret: process.env.  GOOGLE_CLIENT_SECRET  ,
},
},
});
```


It also supports getting` accessToken` ,` accountInfo` and` refreshToken` endpoints without any database.


```text
import   { authClient }   from   "@/lib/auth-client"  ;


const   accessToken   =   await   authClient.  getAccessToken  ();


// get account info
const   accountInfo   =   await   authClient.  accountInfo  ();
```


---


### SCIM Provisioning


SCIM makes managing identities in multi-domain scenarios easier to support via a standardized protocol.


[👉 Read more about SCIM provisioning](https://better-auth.com/docs/plugins/scim)


```text
import   { scim }   from   "@better-auth/scim"  ;
import   { betterAuth }   from   "better-auth"  ;


export   const   auth   =   betterAuth  ({
plugins: [  scim  ()]
});
```


---


### Custom State for OAuth Flows


You can now provide any additional state through the OAuth flow.


[👉 Read more about custom state for OAuth flows](https://better-auth.com/docs/concepts/oauth#passing-additional-data-through-oauth-flow)


```text
import   { authClient }   from   "@/lib/auth-client"  ;


await   authClient.signIn.  social  ({
provider:   "google"  ,
additionalData: {
referralCode:   "ABC123"  ,
source:   "landing-page"  ,
},
});
```


Access additional data in any hook, middleware, or endpoints


auth.ts


```text
import   { betterAuth }   from   "better-auth"  ;
import   { getOAuthState, createAuthMiddleware }   from   "better-auth/api"  ;


const   auth   =   betterAuth  ({
hooks: {
before:   createAuthMiddleware  (  async   (  ctx  )   =>   {
const   additionalData   =   await   getOAuthState  <{   referralCode  :   string  ,   source  :   string   }>();
console.  log  (additionalData);
})
}
});
```


---


### Database Joins Improvements (experimental)


Better-Auth now supports database joins, improving over 50 endpoints by 2 to 3x in latency. This is achieved by using database joins under the hood.


To enable, simply set the` joins` flag to` true` under` experimental` in your auth config. Then, re-run migrations or schema generation to get the updated schema which supports joins!


[👉 Read more about database joins](https://better-auth.com/docs/concepts/database#experimental-joins)


```text
import   { betterAuth }   from   "better-auth"  ;


export   const   auth   =   betterAuth  ({
experimental: {
joins:   true  ,
},
});
```


Although joins are experimental, they are supported by all adapters and will be enabled by default in the next release.


---


### Secondary Storage Support for API Keys


The API-Key plugin now supports secondary storage for faster key lookups!


[👉 Read more about secondary storage for API keys](https://better-auth.com/docs/plugins/api-key#storage-modes)


```text
import   { apiKey }   from   "@better-auth/api-key"  ;
import   { betterAuth }   from   "better-auth"  ;


export   const   auth   =   betterAuth  ({
secondaryStorage: {
//...
},
plugins: [  apiKey  ({ storage:   "secondary-storage"   })],
});
```


###


---


### New Default Error Page


We've revamped our error page to look much better and more informative! You can also customize the stylings of our default error page or simply provide your own path to a custom error page.


[👉 Read more about error page improvements](https://better-auth.com/docs/reference/options#onapierror)


---


### SSO Domain Verification


Domain verification allows your application to automatically trust a new SSO provider by automatically validating ownership via the associated domain:


👉[Read more about domain verification](https://better-auth.com/docs/plugins/sso#domain-verification)


```text
import   { sso }   from   "@better-auth/sso"  ;
import   { betterAuth }   from   "better-auth"  ;


export   const   auth   =   betterAuth  ({
plugins: [  sso  ({ domainVerification: { enabled:   true   } })],
});
```


---


### CLI Database Index Support


The Better-Auth CLI now supports indexing your database out of the box, which greatly improves the performance of your application.


```text
npx   auth   generate


npx   auth   migrate
```


---


### JWT Key Rotation Support


The JWT plugin now supports key rotation, allowing you to rotate your JWT keys without downtime.


```text
import   { jwt }   from   "@better-auth/jwt"  ;
import   { betterAuth }   from   "better-auth"  ;


export   const   auth   =   betterAuth  ({
plugins: [
jwt  ({
jwks: { rotationInterval:   60   *   60   *   24   *   30  , gracePeriod:   60   *   60   *   24   *   30   }
})
],
});
```


👉[Read more about JWT key rotation](https://better-auth.com/docs/plugins/jwt#key-rotation)


### Improved Generic OAuth Plugin


OAuth providers not yet supported natively will be available as pre-configured options for the generic OAuth plugin, with plans to eventually support all providers that Auth.js does.


```text
import   { betterAuth }   from   "better-auth"  ;
import   { genericOAuth }   from   "better-auth/plugins"  ;
import   { keycloak }   from   "better-auth/plugins/generic-oauth"  ;


export   const   auth   =   betterAuth  ({
plugins: [  genericOAuth  ({
config: [
keycloak  ({
clientId: process.env.  KEYCLOAK_CLIENT_ID  ,
clientSecret: process.env.  KEYCLOAK_CLIENT_SECRET  ,
issuer: process.env.  KEYCLOAK_ISSUER  ,
})
]
})
]
});
```


---


### Bundle Size Optimizations


If you're using custom adapters (like Prisma, Drizzle, or MongoDB), you can reduce your bundle size by using` better-auth/minimal` instead of better-auth. This version excludes Kysely, which is only needed when using direct database connections.


[👉 Read more about bundle size optimizations](https://better-auth.com/docs/guides/optimizing-for-performance#bundle-size-optimization)


```text
import   { drizzleAdapter }   from   "better-auth/adapters/drizzle"  ;
import   { betterAuth }   from   "better-auth/minimal"  ;
import   { db }   from   "./database"  ;


export   const   auth   =   betterAuth  ({
database:   drizzleAdapter  (db, { provider:   "pg"   }),
});
```


---


### UUID Support


We now support having UUIDs as the primary ID field type out of the box.


[👉 Read more about UUID support](https://better-auth.com/docs/concepts/database#uuids)


```text
import   { betterAuth }   from   "better-auth"  ;
import   { db }   from   "./database"  ;


export   const   auth   =   betterAuth  ({
database: db,
advanced: {
database: { generateId:   "uuid"   },
},
});
```


---


### Improved Email Change Flow


For added security, you can now require users to confirm the change via their **current** email before the verification email is sent to the new address.


[👉 Read more about improved email change flow](https://better-auth.com/docs/concepts/users-accounts#confirming-with-current-email)


```text
import   { betterAuth }   from   "better-auth"  ;


export   const   auth   =   betterAuth  ({
user: {
changeEmail: {
enabled:   true  ,
sendChangeEmailConfirmation  :   async   ({   user  ,   newEmail  ,   url  ,   token   },   request  )   =>   {
sendEmail  ({
to: user.email,   // Sent to the CURRENT email
subject:   'Approve email change'  ,
text:   `Click the link to approve the change to ${  newEmail  }: ${  url  }`
})
}
}
}
});
```


---


### Device Authorization Plugin


Full support for the OAuth 2.0 Device Authorization Grant (RFC 8628), allowing users to sign in on input-constrained devices like Smart TVs and gaming consoles.


[👉 Read more about Device Authorization](https://better-auth.com/docs/plugins/device-authorization)


```text
import   { betterAuth }   from   "better-auth"  ;
import   { deviceAuthorization }   from   "better-auth/plugins"  ;


export   const   auth   =   betterAuth  ({
plugins: [
deviceAuthorization  ()
]
});
```


---


### Cookie-based Account Storage


You can now store user account data in a signed cookie, which is perfect for stateless applications or when you want to defer database persistence.


```text
export   const   auth   =   betterAuth  ({
account: {
storeAccountCookie:   true
}
});
```


---


## ⚠️ Breaking Changes


We recommend going through each breaking change to ensure a smooth upgrade.


### Existing Auth Flows


Minor auth flow changes to make everything smoother, faster, and more reliable.


-


**Change-Email Flow** : We've removed` sendChangeEmailVerification` from the` changeEmail` flow to use` emailVerification.sendVerificationEmail` instead.


-


**Forgot Password Flow** :` authClient.forgotPassword` is now` authClient.requestPasswordReset` .


-


**Account Info Endpoint** : The` accountInfo` endpoint has changed from` POST /account-info` to` GET /account-info` .


This means any auth.api calls must change from:


```text
auth.api.  accountInfo  ({
body: {
// ...
}
});
```


to:


```text
auth.api.  accountInfo  ({
query: {
// ...
}
});
```


### Plugin Options Changes


Multiple plugin options that accept` request` as an argument for callback functions have now replaced request with` ctx` instead. Passkey plugin is now moved to its own package, and API Key mock-sessions are now disabled by default.


-


**Passkey Plugin** : The passkey plugin is now moved to its own package.


Install it today:


```text
npm   install   @better-auth/passkey
```


-


**Email OTP Plugin** : Plugin options for` sendVerificationOTP` and` generateOTP` now use` ctx` instead of` request` in its callback function. You can still access request via` ctx.request` .


-


**Magic Link Plugin** : Plugin options for` sendMagicLink` now uses` ctx` instead of` request` for one of its callback options. You can still access request via` ctx.request` .


-


**Phone Number Plugin** : Plugin options for` sendOTP` ,` sendPasswordResetOTP` &` callbackOnVerification` have now replaced request with` ctx` in one of its callback options. You can still access request via` ctx.request` .


-


**Organization Plugin** : Plugin options for` teams.defaultTeam.customCreateDefaultTeam` &` teams.maximumTeams` have now replaced request with` ctx` for one of its callback options. You can still access request via` ctx.request` .


-


**OIDC Plugin** : The field` redirectURLs` have been updated to` redirectUrls` , this requires database migration.


-


**API Key Plugin** : Mock-sessions by api-keys are now disabled by default, and should be enabled through the auth-config first.


### Auth Config Changes


Database configuration options changes.


- **` advanced.generateId` removed** : The long-deprecated` advanced.generateId` has been removed. Please use` advanced.database.generateId` instead.
- **` advanced.database.useNumberId` moved** :` advanced.database.useNumberId` has been moved to` advanced.database.generateId: "serial"` instead.


### Tanstack Start


The Better-Auth offered` reactStartCookies` plugin is now moved to` tanstackStartCookies` plugin.


```text
- import { reactStartCookies } from "better-auth/react-start";
+ import { tanstackStartCookies } from "better-auth/tanstack-start";


export const auth = betterAuth({
-    plugins: [reactStartCookies()],
+    plugins: [tanstackStartCookies()],
});
```


### Expo


With the new pooling features for` useSession` on Expo, you're required to install` expo-network` :


```text
npx   expo   install   expo-network
```


### Cloudflare Runtime


Better Auth uses AsyncLocalStorage for async context tracking. To enable this in Cloudflare Workers, add the` nodejs_compat` flag to your` wrangler.toml` :


wrangler.toml


```text
compatibility_flags = [  "nodejs_compat"  ]
compatibility_date =   "2024-09-23"
```


Alternatively, if you only need AsyncLocalStorage support:


wrangler.toml


```text
compatibility_flags = [  "nodejs_als"  ]
```


In the next major release, we will assume AsyncLocalStorage support by default, so this configuration will be necessary.


---


## ✨ More Features


- **Cookie Chunking** : No more cookie size exceeding errors.
- **MongoDB String IDs** : Support for using String IDs instead of ObjectIDs in MongoDB.
- **JWE Cookie Cache** : Session store cookie cache now uses JWE by default for enhanced security.
- **OIDC RP-Initiated Logout** : Trigger logout at the OIDC provider.
- **Server-side IP Detection** : Automatic client IP detection utility.
- **Custom OTP Verification** : Allow custom logic for OTP verification in the Phone Number plugin.
- **Client` disableSignal`** : Option to disable default abort signal behavior in the client.
- **` AuthClient` Type Helper** : New type helper for better client-side type inference.
- **New Providers** : Polar and Paybin OAuth providers added.
- **ESM Only** : Better-Auth is now 100% ESM.
- **Session Refetching** : Support for polling & on-window-focus refetching.
- **Cloudflare Workers** : CLI support for virtual module imports.
- **Error Documentation** : Common OAuth flow errors are now[documented](https://better-auth.com/docs/reference/errors) .
- **Last-Login-Method** : Support for tracking SIWE & Passkey.


---


## 🐛 Bug fixes & Improvements


This release includes over 250+ bug fixes addressing issues across all areas:


- **Adapter Fixes** : Improved query handling, better type transformations, UUID column types, and resolved issues with nullable foreign keys.
- **Session Management** : Fixed session refresh triggers, improved cookie handling, and better cache management.
- **OAuth** : Fixed state management, improved callback handling, and resolved cross-origin issues.
- **Organization** : Fixed member deletion, team management, permission checks, and inference issues.
- **Two-Factor** : Improved TOTP error messages, fixed backup code encryption, and better device trust handling.
- **Email OTP:** Prevented duplicate verification emails and improved session signal triggers.
- **SSO** : Fixed SAML callback handling, improved OIDC scope fallback, and better consent enforcement.
- **API-Keys** : Fixed usage tracking and improved request validation.
- **Phone Number** : Prevent unauthorized phone number updates and improved verification flow.
- And many more!


---


A lot of refinements to make everything smoother, faster, and more reliable. 👉[Check the full changelog](https://github.com/better-auth/better-auth/releases/tag/v1.4.0)


## ❤️ Contributors


Thanks to all the contributors for making this release possible!


[@zain](https://github.com/zain)[@dandamian](https://github.com/dandamian)[@hartbit](https://github.com/hartbit)[@rmarscher](https://github.com/rmarscher)[@kaandok](https://github.com/kaandok)[@acusti](https://github.com/acusti)[@rbayliss](https://github.com/rbayliss)[@kevcube](https://github.com/kevcube)[@bsklaroff](https://github.com/bsklaroff)[@jonathansamines](https://github.com/jonathansamines)[@mburumaxwell](https://github.com/mburumaxwell)[@erquhart](https://github.com/erquhart)[@dmmulroy](https://github.com/dmmulroy)[@iRoachie](https://github.com/iRoachie)[@mohebifar](https://github.com/mohebifar)[@thomasmol](https://github.com/thomasmol)[@borgoat](https://github.com/borgoat)[@XiNiHa](https://github.com/XiNiHa)[@Berndwl](https://github.com/Berndwl)[@xiaoyu2er](https://github.com/xiaoyu2er)[@ebalo55](https://github.com/ebalo55)[@udnes99](https://github.com/udnes99)[@himself65](https://github.com/himself65)[@dvanmali](https://github.com/dvanmali)[@AlexStrNik](https://github.com/AlexStrNik)[@ThibautCuchet](https://github.com/ThibautCuchet)[@zbeyens](https://github.com/zbeyens)[@xuchenhao001](https://github.com/xuchenhao001)[@tnkuehne](https://github.com/tnkuehne)[@Tobix99](https://github.com/Tobix99)[@hieudien14310](https://github.com/hieudien14310)[@3ddelano](https://github.com/3ddelano)[@bdkopen](https://github.com/bdkopen)[@jslno](https://github.com/jslno)[@chhoumann](https://github.com/chhoumann)[@mrl5](https://github.com/mrl5)[@tgrassl](https://github.com/tgrassl)[@hyoban](https://github.com/hyoban)[@redoh](https://github.com/redoh)[@gregtjack](https://github.com/gregtjack)[@ouwargui](https://github.com/ouwargui)[@kanarian](https://github.com/kanarian)[@natetewelde](https://github.com/natetewelde)[@Badbird5907](https://github.com/Badbird5907)[@max-programming](https://github.com/max-programming)[@eni4sure](https://github.com/eni4sure)[@frectonz](https://github.com/frectonz)[@ephraimduncan](https://github.com/ephraimduncan)[@Eazash](https://github.com/Eazash)[@ahmedriad1](https://github.com/ahmedriad1)[@Velka-DEV](https://github.com/Velka-DEV)[@Kinfe123](https://github.com/Kinfe123)[@zy1p](https://github.com/zy1p)[@okisdev](https://github.com/okisdev)[@surafel58](https://github.com/surafel58)[@DevDuki](https://github.com/DevDuki)[@TheUntraceable](https://github.com/TheUntraceable)[@nakasyou](https://github.com/nakasyou)[@JagritGumber](https://github.com/JagritGumber)[@HoshangDEV](https://github.com/HoshangDEV)[@vagxrth](https://github.com/vagxrth)[@Paola3stefania](https://github.com/Paola3stefania)[@Bekacru](https://github.com/Bekacru)[@kira-1011](https://github.com/kira-1011)[@almadoro](https://github.com/almadoro)[@ocherry341](https://github.com/ocherry341)[@QuintenStr](https://github.com/QuintenStr)[@Ridhim-RR](https://github.com/Ridhim-RR)[@ahmed-abdat](https://github.com/ahmed-abdat)[@mohit4bug](https://github.com/mohit4bug)[@Pankaj3112](https://github.com/Pankaj3112)[@ShobhitPatra](https://github.com/ShobhitPatra)[@ping-maxwell](https://github.com/ping-maxwell)[@nbifrye](https://github.com/nbifrye)[@rovertrack](https://github.com/rovertrack)[@GautamBytes](https://github.com/GautamBytes)[@AntonVishal](https://github.com/AntonVishal)[@bytaesu](https://github.com/bytaesu)[@ic4l4s9c](https://github.com/ic4l4s9c)[@jcajuab](https://github.com/jcajuab)[@yutaka5](https://github.com/yutaka5)


---
