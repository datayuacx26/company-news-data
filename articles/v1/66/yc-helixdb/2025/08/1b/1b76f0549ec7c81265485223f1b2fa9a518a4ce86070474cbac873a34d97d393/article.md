---
schema_version: "1.0.0"
document_id: "1b76f0549ec7c81265485223f1b2fa9a518a4ce86070474cbac873a34d97d393"
company_key: "yc-helixdb"
company: "HelixDB"
source_id: "yc-helixdb-news-import-f26087e13605"
canonical_url: "https://www.helix-db.com/blog/helix-cli-docker-deploy"
published_at: "2025-08-29T15:10:16.680+00:00"
first_seen_at: "2026-07-21T22:45:35.407247+00:00"
fetched_at: "2026-07-28T22:25:47.581815+00:00"
content_hash: "sha256:04706eb9782a5931629f723a96dc3114297710d9dc5b0044469234bda8152de7"
---

# Deploy Helix with Docker via the Helix CLI

We have had a lot of feedback asking for an easier and simpler way of deploying Helix via docker. We have finally made a super simple and easy docker integration to run Helix locally!


### TL;DR


- Run` helix dockerdev run` to deploy Helix locally to Docker.


### Pre-requisites


-


Ensure you have the most up-to-date version of the Helix CLI (>v1.0.134) installed


-


see[installation steps](https://docs.helix-db.com/documentation/getting-started/installation) for installation steps


-


if already installed: run` helix update`


-


Ensure you have docker installed, and the docker daemon running


-


[Docker desktop installation](https://docs.docker.com/desktop)


-


[Docker engine installation](https://docs.docker.com/engine/install/)


-


Write your HelixQL queries.


-


See[HelixQL documentation](https://docs.helix-db.com/documentation/hql/hql) for more information about HelixQL.


-


See[Social Network project walk through](https://(https//docs.helix-db.com/guides/social-network) for a project walk through.


### Steps


1.


Run` cd path/to/queries_dir`


2.


Run` helix dockerdev run`


Note that running the` helix dockerdev run` command in a different queries directory overwrite the queries being run in the container but the data will be persisted within the container between deployments. Run` helix dockerdev delete` before running` helix dockerdev run` again to wipe the data in the container.


### Other commands


-


` helix dockerdev run --background` to run the docker container in the background.


-


` helix dockerdev logs` to view Docker container logs.


-


` helix dockerdev stop` to stop the Docker development instance.


-


` helix dockerdev delete` to remove the Docker development instance and data.
