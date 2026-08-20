---
schema_version: "1.0.0"
document_id: "861fe2146c6b34140f70bedf053d85fe178c3578981008bada12b9df3994484d"
company_key: "yc-directed-edge"
company: "Directed Edge"
source_id: "yc-directed-edge-rss-7c8e8fe81473"
canonical_url: "https://blog.directededge.com/2012/09/27/bindings-update-that-removes-extra-server-round-trips/"
published_at: "2012-09-28T06:21:55+00:00"
first_seen_at: "2026-07-27T01:55:50.875550+00:00"
fetched_at: "2026-08-20T02:30:11.821086+00:00"
content_hash: "sha256:986b1169407424313061aea3e15b181c96efc44a0e14728802d3a385e4840dff"
---

# Bindings update that removes extra server round-trips

So, we noticed recently that our Java and Python bindings were doing an extra round-trip to our servers for every request. We’ve just done[updates](https://github.com/directededge/directed-edge-bindings) to each of them that removes this.


Per the HTTP spec, when a request uses HTTP BASIC authentication, as our API does, the client is supposed to first send a request without credentials. When it receives a 401 error, then it should retry with the credentials supplied and repeat that process for each and every request.


While that makes sense for interactive use from a browser, for use in a web services API, it makes much more sense to avoid the extra round-trip and send the credentials already in the first request, thus significantly speeding up request times.


You can[get the updated versions of our bindings on Github](https://github.com/directededge/directed-edge-bindings) .
