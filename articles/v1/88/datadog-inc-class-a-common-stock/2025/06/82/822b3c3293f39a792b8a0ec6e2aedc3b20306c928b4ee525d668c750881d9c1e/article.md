---
schema_version: "1.0.0"
document_id: "822b3c3293f39a792b8a0ec6e2aedc3b20306c928b4ee525d668c750881d9c1e"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/unwinding-shared-database/"
published_at: "2025-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:6b7db7b6bd970d73880b6d48f5137457cc9ee125d56007724e7b1f9bded1d8d0"
---

# Breaking up a monolith: How we’re unwinding a shared database at scale

Fabiana Scala


Tali Gutman


Like many other organizations, Datadog has long relied on the convenience of a large, shared relational database across multiple teams. This pattern is pervasive in the industry because it works well for many workloads—and continues to work well even at surprisingly large scales. But eventually, the trade-offs start to pile up. A simple database upgrade requires coordinating with stakeholders across the entire company. If the company is small, that’s manageable. As the organization grows, even routine maintenance becomes risky. Teams trying to evolve their data models may discover that their database schema **is** their API—and they have no way of knowing who else depends on it.


Breaking apart a shared database is far from an obvious win—it’s expensive, time-consuming, and not always the right answer. But at Datadog, we’re approaching the limits of the shared database pattern. Fortunately, we’ve reached the point in our maturity—with observability, tooling, and platform support—to do something about it.


In this post, we’ll walk through how we’re splitting apart our shared database into independently owned instances. We’ll explain how we defined the right boundaries, minimized risk during migrations, and built the tooling to make the process safe and scalable. If your team is grappling with the pain of a shared database, we hope this gives you a sense of what’s possible—and what to watch out for.


## Why do shared databases exist?


When a company—or even a group within a company—is small and trying to move fast to find product-market fit, it tries hard to reduce overhead and unnecessary work. When there’s little to no workload, isolation of workloads is irrelevant. Organizations rightfully choose to worry about the cost of success when they get there. Everyone connects with the same database user because ain’t nobody got time for managing access, anyway.


**Many organizations discover that the level of success required for the costs of operating a shared database to outweigh the benefits is actually quite high. The cost of breaking up the shared database is also quite high.**


There is no denying that having everything in a single database allows for a more streamlined architecture—one in which everyone can join against everyone else’s data with low latency. Recreating that flexibility in other ways is an investment on its own.


Given all that, of course the shared database limps along for as long as it possibly can—and then some.


## When is it time to take apart the shared database?


The pain a shared database causes can take any number of forms:


-


The size of the data exceeds that of a single machine, or replication is too slow for some of the workloads


-


Noisy neighbor issues are hard to predict or reason about


-


Brittle schema management, whereby one team’s changes may unexpectedly impact another team


-


Some guarantees may be required around access—for example, **access control lists (ACLs)** —that can’t be easily satisfied with a shared database


-


And many more


In any case, all of these issues translate to money—engineering time sinks, user-impacting incidents, degraded performance, and such. When the challenges of a shared database start surfacing in conversations across teams, it’s a sign that the pain may be widespread enough to justify breaking it up.


Sadly, unless a few prerequisites are in place, extending your rope might prove to be a more tenable strategy. Datadog has laboriously “split off” large chunks of its one big shared database into two or three databases before tackling this issue outright. It’s difficult to find the right boundaries, enforce them carefully whilst trying your darndest not to cause an incident, execute the split, and hope that you’ll never have to do it again.


In broad strokes, the process of splitting a database into individual instances along functional boundaries requires:


-


Identifying those functional boundaries


-


Setting up services that support cross-boundary querying, if needed, and making everyone who depends on a domain’s data use the services instead of querying the database directly


-


Spinning up a new database instance, and carefully executing a migration from old to new


## What keeps teams from moving off shared infrastructure?


Even if the problems with a shared database are clear, solving them isn’t always a priority. Teams may hesitate to make the leap if:


-


Creating a service means blowing off quarterly goals completely


-


Maintaining a service is daunting, cumbersome, and ops-intensive


-


Accessing data across domain boundaries has no clear path, or results in unacceptable user impact


-


Owning a database instance yields additional ops burden for the team


-


Migrating from old to new is an artisanal, handcrafted, and manual exercise


And if you were a person responsible and authorized to force the migration, you might still be able to achieve your goal—but the cost would be astronomical, arguably worse than the status quo, and everyone would hate you. So, the engineers who see the pain and want to make it go away often find the headwinds too strong and drop it altogether.


