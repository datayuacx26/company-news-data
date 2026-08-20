---
schema_version: "1.0.0"
document_id: "00f121456d976d3b7a7756039b0efd1ccb447f2179514831994296d6b8b7a4d2"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/cancel-broadcast-api"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T20:48:26.603854+00:00"
fetched_at: "2026-08-18T20:48:29.213529+00:00"
content_hash: "sha256:d2b28067b228646f8723f095893f3da9b27783fbee681170c313768bf6ca6d3f"
---

# Cancel Broadcast API

Today we're releasing a new API endpoint to[cancel a Broadcast](https://resend.com/docs/api-reference/broadcasts/cancel-broadcast) .


The Broadcast API lets you programmatically[create](https://resend.com/docs/api-reference/broadcasts/create-broadcast) ,[update](https://resend.com/docs/api-reference/broadcasts/update-broadcast) ,[schedule](https://resend.com/docs/api-reference/broadcasts/send-broadcast) , and[send](https://resend.com/docs/api-reference/broadcasts/send-broadcast) Broadcasts through code. Programmatic control over Broadcasts enables workflows like:


- trigger Broadcasts from the CLI
- build your newsletter platform in-app
- create clients to build and send newsletters
- enable agent-led marketing flows through MCP


Sometimes things go wrong: you discover a typo or a wrong link mid-send. While you can cancel a Broadcast from the dashboard, starting today you can cancel the Broadcast programmatically using the API, too.


## How it works


Canceling has **different behavior** depending on the status of the Broadcast. You can cancel a Broadcast when it has either been` scheduled` , before any emails are sent, or` queued` , once emails start sending.


When you cancel a Broadcast, here's what happens based on its status:


- **` scheduled` ->` draft`** : no emails are sent, you can update the Broadcast and reschedule it to be sent.
- **` queued` ->` canceled`** : emails that have already been sent are not affected, but any emails still in the queue will no longer be sent.


## Canceling a Broadcast


When you[create](https://resend.com/docs/api-reference/broadcasts/create-broadcast) or[send a Broadcast](https://resend.com/docs/api-reference/broadcasts/send-broadcast) the API returns an ID. You can also get a Broadcast's ID by[listing broadcasts](https://resend.com/docs/api-reference/broadcasts/list-broadcasts) .


To cancel a Broadcast, use the Broadcast's ID with the cancel endpoint.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  broadcasts  .  cancel  (        '559ac32e-9ef5-46fb-82a1-b76b840c0f7b'  ,     )  ;


```


The cancel Broadcast API is part of a larger move to bring the full functionality of the dashboard into the API, so you can build how you want. Build full Broadcast functionality into your own app, use it from the CLI, or delegate it to your agent through the[MCP server](https://resend.com/docs/mcp-server) . Stay tuned for more.
