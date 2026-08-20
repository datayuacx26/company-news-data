---
schema_version: "1.0.0"
document_id: "5e7b73636b04a2eb0d429ce7b577a30cb2f1c9b2b7244ef78ac8f9c2d9153016"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/introducing-opentelemetry-support"
published_at: "2023-11-24T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:f7c6625690861715278dec887641df837c40ef42a4fb754c235e7e1db7da015a"
---

# Introducing OpenTelemetry support

November 24th, 2023[Announcement](https://www.artillery.io/blog/tag/announcement)


# Introducing OpenTelemetry support


Ines Fazlic


Today, we’re announcing native[OpenTelemetry](https://opentelemetry.io/) support for Artillery. Metrics and traces from Artillery tests can now be sent to any monitoring and observability platform that supports OpenTelemetry.


## Why OpenTelemetry?


OpenTelemetry, an open-source observability framework and toolkit for distributed services, is quickly becoming an industry standard for observability. It provides a flexible standardized way of instrumenting, generating, collecting and exporting telemetry data to a variety of destinations. A list of vendors that support OpenTelemetry is available on[https://opentelemetry.io/ecosystem/vendors/](https://opentelemetry.io/ecosystem/vendors/) .


## Using OpenTelemetry in Artillery


OpenTelemetry support is available in Artillery v2.0.0-37 or later, via the official` publish-metrics` plugin.


### Example


In this example configuration we use Artillery to send metrics and traces to[New Relic](https://newrelic.com/) via OpenTelemetry.


```text
config  :
target  :   "http://asciiart.artillery.io:8080"
phases  :
-   duration  :   60
arrivalRate  :   10
plugins  :
publish-metrics  :
# The values for the "endpoint" setting are from NewRelic’s docs:
# https://docs.newrelic.com/docs/more-integrations/open-source-telemetry-integrations/opentelemetry/get-started/opentelemetry-set-up-your-app/#note-endpoints
-   type  :   "open-telemetry"
serviceName  :   "asciiart"
metrics  :
endpoint  :   "https://otlp.eu01.nr-data.net/v1/metrics"
headers  :
api-key  :   "{{ $env.API_KEY }}"
attributes  :
environment  :   "test"
tool  :   "Artillery"
type  :   "Load test"


traces  :
endpoint  :   "https://otlp.eu01.nr-data.net/v1/traces"
headers  :
api-key  :   "{{ $env.API_KEY }}"
attributes  :
environment  :   "test"
tool  :   "Artillery
scenarios:
- name: "Load some ASCII animals"
flow  :
-   get  :
url  :   "/dino"
-   get  :
url  :   "/pony"
-   get  :
url  :   "/armadillo"
```


Metrics and traces from the test will be sent to New Relic:


## Learn more


- [OpenTelemetry plugin documentation](https://www.artillery.io/docs/observability/opentelemetry)
