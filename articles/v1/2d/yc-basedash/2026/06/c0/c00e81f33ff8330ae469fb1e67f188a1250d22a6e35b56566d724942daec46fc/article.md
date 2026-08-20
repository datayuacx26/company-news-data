---
schema_version: "1.0.0"
document_id: "c00e81f33ff8330ae469fb1e67f188a1250d22a6e35b56566d724942daec46fc"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/we-replaced-liveblocks-with-postgres-and-sse/"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:42c6a91375a7ea3dd7f2fc244496ae27f6d4d96ee39550e93ce740f4bd127e45"
---

# We replaced Liveblocks with Postgres and SSE

Open Basedash with a teammate and you see little avatars in the corner: who else is in the org, which dashboard they’re looking at, which chart they’re editing. That’s presence, and for a couple of years we ran it on[Liveblocks](https://liveblocks.io/) .


Liveblocks is good at what it does. It’s a realtime collaboration platform with presence, shared storage, comments, and a CRDT sync engine. We used exactly one of those things. We never stored documents in it, never used its comments, never touched the conflict resolution. We pulled in two SDKs, an auth route, a secret key, a CSP allowance, and a Vite chunk, all to render some avatars.


So we ripped it out and built presence on two things we already ran in production. The PR removed` @liveblocks/node` ,` @liveblocks/react` , the` liveblocks.config.ts` , the` /api/liveblocks-auth` route, and the` LIVEBLOCKS_SECRET_KEY` env var. Here’s how the replacement works.


## What presence actually needs


Presence is the easiest realtime feature to get right, because the bar is low.


It’s ephemeral, so nobody needs yesterday’s avatars. It’s best-effort, so if an update is lost the worst case is an avatar that’s stale for a few seconds. And there’s no conflict to resolve, since each user only ever writes their own location.


That’s the whole reason a full sync engine was overkill here. Durable storage and CRDTs solve problems presence doesn’t have. What presence needs is a way to push small updates to connected clients, and a way for those updates to reach every server process. We already had both.


## The two primitives we already had


The first is Server-Sent Events. We use SSE for Replicache pokes and for aborting in-flight chat messages, so the client plumbing (a long-lived` GET` that streams` text/event-stream` ) was already battle-tested.


The second is Postgres` LISTEN` /` NOTIFY` , wrapped in a small` pg-pubsub` helper. We run multiple web processes behind a load balancer, so a presence update that lands on process A has to reach the clients connected to process B. Postgres pub/sub is how our Replicache pokes already fan out across processes, so we added one more channel called` presence` .


Postgres` NOTIFY` isn’t a real message bus. Payloads cap at 8KB, and it drops messages if nobody’s listening at that instant. For presence, both are fine. Our payloads are tiny, and a dropped update self-heals (more on that below).


## The client side


Each tab generates a` sessionId` with` nanoid` when the` PresenceProvider` mounts. It opens an SSE stream to read everyone’s presence, and POSTs its own changes to a separate endpoint. Four actions cover the lifecycle:` join` ,` update` ,` heartbeat` , and` leave` .


```text
const   body  :   PresenceUpdateRequest   =   { action, orgSlug, sessionId, presence };
void   fetch  (  '/api/presence/update'  , {
method:   'POST'  ,
headers: {   'Content-Type'  :   'application/json'   },
body:   JSON  .  stringify  (body),
keepalive: action   ===   'leave'  ,
});
```


The heartbeat fires every 15 seconds to keep the session alive. Leaving is the interesting case, because “leaving” usually means closing a tab, and a normal` fetch` gets killed when the page unloads. So on` pagehide` we try` navigator.sendBeacon` first, which the browser promises to deliver even as the page dies, and fall back to a` keepalive` fetch.


The read side is a` fetch` -based SSE reader with reconnect built in. If the stream drops, it retries with exponential backoff starting at 1 second and capping at 30.


```text
backoffMs   =   1_000  ;
for   await   (  const   message   of   parseSseStream  (response.body)) {
const   parsed   =   PresenceSnapshotEventSchema.  safeParse  (  JSON  .  parse  (message.data));
if   (  !  parsed.success   ||   parsed.data.orgSlug   !==   orgSlug)   continue  ;
setSessions  (parsed.data.sessions);
}
```


Every message is validated with Zod before it touches React state. The server only ever sends full snapshots, so the client never has to merge diffs. It just replaces its list of sessions and re-renders the avatars.


## The server side


Each web process holds a` PresenceRegistry` , which is an in-memory map of sessions grouped by org. When a client POSTs an update, the registry upserts the session locally, broadcasts a fresh snapshot to every SSE subscriber on that process, and then publishes the change to Postgres so the other processes can do the same.


```text
this  .  setSession  (session);
this  .  broadcastSnapshot  (orgSlug);
void   this  .  publish  ({
type:   'upsert'  ,
originId:   this  .processId,
orgSlug,
session:   this  .  toSession  (session),
});
```


That` originId` is the trick that keeps multi-process fan-out sane. Every process tags its published events with its own id, and ignores anything it sees with its own tag. So a process never reacts to its own echo off the` NOTIFY` channel.


When a process receives an` upsert` from a peer, it stores the session as non-local and broadcasts a new snapshot to its own subscribers. Conflicts get resolved by` lastSeenAt` : if an incoming event is older than what we already have, we drop it. Last write wins, which is exactly right for “where is this user.”


## The part that makes it self-healing


There’s a cold-start problem with pub/sub presence. A new SSE connection lands on a process that has no idea who’s online, because it only knows about sessions it has seen events for since it booted.


We solve it with a` sync-request` . When a subscriber connects, its process publishes a request, and every other process responds by re-announcing its local sessions.


```text
if   (event.type   ===   'sync-request'  ) {
this  .  reannounceLocalSessions  (event.orgSlug);
return  ;
}
```


On top of that, every session carries a TTL. Sessions expire 45 seconds after their last heartbeat, processes re-announce their local sessions every 10 seconds, and a sweep runs every 15 seconds to evict expired ones and push a corrected snapshot.


This is why dropped` NOTIFY` messages don’t matter. If a publish fails, or a peer misses it, or a whole process dies, the stale sessions just age out and the surviving processes keep re-announcing the live ones. There’s no cleanup job to write and no orphaned-avatar bug to chase. The system converges on its own within a heartbeat cycle.


## What we deleted


The new feature is a few hundred lines of TypeScript and zero new dependencies. The diff that came out the other side was the satisfying part:


- 2 npm packages (` @liveblocks/node` ,` @liveblocks/react` )
- the` liveblocks.config.ts` global type declarations
- the` /api/liveblocks-auth` route
- a Content-Security-Policy allowance for the Liveblocks domain
- a dedicated Vite chunk for the Liveblocks client bundle
- the` LIVEBLOCKS_SECRET_KEY` secret, including for self-hosted customers


Self-hosted operators got the best part. Realtime presence now runs on infrastructure they already operate, so there’s one fewer external service and one fewer secret to manage.


## What we gave up, and what we’d reconsider


We’re not pretending this matches Liveblocks feature-for-feature. We gave up live cursors, shared storage, and comments. We never used any of them, so that was an easy trade, but if we wanted multiplayer cursor trails tomorrow we’d be building more than presence.


A couple of things we’re watching. Every process keeps every org’s active sessions in memory, which is cheap at our scale but wouldn’t be free at 100x the concurrent users. And` LISTEN` /` NOTIFY` runs over a single Postgres connection per process, so presence traffic shares a lane with our other pub/sub channels. Neither is a problem yet, and both have known fixes (shard the channel, move to a dedicated bus) if they ever become one.


## What we took away


A few things we’ll carry forward.


1. **Match the tool to the durability you actually need.** Presence is ephemeral and best-effort, so a durable, conflict-free sync engine was solving problems we didn’t have.
2. **TTLs beat cleanup logic.** A 45-second expiry plus periodic re-announce made the whole system self-healing, with no reconciliation job and no way to leak a stale avatar forever.
3. **Reuse your primitives.** SSE and Postgres` LISTEN` /` NOTIFY` were already in production for other features. Building on them meant no new dependency, no new failure mode, and no new thing to learn.
4. **Tag your own messages.** A per-process` originId` is the one-line fix that keeps pub/sub fan-out from echoing back on itself.


If you want more in this vein, we’ve written about[running background jobs on Postgres](https://www.basedash.com/blog/what-we-learned-running-background-jobs-on-postgres) and[why we moved off React Query](https://www.basedash.com/blog/why-we-had-to-move-away-from-react-query) . Both are the same shape: a smaller, more boring system that we understand end to end.


And if you want to see what we build with all this,[Basedash](https://www.basedash.com/) is an AI-native BI platform running on the stack we keep writing about.
