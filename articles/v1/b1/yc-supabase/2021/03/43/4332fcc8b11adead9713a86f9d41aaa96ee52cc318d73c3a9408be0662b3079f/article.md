---
schema_version: "1.0.0"
document_id: "4332fcc8b11adead9713a86f9d41aaa96ee52cc318d73c3a9408be0662b3079f"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgres-as-a-cron-server"
published_at: "2021-03-05T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:4729840e6eb857e0eefa423818e9e60f2a4bb34f7f3b48138c5da5eadbce9a9f"
---

# Postgres as a CRON Server

A Supabase user asked recently if they can trigger a webhook periodically. We haven't yet released Functions yet, so we checked whether it's possible with Postgres.


It is. Here's how.


### What's cron?#


A "cron job" is a script1 that runs periodically at fixed times, dates, or intervals. Traditionally you'd set it up on a Linux server. An example might be an hourly script that downloads emails to your computer.


These days, cron jobs are set up on a remote servers and in the cloud to run internet-related tasks. Like checking an endpoint every hour, or scraping a website every day.


### Postgres + cron#


Postgres has "extensions" which allow you to, well, extend the database with "non-core" features. Extensions essentially turn Postgres into an application server.


The team at[Citus](https://github.com/citusdata) created[pg_cron](https://github.com/citusdata/pg_cron) to run periodic jobs within your Postgres database.


#### Enabling the extension#


If you're using a cloud-hosted Postgres database, make sure that` pg_cron` is installed first. The easiest way to do this is to run this command:


`
_10


select name, comment, default_version, installed_version


_10


from pg_available_extensions


_10


where name = 'pg_cron';


`


If it returns a result then the extension is supported and you can turn it on by running:


`
_10


create extension if not exists pg_cron;


`


If you're using Supabase you can also enable it in the Dashboard.


#### Granting access to the extension#


If you're planning to use a` non-superuser` role to schedule jobs, ensure that they are granted access to the` cron` schema and its underlying objects beforehand.


`
_10


grant usage on schema cron to {{DB user}};


_10


grant all privileges on all tables in schema cron to {{DB user}};


`


Failure to do so would result in jobs by these roles to not run at all.


### Postgres + webhooks#


The Supabase customer wanted to call external endpoints every day. How would we do this? Another extension of course. This time we're going to use[pgsql-http](https://github.com/pramsey/pgsql-http) by[@pramsey](https://github.com/pramsey) . Using the same technique, we can enable the extension (if it exists in your cloud provider).


`
_10


create extension if not exists http;


`


This extension can now be used for[sending](https://github.com/pramsey/pgsql-http#functions)` GET` ,` POST` ,` PATCH` , and` DELETE` requests.


For example, this function would get all the people in Star Wars (using the[Star Wars API](https://swapi.dev/) ):


`
_10


select content::json->'results'


_10


from http_get('https://swapi.dev/api/people');


`


### Postgres + cron + webhooks#


Now the fun stuff. For this example we're going to call[webhook.site](https://webhook.site/) every minute with the payload` { "hello": "world" }` .


Here's the code (with comments` --like this` ).


`
_14


select


_14


cron.schedule(


_14


'webhook-every-minute', -- name of the cron job


_14


'* * * * *', -- every minute


_14


$$


_14


select status


_14


from


_14


http_post(


_14


'https://webhook.site/223c8a43-725b-4cbd-b1fe-d0da73353a6b', -- webhook URL, replace the ID(223c8..) with your own


_14


'{"hello": "world"}', -- payload


_14


'application/json'


_14


)


_14


$$


_14


);


`


Now when we see that the payload is sent every minute, exactly on the minute.


And that's it! We've built a cron webhook. Breaking down the code example above we have 2 key parts:


#### POSTing data#


This is the part that sends the data to the website:


`
_10


select status


_10


from


_10


http_post(


_10


'https://webhook.site/223c8a43-725b-4cbd-b1fe-d0da73353a6b', -- webhook URL


_10


'{"hello": "world"}', -- payload


_10


'application/json'


_10


);


`


#### Scheduling the job#


The HTTP function is wrapped with the CRON scheduler:


`
_10


select


_10


cron.schedule(


_10


'cron-name', -- name of the cron job


_10


'* * * * *', -- every minute


_10


$$


_10


-- Put your code between two dollar signs so that you can create full statements.


_10


-- Alternatively, you can write you code in a Postgres Function and call it here.


_10


$$


_10


);


`


The second parameter uses cron syntax:


`
_10


┌───────────── min (0 - 59)


_10


│ ┌────────────── hour (0 - 23)


_10


│ │ ┌─────────────── day of month (1 - 31)


_10


│ │ │ ┌──────────────── month (1 - 12)


_10


│ │ │ │ ┌───────────────── day of week (0 - 6) (0 to 6 are Sunday to


_10


│ │ │ │ │ Saturday, or use names; 7 is also Sunday)


_10


* * * * *


`


If you're unfamiliar with the cron syntax, useful shortcuts can be found on[crontab.guru](https://crontab.guru/)


`
_10


* * * * * # every minute


_10


*/5 * * * * # every 5th minute


_10


0 * * * * # every hour


_10


0 0 * * * # every day


`


#### Managing your cron jobs#


To see a list of all your cron jobs, run:


`
_10


select * from cron.job;


`


And if you need to see the results of each cron iterations, you can find them in` cron.job_run_details` :


`
_10


select * from cron.job_run_details;


`


To stop a running cron job, you can run:


`
_10


select cron.unschedule('webhook-every-minute'); -- pass the name of the cron job


`


### What can I do with this?#


There are plenty use-cases for this. For example:


- **Sending welcome emails.** If you use an email provider with an HTTP API, then you batch emails to that service. Write a function that` selects` all your signups yesterday, then sends them to your favorite transactional email service. Schedule it every day to run at midnight.
- **Aggregating data.** If you're providing analytical data, you might want to aggregate it into time periods for faster querying (which serves a similar purpose as a[Materialized View](https://supabase.com/blog/postgresql-views#materialized-views-vs-conventional-views) ).
- **Deleting old data.** Need to free up space? Run a scheduled job to delete data you no longer need.


See a detailed list in the[pg_cron README](https://github.com/citusdata/pg_cron#example-use-cases) .


## Addendum#


### Postgres background workers#


You might have noticed[this](https://github.com/pramsey/pgsql-http#why-this-is-a-bad-idea) notice the warning at the bottom of the` http` readme:


> "What happens if the web page takes a long time to return?" Your SQL call will just wait there until it does. Make sure your web service fails fast.


Luckily pg_cron implements[Background Workers](https://paquier.xyz/postgresql-2/postgres-9-3-feature-highlight-custom-background-workers/) :


> Care is taken that these extra processes do not interfere with other postmaster tasks: only one such process is started on each ServerLoop iteration. This means a large number of them could be waiting to be started up and postmaster is still able to quickly service external connection requests.


This means that even if your endpoint takes a long time to return, it's not going to be blocking your core Postgres functions. Either way, you should probably only call endpoints that will return a response quickly, or set the http extension to fail fast (` http.timeout_msec = 300` ).


If you're familiar with` C` , you could also help` @pramsey` to implement async functions:[https://github.com/pramsey/pgsql-http/issues/105](https://github.com/pramsey/pgsql-http/issues/105)


### Should I use Postgres as a cron server?#


There are plenty of ways to run cron jobs these days. You can trigger them from your local machine. You can install them on a VPS. You can schedule Serverless functions. You can use a paid service. You can use GitHub Actions.


Is Postgres the best place to put your cron jobs?` ¯\\_(ツ)_/¯` . Postgres databases are free on Supabase and since it takes only one minute to[get started](https://supabase.com/dashboard/) , why not make your next cron server a Postgres database?


## More Postgres resources#


- [Implementing "seen by" functionality with Postgres](https://supabase.com/blog/seen-by-in-postgresql)
- [Partial data dumps using Postgres Row Level Security](https://supabase.com/blog/partial-postgresql-data-dumps-with-rls)
- [Postgres Views](https://supabase.com/blog/postgresql-views)
- [Postgres Auditing in 150 lines of SQL](https://supabase.com/blog/audit)
- [Cracking PostgreSQL Interview Questions](https://supabase.com/blog/cracking-postgres-interview)
- [What are PostgreSQL Templates?](https://supabase.com/blog/postgresql-templates)
- [Realtime Postgres RLS on Supabase](https://supabase.com/blog/realtime-row-level-security-in-postgresql)


## Footnotes#


1.


Not necessarily a script. The cron is really a scheduler which triggers a job (of some sort, usually a bash script).↩
