---
schema_version: "1.0.0"
document_id: "d483d704e74239ae8c8096738c2c1ec006b7d23cb3c0f716546b58421162ffd5"
company_key: "yc-embrace"
company: "Embrace"
source_id: "yc-embrace-news-import-2dd1f24c69b5"
canonical_url: "https://embrace.io/blog/deep-dive-into-embrace-data-directly-from-grafana-with-seamless-back-links/"
published_at: "2025-05-08T21:22:53+00:00"
first_seen_at: "2026-07-21T17:58:27.596992+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:11ed0bda9263603fab747eefee9e99655c888bc353ce5e6fcad267ef83ba6ad0"
---

# Deep dive into Embrace data directly from Grafana with seamless back-links

Embrace's integration with Grafana now supports back-linking. That means you can seamlessly navigate from Embrace data in your Grafana instance directly to the relevant data in your Embrace dashboard for immediate analysis and troubleshooting.


We know many of our customers love visualizing their critical mobile observability data from Embrace directly within their Grafana dashboards. It’s a powerful way to see key metrics and trends alongside other system data.


But what happens when you spot something interesting – an anomaly, a spike, or a dip – in your Grafana chart and need to investigate the underlying data within Embrace?


Previously, this meant manually navigating to Embrace and trying to recreate the context of your Grafana view.


With our latest update on the Embrace + Grafana integration, this process just got a whole lot easier.


You can configure deep link capabilities in your Grafana instance that are powered by APIs feeding in data from a specific source. Now, Embrace is one of those supported data sources in your Grafana instance.


That means you can seamlessly navigate from Embrace data in a Grafana dashboard to the corresponding data in your Embrace dashboard. In Embrace, you have advanced analysis tools specialized for mobile to further interrogate this data.


For example, you may be forwarding metrics from Embrace to your Grafana instance to create visualizations, alerts, or power your SLOs. If any of those mobile metrics look off, you can seamlessly go from your Grafana dashboard right to the relevant data in your Embrace dashboard with just a single click. That way, you don’t have to waste time trying to find the right exemplar in Embrace to troubleshoot a potential SLO violation or alerts spike.


That’s because the back-link is precise and context-aware. The destination within the Embrace dashboard depends on the metric being visualized


.


For example, if the metric represents session data, clicking the link will redirect you directly to the Sessions page in Embrace.


The data link also preserves the selected time range from your Grafana view. If the time range in Grafana is specified in a relative format (like “Last 12 hours”), it will be converted to absolute time stamps in Embrace


It also preserves some supported aggregations that may be active in the Grafana visualization where the data link is configured. For instance, if your metric is aggregated by operating system version and country, Embrace will recognize and save those aggregations as filters when presenting the corresponding data for your in the Embrace dashboard.


This new back-linking feature streamlines your workflow, enabling faster investigation and deeper analysis of your mobile data directly from your Grafana dashboards.


To try it out for yourself, check out or[docs](https://embrace.io/docs/data-destinations/grafana-cloud-setup/#configuring-backlinks) for guidance. Not an Embrace customer? Get started for free[here](https://dash.embrace.io/signup) .


Deliver incredible mobile experiences with Embrace.


Get started today with 1 million free user sessions.


[Get started free](https://dash.embrace.io/signup/)


Author


[Virna Sekuj](https://embrace.io/author/virna/) Virna Sekuj is a product marketer at Embrace. She has nearly ten years of experience in product management, marketing, and research analysis. Prior to working at Embrace, Virna worked at Bose, Onside Sponsorship, and GWI. In her time with Embrace, she’s used her insight and analysis expertise to lead two research studies polling engineers that have produced two reports — The State of Mobile Experience and The Mobile Developers Pain Points report.


Related Content


grafana


2 January 2025


• 8 min read


### [Building a mobile SLO with Embrace and Grafana](https://embrace.io/blog/building-a-mobile-slo-with-embrace-and-grafana/)


Engineering teams use mobile SLOs to monitor user flows and ensure user-impacting issues are detected and resolved quickly. In this tutorial, we'll show you how to use Embrace, in combination with Grafana, to build and monitor mobile SLO for your app – from initial app instrumentation, all the way to connecting to backend dashboards and alerts.


grafana


31 October 2024


• 1 min read


### [Bring mobile observability to DevOps with Embrace and Grafana Labs](https://embrace.io/blog/embrace-grafana-devops-webinar/)


Watch an on-demand webinar where Embrace and Grafana experts show you how to bring mobile observability into your DevOps practice.


grafana


24 June 2024


• 4 min read


### [Embrace and Grafana Labs: Bringing the mobile app into your modern observability solution](https://embrace.io/blog/embrace-grafana-labs-partnership-technical-integration/)


Enable full-stack observability with unparalleled visibility into mobile user experiences.
