---
schema_version: "1.0.0"
document_id: "6e22c58911ccb276c5355f0c67bfb0ac5f379a60d115489a510695e62385e8f0"
company_key: "yc-increase"
company: "Increase"
source_id: "yc-increase-news-import-16282b6988d7"
canonical_url: "https://increase.com/articles/modernized-banking-core"
published_at: "2024-08-01T00:00:00+00:00"
first_seen_at: "2026-07-24T11:51:21.619731+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:4162e6074a529799d5c72551012b46071196b27d0a1767c3656db1ddb4be8118"
---

# Increase: a modernized banking core — Increase

## Bank cores


A bank core is one of those bedrock systems of modern society (birth registry, electronic healthcare records, air traffic control, etc) that we all want to see operate reliably and not be particularly exciting.


A bank core serves as the database and related logic responsible for maintaining the transactions and balances of a bank. It can also be responsible for services like account holder data, payment network interactions, and reconciliations. It is designed to be highly reliable and scalable while being in compliance with regulatory standards.


In the US, the most widely used bank core providers are[FIS](https://www.fisglobal.com/) ,[Fiserv](https://www.fiserv.com/) , and[Jack Henry](https://jackhenry.com/) . The core providers themselves have many different cores01 targeting sizes or cohorts of financial institutions. In fact, there’s even an open source Apache core project, if you’re interested in seeing code2 .


## Technical limitations


Many financial technology companies building products in partnership with banks run into technical constraints that start with bank cores. This is for a couple of reasons.


Many cores weren’t designed or built with programmatic interaction in mind. Even if a financial technology company integrates into a traditional core, the core was likely built with very different use-cases in mind and therefore very different assumptions. For example, it often takes a day to open a new bank account, and each account has a single account number as its primary identifier. Payments are typically sent via batch files, rather than via API. Visibility into transaction details is limited.


A bank could gain more technical flexibility and data visibility by converting to a more modernized core3 . However, migrating from one core to another is a large project involving months of planning, as well as coordination with the Federal Reserve and other payment network operators. This also fails to solve for the challenges around programmatic data interaction between the core and the financial technology company; there’s substantial product work required to expose a sensible API to clients, and many modernized cores haven’t been designed with client needs in mind.


“Many cores weren’t designed or built with programmatic interaction in mind. Even if a financial technology company integrates into a traditional core, the core was likely built with very different use-cases in mind and therefore very different assumptions.”


To solve for client-facing APIs, a bank could partner with a banking-as-a-service or “middleware” provider. These providers do the heavy lifting of integrating to the bank’s traditional core, and offer abstracted account creation and money movement to financial technology companies via API. However, they’re still dependent on the traditional core for access to underlying network data and account structures. They can’t solve for limited data visibility and inflexible account structures.


## Running a parallel core


One solution to this problem is running a parallel core. A financial institution can maintain its existing core while simultaneously adding a new core. The two cores can interact as necessary (either by exchanging data directly or separately interacting with upstream or downstream services).


This is what Increase does: we operate as a parallel core for our bank partners. We also expose functionality of the underlying depository rails via powerful APIs.


Increase allows bank partners to offer financial technology clients programmatic access to banking and card rails with flexible implementation structures and granular network details.


## Flexible implementations


With Increase, banks can enable financial technology clients to synchronously provision[Accounts](https://increase.com/documentation/api/accounts) . Every Account sits on Increase’s core and is able to receive and originate payments. Abstractions like virtual “accounts”—which can help with ledgering but have more limited functionality—aren’t necessary. Importantly, this also means the bank and the client work off a single system of record, and funds in each Account are always reconciled down the penny.


Banks may also offer many[Account Numbers](https://increase.com/documentation/api/account-numbers) pointing to each Account. Clients can programmatically[spin up unique Account Numbers](https://increase.com/documentation/accounts-and-account-numbers) to track and reconcile inbound payments into one Account.


“We operate as a parallel core for our bank partners. We also expose functionality of the underlying depository rails via powerful APIs.”


## Fine grained network details


With direct connections to the US payment rails, Increase exposes all the details and functionality of the underlying financial network by API,[without abstractions](https://increase.com/articles/no-abstractions) .


**For example:**


1. Every inbound wire allocated to an Account contains an Input Message Accountability Data (“IMAD”), a unique identifier assigned to each Fedwire payment made through the Federal Reserve. Details like this one often get obfuscated when there’s a human-run wire desk involved.
2. When an ACH Transfer is returned, we parse the network message from the Federal Reserve, correlate it to the original transfer, and create a new Transaction to reduce an Account’s balance. A client can immediately view the return details and instantly map each return to its initial Transaction.
3. We expose[network updates](https://status.increase.com/#ach-submission-timeline) in real-time. Originated ACH Transfers update when the transfer is submitted to the Federal Reserve, when the transfer is acknowledged by the Federal Reserve, and when it’s expected to settle with the recipient bank.


Your reaction to all this might range from amazement to ‘huh, that’s a neat little tidy-up I guess’ and either way, we agree. To the original observation that bank cores should be reliable and not particularly exciting, we strive to be lovingly boring. The US financial networks4567 have been around for a long time and offer robust functionality and detail for accounts and payments. Increase simply improves the last mile delivery.


## Notes


1.


Silverlake:[https://jackhenry.dev/open-enterprise-api-docs/enterprise-soap-api/api-provider/silverlake/](https://jackhenry.dev/open-enterprise-api-docs/enterprise-soap-api/api-provider/silverlake/)↩


2.


DNA:[https://www.fiserv.com/en/solutions/core-platforms-for-banks/dna-platform.html](https://www.fiserv.com/en/solutions/core-platforms-for-banks/dna-platform.html)↩


3.


[https://fineract.apache.org/](https://fineract.apache.org/)↩


4.


An example is Thought Machine:[https://www.thoughtmachine.net/](https://www.thoughtmachine.net/)↩


5.


[https://increase.com/documentation/fedach](https://increase.com/documentation/fedach)↩


6.


[https://increase.com/documentation/fedwire](https://increase.com/documentation/fedwire)↩


7.


[https://increase.com/documentation/check-21](https://increase.com/documentation/check-21)↩


8.


[https://increase.com/documentation/real-time-payments](https://increase.com/documentation/real-time-payments)↩
