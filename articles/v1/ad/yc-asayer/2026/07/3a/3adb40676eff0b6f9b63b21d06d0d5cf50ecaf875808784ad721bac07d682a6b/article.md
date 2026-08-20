---
schema_version: "1.0.0"
document_id: "3adb40676eff0b6f9b63b21d06d0d5cf50ecaf875808784ad721bac07d682a6b"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/api-pagination-patterns/"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:93f26c80349f6799a84a58a7aef8ac060e59a4e06050aa2b75bb3bbc5e520d44"
---

# Pagination Patterns for Modern APIs

For new list endpoints, default to cursor pagination backed by a keyset query — it delivers near-constant query time regardless of dataset size and stays stable when rows are inserted or deleted between page fetches. Use offset pagination only when users need to jump to an arbitrary page number, the dataset is small and infrequently updated, or a total page count is a hard UI requirement. The decision turns on two failure modes that offset pagination cannot escape: linear slowdown at deep offsets and a shifting-window bug that duplicates or skips items when data changes mid-traversal. This article maps every pattern in the family — offset, cursor, keyset, page tokens, and Relay-style GraphQL connections — with runnable TypeScript and the SQL that backs each one.


## Key Takeaways


- Cursor and keyset are not synonyms: a cursor is the opaque token your API returns to the client; keyset is the SQL technique — a compound` WHERE (sort_col, id) < (val, id)` predicate — that makes cursor pagination fast by letting the database seek into an index instead of scanning and discarding rows.
- A query with` OFFSET 500000 LIMIT 20` does not skip 500,000 rows; PostgreSQL reads and discards them before returning 20, so query time grows with the offset value regardless of indexes on the sort column.
- When rows are inserted between page fetches, offset pagination shifts the window: the client receives a duplicate item and silently skips another, with no error in the response.
- Returning` total_count` on every response forces a separate` COUNT(*)` ; prefer` has_next_page` , derived cheaply by fetching` limit + 1` rows and checking whether the extra row exists.
- A cursor encodes a position in a specific ordered, filtered result set, so it must be discarded and reset to the first page whenever sort or filter parameters change.


## Why Pagination Strategy Matters: Two Failure Modes


Pagination strategy matters because the naive default —` LIMIT` and` OFFSET` — has two structural failure modes that only surface at scale or under concurrent writes. The first is performance: deep offsets force the database to read and discard rows. The second is consistency: offsets are positional, so any insert or delete between page fetches shifts the window and corrupts the result set. Neither bug appears in a small, static table, which is why offset pagination feels fine in development and breaks in production.


Here is the comparison the rest of the article justifies:


Pattern Random page access Stable under writes DB performance at depth Total count Best for


**Offset / limit** Yes No Degrades linearly Cheap-ish (extra query) Small static data, admin tables, search UIs needing page numbers


**Cursor (API surface)** No Yes Constant with keyset Omit; use` has_more` Infinite scroll, feeds, public list APIs


**Keyset (DB technique)** No Yes Constant (index seek) Hard The implementation under a cursor API


**Page token** No Yes Implementation-defined Optional APIs that want to hide and evolve their strategy


**Relay connection** No Yes Constant with keyset Optional GraphQL clients using the connections spec


The two right-hand mechanics — performance and stability — are what separate the patterns. Everything below explains why.


## Offset Pagination: How the Shifting Window Breaks


Offset pagination uses` page` and` limit` (or` offset` and` limit` ) to compute a numeric position:` offset = (page - 1) * limit` . It is the only pattern that supports jumping to an arbitrary page number, which makes it the right choice for admin tables and search results where users click “page 7” directly. Its weaknesses are both consequences of that positional addressing.


