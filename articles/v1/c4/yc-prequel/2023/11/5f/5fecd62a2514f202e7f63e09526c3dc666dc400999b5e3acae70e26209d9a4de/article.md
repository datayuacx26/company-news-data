---
schema_version: "1.0.0"
document_id: "5fecd62a2514f202e7f63e09526c3dc666dc400999b5e3acae70e26209d9a4de"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/building-cross-cloud-identity-federation-in-go/"
published_at: "2023-11-01T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:6e7b6a3b5d36896530daafb5304a34fa0b2821d06bd9abcef7ad9b14bd5c3d71"
---

# Building cross-cloud identity federation in Go for secure data sharing

We recently shipped passwordless auth for all our AWS + GCP destinations. This is a significant upgrade from authentication using long-term HMAC keys or username/passwords.


To launch this role-based authentication, we needed to solve a number of challenges along the way thanks to our specific product requirements and architecture. Here’s how we solved these problems as well as some of the things we learned along the way.


## What is identity federation?


Identity federation is a system of trust between two parties for the purpose of authentication. In our case, using identity federation obviates the need to pass credentials between clouds. Instead, authentication between clouds relies on a federated agreement between cloud identity systems that will allow a Service Account in Cloud 1 to impersonate a Service Account in Cloud 2.


### Authentication using credentials


Previously, the default authentication mode for all Prequel destinations involved securely sharing credentials (e.g., username & password, HMAC access ID & secret, etc.) which were used to access the third party data storage account.


### Authentication using identity federation


We recently built cross-cloud identity federation to allow authentication from Prequel to most data storage options using identity federation. This involves a third party service account delegating access to a first party service account, which impersonates the third party to transfer data.


## Prequel specific challenges


As a bit of background, Prequel’s product is all about data replication between businesses and their respective cloud data platforms. We help our customers (usually SaaS vendors) sync their customers’ data directly to those customer’s data destinations (S3, Redshift, BigQuery, and Snowflake to name a few).


We anticipated three major challenges with this feature:


1. Prequel connects to data destinations (our customer’s customers) on different cloud products and across different cloud service providers (for example, S3, Redshift, and BigQuery) to sync data. This means we need to support authenticating *to* both Google and AWS clouds.
2. Prequel is often self hosted by our customers on different cloud Service Providers. This means the role impersonation needs to be supported *from* both Google and AWS clouds.
3. We want to be able to use different Service Accounts per Prequel customer, allowing our customer to bring their own service account, or allowing for more flexibility in the way we manage each of our tenants. This provides best-in-class security, and enables us to meet the enterprise security requirements our customers expect.


## Our Solution


We solved this by building a chain of authentication federation: from the Service Account running the Prequel deployment, to a customer-specific Service Account, and then to a destination specific Service Account (the “third party”).


## How we did it


Prequels’ core data transfer logic is written in Go and, based on the deployment, needs to run on K8s hosted in AWS or GCP. This results in at least 4 possible cross-cloud authentication options with varying degrees of difficulty (and documentation):


- GCP → GCP (easy, well documented)
- AWS → AWS (easy, well documented)
- GCP → AWS (straightforward, well documented)
- AWS → GCP (difficult, almost no documentation)


### AWS → GCP federation in Go


While 3 of 4 of our in-scope authentication options were straightforward, we encountered a roadblock when looking for prior art in AWS to GCP federation using Go. One resource we found,[gcpcompat-aws](https://github.com/salrashid123/gcpcompat-aws) , was informative but not straightforward. After some sleuthing and leveraging ChatGPT’s capability to deduce (hallucinate correctly?) API endpoints, we successfully implemented IAM federation from AWS to GCP in Golang.


### Golang Code Example


To share with the community, we’ve created a gist containing a basic example to help anyone needing to federate authentication from AWS to GCP using Go.


View the AWS to GCP IAM Federation[code here](https://gist.github.com/niger-prequel/07e997cdb75d559d9945653a314eb717#file-aws_to_gcp-go) .


## Conclusion


Our security features are of critical importance and always evolving, and federated role-based authentication is our latest improvement towards this goal. Role-based auth not only streamlines the user experience but also adds the additional layer of security our customers have come to trust us to provide. If you’re working on your own cross-cloud authentication, we hope this writeup is helpful. And if you’re working on cross-cloud data sharing, we hope you give us a call.


— The Prequel Engineering Team
