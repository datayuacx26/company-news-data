---
schema_version: "1.0.0"
document_id: "d58e38dc75ed19b9a16bbcb20d426802798b5a9fa93f5a92ca2bfd11119ecc1d"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/svix-sdks-v2/"
published_at: "2026-08-19T12:00:00+00:00"
first_seen_at: "2026-08-20T01:05:38.755361+00:00"
fetched_at: "2026-08-20T01:05:40.232391+00:00"
content_hash: "sha256:28f46714068c50ae013964e9eacf4f287e7f29e3b7580352e9470658c9fbdc8b"
---

# Svix SDKs and CLI v2 released

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


We are excited to announce that our SDKs and CLI have gotten an overhaul and are now at version 2.0.0.


This is not a rewrite or big feature release, so expect the new SDKs to have the same look-and-feel as v1. Nevertheless, there's a decent number of changes because we had been collecting small pieces of tech debt over the years.


If you're using an SDK for a compiled language or common static analysis tools for interpreted languages, the tooling will tell you about most of the changes that affect you.


# Most important breaking changes


The change that I want to highlight first is the only one that will *not* be caught by static analysis: The` with_content` parameter on various message / message-attempt APIs now defaults to` false` in the SDKs.


For` svix.message.create()` in particular the previous default meant that we were always mirroring back the message content from the server that was just received - quite the waste of bandwidth on both sides.


For other APIs, in particular` svix.message.get()` , it is more common that one actually is making the API call to get the message content. Still, we felt not returning the content consistently on all endpoints was the better default. To get a message with its payload, simply set` with_content` to` true` explicitly, e.g. for JS:


```text
messageOut  =    await   svix .  message  .  get  (  appId ,   msgId ,    {    withContent  :    true    }  )  ;


```


… or for Python:


```text
message_out  =   svix .  message .  get (  app_id ,   msg_id ,   MessageGetOptions (  with_content =  True  )  )


```


Apart from this, the thing that will impact most users is the renaming of many` update` operations to` set` or` upsert` , depending on their semantics. For instance,` svix.application.update` is now` svix.application.upsert` , and` svix.endpoint.updateHeaders` is now` svix.endpoint.setHeaders` .


# Language-specific changes


The language-specific changes I want to point out explicitly are these:


- JavaScript: Dropped support for CommonJS
- Python, Ruby: For consistency with other languages and to support non-JSON payloads,` webhook.verify` no longer does JSON parsing
- Kotlin: Upgraded required Kotlin version to 2.3.20, changed timestamps to be of the type` kotlin.time.Instant`
- Go: Changed (sub)resource accessors from properties to methods, e.g.` svix.Ingest.Endpoint` to` svix.Ingest().Endpoint()`
- Rust


- Changed timestamps to be of the type` chrono::DateTime<Utc>`
- Reordered constructor parameters to a more natural order. Most importantly,` EventTypeIn::new()` s` name` and` description` have been swapped.


# Full changelog


As usual, you can find the full changelog[on GitHub](https://github.com/svix/svix-webhooks/blob/main/ChangeLog.md#version-200) .


# Support


The v1.x release line will continue to receive security fixes for a while, so you don't need to rush the upgrade.


If you have any questions or problems related to the new SDK (or CLI) versions, we are glad to help via the usual support channels (e.g.[our community Slack](https://www.svix.com/slack/) ).


---


For more content like this, make sure to follow us on[Twitter](https://twitter.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