The performance weakness is documented behavior, not an implementation quirk. Per the[PostgreSQL documentation on LIMIT and OFFSET](https://www.postgresql.org/docs/current/queries-limit.html) , “the rows skipped by an OFFSET clause still have to be computed inside the server.” A query with` OFFSET 500000 LIMIT 20` does not skip 500,000 rows — the database reads and discards them before returning 20, which means query time grows with the offset value regardless of indexes on the sort column. This semantics is unchanged across recent PostgreSQL majors (16 through 18).


The consistency weakness is the shifting-window bug. When rows are inserted at the top of a feed between page fetches, offset pagination shifts the window: the item at position` offset` on page N+1 is the item that was at position` offset - 1` on page N, so the client receives a duplicate — and the item that fell off the boundary is silently skipped, with no error in the response. A deletion produces the mirror image: the window contracts and an item is skipped.


Here is a correct offset endpoint in TypeScript with Express and[pg 8.22.x](https://www.npmjs.com/package/pg) , running on Node 24 (Active LTS):


```text
// Node 24 (Active LTS), pg 8.22.x, PostgreSQL 18
import   express   from   "  express  "  ;
import   {   Pool   }   from   "  pg  "  ;


const   pool   =   new   Pool  ();
const   app   =   express  ();


interface   OffsetPage  <  T  > {
data  :   T  [];
pagination  :   {
page  :   number  ;
limit  :   number  ;
has_next_page  :   boolean  ;
};
}


function   clampLimit  (  raw  :   unknown  )  :   number   {
const   n   =   Number  (  raw  )   ||   20  ;
return   Math  .  min  (  Math  .  max  (  Math  .  trunc  (  n  ),   1  ),   100  );   // clamp 1–100
}


app  .  get  (  "  /posts  "  ,   async   (  req  ,   res  )   =>   {
const   limit   =   clampLimit  (  req  .  query  .  limit  );
const   page   =   Math  .  max  (  Number  (  req  .  query  .  page  )   ||   1  ,   1  );
const   offset   =   (  page   -   1  )   *   limit  ;


// Fetch limit + 1 to learn has_next_page without a COUNT query
const   {   rows   }   =   await   pool  .  query  (
`  SELECT id, title, created_at
FROM posts
ORDER BY created_at DESC, id DESC
LIMIT $1 OFFSET $2  `  ,
[  limit   +   1  ,   offset  ]
);


const   has_next_page   =   rows  .  length   >   limit  ;
const   data   =   rows  .  slice  (  0  ,   limit  );


const   body  :   OffsetPage  <  typeof   data  [  number  ]>   =   {
data  ,
pagination  :   {   page  ,   limit  ,   has_next_page   },
};
res  .  json  (  body  );
});
```


Two details carry their weight here. The` clampLimit` function caps page size at 100 so a client cannot request` limit=1000000` and exhaust the server. And fetching` limit + 1` rows lets the endpoint report` has_next_page` without a second` COUNT(*)` query — the same trick the cursor implementation uses below. If the extra row exists, there is at least one more page.


## Cursor Pagination: The API Surface


Cursor pagination replaces the numeric offset with an opaque token that marks a fixed position in the result set. The client sends the token back to fetch the next page, and because the token anchors to a row’s actual sort values rather than a count, inserts and deletes elsewhere in the table do not corrupt the window. The trade-off is the loss of random page access: there is no “page 7,” only “the page after this cursor.”


Cursor and keyset are not synonyms, and conflating them is the most common error in pagination writeups. A cursor is the opaque token your API returns to the client, hiding the pagination strategy; keyset is the SQL technique — a compound` WHERE (sort_col, id) < (val, id)` predicate — that makes cursor pagination fast by letting the database use an index seek instead of scanning and discarding rows. You can implement a cursor API with an offset query underneath (don’t — it inherits the deep-offset slowdown) or with a keyset query underneath (do). The cursor is the interface; keyset is the efficient engine.


The token itself is just an encoded position — typically the sort column and a tiebreaker, base64-encoded so clients treat it as opaque and don’t depend on its internal shape. Keep it opaque so you can change what’s inside it later without breaking clients mid-traversal.


```text
// Node 24 (Active LTS), pg 8.22.x, PostgreSQL 18
interface   CursorPage  <  T  > {
data  :   T  [];
pagination  :   {
next_cursor  :   string   |   null  ;
has_more  :   boolean  ;
};
}


type   Cursor   =   {   created_at  :   string  ;   id  :   number   };


function   encodeCursor  (  c  :   Cursor  )  :   string   {
return   Buffer  .  from  (  JSON  .  stringify  (  c  )).  toString  (  "  base64url  "  );
}


function   decodeCursor  (  raw  :   string  )  :   Cursor   {
return   JSON  .  parse  (  Buffer  .  from  (  raw  ,   "  base64url  "  ).  toString  (  "  utf8  "  ));
}


app  .  get  (  "  /feed  "  ,   async   (  req  ,   res  )   =>   {
const   limit   =   clampLimit  (  req  .  query  .  limit  );
const   cursor   =   req  .  query  .  cursor
?   decodeCursor  (  String  (  req  .  query  .  cursor  ))
:   null  ;


// Keyset predicate: compound row comparison on the sort tuple
const   {   rows   }   =   cursor
?   await   pool  .  query  (
`  SELECT id, title, created_at
FROM posts
WHERE (created_at, id) < ($1, $2)
ORDER BY created_at DESC, id DESC
LIMIT $3  `  ,
[  cursor  .  created_at  ,   cursor  .  id  ,   limit   +   1  ]
)
:   await   pool  .  query  (
`  SELECT id, title, created_at
FROM posts
ORDER BY created_at DESC, id DESC
LIMIT $1  `  ,
[  limit   +   1  ]
);


const   has_more   =   rows  .  length   >   limit  ;
const   data   =   rows  .  slice  (  0  ,   limit  );
const   last   =   data  [  data  .  length   -   1  ];


const   body  :   CursorPage  <  typeof   data  [  number  ]>   =   {
data  ,
pagination  :   {
has_more  ,
next_cursor  :
has_more   &&   last
?   encodeCursor  ({   created_at  :   last  .  created_at  ,   id  :   last  .  id   })
:   null  ,
},
};
res  .  json  (  body  );
});
```


The endpoint decodes the cursor into a` (created_at, id)` tuple, runs a keyset query against it, fetches` limit + 1` to compute` has_more` , and encodes the next cursor from the last returned row. No` COUNT(*)` , no offset, no scan of discarded rows.


## Keyset Pagination: The Compound Predicate


Keyset pagination is the database technique that makes cursor pagination fast: instead of an offset, it filters on the sort key itself with a compound predicate. PostgreSQL supports[row value comparisons](https://www.postgresql.org/docs/current/functions-comparisons.html) , so` (created_at, id) < ($1, $2)` compares the tuple lexicographically and lets a matching composite index seek directly to the boundary.


The tiebreaker is not optional.` created_at` alone is not unique — two posts can share a timestamp to the millisecond — and a sort on a non-unique column produces a nondeterministic order, which means a cursor anchored to` created_at` could skip or repeat rows that share that value. Appending the primary key (` id` ) makes the sort tuple unique and the boundary deterministic.


```text
-- PostgreSQL 18
-- Composite index matching the sort tuple, including its direction.
CREATE   INDEX   idx_posts_cursor   ON   posts (created_at   DESC  , id   DESC  );


-- Keyset query: compound predicate seeks into the index.
SELECT   id, title, created_at
FROM   posts
WHERE   (created_at, id)   <   ($  1  , $  2  )
ORDER BY   created_at   DESC  , id   DESC
LIMIT   $  3  ;
```


The index direction matters. Per the PostgreSQL documentation on[indexes and ORDER BY](https://www.postgresql.org/docs/current/indexes-ordering.html) , a B-tree index can be scanned in either direction, but matching the index’s declared order to the query’s` ORDER BY` lets the planner satisfy both the predicate and the sort from a single index scan. Run` EXPLAIN ANALYZE` on both forms against a large table and the structural difference is visible in the plan: the` OFFSET 500000` query shows the server computing and discarding the leading rows, while the keyset query shows an index scan with a low startup cost that returns only the rows it needs. The cost difference is structural, not incidental — it follows directly from the documented` OFFSET` semantics above.


## Page Tokens: Hiding the Strategy


Page tokens are the opaque-strategy variant of cursor pagination, used most cleanly by Google Cloud’s API design guidelines. Under[AIP-158](https://google.aip.dev/158) , the request carries` page_size` and` page_token` , the response returns a` next_page_token` , and the spec requires that page tokens be opaque to the client. Encoding the full pagination state server-side lets the server change its internal strategy — from offset to keyset, for example — without breaking clients that are mid-traversal.


Stripe is frequently cited alongside Google here, but the two are not the same mechanism, and the distinction is worth getting right. Per the[Stripe pagination docs](https://docs.stripe.com/api/pagination) , Stripe’s *list* API uses object-ID cursors (` starting_after` and` ending_before` , with` limit` and a` has_more` flag), not opaque page tokens; its *search* API is the one that returns an opaque` next_page` token. So the precise survey reads: Google Cloud uses opaque page tokens, Stripe’s list endpoints use object-ID cursors,[Shopify](https://shopify.dev/docs/api/usage/pagination-graphql) uses cursor-based connections via its GraphQL Admin API, and[GitHub’s REST API](https://docs.github.com/en/rest/using-the-rest-api/using-pagination-in-the-rest-api) paginates via the` Link` header — page-number-based on most endpoints, with cursor-based (` before` /` after` ) only on specific ones. The common thread is cursor-style traversal, not a single uniform mechanism.


## Relay-Style GraphQL Connections


Relay connections are the GraphQL specialization of cursor pagination, formalized so that every list field in a schema paginates the same way. The[Relay Cursor Connections Specification](https://relay.dev/graphql/connections.htm) defines a connection as a list of` edges` , where each edge has a` node` (the item) and a` cursor` (its opaque position), plus a` pageInfo` object exposing` hasNextPage` ,` hasPreviousPage` ,` startCursor` , and` endCursor` .


```text
query {
posts(  first  :   10  ,   after  :   "  cursor123  "  ) {
edges {
cursor
node { id title }
}
pageInfo {
hasNextPage
endCursor
}
}
}
```


The per-edge cursor is the same opaque token concept from the REST sections, and` pageInfo.hasNextPage` is the same` has_more` flag derived from the fetch-one-extra trick. The structure is heavier than a plain` { data, pagination }` envelope, but it gives clients a uniform contract: paginate any connection by passing` first` /` after` and reading` pageInfo` . Under the hood, a Relay connection should still be backed by a keyset query for the same performance reasons.


## Choosing a Pattern and Avoiding the Common Traps


Default to cursor pagination backed by keyset for any new list endpoint, and reach for offset only when a specific requirement forces it. The decision matrix:


Use case Pattern


Infinite-scroll feed, activity stream, notifications Cursor + keyset


Public list API you may want to re-implement later Page token (opaque)


GraphQL list field Relay connection (keyset underneath)


Bulk export of a large table Cursor + keyset


Admin table with “jump to page N” Offset


Search UI that shows page numbers Offset


Small (<10k rows), rarely-changing reference data Offset (either is fine)


A handful of practices keep all of these correct in production:


- **Cap page size.** Clamp` limit` to a fixed maximum (100 is a common ceiling) so a client cannot request an unbounded page.
- **Index the full sort tuple.** The composite index must match both columns and directions of the` ORDER BY` , or the keyset query falls back to a scan.
- **Prefer` has_next_page` over total counts.** Returning` total_count` on every response forces a separate` COUNT(*)` , whose cost on a large table depends on table bloat and the visibility map; derive` has_next_page` cheaply from the` limit + 1` row instead and reserve total counts for UIs that genuinely require them.
- **Keep cursors opaque.** Encode them so clients can’t depend on the internal format, leaving you free to change the underlying strategy.
- **Reset the cursor on filter or sort change.** A cursor encodes a position in a specific ordered, filtered result set; when the user changes a sort order or applies a new filter, that cursor is invalid for the new query and must be discarded — reset to the first page whenever sort or filter parameters change. In a client, that means tying the cursor state to the filter state so a filter change clears the accumulated pages and refetches.


The shifting-window and deep-offset bugs are easy to miss in testing because they only appear under concurrent writes or at depth, and several of them never reach your error logs. The shifting-window bug is invisible in server logs — the response is a valid 200 with the correct number of items — but in session replay we see it as a specific rendering artifact: an item that was already visible in the viewport renders again in the next batch, immediately adjacent to its previous position. Deep-offset slowdown shows up the same way: not as a timeout or a 4xx, but as a “load more” spinner that never resolves on a slow 200 the client is still waiting for. Because these are interaction-sequence symptoms rather than discrete errors, session replay is often how we surface them — they live in the full sequence of scroll, fetch, and render, not in a single failed request.


## Conclusion


Reach for cursor pagination backed by a keyset query on the next list endpoint you build, and treat offset as the exception you justify rather than the default you inherit — its linear deep-offset cost and shifting-window instability are structural, not tunable. Add the composite index on your sort tuple, return` has_next_page` instead of a total count, and keep the cursor opaque so you can swap the engine underneath without a breaking change.


## FAQs


Can I add cursor pagination to an existing offset-based API without breaking clients?


Yes, but treat it as a new parameter rather than a swap. Accept a cursor query parameter alongside the existing page and limit parameters, and route requests that carry a cursor to the keyset query while leaving page-based requests on the offset path. Keep the cursor opaque so the internal format stays free to change, and document the offset path as deprecated for large or write-heavy endpoints rather than removing it immediately.


What happens to a cursor when the row it points to is deleted?


Nothing breaks, because a keyset cursor encodes sort values rather than a row reference. The compound predicate (created_at, id) < (val, id) selects every row ordered after that boundary tuple, so the next page returns correctly even if the exact anchor row no longer exists. This is the core advantage over offset pagination, where a deletion shifts every subsequent position and causes the client to silently skip an item.


Why use base64-encoded JSON for a cursor instead of just sending the timestamp and ID directly?


Encoding makes the cursor opaque, which signals to clients that the token is not meant to be parsed or constructed by hand. If clients read the raw timestamp and ID, they couple themselves to your internal pagination strategy and break when you change it. An opaque base64 token lets you add fields, switch sort keys, or move from keyset to page tokens without a client-side change. The encoding adds no meaningful overhead.


Does cursor pagination work when sorting by a column other than created_at, like a popularity score?


Yes, provided the cursor encodes the same sort tuple the query orders by and ends with a unique tiebreaker. Sorting by a popularity score requires the predicate and index to cover (score, id) and the cursor to carry both values, since scores collide far more often than timestamps. Without the unique id tiebreaker, rows sharing a score produce a nondeterministic order that can skip or repeat items across pages.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
