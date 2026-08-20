---
schema_version: "1.0.0"
document_id: "a6ccfce387030a7cb8727fbe5f2996429efdd5c84b6be4b92b5ad4fe1083df75"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/backup-wordpress-site/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T10:22:48.360362+00:00"
fetched_at: "2026-08-06T10:22:50.657562+00:00"
content_hash: "sha256:da29735f54aa5fe55f88b2dce10198c2ddd24074381d3867c7be54525b07deab"
---

# How to Back Up a WordPress Site

A complete WordPress backup is a copy of two things captured together: your **files** and your **MySQL database** . Miss either one and the restore fails. This guide covers the reliable way to do that from the command line —` mysqldump` ,` tar` , and a cron job that pushes archives off-site — so you own the backup instead of renting it from a plugin. It also covers the host and plugin options fairly, and shows the restore commands that prove a backup actually works.


## Key Takeaways


- A complete WordPress backup has two parts that must be captured together: the files (core,` wp-content` ,` wp-config.php` , and on Apache` .htaccess` ) and the MySQL database — restore one without the other and the site comes back broken.
- The most common beginner mistake is backing up files over FTP and forgetting the database, where every post, page, comment, user, and setting actually lives.
- The core command is one line:` mysqldump --single-transaction -u USER -p DBNAME > db.sql` , where` --single-transaction` gives a consistent snapshot of a live InnoDB database.
- Automate it with a shell script on cron that dumps the database, tars` wp-content` , and pushes the archive off-server with` rclone` — the difference between a backup you remember to take and one that just happens.
- Follow the 3-2-1 rule and never keep the only backup on the same server as the site.


## What does a complete WordPress backup include?


A WordPress site is two separate systems, and a backup has to capture both. The **files** are the WordPress core, everything under` wp-content` — your themes, plugins, and the` uploads` folder (usually the largest part) — plus the root` wp-config.php` . On Apache you also want` .htaccess` ; on nginx or Caddy there is no` .htaccess` to save because rewrites live in the server config outside the web root. The **database** is a MySQL database that holds your posts, pages, comments, users, taxonomies, and every plugin and theme setting.


The single most common beginner mistake is backing up files over FTP and forgetting the database. Themes and core can be re-downloaded; your content cannot. If a “backup” is only files, the restored site loads with no posts and no settings.


## What are the three ways to back up WordPress?


There are three practical approaches, and they trade convenience for control.


Approach Control Scriptable Off-site by default Notes


Host auto-backups Low No Sometimes Short retention; inaccessible if the host is down; often disclaimed as your responsibility


Backup plugin Medium Limited Yes (configurable) Schedules and cloud upload from the dashboard; can choke on very large or heavily customized sites


Manual / CLI Full Yes You choose` mysqldump` +` tar` + off-site push; the focus of this guide


**Host backups** are convenient but should not be your only copy — retention windows are short, and if the server is compromised or dies, the backups stored on it can go with it. **Plugins** such as UpdraftPlus handle scheduling and cloud upload from the dashboard and are a reasonable choice for non-terminal users. **The CLI route** is the one a developer can version, schedule, and audit.


## Back up WordPress from the command line


The command-line backup is three steps over SSH: dump the database, archive the files, then pull both off the server. Connect first:


```text
ssh   user@example.com   -p   2222
```


Then archive the files and dump the database:


```text
# Archive the site files from the directory above the web root
tar   -zcf   files.tar.gz   public_html


# Dump the database with a consistent snapshot
mysqldump   --single-transaction   -u   DB_USER   -p   DB_NAME   >   db.sql
```


