---
schema_version: "1.0.0"
document_id: "57b3700e4beb494f16a4899b2cdfaa0c48505b249e1d69c13fdeede63cf04f6b"
company_key: "yc-emi-labs"
company: "Emi Labs"
source_id: "yc-emi-labs-rss-87232385bc09"
canonical_url: "https://medium.com/@EmiLabsTech/our-data-stack-375255c20377"
published_at: "2021-09-02T13:28:59+00:00"
first_seen_at: "2026-07-27T09:02:34.241036+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:714dd5d6e19a87a0ab6681220fbbbc6e466bcd4d53da9b4f8fdf2bd617f697d1"
---

# Our Data Stack

Data Science


Database


Etl


Visualization


Sql


# Our Data Stack


[Emi Labs Tech - Ravens](https://medium.com/@EmiLabsTech?source=post_page---byline--375255c20377---------------------------------------)


5 min read


·


Sep 2, 2021


--


*by*[Nico Oteiza](https://www.linkedin.com/in/nicolasoteiza)


Press enter or click to view image in full size


Data Stack, image from[https://chartio.com/](https://chartio.com/)


On this article, we’d like to share how we defined our data analytics stack. It’s nothing fancy, but it has brought us here with a small team and has let us:


- create some very useful and decent looking BI dashboards
- be pretty data driven internally


We hope this can help other teams/companies go faster, and if not, at least we wanted to show how we make (tech) decisions at Emi.


## The scenario


Being a small startup, just testing out ideas, it’s not uncommon to use the same database for operations and for analytics. Of course, this is not recommended as you grow, as the loads of operations could be different to the ones for analytics, and, in most of the cases, you would not want a query for analytics to be blocking a productive process, so it’s a common practice to separate concerns, and to do that by using different databases, one for the service itself, and another one for analytics.


In our case, added to that is that as we have a micro-services architecture, and each service has it’s own DB, we also had the need of a way of combining the data from multiple services to get the “full picture”.


These two ideas were the starting point for building the data analytics stack we have today.


## The business need


As always at Emi, at the heart of every decision we make, is the following question: “what problem are we trying to solve?”. In this case, we put out a list that looked something like this:
1) Avoid analytics workload from affecting operation of services
2) Be able to quickly join data from different services
3) Be able to show embedded analytics in our web app for our clients, without needing the Frontend Team’s help.


Ok, maybe not the most impressive list, but it helped us move forward with a clearer objective. For the first point, it is clear we would need a separate data stack for analytics. From the second point, it was clear we would have to define some sort of extraction process. With respects to the third point, it brings some constraints to the type of BI tool that we should choose, but looks like a standard requirement.


## Data Storage


Ok, so what should our data be stored? Should we use the same relational databases we were using on our services, or should we move to a more warehouse-specific technology?


Press enter or click to view image in full size


PostgreSQL


Being a very small data analytics team (at the time of having to make this decision, only 1 person was dedicated to this), we decided to stick with the technology we best knew (PostgreSQL in AWS’s RDS), betting that it would work for the moment and for the next couple of years. This has been a great choice in retrospect, even when today (2 years later) we are considering other alternatives (Snowflake, Google Big Query).


## Data Extraction


Press enter or click to view image in full size


Apache Airflow


This was a bit more tricky for us, as we had no experience managing or designing productive ETL’s, and we had heard some people say it would be a nightmare. On the other hand, hiring a data engineering specialist at this stage seemed too much: we didn’t have many challenges like this to keep them entertained, and it seemed like something we could bootstrap for some time.


We finally gave it a try ourselves. ** As we are confortable with Python, we decided we’d use Airflow, which is pure python with an out-of-the-box UI to manage DAGs. It was a challenge at first, but, again, in retrospect, we believe it has been a great decision, as it has given us both the confidence to keep building on top of this, or to delegate this to a third party now that we have an idea of what it costs to do it ourselves. This allowed us to replicate the services DBs and to apply transformations to make the data more useful for end users fairly easily.


## Analytics


This is where we would finally be showing value to our clients, so we had to make this work for us: everyone at Emi should be able to access data and analytics reports, new reports and dashboards should be easy to build by a data analyst and we needed to be able to embed dashboards into our web app in a way that their data was secure (one client shouldn’t be able to see other clients’ data).


Press enter or click to view image in full size


Metabase


We evaluated a few tools, and decided to try out self-hosted versions of[Redash](https://redash.io/) and[Metabase](https://www.metabase.com/) , in the spirit of trying both out and deciding after testing them in action. After a few weeks, we stuck with Metabase, that at the time had the ability to embed dashboards using parameters, which Redash did not, and this was key for our use case.


## Limitations of our stack


As every decision in tech, we’ve had to make some tradeoffs, so here are the weak points of our stack. The extent to which these affect you will depend on your use case and please let us know if you have others :)


- PostgreSQL storage in AWS may become expensive if the warehouse becomes bigger. Simply replicating all productive DBs is OK at first, but eventually it will make sense to start choosing what to replicate, or to transform before loading the data.
- PostgreSQL is not optimized for complex queries with many joins on big tables, it’s aimed at production data store, so it can start to become too slow at a certain point
- Airflow had a not-so-easy learning curve. Once we understood how to use it, we were OK, but it’s not so easy to add new[DAGs](https://airflow.apache.org/docs/apache-airflow/stable/concepts/dags.html) . Some python knowledge is needed.[There are tools](https://hevodata.com/) that abstract this part away and could be a better choice if you want to simplify this a bit.
- Metabase is great for the 80–90% of the things we do. It is *not-so-great* at mapping abilities, so we usually end up doing something ad-hoc when we have this need. Also, it is not trivial to use the self-hosted option and make it work well with lots of users. We had a few issues with it’s internal DB a few times. Other than that, it has been great.


## Reflection


There are many ways to do this, and this is not necessarily the best today, but it worked for us, and maybe, at least the thought process, could help someone else building up their data stack for analytics.


If you have any questions or doubts on details or implementation of the different parts of the process, comment here or email me at nico@emilabs.ai.


If you’ve read the article and would like to join us in our quest to increase frontline workers access to professional opportunities,[join us](https://jobs.lever.co/emilabs) !
