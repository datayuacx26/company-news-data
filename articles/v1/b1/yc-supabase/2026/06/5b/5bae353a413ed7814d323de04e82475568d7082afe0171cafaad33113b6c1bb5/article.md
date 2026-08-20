---
schema_version: "1.0.0"
document_id: "5bae353a413ed7814d323de04e82475568d7082afe0171cafaad33113b6c1bb5"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/47093-self-hosted-supabase-api-external-url-to-include-auth-v1"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:b26ff01b3115d742063a268731eb31776f865474317ebcf905ee00e4c40c2779"
---

# Self-hosted Supabase: API_EXTERNAL_URL to include /auth/v1

## What's Changing?#


The week of July 6, 2026, the default[self-hosted Supabase](https://supabase.com/docs/guides/self-hosting) configuration for` API_EXTERNAL_URL` will change to include the` /auth/v1` path prefix:


- ` API_EXTERNAL_URL` will default to` http://localhost:8000/auth/v1` (previously` http://localhost:8000` )
- ` GOTRUE_JWT_ISSUER` in` docker-compose.yml` will change to` ${API_EXTERNAL_URL}` (the actual URL will stay unchanged)
- OAuth redirect configuration placeholders in` docker-compose.yml` will become` ${API_EXTERNAL_URL}/callback`
- The API gateway routes for SAML SSO move from` /sso/saml/*` to` /auth/v1/sso/saml/*` , so SAML ACS and metadata endpoints become` …/auth/v1/sso/saml/acs` and` …/auth/v1/sso/saml/metadata`


This aligns self-hosted Supabase with the[platform](https://supabase.com/) behavior, and the[CLI](https://supabase.com/docs/guides/local-development) .


## Why?#


- **Consistency across deployments.**` API_EXTERNAL_URL` behaves identically on platform, self-hosted, and CLI.
- **Custom OAuth providers work out of the box.** GoTrue builds custom-provider callback URLs as` API_EXTERNAL_URL + /callback` . With the prefix now in the base URL, that resolves to` …/auth/v1/callback` and matches the existing API gateway route.
- **SAML aligns with the` /auth/v1` convention** used by every other auth endpoint, rather than living at a bare` /sso/saml/*` path.
- **The docs become accurate.** The guidance that "your callback URL is built from` API_EXTERNAL_URL` " is now literally true, instead of relying on the` /auth/v1` prefix being manually prepended in the compose file.


## Am I Affected?#


You are affected if you run self-hosted Supabase from the` ./docker` directory and pull updates from` master` , and any of the following apply:


- **You've overridden` API_EXTERNAL_URL`** in your` .env` (e.g.` https://my-domain.com` ) - you'll need to change it to` https://my-domain.com/auth/v1` .
- **You maintain a customized` docker-compose.yml`** with OAuth providers - the redirect URIs change from` ${API_EXTERNAL_URL}/auth/v1/callback` to` ${API_EXTERNAL_URL}/callback` to avoid a doubled` /auth/v1` prefix.
- **You use SAML SSO** - your IdP points at the old` /sso/saml/*` endpoints and must be updated to` /auth/v1/sso/saml/*` . This is the main breaking case.


You are **not** affected if you:


- Use the Supabase[platform](https://supabase.com/)
- Have OAuth providers registered with Google/GitHub/etc. - the **final callback URL is unchanged** (` …/auth/v1/callback` ), so no re-registration in the provider's developer console is needed
- Don't use SAML SSO and pull the new` docker-compose.yml` and` .env.example` together without local changes - the defaults stay consistent


## What Should I Do?#


If you pull the updated` docker-compose.yml` and` .env.example` together with no customizations, no action is required unless you use SAML SSO.


**If you've customized` API_EXTERNAL_URL` :**


1. Append` /auth/v1` to your value (e.g.` https://my-domain.com` →` https://my-domain.com/auth/v1` )
2. If your override file sets OAuth redirect URIs, change them from` ${API_EXTERNAL_URL}/auth/v1/callback` to` ${API_EXTERNAL_URL}/callback`
3. Restart the stack


**If you use SAML SSO:**


1. Pull the updated` docker-compose.yml` (or update your Kong and Envoy routes to` /auth/v1/sso/saml/acs` and` /auth/v1/sso/saml/metadata` )
2. Restart the stack
3. Re-fetch your service provider metadata from` {API_EXTERNAL_URL}/auth/v1/sso/saml/metadata` and update your IdP (ACS URL, SP entity ID) with the new endpoints
4. See the[self-hosted SAML SSO guide](https://supabase.com/docs/guides/self-hosting/self-hosted-saml-sso)


**If you'd rather defer:** keep your existing` API_EXTERNAL_URL` without the` /auth/v1` suffix and retain your current OAuth/SAML configuration. This is a default change, not a removal - but we recommend aligning soon, since custom OAuth providers won't work without it.


## Rollout#


Date Change


2026-06-18 This changelog published


2026-07-06...10 Updated self-hosting OAuth and SAML SSO docs published


2026-07-06...10 Default change ships in the next self-hosted Supabase release
