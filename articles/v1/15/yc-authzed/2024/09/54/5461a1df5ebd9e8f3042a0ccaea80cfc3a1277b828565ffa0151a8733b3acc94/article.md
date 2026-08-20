---
schema_version: "1.0.0"
document_id: "5461a1df5ebd9e8f3042a0ccaea80cfc3a1277b828565ffa0151a8733b3acc94"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/spicedb-relationship-integrity"
published_at: "2024-09-30T17:41:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:59:23.866660+00:00"
content_hash: "sha256:22f8efed21ec02ccbed4028039d7ab0835d0eaa51e107847ec84553dcebce431"
---

# Relationship integrity

One of the major differentiators between Google’s Zanzibar model of authorization and other, more basic, approaches to authorization is its reliance on relationships. Unlike role-based access control or attribute-based access control, Zanzibar uses a relationship-based access control paradigm, where the relationship between pairs of objects (known as “resources” and “subjects”) defines how permissions are granted.


SpiceDB is AuthZed’s open source implementation of Google Zanzibar, including the full set of Zanzibar capabilities. Like Zanzibar (which uses Spanner under the covers), SpiceDB stores relationships in an underlying datastore (Postgres, MySQL, Spanner or CockroachDB) and relies upon this stored data to compute the answers to permissions questions.


Since SpiceDB stores and reads its relationship data from an external datastore, there exists a remote, but real, possibility that relationships within that external datastore could be modified without SpiceDB’s knowledge. If this were to happen, SpiceDB could return incorrect or even malicious answers to permissions questions.


While this scenario is incredibly unlikely in a secured environment such as AuthZed’s Dedicated instances, it is possible in environments that are more open, such as self-hosted SpiceDB clusters where the datastore is accessible from outside of a VPC.


Thus, to address this use case, SpiceDB v1.36.0 introduces a new feature: relationship integrity.


#### Relationship Integrity


Relationship integrity allows for each relationship written into the backing datastore to be signed by a key known only to SpiceDB. When relationships are then read back from the datastore, SpiceDB validates that the signature written for the relationship matches that computed by the same key at this moment in time: if the signature does not match in any way, then the entire relationship data load is rejected and an error is returned to the caller:


By ensuring that all relationships written to the backing datastore are signed by a key only known to SpiceDB, administrators can be certain that all relationships written were written by a trusted instance of SpiceDB.


Currently, relationship integrity is only supported with the CockroachDB datastore driver. Based on community feedback, we may extend support to the other datastore drivers in the future.


Getting started with Relationship Integrity


1.


Generate a new key for use by relationship integrity. The key must be HMAC compatible:


1


```text
openssl rand -hex 256 > spicedb.key


```


1


```text
openssl rand -hex 256 > spicedb.key


```


2.


Run spicedb migrate with the proper flags (must be on a new, empty datastore):


1


2


3


```text
spicedb migrate head
--datastore-relationship-integrity-enabled --datastore-relationship-integrity-current-key-id="mykeyid"
--datastore-relationship-integrity-current-key-filename="spicedb.key"


```


1


2


3


```text
spicedb migrate head
--datastore-relationship-integrity-current-key-filename="spicedb.key"


```


3.


Run spicedb serve with the proper flags:


1


2


3


```text
spicedb serve ...otherflagshere...
--datastore-relationship-integrity-current-key-filename="spicedb.key"


```


1


2


3


```text
spicedb serve ...otherflagshere...
--datastore-relationship-integrity-current-key-filename="spicedb.key"


```


SpiceDB will now be running with relationship integrity enabled.


#### Rotating a key


Relationship Integrity in SpiceDB supports the concept of rotating keys by registering a new key as the “primary” while still retaining the old key as a fallback.


1.


To rotate a key, first generate the new key, and then run SpiceDB with both keys configured:


1


2


3


4


```text
spicedb serve ...otherflagshere...
--datastore-relationship-integrity-enabled --datastore-relationship-integrity-current-key-id="newkeyid"
--datastore-relationship-integrity-current-key-filename="newkey.key"
--datastore-relationship-integrity-expired-keys='{"key_id": "mykeyid", "key_filename": "spicedb.key", "expired_at": "12/31/2024 12:00"}'


```


1


2


3


4


```text
spicedb serve ...otherflagshere...
--datastore-relationship-integrity-current-key-filename="newkey.key"


```


The` --datastore-relationship-integrity-expired-keys` argument is a set of JSON blobs, each containing the key information for an expired key.


When SpiceDB reads the relationships from the datastore, it will verify that an expired key was only used to sign a relationship before the expiration date+time. Any use of the expired key for a relationship after that point-in-time will fail.


2.


(if necessary) Once a new key has been in place, the WriteRelationship call can be used with TOUCH to re-sign each of the relationships in SpiceDB. This will need to be done for every relationship, if desired.


#### What about deletions?


Relationship Integrity currently only supports validation of written relationships: an attacker could still delete relationships from the underlying datastore and impact computations in SpiceDB. However, the attack surface here is much lower, as deletion of relationships can rarely grant elevated access (unless an - operator is used).


If this is a concern, we recommend avoiding the use of the - operator with relationship integrity.


#### Conclusion


Your end users rely on your permissions system to safeguard their data with correct authorization answers. Correctness goes beyond computation- the system must have correct information from the start. By utilizing relationship integrity, you can protect your SpiceDB permissions system from inadvertent or malicious modifications to their relationship data.


Let us know if you have feedback or questions! Reach out to[sales](https://authzed.com/call) or[chat](https://authzed.com/discord) with community members in Discord.


On this page


- Relationship Integrity
- Rotating a key
- What about deletions?
- Conclusion


## Related


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Cormac Foster · Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)


[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. Cormac Foster · May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)
