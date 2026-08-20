---
schema_version: "1.0.0"
document_id: "3a20a553bde5399b8595777061ccd20456ab6eb78bdbebb4bb8acabb80d41579"
company_key: "yc-luciq"
company: "Luciq (formerly Instabug)"
source_id: "yc-luciq-news-import-4fda431b7a4e"
canonical_url: "https://www.luciq.ai/blog/clickhouse-s3-write-cost-reduction"
published_at: "2026-08-10T15:21:08.484+00:00"
first_seen_at: "2026-08-10T16:35:36.656622+00:00"
fetched_at: "2026-08-10T16:35:38.651435+00:00"
content_hash: "sha256:c14a5b7197ba78b98c99b3f5b85533f4e1f69fa8b0633597e16be392c36eb73e"
---

# Born Expired: How We Cut ClickHouse's S3 Write Bill by ~92%

You move your cold data to object storage because object storage is cheap. Bytes on[S3](https://aws.amazon.com/pm/serv-s3/?trk=aebc39a1-139c-43bb-8354-211ac811b83a&sc_channel=ps&trk=74170a9b-a5a7-4ce1-bcfe-f169435ed8a4&sc_channel=ps&ef_id=Cj0KCQjw7eXTBhDBARIsAKF-w45hDWjQjStLEajnnwqx9DNCaSUlwSUXeiJg7ca9koE4NvU69f7K9IwaApfhEALw_wcB:G:s&s_kwcid=AL!4422!3!808747120522!e!!g!!amazon%20s3!23846237039!200318289350&gad_campaignid=23846237039&gbraid=0AAAAADjHtp_R6VqQF-G3qp3SK3PoUS6JC&gclid=Cj0KCQjw7eXTBhDBARIsAKF-w45hDWjQjStLEajnnwqx9DNCaSUlwSUXeiJg7ca9koE4NvU69f7K9IwaApfhEALw_wcB) cost a fraction of bytes on a provisioned SSD, the capacity is effectively infinite, and you stop having conversations about volume sizes. That is the whole pitch, and it's a good one.


Then the first full bill arrives, and about 96% of it is PUT requests.


That was us. We run a very large, write-heavy[ClickHouse](https://clickhouse.com/) deployment for[mobile observability telemetry](https://www.luciq.ai/blog/mobile-observability-proactive-detection) , and we'd just finished tiering it across hot local disks and a cold tier on S3. Storage was 4% of the S3 line. Everything else was the *act of writing* - hundreds of millions of requests a day, steady state, forever.


This is how we got there, why late-arriving telemetry was quietly setting fire to our write budget, and how two staged, metadata-only changes took daily PUTs down 59% and then to roughly 8% of where we started. No data was rewritten. Nothing about what we retain changed.


It helps if you already know your way around[MergeTree](https://clickhouse.com/docs/reference/engines/table-engines/mergetree-family/mergetree) (parts, merges, TTL) and if you remember that S3 charges per request, not just per byte. That second one turns out to be the whole story.


## We didn't move to S3 to save money. We ran out of disk.


Our workload is a firehose: on the order of a million rows a second, background merges shoveling multiple gigabytes per second of uncompressed data, and tens of terabytes of compressed data on each replica. Everything runs on` ReplicatedMergeTree` : one shard, two replicas, both taking writes and replicating from each other. One rule was non-negotiable from day one: every table stays` ReplicatedMergeTree` . No engine swaps, no clever rewrites that fork us from upstream.


The first wall we hit was almost embarrassingly physical. One replica's data no longer fit on one disk.


Every EBS volume type stops at the same place: 64 TiB.` gp3` got there in September 2025, when AWS raised the ceiling from 16 TiB and pushed the performance caps to 80,000 IOPS and 2,000 MiB/s along the way;` io2 Block Express` has been at 64 TiB for years. Different volume types, same wall,. and it isn't a soft quota you can email support about. It's the largest single EBS volume that exists. Past it, your only move is striping several volumes together on every node, forever, with all the operational awkwardness that brings.


Disk size was only the first wall, though. Two more were waiting behind it.


A single multi-gigabyte-per-second merge has to read its inputs *and* write its output, so it drives roughly double its own throughput in disk traffic enough to strain what one node's volumes can sustain. And merges are fundamentally hostile to object storage. S3 has no in-place append, so "add a little to a large part" becomes "download the whole part, re-upload the whole part." Point a merge-active dataset straight at S3 and you get merge backlog, then` TOO_MANY_PARTS` , then throttled inserts.


Which more or less wrote the conclusion for us. We didn't need to move *everything* to S3. We needed to move only the data that had stopped changing. Keep recent data and every merge on fast local disk; let stable, fully-merged data age out to cheap, effectively infinite object storage.


*Figure 1: One shard, two replicas - both take writes, both tier to their own S3 prefix.*


The shape we settled on is a native ClickHouse` s3` disk for the cold tier (not a FUSE mount like` s3fs` , which quietly breaks MergeTree's assumptions about atomic renames and hardlinks) wrapped in a local` cache` disk, with the disk's small pointer files on their own dedicated EBS volume. A storage policy with two volumes,` hot` (the existing local` default` disk) and` cold` (the S3-backed one), and a` TTL … TO VOLUME 'cold'` rule to age parts across the line. Because the new policy is a *superset* of the old one, the hot volume reuses the` default` disk by name: an existing table can adopt it with two ALTERs and no rebuild. That detail is worth stealing.


One more decision shaped everything that followed: ***we left zero-copy replication off*** . Each replica writes its own cold parts under its own S3 prefix, so cold data exists as two fully independent physical copies. That's a deliberate durability choice - a corrupt replica can't poison its twin - but it also means every cold write happens twice. Hold onto that. It becomes the floor we eventually run into.


## We built the whole thing on hardware we planned to destroy


Before any of this touched production, we stood the entire design up in a disposable playground account and then tore it down to nothing. A real cluster: one shard, two replicas across two availability zones, three Keeper nodes for quorum, hot EBS plus the cold s3-in-cache disk, metadata on its own volume.


We created those instances by hand rather than through our usual infrastructure-as-code, which felt wrong right up until it didn't. The whole point of the exercise was to *destroy* nodes and reattach volumes, and a desired-state reconciler fights you at every step of that.


We pushed hundreds of millions of rows through it to answer three questions: does the tiering actually behave, where's the performance ceiling, and can we lose a node without re-downloading the entire cold tier from S3.


The most useful finding was a disappointment, in the best way.


*Figure 2: The ingest ceiling is batch size and disk bandwidth, not vCPUs.*


We assumed a bigger box would buy more throughput. It didn't. Doubling the instance size bought about 22% more ingest for roughly twice the price, while CPU sat at 11.5%. The machine was never the bottleneck. Turning` background_pool_size` up from 48 to 96 to 144 changed nothing at all; the scheduler kept launching the same ten-to-twenty-five concurrent merges no matter what we told it.


What actually moved the needle was more prosaic. How many partitions each INSERT touched: fewer partitions per insert meant fewer, larger merges and far higher throughput. And how big each INSERT batch was: going from ~150k to 1M to 2M rows per insert lifted cluster ingest by about two-thirds, until a single disk's bandwidth became the new ceiling.


So the lesson we carried into production was that ingest and merge throughput are governed by partition fan-out, batch size, and raw disk bandwidth, none of which the storage backend changes. Tiering to S3 is a cost-and-capacity play, never a performance one. We said that out loud, repeatedly, so nobody went in expecting faster queries.


Then came the part that actually decides whether you sleep at night: disaster recovery.


The cold tier's durability rests entirely on those little local pointer files. Each cold part is a few hundred bytes of local metadata pointing at its S3 objects. Protect the pointers and losing a node is a minutes-long metadata reattach instead of a days-long re-download of tens of terabytes. We rehearsed it four ways:


- **Kill an instance, keep its volumes.** Reattach, restart with the same replica identity, and cold data serves immediately with S3 GET traffic sitting at baseline. Nothing re-downloaded.
- **Lose the node *and* its metadata volume.** Restore the pointers from an` ALTER … FREEZE` plus an EBS snapshot. Again, no data comes back over the wire.
- **Lose the Keeper state.**` SYSTEM RESTORE REPLICA` rebuilds it from the local parts, cold pointers included.
- **Snapshot the metadata *without*` FREEZE` , while merges are running.** This is the negative test, and it earned its place in the runbook. Some of the S3 objects your snapshot references get garbage-collected out from under you once the source parts age past` old_parts_lifetime` (480 seconds by default), leaving broken parts on restore.` FREEZE` adds an extra hardlink that pins those objects.


So` FREEZE` isn't advice, it's a requirement, and now we had proof instead of a hunch. The design held, and we shipped it.


## The bill was the writes, not the bytes


Tiering went live. The bytes moved to S3 exactly as designed. And then the first full bill arrived and stopped us cold.


Storage ,the thing we had just spent all this effort moving to cheaper ground was about 4% of the cost. The other 96% was PUT requests, running steadily at hundreds of millions a day. This wasn't a backfill tail that would fade. It was steady-state churn. We were paying, over and over, to write the same cold data.


Three mechanisms were stacked on top of each other, and it took us a while to see all three.


A ClickHouse part is not one file. In the Wide format, every column gets its own data and marks files, so a part on one of our 30-to-57-column tables lands as more than a hundred separate S3 objects, and every object is its own PUT. In the Compact format, all columns share a single data file, so the same part is about a dozen objects regardless of how many columns it has.


*Figure 3: Part format decides how many S3 objects, and therefore how many PUTs, one part becomes.*


The setting that decides which one you get,` min_bytes_for_wide_part` , defaults to 10 MiB, and it measures *uncompressed* bytes. Our aggregate tables compress astonishingly well, on the order of 70–100×, so a part holding a mere 15 MB uncompressed (a few megabytes on disk) had already sailed past the 10 MiB line and was being written Wide, at over a hundred objects apiece. The single largest slice of our write bill turned out to be hundreds of thousands of these small Wide merges every day. Each one small enough to be trivial, and Wide enough to be expensive.


The second mechanism is the one the title is named after, and it's the one that took longest to believe.


**Our move-TTL was keyed on event time.** The tables were partitioned by` toYearWeek(event_time)` and aged to cold with` (event_time) + N days TO VOLUME 'cold'` . That's the obvious, natural choice. It's also a trap when your data arrives late, and mobile telemetry arrives *extremely* late. Devices batch events, go offline, come back days later, retry. On any given day we ingest events dated across the entire multi-week retention window.


*Figure 4: Event-time TTL makes late data born expired; insert-time TTL gives it a hot dwell.*


Follow a single late row. Its event happened three weeks ago, but it arrives *now* . Its move boundary -` event_time + N days` - is already weeks in the past. The instant it lands, it's eligible to move to cold. It is, in the most literal sense, born expired.


So it gets essentially no time to merge on hot disk. The background mover grabs it almost immediately and ships it to S3 as a tiny part, often just a few kilobytes, which then has to merge into that old partition's large existing cold part, a full rewrite of a big object-storage part, triggered by a few kilobytes of late data. Across a normal window this was happening to *every* historical partition at once. Millions of tiny moves and cascading cold merges per day, all of it pure waste.


And the third mechanism is the one we chose on purpose and now had to pay for. With zero-copy replication off, both replicas do all of the above independently, into their own prefixes. Every wasteful write, doubled.


When we finally mapped PUTs to tables, the picture had a clean and slightly maddening symmetry. The two enormous raw tables were about 70% of our cold *storage* but only about 16% of the *writes* . The small aggregate and materialized-view tables were the exact mirror image. And a long tail of genuinely tiny tables - dozens of them, a few hundred gigabytes combined - was generating something like 41% of the entire PUT bill. One 171 MiB table was quietly emitting tens of thousands of PUTs an hour. It never belonged on object storage in the first place.


## Read the mechanism, not the marketing


We gave every candidate fix the same treatment: go read how it actually works, in the ClickHouse source or the docs, and accept or reject it on that mechanism rather than on how good it sounded in a meeting. That discipline killed most of our favorites.


← Scroll to see more →


Lever Mechanism Verdict


**Raise` min_bytes_for_wide_part`** Compact parts → ~12 objects instead of ~110+. Applied


**Un-tier the tiny tables** A few hundred GiB held ~41% of the PUT bill. Applied


**Hot-dwell TTL on` created_at`** Late data dwells, then moves once and fat. Applied


**` prefer_not_to_merge` +` OPTIMIZE`** Write-once cold tier; needs isolated replicas. Needs upgrade


**Lower` max_bytes_to_merge`** Touches a few dozen merges/day; moves are clones. Rejected


**Packed / single-object parts** 31.3 → 2.22 PUTs per insert, but Cloud-only. Unavailable


**Zero-copy replication** Would remove the 2×, but not production-ready. Rejected


**1 GiB Compact (Cloud parity)** S3 selective-read over-read scales with part size. Tuned to 256 MiB


*Every lever was accepted or rejected on its mechanism, not on how it sounded.*


‍
The first keeper was forcing Compact parts by raising` min_bytes_for_wide_part` . Reclassifying those churning small merges from a hundred-plus objects down to about a dozen is close to a 9× cut on the biggest single bucket of writes.


Two details matter here. Apply it as a replicated table-level setting so both replicas agree on the format: a per-node override lets the two replicas disagree and turns cheap local merges into expensive cross-replica fetches. And resist the tempting version of this: ClickHouse Cloud compiles in a 1 GiB threshold, and copying that felt like free parity. But a careful read of the read path shows Compact parts pay a penalty on *selective* reads over S3. When you want a few columns out of many, ClickHouse ends up reading through the gaps rather than issuing precise ranged GETs, and that over-read grows with part size. With a cold cache covering well under 1% of the cold tier, we'd have felt it. So we deliberately chose 256 MiB, not 1 GiB: large enough to sweep up ~99% of the churning small merges as Compact, small enough to leave genuinely large, frequently-read parts as Wide, where the read pattern prefers them.


The second keeper was simply un-tiering the tiny tables, dropping the move-TTL so their new parts stay on local disk and their existing cold parts drain back for free as they merge. Safe, because none of those tables had a *delete* -TTL; the move-to-cold rule was the only TTL they had.


Several attractive ideas died on their mechanics:


- **Lowering` max_bytes_to_merge_at_max_space_in_pool`** sounds relevant, but it caps *compressed* merge input size, and our merges were already overwhelmingly tiny. It would have touched a few dozen merges a day and done nothing at all for the moves, which are clones rather than merges.
- **Packed parts** - a whole part bundled into a single S3 object - would have been transformative. ClickHouse's own numbers put it at 31.3 PUTs per insert dropping to 2.22. The gating settings,` min_bytes_for_full_part_storage` and` min_rows_for_full_part_storage` , *do* exist in open-source builds: they parse, they apply, and they do nothing. The implementation is Cloud-only. The docstring in the source says so in as many words "Only available in ClickHouse Cloud", and in open-source ClickHouse the part-storage factory only ever builds Full storage. The setting is real; the feature isn't.
- **Zero-copy replication** would have collapsed our 2× write amplification to 1× by sharing a single prefix. Its own documentation says plainly that it is not ready for production, it has been off by default since 22.8, and the issue tracker carries a run of data-loss and data-duplication reports filed against it. Re-seeding tens of terabytes to adopt that, and giving up per-replica physical redundancy in the process, was an easy no.


Everything we did apply was a metadata-only ALTER. No data rewritten, no topology touched. The Wide share of our cold merges fell from about 34% to about 1.5%, and daily PUTs dropped about 59%, confirmed on the first full day, with storage and retention untouched.


Two things from that rollout are worth passing on.` min_bytes_for_wide_part` lives in the` <merge_tree>` config block and does *not* hot-reload, so for a while the file had the new value while the server happily kept using the old one. And an out-of-memory scare mid-rollout turned out to be pre-existing memory pressure from concurrent large merges, not the tiering change at all (worth saying because the obvious assumption was wrong, and we nearly rolled back a change that was working).


## Move the clock, not the data


Round one cleared the cheap wins. What remained - still over a hundred million PUTs a day - was almost entirely the born-expired flood.


You can't out-tune a flood. You have to turn off the tap. And the tap here was the decision to key the TTL on event time.


So we changed what the clock measures. Instead of aging a row N days after the event happened, we age it N days after it was *inserted* :


```text
-- before: late data is born expired and ships to S3 immediately
ALTER     TABLE   t MODIFY TTL toDate(event_time)   +     INTERVAL     7     DAY     TO   VOLUME   'cold'  ;


-- after: every row gets a full 7-day dwell on hot disk, however old the event is
ALTER     TABLE   t MODIFY TTL toDate(created_at)   +     INTERVAL     7     DAY     TO   VOLUME   'cold'
SETTINGS materialize_ttl_after_modify   =     0  ;
```


Now a late-arriving row spends a full seven days on hot disk regardless of its event date. It merges into a big, fat part on local storage - where merges are free - and moves to S3 exactly once, already large, instead of arriving as a tiny part and forcing a large cold rewrite.` materialize_ttl_after_modify = 0` keeps the ALTER a pure metadata change: existing parts keep their current schedule, only new parts follow the new rule. Instant, and reversible.


There was one genuinely fiddly bit. The aggregate tables are fed by materialized views and had no ingest-time column to key on. We added one, but typed it` SimpleAggregateFunction(max, DateTime)` rather than a plain` DateTime` , so that when summing merges collapse rows together, the surviving row keeps the *newest* insert time. A plain column would collapse to some arbitrary value and could hand a row an earlier timestamp than it deserved, shortening its dwell and letting a little of the born-expired behaviour leak back in. The` max` aggregate keeps the dwell floor honest.


We flipped it on one high-churn table first and watched for four days.


*Figure 5: Pilot table - cold moves collapsed to zero the day the TTL flipped.*


Cold moves on that table went from about 445,000 a day to zero, and stayed there. Cold merges on the same table fell from around 29,000 a day to roughly 30. The total data held didn't change at all: this is a retention-neutral change, just moving where the seven-day line falls. A little more data lives on hot disk, a little less on S3, and nothing is gained or lost from what we keep.


Rolling it across the fleet came with exactly one real risk: hot-disk headroom, since holding late data on local disk for a week adds to hot usage. So we measured the late-data inflow directly rather than guessing, and it came out at around 10% of daily ingest by bytes.


That number is the whole point in miniature. Born-expired data is enormous in part *count* and trivial in *bytes* , which is precisely why it was a request problem and not a storage problem. The extra hot footprint fit comfortably in free space, so no volume needed growing.


We still staged it carefully, batching the small tables, doing the big ones one at a time while watching disk fill, and nudging` move_factor` from 0.2 down to 0.1 (which, unlike the merge-tree settings, isn't restart-gated) so the space-based eviction safety valve triggers later and doesn't undercut the dwell we just bought.


*Figure 6: Daily S3 PUTs as a share of baseline, across the two staged rounds.*


Projected across the fleet, that takes us from hundreds of millions of PUTs a day to the low tens of millions — roughly a 92% reduction, and the point at which tiering to S3 comfortably beats staying on block storage. Which is where we wanted to be all along.


## What we traded, and what's still open


None of this crossed one hard floor: every cold write still happens twice, once per replica prefix, because we chose durability over zero-copy. Only a shared-storage engine or zero-copy replication removes that, and we rejected both. Everything above lives *inside* that 2× floor.


The cleanest end state we can see - a genuinely write-once cold tier, using` prefer_not_to_merge` on the cold volume plus a scheduled bulk` OPTIMIZE` to keep parts Wide and stop cold merges entirely - is now within reach thanks to a topology change. But it depends on a fix in a newer ClickHouse release, and it's only safe *after* the ingest-time TTL change is in place. Otherwise a non-merging cold volume just accumulates parts until inserts throttle. That's the next chapter.


And the benchmark's verdict is worth repeating for anyone considering this path. Tiering is a cost-and-capacity decision, not a performance one. If you're hoping S3 makes your queries or your ingest faster, it won't; the throughput ceiling is partition fan-out, batch size, and disk bandwidth, and it doesn't care where the cold bytes live.


## On object storage, count requests, not bytes


Our instinct was to optimise the size of what we stored. The entire bill turned out to be the *act* of writing it — driven by part format, part count, and a TTL clock that was measuring the wrong thing.


The fixes that worked were the boring, reversible, metadata-only ones. The glamorous ones — packed parts, zero-copy, matching the cloud's settings, all fell apart the moment we read how they actually work.


So if you're about to tier a write-heavy MergeTree to S3: before you tune anything, go and look at what fraction of your parts are Wide, and go and look at what your move-TTL is actually measuring. Those two answers were worth more to us than every clever idea we had.


### What's Next


This piece is the first in a series on what we learned running ClickHouse cold storage on S3 at[mobile-observability scale](https://www.luciq.ai/platform) . We are still building here, and more from this project is on the way.


‍


*This peice is written against ClickHouse 25.8 on self-managed EC2. Settings referenced: min_bytes_for_wide_part (256 MiB), materialize_ttl_after_modify, move_factor (0.1), old_parts_lifetime, perform_ttl_move_on_insert, background_pool_size. Further reading: ClickHouse docs on*[hot/warm/cold with TTL](https://clickhouse.com/docs/guides/developer/ttl) *,*[separation of storage and compute](https://clickhouse.com/docs/guides/separation-storage-compute) *,*[MergeTree settings](https://clickhouse.com/docs/operations/settings/merge-tree-settings) *and*[part types and storage formats](https://clickhouse.com/docs/knowledgebase/understanding-part-types-and-storage-formats) *;*[AWS's September 2025 gp3 announcement](https://aws.amazon.com/about-aws/whats-new/2025/09/amazon-ebs-size-provisioned-performance-gp3-volumes) *and*[EBS volume constraints](https://docs.aws.amazon.com/ebs/latest/userguide/volume_constraints.html) *; and*[S3 request pricing](https://aws.amazon.com/s3/pricing/) *.*
