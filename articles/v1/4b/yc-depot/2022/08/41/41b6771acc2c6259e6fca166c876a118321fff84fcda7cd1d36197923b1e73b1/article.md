---
schema_version: "1.0.0"
document_id: "41b6771acc2c6259e6fca166c876a118321fff84fcda7cd1d36197923b1e73b1"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/self-hosted-depot"
published_at: "2022-08-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:236456bc704054901cb90d8ddfcd0c29371241a99049d577bffb6b28b4dec222"
---

# Introducing self-hosted Depot builders

In June, we announced that we were working on the ability to run self-hosted Depot builders inside your own cloud accounts. This would allow project builds to run inside your own infrastructure instead of inside Depot's infrastructure providers. Allowing organizations with special requirements to utilize Depot while keeping their build data entirely inside their account.


Today, we are excited to announce that you can now launch self-hosted Depot builders in your own AWS account 🎉


## How self-hosted builders work


Self-hosted builders use a connection that you configure inside your Depot organization with details about your cloud account. You can then decide to use your self-hosted cloud connection on a per-project basis.


Self-hosted builders allow the Depot API to create, start, stop, and delete builder machines and cache volumes in your own cloud account. When the Depot CLI requests a build via` depot build` for a project using a self-hosted connection, the build flow is as follows:


1. The Depot CLI asks the Depot API for a new builder machine connection (with organization ID, project ID, and the required architecture) and polls the API for when a machine is ready
2. The Depot API stores that pending request for a builder
3. A` cloud-agent` process running in your cloud periodically reports current status to the Depot API and asks if there are any pending infrastructure changes — it receives a description of the machine to start for the pending build and launches that machine
4. When the machine launches, a` machine-agent` process running inside the VM registers itself with the Depot API and receives the instruction to launch BuildKit with specific mTLS certificates provisioned for your build
5. After the` machine-agent` reports that BuildKit is running, the Depot API returns a success response to the Depot CLI, along with new mTLS certificates to secure and authenticate the build connection
6. The Depot CLI uses the new mTLS certificates to directly connect the builder instance in your cloud account, using that machine and cache volume for the build


Your build data and context never passes through Depot infrastructure and communicates directly with the builder machine inside your AWS account.


## Open-source components


All the components that make self-hosted builders work are open-source:


-


[cloud-agent](https://github.com/depot/cloud-agent) runs as a Fargate task. This task is responsible for connecting to our Depot API via SSL and a connection token you receive when configuring your connection. It reports the current state of Depot infrastructure in your account, and receives back a list of actions that should be taken to update the Depot infrastructure to meet current build demands.


-


[machine-agent](https://github.com/depot/machine-agent) is a process that runs on the build machines that are launched by the` cloud-agent` . This process reports to our Depot API to inform it that it is available for a build. Once assigned a build, it launches BuildKit to run the build and report its results back.


-


[depot/connection/aws](https://github.com/depot/terraform-aws-connection) is a Terraform module that provisions the necessary AWS infrastructure and IAM permissions necessary for the cloud connection to run and provision Depot infrastructure in response to builds.


-


All the[machine images](https://github.com/depot/machine-images) (AMIs) that run inside your cloud are open-source as well.


-


Our[Depot CLI](https://github.com/depot/cli) as of version` 0.1.4` supports directly connecting to builder machines in your cloud using mTLS.


## Security and transparency


The hosted Depot service and API has no direct access to your cloud account, instead the` cloud-agent` process translates the desired machine state into changes in your account. This means there are no cross-account IAM roles provisioned and no AWS access tokens stored in Depot's database. Additionally, the` cloud-agent` has a limited vocabulary of actions it can perform, limited to simple actions like creating, stopping, starting, and deleting builder machines.


The` cloud-agent` process has a restricted IAM role policy that only allows management of Depot infrastructure in your account. The[IAM policy](https://github.com/depot/terraform-aws-connection/blob/8554439eb10265337be8ed356224f1c28224f5f0/main.tf#L268-L361) provisioned by Terraform allows the` cloud-agent` to interact with EC2 instances and EBS volumes in your account, only if they are tagged with a` depot-connection` tag. It also requires that any new instances or volumes must be tagged with the` depot-connection` tag. This limits what the process is allowed to access to only resources it has launched.


The Depot CLI communicates with self-hosted builder machines directly, using mTLS. Your build data and communication are encrypted with TLS, authenticated for the build, and do not pass through any Depot-hosted proxy or API. This means your build data is private to your own AWS account. The mTLS certificates are provisioned per build machine / per build.


## Getting started


To get started with self-hosted Depot builders,[see our setup and usage guide in our docs](https://depot.dev/docs/managed/overview) .


We think this solves a lot of use cases for organizations that have special requirements or need to use existing infrastructure for CI builds without having to manage any of it. Today, self-hosted builders only support AWS, but we intend to support others in the near future.


If you have any feedback about self-hosted Depot builders or have ideas for improvements, pleasesend us a message and let us know!


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
