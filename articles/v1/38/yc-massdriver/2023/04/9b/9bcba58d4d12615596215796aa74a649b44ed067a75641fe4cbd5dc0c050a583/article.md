---
schema_version: "1.0.0"
document_id: "9bcba58d4d12615596215796aa74a649b44ed067a75641fe4cbd5dc0c050a583"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/changelog-app-first-design"
published_at: "2023-04-06T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:c321f430c5e5d9da7dfd1b56057f8f79288f6126f09f0bc0802f9e0a34815b30"
---

# Changelog: App First Design

## Features


### Application Bundle Generation in the UI


A solid operational solution must help reduce the coveted time to production (TTP). To aid in this task we have created a wizard directly in the UI that allows engineers to start with the thing they know best, their application. Before releasing this feature, getting an application in to massdriver required building files at the command line, editing IaC and publishing by hand. Now you can create your application bundle visually. Select your template, add dependencies and use the[bundle recommender](https://blog.massdriver.cloud/changelog/2023-03-08-metrics-and-recommendations/#connection-recommendations) to build your infrastructure from your application up.


### Github Actions


Making applications easier to manage means being able to deploy them with a simple merge of a pull request. Massdriver Github Actions will allow you to easily set up continuous deployment of your applications to production with all of the ease you expect from Massdriver. Building, configuring and deploying applications have never been easier. Check them out on[Github](https://github.com/massdriver-cloud/actions)


### Mass CLI v1.0!


Our first stable release adds a ton of functionality.


Visual layouts of infrastructure are living documentation, but some tasks require automation. We have rebuilt our CLI to better serve our customer’s automation needs. The new release features the ability to update configuration, initiate deployments and build preview environments.[Download the latest release](https://github.com/massdriver-cloud/mass/releases/tag/1.1.0)


### Show Only Different Values in Environment Diff View


One of the guiding principals of Massdriver is ensuring and enforcing parity between environments. In a world where infrastructure topology is more and more complex and efficiently scaling is the difference in driving profitability, being able to spot the differences between environments is crucial. To make this process easier, by default we will hide like values when diffing environments and deployments to help you spot the problem faster.


### Improved Sparkline Tooltip Interaction


Building on our built in metrics features, we have been hard at work making diagnosing issues easier for engineering teams. This month we released more readable tooltips that round to the 3rd most significant digit.


## Bundles


We are excited to announce the release of several new bundles for our users! Our team ships open-source best-practice reference architectures to the Massdriver Package Manager each week.


These bundles make it easy for teams to experiment in the cloud and ship new features quickly, helping your organization stay ahead of the competition.


Don’t see what you are looking for? Your team can[develop their own bundles](https://docs.massdriver.cloud/bundles) to extend the platform to meet your needs.


- [azure-storage-account-data-lake](https://github.com/massdriver-cloud/azure-storage-account-data-lake)
- [azure-machine-learning-workspace](https://github.com/massdriver-cloud/azure-machine-learning-workspace)
- [aws-kinesis-stream](https://github.com/massdriver-cloud/aws-kinesis-stream)
- [aws-s3-datalake-raw-bucket](https://github.com/massdriver-cloud/aws-s3-datalake-raw-bucket)
- [aws-vpc-peering-connection](https://github.com/massdriver-cloud/aws-vpc-peering-connection)
- [aws-aurora-postgresql](https://github.com/massdriver-cloud/aws-aurora-postgresql)
- [mongodbatlas-cluster-aws](https://github.com/massdriver-cloud/mongodbatlas-cluster-aws)


‍
