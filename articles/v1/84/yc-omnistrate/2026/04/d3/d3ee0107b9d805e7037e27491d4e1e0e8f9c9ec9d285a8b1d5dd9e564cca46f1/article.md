---
schema_version: "1.0.0"
document_id: "d3ee0107b9d805e7037e27491d4e1e0e8f9c9ec9d285a8b1d5dd9e564cca46f1"
company_key: "yc-omnistrate"
company: "Omnistrate"
source_id: "yc-omnistrate-news-import-f408a10afc71"
canonical_url: "https://omnistrate.com/blog/omnistrate-platform-update-february-march-2026"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-07-22T07:13:55.392025+00:00"
fetched_at: "2026-07-28T21:25:41.488077+00:00"
content_hash: "sha256:1cff1ba10af32b30c3406c9c38cf257eefc1822933b04056aa1fdb96e6d56393"
---

# Omnistrate Platform Update (February/March 2026)

## New Features in Omnistrate


### Infrastructure Upgrade to Bottlerocket | Now Available


Omnistrate has automatically upgraded all AWS EKS managed node groups to[Bottlerocket](https://aws.amazon.com/bottlerocket/) , ensuring your infrastructure stays compliant with the latest AWS standards. Bottlerocket is a container-optimized operating system that provides a hardened security posture through a read-only root file system and faster boot times. This upgrade was performed transparently — no effort or action was required on your part — and your workloads now run on a more secure OS by default across all AWS deployments.


### Terraform Now Runs in Isolated Pods | Now Available


To increase the reliability of infrastructure operations, Terraform now executes in dedicated, isolated pods rather than shared processes. This architectural shift prevents resource contention during concurrent infrastructure updates, ensuring that your provisioning workflows remain stable and predictable even under heavy load. The change is fully transparent — no modifications to your existing Terraform modules or Plan specifications are required.


Learn more about Terraform on Omnistrate in our[Terraform guide](https://docs.omnistrate.com/getting-started/build-from-terraform/) .


### Compose Specification JSON Schema | Now Available


A formal JSON schema is now available for Docker Compose specifications, enabling real-time validation and autocompletion for all` x-omnistrate-*` extensions directly in your IDE. Add a single line to the top of your compose file to activate inline linting in VS Code, IntelliJ, or any editor that supports the YAML Language Server. Catch specification errors before you build, ensuring faster and more accurate SaaS Product definitions.


Learn more in the[Compose Specification reference](https://docs.omnistrate.com/spec-guides/compose-spec/#using-schema-validation) .


### Granular Terraform Execution Control — Control Plane vs. Data Plane | Now Available


You can now direct Terraform execution to the control plane (your SaaS Provider account) instead of the data plane. This is especially valuable for BYOC deployments, where you need to provision certain infrastructure — such as networking — on your account for each customer deployment. Previously, Terraform always ran in the data plane; now you have the flexibility to choose where each resource's infrastructure is applied, simplifying and automating setups and reducing the configuration burden for you and your customers.


## Continuous Improvements


### Build Faster


-


**Custom DNS for Helm Load Balancers:** You can now configure custom DNS records for load balancers in Helm-based deployments, giving your customers branded, memorable endpoints for their instances. See the[endpoint aliases guide](https://docs.omnistrate.com/infra-guides/endpoint-aliases/?h=dns#plan-spec-configuration) for more details.


-


**Node Version System Parameter for Helm:** A new` $sys.compute.node.version` system parameter is now available for Helm-based resources, enabling you to reference the node pool version in affinity rules and chart values. See the[Helm customization guide](https://docs.omnistrate.com/build-guides/helm-charts-customize/) for usage examples.


-


**CLI Support for Snapshots:**` omctl` now includes full support for managing snapshots, allowing you to create, list, and restore snapshots directly from the command line.


### Improved Operations


-


**Instance Breakpoints for Workflows:** Instance breakpoints extend workflow capabilities, allowing you to pause a workflow on a specific operation. Pause, inspect, and resume — giving your team full control to debug problems. See the[Instance Breakpoints guide](https://docs.omnistrate.com/operate-guides/instance-breakpoints/) for details.


-


**Improved Scheduled Upgrade Error Handling:** Scheduled upgrade errors now surface clear, actionable diagnostics, reducing time to resolution during recovery operations.


-


**Notification Event Types API:** A comprehensive list of all notification event types is now available through the API, making it easier to configure[alert channels](https://docs.omnistrate.com/operate-guides/alarms/) with precise event type filters.


### Security and Compliance


- **Strong Password Enforcement in Customer Portal:** The[Customer Portal](https://docs.omnistrate.com/tenant-management/customer-portal/) now enforces strong password policies, requiring passwords that differ from the user's email address and meet minimum complexity requirements.


Additional documentation on these and other features[can be found here](https://docs.omnistrate.com/) .


## Neocloud Scaling: Waitlist Available


If you are a YC W26 founder building in the Agentic space and looking to leverage specialized, high-performance GPUs on Neoclouds while remaining seamlessly connected to the rich ecosystem of services on AWS, Azure, or GCP, we'd love to talk.


👉[Book a 15-minute sync with our founders](https://calendly.com/omnistrate/15-min-meeting)


## Trust Center


Review our live security protocols at[trust.omnistrate.cloud/controls](https://trust.omnistrate.cloud/controls) .


## Upcoming Events


-


**Building the Business Behind Open Source AI** | May 14 | AWS San Francisco (525 Market St) | 1 pm – 6 pm PST


Join AWS, Omnistrate, and the open source community at AWS SF for a deep dive into the mechanics of scaling, deploying, and monetizing AI in real-world customer environments.


👉[Register for the event](https://luma.com/h7csg3sj)


-


**BuildDevCon** | April 24 | Virtual | From OSS to Production DBaaS: How ISVs Ship OSS Databases on AWS in Days, Not Years


-


**Data + AI Conference 2026** | June 15–18 | San Francisco, CA


👉[See details & schedule time with our founders](https://omnistrate.com/events)
