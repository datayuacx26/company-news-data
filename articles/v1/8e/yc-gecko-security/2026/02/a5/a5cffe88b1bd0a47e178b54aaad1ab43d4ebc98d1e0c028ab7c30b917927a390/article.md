---
schema_version: "1.0.0"
document_id: "a5cffe88b1bd0a47e178b54aaad1ab43d4ebc98d1e0c028ab7c30b917927a390"
company_key: "yc-gecko-security"
company: "Gecko Security"
source_id: "yc-gecko-security-news-import-02715d60c1a6"
canonical_url: "https://www.gecko.security/blog/cve-2026-21894"
published_at: "2026-02-04T00:00:00+00:00"
first_seen_at: "2026-07-21T21:22:16.875303+00:00"
fetched_at: "2026-07-28T22:21:48.311987+00:00"
content_hash: "sha256:d78ca277aee583846e538ac2235cd61375f85b4de9c04f9372b6df9977180528"
---

# CVE-2026-21894: n8n Missing Stripe-Signature Verification Allows Forged Webhooks

---


### Advisory


- Link:[https://github.com/n8n-io/n8n/security/advisories/GHSA-jf52-3f2h-h9j5](https://github.com/n8n-io/n8n/security/advisories/GHSA-jf52-3f2h-h9j5)
- Affected versions: >= 0.150.0 <2.2.2
- Patched versions: 2.2.2


---


### Description


An authentication bypass in the StripeTrigger node allows any party who knows the webhook URL to forge arbitrary Stripe webhooks without knowing the Stripe webhook signing secret.


The node stores the Stripe webhook secret when creating the webhook endpoint, but the` webhook()` handler never verifies the` Stripe-Signature` header or the request body. It only checks that the` type` field of the incoming JSON matches one of the configured event types, then passes the entire unverified body directly into the workflow.


In contrast, other n8n webhook triggers like Slack, WooCommerce, and HelpScout verify provider signatures/HMAC over the raw request body and reject on mismatch. StripeTrigger stores the secret but performs no verification.


This design means that any unauthenticated HTTP client (not just Stripe) can send POST requests to the publicly exposed webhook URL and cause workflows to execute as if a legitimate Stripe event occurred, enabling payment forgery, subscription manipulation and other downstream attacks depending on how workflows are built.


### Source - Sink Analysis


The StripeTrigger webhook handler never checks that incoming requests really come from Stripe.


**In StripeTrigger.node.ts** , the` webhook()` function:


1. Reads the JSON body from the request
2. Checks that` bodyData.type` matches one of the configured event types
3. Passes the full` req.body` into the workflow


**What it does NOT do:**


- Read the` Stripe-Signature` header
- Use the stored webhook secret
- Verify any HMAC or signature over the raw body


The` create` method stores the Stripe webhook secret when the endpoint is created, but that value is never used in` webhook()` .


**Comparison with secure implementations:** Other trigger nodes (like WooCommerce and HelpScout) correctly compute an HMAC of` req.rawBody` with their secret and compare it to the provider's signature header, rejecting invalid or missing signatures. Because StripeTrigger skips this step entirely, any attacker who knows the webhook URL can send a plain HTTP POST with a JSON body that has a matching` type` (e.g.` payment_intent.succeeded` ), and n8n will treat it as a genuine Stripe event.


### Proof of Concept


On a deployed n8n cloud tenant (e.g.` https://<tenant>.app.n8n.cloud` ):


**1. Create new workflow in n8n cloud workspace**


**2. Add a Stripe Trigger node:**


- Set Events to` payment_intent.succeeded` (or` *` )
- Connect it to any simple node to easily observe execution


**3. Activate the workflow**


**4. Run the following with the Production Webhook URL:**


bash


```text
WEBHOOK_URL= "https://<tenant>.app.n8n.cloud/webhook/<uuid>/webhook"


# Forged Stripe webhook with a fake signature
curl -v -X POST  " $WEBHOOK_URL  "   \
-H  "Content-Type: application/json"   \
-H  "Stripe-Signature: FORGED"   \
-d  '{
"type": "payment_intent.succeeded",
"data": {
"object": {
"id": "pi_FORGED",
"amount": 999999,
"currency": "usd",
"status": "succeeded"
}
}
}'


```


**5. Observe the result:**


- The HTTP response is` 200 OK`
- The n8n UI shows the successful run under Executions
- The input data contains the forged payload, even though the` Stripe-Signature` is invalid and the IDs are fabricated


### Impact


This is an authentication bypass on the Stripe webhook endpoint: any attacker who learns the StripeTrigger webhook URL for a given n8n workflow (including on` *.app.n8n.cloud` tenants) can trigger that workflow by sending arbitrary JSON that has a matching` type` field, without knowing the Stripe webhook signing secret.


All n8n users who use the StripeTrigger node in active workflows are impacted, because those workflows will trust and act on forged payment or subscription events as if they came from Stripe.


In practice this can be abused to:


- **Fake successful payments** — granting free access or shipping goods without actual payment
- **Cancel or modify subscriptions** — denial of service for legitimate customers
- **Business-logic manipulation** — depending on how the workflow uses the incoming data (Code nodes, HTTP requests, database writes, templated emails/web pages), attackers can inject malicious data into downstream systems
