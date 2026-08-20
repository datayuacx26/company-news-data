---
schema_version: "1.0.0"
document_id: "728ccf0a2d0e8c4b84ae1d41e7ecb98f3c1af05fbaa0fdd59ca01c84612d5963"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/oauth-support"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:cd98174828ffe2c08a8af6da6f64bfb6db2c0453efa974b6d40f5ba1923f21f6"
---

# OAuth Support

Today, we're adding **OAuth to the Resend API** . Your app opens an authorization URL, the user approves it on Resend's own consent screen, and you get scoped access tokens back. No API keys change hands, and users can revoke access at any time.


Resend implements **OAuth 2.1 with PKCE** , plus[Dynamic Client Registration](https://resend.com/docs/api-reference/oauth/register) for clients like MCP hosts that can't know their deployment details ahead of time.


> Resend's OAuth API was a cinch to setup. Same full-feature access as using API keys, but more secure and with way better DX.
>
>
> Christian Rocha
>
>
> Co-Founder & CEO, Charm


## What you can build with it


- **Desktop and CLI tools** : register a loopback client, open the browser, and send on the user's behalf without ever touching their API key.
- **Hosted integrations** : let users click "Connect Resend" in your product and grant scoped access.
- **Agents and MCP hosts** : register dynamically at runtime and request the minimum scope they need.


## Integrations already in the ecosystem


The ecosystem is already growing. Today, we launch with two third-party integrations operational.


### Pop by Charm


[Pop](https://github.com/charmbracelet/pop) is a CLI tool for sending email from your terminal with a TUI or CLI, delivered through Resend, now authenticated through OAuth.


### Raycast Resend Extension


Raycast is a favorite tool for so many builders, creators, and developers. The[Raycast Resend extension](https://www.raycast.com/resend/resend) now supports OAuth to enable managing API keys, domains, audiences, contacts, and emails without leaving Raycast.


## How it works


For the full walkthrough, including both remote and local options, see[Building an OAuth client for Resend](https://resend.com/docs/guides/building-a-resend-oauth-client) .


### 1. Register your client


Call the[Register Client](https://resend.com/docs/api-reference/oauth/register) endpoint to dynamically register your OAuth client. Request the smallest scope that works:


- ` emails:send` covers send-only routes like` POST /emails` and` POST /broadcasts/:id/send` .
- ` full_access` is required for every other route.


```text
curl     -X   POST   'https://api.resend.com/oauth/register'     \           -H     'Content-Type: application/json'     \           -d     '{     "client_name": "Example OAuth Client",     "redirect_uris": ["http://127.0.0.1/oauth/callback"],     "grant_types": ["authorization_code", "refresh_token"],     "response_types": ["code"],     "token_endpoint_auth_method": "none",     "scope": "emails:send"   }'
```


The response returns a UUID` client_id` .


### 2. Generate PKCE values and send the user to authorize


Every authorization code exchange requires PKCE, so generate a` code_verifier` , its` code_challenge` , and a` state` value before you redirect.


```text
import     {   createHash  ,   randomBytes   }     from     'node:crypto'  ;
const     base64url     =     (  input  )     =>   Buffer  .  from  (  input  )  .  toString  (  'base64url'  )  ;
const   codeVerifier   =     base64url  (  randomBytes  (  64  )  )  ;     const   codeChallenge   =     base64url  (        createHash  (  'sha256'  )  .  update  (  codeVerifier  )  .  digest  (  )  ,     )  ;     const   state   =     base64url  (  randomBytes  (  24  )  )  ;
```


Open the authorization URL **in the user's browser** so they see the consent screen.


### 3. Exchange the code for tokens


Send the original` code_verifier` using the preferred form encoding.


```text
curl     -X   POST   'https://api.resend.com/oauth/token'     \           -H     'Content-Type: application/x-www-form-urlencoded'     \           -d     'grant_type=authorization_code&client_id=550e8400-e29b-41d4-a716-446655440000&code=AUTHORIZATION_CODE&redirect_uri=http%3A%2F%2F127.0.0.1%3A49152%2Foauth%2Fcallback&code_verifier=CODE_VERIFIER_VALUE'
```


You get back a JWT access token and an opaque refresh token:


```text
{        "access_token"  :     "eyJhbGciOiJFUzI1NiIsImtpZCI6Im9hdXRoX2tleSIsInR5cCI6ImF0K2p3dCJ9..."  ,        "token_type"  :     "Bearer"  ,        "expires_in"  :     900  ,        "refresh_token"  :     "JcL7aYfE7S9h3L4qv0o2e1w8m6n5b3x9RkP2tD4uV6Q"  ,        "scope"  :     "emails:send"     }
```


### 4. Call the API with the access token


Pass the access token as a Bearer token to any route your scope allows.


```text
curl     -X   POST   'https://api.resend.com/emails'     \           -H     'Authorization: Bearer eyJhbGciOiJFUzI1NiIsImtpZCI6Im9hdXRoX2tleSIsInR5cCI6ImF0K2p3dCJ9...'     \           -H     'Content-Type: application/json'     \           -d     '{     "from": "Acme <onboarding@resend.dev>",     "to": ["delivered@resend.dev"],     "subject": "Sent with OAuth",     "html": "<p>No API key required.</p>"   }'
```


Access tokens expire after 15 minutes. Refresh them with the refresh token, which rotates on every use.


## Users stay in control


Users can review and revoke any app's access from their Resend account's Team settings.


You can also revoke a grant yourself by[revoking its refresh token](https://resend.com/docs/api-reference/oauth/revoke) .


```text
curl     -X   POST   'https://api.resend.com/oauth/revoke'     \           -H     'Content-Type: application/x-www-form-urlencoded'     \           -d     'client_id=550e8400-e29b-41d4-a716-446655440000&token=JcL7aYfE7S9h3L4qv0o2e1w8m6n5b3x9RkP2tD4uV6Q&token_type_hint=refresh_token'
```


## Conclusion


OAuth turns Resend into something users can safely plug into any app, and it's the same foundation behind our[remote MCP server](https://resend.com/changelog/remote-mcp-server) .


If you have any questions, please reach out to us and we'll be happy to help.
