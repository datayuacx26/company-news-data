---
schema_version: "1.0.0"
document_id: "a0ecf81cef76ca5fa31777c54c37c57f733d0a8bfc54350d558cb76728f414e0"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-af8c2eac0472"
canonical_url: "https://www.atlassian.com/blog/how-we-build/building-real-time-pwa-atlassians-own-stack"
published_at: "2026-08-07T01:31:24+00:00"
first_seen_at: "2026-08-13T01:30:56.386294+00:00"
fetched_at: "2026-08-13T01:30:58.023305+00:00"
content_hash: "sha256:c64121cf541e82d809fd4067d94a7e42d62131973c43a519fe55c03aa4db4861"
---

# Building a real-time PWA on Atlassian’s own stack

For Atlassian Unleash 2026, our inaugural tech summit in Bengaluru that brought together 500+ engineering leaders, senior engineers, and product builders from across the industry to explore AI-powered ways of working, we didn’t want just another schedule app. We wanted the app *itself* to be the product story.


That decision changed the app’s intent. It was no longer just a utility. It became a single app where attendees could browse the agenda, manage their personal schedule, watch session recordings, earn points through hands-on challenges, ask an AI guide questions about the event, and contribute to production features by building them live during the event.


It was built with React/TypeScript as a PWA, serving 500+ concurrent users on the Atlassian stack, with AI woven through the build. But the real differentiator was this: attendees didn’t just use the app, they shipped real code to it in production, live during the event.


## How we built it: AI shaped the idea, not just the code


The app started out the way many projects do: messy thoughts, brainstorming, and a deadline that didn’t move.


AI was not just a feature we added at the end. It was part of the build process itself: Loom for async context, Rovo Chat to shape the concept (and later guide attendees), and Rovo Dev to scaffold and implement features straight from tickets.


### Unleash Stack – Atlassian Product + Platform


A stateless React PWA sat on top of Atlassian products and platform services, turning the stack itself into part of the event experience. Attendees were interacting directly with the products: their schedule lived in Jira, their questions were answered by Rovo grounded in Confluence, their session recordings played through Loom, and the code they wrote shipped via Bitbucket Pipelines.


**React PWA → thin stateless service → Atlassian product/platform APIs**


## Unleash App Architecture


We kept the architecture intentionally simple and let **Atlassian stack carry the** **heavy lifting for collaboration, workflow, identity, knowledge, deployment, and measurement.** The architecture had five layers, each with a clear job:


**The PWA** was a static React app hosted on a static hosting service backed by a global CDN. It had no server of its own. All authenticated calls went through Atlassian’s platform edge, which handled routing, session management, and API gateway responsibilities.


**Jira Cloud was the system of record.** Sessions, booths, speakers, attendee state, user action events which could gain leaderboard points, leaderboard rows, and push subscriptions all lived as Jira issues with structured fields. The app read and wrote to Jira through its REST APIs.


**A thin stateless service** sat between the PWA and Jira for operations that needed elevated privileges: bot-authenticated writes, notification signing, and smoothing traffic spikes. It held no business state and no database. Its job was to make platform API usage safe from a browser context.


**Jira Automation** ran scheduled jobs: recomputing leaderboard totals, triggering push notification flows through the thin service, and updating workflow state. This replaced what would otherwise have been a cron service or a separate job runner.


**User provisioning** was handled by a separate orchestration layer (built on an internal provisioning orchestrator) that called the user invitation API to invite users into the org and grant Jira product access programmatically at login.


**The product experience layer** connected three additional Atlassian surfaces: Rovo + Confluence powered the AI event guide, Loom provided embedded session recordings with AI summaries, and Rovo Dev + Bitbucket enabled the live coding challenges where attendees shipped PRs to production.


**Analytics** flowed two ways: the PWA sent screen views and UI events to product analytics platform, while Jira-backed exports provided adoption, and leaderboard data for event reporting.


## The Engineering Under the Hood


As part of this app, we solved few interesting engineering challenges.


### Modeling Unleash event in Jira


An event like Unleash has multiple facets which are inter-linked with each other like sessions, speakers, booths, attendee registrations, events which earn reward points, a leaderboard, and push notifications. We needed a system of record that could handle all of those and could adapt to frequent changes we intended to make. This made Jira the perfect choice.


Jira gave us a lot for free: structured records, permissions, audit history, workflow, automation, and an admin UI. Our data was not deeply relational; it was workflow-shaped. A session has fields and owners. An XP (experience point) event has history. A push subscription has a lifecycle. An announcement has approvals and delivery status. Those map well to Jira issues, fields, labels, project roles, and automation rules.


