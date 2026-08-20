---
schema_version: "1.0.0"
document_id: "73cc26e6ed6bdbe1ae3bfaf77b3801663af6787b99bd672894a5814a8047689b"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2019/01/15/gremlin-aws-sigv4"
published_at: "2019-01-15T05:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:416220d18c2a6c4d76f729eaa60d8447b9b519dc5f212aa89bcacd107e668707"
---

# Gremlin AWS SigV4

We recently open-sourced an[extension](https://github.com/shutterstock/gremlin-aws-sigv4) of Apache’s TinkerPop JavaScript driver compatible with Amazon’s Identity and Access Management (IAM) database authentication.


We implemented a robust and scalable solution using Amazon’s Neptune graph database and Node-based API to support the[Editorial](https://www.shutterstock.com/editorial) business. However, we faced a challenge when it became evident that Amazon’s IAM was the only supported authentication method for Neptune, and there was no official JavaScript driver for it.


When using IAM database authentication, each request must be signed with AWS Signature Version 4, and this signature must be provided in the headers. The problem is that the default TinkerPop Javascript driver does not allow you to provide headers when connecting to the database.


We were be able to solve the header and authentication issues by calculating the signature from given parameters and providing it automatically to another[websocket library](https://www.npmjs.com/package/websocket) which allows us to inject headers. Thus we were able to natively connect our Node.js applications to our AWS Neptune graph database.


The driver is publicly available on[npm](https://npmjs.com/package/gremlin-aws-sigv4) and its source code and documentation are on[GitHub](https://github.com/shutterstock/gremlin-aws-sigv4) .
