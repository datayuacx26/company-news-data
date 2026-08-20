---
schema_version: "1.0.0"
document_id: "c44906afaf239849539abf60a211bbc1a68232fb95f4204122eb3f17493f9dfb"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/authentication-strategy-web-app/"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:2e7d6eb850a5109b6464b7135a54ea29dd20eb29d7294e96511328648d7de89d"
---

# Choosing an Authentication Strategy for a New Web App

For a new web app in 2026, the strategy maps to your app’s shape: a server-rendered monolith should start with server-side sessions in a signed, HttpOnly cookie; an SPA, mobile client, or API backend should use short-lived access tokens with refresh-token rotation kept in HttpOnly cookies; anything that needs social login or enterprise SSO should layer OAuth 2.0/OIDC on top; and if authentication isn’t your product, the most defensible default is a managed provider. Add passkeys as the phishing-resistant factor regardless of which base you pick. The rest of this article justifies each branch against concrete criteria — revocation, scaling, CSRF/XSS exposure, complexity, and the UX failure modes that only show up in production.


The trap most new apps fall into is reaching for JWTs because a tutorial used them, without a stateless-scaling reason that justifies the revocation cost. This is a comparative decision, not a default — so here are the criteria, a comparison table, and a matrix that maps app archetype to a recommendation.


## Key Takeaways


- Sessions versus tokens is a question of *where* auth state lives — on the server or in the client — while cookies versus the` Authorization` header only decides *how* the credential travels; conflating the two is the most common reason new apps cargo-cult JWTs they don’t need.
- Storing JWTs in` localStorage` is now an anti-pattern: any XSS on your page can read and exfiltrate the token, so keep credentials in HttpOnly cookies the JavaScript layer can’t touch.
- The hard problem with JWTs isn’t issuing them — it’s revoking them; instant logout requires short token lifetimes with refresh-token rotation or a server-side deny-list, which quietly reintroduces the state JWTs were meant to eliminate.
- OAuth 2.1 is still an IETF Internet-Draft (draft-15, March 2026), not a ratified RFC, but its direction is settled: mandatory PKCE, exact redirect-URI matching, and removal of the Implicit and password-credentials grants.
- If authentication isn’t your product, a managed provider (Auth0, Clerk, Supabase Auth, WorkOS, Stytch) is the pragmatic 2026 default — you inherit sessions, social login, MFA, and passkeys without owning the breach surface.


## The framing that cuts through the noise: where state lives vs how it travels


