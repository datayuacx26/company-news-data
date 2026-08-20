---
schema_version: "1.0.0"
document_id: "d0630b558a464f407b8493614aa5c630e6957568abfeab755ecbb90388e98fab"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/15-10-2025"
published_at: "2025-10-15T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:30.502118+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:32fed901a94315d0f741f40863437b36bd64cf8c1c4e807736ee10d55bf3991f"
---

# Code Use

` CodeAgent` writes Python and JavaScript for native browser interaction via CDP. Custom LLM trained for code generation only.


```text
from   browser_use   import   CodeAgent, ChatBrowserUse


agent   =   CodeAgent(
task  =  task,
llm  =  ChatBrowserUse(),   # requires our LLM
)
await   agent.run()
```


Use cases:


- Extract 1000 products, filter by price
- Get 2000 GitHub stargazers with metadata
- Scrape projects, filter by stars, export CSV


Available in 0.9.0 (OSS). Grab the API key from[cloud](https://cloud.browser-use.com/new-api-key) .
