---
schema_version: "1.0.0"
document_id: "709c7a2bb5b9cd18af004e16f0a48da5856b541334f6dad16789aeb4232a3763"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-receive-and-respond-incoming-mms-messages-in-dotnet/"
published_at: "2022-06-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:43479148897522d83987fbb59ac090ad68b7828b1546edf3f4bf87187c0b7aef"
---

# How to Receive and Respond to Incoming MMS Messages in .NET with ASP.NET MVC and Plivo

## Overview


This guide shows how to receive and automatically respond to incoming MMS messages on a[Plivo number](https://www.plivo.com/phone-numbers/) , as you might want to do for someone who’s out of the office or who leaves the company.


Here’s how to use[Plivo’s SMS APIs](https://www.plivo.com/sms/) to build this use case.


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. To receive incoming messages, you must have a Plivo phone number that supports MMS; you can rent numbers from the[Numbers](https://console.plivo.com/active-phone-numbers/) page of the Plivo console or by using the[Numbers API](https://www.plivo.com/docs/numbers/) . If this is your first time using Plivo APIs, follow our instructions to[set up a .NET development environment](https://www.plivo.com/docs/sdk/server/set-up-dotnet-dev-environment-api-messaging/) .


## Create a .NET controller


Navigate to the Controllers directory, create a controller called Autoresponder.cs, and paste into it this code.


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; padding: 15px 18px 15px 18px; } pre.lineno{ color: #fff; opacity: .3; } .w-richtext figure { max-width: 100%; position: relative; }


```text
using    System  ;
using    Plivo  ;
using    Microsoft.AspNetCore.Mvc  ;
namespace    demo.Controllers
{
public    class    Autoresponder    :    Controller
{
public    IActionResult    Index  ()
{
String    from_number    =    Request  .  Form  [  “From”  ];
String    to_number    =    Request  .  Form  [  “To”  ];
String    text    =    Request  .  Form  [  “Text”  ];
String    media_url    =    Request  .  Form  [  “Media0”  ];
Console  .  WriteLine  (  “Message received - From: {0}, To: {1}, Text: {2}, Media: {3}”  ,    from_number  ,
to_number  ,    text  ,    media_url  );
String    body  ;
String    media  ;
if    (  text  .  ToLower  ()    ==    “hi”  )
{
body    =    “Hello!”  ;
media    =    “ https://media.giphy.com/media/888R35MJTmDxQfRzfS/giphy.gif  ”  ;
}
else    if  (  text  .  ToLower  ()    ==    “bye”  )
{
body    =    “Bye and have a nice day!”  ;
media    =    “ https://media.giphy.com/media/QM5lHSyFjz1XW/giphy.gif  ”  ;
}
else
{
body    =    “I’m glad that we connected”  ;
}
var    api    =    new    PlivoApi  (  “<auth_id>”  ,    “<auth_token>”  );
var    response    =    api  .  Message  .  Create  (
src  :    to_number  ,
dst  :    from_number  ,
text  :  body  ,
type  :    “mms”  ,
media_urls  :    new    string  []    {  media  }
);
return    this  .  Content  (  response  .  ToString  ());
}
}
}


```


Run the project and you should see your basic server application in action at[http://localhost:5001/autoresponder/](http://localhost:5001/autoresponder/) .


[Set up ngrok](https://www.plivo.com/docs/sdk/server/set-up-dotnet-dev-environment-api-xml-voice/#ngrok-setup) to expose your local server to the internet.


## Create a Plivo application for the autoresponder


Associate the controller you created with Plivo by creating a Plivo application. Visiting Messaging >[Applications](https://console.plivo.com/sms/applications/) and click **Add New Application** . You can also use Plivo’s[Application API](https://www.plivo.com/docs/account/api/application/#create-an-application) .


Give your application a name - we called ours Autoresponder. Enter the server URL you want to use (for example https://<yourdomain>.com/autoresponder/) in the Message URL field and set the method to POST. Click **Create Application** to save your application.


## Assign a Plivo number to your application


**Note:** If you’re using a Plivo Trial account, you can send messages only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers >[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


From the Application Type drop-down, select XML Application.


From the Plivo Application drop-down, select Autoresponder (the name we gave the application).


Click **Update Number** to save.


## Test


Send a text message to the Plivo number you specified using any phone.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
