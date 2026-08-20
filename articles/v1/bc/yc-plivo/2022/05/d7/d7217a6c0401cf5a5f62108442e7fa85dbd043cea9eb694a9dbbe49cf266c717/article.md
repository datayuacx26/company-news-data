---
schema_version: "1.0.0"
document_id: "d7217a6c0401cf5a5f62108442e7fa85dbd043cea9eb694a9dbbe49cf266c717"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-send-mms-in-go-using-plivo-api/"
published_at: "2022-05-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:633df8443786b068076e14d6bff7fe391a121120c1818b2854e1121d1aa55dc0"
---

# How to Send MMS in Go using Plivo’s Messaging API

## Overview


This guide shows how to[send an MMS](https://www.plivo.com/docs/sms/use-cases/send-an-mms/java/) message to any phone number. Businesses can make messages more meaningful by using[MMS](https://www.plivo.com/blog/what-is-mms-messaging/) instead of SMS and including images, audio, and video to provide context.


Here’s how to use Plivo’s[SMS APIs](https://www.plivo.com/sms/) to send outbound MMS text messages.


## How it works


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. If this is your first time using Plivo APIs, follow our instructions to[set up a Go development environment](https://www.plivo.com/docs/sdk/server/set-up-go-dev-environment-api-messaging/) .


## Create the send MMS application


Create a file called SendMMS.go and paste into it this code.


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
16
17
18
19
20
21
22
23
24
25
package    main
import    (
“fmt”
plivo    “github.com/plivo/plivo-go/v7”
)
func    main  ()    {
client  ,    err    :=    plivo  .  NewClient  (  “<auth_id>”  ,  “<auth_token>”  ,
&  plivo  .  ClientOptions  {})
if    err    !=    nil    {
panic  (  err  )
}
createResp  ,    err    :=    client  .  Messages  .  Create  (  plivo  .  MessageCreateParams  {
Src  :    “<sender_id>”  ,
Dst  :     “<destination_number>”  ,
Text  :    “Hello, from Go!”  ,
Type  :    “mms”  ,
MediaUrls  :    []  string
{  “ https://media.giphy.com/media/26gscSULUcfKU7dHq/source.gif  ”  },
MediaIds  :    []  string  {  “801c2056-33ab-499c-80ef-58b574a462a2”  },
})
if    err    !=    nil    {
panic  (  err  )
}


```


Replace the auth placeholders with your authentication credentials from the Plivo console. Replace the phone number placeholders with actual phone numbers in E.164 format (for example, +12025551234). In countries other than the US and Canada you can use a sender ID for the message source. You must have a Plivo phone number to send messages to the US or Canada; you can buy a Plivo number from Phone Numbers > Buy Numbers on the Plivo console or via the Numbers API.


Note: We recommend that you store your credentials in the auth_id and auth_token environment variables to avoid the possibility of accidentally committing them to source control. If you do this, you can initialize the client with no arguments and Plivo will automatically fetch the values from the environment variables. You can use os.Setenv and os.Getenv functions to store environment variables and fetch them when initializing the client.


## Test


Save the file and run it.


```text
go run SendMMS.go


```


**Note:** If you’re using a Plivo Trial account, you can send messages only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers >[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
