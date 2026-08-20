---
schema_version: "1.0.0"
document_id: "7cd00e7414a46ffb23a4a077f05cb5c3ac6ba99aa8ace96b90376bb4aaf1d102"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/incident-report-for-february-15-2026"
published_at: "2026-02-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:20:18.906160+00:00"
content_hash: "sha256:6571334b76e772bca8104816ada89470855a07860594fc7d7c23454b2c7b17fa"
---

# Incident report for February 15, 2026

On February 15, 2026, starting at 10:19 PM UTC, Resend experienced an incident that caused email sending delays and errors loading the dashboard.


The outage lasted 3 hours and 31 minutes, with full service recovery at 1:50 AM UTC.


**No emails were lost during this incident.** However, most of the emails were delayed, and the dashboard was inaccessible due to database connection exhaustion.


We're really sorry if you were affected by this incident. You trust Resend with your emails, and we take that seriously. This post is a transparent account of what happened, how we responded, and the steps we're taking to prevent this from happening again.


## Incident overview


The database reached its maximum connection limit, with idle connections not being released fast enough under load. This caused connection exhaustion across the system, preventing the dashboard and non-email API operations from functioning normally.


We resolved the outage by:


1. Reducing connection pool sizes
2. Limiting idle connection times
3. Setting stricter connection limits per database role


Email sending continued to work throughout the incident, but delivery was delayed by approximately 2 hours on average, with all queued emails fully delivered by the end of the incident.


### Timeline (UTC)


*All times are in Coordinated Universal Time (UTC)*


- 22:20: Alerts were triggered for request latency
- 23:07: Noticed dashboard operations getting slow
- 23:55: Internal incident escalation and investigation
- 00:00: Posted on[status page](https://resend-status.com/incidents/01KHHVZA547STNMFTME9EAFA76) , updating every 30-60 minutes until resolution
- 00:19: Enabled attack mode out of an abundance of caution (visitors had to complete a security challenge before accessing the dashboard)
- 00:22: Reduced the maximum database connection pool size
- 00:42: Scaled up compute resources for executing server functions
- 00:50: Altered maximum number of connections for the application's database role
- 00:59: Observed a drop in the number of active database connections while processing the email queue at full speed
- 01:03: Observed the number of 5xx errors decreasing significantly
- 01:36: No more pending emails in the queue


## Background


Yesterday, due to a lack of configuration in one of our databases, a service went from an average of 60 database connections to over 330. This spike was unusual because it was not correlated to the actual traffic.


Graph showing database connections increasing


We have applications using different sets of combinations for: deployment type (long-lived, serverless, cron-based, etc), Postgres library and database connection config (direct vs using Pooler). This mixed setup made it hard to ensure a healthy usage of the database connections.


Since we built the email sending platform to be resilient to database unavailability, emails were delayed but not lost.


## How we responded


After identifying the database starvation issue, we realized the Max Pool Size for the database clients was too high, allowing that single application to use more database connections than it needed.


Graph showing database connections decreasing


We decreased the initial number twice, in a time interval of 20 minutes, allowing Resend's services to recover and open new connections to the database. After that, we altered the max number of connections for the faulty application (using Postgres` connection limit` configuration).


After approximately 40 minutes, the whole platform recovered and started reprocessing the delayed emails.


It's important to note that the internal escalation process took way longer than it should have, and we're actively working on improving it.


## Moving forward


To prevent this from happening again, here's what we're changing:


### 1. More resilient database connections


- Reviewing every Postgres role configuration to ensure that all applications have the proper limits on the number of connections they can open.
- Isolating a few critical tables as a separate database to reduce the blast radius of heavy load times.


### 2. Better incident response


- Improving the on-call rotation and escalation processes to reduce initial response time.
- Implementing a better way for customers to contact us during dashboard incidents.


Thank you to everyone who reported issues and gave us feedback. We hear you, and we're acting on it. If you have questions or concerns, please[reach out to us](https://resend.com/contact) .
