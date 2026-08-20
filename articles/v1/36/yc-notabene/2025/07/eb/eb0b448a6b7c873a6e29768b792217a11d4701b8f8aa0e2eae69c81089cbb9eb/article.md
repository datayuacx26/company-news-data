---
schema_version: "1.0.0"
document_id: "eb0b448a6b7c873a6e29768b792217a11d4701b8f8aa0e2eae69c81089cbb9eb"
company_key: "yc-notabene"
company: "Notabene"
source_id: "yc-notabene-news-import-e4585f8c666e"
canonical_url: "https://notabene.id/post/how-decentralized-identifiers-dids-are-shaping-the-crypto-travel-rule-infrastructure"
published_at: "2025-07-24T00:00:00+00:00"
first_seen_at: "2026-07-26T08:22:57.520792+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:a5fb7019d1e129286ac452b439055e81f6e35ec97b6b6b6c9caa0c276cb69d6b"
---

# How Decentralized Identifiers (DIDs) are Shaping the Crypto Travel Rule Infrastructure

In the data requirements for[Originator](https://notabene.id/glossary/originator-vasp) and[Beneficiary VASPs](https://notabene.id/glossary/beneficiary-vasp) in the[crypto Travel Rule](https://notabene.id/crypto-travel-rule-101/what-is-the-crypto-travel-rule) , VASPs must legally identify each other and route the required transaction information to the appropriate VASP. Many VASPs have entities in more than one jurisdiction, and customers or blockchain analytics services likely won’t be able to determine which entity should receive the transaction. As crypto transactions are inherently cross-jurisdictional, using a unified, secure method of VASP name-matching supports seamless Travel Rule data transfers. Decentralized Identifiers (DIDs) present an answer to this problem.


This article dives into DIDs and how Notabene’s market-leading[Travel Rule compliance solution](https://notabene.id/product-travel-rule) uses this innovative technology to identify VASPs.


### What are DIDs?


Globally, individuals and companies use unique identifiers in various contexts: phone numbers, email addresses, social media usernames, ID numbers (for passports, driver’s licenses, tax IDs, health insurance), and product identifiers (serial numbers, barcodes, RFIDs). Additionally, each website has a globally unique URL.


External agencies control most globally unique identifiers; they decide what they refer to and when to cancel them. They're only valuable in specific contexts and by unelected bodies. Traditional unique identifiers may reveal private info and can be fraudulently copied and used by a third party, resulting in "identity theft."


[DIDs](https://notabene.id/glossary/decentralized-identifiers-dids) are a component of a more extensive system — the[Verifiable Credentials](http://notabene.id/glossary/verifiable-credentials-vcs) ecosystem — and are defined in this specification as a novel type of cryptographically verifiable globally unique identifier. DIDs are designed to enable individuals and organizations to generate their own trusted identifiers and prove control over them through authentication using cryptographic proofs, such as digital signatures. The World Wide Web Consortium defines a DID as: “ *A globally unique persistent identifier that does not require a centralized registration authority because it is generated and/or registered cryptographically.”*


DIDs are entity-controlled, and each entity can have as many DIDs as it needs to keep its identities, personas, and interactions separate as desired. These identifiers can be used in a way that makes sense for each situation. They make it possible for entities to interact with other people, institutions, or systems that need them to identify themselves or the objects under their control. DIDs also allow entities to decide how much personal or private information should be shared without depending on a central authority to guarantee the continued existence of the identifier.


### How do DIDs work?


A DID is a simple text string consisting of three parts:


1. the DID[URI](https://www.techtarget.com/whatis/definition/URI-Uniform-Resource-Identifier) scheme identifier,
2. the identifier for the DID method, and
3. the DID method-specific identifier.


Building an Ethereum DID is equal to making an asymmetric key pair. As a mathematical relation between the DID hash and its public key exists, the hash can be derived from a public key and vice versa.


- DID ~= public key


‍


*(An overview of DID architecture and the relationship of the basic components. Source: Everynym. Illustrated by Notabene)*


‍


DIDs are resolvable to DID documents. A DID URL extends the syntax of a basic DID to include other standard URI parts like path, query, and fragment in order to find a specific resource, like a cryptographic public key inside a DID document or a resource outside of the DID document. DIDs create an ecosystem/protocol for cryptographically secure data exchange, verification, and more. Anyone can create a DID because they are self-managed, open-sourced, and decentralized. Learn more on the[W3 website](https://www.w3.org/TR/did-core/#dfn-uri) .


*(A simplified example of a DID. Source: The*[World Wide Web Consortium](https://www.w3.org/TR/did-core/) *(W3C.)*


‍


### **How are DIDs used in relation to Travel Rule/VASP communication?**


When Alice sends a transaction to Bob, she likely doesn’t know if his account is with Bitstamp Singapore, Bitstamp USA, or any other Bitstamp entity. She simply inputs his alphanumeric address and sends the transaction. A normal crypto transaction flow puts the onus on providers to determine which entity controls Bob’s address.


Leveraging DIDs, Bitstamp would create separate DIDs for each entity, which removes the VASP name-matching operational friction without asking the end user to submit unknown information.


‍


Multi-entity issue when identifying VASPs. (Source: Notabene)


‍


#### DIDs allow for the following in relation to Travel Rule compliance:


- **Matching a blockchain address to the correct VASP entity.** Blockchain analytics services only return the[VASP](https://notabene.id/crypto-travel-rule-101/vasps-virtual-asset-service-providers) name. Having a separate DID for each entity solves difficult counterparty identification by returning Bitstamp EU, Bitstamp DE, or Bitstamp USA, etc.
- **Using DIDs to define a standard market practice for including legal entity identifiers (LEIs) in payment messages as recommended by the FATF.** FATF notes that LEIs could be used as additional information in payment messages without changing the current message structure. ([FATF 2021b](https://www.fatf-gafi.org/media/fatf/documents/recommendations/Updated-Guidance-VA-VASP.pdf) , p. 60, para 189)


- **Creating a decentralized SWIFT code network.** The traditional banking world uses SWIFT codes to identify companies. Keeping in line with the ethos of the industry, DIDs can be used as a standardized decentralized way to identify VASP entities.


‍


### **How does Notabene use DIDs?**


We use DIDs as LEIs for every crypto company or financial institution in our[Notabene VASP Network](https://app.notabene.id/directory) . DIDs allow companies to create separate identities for each entity, meaning that if there are 10 Bitstamp entities, each one would have its own DID. DIDs cut out the painstaking process of name-matching during regulated data transfers.


In the Travel Rule context, DIDs resolve into a document that specifies:


- VASP website
- VASP’s public key
- Which protocol a VASP supports, etc.


‍


Multi-entity support (Source: Notabene).


### **Who provides DIDs for Notabene?**


We work closely with Veramo to encrypt the personally identifiable information (PII) data flow for VASP-to-VASP communication.[Veramo](https://veramo.io/docs/basics/introduction/) is a JavaScript framework that simplifies the use of cryptographically verifiable data in software applications.


**How the PII Escrow flow works:**


- Veramo securely encrypts PII data flow when sending Travel Rule data transfers from VASP A to VASP B.
- Only the Beneficiary VASP can decrypt the data.
- This supplies security and comfort because, in any event of a data leak, no one can decrypt it but the recipient.


**Hybrid PII Encryption flow:**


- The Originator VASP sends two versions of the Travel Rule data transfer, one for us to decrypt and one for the Beneficiary VASP.
- Notabene accesses the version intended for us to perform sanction screening.
- As a SOC2-[compliant](https://notabene.id/training-program) company, we use unique keys per customer to minimize potential hacking cases, leaks, etc.


End-user data security and privacy are a part of our fundamental values at Notabene. Our[SafePII service](https://notabene.id/post/notabene-launches-safepii-enhances-trust-security-and-privacy-in-travel-rule-transactions) presents a unique data escrow system for safely transmitting encrypted customer information only when a beneficiary institution confirms ownership of a blockchain address and fulfills specific rules. Currently, Notabene securely stores PII during the hybrid PII encryption flow. However, customers are open to running their own PII service. Learn more about[Notabene’s commitment to security](https://notabene.id/security) .
