---
schema_version: "1.0.0"
document_id: "c59f4d53a4b786b8164903ae3bc1a14b70c98505fe7873787ddc982024dbd3f9"
company_key: "yc-increase"
company: "Increase"
source_id: "yc-increase-news-import-16282b6988d7"
canonical_url: "https://increase.com/articles/cash-reconciliation"
published_at: "2024-11-21T00:00:00+00:00"
first_seen_at: "2026-07-24T11:51:21.619731+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:c3c0459e2dfc9577514ad15358933c5fa69b1c55ecafb70c90cddefc58bbccc4"
---

# Cash reconciliation: Aligning transaction activity with the Federal Reserve's data — Increase

Increase operates as a[parallel core](https://increase.com/articles/modernized-banking-core) for our bank partners. We connect directly to the Federal Reserve and process wires, ACH transfers, and checks using a dedicated routing number. An important associated responsibility is to reconcile the activity in our systems with the cash movement at the Federal Reserve.


While banks are responsible for ledgering the activity of their customers, the Federal Reserve is the source of truth on where dollars *actually sit* . Ensuring precise reconciliation between bank records and the Federal Reserve’s data is essential to maintain a clear and accurate view of customer funds. Each day, we compare reports provided by the Federal Reserve with our independently generated expectations of those reports to ensure alignment.


## Federal Reserve Accounts


Every chartered financial institution is[eligible for an account](https://www.federalreserve.gov/monetarypolicy/reserve-maintenance-manual-account-structure.htm) at their regional Federal Reserve Bank. In many ways, this account is just like any other bank account. There’s an account number (generally the bank’s main routing number), an interest rate (the[Interest on Reserve Balances](https://www.federalreserve.gov/monetarypolicy/reserve-balances.htm) ), an API (ACH, wires, etc), a web interface, and statements. The Federal Reserve makes these statements available in a machine-readable file they call[Financial Institution Reconcilement Data](https://www.frbservices.org/resources/financial-services/accounting/rules-regulations.html) .


This file has detailed information about debits and credits posting to the account. If a bank has multiple routing numbers, the Financial Institution Reconcilement Data separates activity by routing number. There are many different transaction codes, ranging from` 10010` (“Transfer of Funds”, used for wires), to` 63010` (“Coin Transaction”), to` 84710` (“Dodd Frank Assessment Fee”)—you can[download a full list of transaction codes](https://www.frbservices.org/binaries/content/assets/crsocms/resources/rules-regulations/amg-appendix-b-092923.xlsx) from the Federal Reserve.


## Expectations


To support reconciling Increase activity to this Financial Institution Reconcilement Data, we generate what we call an “Expected Cash Transaction Contribution” whenever something happens in our system that we expect to contribute to some cash transaction. We check daily that our expectations match the actual reality of the Federal Reserve reporting in both directions:


- All expectations correspond to reported cash movement
- All reported cash movement corresponds to expectations


This means that our code needs to deeply understand and model how the Federal Reserve works to generate expectations that can later be matched to the actual reporting. For wires, this is straightforward: there is one Financial Institution Reconcilement Data (FIRD) transaction for each incoming / outgoing wire. This transaction includes the wire’s Input Message Accountability Data ([IMAD](https://increase.com/documentation/api/wire-transfers#wire-transfer-object.submission.input_message_accountability_data) ), which uniquely identifies it. We generate an expectation including the IMAD that is later matched to the actual reporting through a point-lookup.


Things are substantially more complicated for ACH. Whereas there is one FIRD transaction per wire, for ACH the reported transactions are aggregates of many underlying ACH entries, and not just a single aggregate. The FIRD transaction an ACH entry will end up contributing to depends on multiple factors.


1. Whether the entry was a credit or debit
2. Whether the entry was a forward or return
3. Whether the entry was originated or received
4. Whether the entry was same-day or future-dated
5. The specific same-day window it settled in or the date the entry was processed (for a future-dated entry). “Date” here—of course—referring not to a calendar date in Eastern Time or UTC but rather an “ACH–date” which runs 2:15AM Eastern Time to 2:15AM Eastern Time.


Our logic that generates expectations considers all this. We reconcile an ACH FIRD transaction by finding the many expected contributions that correspond to it, adding up their amounts, and making sure the sum matches the aggregated amount reported by the Federal Reserve.


The complexity of the Federal Reserve’s check settlement service, Check 21, is somewhere between ACH and wires. There are fewer transaction codes — 4 rather than 26. But we need to account for edge cases in how the same-day funds availability cutoffs are defined. For example, there can be different same-day deadlines for checks drawn on routing numbers starting with[“1214” or “3214”](https://www.frbservices.org/resources/fees/check-electronic-check-collection-2026#3ftn) , or in lists[published by the Federal Reserve](https://www.frbservices.org/resources/fees/check21-endpoint-listing.html) .


## Operating confidence


While there’s a lot of complexity to encode, it’s all complexity essential to accurately predicting how events in our system impact cash movement at the Federal Reserve. At Increase, we love getting deep into these details, understanding them, then automating that understanding. Reconciling these predictions to the actual reporting gives us confidence that our modeling is correct and that our system is working as expected.


We have alerts that[continuously monitor](https://increase.com/articles/automated-invariant-monitoring) the reconciliation process, ensuring that our expectations align with actual outcomes. While we have numerous other alerts that would trigger if, for instance, we failed to process an incoming ACH file or a wire we think was sent was actually rejected, these alerts provide an additional control and layer of detection in case those earlier mechanisms miss something.


Among the many reconciliations that we perform, this one confirms that we are correctly performing a critical role of a bank core: interacting with our partner banks’ Federal Reserve accounts. By rigorously matching every expected transaction with the Federal Reserve’s reporting, we’re able to identify discrepancies instantly, confirm the accuracy of our models, and ensure seamless operation for our partners. This ongoing alignment doesn’t just give us operational confidence—it embodies our commitment to transparency and reliability, setting a high standard for how banking infrastructure should operate.


At Increase, we believe that mastering the details and automating with precision is the way to advance how financial systems connect and operate.
