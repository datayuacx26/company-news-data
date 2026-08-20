---
schema_version: "1.0.0"
document_id: "0016df072f63f39854e534df6f195f1db18a54e860bda54e2a64f1d8e21d102e"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-7cfcee1b8c2a"
canonical_url: "https://www.porter.run/blog/migrating-postgres-from-heroku-to-rds"
published_at: "2022-09-22T00:00:00+00:00"
first_seen_at: "2026-07-22T09:55:21.208768+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:afb46897dcac597e8d6fe4b924e111b1a96d407be8e7b73bc40ca5cdf4333a48"
---

# Migrating Postgres from Heroku to RDS

In our experience, databases tend to be the sticking point of most migrations away from Heroku. Especially for Heroku Enterprise users managing Postgres at scale, moving hundreds or thousands of gigabytes of mission-critical data with minimal service interruption poses a unique challenge. DB storage utilization, DB downtime, and special extensions are all additional factors we've had to consider while migrating terabyte-scale databases from Heroku to Amazon RDS as part of[our migration program](https://blog.porter.run/heroku-migration-program/) .


‍


In this article, we will summarize the three main migration strategies we've explored for moving Postgres from Heroku to RDS - ranging from simple Postgres dumping/restoration to WAL-based replication pipelines - and discuss the benefits and drawbacks of each.


## Option #1 - Keeping it simple pgdump and pgrestore


The most basic method for migrating a database involves using Postgres’ native dump and restoration tools. This was our initial approach for smaller Heroku users moving to RDS. We’d decide on a downtime/maintenance window beforehand and proceed to run the Heroku CLI (with its wrapper around` pgdump` ) against the source database to trigger a new backup, subsequently moving everything to a freshly-provisioned RDS database populated using` pgrestore` .


‍


The main advantage is that everything is based off of Postgres’ own native tooling and the process is completely bare-bones. Dumping and then restoring the native dump works especially well for much smaller databases that aren't larger than 10GB in size since Postgres compresses the native dumps quite efficiently. The native dumping mechanism happens to be faster than doing a text/SQL dump and so we could also shorten downtime windows.


‍


We did, however, occasionally encounter some interesting problems:


1. If a customer is using a Postgres extension with a particular version on Heroku, the same version has to be installed on RDS. Unfortunately, that’s not always straightforward - if the Heroku and RDS DB versions don’t match up, the extension versions may not match either. This can lead to all sorts of problems while trying to do native dumps, since there’s no way to directly tweak the extension version.
2. This leads to the second problem - native dumps are typically more reliable and less error-prone than text/SQL dumps, but if you have to use a SQL dump (to tweak an extension or some other aspect of the DDL), you can end up introducing other errors during the restoration. Oftentimes these errors won't make much sense, like the apparent absence of a foreign key in a particular table.
3. Finally, any database larger than 10GB ends up being subject to noticeable network latencies, and woe betide you if a dump breaks in the middle because you lose network connectivity. There are ways to automate this, but with other available options that felt to us like overkill.


Still, going for this method makes a lot of sense if:


1. The database isn’t using Postgres extensions and the versioning between Heroku and RDS matches up perfectly.
2. The database doesn’t exceed 10GB in size.


## Option #2 - Hitting a wall with WAL


At this point, we started to investigate others’ experiences with using Postgres Write-Ahead Logs (WAL) to asynchronously replicate data between two Postgres instances.


‍


Before we look at what WALs are about, it's worth acknowledging that Heroku did a terrific job with introducing the concept of using WALs to ensure a continuous level of data protection for Heroku Postgres instances. They introduced this way back in 2013 as part of an open source project called[WAL-E](https://blog.heroku.com/wal_e_and_continuous_protection_with_heroku_postgres?ref=blog.porter.run) . It’s quite useful when it comes to database recovery - or say, to audit how data has mutated. When it comes to replication, the idea is to continuously use the WAL from one Postgres database and replay it against a blank one to recreate the DB. More popularly, this is known as *streaming replication.*