Sessions versus tokens is a question of *where* your authentication state lives — on the server or inside the client’s credential — while cookies versus the` Authorization` header only decides *how* that credential travels over HTTP. These are orthogonal axes, and treating them as one decision is why so many teams adopt JWTs they don’t need. This disambiguation is the clarifying spine for everything below, and the auth vendor[Authgear frames it the same way](https://www.authgear.com/post/session-vs-token-authentication/) .


A session is **stateful** : the server stores the authoritative record and hands the client an opaque ID that points back to it — the hotel-keycard model, where the front desk keeps the master list. A JWT is **stateless** : the claims are self-contained and signed, so any server can verify them without a lookup — the digital-passport model, where any officer trusts the signature without phoning the issuer. You can deliver *either* over a cookie *or* the` Authorization` header. Cookies and headers are transport; sessions and tokens are architecture.


Criterion Server-side sessions JWT / self-contained tokens


Where state lives Server (store) Client (signed token)


Revocation Immediate — delete the record Hard — short TTL + rotation, or deny-list


Scaling Needs shared store (e.g. Redis) Stateless; no per-request lookup


Credential size ~32-byte opaque ID Often 800+ bytes of claims per request


Primary attack surface CSRF (cookies auto-sent) XSS (token theft from JS-readable storage)


Best fit Server-rendered monolith API-first, SPA, mobile, microservices


The size contrast is the stateless-versus-stateful trade in one line: a session cookie carries roughly 32 bytes of opaque ID and requires a server lookup, whereas a JWT carries the claims inline — often 800+ bytes on every request — and skips the lookup. You’re trading bandwidth and revocation latency for the elimination of a store.


## Sessions: the default for server-rendered apps


For a server-rendered monolith, start with server-side sessions in a signed, HttpOnly, Secure, SameSite cookie — it’s the simplest correct option and gives you instant revocation for free. The browser sends the cookie automatically, you store no auth state in JavaScript, and logging a user out is a single delete on the server.


The mechanics are four steps: the user submits credentials, the server creates a session record and returns its ID in a cookie, the browser replays that cookie on every request, and logout destroys both sides. Hash passwords with **Argon2** before they touch the store — the[OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) treats it as the current first-choice algorithm. Generate session IDs with high entropy and never embed sensitive data in them; the[OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html) is the canonical reference for ID length, rotation, and timeout.


```text
# FastAPI: issue an opaque session, store it server-side, set a hardened cookie
import   secrets
from   fastapi   import   FastAPI, Response
# redis client + Argon2 password verification omitted for brevity


app   =   FastAPI()
SESSION_TTL   =   60   *   60   *   24    # 24 hours


@  app  .  post  (  "  /login  "  )
async   def   login  (  response  : Response):    # verify Argon2 hash first
session_id   =   secrets.token_urlsafe(  32  )            # ~256 bits of entropy
await   redis.setex(  f  "sess:  {  session_id  }  "  , SESSION_TTL, user_id)
response.set_cookie(
"  session  "  , session_id,
httponly  =  True  ,      # JS cannot read it -> blunts XSS exfiltration
secure  =  True  ,        # HTTPS only
samesite  =  "  lax  "  ,     # mitigates CSRF on top-level navigations
max_age  =  SESSION_TTL,
)
return   {  "  ok  "  :   True  }
```


This snippet shows the whole point of sessions: the secret never lives in JavaScript, and the server holds the source of truth. Two trade-offs follow. First, scaling: because the record is server-side, a multi-instance deployment needs sticky sessions or — better — a shared store like[Redis](https://redis.io/docs/latest/develop/data-types/strings/) so any instance can resolve any session. Second, CSRF: cookies are sent automatically on cross-site requests, so pair` SameSite` with a CSRF token using constant-time comparison, per the[OWASP CSRF Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html) . The HttpOnly flag closes the XSS read path;` SameSite` plus a token closes the CSRF path.


## JWTs and tokens: stateless scaling with a revocation cost


Reach for raw JWTs when you have a concrete stateless-scaling reason — an API serving multiple client types, microservices that pass identity between themselves, or cross-domain access where cookies are awkward — not as a default. The payoff is real: a signed token is verified locally against a public key, so you skip the per-request store lookup and any node can authenticate any request.


The hard problem with JWTs isn’t issuing them — it’s revoking them. Because the token is self-contained, it stays valid until it expires no matter what your server thinks. Instant logout therefore requires either short token lifetimes with refresh-token rotation, or a server-side deny-list that you check on every request — and that deny-list quietly reintroduces the exact state JWTs were supposed to eliminate. The standard pattern is a short-lived access token paired with a longer-lived, rotated refresh token; the[OAuth 2.0 Security Best Current Practice (RFC 9700, January 2025)](https://www.rfc-editor.org/rfc/rfc9700.html) is the ratified anchor for refresh-token rotation and short access-token lifetimes — a stronger source than the round-number TTLs floating around in blog posts.


Where you store the token decides your blast radius. Storing JWTs in` localStorage` is now an anti-pattern: any XSS on your page can read it and exfiltrate the token, so keep tokens in HttpOnly cookies the JavaScript layer can’t touch. For browser apps specifically, the IETF’s[OAuth for Browser-Based Apps](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-browser-based-apps) best-current-practice draft recommends the Authorization Code flow with PKCE and warns that malicious JavaScript can do far more than read a token.


```text
# FastAPI: verify an access token with PyJWT (not python-jose)
import   jwt    # PyJWT
from   datetime   import   datetime, timezone
from   fastapi   import   HTTPException


def   verify_access_token  (  token  :   str  ,   public_key  :   str  ) ->   dict  :
try  :
return   jwt.decode(
token, public_key,
algorithms  =  [  "  RS256  "  ],            # never accept "none"; pin the alg
options  =  {  "  require  "  : [  "  exp  "  ,   "  iat  "  ,   "  sub  "  ]},
)
except   jwt.ExpiredSignatureError:
raise   HTTPException(  401  ,   "  token expired  "  )    # client should refresh
except   jwt.InvalidTokenError:
raise   HTTPException(  401  ,   "  invalid token  "  )


# When you mint tokens, use timezone-aware UTC:
# exp = datetime.now(timezone.utc) + timedelta(minutes=15)
```


Two version notes keep this current. Use[PyJWT](https://pyjwt.readthedocs.io/en/stable/) —[FastAPI’s official security tutorial](https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/) migrated to it (` import jwt` ) alongside` pwdlib` , and` python-jose` is now only minimally maintained. And mint expiries with` datetime.now(timezone.utc)` :` datetime.utcnow()` was deprecated in Python 3.12 and, per the[Python pending-removals documentation](https://docs.python.org/3/deprecations/pending-removal-in-future.html) , remains deprecated with no scheduled removal version — it still works, but the timezone-aware call is the recommended replacement.


## OAuth 2.0/2.1 and OIDC: delegated identity when you need it


Use OAuth 2.0 with OpenID Connect when you want users to sign in with an existing identity provider — “Sign in with Google,” GitHub, Microsoft, Okta — or when you need enterprise SSO. This is the VIP-guest-list model: you don’t verify the user yourself; you trust an issuer who already knows them. OAuth 2.0 handles delegated *authorization* (access tokens for APIs), and[OpenID Connect](https://openid.net/developers/how-connect-works/) layers an identity token on top so you learn *who* the user is.


It is worth the complexity only when delegation or SSO is a genuine requirement, because the flow brings real moving parts — authorization codes, PKCE, scopes, redirect URIs, and JWKS key rotation. For an MVP with first-party login and no social requirement, OAuth is overkill; you’ll carry the redirect machinery without using the delegation it exists for.


If you do adopt it, target the modern profile. OAuth 2.1 is still an IETF Internet-Draft (draft-15, published 2 March 2026), not a ratified RFC, but its direction is settled.[The major differences from OAuth 2.0](https://oauth.net/2.1/) are that PKCE is required for all clients using the authorization code flow, redirect URIs must be compared using exact string matching, and the Implicit grant and the Resource Owner Password Credentials grant are omitted. The IETF itself notes that[Internet-Drafts are valid for at most six months](https://datatracker.ietf.org/doc/draft-ietf-oauth-v2-1/) and that it is inappropriate to cite them other than as “work in progress,” so treat 2.1 as the *target* consolidation — already enforced in practice by major identity providers — rather than a shipped standard. The substance is what matters: use Authorization Code + PKCE for every client, including confidential server-side ones, and drop wildcard redirect URIs.


## Passkeys and WebAuthn: the phishing-resistant direction of travel


Add passkeys as the phishing-resistant factor, not as a religion. Built on[WebAuthn/FIDO2](https://www.w3.org/TR/webauthn-3/) , a passkey is a public/private key pair where the private key stays in the device’s secure hardware and signs a server challenge after a local biometric or PIN check. Because the credential is origin-bound and never transmitted, it resists phishing, replay, and credential stuffing in a way passwords and SMS codes cannot.


The policy ground has shifted decisively. The NCSC announced in April 2026 that[passkeys should now be consumers’ first choice of login across all digital services](https://www.ncsc.gov.uk/news/ncsc-leave-passwords-in-the-past-passkeys-are-the-future) , and that it will begin recommending passkeys wherever a service supports them, and two-step verification where it does not —[a shift reflected through an ongoing refresh of guidance rather than a single sudden change](https://www.ncsc.gov.uk/blogs/passkeys-are-more-secure-than-traditional-ways-to-log-in) .[Support is effectively universal](https://www.computing.co.uk/news/2026/security/forget-passwords-passkeys-are-the-future-says-ncsc) : passkeys are supported by major operating systems including Windows, ChromeOS, macOS, Android, iOS and many Linux distributions, as well as browsers such as Chrome, Firefox, Edge and Safari.


Passkey *support* is solved; passkey *enrollment* is not — and that’s the real selection constraint. Per[Corbado’s Passkey Benchmark 2026](https://www.corbado.com/passkey-benchmark-2026) (a vendor benchmark, not a neutral population statistic), first-try web enrollment runs roughly 49–83% on iOS but only 25–39% on Windows. Treat enrollment UX and account recovery as first-class design problems: offer passkeys as an opt-in upgrade with a clear fallback, and design recovery before launch, because a lost device with no recovery path is a locked-out user.


## Auth-as-a-Service: the pragmatic default when auth isn’t your product


If authentication isn’t your product, the most defensible 2026 default is a managed provider —[Auth0](https://auth0.com/docs) ,[Clerk](https://clerk.com/docs) ,[Supabase Auth](https://supabase.com/docs/guides/auth) ,[WorkOS](https://workos.com/docs) , or[Stytch](https://stytch.com/docs) — because you inherit sessions, social login, MFA, and passkeys without owning the breach surface or the maintenance. These vendors ship drop-in SDKs that handle the full lifecycle, including the OAuth redirect machinery and WebAuthn ceremonies you’d otherwise build and debug yourself.


The decision is straightforwardly economic. Rolling your own gives you control and zero per-MAU cost; a managed provider gives you a hardened implementation, faster time-to-login, and someone else’s on-call rotation for the security-critical path. For most greenfield apps where login is table stakes rather than a differentiator, the managed route is the rational starting point — and you can migrate off it later if scale or cost demands.


## A decision matrix for authentication methods


The strategy follows the app archetype. Map your shape to the row, start there, and add passkeys across the board as the phishing-resistant factor.


App archetype Start with Add when needed


Server-rendered monolith Server-side sessions (HttpOnly cookie) OIDC for social login; passkeys opt-in


SPA / mobile / API backend Short-lived access tokens + refresh rotation, in HttpOnly cookies OIDC for third-party identity


Needs social login or enterprise SSO OAuth 2.0 + OIDC (target the 2.1 profile) Sessions or tokens for your own surfaces


Auth isn’t your product Managed provider (Auth0/Clerk/Supabase/WorkOS/Stytch) — it already bundles the rest


High-security / consumer-facing Any base above **+ passkeys** Recovery flow designed up front


The common production architecture is a hybrid: sessions for the server-rendered web app, JWTs for the API consumed by SPAs and mobile, OIDC for social sign-in, and passkeys as an opt-in upgrade. The industry trend underneath all of it is to send the password fewer times, in fewer places, with more scope and accountability — which is exactly why scoped tokens replaced password-based Git auth and why passkeys are now displacing passwords outright.


## What session replay reveals about auth UX failure modes


Session replay is a technique that reconstructs the real client-side flow, and replaying authentication journeys surfaces a recognizable catalogue of failure modes that backend logs miss — because picking the right strategy on paper doesn’t guarantee users can actually log in (these are capabilities of the technique, not measured incident rates):


- **Silent token-expiry logout loops.** When a refresh token expires mid-session, an SPA can bounce the user to login repeatedly. Replay shows the redirect loop on the timeline, which error logs miss because each individual request returns a technically valid 401.
- **Abandoned passkey enrollment.** Replay shows where users drop out of the WebAuthn ceremony — the OS prompt appears and the user cancels — which is the exact failure the low Windows enrollment numbers predict.
- **OAuth redirect dead-ends.** A post-consent redirect that lands on a blank or error page (mismatched redirect URI, lost` state` parameter) is invisible in your backend logs because the round-trip happens off your server; replay captures the dead-end as the user saw it.
- **Browser-specific cookie failures.**` SameSite` and Secure-cookie problems that don’t reproduce locally show up when replay correlates the failing session with a specific browser and version.


The point is that authentication is a UX surface, not just a protocol choice — and the strategy you pick determines which of these failure classes you’ll be debugging.


Pick the base that matches your app’s shape, harden the credential storage (HttpOnly cookies, never` localStorage` ), and add passkeys as the opt-in phishing-resistant factor. For most new apps the honest first move is a managed provider or a sessions-first monolith; reach for raw JWTs only when a real stateless-scaling requirement justifies the revocation cost. Once login ships, watch the real flow — the gap between a sound strategy and a working one is exactly the UX detail that only appears in production.


## FAQs


Should I use JWTs for a new web app by default?


No. Use raw JWTs only when you have a concrete stateless-scaling reason, such as an API serving multiple client types or microservices passing identity between themselves. For a server-rendered monolith, server-side sessions are simpler and give you instant revocation. JWTs are self-contained, so revoking them before expiry requires short token lifetimes with refresh-token rotation or a server-side deny-list, which reintroduces the exact server state JWTs were meant to eliminate.


Is a session token the same as a JWT?


No. A session token is an opaque ID, roughly 32 bytes, that points to authentication state stored on the server, so every request requires a lookup. A JWT carries its claims inline and is verified locally against a signature with no lookup, often adding 800 or more bytes per request. The two represent the stateless-versus-stateful trade: a session keeps state on the server, while a JWT moves it into the client's signed credential.


Can I store a JWT in localStorage if I sanitize my inputs?


No, storing a JWT in localStorage is an anti-pattern regardless of input sanitization, because any cross-site scripting flaw anywhere on the page can read localStorage and exfiltrate the token. Keep credentials in HttpOnly cookies, which JavaScript cannot access, and pair them with SameSite and a CSRF token. For browser apps, the IETF OAuth for Browser-Based Apps best-current-practice draft recommends the Authorization Code flow with PKCE rather than client-readable token storage.


Is OAuth overkill for an MVP with first-party login?


Yes, if your MVP only needs first-party login with no social sign-in or enterprise SSO, OAuth 2.0 adds machinery you will not use. The flow brings authorization codes, PKCE, scopes, redirect URIs, and JWKS key rotation, all of which exist to support delegated identity. Adopt OAuth with OpenID Connect only when 'Sign in with Google,' GitHub, or enterprise SSO is a genuine requirement; otherwise start with sessions or a managed provider.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
