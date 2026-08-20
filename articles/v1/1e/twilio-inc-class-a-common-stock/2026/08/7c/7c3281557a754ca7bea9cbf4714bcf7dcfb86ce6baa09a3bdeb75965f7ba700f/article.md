---
schema_version: "1.0.0"
document_id: "7c3281557a754ca7bea9cbf4714bcf7dcfb86ce6baa09a3bdeb75965f7ba700f"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/send-sms-twilio-ireland-region"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T16:59:47.521529+00:00"
fetched_at: "2026-08-19T16:59:49.632670+00:00"
content_hash: "sha256:4ee77b017e979b01b17e20c6ad08a1fe3c180045360c178066763116889d7447"
---

# Hello, Ireland! Sending SMS in Twilio’s Ireland Region

Time to read:


-
-
-
-
-


August 18, 2026


**Written by**[Gary Spencer](https://www.twilio.com/en-us/blog/authors/author.gspencer) Twilion


**Reviewed by**[Bill Higbee](https://www.twilio.com/en-us/blog/authors/author.whigbee) Twilion


[Paul Kamp](https://www.twilio.com/en-us/blog/authors/author.pkamp) Twilion


---


## Hello, Ireland! Sending SMS in Twilio’s Ireland Region


Have you thought about moving your Programmable SMS workloads to Twilio’s Ireland region **(IE1)** within the EU to meet your data residency desires? You are not alone! Since we[announced in June that Data Residency in the EU is generally available](https://www.twilio.com/en-us/blog/products/launches/data-residency-for-SMS-eu) , customers that I work with have been moving their traffic onto the IE1 region. And did I mention that there are no additional costs?


Using Programmable Messaging in IE1 means that the storage and processing of personal data in the SMS messages that you send to your customers, including end user phone numbers and message bodies, stays in the EU all the way to the supplier edge. If your business has data residency requirements, or simply prefers to keep that data within the EU, then this API is for you.


But what does adopting IE1 mean for your code? Not much - and I’ll show you. Let’s build…


Be aware that today SMS messages to and from numbers with +1 prefixes are not currently supported through the IE1 region. Additionally, RCS and WhatsApp are not currently supported.[Learn more here](https://www.twilio.com/docs/global-infrastructure/messaging-eu-feature-availability#unsupported-messaging-features-in-ie1) , and keep your eyes on the[Twilio Changelog](https://www.twilio.com/en-us/changelog) for future enhancement releases.


## Get your IE1 credentials


Twilio accounts themselves are singletons - you do not need a separate account to use IE1. What you will need is a separate Auth Token, or separate API key and secret to make the calls. You can[create those here in the Twilio Console](https://1console.twilio.com/go?to=/account/__account__/ie1/keys-credentials/api-keys) .


### Create an IE1 Messaging Service


A Messaging Service is a container that groups your senders, handles compliance, enables messaging features, and controls what happens when somebody replies to a message. You do not have to use a Messaging Service, but it is a good idea to do so.


Messaging Services are available in IE1, and you can[create one here](https://1console.twilio.com/go?to=/account/__account__/ie1/messaging/messaging-services) . You cannot use your US1 Messaging Services in IE1.


### Call the API


Now that you have your credentials and your Messaging Service, you are ready to send a message!


Here is how to send an SMS using cURL with a Messaging Service:


Copy code


```text
curl --request POST \
--url https://api.dublin.ie1.twilio.com/2010-04-01/Accounts/AC1234xxxx/Messages.json \
--header 'Authorization: Basic xxxx' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data 'Body=Hello, Ireland!' \
--data 'To=+447832690000' \
--data MessagingServiceSid=MG1234xxxx
--data From=TwilioHarp
```


The only difference in the API call (credentials and Messaging Service aside) is the hostname - instead of[api.twilio.com](https://api.twilio.com/) , you are calling[api.dublin.ie1.twilio.com](https://api.dublin.ie1.twilio.com/) . The response that you get and the way that you construct the call is still *exactly the same.*


Using one of[our Client SDKs](https://www.twilio.com/docs/libraries) ? The change is even more straightforward. You just need to switch the region from us1


to ie1


when you set up the Client. Let us take Python as an example, where today you might be using us1


:


Copy code


```text
#SMS from US1
client = Client(
ie1_api_key_sid,
ie1_api_key_secret,
account_sid=account_sid,
region="us1",
edge="dublin"
)
message = client.messages.create(
body="Ahoy from US1!",
from_=TwilioHarp,
to=+447832690000
)
```


Here, I am using Twilio’s processing and storage region in the US, and am entering the Twilio backbone using the Dublin edge. You can use any edge to get into any of Twilio’s regions.


To have that same message be processed and stored in Ireland, all I have to do is switch out that region


identifier.


Copy code


```text
#SMS from IE1
client = Client(
ie1_api_key_sid,
ie1_api_key_secret,
account_sid=account_sid,
region="ie1",
edge="dublin"
)
message = client.messages.create(
body="Ahoy from IE1!",
from_=TwilioHarp,
to=+447832690000
)
```


Switching the region is only a matter of changing the region


parameter.


### It worked! But how do I know this got processed in IE1?


I get it, you need more confirmation – since you may know us as[a company who rickrolls people](https://www.youtube.com/watch?v=NYi_MI9REbw) .


Twilio has a filter in your logs so you can be certain of the region they were sent from and processed.[Here is a direct link to view your IE1 SMS logs](https://1console.twilio.com/go?to=/account/__account__/ie1/messaging-logs/all) . Or, from the sidebar, click on **Messaging** -> **Logs** -> click on the **region** , and *voilà* !


Congratulations, you are sending your messages in Twilio’s IE1 region!


## What is next?


Our engineering team is hard at work bringing new capabilities to the platform across all of our regions. One initiative you might find exciting is our effort to enable[Studio and TaskRouter in IE1](https://www.twilio.com/en-us/changelog/studio-and-taskrouter-private-beta-ie1) .


[Studio](https://www.twilio.com/docs/studio) is Twilio’s low/no code visual workflow builder that lets you design, build, and manage communication apps like chatbots, surveys, and automated alerts.


[TaskRouter](https://www.twilio.com/docs/taskrouter) is a skills/attributes based routing system that matches incoming tasks (like messages) to the best worker to take on the task.


**Want more information?**


- Read our[launch blog post for SMS Data Residency in the EU](https://www.twilio.com/en-us/blog/products/launches/data-residency-for-SMS-eu) .
- Explore our[Developer Documentation](https://www.twilio.com/docs/global-infrastructure/messaging-api-with-twilio-regions#getting-started) to enable data residency for your SMS in the EU, at no additional cost.
- [Contact Sales](https://www.twilio.com/en-us/help/sales-v1) to learn how consolidating on Twilio delivers compliant customer engagement, zero architectural overhauls, and isolated EU data with centralized audit trails.


Thanks for reading, we can’t wait to see what you build in IE1!


---


*Gary Spencer is a Presales Architect working at Twilio, based in London. Gary has over ten years of experience in customer-facing technology roles, helping businesses design and build solutions using APIs. Feel free to reach out to him at[linkedin.com/in/gspncr](http://linkedin.com/in/gspncr)*
