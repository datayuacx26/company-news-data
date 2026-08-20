---
schema_version: "1.0.0"
document_id: "892fe72edee078bb0faccf3cd4db3cb20e135f007f5426f3d3eb558dbbee0d9d"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/new-seam-java-sdk"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:9434f5582562d0ffb968f7f353c58aae2ca80a88cf4067350fd159ceb94d447f"
---

# New Seam Java SDK

This week we are excited to the beta launch of our Java SDK. You can now use Seam inside your Java applications. You can browse the entire SDK repo[here](https://github.com/seamapi/java) .


## Why Release a Java SDKs? ☕


At Seam, we aim to provide the best developer experience to control IoT devices. Part of that effort consists of producing great SDKs that make Seam even easier to use, and IoT devices even simpler to control. We design these SDKs to match the programming environment in which Seam is being used. We also strongly encourage developers to use the Seam SDK in their preferred language versus using the Seam HTTP API directly.


Many Java developers already use Seam to control IoT devices. Unfortunately, until today, we did not have a proper Java SDK for them. We are now trying to make up for this grave injavastice 😊


## Getting Started with the Seam Java SDK


You can use either Gradle or Maven to pull in the Seam Java SDK into your project. As with other Seam SDK, you then just need to provide your API key to begin using it, such as in the example below.


```text
1  SeamApiClient   seam   =     SeamApiClient  .  builder  (  )
2            .  token  (  "MY_API_KEY"  )
3            .  build  (  )  ;
4   AccessCodesCreateResponse   accessCodesCreateResponse   =
5          seam  .  accessCodes  (  )  .  create  (  AccessCodesCreateRequest  .  builder  (  )
6                .  deviceId  (  "123e4567-e89b-12d3-a456-426614174000"  )
7                .  commonCodeKey  (  "My first code"  )
8                .  build  (  )  )  ;
9   System  .  out  .  println  (  accessCodesCreateResponse  .  getAccessCode  (  )  )  ;
```


This Java SDK is still in` beta` but we will be rapidly iterating toward a stable version. If you find issues, please leave us a note atsupport@getseam.com .
