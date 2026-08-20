---
schema_version: "1.0.0"
document_id: "1a6799c1534bf51207157f3d5c090d95b58a8ab35c464495e141cedb27d20981"
company_key: "yc-pinnacle"
company: "Pinnacle"
source_id: "yc-pinnacle-news-import-eb12d43fa2a4"
canonical_url: "https://pinnacle.sh/blog/custom-webhook-headers-authenticate-your-endpoint"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-22T09:08:45.431702+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:e96f990d1e67706ab0f8c919f9d99779c939595af0b8e12e5c6270a065274a98"
---

# Custom Webhook Headers: Authenticate Your Endpoint Without a Proxy

## The Problem With Unauthenticated Webhook Endpoints


Webhooks sound simple: Pinnacle POSTs an event to your URL, your server processes it. In practice, there's a gap between "public HTTPS endpoint" and "authenticated handler":


- Your webhook endpoint sits on the same infrastructure as the rest of your API — which is protected by a bearer token or API key
- The carrier (Pinnacle) can't send those tokens, so you either add a bypass route, a dedicated subdomain, or an ingress rule that exempts` /webhooks/*` from auth
- Each exception is one more thing to forget, misconfigure, or get paged about


Pinnacle's signing secret (` PINNACLE-SIGNING-SECRET` ) verifies the payload came from Pinnacle. But it doesn't get you past your own auth layer. For that, you need **your** headers on the request.


That's what custom webhook headers are for.


## What's New


Every webhook in Pinnacle can now carry a map of custom HTTP headers. When Pinnacle delivers an event, it adds those headers to the outgoing request alongside` PINNACLE-SIGNING-SECRET` . Your endpoint sees them on arrival and your existing auth middleware takes over.


You can set headers when:


