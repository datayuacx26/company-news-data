---
schema_version: "1.0.0"
document_id: "f050f982e11ea59d540812afaca77a2737831d6e7765e3061579273dc48d747b"
company_key: "vtex-class-a-common-shares"
company: "VTEX"
source_id: "vtex-class-a-common-shares-rss-fffff04a1e70"
canonical_url: "https://vtextech.substack.com/p/how-vtex-improved-the-shopper-experience"
published_at: "2024-04-16T10:28:00+00:00"
first_seen_at: "2026-07-20T23:19:02.969240+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:6ae60274e98f9f928b5d0a53e6b7d27bfc7754e7d38e2bbd950798b8273803aa"
---

# How VTEX improved the shopper experience with Amazon DynamoDB

# How VTEX improved the shopper experience with Amazon DynamoDB


### Modernizing VTEX Checkout data layer to shopping cart latency.


[VTEX Tech Blog](https://substack.com/@vtextech)


,[Mateus Ribeiro](https://substack.com/@mateusribeiro)


, and[Tiaraju Smaneoto](https://substack.com/@tiarajusmaneoto)


Apr 16, 2024


In this post, we discuss one of our most recent systems modernization using AWS, which aimed to improve shopping cart latency and increase efficiency on a per-operation basis during the shopping experience.


## The need for speed


Shopping cart management is the core of any ecommerce system. It must be simple, fast, and reliable. A shopping cart can vary in size given the different kinds and volumes of information inserted on it, such as items, addresses, and so on.


For several years, VTEX used Amazon Simple Storage Service (


[Amazon S3](https://aws.amazon.com/pm/serv-s3/?gclid=Cj0KCQjwh7K1BhCZARIsAKOrVqHmo-rXcNdbgx68kjyXPYDLeSRwydMNzcI9g6ZzV_45QbawaB1ElwoaAt1WEALw_wcB&trk=cce3a0ea-fd50-4a4d-8c07-b4d2a4749fa4&sc_channel=ps&ef_id=Cj0KCQjwh7K1BhCZARIsAKOrVqHmo-rXcNdbgx68kjyXPYDLeSRwydMNzcI9g6ZzV_45QbawaB1ElwoaAt1WEALw_wcB:G:s&s_kwcid=AL!4422!3!536324531646!e!!g!!aws%20s3!12195830339!119606871280) ) as its primary storage layer for shopping carts. Its reliability and object size limit flexibility were advantages for a long time. For example, a single shopping cart would vary from 1 KB up to 50 MB, with the number of PUT requests over 300,000 per minute during peak traffic hours.


In the ecommerce segment, faster response times lead to higher conversion rates. We wanted to improve both the purchase experience and customer conversion rates.


To accomplish this goal, we evaluated changes to improve the performance of our data storage. We chose


[Amazon DynamoDB](https://aws.amazon.com/dynamodb/) due to its low latency, its managed service capabilities that removed some of the heavy lifting of maintaining database instances, and the fact that DynamoDB was already deployed as part of the company's stack in areas such as data and analytics.


## Challenges


We ran a proof of concept to better understand if DynamoDB would meet our requirements. After a closer evaluation of our data, we realized that some of our shopping cart items were bigger than 400 KB, which is the


[DynamoDB item size limit](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ServiceQuotas.html) .


We reduced our shopping cart data by more than 60% on average with the following strategies:


-


**Payload optimization** – We removed legacy fields that weren’t used anymore by any API processing or response.


-


**Removing default fields** – We removed additional unnecessary data from the shopping carts, including null fields. The de-serialization process will create these values on the application side.


-


**Compression** – We added a compression step before saving it to DynamoDB as a byte array field.


After that, the remaining larger objects (greater than 400 KB) would be offloaded to Amazon S3.


## Solution overview


VTEX decided to migrate to the new solution gradually, avoiding unnecessary risks and taking the time to validate associated metrics.


1.


In the initial stage, we wrote data in DynamoDB and Amazon S3 but only read from Amazon S3.


2.


Later, we switched to primarily reading from DynamoDB (fallback readings were still going to Amazon S3) but continued to write on both.


3.


In the third step, we stopped writing in Amazon S3 and used it solely for reading fallback and offloading objects.


4.


Finally, we stopped using Amazon S3 as a fallback for reading and kept it only to store large objects that are larger than DynamoDB items size limit.


The following table shows the type of use for each storage service along the four phases. DynamoDB proved itself as a faster and more efficient solution for our current and near-future use cases.


## Migration process


VTEX had over 4 billion items—70 TB in total—to migrate. Because this was a mission-critical application, the team performed a gradual migration with the help of Amazon S3


[TTL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-expire-general-considerations.html) . We divided the migration in four different phases.


#### Passive replication


We wrote the shopping cart elements to DynamoDB and Amazon S3 to prepare for the migration. This allow us evaluate how much capacity we would consume in DynamoDB, as well as its error rate and throttling, and keep writing in Amazon S3 in case of rollback.


#### Fallback reading


We opted to point the application to DynamoDB after 10 days of dual writes while relying on Amazon S3 to handle requests for data older than 10 days. We still wrote in both databases in case of rollback.


We saw consistent latency improvements by adopting DynamoDB for this workload, as shown in the following figure.


#### Stop replication


We kept replicating writes to Amazon S3 to be prepared in case of rollback. However, when the team became confident that phase two was successful, we stopped writing to Amazon S3. This step has had a tremendous impact on latency while removing the possibility of rolling back to Amazon S3 without losing shopping carts.


The new approach helped us reduce shopping cart API latency by around 30%, as illustrated in the following figure.


Also, in the following AWS Cost and Usage Report (CUR), we ‌see the efficiency gains at this stage.


#### Primary DynamoDB


Finally, we stopped using Amazon S3 as the fallback for reading and just use it to offload files larger than 400 KB. With that, we have the final latency improvement.


## Conclusion


In this post, we showed you how VTEX modernized its shopping cart system with Amazon DynamoDB. By doing that, VTEX was able to improve its shopping cart API’s latency by about 30%, improving approximately 70% of its shopping cart storage investments with a smooth and safe migration process.


Managing a solution with millions of requests per minute poses a great technical challenge. It’s crucial that the solution is efficient and reliable, with low latency, in order to apply to the business.


> *This article was produced in collaboration with AWS. **[Original post](https://aws.amazon.com/blogs/database/how-vtex-improved-the-shopper-experience-with-amazon-dynamodb/)** co-written with Alberto Frocht*


---


Thanks for reading VTEX’s Tech Blog! Subscribe for free to receive new posts.


A guest post by


[Mateus Ribeiro](https://substack.com/@mateusribeiro?utm_campaign=guest_post_bio&utm_medium=web)


Software Engineer at VTEX who loves to face technical challenges and provide great and innovative solutions to solve customer issues.


[Subscribe to Mateus](https://mateusribeiro.substack.com/subscribe?)


A guest post by


[Tiaraju Smaneoto](https://substack.com/@tiarajusmaneoto?utm_campaign=guest_post_bio&utm_medium=web)


Staff Software Engineer at VTEX


[Subscribe to Tiaraju](https://tiarajusmaneoto.substack.com/subscribe?)