The` --single-transaction` flag is what makes the dump trustworthy on a live site. Only InnoDB tables are dumped in a consistent state;[MyISAM or MEMORY tables may still change during the dump](https://dev.mysql.com/doc/refman/8.4/en/mysqldump.html) . Because WordPress runs on InnoDB by default,` --single-transaction` is a much better option than` --lock-tables` because it does not need to lock the tables at all — your site stays up while it runs. Use` mysqldump` , not` mysqlpump` —[the latter was removed in MySQL 8.4](https://dev.mysql.com/doc/relnotes/mysql/8.4/en/news-8-4-0.html) , so scripts that call it will simply fail on current servers.


Pull both files down with` scp` :


```text
scp   -P   2222   user@example.com:~/files.tar.gz   .
scp   -P   2222   user@example.com:~/db.sql   .
```


One gotcha the tutorials skip:` scp` uses uppercase` -P` for the port, while` ssh` and` mysqldump` use lowercase` -p` (port / password respectively). Mixing them up is a classic failed-command.


If you have[WP-CLI](https://developer.wordpress.org/cli/commands/db/export/) ,` wp db export` is cleaner: it runs the` mysqldump` utility using the` DB_HOST` ,` DB_NAME` ,` DB_USER` , and` DB_PASSWORD` credentials specified in` wp-config.php` , and accepts any valid mysqldump flags.


```text
wp   db   export   --single-transaction   db.sql
```


On managed and cPanel hosts, a plain` mysqldump` can fail with a` PROCESS` privilege error while dumping tablespaces. WP-CLI already handles this:[wp db export adds --no-tablespaces to mysqldump by default](https://github.com/wp-cli/db-command) . With raw` mysqldump` on a managed host, add` --no-tablespaces` yourself.


## Automate WordPress backups with cron and rclone


Automate the whole thing with a short shell script on a cron schedule that dumps the database, tars` wp-content` , and pushes the archive off-server.[rclone](https://rclone.org/) — “rsync for cloud storage” — targets S3, Backblaze B2, and Google Drive, among others.


```text
#!/usr/bin/env bash
set   -euo   pipefail


SITE_DIR  =  "  /var/www/example.com  "
DEST  =  "  b2remote:example-backups  "       # a configured rclone remote
STAMP  =  "$(  date   +%F  )"
WORK  =  "$(  mktemp   -d  )"


cd   "  $SITE_DIR  "


# Database — WP-CLI reads credentials from wp-config.php
wp   db   export   --single-transaction   "  $WORK  /db-  $STAMP  .sql  "


# Files — themes, plugins, uploads, plus config
tar   -zcf   "  $WORK  /wp-content-  $STAMP  .tar.gz  "   wp-content   wp-config.php


# Push off-site
rclone   copy   "  $WORK  "   "  $DEST  /  $STAMP  "


rm   -rf   "  $WORK  "
```


Schedule it with a crontab line that runs daily at 03:15 and logs output:


```text
15 3 * * * /usr/local/bin/wp-backup.sh >> /var/log/wp-backup.log 2>&1
```


That is the “own your backups” payoff: no dashboard, no plugin, no manual step. If WP-CLI isn’t installed, swap the export line for` mysqldump --single-transaction --no-tablespaces -u DB_USER -pPASS DB_NAME > "$WORK/db-$STAMP.sql"` .


## Store backups with the 3-2-1 rule


Follow the 3-2-1 rule: keep at least three copies of your site, on two different types of media, with one copy off-site. Never let the only backup live on the same server as the site — a hack or a disk failure takes both at once. That is exactly why the automation script above pushes to object storage instead of leaving the archive in the web root. Because the archive contains a full copy of your site, including secrets in` wp-config.php` , secure the destination with a strong credential and two-factor authentication on the storage account.


## Test the restore, and the mistakes to avoid


An untested backup is not a backup. Periodically restore your` .sql` and file archive to a staging or local site to prove the copy actually rebuilds:


```text
tar   -xzf   wp-content-2026-07-06.tar.gz
wp   db   import   db-2026-07-06.sql            # or, without WP-CLI:
mysql   -u   DB_USER   -p   DB_NAME   <   db-2026-07-06.sql
```


Three failure modes account for most lost sites: backing up files with no database, keeping the backup on the same server as the site, and trusting the host’s automatic backups alone. Each is avoidable with the workflow above.


The reliable pattern is small and boring: a cron job that dumps the database consistently, archives the files, ships both off-site, and a restore you actually test. Write the script once, point it at object storage, add the crontab line, and run a test restore this week — that’s a backup you control rather than one you hope is running.


## FAQs


What is the difference between wp db export and running mysqldump directly?


wp db export is a thin wrapper over mysqldump. It runs the mysqldump utility using the DB_HOST, DB_NAME, DB_USER, and DB_PASSWORD credentials already stored in wp-config.php, so you do not look up or pass connection details, and it accepts any valid mysqldump flag. It also adds --no-tablespaces by default, avoiding the PROCESS privilege error common on managed hosts. Raw mysqldump gives the same dump but requires supplying credentials and flags yourself.


Why does mysqldump throw a PROCESS privilege error on managed hosts, and how do I fix it?


On managed and cPanel hosts, mysqldump attempts to dump tablespace information, which requires the PROCESS privilege that shared database users usually lack, producing an 'Access denied; you need the PROCESS privilege' error. Add the --no-tablespaces flag to skip that step and the dump completes normally. WP-CLI's wp db export adds --no-tablespaces automatically. In most cases the export still succeeds despite the error, so verify the dump by checking that your tables are present.


Can I skip file backups since WordPress core and plugins are re-downloadable?


You can reduce what you archive, but never skip files entirely. WordPress core, themes, and plugins are re-downloadable from their sources, so some backup tools store only the database plus the uploads folder. However, the uploads folder holds all media you cannot re-download, wp-config.php holds your database credentials and keys, and any custom or modified files are irreplaceable. Backing up wp-content and wp-config.php captures the parts that are genuinely unique to your site.


How do I restore a WordPress site from a mysqldump and tar backup?


Extract the file archive into the site directory with tar -xzf archive.tar.gz, then import the database. With WP-CLI, run wp db import db.sql, which reads credentials from wp-config.php. Without WP-CLI, run mysql -u DB_USER -p DB_NAME < db.sql against an existing database. Restore both parts together, and if the domain changed, run a search-replace on the database to update stored URLs. Always test the restore on a staging or local site before trusting it in production.


Is mysqldump still safe to use, or should I switch to mysqlpump?


Use mysqldump, not mysqlpump. The mysqlpump utility was deprecated in MySQL 8.0.34 and removed entirely in MySQL 8.4, so scripts that call it fail on current servers. mysqldump remains supported and current; MySQL recommends mysqldump or the MySQL Shell dump utilities as its replacements. For a consistent snapshot of a live InnoDB site, run mysqldump with the --single-transaction flag, which avoids locking tables while dumping.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
