---
schema_version: "1.0.0"
document_id: "ce07e3a3d09c60f8e4181cca5b8c25079a0384f362d3b589b769ddb57956daed"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-b653f977f25a"
canonical_url: "https://building.nubank.com/how-nubank-distributes-infrastructure-ownership-to-operate-more-than-21-thousand-databases-with-a-lean-team/"
published_at: "2026-07-01T15:47:24+00:00"
first_seen_at: "2026-07-20T04:36:16.841262+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:7b373f8bad7950548ed2af2427d38dc6995818e659150ea8ce90b2c11bbaf99d"
---

# How Nubank distributes infrastructure ownership to operate more than 21 thousand databases with a lean team

*Written by: Nubank Editorial*


In a talk presented by Katharine Luiza, Engineering Manager at Nubank, during the 37th CNCF Meetup, held at Nu’s office in São Paulo, one theme came up repeatedly as a central piece of the scalability of Nu’s infrastructure: distributed ownership.


Instead of concentrating operational responsibility in a single platform team, the model adopted by Nubank turns infrastructure into a shared commitment across product engineering, automation and governance.


This approach supports an operation that today runs more than 21 thousand databases in production distributed across Brazil, Mexico and Colombia. And perhaps the most surprising thing is that this ecosystem is managed by just five engineers directly responsible for the database layer.


Even so, the environment maintains extremely high stability metrics, including **more than six months without infrastructure crashes and zero critical incidents related to databases in the last 12 months** .


But the central point lies in the way responsibility over infrastructure was redesigned to scale together with the company’s growth.


## **The real problem is not provisioning infrastructure**


Creating resources in the cloud was never exactly the hardest challenge. After all, *Infrastructure as Code* tools made provisioning relatively simple: any team can declare infrastructure, open a pull request and put new resources into production quickly.


However, as platforms grow, databases multiply, products evolve and squads change structure, resources with no clear context begin to appear. Some remain active, consuming capacity, but no one knows exactly what they are for. Others even have some known history, but have already lost the link to the teams that originally created them.


In many cases, there is an operational and financial cost associated with resources that no one actively monitors anymore. At Nubank, we categorize this scenario into three main patterns:


- The “ **zombies** ” are active resources in production whose purpose is no longer known. They keep running, consuming capacity and potentially storing important data, but no one knows exactly which system depends on them.
- The “ **orphans** ” still have some identifiable context, but have already lost any clear ownership. There is an idea of what they do, but no team is taking responsibility for them.
- And there is also a third layer: **resources with no clearly associated cost center** , which makes financial governance and capacity planning more difficult.


The problem, therefore, was to ensure continuous visibility over the lifecycle of these resources.


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## **Ownership as part of the architecture**


The solution began with an important cultural change: we started requiring ownership in order for new infrastructure to be able to exist.


At Nubank, any team can provision databases using declarative infrastructure, but no resource goes up without mandatory ownership information. Every pull request must mandatorily declare:


- the name of the resource;
- the responsible team;
- the owner;
- and the desired configuration for that environment.


When ownership stops being an informal convention and becomes a structural requirement of provisioning, infrastructure stops being the exclusive responsibility of the platform team. The team that creates the resource also becomes responsible for understanding how the resource grows, how much it consumes, which metrics matter and when it should cease to exist.


This creates a more sustainable model of scalability, because operational responsibility is distributed across the product teams themselves.


The role of the platform, then, comes to require less manual operation and more building of systems capable of automating governance, visibility and lifecycle management.


## **An ecosystem designed for automation**


This model works because there is a strong automation layer supporting the entire lifecycle of the infrastructure.


This process begins in a declarative repository called Nimbus, in which teams describe the resources they want to provision.


From there, an internal system nicknamed “Sorting Hat” comes into play. Its function is to automatically decide where each resource should be allocated within the company’s infrastructure.


This decision considers multiple rules, such as affinity between workloads, available capacity, account limits, resource distribution, corporate rules and future scalability needs.


After allocation, another component called Penseira begins to continuously track these resources. Its role is to keep a living inventory of the infrastructure and to follow not only what was declared, but what actually remains active.


This distinction became important because the team realized that “provisioned infrastructure” and “used infrastructure” were completely different things. In this context, not every resource created keeps performing its real function as time goes by.


## **When lifecycle management becomes part of the platform**


With distributed ownership and continuous observability in place, the next step was to automate decisions related to the lifecycle of resources.


That is how internal systems emerged aimed specifically at the detection, archiving and safe removal of inactive databases.


The first of them was named Memento Mori: a system that cross-references operational metrics, inventory data, ownership and real usage signals to identify potentially inactive resources. It monitors reads, writes, traffic, data input and output operations and even organizational changes in the teams responsible for the resources.


When a database appears to be abandoned, the system begins to automatically notify the responsible people via Slack.


The real goal, however, is to reinforce operational responsibility. The responsible team receives enough context, metrics and evidence to consciously decide what to do.


Today, the archiving approval rate for these resources is around 89%.


## **Automating removal requires automating safety**


Detecting inactive resources was only half the problem. The other half was ensuring that removal happened without operational or regulatory risk, and this is where another internal system comes in: Reducto.


The removal process involves much more than simply deleting databases. Since Nubank operates in multiple countries and in a highly regulated environment, different retention rules must be respected depending on the locality and the type of product.


In Brazil, for example, certain backups must remain stored for up to ten years. In Colombia, the requirements are different. And in Mexico, in turn, the periods vary according to the financial product involved.


When it comes into action, Reducto automates this entire flow, handling:


- archiving;
- backup creation;
- retention;
- validation;
- and safe removal.


Even after archiving, there is still a safety window of approximately 35 days before the definitive deletion of the resource. This avoids problems with sporadic workloads or systems run at low frequency.


Another critical point is the continuous validation of backups, because there is an important premise in this process: a backup without a *restore* is not a backup. That is why the flows include frequent recovery tests to ensure that the data can really be restored if necessary.


The result is a highly automated cycle that does not give up operational safety. Since the full implementation of this process, we have recorded no incidents related to automated removals.


## **Scaling infrastructure is scaling responsibility**


When platforms grow rapidly, the challenge becomes maintaining operational context over thousands of components distributed across different products, teams and regulatory requirements. Without continuous visibility, clear ownership and consistent lifecycle management, infrastructure inevitably accumulates invisible complexity.


The combination of declarative infrastructure, automation and distributed ownership allowed Nu to create an environment where thousands of databases can be managed with a high degree of reliability without depending on bureaucracy or constant manual operation. Instead of indefinitely expanding the team responsible for the platform, the focus shifted to building systems capable of distributing responsibility in a safe and observable way across all of engineering.


This model also changes the relationship between product teams and infrastructure. When ownership comes to exist directly in provisioning, in metrics and in the automated decisions of the platform, infrastructure becomes more sustainable at scale.


In the end, the challenge of operating more than 21 thousand databases was not solved with more centralized control, but by creating mechanisms that make responsibility and governance a natural part of the system itself.
