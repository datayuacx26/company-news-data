---
schema_version: "1.0.0"
document_id: "619f9bdb8915847ea54529f0b3cac161d2a9beae8f8634cda6ba4111a8108a50"
company_key: "box-inc-class-a-common-stock"
company: "Box Inc."
source_id: "box-inc-class-a-common-stock-rss-6b6ba587c738"
canonical_url: "https://medium.com/box-tech-blog/creating-cloud-managed-platform-services-93a52233c8a9"
published_at: "2024-08-05T16:15:54+00:00"
first_seen_at: "2026-07-20T04:36:04.074136+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:9d6df0ddcf021195f6f33fe19bdec0ee8e4de5e5da8b9e4e8a5c561b91e26bcf"
---

# Creating Cloud Managed Platform Services

Learning And Development


Cloud Services


Cloud Platform


# Creating Cloud Managed Platform Services


[ssuresh](https://medium.com/@suhas-suresh?source=post_page---byline--93a52233c8a9---------------------------------------)


7 min read


·


Aug 5, 2024


--


Press enter or click to view image in full size


Illustrated by Navied Mahdavian / Art directed by Erin Ruvalcaba Grogan


## Infrastructure Paradigm Shift


Every tech company goes through a phase of deciding to adopting Public Cloud or managing their own infrastructure. After more than a decade of self-hosting it’s cloud infrastructure, Box decided to embrace the Public Cloud and chose Google Cloud Platform(GCP) as the preferred Cloud provider. It was a shift in the operating model for almost all Engineering teams as we migrated from our on-premise infrastructure to managed services offerings by our Cloud Provider to handle all our compute, storage and networking needs. In this article we will go through one of the frameworks Box adopted to navigate and integrate with Google Managed Services like BigTable, PubSub, BigQuery, Cloud SQL etc to operate natively in the cloud.


One of the cloud migration challenges was to meet our increasing demand for a solution that can handle **relational database needs** for both **internal and non-customer data** . Even though Box has a dedicated database team, their primary focus was to provide custom database solutions to our core applications by optimizing and scaling self-managed database fleets that needed a higher level of performance and custom integration with our data access layers. These customizations were not applicable for most of the non core applications and often required service owners to rely on our database teams for any database related customizations and support.


In order to solve the ever increasing database needs of the non core applications in a consistent and reliable manner in GCP, Box Engineering decided to design a platform engineering approach to provide a secure, mature and an enterprise grade data store solution. This approach had several positive outcomes like


- Reduced database creation time by more than 50%.
- Eliminated custom database configurations via pre-configured settings leveraging[Infrastructure as Code](https://en.wikipedia.org/wiki/Infrastructure_as_code) (IaC). Read about our IaC design in[Box CMF: Infrastructure as Code, Then What?!?](https://medium.com/box-tech-blog/box-cmf-infrastructure-as-code-then-what-8b5b69e50c22)
- Standardizing access patterns to follow security best practices.


> Service typically refers to an application running on containers or Virtual Machines(VMs)


### Rise of Platforms


The shift to Public Cloud provided us opportunities to ensure future infrastructure needs will follow a platform engineering approach where teams can buy into an platform solution created by leveraging managed services by Google and adding Box specific customizations to ensure we adhere to our strict Security, Legal, Privacy and Compliance(SLPC) requirements Eg DataPlatform handles all applications requiring data transformation and management capabilities leveraging solutions like BigQuery, Cloud SQL, Pub Sub, BigTable etc.


Press enter or click to view image in full size


## The Relational Challenge


[MySQL](https://dev.mysql.com/doc/refman/8.0/en/what-is-mysql.html) is used[extensively at Box as a Relational Database(RDB)](https://medium.com/box-tech-blog/strategies-used-at-box-to-protect-mysql-at-scale-35388b85f2a4) solution.[Mojito](https://github.com/box/mojito) (Box’s open source localization platform), our in-house real time user availability measuring tool, our automated infrastructure diagnosis tool etc are some of the internal services running on MySQL. The on-premise version of managed MySQL was operation heavy for both the database team and the service owner as implementation was highly customized per team. This approach created significant challenges for datastore integration and management for service owners and often required support from database team.


### Service Owner Challenges


- Limited expertise and operational knowledge to own and maintain a relational database in GCP.
- Lack of standardized processes to configure database resources and manage their usage within GCP.
- Getting approval to Box’s extremely diligent SLPC requirements for data storage in GCP per database.


### Solution


It was a collaboration that brought together teams from different functions, working in harmony to reach a consensus on the way forward. After engaging in discussions and deliberations, these were the decisions that emerged.


Press enter or click to view image in full size


Source of image[cgdream.ai](https://cgdream.ai/)


In order to efficiently operate in GCP, we decided to leverage platform engineering practices to reduce friction by streamlining processes to improving efficiency and provide seamless integration across different infrastructure components. Google’s **Cloud SQL** fit this use case perfectly as a platform based solution to solve all future non customer related RDB needs with appropriate standardization, governance and security. Site Reliability Engineering(SRE) team alongside Database Infra teams collaborated closely to design and develop an intuitive solution leveraging our Dataplatform(DP).


**Shared Ownership Model**


- DP to design and guide in enabling Cloud SQL as a capability via IaC.
- SRE team to collaborate with database teams to deliver a mature, enterprise grade RDB solution emphasizing security, performance and operational ease.
- Security team to ensure the solution adheres to company’s standardized SLPC requirements for data storage


## Platform Bring Up


### Due Diligence : A Comprehensive Review Process


Press enter or click to view image in full size


Source of image[cgdream.ai](https://cgdream.ai/)


During the initial proposal for our Cloud SQL solution, we encountered several unanswered questions that needed addressing. To ensure a robust implementation, we engaged in an enlightening meeting with the GCP team. This session shed light on upcoming features in GCP’s roadmap for Cloud SQL — features that were already supported by Box’s self-managed MySQL fleet. One of the key requirements was to ensure we periodically rotate our encryption key version used in any data store, at the time, it wasn’t a generally available feature from Google.


At Box Engineering, we take a meticulous approach in analyzing any solution with SLPC (Security, Legal, Privacy & Compliance) at the forefront. As guardians of data security within our organization, maintaining high standards is non-negotiable in the matters of approving a new application. One of the major considerations for even considering Cloud SQL as a viable managed service was the[FedRamp High Compliance](https://cloud.google.com/security/compliance/fedramp) provided by Google. After careful examination of all the regulatory requirements for a data store, our security architects approved the proposal to use Cloud SQL at Box with few caveats:


- **No customer related data(as encryption key rotation was not generally available at the time of implementation)**
- Identity and Access Management ( **IAM** ) based authentication
- Standard encryption leveraging[Customer-managed encryption keys (CMEK)](https://cloud.google.com/kms/docs/cmek) **** to ensure secure access to data.


It was still a win for Box Engineering as it paved a way for future micro-services to easily integrate with a relational data store with limited operational overhead for service owners.


### Implementation


In order to ensure ease of use, we needed to exposed only the required inputs for database provisioning in our IaC. The inputs needed to be agnostic of the underlying database engine as we had few use cases apart from MySQL.


The final result was an intuitive IaC interface that could be effortlessly configured with just a few lines of configuration code. By abstracting most of the provisioning logic from the user, we were able to significantly reduce instance setup time. This powerful platform capability empowered every member of our Box team to effortlessly create Relational Database for their applications, with minimal operational overhead.


Creating a new relational database is as simple as defining a block of configuration with necessary attributes as defined below.


```text
variable "cloudsql_instances" {    type = map(object({      database_version      = string,         // API enum string (e.g. MYSQL_8_0, POSTGRES_14)      tier                  = string,         // API tier string (e.g. db-custom-1-3840)      region                = string,         // us-west1 or us-central1      disk_size             = number,         // In GB (e.g. 10, 40) Cannot be less than 10 GB      ha_enabled            = bool,      read_replicas         = map(number),    // Setting the number of read replicas for zone and/or cross-regions      readers               = list(string),   // List of Service Accounts with read permission      writers               = list(string),   // List of Service Accounts with write permission      admins                = list(string),   // List of Service Accounts with admin permission      maintenance_config    = map(number),    // Specify the day of the week (1-7) and hour of the day (0-23) for maintenance      retain_backups        = bool,                backup_retention_days = number,         // Please specify 3 at a minimum      additional_databases  = list(string)    // list of databases to create     }))    default = {    }  }
```


### Secure and Simple access


Imagine a solution to make it impossible for anyone to breach database passwords! Sounds amazing right? That’s exactly the access model for Cloud SQL. All access to cloud native applications follow our IAM-based authentication and authorization best practices framework.


## Adoption and Support


At Box, we believe in embracing an “Be an Owner” mindset, which is reflected in our approach to the solution process. As a platform user, every Box engineer has the power to leverage solutions and onboard to the required platform through a self-service document. It is the responsibility of the platform owner to create a comprehensive user document, while as a service owner, you have the opportunity to provide feedback and make contributions for future users.


Initially, the requirement was limited to a handful of identified services. However, the adoption continues to increase as more teams at Box recognize the value of leveraging the platform based relational database solution for building robust backends for their applications.


To ensure continuous improvement, we actively collect feedback through a dedicated Slack channel created for Cloud SQL adoption. This valuable input stream allows us to refine our solution on an ongoing basis.


## Conclusion


Having a platform-based solution has really helped service owners at Box accelerate their development process and focus on the application logic to ship features at a rapid rate. Our internal tooling has matured considerably as new solutions were able to easily integrate with Cloud SQL for all the relational database needs.


Developing platform capabilities requires both problem-solving skills and long-term vision towards scalability and adoption. It is crucial to always maintain open lines of communication with users; no product can ever be considered truly complete as there are always opportunities for enhancement.


There is a growing ask to add support to data storage requiring PII/Customer leveraging Cloud SQL. A roadmap is in place to leverage[Enterprise plus Cloud SQL](https://cloud.google.com/blog/products/databases/announcing-the-cloud-sql-enterprise-plus-edition-for-mysql-and-postgresql) for these use cases.


The widespread adoption of any new solution hinges upon its genuine necessity. There was an inherent need for a streamlined, robust, and user-friendly relational database within GCP which led to the creation of a simple yet intuitive solution for future SQL based backends at Box.


*PS: This article was refined using* ***Box AI*** *, learn more about it here*[https://www.box.com/ai](https://www.box.com/ai)


*Interested in learning more about Box? Checkout our*[careers page](https://careers.box.com/c/engineering-jobs) !
