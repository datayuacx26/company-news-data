---
schema_version: "1.0.0"
document_id: "7a4c0627ba931a54b59be58f989ffff17d69ee25020d386cbf677afd2de33bfd"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/logs-api"
published_at: "2026-04-09T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:8b5c06a7408acbff8a79d373123905f77f71f0786fac710b67490185638e8a19"
---

# Logs API

When something goes wrong with an API request, the most important thing is visibility.


You've always had access to view your API logs in the Resend[dashboard](https://resend.com/logs) . That works well for manual debugging, but it falls short when you need to equip an agent, build monitoring tools, or create custom tooling around your email sending.


Today, we're introducing two new endpoints that give you programmatic access to your API request logs.


## Retrieving logs


To use the Logs API, call the following endpoint.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  logs  .  list  (  )  ;


```


The returned list of logs includes details like:


- endpoint
- method
- status code
- user agent


## Drilling down


When you need the full picture, **retrieve a single log entry** to inspect the original request and response bodies.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  logs  .  get  (        '37e4414c-5e25-4dbc-a071-43552a4bd53b'  ,     )  ;


```


For more details, check out the[Logs API reference](https://resend.com/docs/api-reference/logs/list-logs) .


## Getting started


Retrieving and inspecting logs programmatically opens up new possibilities:


- Equip your agent with better debugging tools
- Build custom dashboards, reports, and monitoring tools
- Integrate with your existing infrastructure


The Logs API is available in all[official Resend SDKs](https://resend.com/docs/sdks) . It's also integrated into the[Resend CLI](https://resend.com/changelog/cli) and[MCP Server](https://resend.com/changelog/mcp) .


We hope these new endpoints help you get more visibility into your email events, enabling you to build with more confidence than ever before.
