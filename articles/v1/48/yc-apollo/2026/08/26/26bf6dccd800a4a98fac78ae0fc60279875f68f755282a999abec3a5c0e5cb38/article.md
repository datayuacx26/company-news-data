---
schema_version: "1.0.0"
document_id: "26bf6dccd800a4a98fac78ae0fc60279875f68f755282a999abec3a5c0e5cb38"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/where-apollo-mcp-server-stands-on-the-owasp-mcp-top-10"
published_at: "2026-08-14T17:11:10+00:00"
first_seen_at: "2026-08-14T22:18:30.236053+00:00"
fetched_at: "2026-08-14T22:18:39.129273+00:00"
content_hash: "sha256:5a50df8cf7c5d9f16f23d1d88d062e5b2811f0e0ebbe359ffe5930b3ba29ae48"
---

# Where Apollo MCP Server Stands on the OWASP MCP Top 10

[OWASP’s MCP Top 10](https://owasp.org/www-project-mcp-top-10/) catalogs what can go wrong when an agent calls a tool over Model Context Protocol. This article evaluates one deployed server,[Apollo MCP Server](https://www.apollographql.com/docs/apollo-mcp-server) , against all ten risks on the latest reviewed OWASP revision. Every claim links to the server’s source at the v1.17.0 tag. We wrote the server and this audit.


This audit exists because “secure MCP” is not a binary state. It is a shared responsibility model, and the boundary is usually left undefined. The verdict: architecture alone closes exactly one of the ten risks, with no hardening required. A tool in Apollo MCP Server is a typed GraphQL operation, parsed and type-checked before it runs, so there is no shell in the request path. The other nine ship as primitives that become protection only once your team turns them on. Drawing that boundary for each risk is the point: it tells you what to configure, what to monitor, and what the architecture already handles.


**OWASP risk** **What Apollo MCP Server provides** **What stays yours**


[MCP01: Token mismanagement](https://owasp.org/www-project-mcp-top-10/2025/MCP01-2025-Token-Mismanagement-and-Secret-Exposure)` ${env.VAR}` for header values in the YAML config Static headers in plaintext apart from the GraphOS key;` .graphql` files uncovered; forwarded-token exposure (see MCP07)


[MCP02: Privilege escalation via scope creep](https://owasp.org/www-project-mcp-top-10/2025/MCP02-2025%E2%80%93Privilege-Escalation-via-Scope-Creep) Global[auth.scopes](https://www.apollographql.com/docs/apollo-mcp-server/auth#configure-scope-enforcement) validated on every authenticated request; per-operation[required_scopes](https://www.apollographql.com/docs/apollo-mcp-server/auth#per-operation-scope-requirements) , returning a` 403 insufficient_scope` Per-operation scoping is opt-in and maintained by hand; the global list is one bar for the whole server, and any token that clears it can call any tool


[MCP03: Tool poisoning](https://owasp.org/www-project-mcp-top-10/2025/MCP03-2025%E2%80%93Tool-Poisoning)[Typed schema](https://www.apollographql.com/docs/apollo-mcp-server/define-tools) pins execution;[overrides.descriptions](https://www.apollographql.com/docs/apollo-mcp-server/define-tools#config-level-descriptions) pins description text in config Names, descriptions, and parameter descriptions still steer agents; hot source updates require recurring snapshot review


[MCP04: Supply chain & dependency tampering](https://owasp.org/www-project-mcp-top-10/2025/MCP04-2025%E2%80%93Software-Supply-Chain-Attacks&Dependency-Tampering)[Container images](https://www.apollographql.com/docs/apollo-mcp-server/deploy) attested per architecture; macOS binary signed, notarized For Linux/Windows, build from the pinned source commit in a controlled pipeline and retain provenance.


[MCP05: Command injection & execution](https://owasp.org/www-project-mcp-top-10/2025/MCP05-2025%E2%80%93Command-Injection&Execution) No shell in the request path;[mutation_mode](https://www.apollographql.com/docs/apollo-mcp-server/config-file#overrides) gates ad-hoc mutations On/off switch, not human approval


[MCP06: Intent flow subversion](https://owasp.org/www-project-mcp-top-10/2025/MCP06-2025%E2%80%93Intent-Flow-Subversion) Blast radius bounded to the exposed operation set; a subverted agent can’t invoke what the server doesn’t offer (bound widens with` execute` / ad-hoc mutations) The attack itself: injection that steers the agent into valid, permitted operations; defenses live in the agent framework and in minimizing what you expose


[MCP07: Insufficient auth & authz](https://owasp.org/www-project-mcp-top-10/2025/MCP07-2025%E2%80%93Insufficient-Authentication&Authorization) Checkpoint with issuer-metadata validation:[transport.auth](https://www.apollographql.com/docs/apollo-mcp-server/config-file#auth) ; default[host validation](https://www.apollographql.com/docs/apollo-mcp-server/config-file#host-validation) ; passthrough off-switch[disable_auth_token_passthrough](https://www.apollographql.com/docs/apollo-mcp-server/limitations#oauth-token-passthrough) Defaults to` stdio` , where authentication never runs and the server will refuse to start when an` auth` block is configured there. Validated caller token remains forwarded upstream by default (spec-noncompliant;[rationale](https://www.apollographql.com/docs/apollo-mcp-server/limitations#rationale) ); configured issuers remain optional hardening.


[MCP08: Lack of audit & telemetry](https://owasp.org/www-project-mcp-top-10/2025/MCP08-2025%E2%80%93Lack-of-Audit-and-Telemetry) Traces, metrics, logs tied by` trace_id` Spans unredacted unless[omitted_attributes](https://www.apollographql.com/docs/apollo-mcp-server/telemetry) set


[MCP09: Shadow MCP servers](https://owasp.org/www-project-mcp-top-10/2025/MCP09-2025%E2%80%93Shadow-MCP-Servers) Not this server’s layer Asset inventory, CSPM


[MCP10: Context injection & over-sharing](https://owasp.org/www-project-mcp-top-10/2025/MCP10-2025%E2%80%93ContextInjection&OverSharing) Not this server’s layer Deployment topology


What you’re deploying: one MIT-licensed Rust binary, self-hosted, built from source or pulled as a container. Operations come from local files, manifests, or GraphOS collections, so nothing below needs a hosted control plane.


## What architecture settles and only bounds \[MCP05, MCP03, MCP06\]


**Command injection (MCP05)** .[Tools](https://modelcontextprotocol.io/specification/2025-06-18/server/tools) in Apollo MCP Server come from local files, GraphOS collections, or persisted-query manifests. Even` execute` , the tool for ad-hoc operations, is schema-validated before it runs. The[mutation_mode](https://www.apollographql.com/docs/apollo-mcp-server/config-file#overrides) setting gates writes with three levels:


- ` none` (the default): filters mutations out entirely, even predefined mutation operations don’t become tools.
- ` explicit` : exposes only the mutations you’ve authored as operation files.` execute` still can’t run ad-hoc ones.
- ` all` : lets` execute` run any mutation the schema type-checks.


All three are config-time gates, not per-invocation approval: the human decision in` explicit` happens when the operation file is committed, not when the agent calls it.


**Tool poisoning, the server’s half (**[MCP03](https://owasp.org/www-project-mcp-top-10/2025/MCP03-2025%E2%80%93Tool-Poisoning) **)** . Typed schemas pin execution: rewritten descriptions can’t add server-side behavior, because behavior remains the parsed, type-checked operation itself. But tool names, descriptions, input names, and input descriptions are model-facing text that can steer an agent toward attacker-chosen arguments, tools, or schema-valid exfiltration. Capture the live tool list, inspect it for hidden or encoded instructions, and compare it with the last approved snapshot whenever the operation or schema source changes.


**Intent flow subversion, bounded but not blocked (**[MCP06)](https://owasp.org/www-project-mcp-top-10/2025/MCP06-2025%E2%80%93Intent-Flow-Subversion) . An injected instruction can’t conjure capabilities the server never exposed; it can only invoke operations you already approved. That bounds blast radius, but it is not a defense. Steering the agent into a valid, permitted mutation is the attack, not something this constraint stops. The bound widens when execute or ad-hoc mutations are enabled, since “permitted” then covers anything that type-checks. What’s left is exposing as few operations as the use case allows, and` mutation_mode` staying off.


## What turns on with a configuration key


**Authentication and authorization (MCP07).** Protection depends on` transport.type: stdio` or` streamable_http` , default` stdio` . Over HTTP with an` auth` block, the server is a checkpoint, not an issuer: it validates the caller’s token against any OAuth-compatible identity provider, per the[MCP Authorization spec](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization) , and rejects authorization-server discovery metadata whose issuer differs from that server before trusting its signing keys;` issuers` remains operational hardening. Missing permission returns a[WWW-Authenticate challenge](https://www.rfc-editor.org/rfc/rfc6750) naming what’s missing and[pointing at the resource metadata](https://www.rfc-editor.org/rfc/rfc9728) , so a well-built client knows what to retrieve next.[Host validation](https://www.apollographql.com/docs/apollo-mcp-server/config-file#host-validation) , on by default with an off-switch, blocks DNS rebinding over HTTP. Over` stdio` , or HTTP without an` auth` block, none of this runs. A server refuses to start when` auth` is configured under` stdio` . An environment variable can flip the transport or host validation even when the file itself looks locked down. What the server does with the validated token afterward is a separate failure mode, covered below.


**Scope creep (MCP02).** Two checks, two enforcement points: the global` auth.scopes` list is validated with the token itself, so it runs on every authenticated request but it is one bar for the whole server, and any token that clears it reaches every tool. Per-operation[required_scopes](https://www.apollographql.com/docs/apollo-mcp-server/auth#per-operation-scope-requirements) narrows that: populate it for an operation, and a token scoped to one tool stops working for the rest, rejected at invocation with a 403` insufficient_scope` challenge. The map is opt-in, keyed by operation name, and maintained by hand. Both checks exist only where the` auth` checkpoint does.


**Audit and telemetry (MCP08).** The server logs generously by default: traces, metrics, and logs are tied together by` trace_id` , with arguments, query, and response recorded on each span. That is also the risk. Spans hold everything unredacted unless you set[omitted_attributes](https://www.apollographql.com/docs/apollo-mcp-server/telemetry) on both exporters. Redact everything and you cannot reconstruct what happened. Redact nothing and your secrets travel with the trace.


## The gaps


**Credential storage (MCP01).**` ${env.VAR}` expansion keeps header values out of the YAML config itself, though not out of` .graphql` operation files. Static headers stay in plaintext apart from the GraphOS key. The larger exposure, a bearer token traveling into upstream logs it was never issued for, follows from the passthrough default, covered below.


**Supply chain (MCP04).** Container images carry per-architecture SLSA attestations, a genuine but partial guarantee: the multiarch manifest itself carries none, so pin and verify per-architecture digests rather than pulling by tag. Signing is uneven across binaries. Only the macOS build is signed and notarized. The Linux and Windows tarballs ship without signatures, checksums, or an SBOM at this tag.


**Hot reload, the residual half of tool poisoning (MCP03)** . Apollo MCP Server hot-loads[GraphOS operations](https://www.apollographql.com/docs/apollo-mcp-server/config-file#operation-source) and non-federated schemas, letting names, descriptions, inputs, or behavior change. Initial fetches retry 429 or server failures; a permanent first-load failure prevents loading. After a successful load, failures retain the last working schema or collection while polling continues. That improves availability, but updates still receive no automatic diff, approval step, or comparison with the reviewed tool list. A collection edit (behavior, not just description) goes live within a minute of polling. Anyone with write access can change a tool as easily as an attacker could. The trust boundary moved. It didn’t close.


**Token passthrough (MCP07)** . With auth configured, the caller’s validated token is forwarded upstream by default, which[the spec](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization) forbids. The problem is authority, not storage: the upstream API makes authorization decisions on a token that was never issued to it, and the identity the checkpoint just verified is delegated wholesale rather than re-asserted. Apollo[documents its enterprise and multi-tenant rationale](https://www.apollographql.com/docs/apollo-mcp-server/limitations#rationale) for the default. The off-switch,[disable_auth_token_passthrough](https://www.apollographql.com/docs/apollo-mcp-server/config-file#auth) , is real, but flipping it costs the caller’s identity upstream, since no[token exchange](https://www.rfc-editor.org/rfc/rfc8693) replaces it yet ([planned](https://www.apollographql.com/docs/apollo-mcp-server/limitations#future-plans) ). Until it lands, the tradeoff is conditional. Flip the switch when the upstream doesn’t authorize per caller, or when a narrowly scoped service credential can stand in. Leave it on when upstream authorization keys off the user’s token and no narrow substitute exists, and compensate: audience validation at the upstream, short token lifetimes, redacted spans (MCP08). Flipping the switch alone also proves nothing:[forward_headers](https://www.apollographql.com/docs/apollo-mcp-server/config-file#forwarding-headers) forwards the raw, unvalidated header regardless of the flag, so audit both together. And every token that does cross the boundary lands in upstream request logs and traces, replayable from there, which is where this row touches MCP01.


## Not this server’s layers


Two of the ten risks sit outside what any protocol server can see or control. Shadow servers ([MCP09](https://owasp.org/www-project-mcp-top-10/2025/MCP09-2025%E2%80%93Shadow-MCP-Servers) ) live in asset inventory or CSPM. Tenant isolation ([MCP10](https://owasp.org/www-project-mcp-top-10/2025/MCP10-2025%E2%80%93ContextInjection&OverSharing) ) lives in deployment topology: separate instances, separate credentials. Treat either as a server-selection criterion and you’re shopping for a feature that cannot exist.


## Ownership


Scope-drift review, per-operation re-diffs, redaction settings, source write access, and tool-definition snapshots after every change: each needs an owner and a cadence, or it fails regardless of the configuration. Every claim above is verifiable. Commands, pass conditions, and environment-override checks are in[the interactive companion checklist](https://www.apollographql.com/apollo-mcp-server-audit-checklist) .


Written by


Camille Lawrence


[Read more by Camille Lawrence](https://www.apollographql.com/blog/author/clawrence)
