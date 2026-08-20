---
schema_version: "1.0.0"
document_id: "a42105cacb31ee6b495419ca8a7493d24ea11d8d252df615e8a17a65fcb5f2d1"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/1-000-servers-160-clusters-30-days-zero-downtime-migrating-wix-s-mysql-fleet-to-graviton"
published_at: "2026-03-23T11:27:39+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:78cfe4ff12b1a4f9cb73eeb36a2c88653cb3570365d8a9901a1842a6d94585e7"
---

# 1,000 Servers. 160 Clusters. 30 Days. Zero Downtime: Migrating Wix’s MySQL Fleet to Graviton

Last January, the DB Infra team at Wix embarked on a strategic and very important mission: Migrating all of our MySQL EC2 servers from Intel based CPUs to the new shiny Graviton ones.


Not only will this allow us to work on better CPUs, saving us money and improving our databases, we’ll be able to move to Amazon Linux 2023 OS and move from the EOL CentOS 7.0.


That sounds simple enough, until you realize that we’re dealing with over 1K servers spread over 160+ MySQL clusters. The initial project timeline was set for 3 months.


**We did it in** **one** **.**


###


The Challenge


The main goal here was to do as little manual labor as possible: We can’t realistically manually create and sync over 160 MySQL clusters. The process should be as intuitive as pushing a button and letting AWX (Ansible Task Engine) and Wix’s own Dev Portal do the heavy lifting.


That’s challenging, as MySQL is an RDBMS with an Active-Passive cluster configuration on multiple regions, meaning that there are usually many things you have to consider when adding and removing nodes from a cluster that needs to be as reliable as possible (for example, accounting to replication lag issues, co-primary switches over DCs, etc.)


We could just throw money on the problem: Adding a ton of nodes to each cluster, move the DB nodes and be done with it, but it’s our motto in the Data Platform group to be as efficient as possible - and that includes the total cost of this project.


Wix is also a big company with millions of active users: production incidents and downtime are simply out of the question, and with over 160 MySQL clusters, we had to make sure that everything we do is seamless to both our devs and our users in the business end. Any production error would be critical here.


###


So, how did we do it?


We knew we couldn't do this on our own, so we enlisted the help of the DB Group’s own DevOps team as well as a development team that specializes in the dev to DB connection.


First step was the design of the process: Together, we broke the process down to literal


**building blocks** , each one representing a manual job a DBA would do in order to make this cluster change: Adding new MySQL nodes, replacing storage, putting old Intel nodes in “maintenance mode” to exclude them from production traffic in our ProxySQLs, etc.


The next step was to start implementing the workflow: The two dev teams took our guidelines and the building blocks established earlier, and used AWX to create jobs, and workflows that call those jobs at the correct time and order for each MySQL cluster, working on each of our DCs and taking into consideration node roles such as Primary RW\\RO nodes, BI nodes and more. The goal here was to emulate the DBA’s mindset when doing that type of work and even add stuff like checks and tests to the workflow.


We also tried making smart decisions regarding the replication process: We added new nodes to the clusters, sure, but where we could, we implemented a


**disk switch** that saved us some time (due to the binlogs already being present on the disk, therefore only having to replicate the delta between the start and finish of the disk switch process).


It was also at this point that we decided on a “no rollback” policy. Sure, we had a rollback plan if needed, but the official party line was that we do not roll back. Once we start the workflow - we don’t stop until we’ve moved the entire cluster to Graviton. This wasn’t blind faith though; it was backed by rigorous automated health checks that ensured any failure was caught and fixed forward immediately.


That mindset is what allowed us to move at such a fast pace. An issue, error in the workflow, or a simple mistake didn’t stop the process, on the contrary - it expedited fixes and made sure that the next runs would be even smoother.


While the workflows were being built, the DB team started to map our MySQL clusters and start working on


**a rollout plan** . This included separating clusters into


**three tiers** by load on the cluster, sensitivity of data and the data size itself, keeping the more sensitive clusters for the end.


Once all the pieces were in place, the process was


**fully automated** , and ran on our


**Internal Developer Platform (IDP),** meaning that from the moment we started working on a MySQL DB cluster, all the DBA had to do is monitor the situation from time to time to make sure we didn’t have errors in the process (and if there were, to contact the DevOps or fix the issue on their own).


We have encountered several edge cases during the rollout, such as clusters with very sensitive query cache usage that had query execution time issues, but we pushed through and worked through these challenges, in order to finish this as best and as quickly as possible.


###


The Results


In the end, the process took as about a month, with the end result being a full on migration of


**all** of our production serving MySQL nodes to Amazon Graviton CPUs, a


**reduction of ~15%** in the MySQL compute costs and


**up to ~50% reduction** in CPU utilization.


We also made sure to


**consolidate availability zones** in each Region to further save on AZ data transfer costs, and had the less expected outcome of lowering our total core usage by


**500 cores** as well as reducing the


**total IOPs and throughput** being used by our MySQL clusters’ disks by


**160K** and


**9K(MB/s)** respectively.


###


That’s nice - but what did we learn from the project?


1.


**Perfect is the enemy of progress** : The design wasn’t air-tight, we could’ve perfected the process in the design stage even more, but in the end that would’ve hindered both our growth and our speed. We ran the process, we found issues, we fixed those issues, we moved on to finish way ahead of schedule.


2.


**A project within a project:** We could’ve simply migrated the clusters to Graviton and be done with it, but we took the extra step to look into our clusters and find overprovisioned nodes that we can downscale and shrink. This helped us reduce our total cores usage by 500(!) cores as well as reducing the IOps and throughput usage of our disks.


3.


**The whole is greater than the sum of its parts:** DBAs, DevOps and developers worked together on the project, using each other’s expertise in order to create a working, automated solution which is fast, and seamless for our users. This couldn’t have been done the way it was done by each team on their own, and that cooperation created a faster and more efficient process.


As Wix keeps growing and changing, we realize that Wix Engineering’s principles of growth and speed apply to everything we do - whether it’s developing new tools for our users or improving the infrastructure costs of our Databases.


This post was written by


**Shmuel Mekonen**


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/) |


[Instagram](https://www.instagram.com/wix_eng/)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/subscribe)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)


-


Listen to our podcast on


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) ,


[Spotify](https://open.spotify.com/show/5CmjtjpdcKkHDnr0601uYS?si=PcOf7Rx_RUmGojFj5n7CEA)
