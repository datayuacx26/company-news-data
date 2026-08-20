---
schema_version: "1.0.0"
document_id: "243b2837e0ae5e3f4f19b66b5328e6926625217b01ab8627ae4dfb8fbec0ba76"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/48048-self-hosted-supabase-envoy-becomes-the-default-api-gateway-b"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-08T02:28:33.650553+00:00"
fetched_at: "2026-08-09T18:31:09.294076+00:00"
content_hash: "sha256:449328f9d65a928a662742c827ddef49f286b8751678c9e9e31c3b1e0efd29d7"
---

# Self-hosted Supabase: Envoy becomes the default API gateway (breaking change)

## What's Changing?#


The week of **Aug 9, 2026** , the default[self-hosted Supabase](https://supabase.com/docs/guides/self-hosting) API gateway will change from **Kong** to **[Envoy](https://www.envoyproxy.io/)** . Envoy has shipped as an optional override (` docker-compose.envoy.yml` ) for the last several releases; it now becomes the default in` docker-compose.yml` , and Kong moves to an optional override.


This is a **breaking change** for a subset of self-hosters (see below) - most notably anyone relying on Kong's built-in HTTPS listener, a customized` kong.yml` , or tooling that references the gateway by service/container name.


- The gateway service in` docker-compose.yml` becomes` api-gw` , running Envoy (container` supabase-envoy` ). It keeps` kong` as a network alias for backward compatibility.
- Kong is now opt-in via a new` docker-compose.kong.yml` override:` sh run.sh config add kong` .
- The gateway HTTP port is now configured with` API_GW_HTTP_PORT` (defaulting to the existing` KONG_HTTP_PORT` , then` 8000` ), so existing` .env` files keep working.
- The default gateway listens on plain **HTTP only** (port` 8000` ). Kong's built-in HTTPS listener on` 8443` is **not** part of the Envoy default. Terminate TLS with the shipped` docker-compose.caddy.yml` or` docker-compose.nginx.yml` overrides, or opt back into Kong.
- ` docker-compose.envoy.yml` becomes a no-op shim for one release cycle so existing overrides don't break, then is removed.


## Why?#


- **Kong's open-source line has effectively stopped advancing.** The OSS Kong we currently ship (` kong:3.9.1` ) is roughly a year old; subsequent 3.9.x releases have been security/nginx backports only, with no feature cadence, and Kong[removed free (unlicensed) mode in 3.10](https://developer.konghq.com/gateway/changelog/#3-10-0-0-breaking-change-core) . Continuing on the frozen OSS Kong line carries growing security and compliance risk for self-hosters.
- **One gateway, config-as-code.** Envoy's routing, filters, and access control live in versioned YAML under` volumes/api/envoy/` , with no plugin runtime to manage.
- **First-class support for the new API keys.** The Envoy configuration translates opaque` sb_publishable_*` /` sb_secret_*` keys into internal JWTs. See[New API Keys and Asymmetric Authentication](https://supabase.com/docs/guides/self-hosting/self-hosted-auth-keys) .
- **Hardened defaults.** Path normalization, header-smuggling rejection, edge-proxy client-IP handling, and a locked-down admin interface. See the[Envoy API Gateway guide](https://supabase.com/docs/guides/self-hosting/self-hosted-envoy) .
- **Consistency.** Aligns the self-hosted default with the direction of the wider Supabase stack.


## Am I Affected?#


You are affected if you run self-hosted Supabase from the` ./docker` directory and pull updates from` master` , and any of the following apply:


- **You rely on Kong's built-in` :8443` HTTPS listener** directly (no reverse proxy in front). The Envoy default does not expose` 8443` . Put Caddy/Nginx in front for TLS, or opt back into Kong.
- **You maintain a customized` kong.yml`** (custom routes, plugins, ACLs). These do not carry over to Envoy and will silently stop applying. Opt back into Kong to keep them, or port the customizations to the Envoy config.
- **You have scripts or tooling that reference the gateway by service/container name** (` kong` /` supabase-kong` ) - for example` docker compose logs kong` or` run.sh restart kong` . The service is now` api-gw` and the container is` supabase-envoy` . The` kong` **hostname** still resolves via a network alias.
- **You currently run the Envoy override** (` docker-compose.envoy.yml` in your` COMPOSE_FILE` ). No behavior change now, but you should remove that entry from` COMPOSE_FILE` once you pull the update, since Envoy is the base default.


You are **not** affected if you:


- Use the Supabase[platform](https://supabase.com/)
- Use locally running Supabase services via[CLI](https://supabase.com/docs/guides/local-development)


## What Should I Do?#


If you pull the updated` docker-compose.yml` and` .env.example` together with no gateway customizations, no action is required beyond being aware that requests now flow through Envoy.


**If you want to stay on Kong:**


`
_10


sh run.sh config add kong


_10


sh run.sh recreate


`


Kong remains available as an override for compatibility and migration, but note it tracks the frozen OSS 3.9.x line - we recommend it as a transition aid, not a long-term default.


**If you customized` kong.yml` :** opt back into Kong (` config add kong` ) to keep your config, or port your routes/rules to the Envoy config under` volumes/api/envoy/` . See the[Envoy API Gateway guide](https://supabase.com/docs/guides/self-hosting/self-hosted-envoy) .


**If you already run the Envoy override:** remove` docker-compose.envoy.yml` from` COMPOSE_FILE` (or run` sh run.sh config remove envoy` ) after pulling the update.


**If you'd rather defer:** pin your checkout, or add the Kong override so your gateway behavior is unchanged. This is a default change, not a removal - but we recommend migrating to Envoy, since the OSS Kong line is no longer actively maintained.


## Rollout#


Date Change


2026-07-17 This changelog published


\[TBD ~Aug 13\] Updated self-hosting gateway docs published


\[TBD ~Aug 13\] Default change ships in the next self-hosted Supabase release
