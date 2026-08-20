---
schema_version: "1.0.0"
document_id: "2872309b3a7f818f2b6c0938650b0b2c28eeb91d934d3efdc4446106902dd8f4"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/two-way-sync-mariadb-hubspot"
published_at: "2026-07-22T12:00:00+00:00"
first_seen_at: "2026-07-22T16:41:29.064477+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:a097ce5ddc99d04b052e529aee760475ba1826ad36cd3112ca3514f483633aff"
---

# MariaDB and HubSpot: Both Sides Writing Without Overwrites

MariaDB usually holds the truth about what a customer is doing: the account row, the plan, the seats used, whether the last invoice cleared. HubSpot holds the truth about the relationship: deal stage, owner, what was said on the last call. Both are correct, and both keep changing.


Connect them with a nightly export and one of those truths starts losing. A push out of MariaDB flattens the lifecycle stage a rep set that morning. A HubSpot workflow writes a value the application never sees. Two-way sync is how each side stays authoritative over the fields it should own, without either one overwriting the other.


This guide covers what genuinely two-way means for an application database and a CRM, how to read changes out of MariaDB, how to write into HubSpot without tripping its limits, and how ownership, conflicts, and echo prevention get configured. For the platform view, read the guide to an[enterprise iPaaS for MariaDB](https://www.stacksync.com/blog/enterprise-ipaas-mariadb) ; the[HubSpot two-way sync guide](https://www.stacksync.com/blog/hubspot-two-way-sync-the-complete-guide-to-bidirectional-integration) covers the theory.


## What does two-way sync between MariaDB and HubSpot actually mean?


It means both systems can write, and the integration knows which one is allowed to win for each field. That is the whole definition, and it rules out most of what gets sold as two-way sync.


Two scheduled jobs pointed at each other do not qualify. Job A pushes MariaDB rows into HubSpot at 02:00, job B pulls HubSpot contacts back at 03:00, and neither knows the other exists. When the same field moved on both sides, the value that survives is whichever job ran last. That is a cron schedule, not a policy.


Ownership is set per field, not per table. That is what makes both directions safe.


Real two-way sync has three properties. It detects change at the field level, so a rep editing the record owner and a billing job updating revenue on the same contact do not clobber each other. It carries an ownership rule per field rather than per object. And it tags the origin of every write, so a value the engine just pushed into HubSpot is not read back as a fresh HubSpot change. Skip the third and one edit becomes a write loop that burns a day of API quota.


## How do you capture MariaDB changes without polling the database?


Read the binary log. MariaDB writes every committed change to its binlog, and an engine that reads it sees inserts, updates, and deletes in commit order, within milliseconds, without running a single query against your production tables.


Four settings decide whether that works.` log_bin` has to be on.` binlog_format` has to be` ROW` , worth checking rather than assuming, because MariaDB has historically shipped with` MIXED` and statement entries cannot be turned back into row changes.` binlog_row_image` should be` FULL` so the engine sees before and after values for every column. And retention,` binlog_expire_logs_seconds` on 10.6 and later, has to survive a weekend outage. The replication user needs only` REPLICATION SLAVE` and` REPLICATION CLIENT` .


MariaDB is not MySQL with a different name here. It writes GTIDs as` domain_id-server_id-sequence` , not MySQL's UUID and transaction number, so a change data capture connector tested only against MySQL 8 can attach to MariaDB and still lose its position after a failover. MariaDB also carries binlog options MySQL does not, such as` binlog_annotate_row_events` , which stores the originating statement beside the row events and makes mapping bugs quick to trace.


Topology matters too. On Galera, read from a node with both` log_bin` and` log_slave_updates` enabled, or the local binlog holds only that node's own writes and you quietly miss what the other nodes committed. Pointing the sync at a replica instead of the primary keeps load off the write path, and needs the same setting.


Before the stream there is the backfill. Load existing rows once from a consistent snapshot, record the binlog position or GTID it corresponds to, then stream from exactly there. Skipping that handshake gives you duplicates or a silent gap, and the gap is the one you find months later. The[MySQL two-way sync configuration guide](https://www.stacksync.com/blog/mysql-two-way-sync-configuration-fast-real-time-guide) walks the same handoff.


## How do you write into HubSpot without tripping the rate limits?


Batch the writes, key them on a unique property, and respect the two ceilings that actually bite: the per-app request limit and the much lower, separate limit on the search endpoint.


HubSpot's CRM API allows on the order of 100 requests per 10 seconds on Free and Starter portals and around 190 per 10 seconds on Professional and Enterprise, plus a daily cap. Check your own portal, because the numbers move. The shape does not: a row-by-row writer syncing 50,000 MariaDB accounts spends its day in` 429` backoff, while the batch endpoints accept up to 100 records per call and turn that job into 500 requests.


Key every write on something stable. HubSpot's batch upsert endpoint accepts an` idProperty` parameter, so you can create a unique custom property such as` mariadb_account_id` and upsert against it. That removes the read-then-write round trip and the dependency on email as a dedupe key. Store the returned` hs_object_id` back in MariaDB and both directions share a stable join key.


Keep the search endpoint out of hot paths. It is throttled far more tightly than the object endpoints and caps a single result set well below a full table pass. If you are searching HubSpot to check whether a record exists, the external id property is missing.


In the other direction, webhooks beat polling. HubSpot pushes creation, deletion, and property change events to your endpoint, batched, with retries spread over a multi-day window. Polling the last modified date still earns a place as a nightly reconciliation sweep, but it spends quota continuously and its floor is the poll interval. What matters here is that each event carries a source id and a change source, so you can drop the events your own integration caused.


## Who owns which field, and what happens when both sides change?


Decide ownership field by field before you turn anything on. Most fields have an obvious owner, and writing that list down is most of the design work.


MariaDB should own what the product computes: plan, seat count, usage over the last 30 days, trial end date, billing status. HubSpot should own what people maintain: lifecycle stage, record owner, marketing consent, notes. A small set is genuinely shared, usually company name, primary email, and address, and those are the only ones needing a tie-break rule.


For shared fields, last write wins is the default and it is fine, as long as you are honest about what last means. Do not compare a MariaDB` updated_at` against HubSpot's last modified timestamp and expect a meaningful answer: they come from different clocks, and HubSpot bumps its modified date for reasons unrelated to anyone editing the record. Safer is source of truth per field, with last write wins kept for the few fields where either side is equally trustworthy.


The states one record moves through on a full round trip, including the echo the engine has to suppress.


Echo prevention is what keeps the loop stable. Every write the engine makes into HubSpot has to be recognizable as its own when the webhook comes back, either from the source id on the event or from a short suppression window keyed on the record and field. Without it, one edit becomes an endless ping-pong. A related failure mode is cascading writes, where a database trigger or a CRM workflow fires on the synced value and produces a second change; see[cascading changes and best practices](https://www.stacksync.com/blog/mysql-cascading-changes-and-best-practices) .


Deletes, associations, and custom objects each need their own decision. A` DELETE` in MariaDB rarely should hard-delete a HubSpot record: HubSpot's delete moves it to a recycle bin where it stays restorable for a limited window, while a GDPR erasure is permanent. Map a database delete to a soft flag or a lifecycle change instead. Associations are a separate API surface, so a foreign key between two MariaDB tables does not become a contact-to-company association for free. And custom objects, where product-shaped records like workspaces belong, are an Enterprise feature.


## Which approach should you pick?


There are four realistic ways to build this, and they fail in different places.


Capability Custom scripts One-way ETL + reverse ETL Generic iPaaS Stacksync


Direction One script per direction Two products, two schedules Two flows, no shared state One engine, both directions


Typical latency Whatever cron allows Batch windows to nightly Minutes, queue dependent Sub-second


MariaDB change capture Your own binlog reader Query polling, deletes missed Polling or triggers Binlog CDC with GTID tracking


HubSpot write path Your own backoff code Bulk loads, limits hit on big runs One API call per task Batched, throttled, retried


Conflict and ownership Code you write and maintain None, the later job wins Manual branching per flow Field-level ownership rules


Echo and loop prevention Code you write and maintain Not addressed Filters you maintain Origin tagging built in


Custom logic Anything, and you own it SQL in the warehouse Low-code steps, limited No-code mapping plus Python


Four common builds for a MariaDB and HubSpot loop, and where each one gives out.


Custom scripts are the quickest thing to demo and the most expensive thing to own: the binlog reader, the rate limiter, the retry queue, and the conflict log are all code somebody has to keep alive. One-way ETL plus reverse ETL is two products, two schedules, and no shared idea of who owns a field. Generic iPaaS built for workflow automation handles a handful of triggers well and gets fragile at volume, because every record becomes a task run.


Stacksync is built for this shape: sub-second two-way sync between operational systems, with field-level ownership and conflict rules configured rather than coded, batching and retries handled by the platform, no-code mapping, and Python transforms where real logic is needed.[MariaDB](https://www.stacksync.com/connectors/mariadb) and[HubSpot](https://www.stacksync.com/connectors/hubspot) are both first-class connectors among 1000+. The[HubSpot and MariaDB integration](https://www.stacksync.com/integrations/hubspot-and-mariadb) page has the object-level specifics.


## Start with ownership, not with the pipeline


The hard part of connecting MariaDB and HubSpot is not the transport. It is deciding, before any data moves, which system is allowed to be right about each field, and then running an engine that enforces that decision in both directions without echoing its own writes.


Get that right and the integration becomes boring, which is the goal. Get it wrong and you spend the next quarter explaining why a rep's edit disappeared overnight. To see it running,[book a demo](https://www.stacksync.com/book-a-demo) , or look at the neighbouring pairings:[MariaDB and Salesforce](https://www.stacksync.com/blog/sync-mariadb-with-salesforce) and[HubSpot and PostgreSQL](https://www.stacksync.com/blog/sync-hubspot-postgresql-seconds-not-hours) . For background on why MariaDB exists, there is[the story of the fork](https://www.stacksync.com/blog/the-father-who-forked-his-own-database-to-save-his-daughter-s-name-the-origin-story-of-mariadb) .
