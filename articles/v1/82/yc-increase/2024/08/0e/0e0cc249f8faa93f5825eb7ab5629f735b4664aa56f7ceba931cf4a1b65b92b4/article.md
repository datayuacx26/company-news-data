---
schema_version: "1.0.0"
document_id: "0e0cc249f8faa93f5825eb7ab5629f735b4664aa56f7ceba931cf4a1b65b92b4"
company_key: "yc-increase"
company: "Increase"
source_id: "yc-increase-news-import-16282b6988d7"
canonical_url: "https://increase.com/articles/visa-redundancy"
published_at: "2024-08-23T00:00:00+00:00"
first_seen_at: "2026-07-24T11:51:21.619731+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:75bbda3ed56ade4a0950404dfdc80b95c01a09dd0ccba7757803528e83eafab8"
---

# Visa: half a century of high availability — Increase

From the Fresno drop in 1958 and through Visa’s first 15 years of existence, an authorization with a Visa card was processed by having the merchant call either the cardholder’s bank, if the card was regional, or a switching center to be forwarded onto the bank if it was not. In the early 70s, the overhead of this reached a breaking point and Visa set out to build what would become known as “BankAmericard Authorization System Experimental (BASE)“—an electronic authorization system linking the Visa member banks with merchants or their acquiring centers through a central switching system. While the protocols and implementation details look drastically different 50 years later, many of the core principles from BASE still stand today: a centralized switching system connecting acquirers to issuers through leased line connectivity to both sides, with redundancy built in at each step of the process0 .


Increase’s authorization connectivity with Visa is built on the same principle of redundancy: each of our data centers connect to each of Visa’s two data centers through at least two separate circuits from at least two routers. This in turn connects to our cloud provider through at least two regions. Every layer maintains redundancy of each other: both of our physical data centers can talk to both of Visa’s data centers and both of our cloud regions can reach both of our physical locations.


This summarizes the high-level architecture of Increase’s redundant connectivity with Visa. The rest of the article digs deeper into the details of how the specific messages transmitted over the Visa network work.


## Visa authorization messages


Visa’s BASE I system predates HTTP and the internet by almost two decades, but the way the contents of an authorization message transmit from an acquirer to an issuer is intuitive even from today’s point of view. Unlike HTTP 1.1, where a corresponding response always has to follow serially on a TCP socket after the request, Visa’s authorization system is configurable to allow responses to be sent through almost any live connection the recipient has, resembling HTTP 2’s multiplexing with a unique identifier in each message. For Visa, the messages follow the ISO 8583 standard1 but are preceded by a 4-byte message length header that indicates the size of the message that is to follow. After the 4-byte message length header follows a routing header that includes information about where the message is to be routed, which then is followed by the message itself.


The ISO 8583 standard defines the overall structure of the message but leaves the underlying content to be defined by each card network. From a quick glance they share similar principles: a 4-digit Message Type Indicator that states the purpose of the message and its direction (e.g.,` 0100` is an` Authorization Request` and` 0110` is an` Authorization Response` ), a bitmap that indicates which fields are present, followed by each present field serialized in order. Unlike a message protocol like Google’s Protocol Buffers, deserializing an ISO 8583 message correctly is impossible without first having implemented a parser to a specification. The parser tells the recipient how to handle each separate field—some fields might be simple fixed-length numeric fields, while others will be complicated deeply nested variable length fields.


## Initiating sessions


Both sides of the table, the acquiring processors and the issuer processors, sign on to the Visa network by opening a TCP connection and sending a sign-on message that identifies the specific authorization “station” that they represent. As an issuer processor, we sign on to our stations from our cloud-hosted Message Processor services and wait for Visa to relay us messages from acquiring processors relevant to our cards. When we receive a message, we parse it, redact out sensitive details from a Payment Card Industry Data Security Standards (PCI-DSS) perspective (such as card numbers), and pass the message on to the rest of our authorization processing systems for further checks.


When we’re ready to respond, we send a similar looking message back with a specific numeric identifier (field 37, “Retrieval Reference Number”) set to the same value as we received in the request. This then gets passed back to the acquiring processor, which in turn will match the incoming response to the outgoing pending request they have and provide an answer to the merchant and the cardholder who is waiting at the point of sale.


## From cloud to physical


From our cloud providers, we connect to the routers hosted in our physical data centers through[Cloud Interconnect](https://cloud.google.com/network-connectivity/docs/interconnect/concepts/overview) . Each cloud region maps to a physical data center in regional proximity. However, all of our cloud providers can reach all of our physical data centers and vice versa. From here, our routers connect to a set of Visa-owned routers in the same data center, which in turn handles communication with another set of routers in Visa’s data center.


Avoiding the fragility of the public internet, the communication between the Visa routers in our data centers and the Visa Message Gateways in Visa’s own data centers happen using a combination of leased circuits from redundant telecom providers and Visa’s Multiprotocol Label Switching (MPLS) network. In some ways this has more of a resemblance to the point-to-point behavior observed in analog phone lines than to the packet routed system of the public internet. Instead of labeling a packet with the destination and letting the individual hops decide how the packet gets there, the route through the network is defined ahead of time and the packets state which path they expect to take.


## Visa’s data centers


Outside of European traffic, all Visa authorizations eventually end up in either of Visa’s two US data centers: central and east. These are both active at any time and transactions fall back to either seamlessly. Similar to how we redundantly connect to both of our data centers, each of the two locations also connect to each other and relay messages back and forth when necessary. When Visa receives a message from an acquirer processor, its main responsibility is figuring out where to send it by looking at the first digits of the card number and figuring out which issuer processor is set up to handle it.


As Visa’s system has evolved from manual authorizations to modern redundant systems, it has maintained its foundational commitment to reliability and high availability. Increase’s architecture is built on these same principles, ensuring that transactions are processed securely, reliably, and promptly. If you’re looking to partner with an issuer processor that provides direct access to the Visa network, you cancontact our sales team or learn more through our[API](https://increase.com/documentation/api/cards) .


## Notes


1.


Stearns, D: A History of the VISA Payment System, 1970 – 1984.↩


2.


[https://en.wikipedia.org/wiki/ISO_8583](https://en.wikipedia.org/wiki/ISO_8583)↩
