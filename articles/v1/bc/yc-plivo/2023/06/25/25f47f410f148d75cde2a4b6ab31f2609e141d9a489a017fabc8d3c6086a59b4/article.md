---
schema_version: "1.0.0"
document_id: "25f47f410f148d75cde2a4b6ab31f2609e141d9a489a017fabc8d3c6086a59b4"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/implicit-sms-content-template-types-in-india/"
published_at: "2023-06-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:95c85c7e0b23e75ce6b6b6336445ac7ba2b6b073fc311e2a9b9796fefe740dca"
---

# The Four Content Types for Texting in India

To minimize spam texting in India, the Telecom Regulatory Authority of India (TRAI) mandates that all entities that want to send SMS messages must register information such as their entity/organization and message templates on distributed ledger technology (DLT) platforms. Read our blog post for complete details.


Part of the registration process requires organizations to register message content templates. A content template includes the actual text of an SMS message to be sent, with space designated for variables where appropriate, such as an account number or balance.


[SMS message](https://plivo-webflow.webflow.io/sms) content falls into four categories, and therefore so do content templates: transactional, service implicit, service explicit, and promotional. This table shows each type of content and shows how they differ from each other.


**Transactional** **Service implicit** **Service explicit** **Promotional**


Usage Used only by banks to send one-time passwords (OTP) and alerts. Used to send OTPs, informational messages, and notifications - but not marketing messages - to registered recipients. Used to send offers or updates regarding services to existing customers. Used to send offers, promotions, and marketing content. The messages may or may not be solicited by the recipients.


Examples A message that contains an OTP required to complete a banking or credit or debit card transaction initiated by a bank customer


- Confirmation messages of a credit/debit transaction
- Product purchase confirmation
- Parcel delivery status
- OTP required for ecommerce or app login
- Messages from schools regarding attendance/transport
- Messages from medical providers regarding appointments
- Government advisories


Any service message that doesn’t fall under the category of service implicit, including messages to existing customers recommending or promoting products or services Any message designed to promote or sell products or services


Consent preferences Not required Not required Required Required


Can be sent to number on DND list Yes Yes No No


Sending time restrictions None None None Can be sent only between 10 a.m. and 9 p.m.


Sender ID (header) - must be registered with TRAI Six characters, alphabetic, capitalized. Must be reflective of the brand name and/or trademark and must tie in with the content in the message. Six characters, alphabetic Six characters, alphabetic Six characters, numeric*


*The first character of a promotional header must start with the numeral that represents the business category that matches the category preference in National Customer Preference Register (NCPR) (DND Registry). 1 - Banking/insurance/financial products/credit cards 2 - Real estate 3 - Education 4 - Health 5 - Consumer goods and automobiles 6 - Communication/broadcasting/entertainment/IT 7 - Tourism and leisure 8 - Food and beverages


Finally, certain types of messages are always prohibited: political messaging during elections and gambling- and cryptocurrency-related content.


We’ve included all the information you need to know about registering on DLT platforms into an[article in our support portal](https://support.plivo.com/hc/en-us/articles/360046769131-DLT-Registration-Process-for-Sending-SMS-to-India) . It should guide you through registration for whichever content types you plan to use.


‍