## Platform investments that change the equation


All of these **if** s could be tended to. But those are platform engineering challenges, and not quick fixes.


At Datadog, we addressed them with two major projects:


1.


**Rapid** , an opinionated framework for building and operating API and gRPC services at Datadog


2.


**OrgStore** , a managed platform for Postgres databases


Each of these projects was started for its own merits.


A growing company spins up a lot of new services—at least in the infrastructure side, backend, and middle-tier services. These services often build on top of shared databases and web frameworks because it’s a faster way to ship new features. This approach has benefits, including shared configuration, common data access patterns, and core features—like rate limiting and user management—that appeal to engineers with a backlog of work.


Do this for long enough, however, and the monolith becomes unmanageable. At some point, the tides turn and what was previously easy to use becomes impossible to scale, hard to work with, and a major reliability concern. This became true for us, too, and was the seed for the Rapid framework. When pulling up a service is a half-day’s work, and its maintenance is supported by a group of framework experts, the cost-benefit analysis tips the scale in favor of new services.


Similarly, as the pain of the shared database grows, greenfield projects look for alternatives to using it. Teams feel that adding their new and shiny project to the legacy database that always breaks is unappealing, and the benefits of it from that standpoint start to fade. Those use cases create a clear need for a managed platform that shifts the operational burden of running databases to the experts. With the right tooling and automation in place, those experts become force multipliers, improving the platform over time rather than drowning in operational load as database instances multiply.


