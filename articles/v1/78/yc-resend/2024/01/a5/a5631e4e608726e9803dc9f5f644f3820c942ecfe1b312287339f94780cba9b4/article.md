---
schema_version: "1.0.0"
document_id: "a5631e4e608726e9803dc9f5f644f3820c942ecfe1b312287339f94780cba9b4"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/manage-subscribers-using-resend-audiences"
published_at: "2024-01-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:26:19.240569+00:00"
content_hash: "sha256:f857c4f96c39e24148f1b043cb4671907c2a9a806066e6dc9fbd61f7b32c28ca"
---

# Manage subscribers with Resend Audiences

Managing subscribers and unsubscribers is a critical part of any email implementation. It's important to respect your users' preferences and ensure that they're receiving the right emails at the right time.


Today, we're excited to announce the launch of[Resend Audiences](https://resend.com/audiences) , a new feature that allows you to manage your contacts in a simple and intuitive way.


These new capabilities include 9 new endpoints in the[Resend API](https://resend.com/docs/api-reference/audiences/create-audience) , which you can use to control audiences and contacts programmatically.


Resend Audiences Endpoints


## Import Contacts


To get started, you can add contacts manually or import them via CSV.


After you upload your CSV file, you'll be able to map the fields you want to use.


Currently, the supported fields are` email` ,` first_name` ,` last_name` , and` unsubscribed` .


## Add Contacts Programmatically


Instead of manually importing contacts, you can also add them programmatically.


You can leverage one of the Resend SDKs to add contacts to your audience.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
resend  .  contacts  .  create  (  {        email  :     'steve.wozniak@gmail.com'  ,        firstName  :     'Steve'  ,        lastName  :     'Wozniak'  ,        unsubscribed  :     false  ,        audienceId  :     '78261eea-8f8b-4381-83c6-79fa7120f1cf'  ,     }  )  ;


```


## Send emails to your Audience


Audiences were designed to be used in conjunction with[Broadcasts](https://resend.com/broadcasts) .


You can send emails to your Audience by creating a new Broadcast and selecting the Audience you want to send it to.


Send emails to your Audience


You can include the Unsubscribe Footer in your Broadcasts, which will be automatically replaced with the correct link for each contact.


Unsubscribe Link


## Automatic Unsubscribes


When you send emails to your Audience, Resend will automatically handle the unsubscribe flow for you.


If a contact unsubscribes from your emails, they will be skipped when sending a Broadcast.


Automatic Unsubscribes


We also include the proper unsubscribe headers automatically, according to[Gmail and Yahoo's bulk sending requirements for 2024](https://resend.com/blog/gmail-and-yahoo-bulk-sending-requirements-for-2024) .


```text
List  -  Unsubscribe  :     <  https  :  /  /  example  .  com  /  unsubscribe  >    List  -  Unsubscribe  -  Post  :   List  -  Unsubscribe  =  One  -  Click
```


## Get Started


Go to[Audiences](https://resend.com/audiences) and start adding contacts today.


You can get started with 1,000 contacts for free.


Once you're ready to upgrade, you can get a paid plan starting at $40/mo for 5,000 contacts. See[Pricing](https://resend.com/pricing) for all the different tiers.
