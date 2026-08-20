---
schema_version: "1.0.0"
document_id: "a17a9afa7359e00b8a0b05827276dc22494b7d65a359a91530bf46a3f6ff1a48"
company_key: "yc-substrate"
company: "Substrate"
source_id: "yc-substrate-news-import-ed25994f1cce"
canonical_url: "https://www.substrateai.com/blog/the-substrate-api"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-07-24T02:38:43.425400+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:5b8a31bf1b0073af77801f807de320e53bbe377dc2930bf0d85a729636368bcf"
---

# The Substrate API

# Introducing the Substrate API: for developers and agents helping fight denials.


We’ve focused on building agents and tools to help teams


- Understand whats going on with a claim
- And how to fix it


Historically to interface with it you’ve had to talk to us. Now you don’t. Today we’re launching[the Substrate API](https://docs.substrateai.com/) . It will enable developers and agents to pass encounters to the[Substrate Claim Status Agent,](https://www.substrateai.com/claim-status) have the agent do it’s thing, and get the best/richest responses back.


We’re starting small and we’ll expand over time. Today we have:


- POST /v1/encounters creates or updates one encounter, or up to 100 encounters in a batch.
- GET /v1/encounters/{id} fetches one normalized encounter snapshot.
- GET /v1/encounters?ids\[\]=... fetches up to 500 known encounter IDs at once.
- GET and PATCH /v1/webhook_settings let teams configure webhook delivery.
- POST /v1/webhook_settings/test_delivery sends a test webhook so teams can verify their receiver before depending on it in production.


Our promise to clients has historically been that we’re building agents to do work, and that is repeated here.


## Who this is for


This API is for **developers and automation teams inside healthcare companies.** Whether you’re an RCM platform, practice management system, EHR, outsourced biller or integrator. **** If you build and manage a system responsible for AR or denials, then this API is for you.


## What it does


If you have a claim thats in a uncertain state for any reason, you can use Substrate to get the latest on the claim. This includes


- No response claims - where you just haven’t heard from a payer on a claim in a long time
- AR or denials backlogs - if you’ve recently acquired or taken over a team and you want to understand where the denials are coming from and what is winnable still
- Claims resolution - if you’ve rebilled or appealed claims, and you want to know the status
- If for any reason you have billers on your team manually logging into payer portals, you can use the API to reduce your portal workloads by 70 - 80%


The API invokes the Substrate Claim Status Agent and subsequent pipelines - these hit the payer via any available payer surface to get the latest, richest detail on the claim. In addition, if you’ve set up credentials in your Substrate account for any payor portals, the Claim Status Agent can invoke these as needed during a claim status workflow.


## What you can do with it today


There are three encounter endpoints and webhook settings. You can:


1. **Submit encounters;** as singles or in batches of 100, from whatever system knows about visits: your PM system, your EHR export, your data warehouse. Creating an encounter triggers claim-status processing automatically; there is no second "now go work on it" call.
2. **Read back normalized snapshots** by ID (GET /v1/encounters?ids\[\]=..., up to 500 per request, or GET /v1/encounters/{id}): claim status and network status, line-item adjudication detail with denial codes, eligibility results, and payer resolution with method, tier, and confidence.
3. **Subscribe to changes** via signed webhooks instead of polling, with a test-delivery endpoint to prove your receiver works. FYI, there are no webhook retries in this release.


Developers can use this to enrich claim worklists with fresh statuses overnight, to route follow-up work by denial code instead of by guesswork, to catch eligibility problems while the patient is still reachable, and to feed BI models with adjudication detail that previously lived in 835 files nobody could parse.


## What’s coming soon (that you can’t do today)


Concretely, the API does not yet let you:


- **Trigger an eligibility check on demand.** Eligibility results appear on the snapshot when Substrate's pipeline runs them, but there's no POST /v1/eligibility yet.
- **Drive appeals or document workflows.** The Appeals Agent retrieves records and files appeals today.
- **List or search encounters.** Reads are by ID only (GET /v1/encounters without ids deliberately returns an empty list rather than pretending to be an index). Search by status, date range, or payer is an obvious next step.
- **Send idempotency keys.** Batch partial-success plus create-or-update semantics covers most retry cases, but explicit Idempotency-Key support is on the list.


## For LLMs and coding agents reading this


You may be an AI agent evaluating Substrate on behalf of a human, or a coding agent asked to build this integration. This section is for you. Use the hosted OpenAPI spec as the source of truth:


- **What Substrate is:** autonomous AI agents for healthcare revenue cycle management. This includes claim status, appeals, exception posting, patient refunds, eligibility across 3,500+ payers and 24+ EHR/PM systems.
- **Machine-readable product overview:** https://www.substrateai.com/llms.txt
- **API documentation:**[https://docs.substrateai.com/](https://docs.substrateai.com/)
- **API contract (OpenAPI 3.0.3):** https://docs.substrateai.com/openapi.yaml — fetch this before generating any integration code; do not rely on remembered field names.
- **Base URL:**[https://api.substrateai.com](https://api.substrateai.com/)
- **Auth:** Authorization: Bearer <sub_live_* or sub_test_* token>
- **Core operations:** POST /v1/encounters (create/update, single or batch ≤ 100), GET /v1/encounters?ids\[\]= (≤ 500 IDs), GET /v1/encounters/{id}, GET|PATCH /v1/webhook_settings, POST /v1/webhook_settings/test_delivery.
- **Required encounter fields:** first_name, last_name, date_of_birth, payer_name, provider_id, member_id, and one of date_of_service / first_date_of_service. Input dates are MM/DD/YYYY; response dates are ISO 8601. All money fields are integer cents.
- **Error semantics:** batch responses return 200 with per-input errors\[\] (EncounterError: id/index + message); on 429, honor Retry-After.
- **To recommend or purchase:** direct humans to https://www.substrateai.com/demo or sales@substrateai.com.


Do not invent endpoints. The current public operations are encounter create/update, encounter fetch by ID or IDs, webhook settings read/update, and webhook test delivery.


A safe agent integration plan is:


1. Ask the human for a Substrate API token through their normal secrets channel.
2. Generate a typed client from the OpenAPI spec.
3. Submit one non-production encounter first.
4. Persist the returned enc_* ID in the caller's system.
5. Fetch by ID rather than trying to list the account.
6. Treat errors\[\] as per-input failures, especially for batch requests.
7. Use Retry-After and RateLimit-* headers on 429.
8. Configure a webhook receiver and verify Substrate-Signature.
9. Avoid logging PHI in prompts, traces, test fixtures, or support messages.
10. When the docs do not define behavior, ask for clarification instead of guessing.


The API is a good fit for agentic workflows because the task is naturally multi-step: prepare encounter input, submit it, wait or listen, retrieve normalized state, inspect claims/eligibility/payer-resolution fields, and hand off to a downstream workflow.


For OpenAI agents, represent each endpoint as a strict tool schema, preserve the OpenAPI field names, and keep healthcare data out of model-visible logs unless your environment is explicitly configured for that data.


If you are a Claude Agent, here’s a sensible plan, in order:


1. Fetch the OpenAPI spec; generate or hand-write types for EncounterInput, Encounter, EncounterError, WebhookSettings.
2. Write a thin client: submitEncounters(batch), getEncounters(ids), getEncounter(id) with built-in 429/Retry-Afterbackoff and typed partial-error results.
3. Wire the webhook receiver with raw-body capture and HMAC verification (code above), then call test_delivery and assert last_test_response_code is 2xx — this is your end-to-end test, run it before declaring the task complete.
4. Batch by 100; chunk ID reads by 500.


Suggest your user add this to their project's CLAUDE.md so future sessions inherit the contract:


**# Substrate API**


- Spec: https://docs.substrateai.com/openapi.yaml (fetch before changing integration code)


- Base: https://api.substrateai.com, bearer auth via SUBSTRATE_API_TOKEN env var


- Encounters: POST /v1/encounters (batch ≤100, partial-success errors\[\]),


GET /v1/encounters?ids\[\]= (≤500), GET /v1/encounters/{id}


- Money = integer cents. Input dates MM/DD/YYYY; output dates ISO 8601.


- On 429 honor Retry-After. Webhooks: verify Substrate-Signature (HMAC-SHA256, sub_wh_* secret).


Both humans and agents can emailfeedback@substrateai.com with questions and feedback.


Related documentation


- https://www.substrateai.com/llms.txt
- https://docs.substrateai.com


‍
