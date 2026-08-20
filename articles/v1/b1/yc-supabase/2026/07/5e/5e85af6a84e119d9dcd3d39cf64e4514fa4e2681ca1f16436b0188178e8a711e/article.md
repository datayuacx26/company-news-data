---
schema_version: "1.0.0"
document_id: "5e85af6a84e119d9dcd3d39cf64e4514fa4e2681ca1f16436b0188178e8a711e"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/observability-for-every-supabase-project-with-grafana-cloud"
published_at: "2026-07-23T07:00:00+00:00"
first_seen_at: "2026-07-23T14:12:05.544840+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:af2cc730edf6b433012c1e053d382c86e174ad645f74d1f82e6d0058f032e103"
---

# Observability for every Supabase project with Grafana Cloud

The[Grafana Cloud integration](https://supabase.com/dashboard/project/_/integrations/grafana-cloud/overview?utm_source=supabase-announcement&utm_medium=blog&utm_campaign=supabase_grafana_cloud_one_click_integration_launch) is available in Supabase. You've been able to connect Supabase to Grafana Cloud for years, and now you can connect to it instantly with the push of a button.


From your Supabase Dashboard, in a single click, you get a fully-configured Grafana Cloud instance with authentication, metric scraping, and a pre-built observability dashboard already set up on any plan, including free.


[Grafana Cloud](https://grafana.com/products/cloud/) is the AI-powered open observability cloud built on open source and open standards. Used by developers everywhere, the full-stack observability platform helps you improve the reliability of your applications, resolve incidents quickly, and optimize your telemetry to reduce noise and costs.


## What the Grafana Cloud integration includes#


The integration ships with a pre-built Grafana Cloud dashboard for tracking more than 200 metrics, organized around the signals that matter when you're debugging a production issue:


- CPU and memory pressure
- Disk IO and throughput
- Network traffic
- Connection counts and pooler behavior
- Replication lag
- Scheduled and requested checkpoints
- Database size and query stats


The integration reads from Supabase's[Metrics API](https://supabase.com/docs/guides/telemetry/metrics) . It's one place to monitor your entire stack, whether that's your database, your servers, or anything else your team already tracks in Grafana Cloud.


The dashboards are[open source](https://github.com/supabase/supabase-grafana) and work the same everywhere. The same dashboard definitions power self-hosted Grafana, the existing Grafana Cloud integration, and the new one-click setup. No matter how you connect, you're viewing the same dashboards.


## What's next#


This release covers metrics. The integration is built to accommodate logs without any architectural changes, with support for log drains to Grafana Cloud already in progress.


## Get started with Grafana Cloud on Supabase#


The Grafana Cloud integration is available now on Supabase, on every plan (including free). Get started using the Grafana Cloud free tier with no credit card required on either side.


→[Connect your project to Grafana Cloud](https://supabase.com/dashboard/project/_/integrations/grafana-cloud/overview?utm_source=supabase-announcement&utm_medium=blog&utm_campaign=supabase_grafana_cloud_one_click_integration_launch)


→[Read the Grafana Labs' launch post](https://grafana.com/blog/grafana-cloud-supabase-one-click-integration)