We designed around its strengths: queryable issues, predictable permissions, automation triggers, auditability, and admin-friendly configuration. That meant choosing the right issue types, keeping custom fields intentional, using labels for lightweight metadata, separating source-of-truth records from materialized views, and ensuring attendees, volunteers, and admins could only touch the things they were supposed to. The win was that most updates were configuration changes rather than deployments.


That also made Jira our admin portal. When the schedule had any additions or changes, we didn’t need a deploy, a migration, or a custom admin screen. Someone updated the relevant Jira issue, session time, room, speaker, track, or status, and the app picked it up through the same read path attendees were already using. For an event where the agenda continued to evolve through launch, this adaptability proved especially valuable.


### Designing for eventual consistency


New attendee provisioning was not instantly consistent. After login, account provisioning and permission propagation could take a short moment to settle across the system. During that window, a naive app could display empty screens or misleading error messages.


So we treated readiness as eventual, not binary. The app did not wait for every permission, field, index, and write path to become consistent before giving users something useful to do. The resilience layer had four responsibilities:


- Render useful content immediately from cached or bundled data.
- Persist user intent locally before trusting the remote write path.
- Validate response shape, not just HTTP status.
- Overlay recently completed actions while Jira’s read path caught up.


#### Readiness as a state machine


We modeled readiness as a state machine rather than a boolean:


In the converging state, the app could render safe event data, capture user intent locally, and retry delivery in the background. In the ready state, live reads became authoritative. If later requests degraded, the same path handled recovery instead of introducing a separate failure mode.


#### Capturing user intent with a durable outbox


For writes, we used a durable local outbox. The app recorded important user actions locally before depending on the backend write path. The simplified flow was:


```text
User action -> local outbox -> backend write succeeds -> settled overlay -> live read confirms -> remove overlay
```


The settled overlay mattered because a write can succeed before the read model reflects it. If we removed local state immediately after a successful write, the UI could briefly appear to undo the user’s action. Keeping a settled overlay preserved the user’s intent until the next confirmed live read caught up.


```text
// Simplified for the blog
async function recordUserAction(action: OfflineOperation) {
const operation = {
id: createOperationId(),
type: action.type,
payload: action.payload,
status: 'pending',
createdAt: Date.now(),
retryCount: 0,
};


await offlineStore.operations.put(operation);
applyOptimisticOverlay(operation);
scheduleSync();
}


async function syncPendingOperations() {
const pending = await offlineStore.operations
.where('status')
.equals('pending')
.sortBy('createdAt');


for (const operation of pending) {
if (!(await readinessProbe())) {
scheduleRetry(operation);
return;
}


try {
await sendOperationToBackend(operation);


await offlineStore.operations.update(operation.id, {
status: 'settled',
settledAt: Date.now(),
});


keepOverlayUntilLiveReadConfirms(operation);
} catch (error) {
if (isTransient(error)) {
await offlineStore.operations.update(operation.id, {
retryCount: operation.retryCount + 1,
});
scheduleRetry(operation);
return;
}


await offlineStore.operations.delete(operation.id);
removeOptimisticOverlay(operation);
}
}
}


async function reconcileAfterLiveRead(liveState: LiveState) {
const settled = await offlineStore.operations
.where('status')
.equals('settled')
.toArray();


for (const operation of settled) {
if (liveStateReflects(operation, liveState)) {
await offlineStore.operations.delete(operation.id);
removeOptimisticOverlay(operation);
}
}
}
```


### Supporting Push Notifications


Push notifications were modeled through Jira workflows. Creating an announcement issue would trigger a notification flow. Scheduled automation would find upcoming sessions and send reminders. Jira acted as the control plane, while the stateless service handled signing and delivery.


### Gamifying the Leaderboard


In order to gain reward points for actions performed during the event, the attendees could perform certain tasks in the app. These tasks were captured as XP events in the app and these events became the source of truth for the leaderboard. Leaderboard rows were a materialized view. Jira Automation recomputed totals through a mix of near-real-time updates, periodic incremental recomputation, and full rebuilds. This gave us flexibility: adding a new way to earn XP did not always require a code deployment; in many cases, it meant changing the Jira configuration.


The model looked like this:


` XP Events (write model) -> Jira Automation recomputes totals -> Leaderboard Rows (read model)`


That split gave us a repairable system. If a recomputation failed or the scoring rules changed, the leaderboard could be rebuilt from the source events rather than trusting each displayed score as the source of truth.


XP Events were the write model and source of truth. Leaderboard rows were the read model, a materialized view that could be recomputed, repaired, and audited. We did not need every tap to be a perfect transaction; we needed every score to be reconstructable.


### Performance


