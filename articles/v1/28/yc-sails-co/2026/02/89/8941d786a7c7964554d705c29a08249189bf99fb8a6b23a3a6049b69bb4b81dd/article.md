---
schema_version: "1.0.0"
document_id: "8941d786a7c7964554d705c29a08249189bf99fb8a6b23a3a6049b69bb4b81dd"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/real-time-with-sse-the-sails-way/"
published_at: "2026-02-19T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:54:15.153981+00:00"
content_hash: "sha256:27c6ac2cdcbf79b7e9331ade2bd46d22676e5d7d58453279ea649ecb22eaff8d"
---

# Real-Time with SSE the Sails Way

When you hear “real-time” in Sails, you probably think[WebSockets](https://blog.sailscasts.com/understanding-sails-websockets) . That’s fair — Sails has had first-class WebSocket support through[sails-hook-sockets](https://github.com/balderdashy/sails-hook-sockets) since the early days. Subscribe to a model, get notified when it changes. It’s one of Sails’ superpowers.


But WebSockets aren’t the only way to do real-time. And for a whole class of problems — deployment logs, progress updates, live tails, status streams — there’s a simpler, older technology that’s a better fit.


This post is about[Server-Sent Events](https://en.wikipedia.org/wiki/Server-sent_events) . What they are, how they compare to[WebSockets](https://en.wikipedia.org/wiki/WebSocket) and[polling](https://en.wikipedia.org/wiki/Polling_(computer_science)) , and how I built a[Sails hook](https://sailsjs.com/documentation/concepts/extending-sails/hooks) in[Slipway](https://docs.sailscasts.com/slipway) that makes SSE feel as natural as` res.json()` .


If you’re not familiar with it,[Slipway](https://docs.sailscasts.com/slipway) is an open-source, self-hosted deployment platform I’m building specifically for Sails.js and[The Boring JavaScript Stack](https://docs.sailscasts.com/boring-stack) . Think of it as your own Heroku/Render/Railway — deploy with` git push` or` slipway slide` , stream build logs in real-time, manage databases, monitor containers, and run a production REPL against your live app. All on your own VPS. It’s the kind of platform where real-time streaming isn’t a nice-to-have — it’s core to the experience.


## What are Server-Sent Events?


[Server-Sent Events](https://en.wikipedia.org/wiki/Server-sent_events) (SSE) is a web standard —[part of the HTML spec](https://html.spec.whatwg.org/multipage/server-sent-events.html) — that lets a server push data to the browser over a plain[HTTP](https://en.wikipedia.org/wiki/HTTP) connection. Unlike[WebSockets](https://en.wikipedia.org/wiki/WebSocket) , the communication is **one-way** : the server talks, the browser listens.


Think of it like a radio broadcast. You tune in (open a connection), the station sends audio (data events), and if you lose signal (connection drops), your radio re-tunes automatically. You don’t talk back to the station — you just listen.


On the wire, SSE is surprisingly simple. It’s just an HTTP response with` Content-Type: text/event-stream` that never closes. The server writes lines in a specific text format:


```text
data: {"status": "building"}


data: {"status": "deploying"}


data: {"status": "running"}
```


Each` data:` line is an event. The double newline separates events. That’s it — no binary framing, no[handshake upgrade](https://en.wikipedia.org/wiki/WebSocket#Protocol_handshake) , no protocol negotiation. Just text over HTTP.


The browser has a built-in[EventSource](https://developer.mozilla.org/en-US/docs/Web/API/EventSource) API to consume these streams:


```text
const   es   =   new   EventSource  (  '/api/v1/deployments/123/stream'  )


es.  onmessage   =   (  event  )   =>   {
const   data   =   JSON  .  parse  (event.data)
console.  log  (data.status)   // "building", "deploying", "running"
}
```


` EventSource` handles connecting, parsing, and — crucially — **automatic reconnection** . If the connection drops, the browser reconnects on its own. No retry logic, no exponential backoff, no code at all. It just works.


## SSE vs WebSockets vs Polling — when to use what


All three solve the “show live data” problem, but they make very different tradeoffs. If you’re building a Sails app and wondering which one to reach for, here’s how I think about it:


## [Polling](https://en.wikipedia.org/wiki/Polling_(computer_science))


The client asks “anything new?” on a timer.


**Reach for polling when:** You need something quick, the data changes infrequently, or you’re prototyping. Polling is the simplest thing that works — no persistent connections, no special headers, fully[stateless](https://en.wikipedia.org/wiki/Stateless_protocol) on the server. If your data changes once a minute and you poll every 30 seconds, the overhead is negligible and the simplicity is worth it.


**Avoid when:** Data changes frequently, you have many concurrent viewers, or latency matters. With a 2-second poll interval, every update has up to 2 seconds of built-in delay — and every open tab is making independent requests even when nothing has changed.


## [Server-Sent Events](https://en.wikipedia.org/wiki/Server-sent_events)


The server holds a connection open and pushes data when it’s ready.


**Reach for SSE when:** Data flows in **one direction** — server to client. Logs, status updates, progress bars, notification feeds, live dashboards, deployment output, activity streams. Anywhere the client just needs to listen.


**Avoid when:** The client needs to send data back over the same connection (use WebSockets), or you need binary data (SSE is text-only — though you can Base64-encode if you really need to).


**What makes SSE great:**


- **Built-in reconnection** — The browser’s` EventSource` API handles it. You don’t write a single line of retry logic.
- **HTTP-native** — SSE works through every[proxy](https://en.wikipedia.org/wiki/Proxy_server) ,[CDN](https://en.wikipedia.org/wiki/Content_delivery_network) , and[load balancer](https://en.wikipedia.org/wiki/Load_balancing_(computing)) . It’s just a long-lived HTTP response. No upgrade handshake, no special infrastructure.
- **No client library needed** —` EventSource` is built into[every modern browser](https://caniuse.com/eventsource) . No npm install, no bundle size.
- **Simple server code** — It’s a regular HTTP endpoint that writes to` res` . In Sails, that means a regular action.


## [WebSockets](https://en.wikipedia.org/wiki/WebSocket)


A persistent,[full-duplex](https://en.wikipedia.org/wiki/Duplex_(telecommunications)#Full_duplex) connection where both sides can send data anytime.


**Reach for WebSockets when:** Communication is **bidirectional** . Chat, collaborative editing, multiplayer games, interactive terminals — anywhere the client and server need to talk back and forth in real-time.


**Avoid when:** You only need server-to-client streaming. WebSockets require a[protocol upgrade handshake](https://en.wikipedia.org/wiki/WebSocket#Protocol_handshake) , which some proxies and[firewalls](https://en.wikipedia.org/wiki/Firewall_(computing)) don’t support. They also require you to manage connection lifecycle — reconnection, heartbeats, room subscriptions — yourself, or use a framework like[sails-hook-sockets](https://sailsjs.com/documentation/concepts/realtime) that handles it for you.


In Sails, WebSockets are the right choice when you’re using[resourceful pub-sub](https://sailsjs.com/documentation/concepts/realtime/multi-server-environments) — subscribing to model changes, joining rooms, broadcasting to other users. That’s what` sails-hook-sockets` was built for, and it’s excellent at it.


But for “the server is producing data and the client is consuming it” — which describes every streaming endpoint in[Slipway](https://docs.sailscasts.com/slipway) — SSE is the simpler, more robust choice.


Here’s the summary:


Polling SSE WebSockets


Direction Client → Server (repeated) Server → Client Both directions


Connection New HTTP request each time Single persistent HTTP Upgraded TCP connection


Reconnection You build it Built-in (` EventSource` ) You build it


Proxy support Works everywhere Works everywhere Needs upgrade support


Data format Any Text (usually JSON) Text or binary


Browser API` fetch` /` XMLHttpRequest`` EventSource`` WebSocket`


Best for Infrequent changes, prototypes Logs, status, feeds, progress Chat, collaboration, games


Sails support Actions (built-in) **` res.sse()` hook**` sails-hook-sockets`


## My journey in Slipway


With that context, here’s how I evolved real-time in[Slipway](https://docs.sailscasts.com/slipway) through three stages — and what each one taught me.


### Stage 1: Polling — the one everyone starts with


My first deployment tracking looked like this on the client:


```text
const   pollInterval   =   setInterval  (  async   ()   =>   {
const   res   =   await   fetch  (  `/api/v1/deployments/${  id  }`  )
const   deployment   =   await   res.  json  ()
updateStatus  (deployment.status)
if   ([  'running'  ,   'failed'  ].  includes  (deployment.status)) {
clearInterval  (pollInterval)
}
},   2000  )
```


On the server, a standard[Sails action](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2) . Return the deployment, done.


It worked. But as I added more real-time features — live container logs, active deployment banners, CLI authentication, system update progress — every one added another` setInterval` per connected client. Ten developers watching the same deployment? That’s 10 database queries every 2 seconds, even when nothing has changed.


### Stage 2: SSE — the right tool, but messy in Sails


I moved to SSE. The client got beautifully simple:


```text
const   es   =   new   EventSource  (  `/api/v1/deployments/${  id  }/stream`  )
es.  onmessage   =   (  event  )   =>   {
const   data   =   JSON  .  parse  (event.data)
updateStatus  (data.status)
}
```


But the server side? Every SSE endpoint in Sails needs the same ceremony:


```text
fn  :   async   function   ({   id   }) {
const   req   =   this  .req
const   res   =   this  .res


// 1. Set SSE headers
res.  writeHead  (  200  , {
'Content-Type'  :   'text/event-stream'  ,
'Cache-Control'  :   'no-cache, no-transform'  ,
'Connection'  :   'keep-alive'  ,
'X-Accel-Buffering'  :   'no'  ,
'Content-Encoding'  :   'identity'
})


// 2. Safe write wrapper (client can disconnect at any time)
let   cleanedUp   =   false
function   cleanup  () {
if   (cleanedUp)   return
cleanedUp   =   true
}
function   safeWrite  (  data  ) {
if   (cleanedUp   ||   res.writableEnded   ||   res.destroyed)   return   false
try   {
res.  write  (  `data: ${  JSON  .  stringify  (  data  )  }  \n\n  `  )
return   true
}   catch   (err) {
cleanup  ()
return   false
}
}


// 3. Your actual logic (finally!)
safeWrite  ({ status: deployment.status })
const   interval   =   setInterval  (  async   ()   =>   {
const   current   =   await   Deployment.  findOne  (id)
if   (current.status   !==   lastStatus) {
safeWrite  ({ status: current.status })
}
},   500  )


// 4. Cleanup on disconnect
req.  on  (  'close'  , ()   =>   {   clearInterval  (interval);   cleanup  () })
res.  on  (  'error'  , ()   =>   {   cleanup  () })


// 5. Keep Sails alive (without this, Sails calls res.end() prematurely)
return   new   Promise  ((  resolve  )   =>   {
req.  on  (  'close'  , ()   =>   resolve  ())
})
}
```


Five separate concerns tangled together: headers, safe writes, your logic, cleanup, and the Sails lifecycle hack. And I had **seven** of these endpoints.


The three docker log endpoints were the worst — they piped` docker logs --follow` through a child process, adding` safeWrite` wrappers,` cleanedUp` flags, and` res.on('error')` handlers to kill the docker process on disconnect. Each was 150+ lines, about 60% identical boilerplate.


The code worked. It was correct. But it felt wrong every time I touched it.


### Stage 3: The Sails hook —` res.sse()`


Here’s the thing about[Sails hooks](https://sailsjs.com/documentation/concepts/extending-sails/hooks) : they can extend the response object. That’s how Sails itself gives you` res.json()` ,` res.view()` ,` res.redirect()` . These are all response methods, added by hooks.


SSE is a response type. So` res.sse()` is the natural API.


I built a hook at[api/hooks/sse/index.js](https://github.com/sailscastshq/slipway/blob/main/api/hooks/sse/index.js) that adds` res.sse()` to every request. Call it once, get back a stream object:


```text
fn  :   async   function   ({   id   }) {
const   deployment   =   await   Deployment.  findOne  (id)
if   (  !  deployment)   throw   'notFound'


const   stream   =   this  .res.  sse  ()


stream.  send  ({ status: deployment.status })


let   lastStatus   =   deployment.status
const   interval   =   setInterval  (  async   ()   =>   {
const   current   =   await   Deployment.  findOne  (id)
if   (current.status   !==   lastStatus) {
lastStatus   =   current.status
stream.  send  ({ status: current.status })
}
if   ([  'running'  ,   'failed'  ].  includes  (current.status)) {
clearInterval  (interval)
stream.  close  ()
}
},   500  )


stream.  onClose  (()   =>   clearInterval  (interval))
return   stream.  wait  ()
}
```


Read that again. There’s no` writeHead` . No` safeWrite` . No cleanup flag. No` req.on('close')` . No Promise wrapper.


All five concerns from Stage 2 are handled by the hook:


Concern Before (manual) After (hook)


SSE headers` res.writeHead(200, { ... })` Handled inside` res.sse()`


Safe writes` safeWrite()` +` cleanedUp` flag` stream.send()` returns` false` if closed


Cleanup` req.on('close')` +` res.on('error')`` stream.onClose(fn)` fires once on any trigger


Sails lifecycle` return new Promise(resolve => ...)`` return stream.wait()`


JSON + SSE framing` JSON.stringify()` +` data: ...\\n\\n` Handled inside` stream.send()`


The full stream API:


- **` stream.send(data, event?)`** — JSON-encode and write an SSE frame. Returns` false` if closed.
- **` stream.heartbeat()`** — Comment-only keepalive for proxy idle timeouts.
- **` stream.close()`** — End the stream, fire cleanup callbacks, resolve` wait()` .
- **` stream.onClose(fn)`** — Register cleanup. Runs exactly once on disconnect, error, or explicit close.
- **` stream.wait()`** — Promise that keeps the Sails action alive until the stream closes.
- **` stream.closed`** — Boolean getter.


### The docker log endpoints — the biggest win


Streaming docker container logs to the browser now looks like this:


```text
const   {   spawn   }   =   require  (  'child_process'  )


fn  :   async   function   ({   serviceId  ,   tail   }) {
// ... authorization checks ...


const   stream   =   this  .res.  sse  ()
stream.  send  ({ connected:   true  , container: service.containerName })


const   dockerPath   =   sails.config.docker?.binaryPath   ||   'docker'
const   docker   =   spawn  (dockerPath, [
'logs'  ,   '--follow'  ,   '--tail'  ,   String  (tail),
'--timestamps'  , service.containerName
])


function   onData  (  data  ) {
for   (  const   line   of   data.  toString  ().  split  (  '  \n  '  )) {
if   (line.  length   >   0  ) stream.  send  ({ log: line })
}
}


docker.stdout.  on  (  'data'  , onData)
docker.stderr.  on  (  'data'  , onData)


docker.  on  (  'error'  , (  err  )   =>   {
stream.  send  ({ error: err.message })
stream.  close  ()
})


docker.  on  (  'close'  , ()   =>   {
stream.  send  ({ closed:   true   })
stream.  close  ()
})


stream.  onClose  (()   =>   docker.  kill  ())
return   stream.  wait  ()
}
```


That` stream.onClose(() => docker.kill())` line replaces three separate event handlers and a boolean flag from Stage 2. It handles client disconnect, response error, and docker process exit — all through one callback that runs exactly once.


## Graceful degradation


I designed the hook to never be the thing that crashes your app, even when things go sideways.


**Call` res.sse()` twice? Same stream.** The hook caches the stream per-request. If your code path calls it again — through a helper, a race condition, whatever — it returns the same instance. No double` writeHead` , no crash.


**Headers already sent? No-op stream.** If middleware already committed headers before your action runs,` res.sse()` returns a stream where every method is safe but does nothing.` send()` returns` false` ,` wait()` resolves immediately. Your action runs to completion without throwing.


**` onClose` after disconnect? Fires immediately.** If you register a cleanup callback after the stream has already closed (fast disconnect race condition), it fires right away instead of being silently dropped. No leaked intervals, no orphaned child processes.


**The page loads without SSE.** This is maybe the most important one. In[Slipway](https://docs.sailscasts.com/slipway) , pages are rendered by[Inertia.js](https://inertiajs.com/) through[The Boring JavaScript Stack](https://docs.sailscasts.com/boring-stack) — a regular HTTP request. All the app info — status, environment variables, deployment history, controls — loads via Inertia. SSE only powers the live log tail.


If` EventSource` can’t connect — network blip, proxy issue, browser quirk — the page still shows everything. The log section shows a “Connection lost” indicator and auto-retries. Logs are a[progressive enhancement](https://en.wikipedia.org/wiki/Progressive_enhancement) , not a hard dependency. Your page is never broken because SSE is down.


## The pub/sub layer


The hook also includes channel-based pub/sub for broadcasting to multiple clients:


```text
// In a controller — subscribe a client to a channel
fn  :   async   function   ({   id   }) {
return   sails.sse.  subscribe  (  this  .req,   this  .res,   `deploy:${  id  }`  )
}


// Anywhere in your app — broadcast to all subscribers
sails.sse.  publish  (  `deploy:${  id  }`  , { status:   'building'   })
```


In-memory` Map<string, Set<SseStream>>` , auto-cleans empty channels, auto-unsubscribes on disconnect. No Redis, no external dependencies.


I’m not using pub/sub in Slipway yet — every current endpoint uses Level 1 (` res.sse()` ) directly because each stream has its own logic.


But the use case is clear: imagine multiple team members watching the same deployment.


Right now, each viewer runs their own database polling interval. With pub/sub, one process publishes status changes to a channel and all subscribers receive them instantly — no duplicated work, no wasted queries. It’s ready for when Slipway gets collaborative features.


## The client side:` useEventSource`


The server hook was only half the story. On the client, every Vue component that consumed SSE repeated the same boilerplate too.


By the way, if you’re not familiar, Slipway’s frontend is built with[Vue 3](https://vuejs.org/) through[The Boring JavaScript Stack](https://docs.sailscasts.com/boring-stack) — which pairs Sails with your choice of Vue, React, or Svelte via[Inertia.js](https://inertiajs.com/) .


So when I say “composable” here, I’m talking about[Vue composables](https://vuejs.org/guide/reusability/composables) — reusable reactive logic that you can share across components.


```text
// Repeated in 9 different Vue files...
const   logsConnected   =   ref  (  false  )
const   logsError   =   ref  (  null  )
let   eventSource   =   null


function   connect  () {
eventSource   =   new   EventSource  (url)
eventSource.  onopen   =   ()   =>   { logsConnected.value   =   true   }
eventSource.  onmessage   =   (  event  )   =>   {
try   {
const   data   =   JSON  .  parse  (event.data)
// ... handle data
}   catch   (e) {   /* ignore */   }
}
eventSource.  onerror   =   ()   =>   {
logsConnected.value   =   false
// ... reconnect logic
}
}


onUnmounted  (()   =>   {
if   (eventSource) eventSource.  close  ()
})
```


Connection state, JSON parsing, error handling, cleanup on unmount — all duplicated across 9 components. So I built a Vue composable to match:


```text
import   { useEventSource }   from   '@/composables/sse'


const   {   connected  ,   error  ,   close  ,   connect   }   =   useEventSource  (
`/api/v1/services/${  id  }/logs/stream?tail=200`  ,
{
immediate:   false  ,
onMessage  (  data  ) {
if   (data.log) logLines.value.  push  (data.log)
}
}
)
```


That’s it.` connected` and` error` are reactive refs.` close()` and` connect()` give you manual control. The composable auto-cleans up on unmount and auto-reconnects by default.


The smart defaults:


- **` immediate: true`** — Connects on creation (override with` false` for on-demand streams like log panels)
- **` autoReconnect: true`** — Reconnects after 3 seconds on error (override with` false` for one-shot streams like system updates where disconnection means the server is restarting)
- **` onMessage(data)`** — Called with parsed JSON for each event — no more` try { JSON.parse(event.data) }` wrappers
- **Auto-cleanup** —` onUnmounted` closes the connection automatically. No more` if (eventSource) eventSource.close()` in every component


The before/after for a log streaming component:


```text
// Before: ~40 lines of EventSource boilerplate
const   logsConnected   =   ref  (  false  )
const   logsError   =   ref  (  null  )
let   logsEventSource   =   null


function   connectLogs  () {
if   (logsEventSource)   return
logsEventSource   =   new   EventSource  (url)
logsEventSource.  onopen   =   ()   =>   { logsConnected.value   =   true   }
logsEventSource.  onmessage   =   (  event  )   =>   {   /* parse, push, scroll */   }
logsEventSource.  onerror   =   ()   =>   {   /* disconnect, reconnect */   }
}
function   disconnectLogs  () {
if   (logsEventSource) { logsEventSource.  close  (); logsEventSource   =   null   }
logsConnected.value   =   false
}
onUnmounted  (()   =>   disconnectLogs  ())


// After: 1 composable call
const   {   connected  :   logsConnected  ,   error  :   logsError  ,   close  :   disconnectLogs  ,   connect  :   connectLogs   }   =
useEventSource  (url, {
immediate:   false  ,
onMessage  (  data  ) {   if   (data.log) logLines.value.  push  (data.log) }
})
```


The server hook and the client composable mirror each other. On the server,` res.sse()` returns a stream with` send()` ,` onClose()` ,` wait()` . On the client,` useEventSource()` returns refs with` connected` ,` error` ,` close()` ,` connect()` . Both handle lifecycle automatically. Both have smart defaults. Both degrade gracefully.


## The numbers


Metric Before After


Lines across 7 controllers ~780 ~450


Lines deleted — 329


Lines added — 109


Hook size — ~240 (write once)


Docker log endpoints ~155 lines each ~90 lines each


Boilerplate per endpoint ~60 lines 0


The real win isn’t the line count. It’s that you can now read any SSE controller and immediately see what it *does* — poll a deployment, stream docker logs, check CLI auth status — without wading through 50 lines of connection management.


## Use it in your Sails app


The hook lives in[api/hooks/sse/index.js](https://github.com/sailscastshq/slipway/blob/main/api/hooks/sse/index.js) inside[Slipway](https://github.com/sailscastshq/slipway) . If you’re building SSE endpoints in your own Sails app, drop the file into` api/hooks/sse/` and Sails auto-discovers it.


The pattern for any streaming endpoint becomes:


```text
module  .  exports   =   {
fn  :   async   function   () {
const   stream   =   this  .res.  sse  ()


// send data whenever you want
stream.  send  ({ hello:   'world'   })


// clean up when the client disconnects
stream.  onClose  (()   =>   {   /* stop intervals, kill processes */   })


// keep Sails alive
return   stream.  wait  ()
}
}
```


Three methods on the server. One composable on the client. No ceremony on either side.


Sails has always had great real-time support through WebSockets. Now it has a clean answer for the other kind of real-time too — the kind where the server just needs to talk.
