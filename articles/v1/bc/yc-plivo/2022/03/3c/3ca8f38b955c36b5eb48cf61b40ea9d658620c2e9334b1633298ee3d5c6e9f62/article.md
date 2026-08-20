---
schema_version: "1.0.0"
document_id: "3ca8f38b955c36b5eb48cf61b40ea9d658620c2e9334b1633298ee3d5c6e9f62"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-send-mms-in-java-using-plivo-api/"
published_at: "2022-03-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:2437e4235cee562b708473b99b9345c3f4e5d3e71d5624ca3511a300604ce005"
---

# How to Send MMS in Java using Plivo's Messaging API

You can[send an MMS](https://www.plivo.com/docs/sms/use-cases/send-an-mms/java/) message to any phone number. Businesses can make messages more meaningful by using[MMS](https://www.plivo.com/blog/what-is-mms-messaging/) instead of SMS and including images, audio, and video to provide context. Here’s how to use Plivo’s[SMS APIs](https://www.plivo.com/sms/) to send outbound MMS text messages.


## How it works


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. If this is your first time using Plivo APIs, follow our instructions to[set up a Java development environment](https://www.plivo.com/docs/sdk/server/php-sdk/) .


## Create the send MMS application


Once you have the above prerequisites set, You can follow the below instructions to create an app to send an outbound MMS to deliver your message.


Create a Java class called *SendMMS* and paste into it this code.


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
package    com.plivo.api  ;
import    java.io.IOException  ;
import    com.plivo.api.exceptions.PlivoRestException  ;
import    com.plivo.api.models.media.Media  ;
public    class    SendMMS    {
public    static    void    main  (  String  []    args  )    {
Plivo  .  init  (  “<auth_id>”  ,    “<auth_token>”  );
MessageCreateResponse    response    =    Message  .  creator  (  “<sender_id>“  ,  Collections  .  singletonList  (  “<destination_number>”  ),
“Hello, from Java!“  ).  type  (  MessageType  .  MMS  )
.  media_urls  (  new    String  []{  “ https://media.giphy.com/media/26gscSULUcfKU7dHq/source.gif  ”  })
.  media_ids  (  new    String  []{  “801c2056-33ab-499c-80ef-58b574a462a2”  })
.  create  ();
System  .  out  .  println  (  response  );
}
}
}


```


Replace the auth placeholders with your authentication credentials from the Plivo console. Replace the phone number placeholders with actual phone numbers in E.164 format (for example, +12025551234). In countries other than the US and Canada you can use a sender ID for the message source. You must have a Plivo phone number to send messages to the US or Canada; you can buy a Plivo number from Phone Numbers > Buy Numbers on the Plivo console or via the Numbers API.


Note: We recommend that you store your credentials in the auth_id and auth_token environment variables to avoid the possibility of accidentally committing them to source control. If you do this, you can initialize the client with no arguments and Plivo will automatically fetch the values from the environment variables. You can use System.getenv() to store and retrieve environment variables when initializing the client.


‍


## Test


Save the file and run it.


Note: If you’re using a Plivo Trial account, you can make calls only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers > Sandbox Numbers page.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes. Sign up today.
