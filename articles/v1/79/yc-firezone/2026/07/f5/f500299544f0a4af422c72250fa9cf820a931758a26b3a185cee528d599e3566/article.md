---
schema_version: "1.0.0"
document_id: "f500299544f0a4af422c72250fa9cf820a931758a26b3a185cee528d599e3566"
company_key: "yc-firezone"
company: "Firezone"
source_id: "yc-firezone-news-import-75fbee21a5d6"
canonical_url: "https://www.firezone.dev/blog/single-owner-gateway-tokens"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-24T08:10:10.796715+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:26a48e3a7e5d590159028bb222cf35e7e7bc2c9dc0387a63162fd0641436a5a3"
---

# Single-owner Gateway tokens: what changed | Firezone Blog

**Action recommended.** Existing Gateways will continue to work and there is no migration deadline yet, but we recommend upgrading them to single-owner tokens ahead of a future deprecation of shared Site tokens. Seehow to migrate for the steps.


Firezone Gateways have historically authenticated with a token that belonged to their Site. Every Gateway in the Site shared it. That model is simple to start with, but did not end up as we expected:


- **Rotating the token meant redeploying everything.** Because the token was shared, replacing it required updating every Gateway in the Site, usually in a scramble and usually all at once.
- **You couldn't revoke just one Gateway.** Decommissioning a single machine (or responding to one leaked credential) meant revoking a token that every other Gateway in the Site depended on.
- **Ownership was ambiguous.** Given a token, there was no way to say which Gateway was using it. Given a Gateway, there was no way to say which token it held.


We've reworked this so that each Gateway now gets its own token.


## Why shared Site tokens no longer fit Gateway deployments


The shared Site token wasn't an accident. When we first started building Firezone, we expected Gateways to be treated like cattle, meaning they would be spun up and down dynamically, autoscaled with load, and replaced rather than repaired. A shared token is exactly what that world wants, since any number of anonymous, interchangeable instances can join a Site without anyone minting credentials for each one.


In practice, that world never really showed up. A Gateway sits at a network boundary. It needs a foothold inside a specific subnet, data center, or VPC, with routes to the Resources behind it, and that placement is deliberate infrastructure work, not something an autoscaler does on a whim. There’s also a connection-level reason, as scaling down means cutting live tunnels. A client accessing a TCP Resource through a Gateway can't just hop to another Gateway mid-stream the way an HTTP client hops to a new load balancer backend, so terminating a Gateway terminates every session flowing through it. And even if those problems were solvable, the pressure to solve them never materialized. WireGuard is efficient enough that a single Gateway handles far more traffic than early intuition suggested, so there was rarely a reason to scale herds of them horizontally in the first place. What we see instead is admins running a small, fixed number of Gateways per Site for redundancy, each one on a known, named, long-lived machine.


In other words, Gateways turned out to be pets. And pets deserve their own name tags. Single-owner tokens align the credential model with how Gateways are actually deployed, one identity per machine, managed individually.


## What changed with single-owner Gateway tokens


**Every new Gateway gets a single-owner token.** When you deploy a Gateway from the admin portal, we pre-create the Gateway and mint a token that belongs to it alone. The token authenticates that Gateway and nothing else, so revoking it affects exactly one machine.


**Token rotation is zero-downtime.** Rotating a Gateway's token mints a replacement while the current token keeps working. The old token stays valid until the Gateway first connects with the new one, or four hours pass, whichever comes first. You can update your deployment on your own schedule inside that window, and the running Gateway never drops. If you rotate again before the Gateway has picked up the replacement, only the unused pending token is replaced. The token actually in use is never invalidated out from under you.


**One connection per Gateway, first one wins.** Since a token now maps to exactly one Gateway, we can enforce that only one instance connects with it at a time. Connections follow a first-wins rule, meaning the instance already connected keeps its connection while any duplicate using the same identity is rejected until the original disconnects. This turns a silently misbehaving copy-pasted config into an immediate, visible error instead of two instances fighting over one identity.


**Better visibility in the portal.** Each Gateway's row now shows what it's connected with, either its own gateway token or a legacy site token, and whether a replacement token has been provisioned but not yet used. Gateways can also be renamed, so the machine you're about to rotate or revoke is the machine you think it is.


**API support.** You can mint and rotate single-owner tokens via the REST API (` POST /sites/{site_id}/gateways/{gateway_id}/token` and` .../token/rotate` ), so automated rotation fits into existing tooling.


## What stays the same for existing Gateways


**Existing Site tokens continue working today.** Nothing breaks when you upgrade. Gateways connected with a legacy Site token stay connected, reconnect fine, and behave exactly as before. There is no migration deadline yet, but we plan to announce a deprecation date in the future.


When you're ready to move a Gateway over, there's an **Upgrade token** action on its row in the portal. It mints a single-owner token for that Gateway. The legacy site token remains valid until you revoke it yourself from the Site's **Legacy tokens** tab, so you can migrate one Gateway at a time and revoke the shared token only once nothing depends on it.


## Why single-owner Gateway tokens improve security


Zero-trust access is built around individual identities, so shared long-lived credentials work against that model. Per-Gateway tokens give each machine its own identity and scope of access, so you can rotate, revoke, or replace a single token without affecting other gateways. That also makes regular credential rotation practical, reducing the operational cost of following security best practices.


## How to migrate to single-owner Gateway tokens


We recommend moving your Gateways onto single-owner tokens sooner rather than later. While shared Site tokens will continue to work for now, we will be setting a deprecation date in the future. Migrating your Gateway tokens now will allow you to get the per-Gateway rotation and revocation benefits today, and you won't be racing a deadline when the deprecation date eventually lands. Here's how to migrate, for each way you manage Gateways.


**Admin portal UI.** Gateways deployed from the portal from now on get a single-owner token automatically, so there's nothing to do for new deployments. For each existing Gateway on a legacy token, open its Site, expand the Gateway's row, and choose **Upgrade token** from the actions menu. Confirm, copy the revealed token (it's shown only once), and redeploy the Gateway with it. The legacy site token stays valid throughout, so you can migrate Gateways one at a time. Once no Gateways depend on the shared token, revoke it from the Site's **Legacy tokens** tab.


**Terraform.** The next release of our Gateway modules for AWS, Azure, and Google Cloud supports single-owner tokens. Each module gains a list variable that takes one token per Gateway instance:` firezone_tokens` on the AWS and Azure modules,` tokens` on the Google Cloud module. The module deploys one instance per token, so the instance count is the length of the list; to scale up or down, add or remove tokens instead of setting the old replica-count variable, which now applies only to legacy mode. The existing shared-token variable (` firezone_token` , or` token` on Google Cloud) keeps working for existing deployments, but it's legacy and mutually exclusive with the new list. Mint the tokens in the portal or via the REST API below, and see each module's README for full usage.


**REST API.** If your account has API access, mint a token for a specific Gateway with` POST /sites/{site_id}/gateways/{gateway_id}/token` . This returns 409 if the Gateway already has an active token; rotate instead with` POST /sites/{site_id}/gateways/{gateway_id}/token/rotate` , which follows the zero-downtime grace semantics described above. The old site-scoped token creation endpoint still works, but it's deprecated and new automation should use the per-gateway endpoints.
