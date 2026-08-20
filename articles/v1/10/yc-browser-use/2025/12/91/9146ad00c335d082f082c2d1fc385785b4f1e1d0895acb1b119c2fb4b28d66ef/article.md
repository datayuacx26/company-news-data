---
schema_version: "1.0.0"
document_id: "9146ad00c335d082f082c2d1fc385785b4f1e1d0895acb1b119c2fb4b28d66ef"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/16-12-2025"
published_at: "2025-12-16T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:30.502118+00:00"
fetched_at: "2026-07-28T21:27:04.798558+00:00"
content_hash: "sha256:dc8cd990e3a29650971c4923c5597d6958686857275960ebd488318c7efe8b5e"
---

# Our First Open-Source LLM

BU-30B-A3B-Preview is here.


- **30B total parameters** with only **3B active** at inference time
- **200 tasks per $1** — 4x more cost-efficient than BU 1.0


Try it out on the library or on our cloud:


```text
from   browser_use   import   Agent
from   browser_use.llm   import   ChatBrowserUse


# Initialize with the new model
llm   =   ChatBrowserUse(  model  =  "bu-30b-a3b-preview"  )


agent   =   Agent(
task  =  "Your task here"  ,
llm  =  llm
)


result   =   await   agent.run()
```


Download from[Hugging Face](https://huggingface.co/browser-use/bu-30b-a3b-preview) .
