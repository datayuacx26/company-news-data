---
schema_version: "1.0.0"
document_id: "691db2f0ad95c3ec83d8c886651812894c7024af8a61a60bf12a41e275365362"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/console-tour-voice/"
published_at: "2023-05-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:d3f5915aa8ef43ff6b898a2c9b68946792bc2838355d3e9fc915e2ffb6b3d05a"
---

# A Tour of the Plivo Console: Voice

We began our tour last time with a look at[what you can do on the Plivo console to monitor and manage our SMS API](http://plivo-webflow.webflow.io/blog/console-tour-sms) . The series continues here with a similar survey for our[Voice API](https://plivo-webflow.webflow.io/voice) .


## The Voice menu overview


To get to the Voice menu, visit the[Plivo console](https://console.plivo.com/dashboard/) and click on the second icon.


The console displays the Voice Overview page, which shows some basic usage information.


## Advanced Insights


Moving to the next menu item takes you to the[Advanced Insights](https://plivo-webflow.webflow.io/voice/call-insights) page. It shows information that can help you identify call quality issues so you can fix them quickly.


## XML and PHLO applications


The next menu choice brings you to a list of the Plivo XML applications and[PHLOs](https://www.plivo.com/docs/phlo/) associated with your account. In Plivo parlance, an application is a set of answer, hangup, and message URLs that you use to control what happens to your incoming calls and messages, while our PHLO visual workflow design tool lets you build applications graphically. Clicking on an application name brings you to a screen of details about the application.


The entries in the Voice section of this page define URLs the application looks at in the event of an incoming call to that number or when the call is hung up. Upon receipt of an incoming call Plivo makes a synchronous HTTP request to your application using the URL specified in the Primary Answer URL field. The API expects an XML document in response. Plivo also sends a few parameters with the HTTP request that your application can act upon before responding.


If there were a URL specified for messaging it would work the same way. You can read more about the process in our[documentation](https://www.plivo.com/docs/sms/xml/overview/) .


## Endpoints


The next menu choice is Endpoints, which displays a list of[SIP endpoints](https://www.plivo.com/docs/voice/concepts/sip-endpoint/) associated with your account. A SIP endpoint can be any IP phone, mobile phone, wireless device, or PC that uses Session Initiation Protocol (SIP) to perform communications operations.


## Logs


Click on the Logs submenu to bring up the Voice log: a list of every voice API URL call made, which you can filter a number of ways or export for analysis elsewhere.


When you click on a call entry on the log page, you can see details about the call.


That’s a ton of information about the call itself — and if that doesn’t tell you enough, well, there’s more. A note in the last box on the page directs you to the call debug log entries. The debug log contains details for all callbacks that Plivo sends.


With all the information in the logs you should be able to understand any issues you encounter. (But if you need help, our support team is always here for you.)


In addition to call logs, we give you access to recording and transcription logs.


If you click on an item in the log, you can see details about that item.


## Settings


The final group of menu items falls under the category of Settings. The first setting entry determines whether to use[Call Insights](https://plivo-webflow.webflow.io/voice/call-insights) . Because the feature is still in beta and therefore free to everyone, it’s enabled by default.


The Push Credentials screen lets you store the credentials you need if you’re working with Plivo’s[iOS SDK](https://www.plivo.com/docs/sdk/client/ios/overview/) or[Android SDK](https://www.plivo.com/docs/sdk/client/android/overview/) . Once you have credentials issued by Apple and Google you can enter them here.


Geo Permissions can help you protect your account from fraud by restricting the set of countries your account can call. Get more information from our[documentation](https://www.plivo.com/docs/voice/concepts/geo-permissions/) .


Finally, Other Settings lets you enable[destination number validation](https://www.plivo.com/docs/sms/concepts/destination-number-validation/) (which can give you better delivery rates and cost savings from not sending messages to numbers that are incapable of accepting them), enable HTTP Basic Auth on all GET recording media requests for enhanced security, and providing consent for Plivo to store and have access to logs and media packet capture files.


## All about the console


That’s our quick tour of the Voice menus. To dig deeper, visit[your console page](https://console.plivo.com/dashboard/) and see what your data looks like. Also check out our other console tours of the[SMS](http://plivo-webflow.webflow.io/blog/console-tour-sms) ,[SIP Trunking](http://plivo-webflow.webflow.io/blog/console-tour-zentrunk) ,[Number Lookup](http://plivo-webflow.webflow.io/blog/number-lookup-intro) , and[Phone Numbers](http://plivo-webflow.webflow.io/blog/console-tour-numbers) menus. If you have any questions, you can find the answers in our documentation or on our[support portal](https://support.plivo.com/hc/en-us/categories/360001245191-Voice-API) .