The process is slightly tricky - we deploy a bare instance of Postgres on a fresh EC2 VM and install[wal-g](https://github.com/wal-g/wal-g?ref=blog.porter.run) which is an updated version of[wal-e](https://github.com/wal-e/wal-e?ref=blog.porter.run) . At this point, we shut down the database running on the VM, create a new folder to hold the Postgres DB's contents, and place a script on the server for carrying out the initial ingestion. An example script would look like this:


‍


### ` #!/bin/bash echo "starting wal-g backup-fetch" started=\`date +%s\` export AWS_REGION=us-east-1 export AWS_ACCESS_KEY_ID= export AWS_SECRET_ACCESS_KEY= export WALG_S3_PREFIX= wal-g backup-fetch /database LATEST ended=\`date +%s\` duration=$((ended - started)) echo "wal-g backup-fetch completed after $duration seconds"`


‍


The above script uses credentials supplied by Heroku to connect to the S3 bucket containing the raw WALs, downloads them, and uses` wal-g` to extract those logs and apply them against the database. This script is then run to populate the Postgres` /database` directory. Afterwards, it’s just a matter of adding another script to the Postgres database to periodically fetch newer WALs from S3, tweaking some configuration settings for the database to enable replicating WALs logically, and starting Postgres in recovery mode.


‍


It all sounds solid - powerful, even.


‍


There are, however, some notable limitations we've discovered:


‍


1. First off, getting the WALs for a Heroku Postgres database involves contacting Heroku’s Data Team and asking them for it. They in turn set up a blank S3 bucket and configure the Heroku database to dump its WALs continuously in that bucket. Credentials to access that bucket are then added to your Heroku app’s configuration. We noticed that every now and then, the credentials provided by the Heroku Data Team expire and need to be replaced.
2. There isn't a lot of documentation around this process. You’ll only find a couple of tutorials out there, usually with outdated or incomplete information.
3. The entire process involves setting up another Postgres instance on EC2 which is responsible for reading the WALs on S3 and ingesting them periodically. That involves two separate "types" of ingestions: an initial ingestion where the entire WAL is read in and executed, and subsequent ingestions which are supposed to read the WAL and only run transactions that have occurred after the initial ingestion. In our experience, the second set of ingestions can fail for a variety of reasons. Sometimes the bucket credentials are unexpectedly changed, there's conflicting documentation between differing Postgres versions, or the standby database hasn't been manually switched from` recovery` to` standby` after the initial replay is complete. Another issue we occasionally encountered is that a particular log file in the sequence would be missing - something that was apparent because the names of the log files downloaded onto the secondary instance weren’t matching up. Unfortunately, the logs from Postgres weren't particularly helpful in nailing down the exact cause.


In our opinion, running a WAL-based replication makes a lot of sense if you’re looking at generating a one-time copy of your Heroku database and you wish to run the exact sequence of queries that were run against your Heroku DB. If you’re looking at achieving asynchronous streaming replication however, you should expect to sort through a number of moving pieces and potential issues without a lot of documentation.


‍


Fortunately, at this point we found[Bucardo](https://bucardo.org/Bucardo/?ref=blog.porter.run) .


## Option #3 - Bucardo


Bucardo is… well, I’ll let Bucardo’s nicely-written[FAQ](https://bucardo.org/Bucardo/FAQ?ref=blog.porter.run) explain:


> *Bucardo is a replication program for two or more Postgres databases. Specifically, it is an asynchronous, multi-source, multi-target (sometimes known as master-master and master-slave or master-standby) table-based replication system. It is written in Perl, and makes extensive use of triggers, PL/PgSQL, and PL/PerlU.*


‍


To be fair, I did groan a bit when I saw the portion about Perl - but much more importantly, Bucardo fulfills its designated use case exceptionally well. The best part about Bucardo is that it doesn’t expect you to deal with WALs (we’ve seen how tricky they are), and it doesn’t move data synchronously (unlike a plain old` pgdump` ). In other words, I can go cook up a roast while I’m migrating a Heroku database - and I really like my roasts.


‍


Here’s how our migration process with Bucardo works:


To replicate a live Heroku Postgres instance onto RDS, we set up a VM in one of the public subnets inside the Porter cluster’s VPC which is responsible for running the replication pipeline. The pipeline is given database credentials for the Heroku Postgres instance (in the case of a one-way replication, these can be read-only credentials), along with a list of tables and sequences that need to be replicated. The pipeline then uses its own basic Postgres instance on the same VM, and listens to` NOTIFY` requests emitting from Heroku Postgres. Database triggers in the pipeline’s internal DB are then responsible for storing information on which rows were changed in the tables of interest along with sequence counts.


‍


When the pipeline is run for the very first time, it performs a one-time copy that syncs all rows from all tables on Heroku to RDS. Once this one-time sync is complete, it switches to delta mode, where it only copies changes to tables (i.e., additions, modifications, and deletions).


‍


A slightly advanced mode is where the replication pipeline is responsible for two-way syncing. This involves data being synced in real-time from Heroku to RDS and from RDS to Heroku. This can come in handy for users looking to maintain the same database in two locations if you want to take your DB migration low and slow.


‍


Our first brush with Bucardo involved testing a scenario where a Heroku Postgres database with about 16 million rows was moved to RDS. Setting up the pipeline took about 10 minutes, and then… sheer magic. Bucardo took over and replicated the entire database smoothly before automatically settling into delta mode, where it kept watching for row updates to the Heroku database replicated them asynchronously to RDS.


‍


Pretty rosy, right? Of course, there were still some pitfalls:


1. The most important factor here is that each table in the source database *must* have a primary key column defined. This is to ensure that the replication pipeline is able to track row changes, and the primary key is what enables the pipeline to track each row.
2. While the replication pipeline is running, it’s a good idea not to make schema changes to the source database as this can lead to drift between the source and the destination. This may be circumvented by using database migration tools and ensuring that any schema changes are applied as migrations to both the source and target DBs. In the event where the source database receives migrations first, the replication pipeline will pause upon encountering a schema drift. A subsequent migration applied to the target database will then jump-start replication again.
3. If the database has any materialized views defined, these would typically be recreated without any data; it is then necessary to run a manual query to jump-start the process of populating such views. To that end, it is a good idea to take note of all materialized views so that they can be repopulated during the migration exercise.


‍


All in all, not so bad. Most databases do have primary keys in all tables, and users have been extremely agreeable about adding primary keys in situations where some of their tables don't already have them. Since most teams use some kind of ORM or schema tracking library, ensuring that both database schemas stay in sync is not that troublesome. For repopulating the materialized views, it’s a small manual step in what’s otherwise a smooth and automated process. Slap some Ansible on it, turn it into a Kubernetes` Job` , and you suddenly have a very powerful technique for carrying out error-free, painless Postgres migrations at scale. And I get to have my roasts.
