---
schema_version: "1.0.0"
document_id: "b6ec0869c6b5c54971fa638f80c278440b80075bda91904d1e94e6eccefd8681"
company_key: "yc-etleap"
company: "Etleap"
source_id: "yc-etleap-news-import-75d796be5177"
canonical_url: "https://etleap.com/blog/run-etleap-inside-your-vpc"
published_at: null
first_seen_at: "2026-07-21T18:50:49.183689+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:4644a55af9c93beefb6cca22d92766af4d1788a6adeea224b3d720cb9b756efa"
---

# Run Etleap Inside Your VPC

Did you know that you can run Etleap inside your VPC?


In case you missed it,[Etleap VPC is available on AWS Marketplace](https://aws.amazon.com/marketplace/pp/B08C4KVR5Z/) , which means customers can run Etleap inside their own AWS Virtual Private Cloud (VPC) with just a few clicks.


**We’ve now made Etleap**[available as a Terraform module](https://registry.terraform.io/modules/etleap/etleap-vpc/aws/latest) **to simplify deployment and upgrades.**


So what is Etleap VPC and why did we build it? First I’ll make the case for the VPC SaaS model being the future of enterprise ETL. Then I’ll highlight some of the reasons why being hosted SaaS first has been tremendously beneficial to Etleap’s development as a company.


## How did we get here?


Every organization that has a data warehouse or lake has to deal with ETL – after all, what good is a fancy data repository without up-to-date and clean data in it? Traditionally, ETL has been expensive and time-consuming. Learning complex ETL software, building[Kimball modeling](https://en.wikipedia.org/wiki/Dimensional_modeling) processes, and setting up compute clusters dedicated to ETL led to projects measured in months or years with 7-figure budgets. And that’s before the operational costs of handling changes and errors.


Enter the cloud. Data warehouses like[Amazon Redshift](https://aws.amazon.com/redshift/) and[Snowflake](https://www.snowflake.com/) have taken advantage of cloud computing primitives to deliver vastly superior user experiences, offering scalability and flexibility at a fraction of the cost of traditional data warehouses. Today, cloud-native ETL products tailored to these technologies and to modern data teams’ workflows are starting to eliminate the headaches of traditional ETL projects.


## Advantages of Cloud-Native, Managed SaaS ETL


What makes cloud-native ETL different? Some products, like Etleap’s ETL solution, are easy to learn and let data engineers create ETL pipelines that are fully-managed. This means that customers don’t need a dedicated ops team to operate the hardware that the ETL software runs on, or to manage pipelines and fixing errors. Pipelines can be set up quickly from any data source and transformations to make the data useful can be defined without coding.


This is great news for data teams, because they can avoid hiring engineers dedicated to ETL, and they can get up and running in days or weeks instead of months or years. Teams often see an order-of-magnitude gain in data team productivity as a result. At the same time, it means that a tremendous amount of trust is placed in the product’s data security and operation by the customer.


## Towards the Virtual Private Cloud (VPC)


A question we have been asked many times over the years by our customers is whether we can operate inside their own Virtual Private Cloud (VPC). The security benefit to the customer is that data doesn’t flow outside their VPC on the way from their source to the warehouse or lake, and they have more direct control over infrastructure and data access policies. While hosted Etleap uses only S3 buckets owned by the customer for intermediate data storage, data passes through servers managed by Etleap for processing. For the most privacy-sensitive companies, this is a non-starter. In order to adopt the cloud, it is an absolute requirement that their data remains tightly controlled inside their VPC.


While a large segment of the market is happy to adopt, and even prefers, hosted ETL services, it is now clear to us that enterprise companies are often not, and probably never will be. We asked ourselves what this meant for our offering, and the answer we have come up with is that running Etleap inside one’s own VPC should “feel” just like using the hosted product; hiring ETL engineers should not be necessary and pipelines should “just work”. This is of course easier said than done – without our experienced ops team having direct access to operate infrastructure and troubleshoot pipeline issues, how can we offer the same experience?


Through working with early adopters of our VPC offering we have hardened the infrastructure components in order to become virtually management-free, and honed in on the required operational metrics and de-identified logs shipped back to Etleap’s ops team in order to enable our team to offer proactive pipeline support and assist with reactive troubleshooting.


Setting up Etleap is designed to be fast and pain-free, and it was important to us that we offer the same effortless experience for customers that want to run Etleap inside their own VPC. Using[AWS CloudFormation](https://aws.amazon.com/cloudformation/) the setup of infrastructure components is fully automated, including an automatically scaling[AWS EMR](https://aws.amazon.com/emr) cluster that runs extractions and transformations. Customers enter their name and email address, press “play” on the CloudFormation template, and minutes later they can set up Etleap pipelines in their browser.


## Benefits of Being SaaS-First


Creating a robust ETL system is all about handling edge cases. It’s relatively straight-forward to build a system to ingest files from an SFTP server or replication logs from a SQL database into a warehouse. The complexity is in how you handle issues like errors in the data, schemas that change in unpredictable ways, or ingesting incrementally from an S3 bucket that contains tens of millions of non-alphabetically ordered files. Our focus is on providing intuitive solutions to all these issues, while at the same time making the ETL software a pleasure to use.


Etleap’s hosted multi-tenant deployment is and will continue to be the biggest Etleap deployment in terms of data scale and number of users. This is a great benefit to customers running Etleap inside their own VPCs because of the continuous improvements we make to the scalability, usability, and flexibility of the solution.


Being SaaS-first has enabled us to work very closely with our customers. From the beginning, we set the high bar that pipelines should “just work”, no matter the source, quantity of data, or transformation complexity. Over the last 7 years, we have been fortunate to experience our customer’s new challenges first-hand every day, and as a result have been able to build a robust system that meets their needs. Today, we are thrilled to provide the same pain-free and effortless experience for customers that want to run Etleap inside their own VPC.
