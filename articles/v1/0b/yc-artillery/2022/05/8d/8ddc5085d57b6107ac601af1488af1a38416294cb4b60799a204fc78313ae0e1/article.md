---
schema_version: "1.0.0"
document_id: "8ddc5085d57b6107ac601af1488af1a38416294cb4b60799a204fc78313ae0e1"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/grafana-dashboards-for-prometheus-metrics"
published_at: "2022-05-20T00:00:00+00:00"
first_seen_at: "2026-07-21T22:43:25.559968+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:13ede83d45370a89135950148a0e4fc3f54ac0aa2d6f4a2f6778884cc6d876ee"
---

# Grafana dashboards for your Artillery metrics in Prometheus

May 20th, 2022[How to](https://www.artillery.io/blog/tag/howto)


# Grafana dashboards for your Artillery metrics in Prometheus


Ezo Saleh


We’ve recently integrated[Prometheus](https://www.artillery.io/docs/guides/plugins/plugin-publish-metrics#prometheus-pushgateway) (via the Pushgateway) as a[publish-metrics](https://www.artillery.io/docs/guides/plugins/plugin-publish-metrics) plugin target.


This makes it super easy for you to collect your test metrics on Prometheus. A logical next step would be to visualise those metrics to better make sense of how your tests performed.


Prometheus ♥️[Grafana](https://prometheus.io/docs/visualization/grafana/) . To that end, checkout our[Artillery Grafana dashboards](https://github.com/artilleryio/artillery-examples/tree/main/prometheus-grafana-dashboards) .


They should help you get a leg up with your metric visualisations. Use them as is, or treat them as templates to customise to run your own analysis.


Just[import the dashboards](https://grafana.com/docs/grafana/latest/dashboards/export-import/#import-dashboard) and get visualising!


Here’s a sneak peek of our HTTP metrics dashboard.
