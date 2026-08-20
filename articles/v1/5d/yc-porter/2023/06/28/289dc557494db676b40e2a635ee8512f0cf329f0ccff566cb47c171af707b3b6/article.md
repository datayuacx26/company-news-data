---
schema_version: "1.0.0"
document_id: "289dc557494db676b40e2a635ee8512f0cf329f0ccff566cb47c171af707b3b6"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-7cfcee1b8c2a"
canonical_url: "https://www.porter.run/blog/how-to-ensure-hipaa-compliance-on-aws"
published_at: "2023-06-16T00:00:00+00:00"
first_seen_at: "2026-07-22T09:55:21.208768+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:89bb20608808a9713923bb4de4b33461e36c3883c41258f28edcfe8eab1d3de6"
---

# How to ensure HIPAA compliance on AWS

## What is HIPAA?


The Health Insurance Portability and Accountability Act ([HIPAA](https://www.hhs.gov/hipaa/for-professionals/index.html) ), established as a federal law in 1996, regulates how protected health information (PHI) is used and disclosed by covered entities and business associates during the course of care. PHI is any information in medical records that identifies individuals, including blood test results, X-Ray scans, communication records between doctors and patients regarding treatment, billing information, and so on. A HIPAA covered entity could be a healthcare provider (like doctors) or a health plan (such as health insurance providers), while Business Associates are third-party organizations that a covered entity may engage to help carry out healthcare operations through a Business Associate Agreement (BAA). Organizations that process PHI data must adhere to HIPAA rules and standards in order to be compliant.


**Identifiers of protected health information.**


Most, if not all, companies in the healthcare industry utilize cloud computing technology nowadays to improve the access and quality of their services. Therefore, when acting as Business Associates, all the ePHI data they process over the cloud must also meet HIPAA compliance requirements. Electronic Protected Health Information (ePHI) is just PHI that is produced, stored, transmitted, or received electronically.


**Doctors are healthcare providers, or covered entities under HIPAA, but companies that process ePHI data are business associates.**


## AWS and HIPAA


Amazon Web Services (AWS) is one cloud service provider that offers cloud services like Infrastructure-as-a-Service (IaaS) with Amazon Elastic Compute Cloud (Amazon EC2) compute resources like Amazon EC2 dedicated instances, cloud storage through Amazon S3 buckets, and managed databases through Relational Database Service (Amazon RDS). A full list of AWS HIPAA Eligible Services can be found[here](https://aws.amazon.com/compliance/hipaa-eligible-services-reference/) . Cloud services like these can be utilized by healthcare organizations and associates, but in order to meet HIPAA compliance requirements, they must be configured properly.


## Tips on ensuring AWS HIPAA compliance


### Establishing a Business Associate Addendum


‍


AWS is not HIPAA certified as there is no HIPAA certification for cloud service providers, however, under HIPAA regulations, cloud providers are considered business associates, and therefore, AWS offers a Business Associates Addendum (BAA) agreement, which can be accessed under the[AWS Artifact](https://aws.amazon.com/artifact/) in the AWS console. So the first step to achieving compliance is to activate a BAA agreement.


‍


#### Shared responsibility model


‍


As its name suggests, the AWS Shared Responsibility Model essentially states that Amazon and its customers have shared responsibility when it comes to security and compliance. While the cloud service provider is responsible for the security “of” the AWS cloud, customers are responsible for the security “in” the cloud.


‍


Specifically, AWS is responsible for the physical security of:


- Compute
- Storage
- Databases
- Networking
- Regions
- Availability Zones
- Edge Locations


‍


And customers are responsible for the security management process and technical safeguards of the AWS services they use, specifically regarding:


‍


- Platform
- Applications
- [Identity and Access Management (IAM)](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-set-up.html)
- Operating Systems
- Networking Traffic Protection
- Firewall Configuration
- Server and Client Side Encryption


**Amazon’s Shared Responsibility Model.**


### Responsibility “in” the cloud


‍


Not configuring AWS services properly can result in ePHI data being accessible by the wrong people. This can be avoided by following the best practices outlined below and making sure to adhere to any and all rules and standards set forth by HIPAA.


### Access control


‍


First off, AWS users looking for HIPAA compliance should turn off public access as providing ePHI data access to the public internet would be a violation of HIPAA.


‍


##### Isolated networks


‍


Utilizing an AWS virtual private cloud VPC, an isolated network for AWS resources, and connecting it to your database instance in another Amazon VPC through peering, and using a virtual private network (VPN) service is the best way to have an isolated network that makes sure no ePHI data is accessible by the public. With an Amazon VPC, there is full flexibility over the configuration of IAM roles, security groups, network access control lists (NACLs), peering arrangements, and transit gateway attachments.


‍


Under HIPAA, having control over data access and central identity management are required. Customers should also enable multi-factor authentication for their root AWS account, create an Admin role, and not create a token for their root account. By not using the root account, all users will be held to your IAM policies, increasing security overall. All roles should be granted least privileges, with more privileges granted on a case-by-case basis. Credentials, passwords, and API access keys should be constantly rotated as those that stay active for an extended period of time are at higher risk of becoming vulnerable. In addition to IAM, additional authentication should be established, through protocols like OAuth, or even Amazon Cognito.


### Audit logging and monitoring


‍


HIPAA requires that those processing ePHI data must track, log, and store all related operations. This can be done through Amazon CloudTrail, which allows for logfile scanning and recording API calls. Amazon CloudWatch can be used to monitor your AWS Resources (Elastic Compute Cloud, Relational Database Service, Elastic Load Balancing, etc.), as well. Companies just have to make sure that the logs don’t contain any ePHI data, or if its necessary to have some of this data in logs, it would need to be encrypted before being sent to CloudTrail.


### Data encryption


HIPAA requires that ePHI data is encrypted to NIST standards, both in storage and in transmission. Amazon Simple Storage Service (S3) protects your data in storage by providing additional server side encryption.


‍


#### Data encryption in Transit


While S3 provides client side encryption for data in transmission, one should use Secure Socket Layer (SSL) endpoints; SSL certificates with strong termination policies are recommended, and can be created using AWS Certificate Manager.


‍


#### Disaster recovery


Fortunately, disaster recovery procedures are made easier by the multiple regions and availability zones that AWS offers. Due to these solutions, AWS infrastructure is highly resilient in the case of disaster (furthermore, AWS has designed data centers to be fault tolerant and redundant to mitigate the effect of disasters physically affecting them), but each organization should have an established procedure for data backup and recovery.


## Benefits of Ensuring HIPAA compliance on AWS


### Increased security and data protection


The inherent benefit of being compliant with HIPAA regulations is that companies that process ePHI are following the federal law they are beholden to, and don’t open themselves up to any legal action or costly fines. As of 2022, penalties per year for any tier violation can be as high as $1,919,173.00, whether these violations were deliberate or unintentional. Following AWS HIPAA compliance requirements also make it so a company’s cloud security posture is improved in general, as well.


## HIPAA compliance with a PaaS


Engineers can configure cloud providers’ IaaS to be HIPAA compliant, but managing one's own infrastructure can take away precious engineering bandwidth, so companies may opt to use a cloud computing platform to focus solely on building their applications. The problem with achieving HIPAA compliance with many traditional PaaS offerings such as Heroku is that they utilize multi-tenant cloud hosting (Heroku does have an offering that provides dedicated environments within isolated networks, namely Private Spaces, but it’s only for certain types of instances like private dynos and private Heroku Postgres tiers, which can be prohibitively expensive) – users aren’t on their own private cloud, which can result in security and compliance issues as these cloud computing platform options abstract away the underlying cloud infrastructure from users, not allowing for full control and configuration ability.


## AWS HIPAA compliance on Porter


Say you want to become HIPAA compliant and utilize a cloud computing platform rather than managing EC2 yourself; you’d want a PaaS that sits on top of your own private cloud, like Porter, which would simply be a middleware layer on an AWS VPC. Using Porter does not make you HIPAA compliant by default, but using Porter will not introduce additional compliance overhead compared to just running your own infrastructure on AWS/GCP. This is because Porter does not store or host any of our clients' end user data - all your customer data will be stored inside your AWS/GCP account and accompanying database. Porter also offers an on-premises version where Porter's software itself runs in your own cloud account.


Porter can also ensure HIPAA compliance for all AWS infrastructure that is managed by Porter, including EKS, RDS, S3, and auxiliary services like Cloudwatch. With a click of a button, all infrastructure controls on a compliance management platform like[Oneleet](http://oneleet.com/) (YC S22) will pass instantly. There is only one infrastructure control that Porter will not be able to automate, which is the removal of PHI in application logs. By nature, this is something that the customer will have to ensure on their own.


###### If you're a part of a healthcare startup looking to use AWS and want to know how to get cloud credits, read on[here](https://www.porter.run/blog/how-to-get-aws-credits) .
