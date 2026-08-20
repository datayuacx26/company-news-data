---
schema_version: "1.0.0"
document_id: "d71e25964fc0146edf5438f458ef3fb47079d4520d7864e8f35f96e4a3f0b47e"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-7cfcee1b8c2a"
canonical_url: "https://www.porter.run/blog/what-is-heroku"
published_at: "2023-06-05T00:00:00+00:00"
first_seen_at: "2026-07-22T09:55:21.208768+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:27297ca488298832a6c0d55e60417f7f72d749e16e7deebc0b5b214f49290ea8"
---

# What is Heroku?

## Definition of Heroku


Heroku is a cloud Platform-as-a-Service (PaaS) founded by James Lindenbaum, Orion Henry, and Adam Wiggins in 2007. It allows software engineers to test, deploy, and manage web applications with ease. The Heroku platform originally only supported Ruby and the Ruby on Rails framework, but now supports various programming languages, from Java to PHP to Python.


‍


#### If you want to know more about PaaS in general, read on[here](https://www.porter.run/blog/what-is-platform-as-a-service) .


## Overview of Heroku's features


Heroku’s cloud platform essentially manages all DevOps needs, such as continuous delivery and continuous integration (CI/CD). Heroku allows users to build apps using buildpacks, such as the Nodejs runtime buildpack.


‍


On Heroku, an equivalent of a container is called a dyno and is utilized for application deployment. Dynos are created through containerization like all cloud-based containers, allowing users to deploy applications by providing compute and memory resources. In terms of the maintenance aspect of the software development process, there are Heroku logs; users can view application metrics and logging through the Heroku dashboard in order to manage their Heroku apps, as well. There is also a wide range of Heroku addons that support the development and operation of Heroku apps, like data stores.


‍


Review apps are another feature of Heroku; the code in pull requests are run in disposable applications, allowing users to easily test and merge changes to their codebase. Also, Heroku provides database management and web hosting services.


## Process of deploying an application on Heroku


### Step 1: Create a Heroku account


Heroku users must first create an account for the cloud platform.


### Step 3: Create your Heroku app


After creating an account, one can name and create an app.


### Step 3: Deploy your Heroku app


Once a user has created an app, they can choose a method of deployment, like GitHub or through the Heroku CLI (command line interface), and deploy their app.


## Benefits of using Heroku


### Ease of use


As shown in the steps above, Heroku is extremely easy to use and quite convenient as it manages DevOps for the user, and supports database management and web hosting.


**The DevOps cycle.**


### Simplicity


By abstracting away the underlying infrastructure, Heroku’s users don’t have to worry about anything except for application logic.


‍


#### If you want to learn more about the advantages of PaaS, read[this article](https://www.porter.run/blog/what-is-platform-as-a-service) .


## Drawbacks of using Heroku


### Cost


The Heroku platform can become prohibitively expensive for companies as they begin to scale. The cost of upgrading to a Heroku dyno that allows for more compute and memory resources can be quite high and is generally not cost-effective; for example, manually scaling from a Performance-M dyno to a Performance-L dyno requires users to double their cost, instead of being able to allocate resources granularly to each dyno.


‍


A common pattern for users is[migrating their Heroku Postgres database to RDS](https://www.porter.run/blog/migrating-postgres-from-heroku-to-rds) on an IaaS provider like Amazon Web Services (AWS) due to ballooning costs, then migrating the rest of their Heroku apps to the same cloud service provider, and using its web hosting services as well.


### Flexibility


As companies begin to scale, the convenience of Heroku can become a hindrance as they want greater control and flexibility over their increasingly complex cloud infrastructure. Specifically, Heroku’s convenience is a result of infrastructure abstraction to the point where users cannot access the underlying infrastructure on Heroku and so cannot configure it to their business needs.


### Security


**Cloud security can be a crucial factor when choosing a PaaS.**


‍


Many companies also manage sensitive data which leads to compliance concerns (HIPAA or GDPR for example), so this is often another reason to migrate their PostgreSQL instance off of Heroku as it is hosted on the public cloud. Heroku Private Spaces, one of the platform’s premium offerings on Heroku Enterprise, allows users to host in a VPC, but can be quite expensive.


‍


## Summary of benefits and drawbacks of Heroku


‍


In essence, Heroku is extremely user friendly and is great for engineers whose only worry is developing applications. However, it can become expensive at the point of scale and if companies want greater levels of security, and does not allow users the flexibility to configure their underlying infrastructure.


‍


## Porter


If you’re looking for the convenience of a cloud platform like Heroku but with greater scalability, flexibility, reliability, and security, try Porter[here](https://dashboard.getporter.dev/register) .


‍


Porter is a drop-in alternative to Heroku that runs in your own cloud. The only difference is that the former requires users to create an AWS/GCP/Azure account to connect their private cloud so that Porter can provision the underlying infrastructure, specifically Kubernetes. Users can then deploy apps through the Porter dashboard, API, or CLI.


‍


Porter supports the use of Heroku buildpacks, making the migration a truly simple process for users coming from Heroku (however, containerizing your applications with a Dockerfile is recommended as Docker images are the standard way to deploy on Kubernetes, but buildpacks work as well).


‍


In essence, the developer experience is the same as the Heroku platform but the level of flexibility and control users have over their cloud infrastructure is much higher if they want to go under the hood, although this is certainly not a requirement.


‍


In a similar vein, security and compliance are not an issue on Porter as users can always go under the hood and configure their underlying cloud infrastructure to be compliant with whichever framework they need to, from PCI/DSS to FERPA to FedRAMP.


‍


**Preview environments on Porter.**


‍


[Preview Environments](https://docs.porter.run/enterprise/preview-environments/porter-yaml-reference) are another feature of Porter that serves the same purpose as Heroku’s Review Apps; however, the former is more configurable than the latter.


**Logs on Porter are at parity with Heroku logs.**


Porter also supports logging, metrics, and integrations with several add-ons, like MongoDB, MySQL, Memcached, and Redis, that are at parity with Heroku addons.


**Some of Porter’s add-ons.**


Finally, Porter is much more cost-efficient than the Heroku cloud platform at scale as it allows for granular autoscaling, down to 0.01 vCPU and 1MB RAM. Furthermore, volume discounts are available as users scale, so instead of costs increasing exponentially as they would with dynos, they are more logarithmic.


##### Here are some case studies of companies that chose to migrate from Heroku to Porter:[Landing](https://www.porter.run/case-study/why-landing-chose-porter-to-scale-their-servers) , CommonLit,[Woflow](https://www.porter.run/case-study/why-woflow-moved-from-ecs-to-porter) , and[Memberstack](https://www.porter.run/case-study/how-memberstack-uses-porter-to-serve-30-million-requests) .
