---
schema_version: "1.0.0"
document_id: "f886449ec888f9992a7f8351b5ae9e13c48f3870c4818e4d6ea345197a588923"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/python-sdk-async-support"
published_at: "2026-04-02T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T22:16:03.530620+00:00"
content_hash: "sha256:55c8fb875753ff347df677a50e610bb3192496c0e45ff7bd6b21d86ef32e81f4"
---

# Async Support for the Python SDK

The Python SDK now supports async out of the box. If you are building with FastAPI, async Django, or any other async Python framework, you can now send emails without blocking your event loop.


## Installation


Install the SDK with the` async` extra to pull in` httpx` :


```text
pip   install     "resend[async]"
```


## Sending an email asynchronously


Every method in the SDK has an` _async` counterpart. Await it like any other coroutine:


```text
import   asyncio    import   resend
resend  .  api_key   =     "re_your_api_key"
async     def     main  (  )  :        params  :   resend  .  Emails  .  SendParams   =     {              "from"  :     "onboarding@resend.dev"  ,              "to"  :     [  "delivered@resend.dev"  ]  ,              "subject"  :     "Hello from async Python"  ,              "html"  :     "<strong>it works!</strong>"  ,          }
email   =     await   resend  .  Emails  .  send_async  (  params  )          print  (  email  )
asyncio  .  run  (  main  (  )  )
```


## Batch sending


The same pattern applies to batch sends:


```text
emails   =     await   resend  .  Batch  .  send_async  (  [  params1  ,   params2  ]  )
```


## Custom timeout


If you need to configure the underlying HTTP client, you can swap in your own instance:


```text
resend  .  default_async_http_client   =   resend  .  HTTPXClient  (  timeout  =  60  )
```


## No breaking changes


The existing sync API is unchanged —` resend.Emails.send(params)` continues to work exactly as before. Async is opt-in: if` httpx` is not installed, calling an` _async` method raises a` ResendError` with code` AsyncClientNotConfigured` .


All SDK modules have async counterparts:` Emails` ,` Batch` ,` ApiKeys` ,` Audiences` ,` Broadcasts` ,` Contacts` , and` Domains` .


Check out the[Python SDK on GitHub](https://github.com/resend/resend-python) if you want to follow along or open an issue.
