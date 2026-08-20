---
schema_version: "1.0.0"
document_id: "d31eb6ad57835730d88934a61694a3e68a837c56309a65fcc3efb05914f329ca"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/17-9-2025"
published_at: "2025-09-17T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:30.502118+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:782629fe78afef4bf7a7b91e354325f32352d2584c053adee07df17e3176085c"
---

# Browser Use can write Javascript

In your prompts you can now prompt the agent to write JavaScript code to interact with elements on the website.


This enables more precise control and complex interactions that go beyond standard browser automation—perfect for handling edge cases or custom behaviors.


```text
from   browser_use   import   Agent


agent   =   Agent(
task  =  "Write JavaScript to extract all product prices from the page"  ,
llm  =  llm
)


result   =   await   agent.run()
```


The agent will automatically write and execute the necessary JavaScript to complete your task.