The OrgStore managed platform provides Datadog teams concrete ownership of the databases and their operations. A common problem that comes with operating a shared database is running out of connections for the myriad of services. For a long time, we primarily relied on[PgBouncer](https://www.pgbouncer.org/) to handle our connection pooling. So, at the heart of building OrgStore was also the need to solve problems at the shared proxy layer.


The Datadog homegrown proxy, **PG Proxy** , is another crucial component to being able to safely perform a database migration. It allows individual teams to migrate at their own pace and to connect to any database. Once services are connected to their PG Proxies, it is really up to the platform team to determine how to shift traffic. We will dive deeper into how exactly we do this as we go through the full migration process.


If you have platform support—opinionated service frameworks, managed databases, and connection-layer abstractions—then splitting your shared database is a much safer, more achievable project.


If you do not, ask yourself whether you should. That is, is there a business reason to create them? Are you at the point where a shared database is causing enough pain?


If your engineering team is pushing for this kind of migration, the company has to believe the end state is worth the cost—and the risk.


## How we planned our database migration


Even with managed platforms to rely on for a new promised land, the journey from here to there is infested with dragons. And we **all** know that.


When we embarked on this journey, we tried our best to predict and head off hurdles and failures, and minimize the risk as much as possible. The following path that we describe works for Postgres, which is the database we use at Datadog. We’re hopeful that a similar approach can work for any other choice of relational database management system (RDBMS).


We came up with a three-phase plan:


-


**Logical separation**


-


**Access reduction**


-


**Physical separation**


### Phase 1: Logical separation


The goal of this phase is to create and enforce data domain boundaries. This entails:


1.


Understanding which data domains make sense for Datadog today—that is, a set of tables that serve a common goal or belong to a group of closely related teams—was the first step. We achieved this by exporting and analyzing[Database Monitoring (DBM) query samples](https://app.datadoghq.com/databases/samples?query=dbm_type%3Aactivity%20source%3Apostgres&agg_m=count&agg_m_source=base&agg_q=%40db.statement&agg_q_source=base&agg_t=count&dbType=PostgreSQL&fromUser=false&top_n=10&top_o=top&viz=query_table&x_missing=true&paused=false) , and verifying our clustering with engineers on more than 30 teams. This also required us to agree on which teams are responsible for the data cluster going forward and its future associated infrastructure. For instance, it was a clear indication that cross-domain solutions were necessary for teams to continue to support their features.


Query samples with export button. Query samples with export button.


1.


For each domain, create a manifestation of the domain boundaries, in the form of a “private” schema, and move the relevant tables there. This process includes:


-


Removing inter-domain foreign keys—from object-relational mappers (ORMs) and databases—as they sustain undesired dependencies and connections.


-


Providing for data integrity across boundaries in reaction to changes in other domains. Here, teams may choose to leverage Change Data Capture or a native Postgres cross-domain replication.


-


Creating a schema, database users, and groups to manage the ACLs for the schema, and moving a set of tables into it.


Organizing the database tables into separate domains, with the shared database slowly transitioning from one shared schema to multiple smaller schemas. Organizing the database tables into separate domains, with the shared database slowly transitioning from one shared schema to multiple smaller schemas.


At the end of this phase, denizens of the shared database could still access everyone else’s data—for example, with queries, using joins, and so on—but the ACLs will be explicit.


We designed this phase to minimize disruption, allowing for the change in database connection credentials to happen at any point. Phases 2 and 3 could then be done for each team individually.


#### Tracking progress


-


**#tables by schema:** We expect to see the number of tables in Postgres’ **default** schema go down with every split-out. We get this from Datadog’s **Postgres integration**` postgresql.table.count` metric aggregated by schema.


-


**#connections using the shared user:** The number should go to **0** .


### Phase 2: Access reduction and understanding access patterns


Phase 1 leaves us with domain boundaries with open gates. Effectively, the private schemas will start off being accessible by every single user in the database.


A prerequisite of physically moving a set of tables is to have no one but the owning teams depend on them directly; a direct dependency is a query to the database that hits those tables, including but not limited to JOIN statements. There are multiple approaches to removing direct access dependencies, but we favored the **build a service for teams that depend on your data** approach.


By using their dedicated PG Proxies, teams can begin to connect to the same database as always. At the proxy layer, we support service-level authentication so the service owners don’t have to worry about which specific user is used to access the database. All they need to do is change their database client, which will now connect them to a proxy, and the proxy handles authenticating with Postgres.


Once teams leverage the proxy, we can methodically narrow down the ACLs for every schema. Those may cause incidents, but rolling back a removal of a user from the ACL is simple, and thus far less risky than a physical split. When the ACLs for a schema include only the owning team’s user and all the traffic is flowing through the dedicated proxy, we are ready for a physical split.


#### Tracking progress


-


**#users in schema_ro_group and schema_rw_group:** This goes down until we’re left with a single user in each.


-


**schema_ro and schema_rw:** These are the only users accessing the schema.


### Phase 3: Physical separation


During this phase, we move schemas—and the tables within them—to their own database instance managed by OrgStore. The process is roughly:


1.


Set up continuous replication of the data to the new instance.


2.


Switch service(s) to read from the new instance, while still writing to the original.


3.


Revoke access for any user to perform writes in the original database.


4.


Switch service(s) to write to the new instance. This is the only step in which we encounter minimal downtime of less than five seconds to prevent data loss.


5.


Set up continuous replication of the data to the old instance to support rollbacks.


6.


Delete the data from the original instance.


A few of these things reuse components, wisdom, and/or automations from the OrgStore and Postgres teams. Specifically, this phase hinges on the PG Proxy.


Connect to the shared and the new database instances through a proxy. Applications update their code to read from the new instances; however, the proxy continues to send write traffic to the old instance. Once code is fully migrated to leverage the proxy, migrate all the writes to the new instance and replicate to the shared database. Connect to the shared and the new database instances through a proxy. Applications update their code to read from the new instances; however, the proxy continues to send write traffic to the old instance. Once code is fully migrated to leverage the proxy, migrate all the writes to the new instance and replicate to the shared database.


Make a dynamic configuration change to determine whether the proxy should route writes to the new instance, making it fully transparent to the applications. Make a dynamic configuration change to determine whether the proxy should route writes to the new instance, making it fully transparent to the applications.


#### Tracking progress


-


**#tables in the database:** This number can grow, but organic growth is small and relatively rare. If we move groups of tables out to OrgStore, this metric will have big drops—and, over time, it will converge toward 0.


-


**#schemas in the database:** This is an iffy metric because the number was initially 1. But it should increase as we split out the domains and establish a baseline. When a migration is complete, we delete the schema from the database, which results in a metric headed toward 0 as work progresses.


-


**Traffic to corresponding OrgStore clusters:** As we cut over traffic, we will see increased traffic in the OrgStore cluster until the migration is complete.


## Automate EVERYTHING


Although we could convince our peers that the plan is solid, unless we automate everything and test it to tears, everything that can go wrong will go wrong. Additionally, we will find out about all of the unique designs of our Postgres cloud providers.


Automating all of these operations does more than save time—it provides safety guarantees that artisanally crafted manual operations won’t. It provides consistency and the confidence we need in our assumptions being enforced.


Moreover, some of these operations (primarily Phase 2) make more sense to be handed off to the relevant teams. They definitely shouldn’t be in the business of running manual operations on a live serving database.


Having everything automated allowed us to cement our lessons into the automation, greatly reducing what we need to know or remember about the nuances of database management.


As we have continued to operate many Postgres databases, we have also found many other uses for pieces of this automation. Examples include cutting over between two database instances, extending or reducing access to a schema, and moving tables between schemas to either split or merge domains.


## Where we are now


We knew from the beginning that executing the migration would take a long time. Phase 1 started in Q1 2024 and finished in the middle of Q1 2025. Phase 2 started halfway into Q1 2024 and will continue through 2026. And Phase 3 started in Q3 2024 and will continue through 2026.


### 30+ schemas


Throughout the past year or so, we worked with every team that has data in this shared database to separate their tables into concrete domains. We created around 30 schemas that isolate the data into domains. Most domains have less than a dozen tables, but there are some larger domains. We were not really prescriptive as to how teams isolated those domains, and we remain flexible if teams want to modify them further.


This was a big milestone for us, as we were able to now understand what our future topology could look like.


Progression of schemas and tables from December 2023 through early April 2025, going from 326 tables to 257. Progression of schemas and tables from December 2023 through early April 2025, going from 326 tables to 257.


Here is the query we ran to get the above pie charts from Database Monitoring (DBM):


```text
1   (Schemas starting with `pg*` are Postgres’ own operational schemas.)    2
3   To coordinate Phase 1, we created low-severity incidents for each domain using [Datadog Incidents](https://app.datadoghq.com/incidents). This made communicating with the correct stakeholders and tracking all progress in a single place easy. We largely benefited from standardizing this process and leveraging past incidents to demonstrate to different teams what they can expect the migration to look like—a crucial piece to get buy-in and convince teams that, at this stage, we don’t require much from them at all.    4   <ContentImage src="unwinding-shared-database_screenshot_3.png" caption="Leveraging SEV-5 for migration tracking." alt="Leveraging SEV-5 for migration tracking." border="true" popup="true" />    5   ### Two full migrations    6
7   Alongside that effort, we were able to fully migrate—meaning, take through all phases—two workloads out of the shared database. Two product teams had their own reasons to do so:    8
9   - One team was motivated to have a dedicated database because they observed many instances in which sharing a database made incidents much more severe and, in some cases, impacted other teams. They felt that the cost of hosting their data in this shared environment was higher than the cost to migrate out. This team worked closely with us to carefully leverage the automation built for each phase across all data centers. Now this domain is fully served by an OrgStore database in all of our data centers and the team is able to scale and innovate on their domain with ease.    10   - Another team was building a new Datadog product but relied on specific data already stored in the shared database. This team was interested in building the product without any strong dependencies on that database. First, they didn’t want a new product to have the ability to jeopardize the stability of the rest of the application. They also knew that as part of developing a new product, they would need to increase their datasets, which meant they would increase their footprint on the database. Finally, they wanted to innovate alongside the platform teams, which meant accessing the latest infrastructure we have to offer.    11
12   These two teams were crucial for motivating us to complete and improve the access reduction and physical separation automations. We now know that our automation has been tested across multiple cloud providers and is able to support large workloads with minimal downtime. These success stories are also valuable for other teams, as they know that the platform teams are able to support them in making these types of migrations.    13
14   ### Sustaining momentum and scaling support    15
16   We’re proud of the solution we’ve developed for splitting our monolithic database, a significant step forward in leveraging our platform architecture. At the same time, our top priority remains providing customers with a reliable, secure, and seamless experience.    17
18   To achieve this, it's essential for our teams to clearly understand the problems they are solving and to align on the most effective and pragmatic path forward. While transitioning away from the monolithic database can be the right solution in many cases, we recognize that it may not be the best fit for every situation. Our goal is to continue evolving our platform and iterating on best practices in a thoughtful and strategic way.    19
20   Currently, our focus is on handling requests for physical database separations, while maintaining a healthy balance with other ongoing platform initiatives. We use the OKR planning process to surface and prioritize these requests just as we would any other work. As we jointly determine that a physical migration is the necessary approach, we work together to align on clear timelines and establish safe execution plans.    21
22   We rely on product teams to make the necessary code changes to connect to the new database and update their code to use a specialized client. We also need to make sure that the new database is provisioned with the correct capacity and the relevant services are able to access it.    23
24   These changes require revisiting a lot of application code, coordination among multiple teams, and thorough testing. This means that, despite the automation to execute the migration itself, the product teams are highly involved and they need support in ensuring their migration will be successful.    25
26   As we do more of these migrations, we continue to identify optimizations and request candid feedback on the experience. We do our best to strike the right balance between helping our peers with their migrations and continuing to make enhancements to the automation. Meanwhile, we also continue to innovate in other areas of our platform.    27
28   ## What we have learned so far    29
30   We have learned many lessons already and continue learning through this project. Let's walk through a few key lessons.    31
32   ### Platform success requires collaboration    33
34   One key lesson is the need to collaborate. The success of this project thus far has largely been due to our interest in solving the problems at hand and consistently having a two-way communication with each of the product teams involved.    35
36   There is an understanding that in migrating, the product teams have the ability to influence our roadmap and ask for new features that were previously hard to support in the shared environment. These requests also push us toward better testability, observability, reliability, scalability, and security. We have benefited from a symbiotic relationship as we each push ourselves to operate at a higher level and evolve together.    37
38   Through these communications, we are consistently learning from each other. There have been numerous conversations on this project at different times, with different stakeholders, and in different settings. Each conversation varies because we need to adapt the message based on the realities of each team and their top priorities.    39
40   ### Migrations reveal dead code and domain clarity    41
42   Some of the tables in the database are old, and none of the original engineers who created those features are around. As a result, the team may not be deeply familiar with the code surrounding the feature. By starting conversations around their data, we were often able to determine whether certain tables were still needed at all. These discussions led to the removal of more than 70 tables in the production database—helping teams clarify their datasets, delete dead code, and narrow the scope of their domain.    43
44   Some domains are more strongly intertwined; some tables are core to many features and product domains. We rely heavily on data-owning teams to manage their own dependencies and evolve their offerings so other teams can still read their data safely. It's easy to see why—many features depend on user-related information, for example. So if we tell these teams, “You can no longer directly access these tables,” what should they do instead?    45
46   Implementing new access patterns in tandem with the migration led to a lot of code that needs to be updated. Fortunately, several teams have become actively involved in supporting the decoupling effort. Without that shared investment, this migration project would not have progressed nearly as far as it has.    47
48   ### Flexibility beats purity    49
50   When we initially designed this process, we expected there to be small schemas with teams being able to make progress toward migrating their traffic at the schema level. However, some domains are still quite large, and the effort to migrate them is huge.    51
52   Teams opted to migrate traffic on a per-table basis depending on the risk each table presented. For example, the authentication team decided it would be best to first restrict write access to their tables. Then they moved on to restricting the read traffic. They did this for all of their tables one at a time. To ensure that new dependencies were not introduced, they need our help in supporting ACLs at the table level—a Postgres standard—rather than at the schema level. While this is technically possible, it certainly broke some of the assumptions we initially made as part of our tooling. Their priority here is to restrict the access to their data from other teams; however, they may not be feeling any pressing pain points from being in a shared database.    53
54   Combining the two preceding lessons, some teams can still make significant contributions toward the overall migration, but we have to negotiate. While the ideal state might be to say we no longer allow new tables in this database, it is important to remain pragmatic and focus on delivering value to our customers. We have worked with teams that needed new data to live in the same domain that exists in the shared database. For those cases, we were able to support adding the table within the restricted schema without compromising the objectives of the project.    55
56   ## What's next    57
58   Splitting a shared production database is one of the most complex infrastructure challenges a growing engineering organization can take on. At Datadog, this effort has required thoughtful platform investments, close collaboration across teams, and relentless focus on safety, automation, and scalability.    59
60   While the migration is still in progress, we've proven that with the right tools and partnerships in place, even deeply entrenched systems can evolve. We’re continuing to iterate, automate, and support teams on this journey—not just to reduce operational risk, but to unlock faster, more autonomous development for the long term.    61
62   Learn more in our talks on [the (big) picture of debt](https://www.youtube.com/watch?v=7TtDutHaV5U​​) and [making it easy to do the right thing with Postgres](https://www.youtube.com/watch?v=g9jn51ujjZI).    63
64   If this type of work intrigues you, consider applying to work for our engineering team. **[We’re hiring!](https://careers.datadoghq.com/all-jobs/?parent_department_Engineering%5B0%5D=Engineering)**
```


-
-
-
