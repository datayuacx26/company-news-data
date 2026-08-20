---
schema_version: "1.0.0"
document_id: "52a92983ca443855e22be8308b40e033e0a5bcdbda9db7cb55e70f04c91a4de5"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-97-replicache-subscriptions-stalled-our-dashboard-editor/"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:680f863a6036a1098a9dc8dbabc58f018c5d3c2518911fa7614579137ccaddfe"
---

# How 97 Replicache subscriptions stalled our dashboard editor

For a long time, we read everything in the Basedash client through[Replicache](https://replicache.dev/) . Charts, chart versions, dashboards, sidebar items, members, all of it. One sync engine, one mental model, one place to look when something was stale. It worked, and it still works for most of our state.


Then a customer opened a 59-chart dashboard in edit mode and tried to drag a card. The grid froze for over a second, came unstuck, and froze again on the next move.


We fixed it by moving just the chart read path onto[TanStack DB](https://tanstack.com/db) collections backed by a server-sent events (SSE) topic, and leaving every chart mutation right where it was on Replicache.


## What Replicache is doing under the hood


Replicache gives you a local IndexedDB-backed cache that mirrors a subset of server state. The way you read from it is` experimentalWatch` or a subscription: you register a query function, and Replicache re-runs it any time the underlying data could have changed.


That’s a lovely API for an admin panel with 20 things on screen. The subscription re-evaluates, your component re-renders, you don’t have to think about it.


The trouble starts when you have a lot of components, each with its own subscription, all sharing one local store. Every mutation pokes the store, every subscription re-evaluates, and the bill comes due on the main thread.


## What the 59-chart dashboard was actually doing


We measured the dashboard with the Chrome DevTools performance panel and counted the live Replicache subscriptions. The number was 97.


That came from 1 subscription per chart card (the chart definition), 1 per chart card for the most recent version, a handful for the dashboard layout, the dashboard variables, the dashboard tabs, member access, and so on. Most of them queried the same underlying chart and chart-version stores.


Replicache doesn’t know which subscription cares about which row, so when any chart row changes (a layout drag, a version bump, a rename), all 97 subscription queries re-run. The query functions are fast individually, but 97 of them in a row, on the main thread, with React reconciliation underneath, is not fast.


The performance panel showed about **1.3 seconds of scripting** during a single drag move. INP (Interaction to Next Paint) hovered around 1300ms. That’s the kind of number that makes you reach for the React profiler and then abruptly close it.


## The shape of the fix


We wanted 3 things, in order:


- Stop the all-subscriptions-re-evaluate-on-every-mutation problem.
- Keep our existing mutation pipeline (Replicache mutators, server reconciliation, conflict handling) untouched.
- Get cold-start rendering faster, since dashboards are the first thing most users see.


The plan was to migrate just the **read path** for charts and chart versions to a new client cache, and keep Replicache as the source of truth for writes.


The new cache is a TanStack DB collection. The collection is a thin wrapper around a TanStack Query cache key, with a Zod schema and a` getKey` function. Components read from it with` useLiveQuery` , which only re-runs when the rows the query actually touches change. That’s the structural fix for the “97 subscriptions on every mutation” problem.


We then wired live updates through a new SSE topic,` dashboard:{id}` , that pushes the dashboard’s chart and chart-version data to any client subscribed to it.


```text
export   function   useDashboardSubscription  (  dashboardId  :   string   |   null   |   undefined  ) {
const   queryClient   =   useQueryClient  ();
const   orgId   =   useOrgId  ();
const   topic   =   dashboardId   ?   `dashboard:${  dashboardId  }`   :   null  ;


useStreamSubscription  (topic, (  data  )   =>   {
if   (  !  dashboardId)   return  ;


if   (  isEntitySnapshotEvent  (data)) {
// Replace dashboard's charts/versions in the org-scoped cache,
// preserving entries from other scopes.
// ...
}


if   (  isEntityUpsertEvent  (data)) {
for   (  const   op   of   data.ops) {
if   (op.entity   ===   'chart'  ) {
queryClient.  setQueryData  (  chartsQueryKey  (orgId), (  old  )   =>
upsertEntity  (old, op.data, chartSchema),
);
}
// ...
}
}
});
}
```


The collection itself is org-scoped, not dashboard-scoped, which matters in a few places. A user can have charts from a dashboard, charts from a chat, and charts from the editor all loaded at once. They should accumulate in one cache so that opening a chart by ID works regardless of how it got there.


```text
export   const   chartsCollection   =   (  organizationId  :   string  )   =>   {
const   queryKey   =   chartsQueryKey  (organizationId);
void   restoreQueryFromIdb  (queryKey);


return   createCollection  (
queryCollectionOptions  ({
schema: chartSchema,
id:   'charts'  ,
queryKey,
queryFn  :   async   ()  :   Promise  <  ChartEntity  []>   =>   [],
queryClient,
getKey  : (  item  )   =>   item.id,
staleTime:   Infinity  ,
}),
);
};
```


` staleTime: Infinity` is deliberate. The SSE subscription is the only thing allowed to write to this cache (other than the initial fetch and IDB restore), so we don’t want TanStack Query refetching anything on tab focus.


## Keeping Replicache, but only for writes


This is the part that took the longest to talk ourselves into. We could have moved mutations off Replicache too, and ended up with one consistent stack. We didn’t, because the cost was higher than the win.


Replicache’s mutator design handles optimistic updates, rebasing on server confirmation, conflict reconciliation, and offline queuing. We’ve been using all of that for years. Reimplementing it on top of TanStack DB just to be uniform would have been a project on its own, with new bugs we don’t have today.


So the data flow for a layout drag now looks like this:


1. User drops a chart card on a new grid position.
2. The client calls a Replicache mutator (` updateManyCharts` ) which optimistically writes the new positions into Replicache’s local store.
3. Replicache pushes the mutation to the server.
4. The server applies it, writes to Postgres, and then sends a Postgres` NOTIFY` on` signal:dashboard:{id}` .
5. Our SSE backend listens for that signal, recomputes the dashboard snapshot, and pushes an entity-snapshot event to every connected client subscribed to` dashboard:{id}` .
6. Each client merges the snapshot into the TanStack DB charts collection, and any` useLiveQuery` that touches those rows re-renders.


The reads come over SSE, the writes go over Replicache, and the user sees their drag both optimistically (from the Replicache mutator) and confirmed (from the SSE snapshot). It’s more moving parts on paper, but in practice each part does one thing.


## The optimistic-update problem we ran into


The first version of this pipeline had a subtle bug. A user drops a card on a new tile. The client writes the new position into TanStack DB via Replicache’s mutator path. Then, half a second later, the SSE snapshot lands with the old position (because the server hadn’t processed the mutation yet), and the card snaps back.


The fix is a small in-memory map of pending layout updates:


```text
// 1. Drag-end → addPending(chartId, { positionX, positionY, ... })
// 2. SSE snapshot arrives → applyPending(charts) merges pending positions on top
// 3. SSE snapshot with matching positions → confirmPending(charts) clears them
```


Any SSE snapshot for a chart with a pending position is overridden by the pending values, until the server’s snapshot matches what the client expected. Then the pending entry is dropped and SSE goes back to being authoritative.


This is the kind of thing Replicache does for you for free. We rebuilt a tiny version of it for the SSE read path, scoped to layout fields, because that’s the only case where the user sees a confirmed-then-reverted result.


## The footgun we hit in QA


Most of the migration was mechanical. One component bit us during browser testing.


` ChartMenuContent` is the dropdown menu that appears on every chart card (rename, duplicate, archive). It lives inside the chart card component, and the chart card is rendered in 2 places:


- Inside a dashboard, where` useDashboardSubscription` is mounted and the dashboard’s charts are guaranteed to be in the TanStack DB collection.
- On the org-wide` /charts` list page, where there’s no dashboard SSE subscription. The list page populates its own cache from a different route.


We switched the menu’s “most recent version” read to TanStack DB, which silently broke the` /charts` list page (the menu items rendered with no data because the version wasn’t in the org-scoped collection yet). The fix was to revert that one component’s read back to Replicache, until the` /charts` list page also moves off Replicache. We filed a follow-up issue and moved on.


The lesson, in retrospect: when you scope reads to a subscription that’s mounted by a route layout, every consumer of those reads has to live under that route layout. Shared components that get re-used in unrelated parts of the app violate that assumption.


## IndexedDB for the cold start


A side benefit of the migration: we now persist the TanStack DB collections into IndexedDB, so the dashboard renders with cached data before the first network request returns.


```text
void   restoreQueryFromIdb  (queryKey);
```


The` void` is intentional. The restore is async, but it has to start before` createCollection` so the IDB data wins the race against the placeholder` queryFn` (which resolves with` \[\]` ). If the user has visited a dashboard before, they see a fully populated grid in the first frame, and the SSE snapshot reconciles any deltas a few hundred ms later.


Cold dashboards (first visit ever) still wait for the network. We’re fine with that.


## What we measured after


Same 59-chart dashboard, same drag interaction, Chrome DevTools performance panel:


Metric Before After


Live subscriptions on the page 97 0


Scripting time per drag move (ms) ~1300 ~15


Presentation time per drag (ms) ~100 ~180


INP during edit-mode drag (ms) ~1300 ~195


INP went from “this feels broken” to “this feels normal”. Scripting dropped by about 99% because the 97 subscriptions are gone and the only React work per drag is the grid item that moved.


Presentation time went up a bit, which surprised us. The current best theory is that the grid library now actually has frame budget to lay out cards instead of being blocked by Replicache work, so the layout pass is what we’re measuring instead of a stalled main thread. We’ll take that trade.


## What we’d do differently


A few notes from doing this, in case it’s useful for the next migration:


1. **Migrate reads before writes, on the hottest path first.** The two-engine setup feels wrong on paper. It was a much smaller change than a full migration, and it’s where 100% of the user-visible win came from. Writes can move later, or never, without anyone noticing.
2. **Watch for shared components that don’t live under your new subscription.** The dashboard-scoped subscription was a great fit for the dashboard, and an active hazard for the` /charts` list page. Audit every consumer of a hook before flipping it to a scoped collection.
3. **Don’t try to delete optimistic-update logic just because the new system is “more correct”.** Servers reconcile in their own time, and any read path that lands after an optimistic write needs to handle the gap. Our pending-layout map is 100 lines and saves a lot of visual jank.
4. **` staleTime: Infinity` plus an SSE subscription is a real pattern.** It’s the right default when you’ve decided the server push is the only thing allowed to mutate the cache.
5. **IndexedDB persistence pays for itself the first time a user reopens a dashboard.** It’s a few lines of code and a noticeable change in how the app feels on a warm cache.


If you want more of how we think about client-side performance, we also have writeups on[the Kubernetes default that was killing our Node.js pods](https://www.basedash.com/blog/what-was-killing-our-healthy-kubernetes-pods) ,[paginating user-written SQL across every dialect we support](https://www.basedash.com/blog/paginating-user-written-sql-across-every-dialect-we-support) , and[virtualizing our table to render 100x more rows](https://www.basedash.com/blog/how-virtualization-increased-our-table-performance-by-500) . All the same shape: small change, measured before and after, surprising thing learned along the way.


And if you want to see what we’re building with this stack,[Basedash](https://www.basedash.com/) is an AI-native BI platform that runs on top of it.
