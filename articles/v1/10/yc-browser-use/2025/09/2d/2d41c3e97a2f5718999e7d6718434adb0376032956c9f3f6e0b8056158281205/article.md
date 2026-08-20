---
schema_version: "1.0.0"
document_id: "2d41c3e97a2f5718999e7d6718434adb0376032956c9f3f6e0b8056158281205"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/2-9-2025"
published_at: "2025-09-02T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:30.502118+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:fdfe41385d354758c3c037df1f9f01ff14d79f1cb238a25e5610b8de9b18aff5"
---

# Cloud SDK

You can control the cloud API using the typesafe SDK.


```text
from   browser_use_sdk   import   BrowserUse


client   =   BrowserUse(  api_key  =  "bu_..."  )


task   =   client.tasks.create_task(
task  =  "Search for the top 10 Hacker News posts and return the title and url."  ,
llm  =  "browser-use-llm"
)


result   =   task.complete()


result.output
```


For more information, check out[the documentation](https://docs.cloud.browser-use.com/) .
