---
schema_version: "1.0.0"
document_id: "838264a29b3b802d9e31f8e4b76b2c7b0660fd37448c35052cb0cc485c5451bb"
company_key: "yc-the-hog"
company: "The Hog"
source_id: "yc-the-hog-news-import-764edb381fb6"
canonical_url: "https://thehog.ai/blog/introducing-the-hog-api"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-22T16:24:44.963526+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:9cf99184018c4798c9ce445d408230fd416744688bef7e9e5da8beea532269bf"
---

# Introducing the The Hog API

Agents have gotten remarkably good at reasoning. What they still lack is a clean, real-time view of the web — the kind of context a human researcher would gather before making a decision. Today we're opening up **The Hog** : one API to search, scrape, enrich, and monitor the live web, with output that's ready for an agent to consume.


## Why we built it


Every team building agents ends up rebuilding the same fragile stack: a scraper that breaks weekly, a parsing layer held together with regex, and a queue of proxies nobody wants to own. We wanted that whole layer to disappear behind a single endpoint.


- **Search** the open web and get structured results, not raw HTML
- **Scrape** any page and receive clean, agent-ready markdown
- **Enrich** people and companies from a name or a domain
- **Monitor** sources and get notified when something changes


## A first request


Point your agent at one endpoint and start turning live signals into structured intelligence:


```text
const   res   =   await   fetch  (  "https://api.thehog.ai/v1/search"  , {
method:   "POST"  ,
headers: {
Authorization:   `Bearer ${  process  .  env  .  HOG_API_KEY  }`  ,
"Content-Type"  :   "application/json"  ,
},
body:   JSON  .  stringify  ({
query:   "series B fintech companies hiring in the EU"  ,
limit:   10  ,
}),
});


const   {   results   }   =   await   res.  json  ();
```


Every response is normalized, deduplicated, and typed — so your agent spends its tokens reasoning, not parsing.


> Less parsing. More reasoning. That's the whole idea.


## What's next


This is the first slice of a much larger surface. Enrichment and monitoring are rolling out over the coming weeks. If you're building something that needs the freshest possible context, we'd love to have you in the early cohort.
