---
schema_version: "1.0.0"
document_id: "518f019202b85fc95946dfe166059454cb5521a0a8bca6a597e4d5cff95f52c3"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-easily-migrate-sms-mms-application-from-bandwidth-to-plivo/"
published_at: "2022-07-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:bbb9ccf087e34bb428440f6527f517cc4fde834363b0cfd43c29dafb5b90a54c"
---

# How to Migrate Your SMS/MMS Applications from Bandwidth to Plivo

Migrating from Bandwidth to Plivo is a seamless and painless process. The two companies’ API structures, implementation mechanisms, SMS message processing, and MMS message processing are similar. We wrote this technical comparison between the Bandwidth and Plivo APIs so that you can scope the changes for a seamless migration.


## Understanding the differences between Bandwidth and Plivo development


Most of the APIs and features that are available on Bandwidth are also available on Plivo, and the steps involved are almost identical. This table gives a side-by-side comparison of the two companies’ features and APIs. An added advantage with Plivo is that not only can you code using the old familiar API method, you can also implement your use cases using PHLO (Plivo High Level Objects), a visual workflow builder that lets you create workflows by dragging and dropping components onto a canvas - no coding required.


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; padding: 15px 18px 15px 18px; } pre.lineno{ color: #fff; opacity: .3; } .w-richtext figure { max-width: 100%; position: relative; }


**Features and APIs** **Bandwidth** **Plivo** **Similarities** **Implementation Interface**


[SMS API](https://www.plivo.com/docs/sms/) : Send SMS messages ✅ ✅ Request and response variables’ structure API
PHLO


[MMS API](https://www.plivo.com/docs/sms/) : Send MMS messages ✅ ✅ Request and response variables’ structure API
PHLO


[10DLC](https://console.plivo.com/sms/10dlc/brand/) : 10-digit long code (10DLC) phone numbers ✅ ✅ Registration process and usage[Console](https://console.plivo.com/sms/10dlc/brand/)


[Managed number pool](https://www.plivo.com/docs/sms/powerpack/) for US/CA Messaging ✅ Powerpack Feature parity API
Console


[Phone number management](https://www.plivo.com/docs/numbers/) ✅ ✅ Feature parity API
Console


[HTTP callbacks](https://www.plivo.com/docs/sms/concepts/callbacks/) ✅ ✅ Feature parity API
XML
PHLO


## Plivo account creation


Start by[signing up for a free trial account](https://console.plivo.com/accounts/register/) that you can use to experiment with and learn about our services. The free trial account comes with free credits, and you can[add more](https://console.plivo.com/payments/) as you go along. You can also[add a phone number](https://console.plivo.com/phone-numbers/search/) to your account to start testing the full range of our voice and SMS features. A page in our support portal[walks you through the signup process](https://support.plivo.com/hc/en-us/articles/360041203772) .


You can also port your numbers from Bandwidth to Plivo, as we explain in[this guide](https://www.plivo.com/docs/numbers/number-porting/) .


## Migrating your SMS application


You can migrate your existing application from Bandwidth to Plivo by refactoring the code, or you can try our intuitive visual workflow builder[PHLO](https://console.plivo.com/phlo/list/) . To continue working with the APIs, use one of the quickstart guides to set up a development environment for your preferred language. Plivo offers server SDKs in seven languages:[PHP](https://www.plivo.com/docs/sms/quickstart/php-laravel/) ,[Node.js](https://www.plivo.com/docs/sms/quickstart/node-expressjs/) ,[.NET](https://www.plivo.com/docs/sms/quickstart/dotnet-framework/) ,[Java](https://www.plivo.com/docs/sms/quickstart/java-spring/) ,[Python](https://www.plivo.com/docs/sms/quickstart/python-flask/) ,[Ruby](https://www.plivo.com/docs/sms/quickstart/ruby-rails/) , and[Go](https://www.plivo.com/docs/sms/quickstart/go-gin/) . For another alternative that lets you evaluate Plivo’s[SMS APIs](https://www.plivo.com/sms/) and their request and response structure, use our[Postman collections](https://www.plivo.com/docs/sms/quickstart/postman/) .


### How to send an SMS message


Let’s take a look at the process of refactoring the code to migrate your app from Bandwidth to Plivo to set up a simple cURL application to send an SMS message by changing just a few lines of code.


**Bandwidth** **Plivo**


```text
from    bandwidth.bandwidth_client    import    BandwidthClient
bandwidth_client    =    BandwidthClient  (
messaging_basic_auth_user_name  =  <span  =""  class=“s”>“<username>” ,
messaging_basic_auth_password  =  <span  =""  class=“s”>“<password>” )
messaging_client    =
bandwidth_client  .  messaging_client  .  client
from
bandwidth.messaging.models.message_request    import    MessageRequest
messagingBody    =    MessageRequest  ()
messagingBody  .  to    =
[  “<destination_number>”  ]
messagingBody  .  mfrom    =
“<sender_id>”
messagingBody  .  text    =
“Hello, from Python!”
messagingBody  .  application_id    =    “<app_id>”
messaging_client  .  create_message  (  “<account_id>”  ,    body  =  messagingBody  )


```


```text
import    plivo
client    =    plivo  .  RestClient
(  ‘<auth_id>’  ,  ‘<auth_token>’  )
response    =    client  .  messages  .  create  (
src  =  ‘<sender_id>’  ,
dst  =  ‘<destination_number>’  ,
text  =  ‘Hello, from Python!’  ,)
print  (  response  )
```


Alternatively, you can implement the same functionality using one of our[PHLO templates](https://console.plivo.com/phlo/list/) . For example, if you want to send an SMS message, your PHLO would look like this.


## Migrating your MMS application


### How to send an MMS message


Let’s take a look at the process of refactoring the code to migrate another application from Bandwidth to Plivo - a simple cURL application to send an MMS message - by changing just a few lines of code.


**Bandwidth** **Plivo**


```text
from    bandwidth.bandwidth_client    import
BandwidthClient
bandwidth_client    =    BandwidthClient  (
messaging_basic_auth_user_name  =
“<username>”  ,
messaging_basic_auth_password  =
“<password>”  )
messaging_client    =
bandwidth_client  .  messaging_client  .  client
from
bandwidth.messaging.models.message_request
import    MessageRequest
messagingBody    =    MessageRequest  ()
messagingBody  .  to    =    [  “<to>”  ]
messagingBody  .  mfrom    =    “<from>”
messagingBody  .  text    =    “Hello, from
Python!”
messagingBody  .  application_id
=    “<app_id>”
“from”  :    “+12025553333”  ,
“text”  :    “Hello, from Python!”  ,
“media”  :    [
“ https://media.giphy.com/media/26gscSULUcfKU7dHq/source.gif  ”
],
“tag”  :    “custom string”  ,
“priority”  :    “default”
}  ’


```


```text
import    plivo
client    =    plivo  .  RestClient  (  '<auth_id>'  ,  '<auth_token>'  )
response    =    client  .  messages  .  create  (
src  =  ‘<sender_id>’  ,
dst  =  ‘<destination_number>’  ,
Text    =  ‘Hello, from Python!’  ,
media_urls  =  [  ‘ https://media.giphy.com/media/26gscSULUcfKU7dHq/source.gif  ’  ],
type_  =  ‘mms’  )
print  (  response  )
```


Alternatively, you can implement the same functionality using one of our[PHLO templates](https://console.plivo.com/phlo/list/) . For example, if you want to send an MMS message, your PHLO would look like this.


### More use cases


You can migrate applications for other use cases too:


- [Reply to incoming SMS messages](https://www.plivo.com/docs/sms/use-cases/reply-to-incoming-sms/node/)
- [Two-factor authentication](https://www.plivo.com/docs/sms/use-cases/2-factor-authentication/node/)
- [Forward incoming SMS messages](https://www.plivo.com/docs/sms/use-cases/forward-incoming-sms/node/)
- [Delivery reports](https://www.plivo.com/docs/sms/use-cases/delivery-reports/node/)
- [SMS alerts](https://www.plivo.com/docs/sms/use-cases/sms-alert/node/)
- [SMS marketing](https://www.plivo.com/docs/sms/use-cases/sms-marketing/node/)
- [SMS notifications](https://www.plivo.com/docs/sms/use-cases/sms-notification/node/)
- [SMS survey](https://www.plivo.com/docs/sms/use-cases/sms-survey/node/)
- [SMS autoresponder](https://www.plivo.com/docs/sms/use-cases/sms-autoresponder/node/)
- [Forward SMS to email](https://www.plivo.com/docs/sms/use-cases/forward-sms-to-email/node/)
- [Receive MMS](https://www.plivo.com/docs/sms/use-cases/receive-mms/python/)


## Porting your existing numbers from Bandwidth to Plivo


If you want to continue using your phone numbers from Bandwidth, you can port the numbers to Plivo painlessly without having any downtime on your services for your customers. Our[number porting guide](https://www.plivo.com/docs/numbers/number-porting/) shows you how to initiate the process.


## Simple and reliable


Those are the basics for migrating from Bandwidth to Plivo. Our simple APIs work in tandem with our comprehensive global network, using Plivo’s premium direct routes that guarantee the highest possible delivery rates and the shortest possible delivery times for your SMS messages, making Plivo the best[Bandwidth alternative](https://www.plivo.com/bandwidth-alternative/) . See for yourself -[sign up](https://console.plivo.com/accounts/register/) for a free trial account.
