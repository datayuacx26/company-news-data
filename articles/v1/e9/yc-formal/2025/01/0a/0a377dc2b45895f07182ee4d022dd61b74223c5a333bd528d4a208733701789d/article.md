---
schema_version: "1.0.0"
document_id: "0a377dc2b45895f07182ee4d022dd61b74223c5a333bd528d4a208733701789d"
company_key: "yc-formal"
company: "Formal"
source_id: "yc-formal-news-import-a5b088b50d89"
canonical_url: "https://www.formal.ai/blog/mongodb-protocol-proxy/"
published_at: "2025-01-03T00:00:00+00:00"
first_seen_at: "2026-07-27T09:22:31.452672+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:c28241cbd32215b3f37e0945c9559cde4c7c8787558d8054117fcda56a0e325f"
---

# Building a Protocol-Aware Reverse Proxy: Lessons from Handling MongoDB's Unique Framing

The **Formal** Connector is a protocol-aware reverse proxy that interprets various wire-protocols (HTTP, Postgres, MySQL, Kubernetes, and much more) to provide security teams better visibility and control of their data flows.


Customers often start using Formal for one type of data store and then over time Formal covers more data stores in their stack. To keep the Formal deployment simple we built a feature that enables the Formal Connector to listen on a single port for multiple technologies. This feature works by detecting the incoming wire-protocol and then automatically loading the right wire-protocol interpreter.


## **Initial approach**


The initial approach involved reading the first few bytes of any TCP connection to identify the protocol before forwarding the packets to the appropriate protocol interpreter:


The` \[\]byte` data read from the connection was then passed and explicitly written before using` io.Copy` to forward the traffic. This approach worked well for most protocols but encountered issues with MongoDB due to its unique protocol framing.


MongoDB expects specific framing for its messages. Sending the initial bytes separately disrupted the framing structure, resulting in communication errors. The misaligned initial bytes caused failed connections and protocol misinterpretations.


## **The Solution**


The issue was resolved by wrapping the initial bytes in a` MultiReader` alongside the connection. This adjustment ensured that the proxy transmitted both the initial bytes and the rest of the connection stream in a single operation, preserving MongoDB’s expected message format.


Here’s the updated implementation:


The new implementation wraps the connection in` peekConn` , enabling seamless use of` io.Copy` without requiring an explicit` Write` . This adjustment preserved MongoDB’s protocol framing while maintaining functionality for other protocols.


This fix not only resolved the issue but also emphasized the importance of understanding protocol-specific behaviors when performing deep packet inspection and protocol detection. It demonstrated the significance of adaptability and the complexities of network engineering.


If that sounds like an interesting challenge to you,[join us — Formal is hiring](https://jobs.ashbyhq.com/formal) .