In the early build phase, we optimized for learning and feature coverage over polish. That was the right tradeoff: we needed sessions, schedules, challenges, notifications, and Rovo flows working end to end before we could tune the experience.


And it showed: on first load, the loader stayed up for roughly 12 seconds before users saw useful content. For an event app that people check between sessions, that was too long.


We started with measurement: HAR files to understand request timing, duplication, and waterfalls; Chrome Performance traces to inspect JavaScript and main-thread cost; and Lighthouse for high-level metrics. The HAR timeline turned “feels slow” into a concrete request map.


**Baseline:** 24 page-load API calls, 8 of them duplicates, a JavaScript bundle shipping unminified, and an auth waterfall that blocked returning users on provisioning they no longer needed.


#### Deduplicate in-flight requests


Multiple React providers and components mounted at nearly the same time and independently requested the same data. A cache of completed responses would not have helped because the first response had not returned yet. The fix was sharing in-flight promises:


```text
// Before: two components mount, two network calls
Component A requests /sessions -> network call 1
Component B requests /sessions -> network call 2
// After: second caller reuses the in-flight promise
Component A requests /sessions -> network call starts
Component B requests /sessions -> shares the same promise
Both receive the same result
```


We applied this to identity fetches, project metadata, leaderboard loading, registration status, and event data coordination. **Result: 24 to 16 page-load calls, 8 to 0 duplicates.**


#### Revalidate only what changed


Adding a session to a user’s schedule triggered a broad event-data refresh: sessions, speakers, agenda items, settings. But adding a session changes the user’s relationship to that session. It does not change the session catalog.


We switched from “any completed mutation reloads everything” to “inspect what kind of mutation completed, revalidate only if that category of data could have changed.” The optimistic overlay already showed the correct local state; most mutations needed no follow-up fetch at all. **Result: 11 to 5 API calls on “add to schedule.”**


#### Fix the production build


The production JavaScript was readable in DevTools and much larger than expected. The build was not minifying. This was a build configuration bug, not an application logic problem, but it had massive impact. **Result: ~7.7 MB to ~1.53 MB bundle (~80% smaller).**


#### Break the auth waterfall


The original startup flow was sequential:


```text
Fetch identity -> provision user -> check groups -> set user -> load app data
```


Provisioning was idempotent and only necessary for first-time users. Returning users were already provisioned but still paid the cost every visit. We restructured:


```text
Fetch identity -> check access first
-> if access exists: continue immediately
-> if access missing: provision, then re-check
```


Returning users take the fast path. First-time users still take the safe path. **Result: 5s reduced from the load time.**


#### Show useful data immediately


Even after network and auth improvements, the app waited for live data when useful local data already existed. We changed to stale-while-revalidate:


```text
Before: Wait for live data -> show app
After:  Show local/bundled data -> refresh in background -> swap when ready
```


The resilience pattern from *eventual readiness* was already a performance pattern. Once the app could render from safe local data first and revalidate in the background, users no longer had to wait for every API path before seeing the first useful screen.


**Combined result:** Page-load calls went from 24 to 16. Duplicates went to zero. Add-to-schedule calls dropped from 11 to 5. The bundle shrank from ~7.7 MB to ~1.53 MB. Returning users skip provisioning entirely. Time to first useful content went from ~12 seconds behind a loader to <1s.


### Shipping live: when the demo fights back


What set it apart wasn’t that attendees *used* the app; it was that they **built features and fixed bugs in it, live** , which meant every challenge had to run the full loop: Rovo Dev → Bitbucket → Pipelines → Production, on the same app everyone in the room was using.


And as live demos always do, it surprised us. Mid-event, a scheduled infrastructure change temporarily blocked our deployment path. Our pipelines paused, and no amount of retrying would help because the fix was upstream.


This is where running on the platform paid off. We identified which team owned the deployment path, reached out directly through the incident channel, and had an on-call engineer respond within a minute. Deployments were restored very quickly. Attendees watched the features they had built go live within a few hours.


On a live stage, the system you can operate fast beats the one that is theoretically cleaner.


## The impact


The result was more than a working app. It changed how attendees experienced Unleash.


- We hit **110% app adoption** , because the app didn’t stop at the people in the room.
- **80% of attendees** downloaded the app; 32% were on it *before* the event began.
- **8 features/bugs shipped live** to production during the event itself.


## What are we proud of?


We didn’t demo Atlassian from the outside. We let attendees live inside the stack: discover through Rovo, act through Jira, build with Rovo Dev, review in Bitbucket, ship through Pipelines, and see the result in the same app they were using.


By building it on the same stack we wanted attendees to experience, we shipped more than an event utility; we shipped a live demonstration of how Atlassian teams think, build, collaborate, and ship.
