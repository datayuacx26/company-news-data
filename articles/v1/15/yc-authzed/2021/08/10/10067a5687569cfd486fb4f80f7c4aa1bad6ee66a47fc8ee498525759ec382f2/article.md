---
schema_version: "1.0.0"
document_id: "10067a5687569cfd486fb4f80f7c4aa1bad6ee66a47fc8ee498525759ec382f2"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/prom-authzed-proxy"
published_at: "2021-08-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:1e5ec9bedded4cb53e2e0652a2db7c0bce1e10b4d26b8a942b48021023c93dd2"
---

# Securing Prometheus with prom-authzed-proxy

### Today, we're open sourcing[prom-authzed-proxy](https://github.com/authzed/prom-authzed-proxy) !


[prom-authzed-proxy](https://github.com/authzed/prom-authzed-proxy) is a proxy for[Prometheus](https://prometheus.io/) that authorizes the request's[Bearer Token](https://datatracker.ietf.org/doc/html/rfc6750#section-2.1) with[Authzed](https://authzed.com/) and enforces a label in a PromQL query.


[Authzed](https://authzed.com/) is a database and service that stores, computes, and validates your application's permissions.


Developers create a schema that models their permissions requirements and use a client library, such as this one, to apply the schema to the database, insert data into the database, and query the data to efficiently check permissions in their applications.


## Previously on...


[Last week](https://authzed.com/blog/observability-shouldnt-be-private) , I wrote about how we're maintaining a single observability stack for both internal **and external usage** .


For SaaS products to truly become Cloud Native, they can no longer be treated as a black-box -- **even services you don't run yourself must also be made observable** .


To that end, there's now a metrics tab for each Permissions System in the[AuthZed Serverless](https://app.authzed.com/) that exposes latency metrics. When a user visits that page, their browser makes Prometheus API requests to the very same Prometheus deployment that we use internally.


To secure these requests, we built[prom-authzed-proxy](https://github.com/authzed/prom-authzed-proxy) . And best of all, there is nothing in prom-authzed-proxy that makes it specific to our usage: anybody can use it to secure Prometheus. The proxy was built to have absolutely no assumptions about the[schema](https://docs.authzed.com/guides/schema) of the Permission System or the metrics in Prometheus!


## Prior Art


Proxying queries to Prometheus is not a new concept. In fact, it is the recommended way that the Prometheus community prescribes to implement secure querying.[prom-label-proxy](https://github.com/prometheus-community/prom-label-proxy) is a Prometheus community project that enforces that a particular label is present within a Prometheus request. By running prom-label-proxy, one can guarantee that requests are filtered down to only results from their Permissions System. However, this proxy has no means of authorizing that the request has access to the Permissions System that will be enforced on the query.


This is where[prom-authzed-proxy](https://github.com/authzed/prom-authzed-proxy) comes in: prom-authzed-proxy is configured at startup with almost all of the arguments required to perform an[Authzed permissions check](https://docs.authzed.com/v0/api#aclservicecheck) . The remaining arguments are parsed off of each request to the proxy: the contents of the[Bearer Token](https://datatracker.ietf.org/doc/html/rfc6750#section-2.1) is used as the[Subject ID](https://docs.authzed.com/concepts/terminology#subject) and the value of query parameter specified in the proxy's` --object-id-parameter` flag is used as the[Object ID](https://docs.authzed.com/concepts/terminology#object-id) . If the permissions check fails, the proxy returns an HTTP 403, otherwise it processes the request with the same library that handles requests internally to prom-label-proxy.


## Next Time on...


In the future, we'll break down how to scale Prometheus to handle the cardinality imposed by labeling all of our metrics with each Permissions System.


Keep your eyes peeled for more blog posts or **[subscribe to monthly product updates](http://eepurl.com/hEeb6z)** direct to your inbox.


Huge thanks to[Frederic Branczyk](https://twitter.com/fredbrancz) and[Bartłomiej Płotka](https://twitter.com/bwplotka) who jump-started our understanding of prior art in the Prometheus ecosystem.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Today, we're open sourcing prom-authzed-proxy! <iframe style={{marginBottom: '-7px'}} src="https://ghbtns.com/github-btn.html?user=authzed&repo=prom-authzed-proxy&type=star&count=true&size=large" frameborder="0" scrolling="0" width="170" height="30" title="GitHub"></iframe>
- Previously on...
- Prior Art
- Next Time on...
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
