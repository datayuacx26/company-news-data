---
schema_version: "1.0.0"
document_id: "b64effb4082abba5971b5b9ba2ef2b064ad4e04cd824929ad3578aca75842971"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-rss-5a4068f5753f"
canonical_url: "https://forum.openreplay.com/t/installing-is-always-failed-today/568"
published_at: "2026-05-12T09:27:16+00:00"
first_seen_at: "2026-08-17T14:12:50.872421+00:00"
fetched_at: "2026-08-17T14:12:52.808979+00:00"
content_hash: "sha256:6eb31615b2c2e5b43c3c415e99335e671a5a313e8b9aa3b8037eb3c5b43d659e"
---

# Installing is always failed today

Today, the OpenReplay installation using the following instructions:


sudo wget[https://raw.githubusercontent.com/openreplay/openreplay/main/scripts/helmcharts/openreplay-cli](https://raw.githubusercontent.com/openreplay/openreplay/main/scripts/helmcharts/openreplay-cli) -O /bin/openreplay
sudo chmod +x /bin/openreplay
openreplay -i DOMAIN_NAME


is consistently failing.


The database migration step does not complete:


\[INFO\] Using KUBECONFIG /etc/rancher/k3s/k3s.yaml


-


app databases-migrate-2hfpk › postgres-check


-


app databases-migrate-2hfpk › git
app databases-migrate-2hfpk postgres-check psql: error: connection to server at “postgresql.db.svc.cluster.local” (10.43.7.33), port 5432 failed: FATAL: password authentication failed for user “postgres”
app databases-migrate-2hfpk postgres-check Need version 16.4 or higher. Current version:
app databases-migrate-2hfpk postgres-check \[error\] postgresql version is which is not within the allowed range 16.4 - 17. Exiting.
app databases-migrate-2hfpk postgres-check For upgrade steps, refer:[https://docs.openreplay.com/en/deployment/openreplay-admin/#upgrade-postgresql](https://docs.openreplay.com/en/deployment/openreplay-admin/#upgrade-postgresql)


As of yesterday, the installation was working. This has been verified both on a local Ubuntu virtual machine and on an AWS instance.
