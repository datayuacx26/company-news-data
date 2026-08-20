---
schema_version: "1.0.0"
document_id: "07b3fb564ae5db03f6639c82f92fd7bfca18f4d2d33e1ebd5c7c206671a86a23"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/hello-c-sdk"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:10533cf6e6da7483f40ec78e386873fc1e7bc43fd0f8f369b411354d91ee8f40"
---

# Hello C# SDK!

Today we are releasing a beta version of our official C# SDK! You can find[official packages here](https://www.nuget.org/packages/Seam) .


## Why Release SDKs?


At Seam, we spend countless hours designing the best possible developer experience for interacting with IoT devices. Importantly, we think being able to control devices directly in your language of choice by providing language-specific SDKs is better than using the HTTP API directly (although you still can!). We craft these SDKs to give you object & types, error handling, and casing/styling that matches your preferred language.


## Rolling out C#


When we launched our API in 2022, we initially only released Javascript and Python SDKs. Soon after, we introduced PHP and Ruby as more developers began asking for these languages.


Today, we are releasing a beta of our C# SDK. This SDK comes with all the functions exposed by the Seam API and lets you control devices more effortlessly. For example, you can retrieve your devices with the following function call.


```text
1  using     Seam  .  Client  ;
2
3   var   seam   =     new     SeamClient  (  apiToken  :     "YOUR_API_KEY"  )  ;
4
5   var   myDevices   =   seam  .  Devices  .  List  (  )  ;
6
7  Console  .  WriteLine  (  "First Device Name: "     +   myDevices  [  0  ]  .  Properties  .  Name  )  ;
8
9   var   accessCode   =   seam  .  AccessCodes  .  Create  (  deviceId  :   myDevices  [  0  ]  .  DeviceId  ,     code  :     "1234"  )  ;
```


This SDK is still in early beta and we expect to be moving rapidly toward a stable release in the near future. If you find bugs, inconsistencies, or issues, please let us know by emailingsupport@getseam.com .
