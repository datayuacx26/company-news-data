---
schema_version: "1.0.0"
document_id: "4c94290f9d706dc19e716907b9d5aa84b81ebc96069bf1acc969dab0911fa1dc"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/insights/aggregate-performance-reporting-feedback"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T22:40:50.920610+00:00"
fetched_at: "2026-08-03T22:52:40.984273+00:00"
content_hash: "sha256:6dc5624d3d311a2c5bde0d5f840c36a93084853aea5ed038c1a07fb275675636"
---

# What is Aggregate Performance Reporting Format (APRF)?

## What is Aggregate Performance Reporting Format (APRF)?


Email marketers have had to navigate significant and sometimes challenging shifts in the email landscape in the past few years.[Apple Mail Privacy Protection](https://www.twilio.com/en-us/blog/insights/apple-mail-privacy-protection) changed how we understand and leverage email open data, while[Apple Link Tracking Protection](https://dotdigital.com/blog/apple-ios-17-link-tracking-protection-guide/) made it harder to track customer journeys and engagement history. In Europe, new regulatory guidance from[France’s CNIL](https://www.twilio.com/en-us/blog/insights/tracking-consent-cnil-france) and Italy’s Garante, respectively, make it[illegal to use tracking pixels without explicit recipient consent](https://emparrot.com/blog/2026/07/09/cnil-garante-pixel-consent) . Meanwhile, AI-powered[pre-open summaries](https://www.twilio.com/en-us/blog/insights/mailbox-provider-ai-apple-intelligence) and[automatic extractions](https://www.twilio.com/en-us/blog/insights/mailbox-provider-AI-automatic-extraction-and-email-previews) make email conversion attribution far less reliable.


Mailbox providers and governing bodies alike are tightening up how consumer data is shared and stored, a trend that will only continue as AI abuse grows. These changes have improved the mailbox user experience and strengthened consumer privacy, bringing huge benefits to the email ecosystem and to consumers. Unfortunately for email marketers, these changes have also shrunk the pool of email activity data historically used to better understand the health of their email programs, how mailbox providers are treating their content, and whether critical messaging is reaching the eyes of their target audiences.


But for the first time in a long time, there is a potential addition to the email industry that will be pure upside for senders: Aggregate Performance Reporting Format (APRF). APRF is a newly proposed email feedback mechanism that would give senders a direct look into what is actually happening to their emails once major mailbox providers accept them. After years of shrinking access to granular, recipient-level activity data, mailbox providers may just throw us a bone in the form of direct high-level reporting.


## What is Aggregate Performance Reporting Format (APRF)?


At the moment, APRF is a proposed feedback loop designed to bridge the long-standing gap between mailbox providers and email senders. It’s important to note that it’s currently in draft form and is not yet formally endorsed by the Internet Engineering Task Force (IETF). Spearheaded by industry veterans Alex Brotman, Emil Gustafsson, and Tom Corbett, APRF is meant to take away some of the ambiguity around email reputation systems that impact email spam filtering.


Using reports based on real user interaction with your emails, the protocol would allow you to see how your sending behaviors directly impact inbox placement. With direct insight into inbox performance, senders will be much better equipped to identify problems and make course corrections that benefit their email programs and ultimately the end-user experience.


APRF would give senders a clear answer to the question we’ve only been able to make educated guesses about in the past: **Are my emails reaching the inbox, or landing in the spam folder?**


Once you’ve added a straightforward text record to your DNS, mailbox providers that support APRF will give you two incredibly useful pieces of information:


1.


**Classification:** Number of emails that went to the spam folder (“unwanted”) vs. the number of emails that went to the inbox.


2.


**Engagement:** Counts of positive actions like pulling your email out of the spam folder and counts of negative actions like reporting the email as spam.


## How does APRF work?


By now, we are all familiar with[how DMARC reporting works](https://www.twilio.com/en-us/blog/insights/dmarc-monitoring) : you create a TXT record in your domain’s DNS provider and specify in it where mailbox providers should send the reports via the rua= destination address.


APRF uses a very similar setup, but focuses specifically on your DKIM-signing domain to route reports. A mailbox provider generating APRF reports looks up the selector and domain found in your DKIM signature (at _aprf._domainkey.yourdomain.com) and sends reports to whatever address you’ve specified in that domain's rua= field.


The[APRF draft](https://datatracker.ietf.org/doc/draft-brotman-aggregate-performance-reporting/) provides all the technical specs, including a sample basic APRF TXT record:


**v=APRFv1;rua=mailto:reports@example.org**


Even better, there is an optional attribute called SDI (Signer-Defined Identifiers) that would operate much like the[Gmail feedback-id header](https://www.twilio.com/en-us/blog/insights/leveraging-gmail-feedback-loop-identifiers) does, allowing senders to segment the performance reports by specific unique identifiers defined by the sender. Similar to the Gmail feedback-id header, you can include up to 4 identifiers that increase in granularity to help you zoom in on specific problems.


In order to take advantage of SDI, you need to create a custom header for your emails, then specify in your DNS record which email header to look at for SDI and which character you’ll use to separate data tags.


For example, you could set up your custom header this way:


**Signer-Info: SenderName^SendGridSubuserName^CampaignID**


While your APRF TXT record might look like this:


**v=APRFv1;rua=mailto:reports@example.org;sdi=Signer-Info,^**


Because you specified sdi=Signer-Info,^ in your DNS record, you are telling the recipient mailbox provider: *"Look at my Signer-Info header, and segment the data wherever you see a ^ symbol."*


## The current status of APRF


We’ll say it one more time to be extra clear: APRF is currently just a draft, so things could change before IETF endorsement becomes reality. Comcast is currently running a beta (check out Al Iverson’s[sample of the beta feedback here](https://www.spamresource.com/2026/07/aprf-new-standard-for-deliverability) ), so while you could set this up today, feedback you receive back would be limited to performance at Comcast domains. Right now we’re waiting to see if the IETF will accept and publish APRF as an RFC.


In the meantime, we are crossing our fingers and dreaming of all the useful dashboards we could build off of this reporting. The data sharing proposed by APRF would go a long way to help well-intentioned senders become better at what they do. Ultimately, this would create a healthier email ecosystem for everyone. When senders can pinpoint and fix bad sending practices, recipients end up with far less spam in their inboxes.


*Don't navigate changing email metrics alone. Our email deliverability experts can help you adapt, optimize, and keep your email program thriving.[Get in touch with our Professional Services team today](https://www.twilio.com/en-us/professional-services) !*
