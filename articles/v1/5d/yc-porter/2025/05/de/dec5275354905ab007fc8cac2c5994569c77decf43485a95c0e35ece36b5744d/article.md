---
schema_version: "1.0.0"
document_id: "dec5275354905ab007fc8cac2c5994569c77decf43485a95c0e35ece36b5744d"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-d854f87d935e"
canonical_url: "https://www.porter.run/changelog/multi-app-porter-yaml-improved-cli-and-audit-logs-faster-docker-build-times"
published_at: "2025-05-21T00:00:00+00:00"
first_seen_at: "2026-07-25T19:39:33.596384+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:43696924d0ca6bf3d7ce7468e98b8b60f6ca1bb75299bb9ecbaa02a03b69c221"
---

# Multi-app porter.yaml, Improved CLI and Audit Logs, Faster Docker Build Times

## **Multi-application porter.yaml**


From one location and using Gitops best practices, users can now deploy and manage multiple, different docker images (multiple Porter apps) from a single file.


Users can also deploy development instances of Redis and Postgres in the same file, with support for Porter-managed production datastores and for deploying custom helm charts coming soon. More info[here](https://docs.porter.run/deploy/configuration-as-code/addons-porter-yaml) .


## **Faster Docker Build Times**


We’ve added functionality for caching every stage of users' Docker builds, resulting in up to a >25x improvement in build times for builds using dockerfiles.


Please let us know if you’re using dockerfiles and would like this enabled or have any questions about converting from buildpacks to dockerfiles!


## **Cheapest AWS GPU**


While Porter already supports most instance types available on AWS/GCP/Azure, a noteworthy recent addition is the g5g.xlarge instance type - the least expensive GPU instance available on AWS! This instance type is only supported in select AWS regions, however.


If an instance type you’d like to use on AWS/GCP/Azure is currently unavailable for use via Porter, please let us know and we’ll add it.


## **Audit Logs Update**


Users can now filter audit logs by date and export to CSV. Audit logs now show the specific Porter operator who performed an action as well.


## **CLI Updates**


### Revamped Logs


Users can now view historical logs, with more filters, rather than only having access to a live stream of logs. More info on the` porter app logs` command[here](https://docs.porter.run/standard/cli/command-reference/porter-app#porter-app-logs) .


### Improved Job Support


Users can now run jobs via the` porter app run` command (docs[here](https://docs.porter.run/standard/cli/command-reference/porter-app#porter-app-run) ) and get visibility into historical job runs (docs[here](https://docs.porter.run/standard/cli/command-reference/porter-job) ).


Equivalent functionality via the Porter API is in the works.
