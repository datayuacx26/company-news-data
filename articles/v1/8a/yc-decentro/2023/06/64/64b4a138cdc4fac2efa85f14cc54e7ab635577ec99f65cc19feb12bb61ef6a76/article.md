---
schema_version: "1.0.0"
document_id: "64b4a138cdc4fac2efa85f14cc54e7ab635577ec99f65cc19feb12bb61ef6a76"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-rss-d020e209c049"
canonical_url: "https://decentro.tech/blog/blue-green-deployment-aws/"
published_at: "2023-06-09T06:41:53+00:00"
first_seen_at: "2026-07-25T01:09:32.803029+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:8c2f118dc502935be2d3325e3332e000654dc7fdb90e865b0146c437682cb476"
---

# Minimize Downtimes in Financial Services with RDS Blue Green Deployment

Table of Contents


It was just another deployment cycle.


A housekeeping exercise.


A planned two-hour maintenance window turned into a six-hour nightmare as the update script took more time than expected.


Unfavorable? Yes.


Unacceptable? Not as much.


If you’ve spent a decent amount of time in software development, chances are you have encountered similar downtimes at least once in your career. As much as it’s a nightmare for the entire engineering team, its impact on the business is also pretty significant.


To put things into perspective, according to ****[Information Technology Intelligence Consulting (ITIC)](https://itic-corp.com/forty-percent-of-enterprises-say-hourly-downtime-costs-top-1million/?ref=geektime.com) **, 40% of enterprises said a single hour of downtime could cost between $1 million and over $5 million** – exclusive of any legal fees, fines, or penalties. In a[Gartner](http://blogs.gartner.com/andrew-lerner/2014/07/16/the-cost-of-downtime/?ref=geektime.com) survey, **98% of companies stated the cost of IT downtime ranged from $100,000 to $540,000 per hour** .


Now imagine the same in the context of working in the Fintech domain.


No enterprise has ever wanted downtime of their services, especially in the financial services industry. When your business use case relies on trust and funds, downtime’s impact snowballs into skewed customer experience, loss of trust, and brand value in the market.


So what is the way around it?


Downtime happens. It’s critical to acknowledge and accept. But there are steps companies can take to reduce and mitigate the risk. They can build resilient infrastructure and implement disaster recovery and redundancy across their infrastructure.


In the case of Decentro, it was a simple yet effective RDS Blue/Green Deployment.


##


What is RDS Blue/Green Deployment


Downtime can be a significant headache if you’re running a production database. Not only does it disrupt business operations, but it can also result in lost revenue, frustrated customers, and a tarnished reputation. Many organizations are turning to RDS Blue/Green deployments to minimize the impact of downtime. And so have we.


The general idea is that when you have an RDS instance, the tool will take the production DB environment (known as “Blue”) and create a read-replica (clone) of it in a new staging environment (also known as “Green”).


This clone is kept in sync with production by replicating all data updates. You can change the staging environment, like upgrading server types or applying security patches. When satisfied with the changes, you can switch the servers over in minutes, with minimal downtime and complete confidence that no data will be lost.


As it’s a managed service by AWS, you essentially are delegating the internal housekeeping to the AWS team, taking a significant load off your shoulders.


Benefits


### So if we had to make a checklist of its benefits, we would have


- **Managed service** : You don’t have to worry about maintenance, which eventually saves you time.
- **Minimized downtime** : By having two separate environments, you can switch between them as needed, minimizing the rest that your customers experience.
- **Ease of deployment** : With RDS Blue/Green deployments, you can deploy changes to the green environment and test them thoroughly before making them live. This helps to ensure that your changes are working as expected and reduces the risk of downtime.
- **Reduction of risk** : With RDS Blue/Green deployments, a fully functional production environment is always available. If anything goes wrong during the deployment, you can switch back to the blue background, ensuring your customers are not impacted.
- **Increased confidence** : By testing changes in a separate environment before making them live, you can improve your faith in the changes and reduce the risk of bugs or other issues cropping up in production.


Now that we have sold its benefits to you allow us to walk you through its seamless set-up!


##


Setting up RDS Blue-Green Deployment


We will use the AWS console to set up RDS Blue-Green Deployment.


**Step 1** : Log in to the AWS console, go to the RDS section, and click on the database for which you want to configure RDS Blue Green.


Step 1


**Step 2** : You will see ***Create Blue/Green Deployment under Actions*** . Let’s open that.


Step 2


**Step 3** : A form opens, and we must fill in a few basic details about our RDS Blue green setup.


Step 3


***Blue/Green Deployment identifier*** – Identifier for our setup. Let’s put “blue-green-setup.”


***Engine version for your Green database*** – We can upgrade the engine version. Major upgrades can introduce changes that are not compatible with existing applications. A minor upgrade includes only changes that are backward compatible with existing applications. To reduce complexity, select the same version in our central RDS instance.


***Parameter Group*** – A parameter group is a collection of engine configuration values you set for your RDS database instance. We will be using the same parameter group of our central RDS instance.


**Step 4** : It will take some time to provision the Green setup for our database. We have our blue-green setup ready. AWS will configure that replica of the db setup.


Step 4-a Step 4-b Step 4-c


**Applying changes**


As our blue-green setup is ready, we can connect to our green database and apply the necessary changes. The URL for the green database can be found in the RDS console. The username and password will remain the same as the blue (current database).


##


**The Switchover**


A switchover refers to redirecting the traffic from the blue environment (current production environment) to the green environment (updated environment) once the green environment is thoroughly tested and validated. The switchover is pivotal when the green environment becomes the new live production environment.


The Switchover


When you are ready for the switchover, start your downtime, as the database won’t be available during the switchover.


RDS gives you the option to configure the timeout for your switchover. The timeout setting in switchover refers to the duration for switching traffic from the blue to the green environment. It defines the maximum time the deployment process can take before timing out. If the switchover doesn’t complete within the configured time, the process will be aborted, and the original RDS instance will continue to serve traffic.


##


**Now for the things that need a little more attention,**


PSA before Switchover


Things that should be kept in mind while performing the switchover.


- **Replication lag:** Make sure there is no replication lag when switching over. This will increase the chances of the switchover not timing out.
- **Validation** : Once the traffic is redirected, monitoring the green environment is crucial to ensure it functions correctly and handles the incoming traffic as expected. This validation phase helps confirm that the deployment was successful and that the green environment operates as intended.
- **Rollback option** : It’s essential to have a rollback option available during the switchover procesIfase any unexpected issues arise with the green environment, you can quickly revert the traffic to the blue environment to minimize the impact on users.
- **Post-switchover activities** : After the switchover, it’s essential to conduct post-deployment activities, such as monitoring the system’s performance, analyzing logs, and performing any necessary cleanup tasks.


Develop via Developer Friendly Kit


### The following salient features also help you navigate the switchover seamlessly.


- RDS Blue/Green deployment does not mean Zero downtime deployment. As mentioned earlier, during the switchover, the database connection in your application will fail.
- Thoroughly test the DB instances in the green environment before switching over.
- When using a blue/green deployment to implement schema changes, make only replication-compatible changes.


- For example, you can add new columns at the end of a table, create indexes, or drop indexes without disrupting replication from the blue deployment to the green deployment. However, schema changes, such as renaming columns or re-naming tables, break binary log replication to the green deployment.[Click here for more details](https://dev.mysql.com/doc/refman/8.0/en/replication-features-differing-tables.html) .


- Please ensure your setup supports RDS Blue/Green deployment. In our setup, we were using RDS proxy, and RDS Blue/Green doesn’t support it, so we have to remove the RDS proxy from our setup to use this feature. You can find detailed limitations[here](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/blue-green-deployments-overview.html#blue-green-deployments-limitations) .


##


Conclusion


To conclude, RDS Blue/Green Deployment is a powerful strategy for reducing database downtime during updates. Organizations can minimize the customer impact and ensure uninterrupted service by maintaining two identical environments and carefully testing changes in the green environment before switching traffic.


With its minimal downtime approach, easy rollback capability, and simplified deployment process, RDS Blue/Green Deployment offers a reliable and efficient solution for managing database updates in production environments.


With that, we have come to the end of another immersive experience of the production world.


If you wish to read more such blogs, please check our website’s \[[Blogs](https://decentro.tech/blog/engineering-and-apis/) \] section. We have previously covered topics like[JsPDF](https://decentro.tech/blog/jspdf/) ,[Locust](https://decentro.tech/blog/locust-load-testing/) , and much more.


[Let’s Connect](https://decentro.tech/signup?)
