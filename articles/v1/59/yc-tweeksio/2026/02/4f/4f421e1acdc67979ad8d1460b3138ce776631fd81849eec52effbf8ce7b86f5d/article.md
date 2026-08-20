---
schema_version: "1.0.0"
document_id: "4f421e1acdc67979ad8d1460b3138ce776631fd81849eec52effbf8ce7b86f5d"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-rss-bbf1d949eada"
canonical_url: "https://www.tweeks.io/blog/auth-mv3-architecture"
published_at: "2026-02-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.378405+00:00"
fetched_at: "2026-07-28T22:19:30.840186+00:00"
content_hash: "sha256:f7cf1ffcc0b2c25101acee12b78be0a9a3dae52f59bd0ce4f02a3c71f8218346"
---

# Fixing Intermittent Auth Failures in Chrome Manifest V3

A user opens the extension popup, sees they are signed in, clicks Generate, and gets` AUTH_REQUIRED` . They close and reopen the popup. Most times it works, but *sometimes* it doesn't.


Due to the sporadic nature of the errors, auth unexpectedly ended up being one of the most frustrating early challenges we faced while building the extension. Generating a userscript needs a signed-in session, and so does publishing one to the community library, like the[Universal AI Slop Finder](https://www.tweeks.io/t/PUjj1S2F) or[Remove Google Search Sponsored Results](https://www.tweeks.io/t/b3bb5a6573f949899f779e86) . This post explains the root cause of the intermittent failures and the architecture that made behavior predictable.


## TL;DR


The model that worked for us in MV3 was:


1. IndexedDB as the canonical auth store
2. Read-time refresh (` refresh-if-needed` ) at token-use boundaries
3. Serialized refresh across contexts (single-writer semantics)
4. Message passing for commands, not for session replication


If you are debugging intermittent auth in MV3, start with storage topology and refresh coordination.


## Why auth looked random in Manifest V3


Auth in MV3 extensions is a cross-context coordination problem.


In our extension, authenticated work can run in multiple contexts (popup, service worker, offscreen document, and our "bridge" page), but those contexts do not share an always-on runtime. Each context wakes up and suspends on its own schedule, so any context can become active with a stale view of the session and make a “freshness” decision (whether an auth token is still valid or needs refresh) based on outdated information.


If each context tries to manage session freshness locally, the system becomes vulnerable to stale reads and refresh races. These coordination failures only surface intermittently and fully explain the "random" auth behavior.


Three failure modes drove nearly all intermittent incidents:


1. ` Refresh races with token rotation` : two contexts refreshed at nearly the same time, one rotated the token, and the other received` invalid_grant` with a stale token.
2. ` Background refresh assumptions in MV3` : service worker suspension made timer-based freshness unreliable after idle periods, sleep/wake, or restart boundaries.
3. ` Message channels becoming state channels` : messaging worked for explicit actions (` signOut` , handoff, status) but produced stale snapshots when used to replicate session state.


## Why we moved from chrome.storage to IndexedDB


` chrome.storage.local` is the standard recommendation for managing extension auth, and it worked while authentication state was read and written from a mostly single execution path. In MV3, and especially in more complex extensions, auth work can happen concurrently across independent contexts, and that storage model started to accumulate proxy reads, fallback logic, and synchronization code.


All the cross-context coordination depended on a robust refresh lock, but` chrome.storage` doesn’t support transactions, so lock acquisition was a two-step read-then-write sequence. Under contention, two contexts could both observe “unlocked” before either write landed, and both would proceed. When that overlapped with token refresh, we’d see the downstream symptoms: stale-token failures and intermittent` AUTH_REQUIRED` .


IndexedDB solved that specific failure mode by supporting real transactions. We moved refresh lock acquisition into a single atomic readwrite transaction (check + acquire), eliminating the lost-update race inherent in chrome.storage lock steps. Unlike` chrome.storage` , it also let the offscreen context participate directly in the same durable auth store, which cut down proxy messaging and simplified coordination overall.


## Improved Architecture


```text
flowchart TB
subgraph Contexts
P[Popup]
SW[Service Worker]
O[Offscreen]
C[Content Bridge]
end


S[(IndexedDB Auth Store)]
L[(Refresh Lock Record)]
SB[Supabase Auth API]


P -->|read session| S
SW -->|read session| S
O -->|read session| S
C -->|intent message| SW


P -->|refresh-if-needed| L
SW -->|refresh-if-needed| L
O -->|refresh-if-needed| L
L -->|single refresh owner| SB
SB -->|new session| S
```


Auth behavior became predictable only after enforcing one consistent model across all entry points:


- IndexedDB is the canonical auth store.
- Refresh is evaluated where tokens are consumed.
- Only one context may own refresh at a time.


### IndexedDB-backed Supabase storage adapter


` db.js` (relevant excerpt):


ts


```text
import   { openDB }  from    "idb"  ;


const    USER_DB_NAME   =  "user"  ;
const    USER_DB_VERSION   =  1  ;
let   userDbPromise =  null  ;


function    isClosingError  ( error  ) {
if   (!error)  return    false  ;
return   (
error. name   ===  "InvalidStateError"   ||
/connection is closing/i  . test  (error. message   ||  ""  )
);
}


function    resetUserDbConnection  (  ) {
if   (userDbPromise) {
userDbPromise
. then  ( ( db  ) =>   {
try   {
db. close  ();
}  catch   (_) {}
})
. catch  ( () =>   {});
}
userDbPromise =  null  ;
}


async    function    getUserDb  (  ) {
if   (!userDbPromise) {
userDbPromise =  openDB  ( USER_DB_NAME  ,  USER_DB_VERSION  , {
upgrade  ( db  ) {
if   (!db. objectStoreNames  . contains  ( "sessions"  )) {
db. createObjectStore  ( "sessions"  );
}
},
}). then  ( ( db  ) =>   {
db. addEventListener  ( "close"  , resetUserDbConnection);
db. addEventListener  ( "versionchange"  , resetUserDbConnection);
return   db;
});
}
return    await   userDbPromise;
}


async    function    withUserDb  ( callback, { retries =  1   } = {}  ) {
for   ( let   attempt =  0  ; attempt <= retries; attempt++) {
try   {
const   db =  await    getUserDb  ();
return    await    callback  (db);
}  catch   (error) {
if   (! isClosingError  (error) || attempt === retries) {
throw   error;
}
resetUserDbConnection  ();
}
}
}


export    function    createSupabaseStorageAdapter  (  ) {
return   {
async    getItem  ( key  ) {
try   {
const   value =  await    withUserDb  ( ( db  ) =>   db. get  ( "sessions"  , key));
return   value ??  null  ;
}  catch   (error) {
console  . error  ( "[DB] Failed to get session item:"  , error);
return    null  ;
}
},
async    setItem  ( key, value  ) {
await    withUserDb  ( ( db  ) =>   db. put  ( "sessions"  , value, key));
},
async    removeItem  ( key  ) {
await    withUserDb  ( ( db  ) =>   db. delete  ( "sessions"  , key));
},
};
}
```


` supabase-client.js` (uses the adapter):


ts


```text
import   { createClient }  from    "@supabase/supabase-js"  ;
import   { createSupabaseStorageAdapter }  from    "../db.js"  ;
import   { supabaseLock }  from    "./cross-context-lock.js"  ;


let   storageAdapter =  null  ;


function    getStorageAdapter  (  ) {
if   (!storageAdapter) {
storageAdapter =  createSupabaseStorageAdapter  ();
}
return   storageAdapter;
}


const   clientConfig = {
auth  : {
persistSession  :  true  ,
autoRefreshToken  :  true  ,
detectSessionInUrl  :  false  ,
lock  : supabaseLock,
lockAcquireTimeout  :  30000  ,
},
};


try   {
clientConfig. auth  . storage   =  getStorageAdapter  ();
}  catch   (error) {
console  . warn  (
"[SupabaseClient] IndexedDB not available, creating client without persistence:"  ,
error. message  ,
);
clientConfig. auth  . persistSession   =  false  ;
clientConfig. auth  . autoRefreshToken   =  false  ;
}


const   supabase =  createClient  ( SUPABASE_URL  ,  SUPABASE_ANON_KEY  , clientConfig);
```


### Read-time refresh policy


Instead of relying on background refresh, we refresh where tokens are consumed.


ts


```text
const   { data } =  await   supabase. auth  . getSession  ();
const   session = data. session  ;
if   (!session)  return    AUTH_REQUIRED  ;


if   (! isExpired  (session. access_token  )) {
return   session. access_token  ;
}


return    await    withRefreshLock  ( async   () => {
const   {  data  : current } =  await   supabase. auth  . getSession  ();
if   (current. session   && ! isExpired  (current. session  . access_token  )) {
return   current. session  . access_token  ;
}


const   {  data  : refreshed, error } =  await   supabase. auth  . refreshSession  ();
if   (!error && refreshed. session  )  return   refreshed. session  . access_token  ;


if   ( isTransientNetworkError  (error))  return    RETRYABLE_AUTH_FAILURE  ;
await   supabase. auth  . signOut  ();
return    AUTH_REQUIRED  ;
});
```


### Serialized refresh (lock semantics)


The read-time refresh path above only stays safe if refresh ownership is serialized.


Refresh token rotation is a shared critical section, so we enforce single-writer semantics with a lease record (` ownerId` ,` expiresAt` ) to prevent concurrent refresh attempts. Acquisition uses one readwrite transaction to check lease state and claim ownership when free or expired. Waiters use bounded backoff with jitter, then re-read session state before attempting guarded recovery. Release is owner-checked so one context cannot clear another context's active lease.


Invariant: no context may call` refreshSession()` unless it currently owns the refresh lease.


### Error handling policy


Error class Typical source Behavior


` invalid_grant` (or equivalent token-invalid response) stale/invalid refresh token, revoked session treat as likely unrecoverable auth; clear and require sign-in


network timeout/transient fetch errors network instability, service hiccup keep session; return retryable auth error


offline` navigator.onLine === false` or explicit offline path do not force sign-out; surface retryable/offline state


lock acquisition timeout contention or stale lock path re-read canonical session, then attempt one guarded recovery path


## Migration from chrome.storage to IndexedDB


By the time we arrived at this design we already had a steady user base, so we shipped migration as a one-way move with rollback-safe behavior. First, we added IDB read support while keeping legacy records readable so no user was forced through a hard cutover. Next, on startup, contexts attempted migration only when IDB was missing and a legacy session existed. The migration validated required token and user fields before writing durable IDB state, then removed the legacy key only after the write succeeded.


After migration, we broadcast a migration event so other contexts refreshed their local auth view instead of waiting for incidental activity. We treated the whole routine as idempotent so retries were safe after interruption.


### Pitfalls to avoid


- Migrating malformed/partial sessions without validation.
- Clearing legacy storage before durable IDB write succeeds.
- Forgetting cross-context refresh after migration.
- Running migration in only one context and assuming all contexts observe it immediately.


## Production readiness checklist


Because these bugs were intermittent, rollout decisions came from telemetry and alert thresholds collected from real sessions.


1. Add auth telemetry: refresh attempts/success/failure, lock wait duration, lock timeouts, stale-lock takeovers, and session-clear reason codes.
2. Define alert thresholds before rollout (for example: lock timeout rate,` invalid_grant` rate, and` AUTH_REQUIRED` after recent successful sign-in).
3. Run multi-context race tests (popup + service worker + offscreen waking at the same time).
4. Run lifecycle tests (worker suspension, browser restart, sleep/wake, mid-refresh interruption).
5. Run degraded-network tests (offline, high latency, transient timeout bursts).


## Supabase-specific notes


The auth pattern we've described is generic, but Supabase has a few specifics. We use` onAuthStateChange` only as a trigger to re-read state. Privileged operations always read the current session from the shared store before acting. OTP and OAuth entry points go through the same storage, refresh, and lock path so behavior stays consistent across sign-in methods.


## Decision guide and tradeoffs


In engineering, there is no one-size-fits-all approach, and in many cases the basic chrome.storage approach works fine. Based on our findings, here's the criteria when choosing IndexedDB + refresh lock makes sense:


- multiple contexts can independently trigger authenticated calls
- offscreen is in the auth-critical path (requires more message passing)
- refresh token rotation makes concurrent refresh attempts unsafe
- worker suspension/restarts and offline transitions are common
- auth failures are expensive (user churn and hard-to-reproduce incidents)


Prefer a simpler chrome.storage-first design when one context owns most auth calls, refresh contention is rare, and occasional re-auth is acceptable.


## References


### Supabase


- Sessions


- [https://supabase.com/docs/guides/auth/sessions](https://supabase.com/docs/guides/auth/sessions)


- JavaScript client initialization and storage customization


- [https://supabase.com/docs/reference/javascript/initializing](https://supabase.com/docs/reference/javascript/initializing)


- PKCE/session guidance


- [https://supabase.com/docs/guides/auth/sessions/pkce-flow](https://supabase.com/docs/guides/auth/sessions/pkce-flow)


### Chrome extension platform


- Storage API


- [https://developer.chrome.com/docs/extensions/reference/api/storage](https://developer.chrome.com/docs/extensions/reference/api/storage)


- Can extensions use web storage APIs?


- [https://developer.chrome.com/docs/extensions/reference/api/storage#can_extensions_use_web_storage_apis](https://developer.chrome.com/docs/extensions/reference/api/storage#can_extensions_use_web_storage_apis)


- Identity API


- [https://developer.chrome.com/docs/extensions/reference/api/identity](https://developer.chrome.com/docs/extensions/reference/api/identity)


- Offscreen API


- [https://developer.chrome.com/docs/extensions/reference/api/offscreen](https://developer.chrome.com/docs/extensions/reference/api/offscreen)


- MV3 service worker migration


- [https://developer.chrome.com/docs/extensions/develop/migrate/to-service-workers](https://developer.chrome.com/docs/extensions/develop/migrate/to-service-workers)


- Storage and cookies concepts


- [https://developer.chrome.com/docs/extensions/develop/concepts/storage-and-cookies](https://developer.chrome.com/docs/extensions/develop/concepts/storage-and-cookies)


### Service worker behavior


- MDN service worker lifecycle concepts


- [https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers)


### Chromium discussion


- Offscreen access, messaging, and IDB tradeoffs


- [https://groups.google.com/a/chromium.org/g/chromium-extensions/c/99aUpv85-8A](https://groups.google.com/a/chromium.org/g/chromium-extensions/c/99aUpv85-8A)
