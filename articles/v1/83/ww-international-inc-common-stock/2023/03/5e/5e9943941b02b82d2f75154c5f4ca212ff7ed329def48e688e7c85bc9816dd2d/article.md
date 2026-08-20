---
schema_version: "1.0.0"
document_id: "5e9943941b02b82d2f75154c5f4ca212ff7ed329def48e688e7c85bc9816dd2d"
company_key: "ww-international-inc-common-stock"
company: "WW International Inc."
source_id: "ww-international-inc-common-stock-rss-f4e3db081dfb"
canonical_url: "https://medium.com/ww-tech-blog/using-x12-edi-with-red-hat-fuse-apache-camel-apps-1b6536507d27"
published_at: "2023-03-21T21:42:05+00:00"
first_seen_at: "2026-07-20T23:20:13.695109+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:1eb13d2720127d8d8c0bb8c25527c61e2257f442e12aeed37462ebd3c33408de"
---

# Using X12 EDI with Red Hat Fuse/Apache Camel Apps

# Using X12 EDI with Red Hat Fuse/Apache Camel Apps


[Christopher Scalise](https://medium.com/@christopher.scalise?source=post_page---byline--1b6536507d27---------------------------------------)


3 min read


·


Mar 21, 2023


--


A few years ago, we started working with a vendor that uses X12 EDI for sharing data. Although I have been in the IT field for many years, this was my first experience with EDI. For anyone new to X12 and EDI, here is a brief description that I found online:


*X12 is a message formatting standard used for Electronic Data Interchange (EDI) documents. It’s used by trading partners to share business documents in an agreed-upon and standard format. X12 is the most common EDI standard used in the United States.*


Our vendor was using EDI to send payment details to WW via an 835 file. A brief description of the 835 transaction is below:


*The Electronic Remittance Advice (ERA), or 835, is the electronic transaction that provides claim payment information. These files are used by practices, facilities, and billing companies to auto-post claim payments into their systems.*


The message flow was unidirectional — from the vendor to WW and only the 835 transaction was being sent to us.


Research was done to find 3rd party software that we could leverage to easily communicate using EDI. We decided to use the Transformer software provided by Trace Financials as it provided flexibility for parsing EDI 835 transactions as well as dozens of other EDI transactions.


Via the Transformer tool, we were able to parse the 835 transaction, and via scripting provided by the tool, extract the required fields from the transaction that were needed for processing. The Transformer tool enables you to create a service and export a jar file with callable APIs that can be used by an application.


Below is a code fragment that shows the service API being called from a route in Java code. Some items were removed or generalized for privacy reasons.


// Process 835 file to extract information and store it in the database
from(“direct-vm://process835”)
.routeId(“process835”).routeDescription(“process835”)
.pollEnrich().simple( /* file details were removed */ )
.convertBodyTo(String.class)
.log(“calling txfrmr”)
**.to(“txfrmr://ServiceName/APIName”)** .log(“txfrmr called — ${body.size} records”)
.split(body()).unmarshal()
.jacksonxml()
.to(“direct-vm://insertDataToDatabase”);


We configured the Transformer tool to parse the 835 transaction and extract the data fields of interest. Via the tool, we generated a jar file with an API endpoint for the data mapping and used this jar with our deployed application. When an 835 file is sent from the vendor, our route calls the exposed API which parses the transaction and returns the data which we split, unmarshal, and process.


This past year, we started working with a new vendor that also uses EDI transactions. The interface for this vendor is more complex. The message flow is bidirectional and uses multiple EDI transaction types including acknowledgment transactions.


A high-level description of the transaction flow is given below:


- When an order is placed, WW constructs and sends an EDI 850 “purchase order” transaction to the vendor and they respond with an EDI 997 “acknowledgment” with a status indicating if it was successfully processed.
- When the vendor ships the item, they send an EDI 856 “advance shipping notification” transaction to WW and we process it and return an EDI 997 “acknowledgment”.
- At some point, the vendor sends an EDI 810 “invoice” transaction to WW and we process it and return an EDI 997 “acknowledgment”.


The Transformer tool was configured to parse the different EDI transactions. We created a jar with multiple API endpoints (one for each transaction’s data mapping) and used this jar with our deployed application.


In addition to the added complexity of using bidirectional EDI transactions along with acknowledgment transactions, we needed to generate a unique sequence number for each outgoing transaction as this was required by our vendor. Since our application uses a database to keep track of purchase orders, shipment details, invoice information, and other data — we used the database to generate a unique sequence number for any outgoing transactions.


Due to the sensitive nature of some of the data being sent between WW and our vendor, we are encrypting EDI message files sent to the vendor and the vendor is encrypting EDI message files sent to WW.


I hope I gave you a high-level idea of how we incorporated X12 EDI transaction processing into a few of our apps at WW. For details on X12, please visit x12.org.


**Interested in joining the WW team?** Check out the[careers page](https://www.weightwatchers.com/us/corporate-careers) to view technology job listings as well as open positions for other teams.
