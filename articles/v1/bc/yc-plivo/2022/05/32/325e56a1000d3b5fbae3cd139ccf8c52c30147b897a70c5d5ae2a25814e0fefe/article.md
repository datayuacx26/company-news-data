---
schema_version: "1.0.0"
document_id: "325e56a1000d3b5fbae3cd139ccf8c52c30147b897a70c5d5ae2a25814e0fefe"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-send-mms-in-ruby-using-plivo-api/"
published_at: "2022-05-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:7eea44299a65380335c31fd43410db36dc3b325407635051cff705c7af4e666a"
---

# How to Send MMS in Ruby using Plivo’s Messaging API

## Overview


This guide shows how to[send an MMS](https://www.plivo.com/docs/sms/use-cases/send-an-mms/java/) message to any phone number. Businesses can make messages more meaningful by using[MMS](https://www.plivo.com/blog/what-is-mms-messaging/) instead of SMS and including images, audio, and video to provide context.


Here’s how to use Plivo’s[SMS APIs](https://www.plivo.com/sms/) to send outbound MMS text messages.


## How it works


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. If this is your first time using Plivo APIs, follow our instructions to[set up a Ruby development environment](https://www.plivo.com/docs/sdk/server/set-up-ruby-dev-environment-api-messaging/) .


## Create the send MMS application


Create a file called send_mms.rb and paste into it this code.


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; } pre.lineno{ color: #fff; opacity: .3; } .w-richtext figure { max-width: 100%; position: relative; } td{ vertical-align: top; text-align: left; border-bottom: hidden; padding: 5px; }


```text
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
require    'rubygems'
require    'plivo'
include    Plivo
api    =    RestClient  .  new  (  “<auth_id>”  ,  “<auth_token>”  )
response    =    api  .  messages  .  create  (
src  :“<sender_id>”  ,
dst  :“<destination_number>”  ,
text  :“Hello, from Ruby!”  ,
media_urls  :[  “ https://media.giphy.com/media/26gscSULUcfKU7dHq/source.gif  ”  ],
media_ids  :[  “801c2056-33ab-499c-80ef-58b574a462a2”  ],
type:   “mms”
)
puts    response


```


Replace the auth placeholders with your authentication credentials from the Plivo console. Replace the phone number placeholders with actual phone numbers in E.164 format (for example, +12025551234). In countries other than the US and Canada you can use a sender ID for the message source. You must have a Plivo phone number to send messages to the US or Canada; you can buy a Plivo number from Phone Numbers > Buy Numbers on the Plivo console or via the Numbers API.


Note: We recommend that you store your credentials in the auth_id and auth_token environment variables to avoid the possibility of accidentally committing them to source control. If you do this, you can initialize the client with no arguments and Plivo will automatically fetch the values from the environment variables. You can use ENV to store environment variables and fetch them when initializing the client.


## Test


Save the file and run it.


```text
ruby send_mms.rb


```


**Note:** If you’re using a Plivo Trial account, you can send messages only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers >[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
