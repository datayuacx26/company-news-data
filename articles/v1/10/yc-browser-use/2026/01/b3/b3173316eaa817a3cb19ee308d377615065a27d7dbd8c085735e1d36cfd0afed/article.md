---
schema_version: "1.0.0"
document_id: "b3173316eaa817a3cb19ee308d377615065a27d7dbd8c085735e1d36cfd0afed"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/27-1-2026"
published_at: "2026-01-27T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:30.502118+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:52a607cb9d004cded246f51ef10b0f361fc98d888bec0eb05ba7db5b3e959c77"
---

# Browser Use Model - BU 2.0

BU 2.0 is here.


- **+12% accuracy** over BU 1.0 (74.7% → 83.3%)
- **Similar speed** — ~62s average task duration
- **Matches Claude Opus 4.5** accuracy while being **40% faster**


### Benchmark Results


Model Accuracy Avg Task Duration


**BU 2.0** **83.3%** **62s**


BU 1.0 74.7% 58s


Claude Opus 4.5 82.3% 104s


Gemini 3 Pro 81.7% 143s


GPT-5.2 70.9% 196s


### Pricing


Model Input Cached Output


bu-1-0 / bu-latest $0.20/1M $0.02/1M $2.00/1M


**bu-2-0** $0.60/1M $0.06/1M $3.50/1M


### Quick Start


```text
import   asyncio
from   browser_use   import   Agent
from   browser_use.llm   import   ChatBrowserUse


async   def   main  ():
# Use the new bu-2-0 model
llm   =   ChatBrowserUse(  model  =  "bu-2-0"  )


agent   =   Agent(
task  =  "Your task here"  ,
llm  =  llm
)


result   =   await   agent.run()
return   result


asyncio.run(main())
```


Get your API key at[cloud.browser-use.com](https://cloud.browser-use.com/new-api-key) .
