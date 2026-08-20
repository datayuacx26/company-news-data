---
schema_version: "1.0.0"
document_id: "bd96ba3551675b466079e5ec15c392ac3c69922061e794aff1e95c7d3174e68f"
company_key: "hubspot-inc-common-stock"
company: "HubSpot Inc."
source_id: "hubspot-inc-common-stock-rss-7ea402701b4d"
canonical_url: "https://product.hubspot.com/blog/incident-report-for-october-20-2025"
published_at: "2025-11-11T17:09:36+00:00"
first_seen_at: "2026-07-20T03:32:21.515438+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:974b71823fb6a38a3dfd95871c90ede582ef08e1b839b9dab8e83811e9829652"
---

# HubSpot Incident Report for October 20, 2025

On October 20, 2025, HubSpot experienced a significant service disruption affecting multiple product features due to a severe AWS outage in the us-east-1 region. While our infrastructure remained intact, the widespread nature of the cloud provider failure impacted both our services and critical third-party vendors we rely on. We've completed a thorough analysis of this incident and are implementing comprehensive improvements to strengthen our resilience against future cloud provider disruptions.


### **What Happened**


At 2:48 AM ET on October 20, AWS experienced one of its most severe service[disruptions](https://aws.amazon.com/message/101925/) in recent history, affecting numerous services in the us-east-1 region where HubSpot's primary infrastructure operates. The outage began with DynamoDB (a database service) failures that cascaded to affect IAM (Identity and Access Management), SQS (Simple Queue Service), and EC2 (compute instances).


### **Technical Impact Details**


**1. Infrastructure Layer Failures**


-


- Compute Instance Creation: New compute instances couldn't be provisioned, preventing our infrastructure from automatically scaling to handle load
- Credential Rotation: System credentials that refresh periodically began failing, causing authentication errors across our infrastructure
- Network and Storage Attachments: Network interfaces and storage volumes failed to attach to instances, preventing applications from starting properly
- IP Address Exhaustion: During recovery, our infrastructure exhausted available private IP addresses in the primary ranges. We deployed pre-staged backup IP ranges, though AWS's infrastructure took longer than expected to propagate these changes, temporarily affecting application connectivity


**2. Task Queue System (TQ2) Cascade Failure** **** TQ2 (Task Queue 2) is HubSpot's core distributed task processing system that handles asynchronous background work across our platform. Processing millions of tasks daily, TQ2 manages everything from scheduled emails and workflow executions to report generation, data imports/exports, and list processing.


When services need work performed in the background or at a future time, they enqueue tasks to TQ2, which reliably processes them using Amazon SQS as its underlying message queue. This architecture keeps HubSpot's user-facing features responsive while handling time-intensive operations behind the scenes.


During the AWS outage, TQ2 experienced significant processing degradation that extended well beyond the initial cloud provider disruption:


-


- When AWS SQS became unavailable, TQ2's failover mechanism automatically redirected tasks to Kafka (a backup message queue system), successfully capturing incoming tasks.
- However, the failover consumer responsible for replaying these tasks back to SQS after recovery encountered a critical bug in message size validation that prevented certain tasks from being reprocessed.
- Processing constraints in the failover consumer meant that clearing the accumulated backlog took significantly longer than the initial AWS outage, extending the recovery window.
- This meant that background job processing remained degraded for hours after AWS services were restored.


**3. Vendor Service Disruptions**


-


- Our calling provider's control plane experienced significant degradation as it shares the same AWS region.
- Our analytics database provider experienced similar regional failures, making data warehousing operations impossible.
- Cloud storage and CDN services were impaired, affecting file uploads and downloads.


### **Customer Impact**


During this incident, customers experienced degraded performance across multiple product areas.


- Calling functionality was impacted with elevated failure rates for both outbound and inbound calls.
- Reporting and analytics services including Custom Report Builder, journey analytics, Data Studio, and list evaluations experienced significant degradation with many customers encountering error messages.
- Email sending was severely degraded during multiple periods, with scheduled emails eventually delivered once service was restored.
- Quote operations experienced failures and elevated error rates.
- Data operations were also affected, with export jobs failing to complete and import processing experiencing significant degradation due to file download issues.


### **Timeline of Events**


- **2:48 AM ET** : AWS DynamoDB disruption begins, triggering cascade failures in IAM and SQS
- **3:00 AM ET** : TQ2 task processing experiences severe degradation; Kafka failover activates
- **8:35 AM ET** : Our engineers disabled automated scaling systems to maintain stability during the AWS outage
- **9:07 AM ET** : Manual interventions performed to allow applications to continue running without requiring AWS API calls
- **12:43 PM ET** : IP address exhaustion detected; emergency backup range deployment initiated
- **3:50 PM ET** : AWS announces service restoration
- **9:11 PM ET** : TQ2 backlog cleared


### **Our Response During the Incident**


While AWS services were impaired, our engineering teams took several defensive actions to protect service stability and minimize customer impact:


**Infrastructure Stabilization** Our engineers immediately disabled automated scaling and deployment systems to prevent the AWS API failures from causing additional service disruption. This manual intervention maintained the stability of our existing infrastructure while AWS services were degraded.


**Workload Management** We performed manual interventions to allow critical applications to continue running without requiring calls to failing AWS APIs, enabling some services to maintain partial functionality during the outage.


**Task Queue Failover** Our TQ2 system's automatic failover mechanism successfully redirected millions of background tasks to our Kafka backup system, preserving task data during the SQS outage and enabling recovery once AWS services


were restored.


### **What We're Doing to Improve**


**Enhanced Incident Response Procedures** We're documenting and automating our defensive procedures that were executed manually during this incident. This includes automated deployment freezing, infrastructure scaling controls, and recovery procedures. These runbooks will ensure any engineer can execute critical defensive measures within minutes rather than requiring specific expertise.


**Vendor Diversification** We're exploring vendor diversification strategies across our stack to reduce dependency on any single provider. This includes evaluating alternative providers and building abstraction layers that enable automatic failover when vendors experience regional failures.


**Advanced Monitoring and Early Warning Systems** We're improving monitoring around critical cloud provider APIs and dependencies to detect failures before they impact production workloads and identify degradation patterns early.


**Improved Task Queue Recovery** The TQ2 failover consumer is being converted from synchronous to asynchronous processing to significantly increase throughput. We're also migrating the failover topic to a dedicated infrastructure with substantially increased capacity to enable massive parallel processing during recovery scenarios.


**Expanded Chaos Engineering** We're expanding our existing chaos engineering program to include more comprehensive AWS service failure scenarios. These exercises will validate our runbooks, train our teams, and identify weaknesses before they impact customers.


**Customer Communication Improvements** We're improving error messages to be more timely and informative when service issues occur. Additionally, we're implementing automatic in-app banners that activate when critical dependencies fail, ensuring customers have clear visibility into major service disruptions.


### **Our Commitment to Reliability**


While cloud provider outages of this magnitude are rare, we recognize that our customers depend on HubSpot for mission-critical business operations. This incident has catalyzed a comprehensive reliability initiative spanning infrastructure, architecture, and operational improvements.


We're investing in building anti-fragile systems - systems that not only withstand failures, but also improve from stress testing. Through vendor diversification, architectural evolution, and rigorous failure testing, we're working to ensure that future cloud provider incidents have minimal impact on your business operations.


Thank you for your patience and continued trust in HubSpot. We're committed to maintaining that trust through continuous improvement of our platform's reliability and resilience.
