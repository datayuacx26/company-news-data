---
schema_version: "1.0.0"
document_id: "d87e8edacb048bba7b8ba301c64da4b6e3dfa330806797f6acdf79ed66a0812c"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-30112aa6d3bf"
canonical_url: "https://tiptap.dev/blog/release-notes/hocuspocus-4-stable-release"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-07-22T16:45:40.643427+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:4019179002a5c4d79e44f70407dd7b520c38628087d75a6950207f1f61e0cb13"
---

# Hocuspocus 4 stable release

Hocuspocus 4 has been out in the wild for a while and today we're calling it stable. It now runs on Bun, Deno, Cloudflare Workers, and Node with uWebSockets, not just plain Node. We also added a generic` Context` type for end-to-end type safety, sequential message processing, and structured transaction origins.


The wire protocol stays compatible in both directions. v3 providers can talk to v4 servers and v4 providers can talk to v3 servers, so you can roll this out gradually.


---


## What is Hocuspocus


Hocuspocus is an open source WebSocket backend for real-time collaboration, built on[Y.js](https://github.com/yjs/yjs) . It handles the hard parts: merging concurrent edits without conflicts, syncing awareness state (cursors, selections, presence), persistence, scaling with Redis, and offline-first sync.


It’s created and maintained by Tiptap, but it's not tied to the Tiptap editor. It works with any Y.js client: Tiptap, Slate, Quill, Monaco, ProseMirror, or your own custom Y.js setup. And it's not just for text editors. Y.js documents can hold arbitrary structured data, maps, arrays, nested objects, whatever you want. If your app has shared state that multiple users or processes need to read and write, Hocuspocus can sync it.


Install:


```text
npm install @hocuspocus/server @hocuspocus/provider
```


Docs:[tiptap.dev/docs/hocuspocus](https://tiptap.dev/docs/hocuspocus/getting-started/overview) .


---


## What's new in version 4


### Cross-runtime support


We swapped the Node-only` ws` library for[crossws](https://github.com/unjs/crossws) , a universal WebSocket adapter. Hocuspocus now runs on:


- Node.js (with` ws` or` uWebSockets.js` )
- Bun
- Deno
- Cloudflare Workers


The built-in` Server` class still works exactly as before for Node users. For other runtimes, use` Hocuspocus` directly with` handleConnection()` , which now accepts any` WebSocketLike` object and a web-standard` Request` . This means you can deploy collab at the edge, ship it on Workers, or run it inside Bun without forking the project.


### Generic Context type


Every core class and hook payload now takes a generic` Context` parameter. You define your context shape once and it flows through every hook with full type safety:


```text
interface MyContext {
userId  : string
permissions  : string[]
}


const   server = Server.configure<MyContext>({
async     onAuthenticate  (  { token }  )   {
return   {   userId  :   '123'  ,   permissions  : [  'read'  ,   'write'  ] }
},
async     onChange  (  { context }  )   {
// context.userId is typed as string
console  .log(context.userId)
},
})
```


The generic defaults to` any` , so existing code without explicit typing keeps working.


### Ordered message processing


Document updates are now processed sequentially in the order they arrive. Previously, concurrent messages could be reordered if async hooks were involved. There's now an internal queue per connection that guarantees CRDT updates are applied consistently. If you've ever seen weird state drift under load, this fixes it.


---


## CRDTs, Hocuspocus, and AI agents


CRDTs are a natural fit for agents writing into shared state alongside humans. There's no locking, no "who wins," and concurrent writes from multiple agents and a human user merge cleanly into the same document. You don't have to invent a coordination protocol.


This makes Y.js a solid substrate for human-in-the-loop workflows. The human and the agent are just two writers on the same doc. The human can keep typing while the agent is working, the agent can react to what the human just wrote, and nothing locks up waiting for the other side to finish.


A few things that come out of this for free:


- Multiple agents can edit the same Y.js document concurrently without stomping on each other or a human collaborator.
- Y.js gives you a deterministic update log, which is useful for agent observability. You can see exactly what an agent changed and when.
- Hocuspocus hooks (` onChange` ,` onStoreDocument` ,` onAwarenessUpdate` ) let agents react to human edits in real time, and let you react to agent edits the same way.
- If you want agents to propose edits rather than apply them directly, the Tiptap[Server AI Toolkit](https://tiptap.dev/docs/content-ai/capabilities/server-ai-toolkit/overview) supports tracked changes, so agent output surfaces as reviewable suggestions the human can accept or reject.


If you're building an agent that touches a document, sharing state through a CRDT instead of a database row will save you a lot of pain.


---


## Other things worth knowing


Transaction origins are now structured objects with a` source` field (` 'connection' | 'redis' | 'local'` ) and helper functions (` isTransactionOrigin()` ,` shouldSkipStoreHooks()` ). Hook payloads use web-standard` Request` and` Headers` instead of Node's` IncomingMessage` . Session awareness lets multiple providers share a WebSocket with the same document name. There's now application-level Ping/Pong (works in runtimes without WebSocket-level ping). Store hooks fire on every document change, not just WebSocket-originated ones, so you don't lose data on server-side edits. The SQLite extension switched from the unmaintained` sqlite3` to` better-sqlite3` (existing DB files are compatible). Node 22+ is required.


---


## Upgrading from v3


Because the wire protocol is backward compatible, you can upgrade servers and providers independently. Most breaking changes are at the TypeScript or hook-payload level, not behavioral.


```text
npm install @hocuspocus/server@^  4   @hocuspocus/provider@^  4
```


The full upgrade guide is in the docs:[tiptap.dev/docs/hocuspocus/getting-started/upgrade-guide](https://tiptap.dev/docs/hocuspocus/getting-started/upgrade) .


---


## Links


- Docs:[tiptap.dev/docs/hocuspocus](https://tiptap.dev/docs/hocuspocus/getting-started/overview)
- GitHub:[github.com/ueberdosis/hocuspocus](https://github.com/ueberdosis/hocuspocus)
- Release notes:[v4 on GitHub](https://github.com/ueberdosis/hocuspocus/releases)
