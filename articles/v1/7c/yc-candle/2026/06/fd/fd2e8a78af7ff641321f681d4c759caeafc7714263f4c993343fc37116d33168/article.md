---
schema_version: "1.0.0"
document_id: "fd2e8a78af7ff641321f681d4c759caeafc7714263f4c993343fc37116d33168"
company_key: "yc-candle"
company: "Candle"
source_id: "yc-candle-news-import-d1745da95f35"
canonical_url: "https://www.trycandle.app/blog/thumb-kiss-realtime"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-21T12:14:31.667387+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:d1270b96ca9f3fd05c748e7caf57d77812c8f2f3e53387fe4b6a25667b08fdec"
---

# The Realtime Engineering Behind Two Thumbs

At Candle we build software for couples, and a lot of that software is more complicated than it looks: shared games, daily rituals, photo memories, widgets, notifications, and the endless bookkeeping that comes with two people sharing one piece of state. Thumb Kiss doesn't look like one of those features.


You press your thumb on the screen. Your partner presses theirs. As the two thumbs get close, the surface glows, pulses, vibrates, and, if you picked one, drops an emoji across your partner's screen. That's it.


The product goal was just as small: a gesture that says "I'm here" without turning into a whole conversation. The trick is that "small and effortless" and "easy to build" are not the same property, and this one only works if it feels genuinely live. If your partner's thumb jumps, lags, sticks after they've let go, or takes a couple of seconds to show up, the magic immediately reads as a bug. But we also didn't want every Thumb Kiss tile on the home screen holding ahot socket open all day just in case.


So the real question was:


> How do we make a tiny interaction feel instant without making the whole app permanently realtime?


"Use WebSockets" is the answer to a different, easier question. Transport was the part we worried about least. The hard part was deciding what should be live, what should be durable, and what should be allowed to quietly disappear.


## The Database Is Not An Animation Pipe


The obvious prototype is the one you'd reach for first:


```text
user A touches
-> write A's position
-> B's subscription fires
-> repeat, many times per second
```


This is a great way to prove the idea works, and a bad way to ship it.


Our backend is[Convex](https://www.convex.dev/) , and we lean on it hard for canonical state: who's paired, who's allowed in a room, whether a partner was recently live, and what the app should recover after a reconnect. Convex even has a lovely[multiplayer-cursors demo](https://www.youtube.com/watch?v=Tx_YIN-87_M) that does roughly the write-on-every-move thing. And for cursors it's the right call, because a cursor that's a beat behind is still a useful cursor. A thumb that's a beat behind stops feeling like your partner is in the room.


The cost math doesn't work either. We flush motion several dozen times a second while a thumb is moving. A one-minute kiss is already thousands of updates; put that behind a Convex mutation and you're paying for a function call per animation frame, on the one code path where you can least afford the latency of durable writes.


And most of that data is worthless a moment later. Thumb motion is high-frequency, disposable, and only meaningful in the present. If your partner moved through ten positions while your phone was catching up, you don't want to replay all ten. You just want to know where their thumb is *now* .


So we split the system three ways:


```text
durable backend (Convex):  auth, pair state, coarse presence,
final position, recovery


live relay:                active motion, release events,
current partner position


client:                    smoothing, haptics, position cache,
lifecycle cleanup
```


Convex stays the source of truth. The relay stays fast. The client stitches the two together so the seams don't show. If we did it right, you never think about any of this.


## Thumb Motion Is Not Chat


The mental model that unlocked the rest:


> For live motion, stale data isn't history. It's simply wrong.


In chat, every message matters. Drop one and the conversation changes. In a collaborative editor, edits need ordering and durability. Thumb Kiss is the opposite. If the network queues up old motion and delivers it late, your partner's thumb lurches backward through time. You technically received *more* data and the experience got worse.


So the live path is tuned for freshness over completeness:


-


show the newest position, full stop


-


let newer motion supersede older motion that hasn't been sent yet


-


treat release as a high-priority state change, not just another sample


-


under load, drop frames rather than queue and replay them


That last bullet is the one that quietly shaped everything else. We'd rather skip a few stale samples than faithfully animate the past.


## Warm, Not Always On


The next thing to fight was startup latency.


The tile lives on the home screen, and people sit on the home screen for a long time. If every visible tile kept a socket open, we'd be paying for a lot of idle presence to make one small feature feel ready. But if the user taps in and *then* waits on auth, routing, and a fresh connection, the first second feels broken.


The compromise was to prepare without connecting. The client can fetch enough signed routing info to know *where* it would connect. Convex hands it a short-lived, signed route grant, and the client caches that. No socket opens. No room is joined. It's just holding a resolved address it hasn't dialed yet.


Then, when intent becomes real, the join is quick. Intent can be opening fullscreen, touching the tile, or waking up because your partner just went live.


```text
visible  != connected
prepared != joined
touching == live
```


That gave us a feature that feels warm without keeping every room hot.


## Keep The Couple In The Same Room


Once you have realtime rooms, routing starts to matter.


The lazy version is to let each user land on whatever relay node the load balancer picks, then coordinate across nodes. That's fine for plenty of systems. For Thumb Kiss it's the wrong shape: when partner A is on one node and partner B is on another, every motion packet takes an extra hop between them. That hop is usually fast. But "usually fast" is a weak promise for something that's supposed to feel like touch. (When it *does* happen, a layer of Valkey pub/sub ferries motion between the nodes, so a split pair still works; we just don't want it to be the common case.)


What we want most of the time is the short path:


```text
A's socket -> the pair's relay node -> B's socket
```


