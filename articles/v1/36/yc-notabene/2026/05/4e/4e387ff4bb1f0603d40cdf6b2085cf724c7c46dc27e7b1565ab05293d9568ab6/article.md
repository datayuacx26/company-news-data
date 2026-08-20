---
schema_version: "1.0.0"
document_id: "4e387ff4bb1f0603d40cdf6b2085cf724c7c46dc27e7b1565ab05293d9568ab6"
company_key: "yc-notabene"
company: "Notabene"
source_id: "yc-notabene-news-import-e4585f8c666e"
canonical_url: "https://notabene.id/post/the-undo-button-for-crypto-solving-the-unilateral-transaction-problem"
published_at: "2026-05-14T00:00:00+00:00"
first_seen_at: "2026-07-26T08:22:57.520792+00:00"
fetched_at: "2026-07-28T21:45:24.644708+00:00"
content_hash: "sha256:9b624710b35e4a8a01b67198beab7224d47ec80bf0c3ee34241cd00004720560"
---

# Solving the Unilateral Transaction Problem

### ‍ **The problem with crypto’s one-way street**


Blockchain transactions are natively unilateral. Whoever holds the private keys decides if settlement happens. Once a transaction is broadcast and confirmed on-chain, the receiver doesn’t get a “yes” or “no” moment. There is no native ability to accept or reject a payment.


When an unwanted transaction is received, there is also no straightforward way to send it back even though, in many cases, returning funds is a mandatory regulatory requirement.


For example, under the EU Travel Rule regime, if a beneficiary Crypto-Asset Service Provider detects *post-settlement a* transaction that lacks the required Travel Rule information to unambiguously identify the parties, it must return the funds to the originator.


In practice, however, returning funds is anything but simple. Institutions face a minefield of liability. They cannot assume that the original sending address is able to receive a return. If that address is not prepared for this purpose, the returned funds may be permanently lost.


The scale of this problem is significant. The Joint Money Laundering Steering Group (JMLSG)—the UK body providing guidance on AML compliance—explicitly recognizes this, stating that crypto asset businesses *“should consider the risks and complexities \[of returning funds\] prior to making a return, as it may create operational challenges … to reattribute it to the originator.”*


### **The long-awaited "undo button" for crypto**


To solve this, Notabene has introduced **Revert** —a post-settlement control layer for digital asset transactions, enabling institutions to safely coordinate, authorize, and complete returns of funds across counterparties. This is effectively an "undo button" for crypto transactions.


Here’s how it works:


When a beneficiary institution needs to return funds, it initiates a revert request. The originator is notified of the request and can respond by authorizing the return and providing a verified wallet address where the funds should be sent. The beneficiary can then execute the return with confidence, while both sides retain full visibility and auditability across the entire lifecycle.


To be clear: **this is an innovation in trust and coordination** . There is no change to how blockchains natively work. Settlement remains immediate and irreversible according to first principles of blockchain design. However, within a network of cooperative institutions, we can introduce a novel new control layer to correct mistakes.


This functionality is built on the[Transaction Authorization Protocol (TAP)](https://notabene.id/tap) , an open standard for communicating about transactions. Any adopter of TAP, regardless of a commercial relationship with Notabene, can participate in coordinated return-of-funds flows. This is infrastructure for the entire industry, not just Notabene customers.


### **The importance of pre-transaction authorization**


Revert addresses a necessary gap today, but it is not on its own a holistic solution to end-to-end transaction orchestration.


When counterparties verify compliance data, assess risk profiles, and explicitly authorize transactions *before* settlement, the need for post-settlement remediation and fund returns largely disappears.


The Notabene Network has already powered **over $2 trillion in compliant transaction volume** using this approach. Pre-transaction authorization is becoming a standard, and we are advancing it further by[enforcing authorization by design within Notabene Flow payments](https://notabene.id/post/how-notabene-flow-makes-authorization-and-travel-rule-a-built-in-component-of-stablecoin-payments) .


But standards take time to become universal. Until pre-transaction authorization is widespread across all digital asset transactions, the industry needs a way to handle edge cases: non-compliant counterparties, missing information, and operational mistakes.


That’s the safety net that Revert provides.


### **Trust, coordination, and compliance infrastructure**


Notabene's Revert functionality demonstrates the power of coordination and an end-to-end transaction control layer when institutions transact within shared standards and trusted networks.


As digital asset payments evolve, infrastructure like this is what turns crypto rails into something businesses can actually rely on.


With the introduction of Revert, we’re covering all the bases so that compliant, coordinated transactions become the default.
