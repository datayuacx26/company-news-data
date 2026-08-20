---
schema_version: "1.0.0"
document_id: "f119ab5db21579d0bc8796885b896cca57d93f902b9e18ab29b0ee2cd0e48cac"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/changelog-metrics-and-connections-recommendations"
published_at: "2023-03-08T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:904032fd6a2bdbd64153769f1f21c5da85423ec455551023d5c86c1ad134ddba"
---

# Changelog: Metrics and Connections Recommendations

## Features


### Metrics Integration


Visualizing your cloud infrastructure is important but monitoring is even more important. We have been hard at work intgrating metrics and observability right in the graph!


If you are running custom infrastucture or applications using Massdriver, we have open sourced modules which will create alerts and pull metrics in to Massdriver no additional assembly required.


- [Azure](https://github.com/massdriver-cloud/terraform-modules/tree/main/azure/monitor-metrics-alarm)
- [AWS](https://github.com/massdriver-cloud/terraform-modules/tree/main/aws/cloudwatch-alarm)


Listed below are the bundles which currently support this feature, with many more on the way.


- [azure-cosmosdb-sql](https://github.com/massdriver-cloud/azure-cosmosdb-sql)
- [azure-cosmosdb-mongo](https://github.com/massdriver-cloud/azure-cosmosdb-mongo)
- [azure-mysql-flexible-server](https://github.com/massdriver-cloud/azure-mysql-flexible-server)
- [azure-postgresql-flexible-server](https://github.com/massdriver-cloud/azure-postgresql-flexible-server)
- [aws-aurora-serverless-mysql](https://github.com/massdriver-cloud/aws-aurora-serverless-mysql)
- [aws-elasticache-redis](https://github.com/massdriver-cloud/aws-elasticache-redis)
- [aws-rds-postgres](https://github.com/massdriver-cloud/aws-rds-postgres)
- [aws-rds-mysql](https://github.com/massdriver-cloud/aws-rds-mysql)
- [aws-aurora-serverless-postgres](https://github.com/massdriver-cloud/aws-aurora-serverless-postgres)


‍


### Connection Recommendations


Understanding the nitty gritty of cloud infrastructure can be a daunting task, especially when you have an application to deliver. We here at Massdriver understand that and wanted to make it easier to start with your application and work backwards to a complete infrastructure putting the hard work on the platform.


Recommendations will allow engineers to upload an application to Massdriver describing the apps direct dependencies. From there we fill in the rest. Whether it is a new bundle or something that already exists on the canvas, Massdriver will guide you to production excellence!


‍


### Container Repositories


At Massdriver we use containers extensively. Docker, Podman, and OCI Compliant containers are a great solutions to ensure that what you have running in dev will run in production. Dealing with registries and repositories on the cloud can be a real headache. We wanted to make that easier by introducing a cloud-agnostic method to push container repositories to Azure, AWS, and GCP.


Using the Massdriver CLI it is as simple as running the following command to


- Build and tag your docker image.
- Make a registry if one does not exist.
- Create a repository if one does not exist
- And finally ship your image for deployment to whatever runtime you choose.


```text
mass image push massdriver/fizzbuzz -r us-west-2 -a {{your_credential_artifact_id}} -t v6


```


‍


## Bundles


We are excited to announce the release of several new bundles for our users! Our team ships open-source best-practice reference architectures to the Massdriver Package Manager each week.


These bundles make it easy for teams to experiment in the cloud and ship new features quickly, helping your organization stay ahead of the competition.


Don’t see what you are looking for? Your team can[develop their own bundles](https://docs.massdriver.cloud/bundles) to extend the platform to meet your needs.


- [azure-communication-service](https://github.com/massdriver-cloud/azure-communication-service)
- [azure-cognitive-service-language](https://github.com/massdriver-cloud/azure-cognitive-service-language)
- [aws-ecs-cluster](https://github.com/massdriver-cloud/aws-ecs-cluster)
- [aws-opensearch](https://github.com/massdriver-cloud/aws-opensearch)
- [aws-apigateway-rest-api](https://github.com/massdriver-cloud/aws-apigateway-rest-api)


‍


## Application Templates


New Application templates to help get your application into the cloud!


Enabling developer self-service can be difficult. An engineering platform’s specific capabilities depend entirely on its end users’ needs; those end users are your developers.


Organizations’ needs vary wildly,[but so can your development teams](https://www.gartner.com/en/articles/what-is-platform-engineering#:~:text=useless%20to%20another%20company) . Your legacy product might be running on VMs, while your Data Science team is running on cloud-managed services, and your new product is built on serverless.


There must be many ways to build and run applications to deliver an excellent platform for engineers to build upon; Massdriver supports many types of application workloads, including virtual machines, containers, and serverless.


We ship new support for workloads weekly, but your team can also extend Massdriver through[application templates](https://github.com/massdriver-cloud/application-templates/) .


- [azure-function-app](https://github.com/massdriver-cloud/application-templates/tree/main/azure-function-app)
- [aws-ecs-service](https://github.com/massdriver-cloud/application-templates/tree/main/aws-ecs-service)
- [aws-lambda](https://github.com/massdriver-cloud/application-templates/tree/main/aws-lambda)
- [rails-on-kubernetes](https://github.com/massdriver-cloud/application-templates/tree/main/rails-kubernetes)
- [gcp-vm](https://github.com/massdriver-cloud/application-templates/commits/main/gcp-vm)


‍
