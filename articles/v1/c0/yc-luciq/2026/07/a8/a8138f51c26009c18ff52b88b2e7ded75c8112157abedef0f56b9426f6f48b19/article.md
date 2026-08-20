---
schema_version: "1.0.0"
document_id: "a8138f51c26009c18ff52b88b2e7ded75c8112157abedef0f56b9426f6f48b19"
company_key: "yc-luciq"
company: "Luciq (formerly Instabug)"
source_id: "yc-luciq-news-import-4fda431b7a4e"
canonical_url: "https://www.luciq.ai/blog/grafana-flamegraph-hierarchical-metrics"
published_at: "2026-07-22T06:55:35.100+00:00"
first_seen_at: "2026-07-24T03:18:24.794445+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:582c50eb829e448a744a1a74cb9da5d1672273f3622d8900024f2b27d78abb67"
---

# Stop Scanning 400-Row Tables: The Flamegraph Hiding in Your Metrics

The dashboard is telling you something is wrong. Traffic to one region is up. Error counts are climbing somewhere you can't see yet, and a cluster is filling toward its limit. You open the breakdown to find the culprit, and you get what you always get: a table. Four hundred rows of region, service, endpoint, and a number. Sorted by that number, if you're lucky.


So you start scanning. Row by row, holding the shape of the tree in your head, trying to remember whether checkout in us-east was the big one or whether that was search in eu-west. The information is all there.


That table hides something: your metric isn't flat, it's a tree. And there's a visualization built for trees that turns " *scan four hundred rows* " into " *look at the widest bar* ." The busiest path becomes the widest thing on the screen; the workload quietly eating your cluster becomes impossible to miss. It's called a flamegraph. You've probably only ever seen one profiling CPU.


## A table is the wrong shape for a hierarchy


Flamegraphs might be the best data visualization nobody lets you use. Profilers worked this out a long time ago: stack a set of nested bars, size each one by its total, and your eye goes straight to the widest. No reading. No scanning.


But you have hierarchy all over your metrics. Request traffic by region → service → endpoint. Storage by cluster → namespace → volume. Memory by zone → nodepool → namespace → workload. Anything with a natural nesting is a flamegraph waiting to happen, and you're staring at it as a 400-row table instead.


## The flamegraph panel doesn't care where the data comes from


Rendering a flamegraph doesn't need a plugin. Grafana has shipped a real, interactive flamegraph panel in core since v10.1, and we ran this exact setup on 10.1, 11.1, 12.4, and 13.1 - the same frame renders on every one. It was built for Pyroscope, but the panel itself is indifferent to the source: feed it any datasource frame that matches its contract and it renders a zoomable, click-to-focus flamegraph.


So you can point it at a plain PromQL query over a hierarchical metric and get the real thing. Here's how, including the handful of things that will cost you an afternoon if nobody warns you.


## What you want to see


Say you have` http_requests_total` with three hierarchy labels:


‍` region = us-east | eu-west | ap-south`


` service = checkout | search | profile | ...`


` endpoint = /cart | /pay | /query | ...`


You want the root to be all traffic, then a level per label: region, then service, then endpoint. Click the widest bar and everything outside that subtree falls away. The recipe is the same for any hierarchy; the version we run is four levels deep.


## The contract the panel actually enforces


The panel reads exactly one table frame, data.series\[0\], and it matches fields by field.name, not display name. Hold onto that; it's the source of the most infuriating error below.


It wants four fields:


← Scroll to see more →


Field Type Meaning


**level** number Depth in the hierarchy (0 = root).


**label** string The node name shown on the bar.


**value** number Total for this node *and* its subtree.


**self** number Amount attributed to this node alone.


‍


The one non-obvious rule: rows must be in depth-first pre-order (nested-set order), every node immediately followed by its entire subtree. That falls out of the paths themselves: each non-root node's path is a strict prefix of its descendants', so sorting by path reproduces the order. Get the order wrong and the bars come out as nonsense widths even though every number in the table is correct, which is the worst kind of bug to chase.


Good news on` self:` set it to` 0` everywhere. The flame widths are driven entirely by` value` , so unless you want the "self time" shading,` self = 0` is fine.


## Building the frame from a hierarchical metric


Two halves: a query that emits the right rows, and a transform chain that shapes them into the frame. Start with the query.


You emit one instant,` format=table` query that` or` -unions a root row with one row-set per level. Each term produces just three columns,` {level, label, path}` , using nothing but stock PromQL:` label_replace` and` label_join` .


