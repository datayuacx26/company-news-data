---
schema_version: "1.0.0"
document_id: "cb66f97ea0bc9bd2d58d40bc4f665db45a96bcab3f506507450952037373f765"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-enterprise"
published_at: "2022-03-30T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:18aa0534cdaab547d5f2cbee9ad583d27819b4e2dc86b4fa04502346c8314dbf"
---

# Introducing Supabase Enterprise

As our platform continues its rapid adoption within the developer community, we're seeing a growing segment of users building business-critical applications that require enterprise-grade resilience.


## Enterprise features for everybody#


Some features are too good to limit to large customers, so today we're introducing a few enterprise features into the Pro Plan.


### Spend caps#


To simplify pricing, we've merged the “Pro” and “Pay as you go” plans and introduced monthly spend caps to avoid nasty billing surprises. These changes are to keep Supabase pricing[predictable, transparent, and developer friendly](https://supabase.com/blog/pricing) .


When you upgrade to the Pro Plan, spend caps are turned on by default, limiting your per-project costs to $25 per month. We're also retaining our soft limits while we manage the transition to granular spend-caps, so your service will continue to run even if your usage exceeds $25 (we'll contact you directly when you go over the limit). Right now there is a global project spend-cap, and in the future you'll have full control with configurable spend-caps on a “per-feature” basis.


### Database Add-ons#


Today we're releasing self-serve Database Add-ons.


What do you do when your project hits production and your userbase is sky-rocketing? Many developers have been asking to scale their projects on-demand. Database Add-ons give everyone this control.


Today, Database Add-ons are available for a small set of customers. We will progressively release this for everyone by the end of next week (Friday 8th April).


### New Log Explorer#


Today we're releasing a brand new[Log Explorer](https://supabase.com/docs/guides/platform/logs) in the Supabase Dashboard. As big advocates of SQL, we've done what any good Postgres fanatics would do - we're giving you the ability to query your logs using SQL. And if you're new to SQL, we have plenty of templates included. This is just one of the exciting features we are releasing through our[Logflare acquisition](https://supabase.com/blog/supabase-acquires-logflare) . Log history is available to every Supabase project1 :


- Free Plan: 1 day of log history
- Pro Plan: 7 days of log history
- Enterprise Plan: 90 days of log history


### Elixir Livebooks#


Today we're releasing[Elixir Livebooks for Monitoring](https://github.com/supabase/livebooks) , starting with built-in monitoring for PgBouncer, a Postgres connection pooler.


While our default PgBouncer settings work for 98% of our customers, sometimes they require customization for unique load patterns. For example, Supabase is popular with web3 projects where traffic can be very unpredictable - especially when you partner with Snoop Dogg. When[sound.xyz](http://sound.xyz/) dropped[an NFT with Snoop](https://www.sound.xyz/snoopdogg/death-row-mix-vol-1) , we customized their pooler to handle significant load. Their database handled over 9,000 simultaneously connections from Vercel's serverless API.


> In our MVP, we started with a serverless stack to simplify DevOps. Supabase made it dead simple to get PgBouncer and Postgres running so we could focus on the product.
>
>
> As we started hitting scale, they've been crucial to supporting our drops. When Snoop Dogg debuted on Sound, Supabase was able to help us provision our data store to handle the load.
>
>
> As we rearchitect our backend stack towards scaled microservices, we can be confident that managing Postgres won't be a bottleneck.
>
>
> Supabase takes out the mental effort from our back-end infrastructure so we can focus on our customers needs.
>
>
> Vignesh - CTO @ sound.xyz


If you want to monitor your own PgBouncer connections you can easily[spin up a Livebook](https://github.com/supabase/livebooks) on a free[Fly.io](http://fly.io/) instance.


## Enterprise Features#


With the release of our new Enterprise Plan, we're announcing a tonne of new features for Enterprise customers.


### Point-in-Time Recovery#


Disaster Recovery is a critical process for any company, even for the most fault-tolerant products. What happens when you accidentally delete that database column because of a clumsy` where` clause? Even with Supabase's daily backups, a day's worth of data can be lost if disaster strikes at the most inopportune time.


Every Enterprise project on the Supabase platform has access to Point-in-Time Recovery (PITR), allowing projects to recover from a snapshot mere seconds after a disaster. Supabase PITR is powered by[WAL-G](https://github.com/wal-g/wal-g) , an open source archival and restoration tool.


### Prometheus Endpoints#


If you run critical infrastructure you likely use Prometheus to monitor your metrics. We've now exposed a Prometheus compatible endpoint for our Enterprise customers. This allows them to scrape Supabase metrics into their own metrics infrastructure for real-time monitoring and alerting. This makes monitoring Supabase as easy as building with Supabase.


If you want access to the Prometheus Endpoint,[contact the Supabase Enterprise team](https://supabase.com/contact/enterprise) today and we'll get you setup.


### SLAs & Enterprise Support#


For our enterprise customers we know that service level agreements and support response times are also critical features. We continue to treat support as an important priority for all plans, though enterprise users require confidence that our response times meet their business needs. We now offer faster response times for Enterprise customers, alongside Priority and Priority Plus support packages for those who need more comprehensive support. You can find further details around our SLAs & support[in our documentation](https://supabase.com/docs/company/sla) .


### Enterprise Pricing#


If you need more information on our Enterprise plan, Pricing, SLAs, Support packages, or want to learn more about how Supabase can meet your scaling needs just[contact us](https://supabase.com/contact/enterprise) .


## Coming soon#


We have a lot more Enterprise features under development, available to early-access customers:


### SOC2#


Supabase is now SOC2 Type 1 compliant, as announced during Launch Week 5. You can read all about the process we went through to get there in this[blog post](https://supabase.com/blog/supabase-soc2) . Getting the Type 1 certification is just the start, and we will be working on getting certified for SOC2 Type 2 and HIPAA next.


### Log ingestion#


Supabase is now ingesting over 4 billion log events every week, just from the Postgres instances we host on the platform. Soon you'll be able to ingest logs and analytics from anywhere directly into your Supabase project.


### Foreign Data Wrappers#


Ingesting your logs is one thing, but what about querying them directly from your PostgreSQL database? What if you can figure out how many API requests one of your users made last month? Or how many gigabytes of video a user streamed from[Supabase Storage](https://supabase.com/storage) ? Watch this space!


### Materialized Views#


Querying huge datasets can be time-consuming, especially when you are aggregating billions of rows of historical logs. Supabase will make this simple with auto-updating views, saved to disk for increased performance. These views update periodically at a cadence that you decide. You'll even be able to fetch these views with your[Supabase API](https://supabase.com/docs/guides/database/api) !


## Next steps#


Get started today with all of our Enterprise Features on[supabase.com/dashboard](https://supabase.com/dashboard) , or[contact the Supabase Enterprise team](https://supabase.com/contact/enterprise) if you want to access our Enterprise features.


## Footnotes#


1.


Updated on May 31 2022 for accuracy: Changed to accurately reflect the log periods we advertise on our[pricing page](https://supabase.com/pricing) .↩
