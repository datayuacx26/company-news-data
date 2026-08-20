---
schema_version: "1.0.0"
document_id: "de89f62621ff215382cf0962d1973d26a642429743c2929b9e24b40b7c67eabb"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-secure-plivo-webhook-urls-in-python/"
published_at: "2022-08-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:a1314eb6039b7b4e0e7ca009a05bcb381313208b1915d54d17795ca2e25367ad"
---

# How to Secure Plivo Webhook URLs in Python

Plivo customers control call flows and business logic based on webhook requests Plivo sends to their servers, which also convey critical information such as call records and message status. To keep these webhooks secure, we enable signature validation using hash-based message authentication codes ([HMAC-SHA256](https://en.wikipedia.org/wiki/HMAC) ) so you can check whether the webhook delivered to your server originated from Plivo. This post walks you through how to secure webhooks and authenticate webhook requests using the Plivo Python SDK.


All voice API requests from Plivo’s platform to your application server contain three custom HTTP headers for[signature validation](https://www.plivo.com/docs/voice/concepts/signature-validation/) : X-Plivo-Signature-V3, X-Plivo-Signature-Ma-V3, and X-Plivo-Signature-V3-Nonce. (For SMS we use X-Plivo-Signature-V2 as of now.) To verify that the request to your server originated from Plivo and that it was not altered en route, you can generate your own request signatures and compare them with the Plivo-Signature headers we pass.


### Validating requests on the application server


Plivo Server SDKs include functions to help you validate incoming requests with X-Plivo-Signature HTTP headers. Here’s some sample code that shows how to validate a signature and return an XML element if the webhook is authenticated.


To start, create a working directory and change into it. Within the directory, create a Python virtual environment and activate it, then install the Python libraries we’ll be using.


Create a file named signature.py and paste into it this code.


When someone makes an outbound call to a destination number, Plivo requests the webhook associated with this code. The code runs and returns a Speak XML element if it can authenticate the request sent. If the validation fails the call will terminate, since no XML is returned to the Plivo server.


Run the file using the command python signature.py to initialize the server locally.


When you’re satisfied, make the application publicly available with[ngrok](https://www.plivo.com/docs/sdk/server/set-up-python-dev-environment-api-xml-voice/#ngrok-setup) . To test it, make a call to the API using[Postman](https://www.plivo.com/docs/voice/quickstart/postman/) and use the ngrok URL as the answer URL.


### Stay secure


Be sure to verify your webhooks using Plivo’s Python SDK and avoid causing any harm to your servers. You can learn more about signature validation logics for[voice](https://www.plivo.com/docs/voice/concepts/signature-validation/) and[messaging](https://www.plivo.com/docs/sms/concepts/signature-validation/) .


By the way, signature validation isn’t the only technique we use to keep webhooks secure. All webhook communications run over HTTPS, which are secured by SSL certificates on your server. We also provide a list of IP addresses for our voice API from which you can expect callbacks. You can[whitelist](https://www.plivo.com/docs/voice/concepts/ip-address-whitelisting/#callbacks-from-plivo) these addresses in your infrastructure.


Haven’t tried[Plivo](https://www.plivo.com/) yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
