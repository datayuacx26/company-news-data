---
schema_version: "1.0.0"
document_id: "c9ae55f56479aa4036384b0ba059f2c83a8db2dbe87e764125fc39734dde36f0"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/introducing-new-zentrunk-apis/"
published_at: "2023-06-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:1a71eabf798d77689df966900e3acc24486666f067abd601e346d2096ab3731d"
---

# Announcing Two New SIP Trunking APIs

We’re constantly and continually improving Plivo services, with particular attention paid to enhancements suggested by our users. Today we’re happy to announce two new APIs for[SIP Trunking](https://plivo-webflow.webflow.io/sip-trunking) , our SIP trunking service: the Trunk Properties API, which lets businesses create, modify, and update their SIP trunk configurations programmatically, and the Call Object API, which lets users retrieve SIP Trunking call details records (CDR).


## Trunk Properties API


SIP Trunking users can already use the Plivo console to manage SIP trunk configuration and subresources such as credentials lists, IP access control lists, and origination URIs. But working in the console can be time-consuming when you have a lot of resources to manage. With the new API you can fetch and manage trunk properties programmatically, saving you time and effort. Our[API documentation](https://www.plivo.com/docs/sip-trunking/api/trunk/) provides all the information you need to get started.


The Trunk Properties API offers SIP Trunking users several benefits:


- **Improved developer experience:** The API lets service providers and enterprises create, modify, and delete SIP trunks programmatically.
- **Access to subresources:** The Trunk Properties API lets developers access subresources such as credentials, IP access control lists, and origination URIs, making it a complete trunk configuration management toolkit.
- **Ease of use:** The API uses standard RESTful web services, making it simple to integrate with existing systems.
- **Enhanced security:** The API supports authentication via Basic Auth, which enhances security.


### Streamlined SIP Trunking management


The Trunk Properties API transforms the process of setting up and managing SIP trunks. It offers an efficient and automated way to create, modify, and delete trunks. Its ease of use and real-time visibility features make it an excellent tool for service providers and enterprises. With the Trunk Properties API, users can streamline their SIP trunking service management and focus on delivering a better customer experience.


## Call Object API


Our second new API addresses call details.


A call detail record, as you’d expect, contains all the details about a call. Until now you could view and export CDRs only by visiting SIP Trunking >[Logs](https://console.plivo.com/zentrunk/logs/calls/) on the Plivo console. The new Call Object API lets you retrieve CDRs for any completed calls using a variety of filters, saving you time and effort.


SIP Trunking creates a Call object when an outbound call is initiated or when an inbound call is received. The Call object contains all of the information about completed calls. See our[API documentation](https://www.plivo.com/docs/sip-trunking/api/call/) for complete details and code examples.


In brief, the Call Object API lets you retrieve all call records or retrieve calls for a given call UUID. The API supports filters that let you retrieve specific records based on different needs.


By using the Call Object API, SIP Trunking customers can gain several benefits:


- **Real-time data** : The API provides real-time access to call data, allowing businesses to access and analyze information as it becomes available, without having to wait for manual exports or generate reports through the console.
- **Improved automation** : Businesses can automate the process of fetching CDRs and integrating their data into external systems for analytics, eliminating the need for manual data extraction.
- **Improved call analytics** : Businesses can use CDR information to gain insights into call patterns, call durations, and call costs, helping them make data-driven decisions to optimize their communication strategies.
- **Efficient data retrieval** : By using filters, businesses can perform targeted analyses of specific segments of their call data.
- **Increased security and fraud detection** : CDR information can help organizations detect unusual call patterns and identify potential security threats or fraudulent activities.
- **Customized reporting** : With data from the Call Object API, businesses can create reports tailored to their specific needs, allowing for more informed decision-making.


## Try our new APIs


If you’re already a SIP Trunking customer, we encourage you to peruse the documentation for the new APIs and consider how they could help your business. If you’re thinking about exploring the[benefits of SIP trunking](http://plivo-webflow.webflow.io/blog/sip-trunking-benefits) , we encourage you to[sign up](https://console.plivo.com/accounts/register/) for a Plivo account, read our[Quickstart Guide](https://www.plivo.com/docs/sip-trunking/) , and find out for yourself why people are so excited about SIP trunking.
