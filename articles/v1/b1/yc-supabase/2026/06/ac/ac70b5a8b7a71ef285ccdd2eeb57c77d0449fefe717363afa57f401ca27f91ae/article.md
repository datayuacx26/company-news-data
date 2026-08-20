---
schema_version: "1.0.0"
document_id: "ac70b5a8b7a71ef285ccdd2eeb57c77d0449fefe717363afa57f401ca27f91ae"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/46834-realtime-broadcast-now-supports-binary-payloads"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:6ff4524e6738c72b89bad92a0741231c6a376f441b3cab3d747125d61d312e5d"
---

# Realtime Broadcast now supports binary payloads

Realtime Broadcast can now send and receive binary payloads in addition to JSON. This lets you broadcast compact binary data without the overhead of JSON encoding.


Binary payloads are supported across every way you send a Broadcast message: the client libraries (over WebSocket), the REST API, and directly from your database.


## When to use binary payloads#


Binary shines whenever your data is numeric, high-frequency or densely packed. In these cases JSON's textual encoding adds significant overhead.


- **Sensor and telemetry streams.** Connected devices emit steady streams of numeric readings. These can be temperature, accelerometer values, GPS coordinates, battery levels, etc. They can be packed into fixed-width binary fields, where a reading that takes 20–30 characters as JSON text fits in just a few bytes across thousands of devices.
- **Screenshot and presentation streaming.** A presenter or support agent shares a live view by broadcasting periodic image frames (JPEG/PNG) to viewers. Each frame is transient and inherently binary, so there's no need for durable storage and no reason to pay the base64 tax. Just send the image bytes straight to everyone watching.


## Usage#


### Client libraries (WebSocket)#


Pass an` ArrayBuffer` /` ArrayBufferView` (` Uint8Array` for example) or the equivalent in non-JS SDKs as the payload.


### REST API#


Single-message endpoint where` Content-Type` selects` application/json` vs` application/octet-stream` .` channel.httpSend()` from JS.


### Database#


` realtime.send_binary()` with a bytea payload.


## Minimum versions#


- WebSocket (supabase-js):` 2.91.0`
- WebSocket (supabase-swift):` 2.44.0`
- REST API through` httpSend` (supabase-js):` 2.107.0`
- Realtime server version:` 2.103.2`


We are in the process of adding support to binary payloads across all SDKs.


⚠️ Binary payloads sent to clients on older SDK versions (or SDKs that don't support binary payloads) are silently dropped.


More information can generally be found in the[Broadcast](https://supabase.com/docs/guides/realtime/broadcast) page of the Realtime guides.
