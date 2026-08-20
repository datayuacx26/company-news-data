---
schema_version: "1.0.0"
document_id: "c644a33a873015cd7a7b2c2658fa007bc330a1f4b2f3a1bc7be1e61e05c37625"
company_key: "yc-better-auth"
company: "Better Auth"
source_id: "yc-better-auth-rss-ab6aa45cffa8"
canonical_url: "https://better-auth.com/blog/1-7"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T00:50:13.485983+00:00"
fetched_at: "2026-08-18T00:50:16.166509+00:00"
content_hash: "sha256:732260b4e3dc66825c43bdfdc2cfb1a927407d3de518b8d8821dc5ff9b2ee291"
---

# Better Auth 1.7

# Better Auth 1.7


We're excited to announce Better Auth 1.7 🎉


Better Auth 1.7 is one of our largest releases. It brings major improvements across OAuth and OpenID Connect, enterprise identity, MCP authorization, and device access.


If other applications sign in through your app, the OAuth provider now supports stronger tokens, explicit rules for protected APIs, more client authentication methods, and broader OpenID conformance. If your customers provision employees through SCIM, 1.7 adds Groups, role projections, richer employee profiles, and an optional bridge to SSO. MCP clients and limited-input devices can also request standards-based OAuth access.


We introduced the first part of this work in the[1.7 release candidate](https://better-auth.com/blog/1-7-rc) . Some improvements require migration work, especially for OAuth Provider, MCP, SSO, SAML, SCIM, account identities, and custom storage. The highlights below explain what each area gains. Follow the[1.7 upgrade guide](https://better-auth.com/docs/guides/1-7-upgrade-guide) for the migration steps.


---


## Highlights


Expanded


### OAuth and OpenID Connect for more apps and APIs


The OAuth provider is the center of Better Auth 1.7. It already let other apps sign users in and request access. This release adds stronger client authentication, explicit rules for each protected API, DPoP, sign-out across connected apps, and broader OpenID conformance.


We also tested 1.7 with the official OpenID Conformance Suite. It now handles more of the tested rules for login, token refresh, client registration, key rotation, and logout. These results are not a certification claim, and some optional OpenID features remain unsupported.


The main additions are:


- **Explicit rules for each API:** protected APIs can now have their own permissions, token lifetime, claims, and signing policy. Better Auth enforces which API a token was issued for (RFC 8707).
- **Tokens that are harder to steal:** DPoP ties a token to the client that requested it. Copying the token alone is not enough to use it (RFC 9449).
- **Back-channel logout:** clients that register a logout URL can be notified when the user's provider session ends.
- **Enforced recent login:** OpenID's` max_age` now works as intended, so an app can require a recent login before a sensitive action.
- **More ways for clients to prove who they are:** clients can use shared secrets or signed keys. Keys can also rotate without replacing the whole client.
- **Stronger client registration:** dynamic registration adds better admission rules and support for initial access tokens. MCP clients can instead identify themselves with a document hosted on their own website.
- **Ask for specific user details:** a client can request only the user information it needs, and the user sees that request during consent.
- **More consistent token checks:** APIs can check tokens under clearer resource and audience rules.
- **Safer retries:** native clients can recover when 2 refresh requests happen at nearly the same time, without weakening the default replay protection.


The provider can also be extended without changing its core. That is how the new MCP, Client ID Metadata Documents (CIMD), and device authorization features plug into the same OAuth system.


If you still use the deprecated` oidcProvider` plugin, migrate to[@better-auth/oauth-provider](https://better-auth.com/docs/plugins/oauth-provider) . The replacement provider existed before 1.7, and 1.7 removes the old plugin.


---


New


### MCP moves to its own package


MCP authentication now lives in its own package,[@better-auth/mcp](https://better-auth.com/docs/plugins/mcp) . It uses the OAuth provider from this release, supports the MCP 2026-07-28 authorization profile, and works with version 2 of the official MCP TypeScript SDK.


The responsibilities are clear: Better Auth handles login, consent, and access tokens. The official MCP SDK handles the messages sent between an MCP client and server.


The new package keeps the existing stateless bearer-token model and makes it safer to run across several server instances. Better Auth still stores normal login records, such as clients, consent, and refresh tokens.


The package expands MCP authorization with:


- **Updated discovery:** standard protected-resource information tells an MCP client where to sign in, which server the token is for, and which permissions are available.
- **Client ID Metadata Documents:**[@better-auth/cimd](https://better-auth.com/docs/plugins/cimd) lets an MCP client identify itself with a document on its own website. Older clients can still use Dynamic Client Registration when you enable it.
- **Tokens for the right server:** tokens are issued for one MCP server and rejected by a different server.
- **Stronger local checks:** MCP keeps local token verification and now checks that the token was made for this specific server.
- **Ask for more access when needed:** if a tool needs another permission, a compatible client can ask the user and try again.
- **Stronger token protection:** MCP servers can use DPoP so a copied token is not enough on its own.


Separate MCP services are now easier to configure because they can check tokens made explicitly for that service. Existing MCP setups need a migration because the package, setup, and OAuth paths have changed.


---


New


### Device authorization expands to OAuth clients


Better Auth already supported signing into the same application from a device with limited input. A new RFC 8628 grant expands that flow to registered OAuth clients. A CLI, smart TV, game console, or IoT device shows a code, and the user approves API access in a browser.


The approval page shows which app is asking and what it wants to access. After approval, the device receives a normal OAuth token for that API. It cannot ask for extra access after the user has approved the request.


This is separate from Better Auth's existing device sign-in flow, so both can run in the same application. It also works with products that ship their own MCP command-line tool. See the[device authorization documentation](https://better-auth.com/docs/plugins/device-authorization) for both options.


---


Rebuilt


### SCIM expands beyond User provisioning


Better Auth 1.6 already let enterprise directories create, update, deactivate, reactivate, and remove Users. Better Auth 1.7 rebuilds that service around isolated connections and adds first-class Groups, direct memberships, role projections, and richer employee profiles.


Connections now own their Users, Groups, memberships, and credentials. A separate provisioning domain defines where those changes apply in your product, such as a workspace, tenant, project, or organization.


The rebuilt service includes:


- **First-class Groups:** directories can now create, replace, update, and delete Groups, with direct User memberships.
- **Roles from Group membership:** map a directory Group to a role in your application, such as` admin` or` billing` .
- **Broader SCIM 2.0 behavior:** existing discovery and User filtering expand to Groups, pagination, field selection, and safer multi-step updates.
- **More employee details:** the rebuilt User resource adds fields such as department, manager, employee number, locale, phone numbers, and addresses.
- **Better provider support:** provider-specific handling and tests improve compatibility with Microsoft Entra ID, Okta, and Google.
- **Stronger employee identity:** stable directory IDs can survive deletion and drive exact SSO linking without falling back to email or username.
- **Safe retirement:** connections can be decommissioned while keeping their provisioning history and removing the access they contributed.


#### Self-service connections at runtime


Better Auth 1.7 replaces the legacy runtime management endpoints with 2 application-controlled options: resolve connections from your own catalog, or use the managed catalog built into the SCIM plugin.


The managed catalog supports several credentials per connection, with their own permissions and expiry dates. It can rotate and revoke credentials, record who made each change, and retire a connection safely. New secrets are returned only when they are created or rotated, while Better Auth stores protected digests instead of plaintext values.


If your application already manages customer connections, it can verify a credential and return the matching connection in one safe operation instead of adopting the managed catalog.


#### Provisioning and SSO now meet on an exact identity


SCIM creates and manages employee records, but it does not sign employees in. Better Auth 1.7 adds an explicit, transaction-safe bridge from a SCIM identity to a verified SSO identity.


Applications can configure this bridge so OpenID or SAML signs in the exact User created or linked by SCIM. The match uses the stable identity confirmed by both systems, not an email or username. When this integration is configured, an inactive, deleted, or decommissioned directory identity is rejected on its next SSO attempt.


The Next.js demo shows the configured integration. An administrator creates a connection, adds Users and Groups, changes access, disables employees, and later restores them. A separate employee view shows session revocation, rejected sign-ins, reactivation, and reprovisioning.


SCIM 1.7 replaces the previous setup and database model. Existing installations need a planned cutover and must ask their directory to send all users and groups again. Cloudflare D1 is not supported because it cannot provide the database transactions this flow needs. Follow the[SCIM migration steps](https://better-auth.com/docs/guides/1-7-upgrade-guide#scim-requires-full-reprovisioning) before turning provisioning back on.


---


Breaking


### A normalized identity model across OAuth, OpenID, and SAML


External accounts now use a trusted provider identity plus the stable subject confirmed by that provider. OpenID uses` sub` , SAML uses the signed` NameID` , and plain OAuth providers use their declared account ID.


Configurations that return the same trusted provider and subject now share one identity. Equal subject values from different providers remain separate. Existing applications need to review and migrate their saved account identities.


The SSO plugin builds on that model:


- **Link the exact User:** a new SSO resolver can connect either a verified OpenID or SAML identity to an existing Better Auth User or reject the sign-in.
- **All-or-nothing sign-in:** resolver changes, account linking, profile updates, and session creation now succeed or fail together.
- **More control over provider changes:** existing safeguards now include an application hook that can block a provider update or deletion while another system still depends on it.
- **Stronger SAML checks:** 1.7 adds working request-response matching and verifies the signed identity directly, building on the audience and destination checks already in 1.6.
- **Certificate rotation:** SAML providers can publish old and new certificates together, so rotation does not require downtime.
- **Safer provider-started login:** SAML login started from the identity provider is off by default. Applications can enable it when needed.
- **More reliable logout:** SAML logout now uses the actual session identifier, fixing cases where the intended session did not end.
- **More runtimes:** OpenID SSO with automatic provider discovery now works on Cloudflare Workers.


With an exact mapping configured, SCIM and the paired login provider can point to the same Better Auth User. See the[SSO documentation](https://better-auth.com/docs/plugins/sso) for the final setup.


---


Expanded


### More providers and better login flows


Better Auth as an OAuth client also received a large update. Generic OAuth providers now use the common social sign-in client path instead of a separate client plugin. PKCE is on by default, and providers configured through discovery get stronger identity-token and nonce checks.


You can also change provider options for one sign-in. This supports choosing an Amazon Cognito provider, giving Microsoft Entra ID a domain hint, or asking Google for offline access. Entra ID and custom providers can use signed keys instead of shared secrets. Providers such as Auth0 and Zitadel can receive the extra information they need when a token refreshes, without sending the user through login again.


Scopes already granted to an account now survive later sign-ins and token refreshes.


Provider-started OpenID login can restart through a fresh protected login flow when enabled. Anonymous account linking also works in Expo and other in-app browsers.


---


Also included


### Improvements across Better Auth


- **More built-in translations:**` @better-auth/i18n` expands its catalog to 22 languages.
- **Drizzle Relations v2:** generated Better Auth relations can be combined with the relations already in your application.
- **Stable database joins:** existing database joins graduate from experimental status.
- **Signed session cache:** services can verify cached sessions with a public key instead of sharing the signing secret.
- **Faster server-rendered pages:**` hydrateSession` gives the browser the session that the server already loaded, avoiding another request.
- **Passkey onboarding:** passkey registration can create a session as part of registration.
- **Two-factor enrollment:** applications can now explicitly choose OTP or TOTP when enabling two-factor authentication.
- **Safer passwordless recovery:** after a magic link or email code proves mailbox ownership, Better Auth can remove unproven credentials and revoke older sessions.
- **Username controls:** applications can make usernames immutable or remove the separate display-username field.
- **CLI administration:**` npx auth create-admin` creates an administrator from the command line.
- **Expanded organization APIs:** load basic organization details without every member, and list a user's teams from trusted server code.
- **Extensible SSO providers:** store application-specific details with an SSO provider.
- **Database tooling:** Drizzle supports PostgreSQL schema namespaces, and generated indexes are more consistent across databases.
- **Direct Fetch integration:** Better Auth instances now integrate more directly, with stronger types, where a standard` fetch` -compatible handler is expected.
- **Mobile and desktop changes:** Expo storage integrations move to asynchronous SecureStore methods, while Electron tightens callback-origin and custom URL-scheme validation.
- **SIWE identity:** Sign-In with Ethereum now derives the wallet address and chain from the verified message, removing duplicate request fields.
- **Captcha path rules:** protect exact endpoints or explicit wildcards, without partial-path matches that can skip a rule.
- **Session cleanup:** sign-out with external session storage now runs deletion hooks, ends preserved sessions, and revokes related OAuth tokens.
- **Safer concurrent actions:** one-time codes, refresh tokens, invitations, counters, and rate limits now use the adapter's atomic operations.
- **Identity admission:**` user.validateUserInfo` can reject a user before creation or account linking across OAuth, SSO, credentials, passwordless methods, and SCIM.


---


## Important changes


The table below shows where to start. The[1.7 upgrade guide](https://better-auth.com/docs/guides/1-7-upgrade-guide) contains the exact schema, data, configuration, and API changes.


If you use Start here


Basic Better Auth setup Upgrade all Better Auth packages together, then generate and review schema changes


Social login, custom OAuth, One Tap, or SSO Review how existing accounts map to identities confirmed by each provider


` @better-auth/oauth-provider` Review saved clients and apply the database changes for protected APIs, stronger tokens, and logout


MCP Install` @better-auth/mcp` , update the integration, and apply the OAuth client database changes


Device authorization Apply the device-code database changes and choose between app sign-in and OAuth access


SAML or SSO Review saved account identities, provider settings, certificates, and callback URLs


SCIM Stop provisioning, replace the old setup, create new credentials, then send all Users and Groups again


Expo or React Native Await cookie reads and update custom secure-storage implementations


Two-factor authentication Review OTP and TOTP enrollment and handle the new` enableTwoFactor` response shape


Magic links or email OTP Review the new cleanup behavior for accounts whose mailbox was not confirmed


Custom adapters, storage, or rate-limit stores Add the new methods for safe one-time actions and counters before deploying


Custom proxy or TLS termination Confirm that Better Auth knows the public URL of your application


---


## Migrating to v1.7


Upgrade` better-auth` and every` @better-auth/*` package together:


```text
npx   auth   upgrade
```


Then run` npx auth generate` or` npx auth migrate` for schema changes. Do not treat the generated migration as the full upgrade: account identity, OAuth clients, MCP, and SCIM need reviewed manual data steps. The[upgrade guide](https://better-auth.com/docs/guides/1-7-upgrade-guide) walks through each one.


---


## Contributors


Thanks to all the contributors for making this release possible!


[@0-Sandy](https://github.com/0-Sandy)[@0xHouss](https://github.com/0xHouss)[@9hsein5](https://github.com/9hsein5)[@aarmful](https://github.com/aarmful)[@adityachaudhary99](https://github.com/adityachaudhary99)[@adrianmxb](https://github.com/adrianmxb)[@ahmedivy](https://github.com/ahmedivy)[@allandelmare](https://github.com/allandelmare)[@Andrew1326](https://github.com/Andrew1326)[@arnnvv](https://github.com/arnnvv)[@baptisteArno](https://github.com/baptisteArno)[@Bekacru](https://github.com/Bekacru)[@Bekione](https://github.com/Bekione)[@bennettdams](https://github.com/bennettdams)[@benpsnyder](https://github.com/benpsnyder)[@birkskyum](https://github.com/birkskyum)[@bjorntechTobbe](https://github.com/bjorntechTobbe)[@brentmitchell25](https://github.com/brentmitchell25)[@brone1323](https://github.com/brone1323)[@bytaesu](https://github.com/bytaesu)[@Byte-Biscuit](https://github.com/Byte-Biscuit)[@CatLover01](https://github.com/CatLover01)[@cb-alish](https://github.com/cb-alish)[@chdanielmueller](https://github.com/chdanielmueller)[@ChrisMGeo](https://github.com/ChrisMGeo)[@Craga89](https://github.com/Craga89)[@cyphercodes](https://github.com/cyphercodes)[@demhadais](https://github.com/demhadais)[@dipan-ck](https://github.com/dipan-ck)[@DougInAMug](https://github.com/DougInAMug)[@dvanmali](https://github.com/dvanmali)[@ejirocodes](https://github.com/ejirocodes)[@ElGauchooooo](https://github.com/ElGauchooooo)[@elliotBraem](https://github.com/elliotBraem)[@eluce2](https://github.com/eluce2)[@Emmaccen](https://github.com/Emmaccen)[@erquhart](https://github.com/erquhart)[@fabian-hiller](https://github.com/fabian-hiller)[@FaryalRizwaan](https://github.com/FaryalRizwaan)[@florianamette](https://github.com/florianamette)[@formatlos](https://github.com/formatlos)[@frankeld](https://github.com/frankeld)[@GautamBytes](https://github.com/GautamBytes)[@GoPro16](https://github.com/GoPro16)[@gustavovalverde](https://github.com/gustavovalverde)[@himself65](https://github.com/himself65)[@IcanDivideBy0](https://github.com/IcanDivideBy0)[@IdrisGit](https://github.com/IdrisGit)[@ItalyPaleAle](https://github.com/ItalyPaleAle)[@Jadenstanton](https://github.com/Jadenstanton)[@jashkarangiya](https://github.com/jashkarangiya)[@jaydeep-pipaliya](https://github.com/jaydeep-pipaliya)[@jeroenvandermerwe](https://github.com/jeroenvandermerwe)[@jjluzgin](https://github.com/jjluzgin)[@jlucaso1](https://github.com/jlucaso1)[@jonathansamines](https://github.com/jonathansamines)[@jsj](https://github.com/jsj)[@kgarg2468](https://github.com/kgarg2468)[@KingIronMan2011](https://github.com/KingIronMan2011)[@Kinfe123](https://github.com/Kinfe123)[@krish-vachhani](https://github.com/krish-vachhani)[@Ktryberceo](https://github.com/Ktryberceo)[@Kvizas](https://github.com/Kvizas)[@lubiah](https://github.com/lubiah)[@mausic](https://github.com/mausic)[@momomuchu](https://github.com/momomuchu)[@moonevm](https://github.com/moonevm)[@mrosberghaus](https://github.com/mrosberghaus)[@MuzzaiyyanHussain](https://github.com/MuzzaiyyanHussain)[@nphlp](https://github.com/nphlp)[@Oluwatobi-Mustapha](https://github.com/Oluwatobi-Mustapha)[@OliverCordingl1](https://github.com/OliverCordingl1)[@onmax](https://github.com/onmax)[@OscarCornish](https://github.com/OscarCornish)[@ouwargui](https://github.com/ouwargui)[@Paola3stefania](https://github.com/Paola3stefania)[@pbacza](https://github.com/pbacza)[@peyremorgan](https://github.com/peyremorgan)[@pi0](https://github.com/pi0)[@ping-maxwell](https://github.com/ping-maxwell)[@programming-with-ia](https://github.com/programming-with-ia)[@rachit367](https://github.com/rachit367)[@ramonclaudio](https://github.com/ramonclaudio)[@reslear](https://github.com/reslear)[@ruban-s](https://github.com/ruban-s)[@Saiyaswanthpasupuleti](https://github.com/Saiyaswanthpasupuleti)[@seanfilimon](https://github.com/seanfilimon)[@seebykilian](https://github.com/seebykilian)[@SferaDev](https://github.com/SferaDev)[@skalkii](https://github.com/skalkii)[@sleepe229](https://github.com/sleepe229)[@sovetski](https://github.com/sovetski)[@stewartjarod](https://github.com/stewartjarod)[@TanishValesha](https://github.com/TanishValesha)[@terijaki](https://github.com/terijaki)[@tonytkachenko](https://github.com/tonytkachenko)[@tsokolovs](https://github.com/tsokolovs)[@tsushanth](https://github.com/tsushanth)[@Tushar-Khandelwal-2004](https://github.com/Tushar-Khandelwal-2004)[@Vishesh-Verma-07](https://github.com/Vishesh-Verma-07)[@WilsonnnTan](https://github.com/WilsonnnTan)[@wobedi](https://github.com/wobedi)[@XXMOHAMED012](https://github.com/XXMOHAMED012)[@yordis](https://github.com/yordis)[@zeroknowledge0x](https://github.com/zeroknowledge0x)[@zllovesuki](https://github.com/zllovesuki)