So we route through a stable logical home that's decoupled from the physical machine currently serving it:


```text
pair -> virtual shard -> physical relay node
```


We hash the pair ID to one of a few thousand virtual shards, and a routing table maps that virtual shard to a physical node. The virtual shard is the pair's permanent address; the physical node behind it can change as we add or drain capacity. New joins drift toward healthy nodes without the app ever learning how many machines exist, and without re-implementing pair authorization outside Convex. The couple gets one shared room. We get to decide which physical room that actually is.


## The Client Is Part Of The Protocol


It's tempting to file all of this under "backend," but a surprising amount of the feel lives on the device.


The client normalizes touch coordinates to a 0–1 space so the tile and the fullscreen view agree on where a thumb is, regardless of their pixel dimensions. It tags every motion sample with a session ID and a sequence number, so a straggling old packet can't overwrite newer truth; anything with a sequence number we've already passed just gets dropped. And it smooths incoming partner motion (a light exponential ease, plus a small predictive lead to covernetwork jitter) so the other thumb glides instead of stuttering, without feeling laggy.


It also caches the latest trustworthy position for both thumbs. That sounds like a throwaway detail until you hit this sequence:


1.


both people are kissing on the small tile


2.


both tap into fullscreen


3.


the live interaction was perfectly correct the whole time


4.


the two fullscreen views boot up with the thumbs in different places


That's the kind of bug that gets two people tilting their heads at their phones in sync. The fix wasn't to write every frame to Convex. It was to carry the last known-good position across surfaces locally, keyed by pair, user, and role.


The priority order ended up being:


1.


fresh motion off the relay


2.


the local position cache, when moving between surfaces


3.


the durable Convex snapshot, for cold start or recovery


Each layer has exactly one job. Every weird bug we hit came from letting two of those jobs blur into each other.


## Release Is The Edge Case That Matters Most


The happy path is four lines and not worth writing about: connect, send motion, receive motion, disconnect. The interesting part is everything around it. What happens if the app backgrounds mid-touch? If one partner is in fullscreen and the other is still on the tile? If a long hold goes quiet? If an emoji keeps firing off stale state?


The one that mattered most was **release** .


When your partner lifts their thumb, the UI has to stop pretending they're still pressing, immediately. A stale release doesn't read as a network hiccup; it reads as the app lying to you. So while ordinary motion can be coalesced and old samples can be dropped, release is a high-priority transition that has to punch through regardless of what else is queued.


The rest of the lifecycle policy fell out of the same principle:


-


send motion immediately while touching


-


skip durable writes for the in-between movement


-


advertise live presence only when it actually helps your partner find you


-


keep a long hold discoverable without hammering the backend


-


flush cleanup when the app backgrounds or the surface closes


-


make release loud and impossible to miss


A good chunk of this feature, it turns out, is just making sure a digital thumb knows when to leave.


## What Convex Still Does


We didn't take the backend out of Thumb Kiss. We narrowed its job to the things it's actually good at. Convex still owns the durable truth:


-


user and pair authorization


-


issuing the signed route grants


-


coarse "this session is live" advertisement


-


final position snapshots


-


fallback state for cold starts


-


protection against stale or replayed sessions


What it explicitly does *not* do is animate the thumb. If your partner's phone was offline, the durable snapshot lets the app recover a sane latest position later. While both phones are actively touching, the relay carries the motion and Convex never sees it.


That split also keeps cost in the right shape. A couple tapping away shouldn't generate a function call per movement; the backend should only hear about meaningful lifecycle transitions: session started, session refreshed, session ended, not the pixels in between.


## Testing The Feeling


We have real tests for the parts you'd expect: auth, route issuance, session ordering, rejecting stale updates, final snapshots, plus the client utilities around freshness and the position cache. Those are worth having and deeply unglamorous.


But the bugs that actually shipped problems only showed up with two phones and some patience:


-


release works in fullscreen but not on the tile


-


a reconnect lets an old position win


-


motion arrives but feels jagged


-


the tile and fullscreen view disagree after navigation


-


a partner looks active after their session already ended


None of those reproduce in a unit test. Finding them mostly involved two of us tapping at phones, squinting, and saying "wait, do that again" until everyone had strong opinions about thumb inertia. Those types of details and opinions matter to us a lot at Candle.


## What We Learned


We learned that realtime is a product feeling, not a transport decision. The useful question was never "should we use WebSockets?" It was: "what needs to feel alive, and when," and most of the architecture followed from answering that honestly.


A few things that generalize past this feature: not all state deserves durability, and treating disposable state as if it were precious is how you end up paying for cost, lag, and strange recovery behavior all at once. Freshness beats completeness when the data is only meaningful right now. And "visible" and "connected" are worth keeping separate. Preparing a route and joining a room are different actions with very different costs.


Local state also stopped feeling like a hack once it had a clear job. Caching the last known position isn't cutting a corner; it's the only thing that keeps the experience coherent as you move between surfaces in a single session.


## Parting Thoughts


Thumb Kiss is one of the smallest features we've built, and it forced more clarity about what "realtime" means inside Candle than most of the big ones did. We didn't want a heavy always-on multiplayer system, and we didn't want a database-backed prototype that looks live right up until two people actually try it. So we split the problem along the grain: durable state for what should survive, live state for what should feel present, prepared routes for what should be ready, a client cache for what should stay coherent, and aggressive cleanup for what should disappear.


For relationship software, performance isn't really about speed. It's about preserving the feeling that someone is there. The system is working when all the machinery fades out and the only thing left on the screen is your partner, on the other side, saying hello with their thumb.
