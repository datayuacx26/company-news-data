---
schema_version: "1.0.0"
document_id: "bf7da13526908523ab8e77a1fbf309b01444eaf00983c21010fb196d959916f3"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/20-7-2026"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-21T23:47:20.405701+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:d2c0b46b46aa0ca4933f073e9633c6995b893f88300a8160a3cc318aa4b21795"
---

# Recording Controls & OAuth Browser Access

## Recording Controls


The Browser Harness (v4) API now lets callers control session recording instead of it being always-on.


- ` browserSettings.record` opts API calls into recording (off by default) or lets UI sessions opt out (on by default).
- An explicit` record` value always wins over the client default, and the setting is inherited by follow-up runs on the same browser.


```text
curl   https://api.browser-use.com/api/v4/runs   \
-H   "X-Browser-Use-API-Key:   $BROWSER_USE_API_KEY  "   \
-H   "Content-Type: application/json"   \
-d   '{"task": "...", "browserSettings": {"record": true}}'
```


## OAuth


OAuth apps using the terminal flow can now read existing browsers — the` v3:browsers:read` scope joins the existing create and profile scopes.
