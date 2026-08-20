---
schema_version: "1.0.0"
document_id: "53c9ad4d5000943e34f1a39aed49e92bc7bcd7c9175f47d2269dc0a9c4614eac"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/19-9-2025"
published_at: "2025-09-19T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:30.502118+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:cddfab21925d31ec4671d253ad1bdbbc4dbcaa3fb1e5a0db849d836f151b2f38"
---

# Actor Use

Combine precise element interactions with AI-driven automation in a single workflow.


```text
from   browser_use   import   Browser, Agent
from   browser_use.llm.openai   import   ChatOpenAI


async   def   main  ():
llm   =   ChatOpenAI(  api_key  =  "your-api-key"  )
browser   =   Browser()
await   browser.start()


# 1. Actor: Precise navigation and element interactions
page   =   await   browser.new_page(  "https://github.com/login"  )
email_input   =   await   page.must_get_element_by_prompt(  "username field"  ,   llm  =  llm)
await   email_input.fill(  "your-username"  )


# 2. Agent: AI-driven complex tasks
agent   =   Agent(  browser  =  browser,   llm  =  llm)
await   agent.run(  "Complete login and navigate to my repositories"  )


await   browser.stop()
```
