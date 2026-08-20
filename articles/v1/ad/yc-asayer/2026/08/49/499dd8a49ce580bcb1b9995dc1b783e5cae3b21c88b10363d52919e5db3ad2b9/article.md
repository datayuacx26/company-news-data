---
schema_version: "1.0.0"
document_id: "499dd8a49ce580bcb1b9995dc1b783e5cae3b21c88b10363d52919e5db3ad2b9"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/browser-tab-sync-broadcastchannel/"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T18:30:11.746057+00:00"
fetched_at: "2026-08-04T18:43:13.169222+00:00"
content_hash: "sha256:a6a1931593760d4199656c73fbf010e20d2a0480a7d4c497f0c1408e6a2abb64"
---

# Browser Tab Synchronization with BroadcastChannel

The[BroadcastChannel API](https://developer.mozilla.org/en-US/docs/Web/API/Broadcast_Channel_API) is a native browser message bus that lets tabs, windows, iframes, and workers on the same origin talk to each other in real time — the purpose-built fix for multi-tab state drift, where a logout in one tab leaves the others still showing a logged-in UI. It’s a four-call API with no server, no handshake, and no dependencies. This article covers the minimal API, the action-typed message pattern that scales, framework integration with correct cleanup, the non-obvious gotchas, and current browser support with a runnable fallback.


## Key Takeaways


- The entire API is four calls —` new BroadcastChannel(name)` ,` postMessage(data)` ,` onmessage` , and` close()` — with no server, handshake, or configuration.
- Because` postMessage` uses the structured clone algorithm, you send objects, Maps, Sets, and Blobs directly — no` JSON.stringify` , no manual parsing on the other side.
- A tab never receives its own broadcasts: messages fire on every listening channel *except* the object that sent them, which stops the feedback loops you’d otherwise hand-guard against.
- BroadcastChannel is a pipe, not a bucket — it transports messages but stores nothing, so pair it with localStorage or IndexedDB for durability and to hydrate tabs opened after an event fired.
- BroadcastChannel is Baseline, widely available across Chrome, Firefox, Edge, Safari, Opera, and Samsung Internet since March 2022, with Safari support landing in 15.4; only Internet Explorer lacks it.


## What is multi-tab state drift?


Multi-tab state drift is the class of bug where two open tabs of the same app hold divergent state: you log out in one and the other still renders the dashboard, or you empty a cart in one and the other still shows items. Session replays of these flows regularly surface the visible symptom — a user submitting against a cart emptied elsewhere, or continuing in a tab that should have logged out — which is exactly the desync BroadcastChannel eliminates.


Developers have patched this with three inferior workarounds. The` localStorage`` storage` event fires across tabs but carries strings only, forcing serialize/parse on every message and awkward key juggling. Polling a server or` localStorage` on an interval is wasteful and laggy — you pay for checks that mostly find nothing.` SharedWorker` and WebSockets are real tools, but a socket round-trip through a server to sync two tabs on the same machine is heavy for a purely local problem.


Mechanism Payload type Persists? Network needed Best for


**BroadcastChannel** Structured clone (objects, Maps, Blobs) No No Local same-origin tab/worker sync


` storage` event Strings only Yes (localStorage) No Simple sync with built-in durability


SharedWorker Structured clone No No Shared computation/connection across tabs


WebSocket Strings/binary Server-side Yes Live server-pushed data across clients


## The BroadcastChannel API in four calls


The entire API is four calls, and any context that constructs a channel with the same name joins the same bus. You connect, listen, post, and close:


```text
const   channel   =   new   BroadcastChannel  (  "  app-sync  "  );


channel  .  onmessage   =   (  event  )   =>   {
console  .  log  (  "  Received:  "  ,   event  .  data  );
};


channel  .  postMessage  ({   hello  :   "  world  "   });


channel  .  close  ();   // on unmount / teardown
```


The payload is the key advantage.[Data sent through postMessage is serialized with the structured clone algorithm](https://developer.mozilla.org/en-US/docs/Web/API/BroadcastChannel/postMessage) , so you pass objects, arrays,` Map` ,` Set` , and` Blob` directly without stringifying — the receiver reads` event.data` as a live object. Symbols and` SharedArrayBuffer` are the notable exceptions that won’t clone. Channels are same-origin only, and reusing one channel instance per context matters: constructing a fresh` BroadcastChannel` on every send leaks listeners.


## How should you structure BroadcastChannel messages?


The pattern that scales is a discriminated message —` { type, payload }` — plus one listener that switches on` type` and applies each action to local state. This keeps every context speaking the same protocol; the API itself assigns no meaning to messages, so you define one.


```text
const   channel   =   new   BroadcastChannel  (  "  cart-sync  "  );
let   cart   =   [];


channel  .  onmessage   =   ({   data   })   =>   {
if (  data  .  type   ===   "  ADD_ITEM  "  )   cart  .  push  (  data  .  payload  );
else if (  data  .  type   ===   "  CLEAR  "  )   cart   =   [];
render  ();
};


function   addItem  (  item  ) {
cart  .  push  (  item  );         // update this tab immediately
render  ();
channel  .  postMessage  ({   type  :   "  ADD_ITEM  "  ,   payload  :   item   });
}
```


Note the sender updates its own state directly *before* broadcasting — because a tab doesn’t hear its own messages, you apply the local change inline and let the broadcast fan out to everyone else.


## Syncing auth, cart, and settings across tabs


The highest-value uses are session sync (a logout broadcast clears every tab), cart/settings/theme sync, and multi-tab form autofill. In React, create the channel inside` useEffect` and return a cleanup that calls` close()` ; skip it and every remount leaks a live listener.


```text
import   {   useEffect   }   from   "  react  "  ;


function   useAuthSync  (  onLogout  ) {
useEffect  (()   =>   {
const   channel   =   new   BroadcastChannel  (  "  auth  "  );
channel  .  onmessage   =   ({   data   })   =>   {
if (  data  .  type   ===   "  LOGOUT  "  )   onLogout  ();
};
return   ()   =>   channel  .  close  ();
}, [  onLogout  ]);
}


// on logout: clear local session, then
// new BroadcastChannel("auth").postMessage({ type: "LOGOUT" })
```


In Svelte 5 (runes), wrap the channel in a store. One caveat:` postMessage` needs a plain clone, not a reactive proxy, so pass the value through[$state.snapshot](https://svelte.dev/docs/svelte/$state) before broadcasting — a proxy would choke the structured clone step.


```text
// theme.svelte.js — Svelte 5 (runes)
export   class   ThemeStore   {
value   =   $state  (  "  light  "  );
#channel   =   new   BroadcastChannel  (  "  theme  "  );
constructor  () {
this  .  #channel  .  onmessage   =   ({   data   })   =>   {   this  .  value   =   data  ; };
}
set  (  next  ) {
this  .  value   =   next  ;
this  .  #channel  .  postMessage  (  $state  .  snapshot  (  next  ));
}
}
```


## The gotchas most tutorials skip


Four behaviors trip up implementations, and they’re where most write-ups go quiet:


- **A tab cannot hear its own broadcasts.**[Messages fire at every channel listening except the object that sent the message](https://developer.mozilla.org/en-US/docs/Web/API/BroadcastChannel) — this is spec behavior, and it’s helpful: it prevents the infinite feedback loops you’d otherwise guard against with a self-ID check. Apply the sender’s own change inline.
- **“Same-origin” is really “same storage partition.”** Storage partitions by top-level site, so a cross-site iframe won’t share a channel with a top-level page even at the same origin, and channels never bridge different subdomains.
- **It’s a pipe, not a bucket.** BroadcastChannel transports messages but stores nothing. Pair it with localStorage or IndexedDB both for durability and to hydrate tabs that open *after* an event fired — a late tab that never heard the broadcast reads persisted state on mount.
- **Always` close()` on unmount.** Closing disconnects the object and frees it for garbage collection; a channel created per component and never closed leaks a listener on every remount.


## Browser support and a feature-detected fallback


BroadcastChannel is Baseline and widely available since March 2022, and current Safari fully supports it — the “doesn’t work in WebKit” claim predates Safari 15.4. Per[caniuse](https://caniuse.com/broadcastchannel) and the[Chrome for Developers blog](https://developer.chrome.com/blog/broadcastchannel) , the floors are Chrome 54+, Edge 79+, Firefox 38+, Safari 15.4+ (macOS and iOS), Opera 41+, and Samsung Internet 7.2+; only Internet Explorer never shipped it.


For pre-2022 browsers, feature-detect with` 'BroadcastChannel' in window` and fall back to a` storage` -event shim exposing the same interface:


```text
function   createChannel  (  name  ) {
if (  "  BroadcastChannel  "   in   window  )   return   new   BroadcastChannel  (  name  );
// Fallback: localStorage "storage" event (strings only)
return   {
postMessage  :   (  data  )   =>
localStorage  .  setItem  (  name  ,   JSON  .  stringify  ({   data  ,   t  :   Date  .  now  ()   })),
set   onmessage  (  fn  )   {
window  .  addEventListener  (  "  storage  "  ,   (  e  )   =>   {
if   (  e  .  key   ===   name   &&   e  .  newValue  )   fn  ({   data  :   JSON  .  parse  (  e  .  newValue  ).  data   });
});
},
close  ()   {},
};
}
```


Reach for the native API first and treat the shim as insurance. On every current browser, BroadcastChannel is already there — pick one channel name, standardize on` { type, payload }` , persist what must survive a full close, and the stale-tab bug hiding in your app disappears.


## FAQs


Does BroadcastChannel work across different subdomains?


No. BroadcastChannel is scoped to the same storage partition, not just the same origin, and it never bridges different subdomains. A page on app.example.com and a page on account.example.com cannot share a channel. Even at the same origin, a cross-site iframe will not share a channel with the top-level page because storage partitions by top-level site. For cross-subdomain sync, use a server or a WebSocket.


What is the difference between BroadcastChannel and the storage event for cross-tab sync?


BroadcastChannel transports structured-clone payloads such as objects, Maps, Sets, and Blobs directly, but stores nothing. The localStorage storage event carries strings only, forcing you to serialize and parse every message, but it writes durable state as a side effect. Use BroadcastChannel for rich, message-driven sync and pair it with localStorage or IndexedDB when you also need persistence or need to hydrate tabs opened after an event fired.


Does the tab that sends a BroadcastChannel message receive its own message?


No. Per spec, a message fires at every BroadcastChannel object listening to the channel except the object that sent it. A tab never hears its own broadcasts, which prevents the infinite feedback loops you would otherwise guard against with a self-ID check. Because of this, apply the sender's own state change inline before calling postMessage, then let the broadcast fan out to every other context.


What happens to BroadcastChannel messages if all tabs are closed?


They are lost. BroadcastChannel is a pipe, not a bucket: it transports messages but stores nothing, so any state broadcast while no other tab was listening is gone once all instances close. A tab opened after an event fired never hears the original message. To survive this, persist state to localStorage or IndexedDB and hydrate late-opening tabs from that storage on mount rather than relying on the channel alone.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
