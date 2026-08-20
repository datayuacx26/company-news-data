---
schema_version: "1.0.0"
document_id: "3be9244a48a264eb120fd43aca4687bab0c4caf5fde8e6f4e0cbb257abdc3ec4"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/airbyte-oss-to-cloud"
published_at: "2026-07-24T11:24:00+00:00"
first_seen_at: "2026-07-24T23:55:54.462313+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:634df819376c2ad22fd292b7aeedd140507be41153101431646514eecf201aec"
---

# Move from Airbyte OSS to Cloud Without Missing a Sync

One of my favorite experiences at conferences and events is talking to the open-source Airbyte community. Airbyte Core is beloved by the data community, and for good reason. But as with any self-hosted software, there are tradeoffs.


You chose OSS for the control and the zero licensing cost, and it delivered. But that means you also own the upgrades, the infrastructure, the scaling, and the 2am pages when a sync fails. This is why many of our users start exploring our Airbyte Cloud plans. As most of you know, our Cloud plans are pretty much the same as Airbyte Core (OSS). They feature the same connectors and the same sync engine, just managed and scaled by us instead of your on-call rotation. So from a technical perspective, theoretically it seems like an easy shift.


But a lot of teams stop when it actually comes to the migration, and we completely understand why.


Many of our users have put hours of work into their pipelines and infrastructure using Airbyte, and until recently, migration has not been easy. That’s because moving to a new instance has traditionally meant recreating everything by hand and re-syncing from scratch. This is costly both from a warehouse compute perspective as well as engineering hours.


We've built tooling that removes that cost. It migrates your workspace, connections, and sync state from a self-managed instance to Airbyte Cloud, so your first sync in Cloud picks up where your last sync in OSS left off.


## **What the migration preserves**


Three things make a migration painful if you lose them:


1. **Your resource configuration.** Every source, destination, and connection, with its streams, sync modes, and schedules.
2. **Your sync state.** The checkpoints that tell each connection where the last incremental sync ended.
3. **Your existing data.** This is the data already stored in your warehouse, containing your entire business history and context.


The migration tooling handles the first two directly, and because your destinations don't change, the third never moves at all. Your warehouse maintains all existing data, and Airbyte Cloud continues writing directly to it without missing a beat.


## **How it works**


The process runs in five stages, and a member of our Solutions team manages it end to end. You aren't handed a runbook and left alone with it. Your part is approving each stage, adding credentials, and choosing when to cut over.


**1. Export from your existing instance.** We capture connection state (sync checkpoints plus metadata) from your running OSS instance and generate migration-ready Terraform describing all of your resources. You don't need to know Terraform; it's the migration vehicle, and we drive it.


**2. Stand up sources and destinations in Cloud.** We apply the generated Terraform to create your sources and destinations in your Cloud workspace. Secrets never leave your instance, so this is your main hands-on step: you add credentials and test each connector in the UI. This is deliberate, since it confirms every source and destination is healthy before any connection exists.


**3. Create the connections.** A second apply creates your connections in Cloud. They come up inactive and on a manual schedule, so nothing syncs before you're ready. Each new connection is mapped back to its original, which gives you a clear audit trail from old instance to new.


**4. Transfer state and restore settings.** We apply the saved sync checkpoints to the new connections, then restore the original names, schedules, and active status. At this point your Cloud workspace mirrors your OSS instance, checkpoints included.


**5. Validate.** Together we run your syncs and confirm they resume from the correct checkpoint rather than re-syncing all your existing data. Incremental connections continue incrementally.


The connections stay inactive until step four, so you choose the cutover moment. There's no window where two instances are syncing the same connection into the same destination.


## **Why the checkpoint transfer matters**


We all know that re-syncing existing data is slow. For sources with strict rate limits, a full historical re-sync can take longer than the original backfill did, and some APIs can't serve old data at all.


There’s also a cost element. On the destination side, reprocessing months of records means real warehouse spend and, depending on your sync modes, deduplication work to clean up afterward.


Transferring state sidesteps all of it. A connection that was syncing incrementally on your OSS instance on Monday syncs incrementally in Cloud on Tuesday, from the same cursor.


## **We've done this with real workspaces**


This has been built the right way, alongside real customers managing production-grade data infrastructure.


We built this tooling by migrating actual customer instances, with the checkpoints, quirks, and connector variety that real production workspaces accumulate. Those teams are now running in Cloud with their existing data intact, and the stages above are the same ones we ran with them.


## **Who should consider this**


If any of these sound familiar, the migration is worth a conversation:


- You spend more time upgrading and debugging your Airbyte deployment than building with the data it moves.
- Your team is small enough that self-hosting is a tax on your actual roadmap.
- You want capabilities from our Cloud plans, like managed scaling, support SLAs, or predictable capacity-based pricing on Pro, without rebuilding what you have.


The value is real. Based on a[Forrester TEI Study](https://airbyte.com/blog/forrester-tei-report) , teams moving off self-managed data pipelines to managed Airbyte see 239% ROI and value in <6 months.


This isn’t a push to get you off open-source. Airbyte Core stays free and self-managed, and plenty of teams should keep running it. But if the only thing keeping you on OSS is fear of the move, we’re confident our new tooling addresses that concern.


## **Get started**


Reach out to your account team or[contact us](https://airbyte.com/talk-to-sales) , and we'll walk through your instance and plan the cutover with you. Bring your gnarliest workspace… we’re up for the challenge!
