---
schema_version: "1.0.0"
document_id: "268af2c1a283ad8701be1656932e5226596b26a5b3ac98a3327a5bf148563038"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/advanced-message-storage-options/"
published_at: "2024-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:a2f478082802f06ceb39a77753878626c2058a03114a8e49bf1fad2c94742975"
---

# Enhancing Data Privacy: Advanced Message Data Storage Options

Data storage in the context of SMS/MMS/WhatsApp involves recording and storing messaging data, including recipient information and message content. Companies often use SMS/MMS/WhatsApp data storage to track communication with clients or for internal monitoring purposes.


However, data storage is subject to privacy regulations such as GDPR or HIPAA. So organizations processing third-party personal data are required to implement privacy controls, such as message content storage redaction.


## **Plivo's Enhanced Data Storage Options**


To address this we're thrilled to introduce new comprehensive data storage options for SMS, MMS and WhatsApp messaging. This empowers Plivo customers to fine-tune their data storage preferences, aligning with regulatory requirements and their unique business needs.


## **Customizable Data Storage Preferences for Messaging**


### **Outbound Messages**


Plivo now offers four custom data storage preferences, enabling customers to personalize the handling of their outbound messaging data in alignment with global data protection standards and business needs.


The preference for data logging of outbound messages is set as a parameter in the[Send Message endpoint](https://www.plivo.com/docs/messaging/api/message#send-a-message) and is applied at the **message level** . The four available settings for modifying content storage preferences include:


Setting option Description Log Parameter Status Destination Number Stored Message Content Stored


Store Both Number and Message Content (Default) Stores the destination number for 90 days and message content for 7 days. Set to ‘true’ Yes, for 90 days Yes, for 7 days


Redact Both Number and Message Content Partially redacts destination numbers. Does not store the message content. Set to ‘false’ Partially redacted Not stored


Store Only Number Stores the destination numbers 90 days. Does not store the message content. Set to ‘number_only’ Yes, for 90 days Not stored


Store Only Content Stores the message content for 7 days. Partially redacts destination numbers. Set to ‘content_only’ Partially redacted Yes, for 7 days


‍


### **Illustrative Example of Data Storage Options**


Consider a message with the destination number '+12025550123' and content: 'Hello, your appointment is confirmed for tomorrow at 3 PM.' processed under each storage setting:


Setting option Processed Destination Number Processed Message Content


Store Both Number and Message Content +12025550123 (Stored for 90 days) Hello, your appointment is confirmed for tomorrow at 3 PM. (Stored for 7 days)


Redact Both Number and Message Content +12025550*** ***Text Content Redacted***


Store Only Number +12025550123 (Stored for 90 days) ***Text Content Redacted***


Store Only Content +12025550*** Hello, your appointment is confirmed for tomorrow at 3 PM. (Stored for 7 days)


**Important Note:** Message content redaction prevents Plivo from troubleshooting or retrieving messages in case of issues. The default setting is to store both the number and message content, ensuring outbound messages are fully retained unless the log parameter is explicitly set to ‘false’.


### **Inbound Messages**


Similarly, for inbound messages, customers can specify their storage preferences, which are configured at the **application level** . The available settings for inbound messages are:


Setting option Description Log Parameter Status From Number Stored Message Content Stored


Store Both Number and Message Content (Default) Stores the from number for 90 days and message content for 7 days. Set to ‘true’ Yes, for 90 days Yes, for 7 days


Redact Both Number and Message Content Partially redacts the from number. Does not store the message content. Set to ‘false’ Partially redacted Not stored


**Important Note:** If inbound messages are redacted, Plivo cannot debug or recover message content if there are any issues with the callback URL.


## **Default Settings for Outbound and Inbound Messages**


Standard policy ensures the accessibility of outbound message content for 7 days and destination numbers for 90 days. After this period, both message content and numbers are stored in compliance with GDPR. This policy similarly applies to inbound messages, facilitating content and number access under the same terms.


These features depend on the selected storage settings and are available unless message data has been redacted. Users can explore this data via the[Messaging Logs](https://console.plivo.com/sms/logs/messages/) section of the console - with options to review a specific timeframe or search a UUID to retrieve message details.


Discover more about our message log redaction processes in our[developer documentation](https://www.plivo.com/docs/messaging/concepts/sms-data-redaction) .
