---
schema_version: "1.0.0"
document_id: "286f0dc8ff1e2204b3ab83cf2bf7f3b810418661bdba1a803b6f41061e2cbc4e"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/offline-form-submission-background-sync/"
published_at: "2026-07-19T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:ee6ed4c990611e76d2b6f09e1be879e5fa7d5f5ea1c6b34d84769669904096d7"
---

# Offline Form Submission with Background Sync

To make a form submission survive an offline or flaky connection, write the submission to IndexedDB *before* you touch the network, register a Background Sync tag, and let the service worker replay the queued request from its` sync` event when connectivity returns — then add an` online` -event fallback for the browsers that don’t support Background Sync. That last clause is not optional: the Background Sync API ships only in Chromium browsers, so the IndexedDB queue plus an` online` -event listener is the baseline that works everywhere, and Background Sync is the enhancement layered on top.


This article carries one worked example — a contact/order form — through the full pattern: durable queue, sync registration, service-worker replay, the everywhere fallback, duplicate-submission prevention, and the confirmation problem nobody warns you about. It assumes you’ve already registered a service worker and are comfortable with promises and` fetch` . Generic caching (cache-first, network-first) is a prerequisite, not the subject here — see[MDN’s caching strategies](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching) and the[Workbox strategy modules](https://developer.chrome.com/docs/workbox/modules/workbox-strategies) for that.


## Key Takeaways


- Write the submission to IndexedDB with a client-generated request ID, a` queuedAt` timestamp, and a retry count *before* the network call, so an interrupted POST is already durable rather than lost.
- One-off Background Sync (` SyncManager` ) is supported only in Chromium browsers — Chrome, Edge, Opera, and Samsung Internet — and is absent in Firefox, Safari, and iOS, which is why an` online` -event fallback is mandatory in 2026.
- In the service worker’s` sync` handler, POST each queued item, delete it on success, and *throw* on failure — throwing tells the browser to keep the registration and retry with browser-managed exponential backoff.
- Send the request ID as an` Idempotency-Key` header so a submission the server already accepted is deduplicated when the replay arrives, preventing duplicate orders.
- Workbox’s` BackgroundSyncPlugin` queues and replays for you, but it retries only true network failures — a 4xx or 5xx response is treated as delivered and will not be replayed.


## Why a plain fetch on submit fails offline


A bare` fetch` on form submit fails silently when the device is offline: the promise rejects, the request never reaches the server, and unless you wrote explicit handling, the user gets no signal that anything went wrong. Service workers solve asset caching, but they don’t automatically retry the *new data requests* a form makes — a failed POST is simply gone.


A silently failed offline POST is the canonical “user believes they succeeded; the data never arrived” bug. It’s also invisible to backend monitoring, because the request never reached the backend — there is nothing to log. Session replay is the technique that closes that visibility gap: a replay reconstructs the client-side reality — the tap on Submit, the optimistic “Thanks!” screen, the navigation away — which you can then correlate against whether a server record ever appeared. That correlation is exactly how you catch the trust gap this whole pattern exists to prevent.


“Submit and forget” should feel instant to the user and durable to you: the submission is captured locally the moment they tap Submit, and delivery is the system’s problem, not theirs.


## Step 1: Queue the submission in IndexedDB before the network


Persist the submission to IndexedDB first, then attempt delivery. Storing before any network call means a failed, interrupted, or offline POST is already durable — you replay from the store, never from volatile page state. Attach three fields to every queued item: a client-generated request ID (for deduplication later), a` queuedAt` timestamp, and a` retryCount` .


```text
// db.js — a thin promise wrapper around IndexedDB
const   DB_NAME   =   '  outbox  '  ;
const   STORE   =   '  submissions  '  ;


function   openDB  () {
return   new   Promise  ((  resolve  ,   reject  )   =>   {
const   req   =   indexedDB  .  open  (  DB_NAME  ,   1  );
req  .  onupgradeneeded   =   ()   =>   {
req  .  result  .  createObjectStore  (  STORE  , {   keyPath  :   '  id  '   });
};
req  .  onsuccess   =   ()   =>   resolve  (  req  .  result  );
req  .  onerror   =   ()   =>   reject  (  req  .  error  );
});
}


export   async   function   enqueue  (  payload  ) {
const   db   =   await   openDB  ();
const   item   =   {
id  :   crypto  .  randomUUID  (),       // request ID for idempotency
payload  ,
queuedAt  :   Date  .  now  (),
retryCount  :   0  ,
};
return   new   Promise  ((  resolve  ,   reject  )   =>   {
const   tx   =   db  .  transaction  (  STORE  ,   '  readwrite  '  );
tx  .  objectStore  (  STORE  ).  put  (  item  );
tx  .  oncomplete   =   ()   =>   resolve  (  item  );
tx  .  onerror   =   ()   =>   reject  (  tx  .  error  );
});
}
```


` crypto.randomUUID()` is available in all modern browsers and in service-worker scope, per[MDN’s Crypto.randomUUID() reference](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/randomUUID) . The form handler calls` enqueue` , shows an optimistic “Queued” state, then asks for a sync.


## Step 2: Register Background Sync, then handle the sync event


Background Sync lets you register a named tag from the page; the browser fires a` sync` event in your service worker when it judges connectivity to be back, and it can deliver that event even after the user has navigated away or closed the tab. That deferred-delivery-after-navigation behavior is what makes it stronger than an` online` listener, which only fires while the page is open.


Feature-detect both` serviceWorker` and` SyncManager` before relying on it, and fall back immediately if either is missing:


```text
// form handler, after enqueue()
async   function   requestSync  () {
if (  '  serviceWorker  '   in   navigator   &&   '  SyncManager  '   in   window  ) {
const   reg   =   await   navigator  .  serviceWorker  .  ready  ;
try   {
await   reg  .  sync  .  register  (  '  sync-forms  '  );
return  ;
}   catch   {
// registration failed — fall through to the fallback
}
}
flushQueue  ();   // the everywhere fallback (Step 3)
}
```


The` register('sync-forms')` call and the` SyncManager` interface are documented in[MDN’s Background Synchronization API](https://developer.mozilla.org/en-US/docs/Web/API/Background_Synchronization_API) . In the service worker, match the tag, wrap the work in` event.waitUntil()` so the worker stays alive, POST each item, delete on success, and **throw on failure** so the browser keeps the registration and retries:


```text
// service-worker.js
self  .  addEventListener  (  '  sync  '  , (  event  )   =>   {
if (  event  .  tag   ===   '  sync-forms  '  ) {
event  .  waitUntil  (  replayQueue  (  event  ));
}
});


async   function   replayQueue  (  event  ) {
const   items   =   await   getAll  ();           // read from IndexedDB
for (  const   item   of   items  ) {
const   res   =   await   fetch  (  '  /api/orders  '  , {
method  :   '  POST  '  ,
headers  :   {
'  Content-Type  '  :   '  application/json  '  ,
'  Idempotency-Key  '  :   item  .  id  ,        // dedupe on the server
},
body  :   JSON  .  stringify  (  item  .  payload  ),
});
if (  res  .  ok  ) {
await   remove  (  item  .  id  );               // delete on success
} else if (  res  .  status   >=   500  ) {
if (  event  .  lastChance  )   await   notifyFailure  (  item  );
throw   new   Error  (  '  server error, retry  '  );     // keep the registration
}
// 4xx: the payload is bad — don't retry blindly; surface it instead
}
}
```


Throwing inside` waitUntil` is the signal that keeps the sync pending. Browsers that support the API replay failed requests on your behalf at an interval managed by the browser,[likely using exponential backoff between replay attempts](https://developer.chrome.com/docs/workbox/modules/workbox-background-sync) . The number of attempts and the exact interval are browser-managed and not contractually documented, so don’t hard-code an assumption like “three retries.” Instead, check` event.lastChance` — documented in[MDN’s SyncEvent reference](https://developer.mozilla.org/en-US/docs/Web/API/SyncEvent/lastChance) — to detect the final attempt and tell the user the submission ultimately failed rather than letting it vanish.


## Step 3: The fallback that works everywhere


Because Background Sync is Chromium-only, the` online` -event fallback isn’t a footnote — it’s the baseline every browser runs. Two triggers cover the non-Chromium path: re-flush the queue whenever the` online` event fires, and flush once on every page load to catch submissions queued in a previous session.


```text
// runs on the page, everywhere
export   async   function   flushQueue  () {
if (  !  navigator  .  onLine  )   return  ;
const   items   =   await   getAll  ();
for (  const   item   of   items  ) {
try   {
const   res   =   await   fetch  (  '  /api/orders  '  , {
method  :   '  POST  '  ,
headers  :   {
'  Content-Type  '  :   '  application/json  '  ,
'  Idempotency-Key  '  :   item  .  id  ,
},
body  :   JSON  .  stringify  (  item  .  payload  ),
});
if (  res  .  ok  )   await   remove  (  item  .  id  );
}   catch   {
await   bumpRetryCount  (  item  .  id  );     // stays queued for the next trigger
}
}
}


window  .  addEventListener  (  '  online  '  ,   flushQueue  );
window  .  addEventListener  (  '  load  '  ,   flushQueue  );
```


The fallback’s honest limitation: it only runs while a page of your origin is open, since it depends on page-level events. It cannot wake a closed tab the way Background Sync can. That’s the precise tradeoff — universal reach, weaker delivery guarantees — and it’s why you register Background Sync *first* and fall back *second* .


## Browser support for Background Sync in 2026


As of June 2026, one-off Background Sync (` SyncManager` ) is a Chromium-only API. MDN classifies it as[“Limited availability”](https://developer.mozilla.org/en-US/docs/Web/API/SyncManager) — not Baseline, because it doesn’t work in some of the most widely used browsers. The[caniuse data](https://caniuse.com/background-sync) confirms the split below.


Browser One-off Background Sync (` SyncManager` )


Chrome ✅ Supported


Edge (Chromium) ✅ Supported


Opera ✅ Supported


Samsung Internet ✅ Supported


Firefox ❌ Not supported


Safari (macOS) ❌ Not supported


Safari (iOS) ❌ Not supported


Android WebView ❌ Not supported


Two gotchas worth flagging. Microsoft Edge gained support only after moving to Chromium, so any 2019-era claim that “Edge doesn’t support it” is now wrong. And Android WebView — the component native apps embed for in-app browsing — does not expose` SyncManager` , so wrapping a PWA in a WebView shell loses the native sync queue and falls back to the` online` -event path. Note that this is the *one-off* Background Sync API;[Periodic Background Sync](https://developer.mozilla.org/en-US/docs/Web/API/Web_Periodic_Background_Synchronization_API) is a separate, experimental API for recurring updates — don’t conflate the two.


## Idempotency: stop replays from creating duplicate orders


Any retried POST risks a duplicate: the first attempt may have reached the server and committed before the connection dropped, so the response never came back and your client replays a request the server already honored. Prevent this with a client-generated request ID sent as an` Idempotency-Key` header — the same` item.id` you stored at enqueue time. The server keys on it and returns the original result for a repeat instead of creating a second record.


` Idempotency-Key` is an established API convention popularized by Stripe and PayPal, and it’s the subject of an IETF Internet-Draft,[draft-ietf-httpapi-idempotency-key-header](https://datatracker.ietf.org/doc/draft-ietf-httpapi-idempotency-key-header/) . Treat it as a work-in-progress convention, not a ratified standard: the HTTP Idempotency-Key request header field[can be used to make non-idempotent HTTP methods such as POST or PATCH fault-tolerant](https://datatracker.ietf.org/doc/html/draft-ietf-httpapi-idempotency-key-header-07) , but the draft itself carries the standard notice that it must not be cited other than as work in progress. The server side is straightforward: on a duplicate request whose idempotency key has been seen,[the resource server should respond with the result of the previously completed operation](https://datatracker.ietf.org/doc/draft-ietf-httpapi-idempotency-key-header/03/) , success or an error.


## Confirmation UX after the user has left


The hard UX problem is that deferred delivery often completes after the user has navigated away, so you need a path to reconcile the optimistic “Queued” state once the request actually lands. Three moves cover it:


- **Flip “Queued” → “Sent” live.** When the service worker successfully replays an item,` postMessage` to any open client so the UI can update in place. Listen with` navigator.serviceWorker.addEventListener('message', ...)` .
- **Reconcile on next load.** On startup, read the queue’s drained state: items still present are pending; their absence means delivery succeeded. Render status from the store, not from a flag set at submit time.
- **Surface terminal failure.** Use` event.lastChance` in the` sync` handler to fire a notification (or persist a “failed” marker the UI reads on next load) so a submission that exhausted its retries doesn’t disappear silently.


Confirmation UX is the second place session replay earns its keep: replay makes the user-side timeline visible — the queued submission, the screen they saw, whether the “Sent” confirmation ever rendered — which is the only view that shows the trust gap when a request never reached the server to be logged.


## Workbox as the production shortcut


If you’d rather not hand-roll the queue,[Workbox](https://developer.chrome.com/docs/workbox/modules/workbox-background-sync) (currently major version 7) provides` BackgroundSyncPlugin` , which queues failed requests in IndexedDB and replays them on` sync` events. Critically, it ships its own fallback: in browsers that don’t natively support the BackgroundSync API, Workbox Background Sync will automatically attempt a replay whenever your service worker starts up.


```text
import   {   BackgroundSyncPlugin   }   from   '  workbox-background-sync  '  ;
import   {   registerRoute   }   from   '  workbox-routing  '  ;
import   {   NetworkOnly   }   from   '  workbox-strategies  '  ;


const   bgSync   =   new   BackgroundSyncPlugin  (  '  order-queue  '  , {
maxRetentionTime  :   24   *   60  ,   // minutes; the documented example keeps items 24h
});


registerRoute  (  /  \/api\/orders  /  ,   new   NetworkOnly  ({   plugins  :   [  bgSync  ]   }),   '  POST  '  );
```


Two behaviors trip people up. First, BackgroundSyncPlugin hooks into the fetchDidFail plugin callback, and fetchDidFail is only invoked if there’s an exception thrown, most likely due to a network failure, which means requests won’t be retried if there’s a response received with a 4xx or 5xx error status. If you want 5xx responses requeued, add a` fetchDidSucceed` plugin that throws on` response.status >= 500` . Second, when testing, you can check the requests have been queued by looking in Chrome DevTools > Application > IndexedDB, and force a replay from DevTools > Application > Service Workers. Don’t validate offline behavior with the DevTools “Offline” checkbox — it blocks page requests but lets service-worker requests through, so it hides exactly the bugs you’re testing for. Disconnect the real network instead.


Whether you hand-roll or adopt Workbox, the architecture is the same:


1. capture locally before the network;
2. prefer Background Sync where it exists;
3. fall back to` online` events everywhere else;
4. dedupe replays with an idempotency key; and
5. reconcile the UI when delivery finally lands.


Wire those five pieces together against your own form, then verify a real replay by queuing offline, reconnecting, and confirming a single server record appears.


## FAQs


What is the difference between one-off Background Sync and Periodic Background Sync?


One-off Background Sync uses the SyncManager interface to replay a single deferred task, such as a queued form submission, once connectivity returns, and the browser fires a sync event that can arrive even after the user navigates away. Periodic Background Sync uses a separate PeriodicSyncManager interface for recurring updates on a browser-managed schedule, like refreshing content. They are distinct APIs, and Periodic Background Sync is experimental, so check its compatibility table before relying on it in production.


Why does my queued request still fail to retry when the server returns a 400 or 500 error?


Background Sync and Workbox's BackgroundSyncPlugin retry only requests that fail from a true network error, because the plugin hooks into the fetchDidFail callback, which fires only when an exception is thrown. A 400 or 500 response counts as a received response, so it is treated as delivered and will not be replayed. To requeue 5xx responses, add a fetchDidSucceed plugin that throws when response.status is 500 or higher, or throw manually in a hand-rolled sync handler.


Does Background Sync work inside an Android WebView wrapping a PWA?


No. Android WebView, the component native apps embed for in-app browsing, does not expose SyncManager, so a PWA wrapped in a WebView shell loses the native Background Sync queue. The submission falls back to the online-event path, which fires only while a page of your origin is open and cannot wake a closed tab. Because of this and the absence of support in Firefox, Safari, and iOS, an IndexedDB queue with an online-event fallback must remain the baseline.


How can I test offline Background Sync behavior reliably?


Disconnect the actual network rather than using the DevTools 'Offline' checkbox. The Offline checkbox blocks requests from the page but lets service-worker requests pass through, so it hides the exact bugs you are testing for. Verify queued items in Chrome DevTools under Application then IndexedDB, and force a replay from Application then Service Workers. To confirm the full flow, queue a submission offline, reconnect, and check that a single server record appears.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
