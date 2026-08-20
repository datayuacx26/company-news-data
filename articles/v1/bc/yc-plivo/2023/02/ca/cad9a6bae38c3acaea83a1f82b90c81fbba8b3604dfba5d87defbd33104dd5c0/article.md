---
schema_version: "1.0.0"
document_id: "cad9a6bae38c3acaea83a1f82b90c81fbba8b3604dfba5d87defbd33104dd5c0"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/console-tour-sms/"
published_at: "2023-02-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:4ab6052c2a25f94f46592af4e68c503424cd2d44e5d813e295b978a9d164f490"
---

# A Tour of the Plivo Console: SMS

We’ve never taken our blog readers on a tour of the console. Did we assume that since they could sign up for free they could give themselves a self-guided tour? That’s certainly an option, but we’ll take a moment here and now to be your tour guide. Please keep together.


We’ll start our tour with a look at the SMS features available on the console. We were inspired to call out this section by a comment from Sharita Passariello, who works for Plivo customer Fluent, and who said in a recent customer story, “I love the self-service console and ease of use. It’s critical for us to check the SMS logs and see the messages that were delivered and undelivered, and see why messages were undelivered.”


Passariello shared additional insights about the state of[SMS API](https://www.plivo.com/sms/) marketing in a recent webinar.


## The SMS menu overview


To get to the SMS menu, visit the[Plivo console](https://console.plivo.com/dashboard/) and click on the third icon.


The console displays the Messaging Overview page.


Below some account information, the overview page shows a quick usage summary, a map (holy Mercator, look at the size of Greenland!) with a table showing usage by location, and a graphical and tabular breakout of messaging by source number type.


## SMS and debug logs


That’s all very pretty and good information to pass along to upper management, but the console exposes lots more information. To see it, click on the Logs submenu to bring up the SMS/MMS log: a list of every messaging API URL call made, which you can filter a number of ways or export for analysis elsewhere.


When you click on a message entry on the log page, you can see details about the message.


We won’t make this long post longer by pointing out the information in each section of the page — the point is, you can see all the information you need about a given message, including status, content, and charges.


In addition to the SMS/MMS log, for API calls that include callback URLs, we expose details of our API responses in a debug log; it contains details for all callbacks that Plivo sends. If you click on MessageUUID of an entry in the debug log you can zoom in and see all the details about the message.


## 10DLC


Since Fluent’s Passariello also complimented us on our 10DLC console capabilities in the company’s customer story, let’s look at those next. From the 10DLC menu choice you can add the 10DLC brand you’ve registered for your business, or register one or more new ones. (Resellers, for instance, register multiple brands.) If you click on a brand, you can see all the campaigns registered for it, register new ones, and deactivate existing ones.


If you click on a campaign, you can see all of the phone numbers registered for it, and link and unlink numbers, as well as see the throughput allocation for the campaign.


## XML and PHLO applications


From the SMS menu you can also look at the Plivo applications associated with your account. In Plivo parlance, an application is a set of answer, hangup, and message URLs that you use to control what happens to your incoming calls and messages. This console page shows the mappings.


Here’s an example of an interactive voice response ([IVR](https://www.plivo.com/docs/voice/use-cases/ivr/node/) ) XML application; we can tell that by looking at the information about it in the Application Details section. The entries in the Voice section define URLs the application looks at in the event of an incoming call to that number or when the call is hung up. Upon receipt of an incoming call Plivo makes a synchronous HTTP request to your application using the URL specified in the Primary Answer URL field. The API expects an XML document in response. Plivo also sends a few parameters with the HTTP request that your application can act upon before responding.


Messaging works the same way. Plivo makes a synchronous HTTP request to your application using the URL specified in the Message URL field. Your web application should return an XML document that provides instructions to the Plivo API on how the event should be handled. You can read more about the process in our[documentation](https://www.plivo.com/docs/sms/xml/overview/) .


In addition to XML applications, you can see information about[PHLO](https://www.plivo.com/docs/phlo/) applications. Plivo High-Level Objects is a visual design studio that lets you build workflows with little or no code. When you click on a PHLO in the list, you bring up the design page for that PHLO, so you can change the workflow and settings.


## Sender IDs, MMS media upload, and Powerpack


Many countries — but not the US or Canada — support[sender IDs](https://www.plivo.com/docs/sms/concepts/sender-id-usage/) , which are like caller IDs but for messaging. From this menu page you can see what sender IDs you’ve registered and register new ones.


If you’re planning on sending[MMS messages](https://www.plivo.com/blog/what-is-mms-messaging/) with image, audio, or video attachments, you can specify your own URL or you can use the MMS Media Upload screen to upload the multimedia files to Plivo. then use the media ID associated with the file in the SMS API to include the attachment. Hosting it with us gives you lower latency and eliminates the need for you to manage attachments elsewhere.


Plivo also offers[Powerpack](https://www.plivo.com/sms/powerpack/) , which lets companies manage messaging at scale. With the introduction of[10DLC](https://www.plivo.com/blog/10dlc-10-digit-long-code/) , which offers higher throughput on long code numbers than was formerly available, Plivo no longer recommends using Powerpack for US customers, but it’s a great tool for other countries.


## Settings


The final SMS menu choice lets you tailor certain settings. For example, under Alerts, you can choose whether to notified when


- the delivery rate for outbound messages drops significantly
- delivery failures for inbound messages rise significantly
- SMS volumes display unusual surges


Another screen lets you set[geo permissions](https://www.plivo.com/docs/sms/concepts/geo-permissions/) as a way to help curb SMS fraud. You can select a list of destination countries to which outbound messaging from your account is allowed. We’ll block (and not charge for) any messages sent to countries not in this list. You can also define a list of countries to which outbound SMS traffic is allowed at a subaccount level.


The third and final settings submenu lets you tailor a few miscellaneous settings.


## All about the console


The console is the nerve center for administering the Plivo platform in action. This post touched on some highlights; to dig deeper, visit your own console and see what your data looks like. Also check out our other console tours of the[Voice](https://www.plivo.com/blog/console-tour-voice/) ,[SIP Trunking](https://www.plivo.com/blog/console-tour-zentrunk/) ,[Number Lookup](https://www.plivo.com/blog/number-lookup-intro/) , and[Phone Numbers](https://www.plivo.com/blog/console-tour-numbers/) menus. If you have any questions, you can find the answers in our[documentation](https://www.plivo.com/docs/sms/concepts/overview/) or on our[support portal](https://support.plivo.com/hc/en-us/categories/360001247012-Messaging-API) .
