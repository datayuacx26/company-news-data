---
schema_version: "1.0.0"
document_id: "d07c5dc80ba1e346acba6b6444c5325704cce2f8a1913b7ab68f6766ed798ebc"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/announcing-seam-go-sdk"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:de1133889934bcf67720f3466e5b56e69a854fe01442b5b2fe1b3265e07ecf0b"
---

# Announcing our Seam Go SDK

This week we are excited to the beta launch of our Go SDK. You can now use Seam inside your Go-lang applications. You can browse the entire SDK repo[here](https://github.com/seamapi/go) .


## Why Release a Go SDKs? ⏩


As we continue to expand our device coverage, we also want to focus on identifying additional improvement to the developer experience we can provide. A few weeks ago, we were contacted by a developer interested in using Seam inside their Go application. Though this developer was planning to use our HTTP API, our team immediately began to look into the possibility of providing a Go SDK for this individual. Our announcement today is the result of this work.


## Unleashing the Gophers


To start run the following command to bring it into your project:


```text
1  go   get github  .  com  /  seamapi  /  go
```


Next, as with other Seam SDK, you just need to provide your API key to begin using it. For example, creating an[Access Code](https://docs.seam.co/latest/products/smart-locks/access-codes) can be achieved in the following manner:


```text
1  import   goclient   "github.com/seamapi/go/client"
2
3  client   :=   goclient  .  NewClient  (  goclient  .  WithApiKey  (  "<YOUR_AUTH_TOKEN>"  )  )
4  accessCode  ,   err   :=   client  .  AccessCodes  .  Create  (
5    context  .  TODO  (  )  ,
6      &  seamgo  .  AccessCodesCreateRequest  {
7      DeviceId  :   someDevice  .  DeviceId  ,
8      Name  :       seamgo  .  String  (  "Test code"  )  ,
9      Code  :       seamgo  .  String  (  "4444"  )  ,
10      }  ,
11   )
```


This SDK is in early` beta` . We will be upgrading it in the coming weeks toward a more table version. Please leave us a note atsupport@getseam.com or file issues in the Github repo if you find issues.
