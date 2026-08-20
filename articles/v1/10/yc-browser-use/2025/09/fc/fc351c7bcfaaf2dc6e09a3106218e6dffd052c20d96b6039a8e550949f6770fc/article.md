---
schema_version: "1.0.0"
document_id: "fc351c7bcfaaf2dc6e09a3106218e6dffd052c20d96b6039a8e550949f6770fc"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/30-9-2025"
published_at: "2025-09-30T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:30.502118+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:336e5a6ab2481c27661508da7eeacead33d975f82f94e036028ebc99ba6cd27e"
---

# Stealth Browser Infrastructure

### Features


- **Persistent profiles** - Maintain cookies, local storage, and session data across browser instances
- **Proxy support** - Route traffic through proxies to bypass geo-restrictions and bot detection
- **File handling** - Download files directly through the API without manual intervention


### Browser Sessions with SDK


Access raw browser instances via Chrome DevTools Protocol:


```text
from   browser_use_sdk   import   BrowserUse


client   =   BrowserUse(  api_key  =  "bu_..."  )


# Basic browser session
browser_session   =   client.browsers.create_browser_session(
timeout  =  30    # minutes
)


# With profile (inherit login state)
profile_browser   =   client.browsers.create_browser_session(
profile_id  =  "profile_123"  ,
proxy_country_code  =  "US"  ,
timeout  =  60
)


print  (  f  "CDP URL:   {  browser_session.cdp_url  }  "  )
print  (  f  "Watch live:   {  browser_session.live_url  }  "  )
```


For more technical details, read the blog at[/posts/browser-infra](https://browser-use.com/posts/browser-infra) .
