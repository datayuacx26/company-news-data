---
schema_version: "1.0.0"
document_id: "8cdd4077e069c61e3efd6d6c34eaece1d1616c398e2a9e71a0618ec8ca5faf78"
company_key: "yc-strac"
company: "Strac"
source_id: "yc-strac-news-import-28a26672fe0a"
canonical_url: "https://www.strac.io/blog/how-to-securely-store-sensitive-pii-phi-data--api-keys-in-bubble"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-07-22T15:01:18.002959+00:00"
fetched_at: "2026-07-28T22:25:54.866601+00:00"
content_hash: "sha256:2260fb539da39fd738470022981f59b3a00e036b3e79fe74b7eb27ab8ae40e57"
---

# How to securely store sensitive PII, PHI data & API Keys in Bubble

## Bubble


[Bubble](https://bubble.io/) is the leader in NoCode. It is the best way to build webapps without code. Bubble is the most powerful no-code platform for creating digital products. Innovative companies like[Zendesk](https://zendesk.com/) ,[Lyft](https://lyft.com/) ,[Loreal](https://www.loreal.com/en/) use Bubble to create webapps.


## Is it safe to store sensitive data in Bubble?


1. **No** . According to[Bubble founder (Emmanuel) in this Bubble post](https://forum.bubble.io/t/hippa-compliancy/6718/2) , Bubble is not HIPAA Compliant and hence can't sign BAA Agreement.
2. Also, anyone accessing a Bubble account can view sensitive data in plain-text, introducing liability if that data is leaked or stolen.
3. Bubble logs sensitive data, including API Keys on servers. So, from a security and compliance perspective, that violates security best practice recommended by compliance and privacy laws.


## How does Strac protect Bubble customers?


Strac launched a Bubble plugin and[you can see the launch post on Bubble forum](https://forum.bubble.io/t/strac-securely-store-sensitive-pii-phi-data-api-keys/185444) . Strac is HIPAA Compliant and will sign BAA agreement with customers who want to secure their sensitive PHI (Personal Health Identifiable) data.


### Use Strac Bubble Plugin


Strac has built a Bubble Plugin that makes it easy for Bubble developers to collect and display sensitive data and send data to third-party partners if needed. Strac Bubble Plugin does the following:


#### To Collect Sensitive Data


1. Strac uses widgets (iFrames) on the front-end where Strac's widget will collect data. Due to iFrames, Strac can never access data residing on Customer's page, and vice-versa, the Customer can't access Strac's data residing in the iFrame. This security isolation ensures that Bubble never sees sensitive data during collection.
2. Strac will store sensitive data in its secure vault and generate tokens for the sensitive data.
3. The front-end JavaScript gets tokens and these tokens will be stored on the Bubble instance of the Customer


Strac Widget that tokenizes sensitive data in a form


Check out more here:[https://www.strac.io/collect-pii-and-phi-data-on-apps](https://www.strac.io/collect-pii-and-phi-data-on-apps)


#### To Display Sensitive Data


1. When the front-end wants to display sensitive data, front-end makes the call to Strac, Strac ensures the request is authenticated. If the request is authenticated, Strac will detokenize tokens and give back real values to Front End JavaScript.


In both cases, the Strac widgets are highly customizable.


### Use Bubble API Connector & Call Strac APIs


Bubble customers can leverage rich Strac APIs. Strac Bubble Plugin is built on top of Strac's APIs. Check out more here:[https://www.strac.io/send-personal-data-pii-phi-to-partners](https://www.strac.io/send-personal-data-pii-phi-to-partners)


## Bubble Customer Testimonials


Bubble Customer Testimonial for Strac - 1


Bubble Customer Testimonial for Strac - 2


## ‎Learn More About Strac


Please[book a demo](https://calendly.com/strac-sensitive-personal-data/30min) if you'd like to get access to Strac's Bubble Plugin and API Keys to secure sensitive data on Bubble account. In less than **15 minutes** , you will secure sensitive data on Bubble.‎
