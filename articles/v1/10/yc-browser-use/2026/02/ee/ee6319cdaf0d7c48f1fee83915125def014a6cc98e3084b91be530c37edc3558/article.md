---
schema_version: "1.0.0"
document_id: "ee6319cdaf0d7c48f1fee83915125def014a6cc98e3084b91be530c37edc3558"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/25-2-2026"
published_at: "2026-02-25T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:30.502118+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:33ae90c621f2e6ec1580e77815f2eb21418615d831a6421f141bc4752d74a758"
---

# BU Agent API & SDK 3.0

### BU Agent API (Experimental)


A completely new experimental agent built from scratch. Think **Claude Code for the browser** : web scraping, data extraction, file manipulation, and complex multi-step workflows.


*"Here's a CSV with 50 people. For each person, find their LinkedIn profile, extract their current title and company, and return an enriched CSV."* — the BU Agent handles the entire pipeline in a single task.


```text
from   browser_use_sdk.v3   import   AsyncBrowserUse


client   =   AsyncBrowserUse()
result   =   await   client.run(  "Find the top 3 trending repos on GitHub today"  )
print  (result.output)
```


### SDK 3.0


New version of the SDK (` 3.0.x` ) with breaking changes. Please upgrade — the new` client.run()` API is much cleaner. All existing functionality — sessions, profiles, browser control, structured output, streaming, files, skills — is still there.


[Documentation](https://docs.cloud.browser-use.com/new-features/api-v3) ·[GitHub](https://github.com/browser-use/sdk)
