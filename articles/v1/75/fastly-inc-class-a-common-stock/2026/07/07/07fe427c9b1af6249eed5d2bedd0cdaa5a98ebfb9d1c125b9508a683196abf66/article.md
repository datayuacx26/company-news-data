---
schema_version: "1.0.0"
document_id: "07fe427c9b1af6249eed5d2bedd0cdaa5a98ebfb9d1c125b9508a683196abf66"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/automated-payments-at-the-edge-with-x402-and-fastly-compute/"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T18:09:11.870504+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:57766ed4b528dc0c639e243c6ac362c384694b1c00b484347704f7e61c618616"
---

# Automated Payments at the Edge with x402 and Fastly Compute

[The first article in this technical series](https://www.fastly.com/blog/state-of-pay-agentic-commerce-payments-and-the-edge) explored the landscape of agentic commerce and the protocols beginning to shape it. One of those protocols was x402, which brings HTTP-native payment challenges to automated transactions. In this blog, we'll move from theory to practice by building a simple x402 application on[Fastly Compute](https://www.fastly.com/products/edge-compute) and using the edge to enforce payment before requests reach the origin.


## **From Theory to Implementation**


HTTP has had a 402 Payment Required status code for a long time, but most of the web never really used it.


X402 ([https://www.x402.org/](https://www.x402.org/) ) makes that status code useful for automated payments.


The idea is simple: an HTTP service can say, “this resource requires payment,” return a 402 Payment Required response, and include payment instructions in the response. A client, bot, agent, device, or service can read that challenge, pay it, retry the request, and get the protected resource.


The next wave of commerce will not always look like a person clicking a checkout button. It may be an agent paying for an API call, a device buying access to a service, a crawler paying for premium data, or one application paying another application for a task.


That is where x402 starts to make sense. It gives HTTP a payment challenge that software can understand and respond to automatically.


The question then becomes where to enforce that challenge. For many applications, the answer is the edge.


## **Why the edge is the right enforcement point**


Fastly Compute is not acting as the wallet, bank, or payment provider in this model.


The payment itself still happens through the x402 flow and the payment infrastructure the application chooses, whether that is a facilitator, wallet, network, or payment provider. What Fastly provides is the programmable enforcement point.


A protected route should not have to handle every unpaid request itself. Compute can sit in front of that route, apply payment policy, and only allow the request through after the x402 payment has been verified.


In the following demonstration, an unauthenticated request to /protected-route triggers a 402 Payment Required status. Once a request includes the necessary x402 payment, it undergoes verification at the edge before the system delivers the secured JSON content.


That same pattern could apply to an API, a data product, a premium crawler endpoint, a device-initiated purchase, or a machine-to-machine service call. The origin does not need to own the whole payment challenge flow. It can receive traffic that has already passed the payment check.


## **Building an x402 payment flow on Fastly Compute**


The demo is intentionally small.


It runs on Fastly Compute and protects one HTTP route:


Copied!


```text
GET /protected-route
```


That route costs:


Copied!


```text
$0.001
```


A direct request does not get the protected content. It returns:


Copied!


```text
HTTP 402 Payment Required
```


The response includes a payment-required header with the x402 challenge.


The paid path calls:


Copied!


```text
POST /api/fetch-protected-route
```


That endpoint uses a configured payer wallet, signs the x402 challenge, retries the protected request, and returns the protected response after payment verification succeeds.


The response looks like this:


Copied!


```text
{
"ok"  :   true  ,
"paid"  :   true  ,
"status"  :   200  ,
"statusText"  :   "OK"  ,
"result"  : {
"message"  :   "This content is behind an x402 paywall. Thanks for paying!"  ,
"servedBy"  :   "Fastly Compute"  ,
"paid"  :   true
}
}
```


There is also a small browser UI. One button shows the unpaid 402 response. Another button runs the paid flow. That makes the demo easy to explain in curl and easy to show in a browser.


### **Test wallets and stablecoin payments**


For the demo, I used test wallets and stablecoin on a test network.


There are two wallets:


**Wallet**


**Purpose**


Receiver / merchant


Receives payment


Payer / agent


Signs and pays the x402 challenge


The receiver only needs an address in the app config. The payer needs a test private key because the demo signs the payment from inside the flow.


The demo uses Base Sepolia USDC. That means no real funds are involved. The payer wallet gets testnet USDC from a faucet, then uses that balance to complete the x402 payment flow.


That setup keeps the demo easy to run while still proving the important part: an HTTP request can be blocked with 402, paid by a machine client, verified, and then allowed through.


### **How the flow works**


The request flow is straightforward.


First, an unpaid client requests the protected route:


Copied!


```text
curl -i http://127.0.0.1:7676/protected-route
```


The app responds with 402 Payment Required.


Then the paid client calls the helper endpoint:


Copied!


```text
curl -sS -X POST http://127.0.0.1:7676/api/fetch-protected-route | jq
```


Inside Compute, the demo:


1.


Loads the payer private key from config.


2.


Creates an x402 client.


3.


Registers the EVM payment scheme.


4.


Requests the protected route.


5.


Receives the x402 challenge.


6.


Signs and pays the challenge.


7.


Retries the request.


8.


Returns the protected content after verification.


The protected route itself is guarded by x402 middleware. If the request is not verified, the content is not served.


That fail-closed behavior is important. A direct unauthenticated request should return 402. If the middleware fails, the route should not accidentally return 200.


### **Practical Applications**


This is not meant to be a production payment architecture by itself.


It is a working proof that Fastly Compute can sit in front of an HTTP resource and enforce an x402 payment requirement at the edge.


That opens the door to more practical patterns:


-


API endpoints that charge per request.


-


Agent-to-agent or agent-to-service transactions.


-


Premium content or data access without a full checkout session.


-


Device or service-initiated purchases.


-


Origin protection where unpaid traffic never reaches the backend.


The same pattern could be paired with identity, bot controls, rate limits, logging, WAF policy, and origin routing.


### **Production notes**


For a demo, it is fine to use a test payer key and testnet USDC.


For production, move sensitive configuration into[Fastly Secret Store](https://www.fastly.com/documentation/reference/api/services/resources/secret-store/) rather than baking it into the Compute package. For higher-value flows, use Secret Store with a dedicated signer, wallet provider, or payment service so Compute enforces the payment policy while key custody stays appropriately constrained.


## **Why this matters**


The web is getting more automated.


Agents, APIs, crawlers, devices, and services are going to call each other more often. Many of those calls will need authorization. Some will need payment. For small transactions, a traditional checkout flow is too heavy.


x402 gives HTTP a simple payment challenge.


Fastly Compute gives that challenge a practical place to run.


The demo shows the core pattern: request, challenge, pay, verify, serve.


That is enough to start building real automated payments flows at the edge.


*Want to see the full implementation? Check out*[the complete demo repository on GitHub](https://github.com/sflagg-fastly/fastly-x402-http-demo) *.*