The one function you'll reach for that PromQL doesn't obviously have is "set a label to a constant." The idiom is` label_replace(v, "dst", "constant", "", "")` : an empty source label and empty regex match every series, so it stamps` dst = "constant"` on all of them. That's the pure-PromQL stand-in for MetricsQL's` label_set` , and it works right down to setting the control-byte sentinel (<SEP> below, the empty-value guard explained in trap #3):


‍


The middle levels are the same shape, summing by region alone, then by` region, service` . Wrapping each term in` sum by(level,label,path)(...)` is doing real work: it throws away every other label, so the frame comes back with only those three columns instead of dragging the raw dimensions along.


Now the transforms. All built-in:


` labelsToFields`


` → merge`


` → convertFieldType(level → number)`


` → calculateField(value = Value * 1) # creates a field literally named "value"`


` → calculateField(self = Value - Value) # self = 0; widths come from value`


` → sortBy(path asc) # this IS the depth-first ordering`


` → organize(exclude Time, Value, path)`


That's the whole thing. The` sortBy(path asc)` step is the entire depth-first ordering trick; trap #4 explains why the separator matters.


One note if you're on VictoriaMetrics: MetricsQL gives you` label_set(v, "dst", "constant")` as a tidier shorthand for that` label_replace(..., "", "")` dance. The query above stays in stock PromQL on purpose, so it runs unchanged on plain Prometheus, and both forms produce byte-for-byte the same frame.


That's the technique. The rest is knowing where it breaks.


## The six traps that fail silently


The chart looks fine until it isn't. They're in the order they'll bite you, and the first two are the killers.


**1.` organize's` rename doesn't do what you think.** It sets config.displayName, not field.name, and the panel matches on field.name. So you rename your columns to level/label/value/self in organize, the table preview looks perfect, and the panel still throws "Data is missing fields." The fix is baked into the recipe: name level/label/path in the query, and mint value/self with calculateField, which (unlike organize) sets a real field.name.


**2. An instant format=table query returns one frame per series.** This is the big one. Those level/label/path values don't come back as table columns; they ride as labels on the value field of each single-row frame. The panel only reads series\[0\], so without labelsToFields → merge up front it sees one lonely series and gives up. Don't trust a glance at Explore or a Table panel to tell you what the flamegraph sees: what you're looking at there is the response *after* Grafana's frontend has reshaped it into a table, and the panel works off the raw shape. The verify step below shows you that raw shape.


**3. Prometheus and VictoriaMetrics both drop empty label values.** So your carefully built root row with path="" evaporates, and any node with an empty dimension goes with it. Now you have frames with mismatched schemas, and merge quietly produces garbage. Two fixes, both in the recipe: make the root path a sentinel byte (we use \\x1f, ASCII unit separator), and map empty nodes to (untagged) with label_replace(v, "label", "(untagged)", "label", ""). The sentinel survives the round trip (it comes back JSON-encoded as ), which is the point: it's non-empty, so it isn't dropped.


**4. Depth-first order means sorting by path ascending.** Using \\x1f as the separator between path segments is deliberate: it sorts below every letter, digit, and dash, so no sibling's subtree can interleave with another's, and the root row (whose path is the lone sentinel) sorts first. This is also why a "nice" separator like / or . will bite you the moment an identifier contains one.


**5. You can't persist "full-width, flame-only, and clickable" all at once.** The showFlameGraphOnly option gives the flame the whole width but kills click-to-focus, and it's undocumented: no control in the panel editor, settable only in the dashboard JSON. The live in-panel view toggle ("Top Table / Flame Graph / Both") is a different switch. Flip it to "Flame Graph" and you keep the full-width flame *and* click-to-zoom, but that choice isn't saved. (The gate is {!showFlameGraphOnly} in the flamegraph component source; there's no persisted selectedView option. In the default "Both" mode the table sits beside the flame past ~800px.)


**6. Whether the panel respects your unit depends on the Grafana version.** This is the one place the versions diverge. On 10.1 and 11.1, the flamegraph ignores the field's unit entirely: it labels everything Count and shows the raw number ("1.98 Tri samples"), so a memory chart reads as if you're counting samples. Unit support landed later; by 12.4 and 13.1 it honors the field, so set the unit to bytes and the flame and its table format as TiB/MiB (with 0 B selves). Everything else is identical across all four versions: the frame contract, the transforms, the DFS ordering, the sentinel.


## How to verify without guessing


The transforms run **client-side** , which has an annoying consequence: you can't validate the final frame by hitting the datasource API, because the API only ever hands you the raw series, never the post-transform result.


What you can do, and what we'd recommend as step one every time, is POST your query to /api/ds/query, the exact endpoint the panel uses, and read the frames it returns. That tells you the two things that matter. You'll see the one-frame-per-series shape from trap #2, with level/label/path sitting in each value field's labels. And you'll see there's no value or self column yet, so you know exactly which transforms you still owe the panel. It turns "why is it erroring" into something you can just look at.


Since that's the pre-transform view, confirm the final result by rendering the panel and looking at it. If you want a proper unit test of the transform chain for CI, replay it headlessly with @grafana/data: register the standard transformers, run transformDataFrame, and assert on the field names the panel checks. But for day-to-day work, "inspect the raw frame, then render it" catches everything.


## Where to take it next


You can push the hierarchy out of the query and make it configurable from the dashboard: enumerate your dimension names into a query variable per level, give each level an exclusion regex that references the ones above it, and Grafana re-evaluates the chain whenever an upstream variable changes. One dashboard then gives you any ordering on the fly, region → service today and service → region tomorrow. Treat that as a direction, not a validated recipe; the core technique is everything above it.


## The widest bar is always the next thing to look at


A hierarchy in a table is something nobody reads. As a flamegraph, the same numbers answer your question before you've finished scrolling.


No plugin required: just the frame contract, about six transforms, and the six traps above, enough to skip the afternoon this cost us.


So the next time the dashboard goes red, the row that matters won't be hiding on line two hundred of a table. It's the first bar you see.
