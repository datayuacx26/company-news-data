---
schema_version: "1.0.0"
document_id: "f02cf576d02ead6fc4414b901521a9070058090e9a74e6e656cabe82a8240070"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/new-acs-api-and-sdk"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:36072ba5e8e28b60f2607da0ea520f8f6adbe3d5e60f97768cb48bad04c38af9"
---

# Meet Our New Access Control Systems API & SDK

Today, we’re thrilled to announce **our new API for integrating physical access control systems** . This API enables software developers and AI agents to grant access across a number of systems, such as[Salto Space](https://www.seam.co/manufacturers/salto) ,[Salto KS](https://www.seam.co/manufacturers/salto) ,[Latch](https://www.seam.co/manufacturers/latch) ,[Assa Abloy Visionline](https://www.seam.co/manufacturers/assa-abloy) ,[Assa Abloy Vostio](https://www.seam.co/manufacturers/assa-abloy) , and[Dormakaba Community](https://www.seam.co/manufacturers/dormakaba) —with many more coming.


This API simplifies complex operations such as creating[plastic cards](https://docs.seam.co/latest/capability-guides/access-systems/acs-quick-starts/encodable-key-card-quick-start) ,[pin codes](https://docs.seam.co/latest/capability-guides/access-systems/acs-quick-starts/pin-code-quick-start) , and[mobile keys](https://docs.seam.co/latest/capability-guides/access-systems/acs-quick-starts/mobile-key-quick-start) , and comes with native iOS and Android SDKs for embedding mobile keys directly inside any mobile app.


This API has been in private beta for a while, and we want to thank all our design partners for your helpful and appreciated contributions over the past few months. It’s been a long journey, we have much more to go, and we can’t wait to see what you’ll build.


To get started, check out our quick start guides!


- [PIN Code Quick Start](https://docs.seam.co/latest/capability-guides/access-systems/acs-quick-starts/pin-code-quick-start)
- [Encodable Key Card Quick Start](https://docs.seam.co/latest/capability-guides/access-systems/acs-quick-starts/encodable-key-card-quick-start)
- [Mobile Key Quick Start](https://docs.seam.co/latest/capability-guides/access-systems/acs-quick-starts/mobile-key-quick-start)


For more information about our access control system API, see[our docs](https://docs.seam.co/latest/capability-guides/access-systems) .


## Why Build an API for Access Control Systems?


Many modern SaaS applications need to integrate with access control systems to deliver features to their users. For example, Envoy—a employee workplace app—integrates Bluetooth credentials into their mobile app to replace physical employee badges. Similarly, MEWS—a leading hospitality PMS—programs hotel room plastic cards for each guest at check-in.


Developers shared with us that these integrations are becoming must-have features. They also described the formidable challenges of integrating each of these systems to grant access.


Each integration demands painstaking work: requesting access, developing against complex APIs, procuring test hardware, and launching. Moreover, as many SaaS companies serve customers on a global basis, they must repeat this process across dozens of brands to support an ever-growing number of access control systems.


When we heard these stories and saw all the hoops that app developers needed to jump through, we knew there was an opportunity for us to provide a better experience, similar to what we've already done with smart locks and thermostats.


### Our Solution


As with all things Seam, our goal is to provide a simple API that makes it easy and intuitive for you to integrate multiple access control systems and issue access credentials to your users.


**Connecting a System**


To start, you can connect any supported access control system with a few clicks using our Connect Webview. We are also providing an on-premises bridge to connect offline systems, such as Assa Abloy Visionline or Salto Space.


**Provisioning New Users**


Once your system is connected, you can use our API to create users, grant them access to specific entrances, and issue credentials to them, such as PIN codes, plastic key cards, or mobile keys.


```text
1  // Add a user to the access control system.
2  myUser   =     await   seam  .  acs  .  users  .  create  (  {
3      acs_system_id  :   myAccessSystemID  ,
4      full_name  :     "Jane Doe"  ,
5      email_address  :     "jane@example.com"  ,
6      phone_number  :     "+15555550101"
7   }  )  ;
8
9   // Specify the user's access through access groups.
10   await   seam  .  acs  .  users  .  addToAccessGroup  (  {
11      acs_user_id  :   myUser  .  acs_user_id  ,
12      acs_access_group_id  :   accessGroupId
13   }  )  ;
14
15   // Create a credential to define the user's access method.
16   await   seam  .  acs  .  credentials  .  create  (  {
17      acs_user_id  :   myUser  .  acs_user_id  ,
18      access_method  :     "mobile_key"  ,
19   }  )  ;
```


**Unlock Doors Using Mobile Keys**


If you’ve issued a mobile key, you can use the[Seam mobile SDK](https://docs.seam.co/latest/capability-guides/mobile-access) to download this credential automatically and begin unlocking the door. Alternately, if you’ve issued a plastic card, you can use our[seam.acs.encoders API](https://docs.seam.co/latest/capability-guides/access-systems/working-with-card-encoders-and-scanners) to encode the key and begin using it.


### What's Next


While our new access control system API is already being used by dozens of apps, we are keenly aware that much still needs to be done. In the coming months, we plan not only to roll out integrations, but also to simplify many of the low-level concepts and expand on the types of credentials that we can support.


We are incredibly grateful for the feedback of our early design partners during this private beta, and we can’t wait to see what you can build with our new access control system API!