- **Creating** a new webhook via[POST /webhooks/attach](https://docs.pinnacle.sh/api-reference/webhooks/attach-webhook) with` name` +` url`
- **Attaching** an existing webhook by` webhookId` — headers you supply **overwrite** whatever was stored; omit the field to leave the existing headers alone


Headers are returned on every webhook read endpoint (` list_webhooks` ,` get_webhooks` ,` attach_webhook` ) so you always know exactly what will land at your endpoint.


## Creating a Webhook With Headers


TypeScript


```text
import   {   PinnacleClient   }   from   "  rcs-js  "  ;
const   client   =   new   PinnacleClient  ({   apiKey  :   process  .  env  .  PINNACLE_API_KEY   });


await   client  .  webhooks  .  attach  ({
senders  :   [  "  +18005550001  "  ],
name  :   "  Production Delivery Events  "  ,
url  :   "  https://api.yourapp.com/webhooks/pinnacle  "  ,
event  :   "  MESSAGE.STATUS  "  ,
headers  :   {
AUTHORIZATION  :   `  Bearer   ${  process  .  env  .  WEBHOOK_INGRESS_TOKEN  }  `  ,
"  X-TENANT-ID  "  :   "  tenant_acme  "  ,
},
});
```


Every delivery from Pinnacle to` https://api.yourapp.com/webhooks/pinnacle` will now include:


```text
AUTHORIZATION: Bearer <your token>
X-TENANT-ID: tenant_acme
PINNACLE-SIGNING-SECRET: <pinnacle-generated>
Content-Type: application/json


```


Your auth middleware sees the bearer token, validates it, and passes the request to your handler. Same endpoint, same auth, zero bypass rules.


## Updating Headers on an Existing Webhook


Supply` headers` with a` webhookId` to rotate tokens or change routing metadata:


TypeScript


```text
await   client  .  webhooks  .  attach  ({
senders  :   [  "  +18005550001  "  ],
webhookId  :   "  wh_abc123  "  ,
event  :   null  ,
headers  :   {
AUTHORIZATION  :   `  Bearer   ${  process  .  env  .  WEBHOOK_INGRESS_TOKEN_V2  }  `  ,
"  X-TENANT-ID  "  :   "  tenant_acme  "  ,
},
});
```


A few important semantics:


- Supplying` headers` **replaces** the entire stored header map — if you had` X-API-KEY` on the webhook before and you send a map without it,` X-API-KEY` is gone
- Omitting` headers` entirely leaves the stored headers untouched — useful when you're re-attaching to add more senders and don't want to touch auth config
- To clear all custom headers, pass` headers: {}`


## Header Rules


Header names are validated at the API layer. The rules are strict and predictable:


- **Name regex:**` ^\[A-Za-z0-9\]\[A-Za-z0-9_-\]*$` — start with a letter or digit, then letters, digits,` -` , or` _`
- **Case-insensitive:** Names are normalized to uppercase before storage (per[RFC 9110](https://datatracker.ietf.org/doc/html/rfc9110#name-field-names) ), so` X-Tenant-Id` ,` x-tenant-id` , and` X-TENANT-ID` are the same header
- **Values:** Strings only — no numbers, booleans, or nested objects
- **Reserved:** The` PINNACLE-SIGNING-SECRET` header is stripped on every write. You cannot override it — it is always set by Pinnacle with the webhook's signing secret so you can verify the payload


Invalid header names fail with a 400 at request time, so misconfigurations are caught before they make it to production.


## Use Cases


### 1. Bearer token auth (the 90% case)


Your API is protected by` Authorization: Bearer <token>` . Your webhook endpoint is part of that API. Set the bearer token as a header on the webhook and the endpoint authenticates like any other route.


TypeScript


```text
headers  :   {
}
```


### 2. Multi-tenant routing without parsing the payload


You run a multi-tenant platform and each tenant has their own webhook on Pinnacle. Stamp the tenant ID in a header so your router doesn't have to parse the JSON body to figure out where to send the event.


TypeScript


```text
headers  :   {
"  X-TENANT-ID  "  :   "  tenant_acme  "  ,
}
```


### 3. Legacy systems with pre-shared keys


Not every system supports OAuth. If you're integrating with an older platform that only accepts a pre-shared key in a custom header, you can set it here.


TypeScript


```text
headers  :   {
"  X-API-KEY  "  : process  .  env  .  LEGACY_INTEGRATION_KEY  ,
}
```


### 4. Observability headers


Add trace correlation IDs or feature flags so your observability pipeline can fan out events without extra config on the receiving side.


TypeScript


```text
headers  :   {
"  X-TRACE-SOURCE  "  :   "  pinnacle  "  ,
"  X-ENV  "  :   "  production  "  ,
}
```


## Verifying With Your Existing Auth Layer


Here's an Express example that combines the bearer-token middleware you already have with Pinnacle's signing-secret verification:


TypeScript


```text
import   express   from   "  express  "  ;
import   crypto   from   "  crypto  "  ;


const   app   =   express  ();
app  .  use  (  express  .  json  ());


// Your existing auth middleware — no change required
const   requireBearer   =   (  req  ,   res  ,   next  )   =>   {
const   token   =   req  .  headers  .  authorization  ?.  replace  (  "  Bearer   "  ,   ""  );
if   (  token   !==   process  .  env  .  WEBHOOK_INGRESS_TOKEN  )   {
return   res  .  status  (  401  ).  send  (  "  unauthorized  "  );
}
next  ();
};


// Pinnacle signing verification runs after your auth layer
const   verifyPinnacleSignature   =   (  req  ,   res  ,   next  )   =>   {
const   signature   =   req  .  headers  [  "  pinnacle-signing-secret  "  ];
if   (  signature   !==   process  .  env  .  PINNACLE_SIGNING_SECRET  )   {
return   res  .  status  (  403  ).  send  (  "  invalid signature  "  );
}
next  ();
};


app  .  post  (
"  /webhooks/pinnacle  "  ,
requireBearer  ,
verifyPinnacleSignature  ,
(  req  ,   res  )   =>   {
res  .  status  (  200  ).  send  ();


const   {   event  ,   data   }   =   req  .  body  ;
// ... handle event
},
);
```


The key insight: your auth middleware and the Pinnacle signing check are **independent** . The custom header gets you through your own ingress. The signing secret proves the payload came from Pinnacle. Both checks must pass.


## Custom Headers in the MCP Server


The Pinnacle MCP server's[attach_webhook tool](https://github.com/pinnacle-dev/pinnacle-mcp) accepts the same` headers` field, so you can wire up authenticated webhooks from an AI-powered workflow:


> "Attach a webhook at` https://api.myapp.com/events` to every phone number in my account, and include` Authorization: Bearer $WEBHOOK_TOKEN` on every delivery."


The MCP validates the header regex client-side and calls` POST /webhooks/attach` . The gateway strips` PINNACLE-SIGNING-SECRET` before storing and applies your headers on every delivery — zero manual API wiring.


## Frequently Asked Questions


### Can I override the` PINNACLE-SIGNING-SECRET` header?


No. It's reserved. If you include it in your` headers` map, Pinnacle strips it before storing. The signing secret is always generated by Pinnacle and must remain under Pinnacle's control for signature verification to be trustworthy.


### What happens if I send an invalid header name?


The API responds with 400 and an error message pointing at the invalid key. Fix the name to match` ^\[A-Za-z0-9\]\[A-Za-z0-9_-\]*$` and retry.


### Are header values redacted in the dashboard?


Headers are returned in plain text on webhook read endpoints so you can verify what's being sent. If you store secrets in headers, treat the API response as sensitive — the same way you would an API key.


### Can I rotate header values without losing my webhook?


Yes. Call` POST /webhooks/attach` with the existing` webhookId` and the new` headers` map. The webhook stays in place, its subscriptions stay attached, and the headers are replaced atomically.


### Do headers show up on` list_webhooks` and` get_webhooks` ?


Yes. Every webhook read endpoint returns the configured headers so you can inspect the full delivery configuration without a round trip to support.


### How do I clear all headers from a webhook?


Send` headers: {}` when attaching by` webhookId` . An empty map replaces the stored headers with an empty map. Omitting the field leaves them unchanged — only an explicit empty object clears them.


## Key Takeaways


- **Custom HTTP headers** can now be attached to any Pinnacle webhook via` POST /webhooks/attach`
- **Overwrite semantics:** supplying` headers` with an existing` webhookId` replaces the stored map; omit to leave it unchanged; pass` {}` to clear
- **Name regex**` ^\[A-Za-z0-9\]\[A-Za-z0-9_-\]*$` , case-insensitive, uppercase-normalized — invalid names fail with a 400
- **Reserved:**` PINNACLE-SIGNING-SECRET` is silently stripped and always set by Pinnacle
- **Use cases:** bearer token auth on your existing API, multi-tenant routing, legacy pre-shared keys, observability stamps
- **MCP-ready:** the MCP` attach_webhook` tool accepts headers with the same validation


## Get Started


Attach your first authenticated webhook at[app.pinnacle.sh/dashboard/development/webhooks](https://app.pinnacle.sh/dashboard/development/webhooks) or via the[webhooks API](https://docs.pinnacle.sh/api-reference/webhooks/attach-webhook) . For the full webhook walkthrough — event types, signature verification, failure notifications — see the[SMS and RCS Webhooks guide](https://pinnacle.sh/blog/sms-rcs-webhooks-real-time-delivery-receipts-and-inbound-messages) .


Want to see how Pinnacle fits your stack?[Book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+webhook+authentication) with one of our engineers — we'll walk through your use case and get you live.
