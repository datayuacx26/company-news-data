---
schema_version: "1.0.0"
document_id: "b5e1647ca335bcefe6313899484ad7f605beca6a2558afff9647880edd6f7e5f"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/access-control"
published_at: "2025-06-03T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:3cb4e04930fd77d26b6d74824473d2d5d6c933ae2099a74d2fd2f3889de8a3b1"
---

# Streams as web resources with access controls

One of the things we have learned since[introducing S2](https://s2.dev/blog/intro) is that fine-grained access control is essential to bringing streaming data truly **online** .


We have seen a lot of excitement around our[object-storage-like API](https://s2.dev/docs/concepts/records) , and a recurring ask has been a parallel mechanism to *pre-signed URLs* .


If you are not familiar with it, pre-signed URLs are a neat way to grant temporary, controlled access to objects in your storage bucket. The URL embeds time-bounded authentication and authorization information, allowing anyone with the URL to perform a specific action (like` PUT` or` GET` ) on a specific object until the URL expires.


You can see where this is going — S2 streams are already available at public endpoints[over HTTP](https://s2.dev/docs/api/protocol) . The API simply needed to support more granular access control.


[Now it does!](https://s2.dev/docs/concepts/access-tokens)


> S2 supports an **unlimited** number of **revokable** access tokens that can be **scoped** to a set of resources and the operations that are allowed on those resources.
>
>
> You can issue **permanent** access tokens for services, or **time-bounded** ones for ephemeral usage, e.g. for end users to access an S2 stream.


Just like streams, there is no limit on the number of access tokens. After all, a key use case is being able to grant access to streams for "edge" clients — be they browsers, apps, or agents.


You get the flexibility to scope resources by an exact name, or a name prefix. In fact, when scoping streams with a prefix, you can enable transparent namespacing with a knob telling S2 to *auto-prefix* .


To make this concrete, let's take it for a spin!


## Usage example


First, a quick review of S2[concepts](https://s2.dev/docs/concepts/records) would be helpful.


We will assume we have a basin storing data from headless browser sessions, with streams like this:


```text
$   s2   ls   browser-instances-wtvr
s2://browser-instances-wtvr/session/g7wrzr9t/console   2025-06-03T18:05:19Z
s2://browser-instances-wtvr/session/g7wrzr9t/dom-events   2025-06-03T18:05:26Z
s2://browser-instances-wtvr/session/g7wrzr9t/telemetry   2025-06-03T18:05:31Z
s2://browser-instances-wtvr/session/rhc2q95x/console   2025-06-03T18:05:35Z
# ...
```


While we could issue a new access token with the[CLI](https://s2.dev/docs/quickstart) too, let's try with good old` curl` for a change. (Of course, you will need an existing access token, which was perhaps issued from the[dashboard](https://s2.dev/dashboard) , or itself using the[API](https://s2.dev/docs/api) .)


```text
$   curl   --request   POST   \
--url   https://a.s2.dev/v1/access-tokens   \
--header   "Authorization: Bearer ${  S2_ACCESS_TOKEN  }"   \
--header   "Content-Type: application/json"   \
--data   '{
"id": "agent/g7wrzr9t",
"scope": {
"basins": { "exact": "browser-instances-wtvr" },
"ops": ["list-streams", "append"],
"streams": { "prefix": "session/g7wrzr9t/" }
},
"auto_prefix_streams": true,
"expires_at": "2025-06-04T08:00:00Z"
}'
{  "access_token"  :  "YAAAAAAAAABoPzx+yjvI25QSuwCzq9nnFna3rZF2tSkqRnma"  }
```


We can confirm we got that right in the dashboard:


Note the scoping above, and how if we hand this access token to our beloved agent` g7wrzr9t` , it can only observe streams under its prefix:


```text
$   S2_ACCESS_TOKEN="YAAAAAAAAABoPzx+yjvI25QSuwCzq9nnFna3rZF2tSkqRnma"   s2   ls   browser-instances-wtvr
s2://browser-instances-wtvr/console   2025-06-03T18:05:19Z
s2://browser-instances-wtvr/dom-events   2025-06-03T18:05:26Z
s2://browser-instances-wtvr/telemetry   2025-06-03T18:05:31Z
```


... but it is completely oblivious to that naming scheme, the prefix has been stripped.


We did allow it to write to those streams ✅


```text
$   curl   --request   POST   \
--url   https://browser-instances-wtvr.b.s2.dev/v1/streams/console/records   \
--header   'Authorization: Bearer YAAAAAAAAABoPzx+yjvI25QSuwCzq9nnFna3rZF2tSkqRnma'   \
--header   'Content-Type: application/json'   \
--data   '{ "records": [ { "body": "hello world" }, { "body": "nice to be online" } ] }'
{  "start"  :  {  "  seq_num  ":0,"  timestamp  ":1748976301339},"  end  ":{"  seq_num  ":2,"  timestamp  ":1748976301339},"  tail  ":{"  seq_num  ":2,"  timestamp  ":1748976301339}}
```


... but not delete them 🚫


```text
$   curl   --request   DELETE   \
--url   https://browser-instances-wtvr.b.s2.dev/v1/streams/console   \
--fail-with-body
curl:   (22) The requested URL returned error: 403
{  "message"  :  "Operation not permitted"  }
```


## New possibilities


What would be the alternative to an edge client directly accessing a stream? The answer, as always in computing, is a layer of indirection. You would have to implement a proxy that takes care of authenticating and authorizing, and all data flows through.


For many use cases, this can now be obviated, and you can offload your streaming data plane to S2!


With horizontal building blocks like granular access control — and S2 itself, designed to deliver streams as a cloud storage primitive — the possibilities are endless. Nevertheless, let's consider some examples:


-


Running jobs on behalf of your users, such as builds? Let their browser **tail logs directly** , without having to implement a streaming endpoint yourself. You can simply hand out a read-only access token to a stream that you write to from the build runner. Long-term storage with real-time tailing, converged.


-


Multi-player applications like **collaborative document editing** — allow reads and writes against a shared stream, but forbid trimming. The stream can serve as an immutable ledger, and the backend can retain responsibility to checkpoint state and trim.


-


Building an application powered by a **sync engine** — the tech used by Linear, Figma, and Notion to power amazing UX? You may want change logs that your clients get read-only access to, while accepting updates over a write-only stream.


-


In the business of **personalization** ? Ingest user events from your customers as they happen over a write-only stream, and provide them read-only access to user-specific recommendation streams.


-


Exercising S2's[MCP server](https://s2.dev/docs/integrations/mcp) , or developing an agentic app that reacts to real-time events? You probably want some **guardrails for your AI** with tight scoping.


## What's next


S2 will keep solving for bringing streams online. We have some immediate followups on our roadmap:


-


**Massive read fanout:** one stream being read by thousands of clients? We intend to be up to the challenge!


-


**Usage visibility and limits per access token:** so you can grant untrusted code access without letting it go amok.


Reach out byemail or our[Discord](https://discord.gg/vTCs7kMkAf) if you want to find out more or share your requirements.
