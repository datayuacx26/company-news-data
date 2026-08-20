---
schema_version: "1.0.0"
document_id: "900f1789a3e7c3c9055bdfada5ccdb17614f69d801a4afbc7ff79a97dc4e7b9e"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/a-closer-look-at-authzed-dedicated"
published_at: "2025-05-20T13:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:0c376cfe9568c09eeb1026bf89f54c98823690505b43282f8467355a77aa0395"
---

# A Closer Look at AuthZed Dedicated

At AuthZed, our mission is to fix broken access control. After years of suffering in industry from insufficient solutions for building authorization systems, we concluded that we'd have to start from the ground up by building the right infrastructure software.[SpiceDB](https://authzed.com/spicedb) , open sourced in late 2021, was our first-step to providing the solution that modern enterprises need. AuthZed Dedicated Cloud, often referred to as simply Dedicated, launched in early 2022 and productized SpiceDB by offering a dedicated cloud platform for provisioning SpiceDB deployments similar to the user experience you'd find provisioning infrastructure on a major cloud provider.


## What Are Dedicated Clouds?


Dedicated Clouds are a relatively new concept. When AWS hit the market, the term Public Cloud was coined; Public Clouds are cloud platforms that share their underlying hardware resources across a variety of customers. At the same time this term got coined, folks needed a term used to refer to what most folks were already doing before AWS launched: running their own dedicated infrastructure. Unfortunately, instead of calling this Dedicated Cloud, it became known as Private Cloud. So what are Dedicated Clouds? Well, they're the middle ground between Private and Public Clouds; Dedicated Clouds provide varying levels of isolation and dedicated resources than Public Clouds, but aren't placing end users fully in control quite like the traditional Private Cloud. Enterprises in regulated industries, or those that want to isolate particularly sensitive data, increasingly reach for Dedicated Cloud because it can provide most of the niceties of the Public Cloud while also delivering better security.


## The Evolution of AuthZed Dedicated


When AuthZed looked to create the first commercial offering of SpiceDB, we looked at where the industry was heading and implemented a Serverless product. However, it turned out that most enterprises value peace of mind that comes from isolating their authorization data from a shared data plane with other tenants. This was a happy coincidence because at the same time we learned that the best way to operate low-latency systems is to isolate workloads by having dedicated hardware resources. With our new insights, we launched Dedicated, our "middleground" that provided dedicated cloud environments with reserved compute resources and private networking. Dedicated customers get a private control plane deployed into their cloud regions of choice where they can provision their own deployments using our web console, API, or Terraform/OpenTOFU. Remaining true to the Infrastructure-as-a-Service (IaaS) spirit, pricing is done on a resource consumption basis.


## Looking Ahead to AuthZed Cloud


Since launch, Dedicated immediately became our flagship product. However, we recognized that some customers didn't require all of its isolation features.These are the same users looking for a self-service product to try things out without a long enterprise sales cycle. Our Serverless product inadvertently fits this description, but it's a limited experience compared to Dedicated. What if we could bridge the gap and bring a version of our Dedicated product where customers could share the control plane? We're calling this AuthZed Cloud (as opposed to AuthZed Dedicated Cloud) and it's under active development and expected to launch later this year. Best of all, because both Cloud and Dedicated will share the same codebase, all of the self-service features we're building will also be coming to Dedicated.


If you are interested in learning more about AuthZed Cloud, you can[sign up](https://authzed.com/z/authzed-cloud-waitlist) here for the beta waitlist.


On this page


- What Are Dedicated Clouds?
- The Evolution of AuthZed Dedicated
- Looking Ahead to AuthZed Cloud


## Related


[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Jimmy Zelinskie · Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)


[Engineering AuthZed Dedicated Now Available on Microsoft Azure AuthZed now supports Microsoft Azure, giving customers the opportunity to choose from all major cloud providers - AWS, Google Cloud, and Microsoft Azure. Deploy authorization infrastructure to 23+ Azure regions for globally distributed applications. Oct 21, 2025 · 2 min](https://authzed.com/blog/authzed-adds-microsoft-azure-support)[Engineering AuthZed Dedicated Now Available on Microsoft Azure AuthZed now supports Microsoft Azure, giving customers the opportunity to choose from all major cloud providers - AWS, Google Cloud, and Microsoft Azure. Deploy authorization infrastructure to 23+ Azure regions for globally distributed applications. Jimmy Zelinskie · Oct 21, 2025 · 2 min](https://authzed.com/blog/authzed-adds-microsoft-azure-support)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
