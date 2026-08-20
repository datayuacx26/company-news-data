---
schema_version: "1.0.0"
document_id: "0d5ff53cd3cc721d0a0b0152264b862f07fe3cdb0c5b0e8a3cef4386fd0b1658"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/authzed-http-api"
published_at: "2022-03-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:f4481d2f459b92f72cce361b0c950c2a12a39b6c1a67e4c0961c49c5becb75c5"
---

# Have you met...our HTTP API?

[SpiceDB](https://github.com/authzed/spicedb) is best known for being a Zanzibar-inspired open-source database system for managing security-critical application permissions, but did you know that the *spicedb* binary has additional commands and configurable features?


We recently received some questions about the HTTP API in our[Discord channel](https://authzed.com/discord) so we thought we’d take the opportunity to more formally introduce this particular feature.


The SpiceDB HTTP API is built using[grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway) which takes the[Authzed API protobuf](https://buf.build/authzed/api) definitions, generates a JSON interface, and provides a reverse proxy to respond to HTTP requests.


It is one of the many ways we take advantage of the gRPC ecosystem and you can see exactly how we implement our API service[here](https://github.com/authzed/spicedb/blob/main/internal/gateway/gateway.go) (open source development FTW!)


We generally recommend using our[official client libraries](https://docs.authzed.com/reference/clients#official) as they are more performant but the HTTP API can be a good starting point for getting introduced to SpiceDB and the API.


This post's primary goal will be to guide you through running SpiceDB in your local workspace and making API calls to begin exploring.


## Running SpiceDB


If you haven’t installed SpiceDB yet, get the latest version via homebrew:


shell


1


```text
$   brew install authzed/tap/spicedb


```


shell


1


```text
$   brew install authzed/tap/spicedb


```


Next, print all the available features and configuration options by running:


shell


1


2


```text
$   spicedb  help
$   spicedb  help   serve


```


shell


1


2


```text
$   spicedb  help
$   spicedb  help   serve


```


You'll see quite a few commands and options which will be useful when you get to set up and tune your SpiceDB deployment.


For now, we’ll rely on the helpful defaults in SpiceDB to focus on running the HTTP API server locally.


shell


1


```text
spicedb serve --grpc-preshared-key "secrettoken" --http-enabled


```


shell


1


```text
spicedb serve --grpc-preshared-key "secrettoken" --http-enabled


```


The above command serves the permissions database with the given preshared secret and enables the HTTP API server.


The preshared key will be the secret used as the bearer token to authorize our HTTP API requests.


From the start-up log messages, we can note a few of the helpful defaults:


- The datastore is the in-memory database
- A warning that data in the in-memory database won’t persist beyond the SpiceDB process.
- [Request hedging](https://medium.com/swlh/hedged-requests-tackling-tail-latency-9cea0a05f577) is enabled
- The grpc and HTTP servers will be communicating over plaintext
- The HTTP API server will be listening on port 8443


## Browse the API


An OpenAPI spec is generated along side the API proxy so we can use standard tooling such as swagger to browse:


[https://petstore.swagger.io/?url=https://raw.githubusercontent.com/authzed/authzed-go/main/proto/apidocs.swagger.json](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/authzed/authzed-go/main/proto/apidocs.swagger.json)


Here you can view available operations and models which is especially handy for browsing enum values.


## Making API calls


We can make an API call by simply sending a json message over HTTP. Using curl that would look like the follow for the read schema operation:


shell


1


2


3


4


```text
curl -X POST http://localhost:8443/v1/schema/read \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer secrettoken' \
-d '{}'


```


shell


1


2


3


4


```text
curl -X POST http://localhost:8443/v1/schema/read \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer secrettoken' \
-d '{}'


```


Using an HTTP client app can make our exploration easier by helping us craft HTTP requests and visualize responses. For your convenience, we’ve created a[Postman collection](https://www.postman.com/authzed/workspace/spicedb/collection/21043612-16c97691-99f9-436f-a214-d4e14abf1c28) that contains examples of each API operation.


You can run APIs from the collection or fork it into your personal Postman workspace.


In the collection, you’ll find a Schema and a Permissions folder, both containing their respective operations. Generally, each operation includes its endpoint, HTTP method, authorization header, and body JSON.


Note: you'll need to replace the Bearer Token value with the preshared key value you used to start your SpiceDB process.


Lastly, to kick off your API exploration, run the APIs in the following order.


Each request is pre-filled with example body data so you should be able to simply click the Send button.


1. **Schema Write:** writes a sample schema
2. **Schema Read:** view the sample schema
3. **Relationships Write:** writes 2 sample relationships
4. **Relationships Read:** view the sample relationships
5. **Permissions Check:** check a computed permission for a user
6. **Permissions Expand:** view the expanded relationships for the sample document
7. **Lookup Resources:** list the documents with the view permission for a user
8. **Relationships Delete:** delete a relationship that matches the precondition


Hopefully, the sample API requests are sufficient to give you a feel for interacting with SpiceDB.


If you’ve found this guide helpful or have questions as you continue using SpiceDB, drop us a message in our[Discord channel](https://authzed.com/discord) !


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Running SpiceDB
- Browse the API
- Making API calls
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
