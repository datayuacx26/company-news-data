---
schema_version: "1.0.0"
document_id: "41ab40679549ac809a88d9bf553d0d5f4db27660ae43bf437149d6357c309c1c"
company_key: "yc-convoy-2"
company: "Convoy"
source_id: "yc-convoy-2-news-import-6ac8b1bcd379"
canonical_url: "https://www.getconvoy.io/changelog/broker-message-id-tracking-and-mtls-support"
published_at: null
first_seen_at: "2026-07-25T00:37:26.853065+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:235f0b17f834463f7b1ef545a98a3a43e79ba8300c293bf9d75415b2c14db678"
---

# Broker Message ID Tracking & mTLS Support

We shipped three features this week that make Convoy more reliable for production workloads: broker message ID tracking, mTLS client certificates, and better content type handling.


**Broker Message ID Tracking**


When events flow through message brokers like Kafka, Google Pub/Sub, SQS, or AMQP, tracking them back to their source message used to be tricky. You'd see an event in Convoy but couldn't easily trace it to the original broker message.


We now capture and store the broker message ID for every event ingested from these brokers. This means you can filter events and deliveries by their original broker message ID, making debugging and tracing much simpler.


**mTLS Client Certificate Support**


Some endpoints require mutual TLS (mTLS) authentication. You can now configure client certificates directly on your endpoints. When Convoy delivers webhooks, it presents the client certificate for authentication.


Configure your client certificate and key in the endpoint settings. This feature requires an enterprise license.


**Form-Encoded Content Type**


We added support for` application/x-www-form-urlencoded` content types. If your endpoint expects form-encoded data instead of JSON, you can now set that in the endpoint configuration.


**Other Improvements**


- **TLS for Redis** : Redis connections now support TLS encryption. Set` CONVOY_REDIS_TLS_ENABLED=true` to enable it.
- **Circuit breaker notifications** : Fixed an issue where email notifications weren't being sent when circuit breakers activated.
- **Configurable root path** : You can now configure the root path for Convoy deployments using` CONVOY_ROOT_PATH` .


All features are available now. See the[sources documentation](https://www.getconvoy.io/docs/product-manual/sources) for details on broker message tracking, and the[endpoints documentation](https://www.getconvoy.io/docs/product-manual/endpoints) for mTLS configuration.
