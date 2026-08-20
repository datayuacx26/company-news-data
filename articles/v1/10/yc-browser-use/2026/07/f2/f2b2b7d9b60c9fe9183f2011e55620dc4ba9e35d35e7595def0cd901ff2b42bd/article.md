---
schema_version: "1.0.0"
document_id: "f2b2b7d9b60c9fe9183f2011e55620dc4ba9e35d35e7595def0cd901ff2b42bd"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/17-7-2026"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:30.502118+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:43223aad760d1fed654200ca02f704c9cecfb93a9ba42a02ee9b78b113bb4723"
---

# Browser Harness API, New Models & Lower Top-Up Minimum

## Browser Harness API


The Browser Harness (v4) agent API is now part of our public API surface — listed in the OpenAPI spec, docs, and SDK codegen.


- Run and session` status` fields are now typed enums instead of raw strings.
- ` GET /runs` ,` GET /sessions` , and the run event log now use cursor-based pagination instead of offset paging.
- Follow-up messages are now queued per session at` /sessions/{id}/queue` , with an` interrupt` flag to cancel the active run and apply a message immediately.
- ` GET /runs/{id}/messages` is now` GET /runs/{id}/events` .
- Per-run spend caps are now configurable instead of a fixed $5 default:


```text
curl   https://api.browser-use.com/api/v4/runs   \
-H   "X-Browser-Use-API-Key:   $BROWSER_USE_API_KEY  "   \
-H   "Content-Type: application/json"   \
-d   '{"task": "...", "maxCostUsd": 25}'
```


## New Models


- **Grok 4.5** (xAI) replaces GLM 5.2 in the Browser Harness model picker.
- **Kimi K3** (Moonshot) is now available in Browser Harness.
- **Claude Fable 5** is available via the API on Browser Harness.
- **GLM** and **MiniMax** are now available natively on Computer Use and Browser Use.


## Pricing


Pay-as-you-go minimum top-up lowered from **$25 to $5** , so it's easier to try Browser Use Cloud without committing to a larger purchase.


## Fixes


- Cancelled runs could briefly flip back to "completed" after the cancellation landed — run status is now sticky once it reaches a terminal state.
- Runs cut off by the output limit were sometimes reported as producing no response, or incorrectly marked successful — they're now reported as truncated.
- Automated (API and scheduled) runs on V3 now wait for CAPTCHA auto-solve instead of giving up early.
