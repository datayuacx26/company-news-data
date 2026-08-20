---
schema_version: "1.0.0"
document_id: "8785bdd2522ace35b6c73506d9c941790d7ebde6bc6caa9573d370d5d22bd024"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-send-mms-in-dotnet-using-plivo-api/"
published_at: "2022-04-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:d04f6b5d02952b19c5c632aa09ca05d7742fb46d99d8af56fa50845f399c7ae8"
---

# How to Send MMS in .NET using Plivo’s Messaging API

You can use Plivo’s[SMS API](https://www.plivo.com/sms/) to[send an MMS](https://www.plivo.com/docs/sms/use-cases/send-an-mms/dotnet/) message to any phone number. Businesses can make messages more meaningful by using[MMS](https://www.plivo.com/blog/what-is-mms-messaging/) instead of SMS and including images, audio, and video to provide context.


## How it works


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. If this is your first time using Plivo APIs, follow our instructions to[set up a .NET development environment](https://www.plivo.com/docs/sdk/server/set-up-dotnet-dev-environment-api-messaging/) .


## Create the send MMS application


In Visual Studio, in the CS project, open the file Program.cs and paste into it this code.


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
26
27
28
29
30
31
using    System  ;
using    System.Collections.Generic  ;
using    Plivo  ;
using    Plivo.Exception  ;
namespace    SdkTestDotnet
{
class    Program
{
static    void    Main  (  string  []    args  )
{
var    api    =    new    PlivoApi  (  “<auth_id>”  ,  “<auth_token>”  );
try
{
var    response    =    api  .  Message  .  Create  (
src  :  “<sender_id>”  ,
dst  :  “<destination_number>”  ,
text  :  “Hello, from .NET!”  ,
type  :  “mms”  ,
media_urls  :    new    string  []{  “ https://media.giphy.com/media/26gscSULUcfKU7dHq/source.gif  ”  },
media_ids  :    new    String  []{  “801c2056-33ab-499c-80ef-58b574a462a2”  }
);
Console  .  WriteLine  (  response  );
}
catch    (  PlivoRestException    e  )
{
Console  .  WriteLine  (  “Exception: ”    +    e  .  Message  );
}
}
}
}


```


Replace the auth placeholders with your authentication credentials from the Plivo console. Replace the phone number placeholders with actual phone numbers in E.164 format (for example, +12025551234). In countries other than the US and Canada you can use a sender ID for the message source. You must have a Plivo phone number to send messages to the US or Canada; you can buy a Plivo number from Phone Numbers > Buy Numbers on the Plivo console or via the Numbers API.


Note: We recommend that you store your credentials in the auth_id and auth_token environment variables to avoid the possibility of accidentally committing them to source control. If you do this, you can initialize the client with no arguments and Plivo will automatically fetch the values from the environment variables. You can use the Environment.SetEnvironmentVariable method to store environment variables and Environment.GetEnvironmentVariable to fetch them when initializing the client.TestSave the file and run it.


Note: If you’re using a Plivo Trial account, you can send messages only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers > Sandbox Numbers page.


Haven’t tried[Plivo](https://www.plivo.com/) yet? Getting started is easy and only takes minutes. Sign up today.
