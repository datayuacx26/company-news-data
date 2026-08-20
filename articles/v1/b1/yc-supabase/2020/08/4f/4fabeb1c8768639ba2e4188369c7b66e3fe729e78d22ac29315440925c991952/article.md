---
schema_version: "1.0.0"
document_id: "4fabeb1c8768639ba2e4188369c7b66e3fe729e78d22ac29315440925c991952"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/continuous-postgresql-backup-walg"
published_at: "2020-08-02T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:0a510372852ab13c7d74052a3d67e7d096280de00391a1242e13b40e16cd1d44"
---

# Continuous PostgreSQL Backups using WAL-G

Have you ever wanted to restore your database's state to a particular moment in time? This post explains how, using WAL-G.


## Introduction#


[WAL-G](https://github.com/wal-g/wal-g) is an[open-source continuous archiving tool](https://www.citusdata.com/blog/introducing-wal-g-faster-restores-for-postgres/) used to easily set up and recover from[physical backups](https://supabase.com/blog/postgresql-physical-logical-backups) in Postgres. It mainly handles the storage and retrieval of physical backups and WAL archives to and from a chosen cloud storage provider. In this post, we will walk you through on how to effortlessly set up WAL-G for your database as well as guide you on what to do if and when disaster strikes.


## Prerequisites#


For this tutorial, we will be using two instances running Postgres databases on[Ubuntu 18.04](https://releases.ubuntu.com/18.04/) . One instance will act as your main database, the other is your recovery database. If you’re using another operating system some file paths may vary.


### Installations#


Make sure the below packages are installed in your instances. Alternatively, you can spin up the[latest version](https://github.com/supabase/postgres/releases/tag/v0.13.0) of[Supabase Postgres](https://github.com/supabase/postgres) which would already have everything configured and installed, along with other[goodies](https://github.com/supabase/postgres#features) . It is readily available in either the[AWS](https://aws.amazon.com/marketplace/pp/B08915TCJ2?qid=1595854723755&sr=0-1&ref_=srh_res_product_title) or[Digital Ocean](https://marketplace.digitalocean.com/apps/supabase-postgres) marketplaces and only takes[a few minutes](https://supabase.com/docs/postgres/postgres-intro) to get running.


#### Postgres 12#


A quick installation guide can be found[here](https://www.postgresql.org/download/linux/ubuntu/) .


#### envdir#


[envdir](http://manpages.ubuntu.com/manpages/bionic/man8/envdir.8.html) allows us to run other programs with a modified environment based on the files in the provided directory. This can be installed through the[daemontools](https://cr.yp.to/daemontools.html) package:


`
_10


$ sudo apt-get install -y daemontools


`


#### WAL-G#


`
_10


$ wget https://github.com/wal-g/wal-g/releases/download/v0.2.15/wal-g.linux-amd64.tar.gz


_10


$ tar -zxvf wal-g.linux-amd64.tar.gz


_10


$ mv wal-g /usr/local/bin/


`


### AWS credentials and resources#


When storing backups, WAL-G has numerous[cloud storage provider options](https://github.com/wal-g/wal-g#configuration) for us to choose from. For this tutorial, we will be using AWS. Have the following prepared:


- AWS Access & Secret keys.
- An S3 bucket.


## Setting it up#


### 1. Configure environment variables#


The directory` /etc/wal-g.d/env` is created and contains files that stores environment variables. It would later be used in WAL-G commands via envdir.


`
_10


$ umask u=rwx,g=rx,o=


_10


$ mkdir -p /etc/wal-g.d/env


_10


$ echo 'secret-key-content' > /etc/wal-g.d/env/AWS_SECRET_ACCESS_KEY


_10


$ echo 'access-key' > /etc/wal-g.d/env/AWS_ACCESS_KEY_ID


_10


$ echo 's3://backup-bucket/project-directory' > /etc/wal-g.d/env/WALG_S3_PREFIX


_10


$ echo 'db password' > /etc/wal-g.d/env/PGPASSWORD


_10


$ chown -R root:postgres /etc/wal-g.d


`


### 2. Enable WAL archiving#


Here, we enable[WAL archiving](https://www.postgresql.org/docs/12/continuous-archiving.html) and instruct Postgres to store the archives in the specified S3 bucket via WAL-G.


`
_10


$ echo "archive_mode = yes" >> /etc/postgresql/12/main/postgresql.conf


_10


$ echo "archive_command = 'envdir /etc/wal-g.d/env /usr/local/bin/wal-g wal-push %p'" >> /etc/postgresql/12/main/postgresql.conf


_10


$ echo "archive_timeout = 60" >> /etc/postgresql/12/main/postgresql.conf


`


### 3. Restart the database#


The database is restarted to let the changes in the configuration to take effect.


`
_10


$ sudo /etc/init.d/postgresql restart


`


### 4. Create your first physical backup#


`
_10


$ sudo -su postgres envdir /etc/wal-g.d/env /usr/local/bin/wal-g backup-push /var/lib/postgresql/12/main


`


At this point, if you were to check the S3 path that you provided, the following two newly created and populated directories would be observed:


From then on, subsequent physical backups would be found in the directory` basebackups_005` and any WAL archives would be sent to the directory` wal_005` .


### 5. \[Optional\] Schedule regular physical backups#


A CRON job can then be set to schedule physical backups to be performed everyday:


`
_10


$ echo "0 0 * * * postgres /usr/bin/envdir /etc/wal-g.d/env /usr/local/bin/wal-g backup-push /var/lib/postgresql/12/main" > /etc/cron.d/pg_backup


`


Here, the instance has been instructed to back up the database at the start of each day at midnight. By physically backing up your instance regularly, overall recovery time could be faster. Restoring from a physical backup from yesterday would lead to fewer WAL archive files to be replayed as compared to restoring from one from a month ago.


---


## Disaster strikes#


Something goes wrong with the database or instance. We will now use what available physical backups we have in the S3 bucket to recover and restore all of our data on to a new instance.


### 1. Configure environment variables#


The configuration should be the **same** as the original instance. For recovery and restoration, we would not need the variable` PGPASSWORD` .


`
_10


$ umask u=rwx,g=rx,o=


_10


$ mkdir -p /etc/wal-g.d/env


_10


$ echo 'secret-key-content' > /etc/wal-g.d/env/AWS_SECRET_ACCESS_KEY


_10


$ echo 'access-key' > /etc/wal-g.d/env/AWS_ACCESS_KEY_ID


_10


$ echo 's3://backup-bucket/project-directory' > /etc/wal-g.d/env/WALG_S3_PREFIX


_10


$ chown -R root:postgres /etc/wal-g.d


`


### 2. Stop the database#


`
_10


$ sudo /etc/init.d/postgresql stop


`


### 3. Switch to the user` postgres`#


`
_10


$ sudo -su postgres


`


### 4. Prepare the database for recovery#


#### Set restore_command#


Through[restore_command](https://www.postgresql.org/docs/12/continuous-archiving.html#:~:text=must%20specify%20is%20the%20restore_command,%20which%20tells%20PostgreSQL%20how%20to%20retrieve%20archived%20WAL%20file%20segments) , we instruct Postgres to pull all WAL archives from our S3 bucket to use during recovery.


`
_10


$ echo "restore_command = '/usr/bin/envdir /etc/wal-g.d/env /usr/local/bin/wal-g wal-fetch \\"%f\\" \\"%p\\" >> /tmp/wal.log 2>&1'" >> /etc/postgresql/12/main/postgresql.conf


`


#### \[Optional\] Achieve Point in Time Recovery (PITR)#


If we want to restore the database only up to a certain point in time (eg. right before the disaster), we can do so by setting both[recovery_target_time](https://www.postgresql.org/docs/12/runtime-config-wal.html#:~:text=recovery_target_time%20(timestamp)) and[recovery_target_action](https://www.postgresql.org/docs/12/runtime-config-wal.html#:~:text=recovery_target_action%20(enum)) . Do note that the timezone would need to match that of the original instance. This is usually at the UTC (+00) timezone.


`
_10


$ echo "recovery_target_time = '2020-07-27 01:23:00.000000+00'" >> /etc/postgresql/12/main/postgresql.conf


_10


$ echo "recovery_target_action = 'promote'" >> /etc/postgresql/12/main/postgresql.conf


`


### 5. Restore from physical backup#


The current data directory is deleted and is replaced with the latest version of the physical backup from the S3 bucket.


`
_10


$ rm -rf /var/lib/postgresql/12/main


_10


$ envdir /etc/wal-g.d/env /usr/local/bin/wal-g backup-fetch /var/lib/postgresql/12/main LATEST


`


### 6. Create a` recovery.signal` file#


This file[instructs](https://www.postgresql.org/docs/12/continuous-archiving.html#:~:text=Set%20recovery%20configuration%20settings%20in%20postgresql.conf%20(see%20Section%2019.5.4)%20and%20create%20a%20file%20recovery.signal%20in%20the%20cluster%20data%20directory) Postgres that the database should undergo recovery mode upon start.


`
_10


$ touch /var/lib/postgresql/12/main/recovery.signal


`


### 7. Log out of` postgres` and start the database#


`
_10


$ exit


_10


$ sudo /etc/init.d/postgresql start


`


Once Postgres finishes starting up and completes recovery mode, all data or data up to the specified point in time would have been successfully restored on to the new instance. Disaster averted.


## More Postgres resources#


- [Implementing "seen by" functionality with Postgres](https://supabase.com/blog/seen-by-in-postgresql)
- [Partial data dumps using Postgres Row Level Security](https://supabase.com/blog/partial-postgresql-data-dumps-with-rls)
- [Postgres Auditing in 150 lines of SQL](https://supabase.com/blog/audit)
- [Postgres Views](https://supabase.com/blog/postgresql-views)
- [Cracking PostgreSQL Interview Questions](https://supabase.com/blog/cracking-postgres-interview)
- [What are PostgreSQL Templates?](https://supabase.com/blog/postgresql-templates)
- [Realtime Postgres RLS on Supabase](https://supabase.com/blog/realtime-row-level-security-in-postgresql)
