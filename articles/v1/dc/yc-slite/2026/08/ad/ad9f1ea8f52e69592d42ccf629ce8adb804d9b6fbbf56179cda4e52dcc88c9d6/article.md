---
schema_version: "1.0.0"
document_id: "ad9f1ea8f52e69592d42ccf629ce8adb804d9b6fbbf56179cda4e52dcc88c9d6"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/help-center-ai-search"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T03:01:16.051845+00:00"
fetched_at: "2026-08-07T03:01:17.125362+00:00"
content_hash: "sha256:f6938214332180c3be8d7921c6d833ffdcd84de234503f3aa35ee1536308341b"
---

# Help Center AI search

AI-powered search is now live on the Help Center. Ask a question in natural language at[slite.com/help](http://slite.com/help) and get a direct, sourced answer generated from our documentation instead of a list of articles.


### Agent badges & cleaner diffs


Doc history now shows which agent made each edit. Every change is attributed to its author (Slite Agent, Claude, or ChatGPT), and per-author change bars have been redesigned for clearer, more readable diffs.


### Other improvements


- Out-of-credits experience improved: the error now appears above the Ask input (no more phantom "Analysing query" flash), your question is restored, and admins see a "Top up credits" button.
- Automated run failure notifications: when an agent run or digest fails due to missing credits, the scheduler is now notified instead of a silent failure.
- Desktop app: Cmd+T opens a new tab, Cmd+Shift+T reopens the last closed tab
- Workspace admins can now manage doc roles on read-only docs.


### Fixes


- Clicking a source in the Ask modal now opens the actual file, PDFs and uploaded files open correctly instead of a broken Slite doc URL.
- Improved filters in the user management panel now show which filters are active, including defaults.
- MCP tools no longer time out on most agent queries — the previous 60-second limit was causing widespread failures and has been fixed.
- Removed the broken "Share to Slack" button from the share modal.
