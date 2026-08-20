---
schema_version: "1.0.0"
document_id: "657376bac283fa76d4b9ca4026ddb0065f86d91527d5fac2cc0719524b3601a0"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/how-datadogs-it-team-automated-account-inactivity-and-saas-spend-management/"
published_at: "2022-05-12T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:4fca4a58def09a1f0af12adc006cdc9163bc49383327681dcd5079b4f19cc500"
---

# How Datadog’s IT team automated account inactivity and SaaS spend management

Jason Satti


Rishi Dhar


Employees at companies of all types use dozens and even hundreds of types of commercial software to do their jobs and to develop their product. Many of these services have a monthly recurring cost per user model (Datadog does not follow a monthly recurring cost per user model; for more information about our pricing, see the[Datadog Pricing page](https://www.datadoghq.com/pricing/) ). While the plethora of tools out there is great for employees, the management and cost of all these different solutions can be burdensome on IT and Finance teams.


In our blog post[How Datadog’s IT Team Automated Monitoring Third Party Accounts](https://www.datadoghq.com/blog/engineering/how-datadogs-it-team-automated-monitoring-third-party-accounts/) , we told you about how we built a tool called Clarity to flag accounts in SaaS applications that do not match an active employee record in our HR source of truth, Workday. In this blog post, we’re going to talk about how we expanded our Clarity tool to focus on optimizing software licensing efficiency of the third party tools we are using, and how we cut costs on these third party tools. With a very quickly growing number of employees, we needed to gain insight into how our SaaS applications are being used within the company. We had multiple goals:


1.


Monitor and automatically deactivate unused accounts in services, particularly sensitive services such as cloud service providers (CSPs).


2.


Reduce the risk of stale credentials being leaked and abused in a system.


3.


Limit the impact bad actors may have during such incidents.


4.


Reduce overall SaaS spend and enable data driven procurement decisions when purchasing additional licensing.


5.


Minimize the effect on employee productivity with a frictionless workflow for employees to gain back access to any services they need.


To accomplish this goal, we expanded our tool into a service called Clarity License Manager (CLM). This service provides monitoring of account activity across multiple SaaS applications, interacts with employees through a Slackbot, and includes automated account deactivation and reactivation. This service is rolled out across several SaaS applications and has already deactivated hundreds of inactive accounts, removing the obvious liability of having unused accounts and saving Datadog money as an added bonus.


## The Problem


Prior to building CLM, there was little up-to-date insight into our SaaS license usage, and the data we did have was gathered with manual audits by our IT Support team once a quarter. This process was inefficient and tedious for our IT Support team, and a poor user experience for our employees because an IT support member would contact each user to determine if they needed to keep their account.


## The Solution


To solve the problem of licensing cost and employee efficiency, we continued to build on Clarity, turning it from an audit only tool focusing on security to a license management tool. We used a combination of direct integrations via individual SaaS APIs and indirect integrations using Google Workspace SAML Audit logs to retrieve employee activity data in a flexible and secure way.


We built a backend for Clarity using Amazon RDS (Relational Database Service) and we store employee activity data individually for each SaaS application we monitor. Having access to this information allows us to provide a user focused experience for our employees with timely notifications via email and Slack direct messages. Employees are notified when they haven’t used a specific SaaS application for a certain amount of time; this timeline is configurable for each SaaS app and the default is 90 days. They are given an opportunity to take action if they want to continue to use the app. To be considered active in an application, they need to log into or take a specific action in the app - they are told exactly what they need to do. If they do not complete the action, Clarity notifies them multiple times and provides them with resources to determine what they would like to do. If the employee does not take any action over this time, Clarity automatically deactivates their account in the SaaS application.


If the employee later on decides they need access to a specific SaaS application again, we built an automated workflow where they can open a ticket in Freshservice (our ticketing service) and their account is restored in seconds. We’ve built this process to be as unobtrusive and automated as possible, and it is designed to restore their account back to its previous role and permissions in the SaaS app.


This is an example of a Slack message we send:


## Architecture


The CLM architecture consists of several microservices, written in Python and run with AWS Lambda, that interact with a central Postgres database. We implemented a microservice architecture to allow the tool to more easily scale with our SaaS footprint and be easier to build up over time.


The AWS Lambdas that make up the service include the following:


So, a general workflow for CLM would look like this:


The transition to a microservice architecture allowed us more flexibility, better resilience, and increased scalability which are key for a tool like this. The challenges for us as we made this transition were related to the complexity and adaptability of the codebase, as each of these microservices needed access to different libraries and APIs and there was significant overlap in use case. This led us to create application specific adapters.


## Application Specific Adapter


Each SaaS application added to CLM is in the form of an adapter, a shared resource that all the individual microservice can access. The main purpose of creating these adapters was to bridge the needs of all the different microservices into a single source but it also provided additional benefits such as:


-


**Principle of Single Responsibility**


We can achieve the principle of single responsibility with our microservices because we can separate the application specific code from the primary logic of the microservice.


-


**Flexibility and Reusability of Code**


The adapters being accessible by all the microservices helped us in achieving the flexibility and reusability of the code.


-


**Less Complicated Microservices**


Our microservices are not complicated by having to connect to each individual application or handle any unique differences, they can rely on the adapters for that.


Here’s what an example SaaS application adapter file structure looks like:


```text
1   class     AtlassianAdapter  (ServiceAdapter):    2         """    3         Atlassian Adapter to implement the ServiceAdapter Interface    4         """    5
6         def __init__(self, **kwargs):    7             # Adapter setup to connect to the Atlassian API    8
9         def get_users(self, *args, **kwargs) -> dict:    10             # Return all active users in Atlassian    11
12         def get_login_data(self, profile, service_config, *args, **kwargs):    13             # Return a last login report for Atlassian users in the following format    14             # {  "john.doe@datadoghq.com"  :   [  datetime  (  2018  ,   12  ,   10  ,   14  ,   15  ,   23  ,   286000  )]}    15
16         def   deactivate_user  (self, profile, service_config, *args, **kwargs)  :    17             #   Deactivate   an already existing user account in   Atlassian    18
19         def   activate_user  (self, user_record, service_config, *args, **kwargs)  :    20             #   Activate   an already existing user account in   Atlassian    21
22         def   onboard_user  (self, profile, service_config, *args, **kwargs)  :    23             #   Create   a   new   user account in   Atlassian    24
25         def   offboard_user  (self, profile, service_config, *args, **kwargs)  :    26             #   Disable   a user account in   Atlassian
```


Every adapter we create has this structure and all the microservices we have tap into this shared resource to connect with the individual SaaS application.


## Application Specific Configuration


Each SaaS application added to CLM allows for app-specific configuration, allowing you to customize key settings based on your needs. The primary setting that CLM allows customization of is what defines activity in a SaaS application. Typically we define activity based on user login but CLM supports custom definitions of activity within a SaaS app, for example we define activity in Zoom based on the attributes required for use of a Zoom Pro license:


1.


The user hosted a meeting with 3 or more people


2.


The user hosted a meeting that lasted 40 minutes or longer


We track both of these attributes over 90 days to alert users that may not need a Zoom Pro license. CLM also allows defining attributes such as inactivity thresholds (for example, 90 days for Zoom, 120 days for Slack), maximum number of user notifications before account deactivation, whether the service is in a “notification-only” state or actively deactivating accounts, etc. CLM also allows for custom Slack messages for each SaaS application which helps us inform users and provide targeted context for specific SaaS applications.


We supplement user activity data with metadata from our HRIS system, Workday, which enables us to filter employees based on status such as new hires or employees on-leave.


This is an example of the YAML file with configuration options available for each service:


```text
1        google_workspace:    2          inactive_threshold: 90 # indicates, in days, when to determine an account has been inactive    3          new_hire_grace_period: 30 # indicates, in days, how many days an employee is considered to be a new hire    4          approval_required: True # whether a service requires manager approval to be reactivated.    5          notify_users: True # send users Slack notifications about inactive accounts    6          min_notifications: 3 # indicates how many warning notifications will be sent before automatic deactivation    7          deactivate_accounts: True # automatically deactivate accounts that have hit the inactivity threshold
```


## Application Specific Interfaces


To provide a modular development interface to CLM, as well as to allow flexibility for application specific logic, we implemented an interface for each SaaS app within CLM. These interfaces define the minimum set of required methods that each service needs to support, allowing customization within the interface method for how each service needs to implement the logic. For example, we have the following methods defined for each service:


-


` get_users`


-


` get_last_activity_data`


-


` deactivate_user`


-


` activate_user`


Using deactivation as an example, deactivating a user is not the same across all SaaS applications. A` deactivate_user` interface method allows us to define what CRUD operation needs to be called against the appropriate API Endpoint, specific to each SaaS app. Additionally, the ability to customize the` get_last_activity_data` method for each application is very robust. Some services rely on SAML login data retrieved from our Identity Provider (IdP), while others rely on service-specific activity data, such as last commit to a Github repo, or last Zoom meeting requiring a “Pro” plan versus “Basic”. By utilizing interfaces, we are able to customize CLM’s interaction with a SaaS application to perfectly match our organization’s needs.


In addition to custom logic, the interfaces provide an easy way to customize the database table for each application, allowing us to store application-specific data, such as usernames or associated roles with an account, without needing to adjust the schema for each app-specific table. The database schema for the AWS table might look something like so:


email last_activity past_threshold last_notified aws_username aws_roles


john.smith@acme.org 2022-01-01 False NULL john.smith read-only


mary.smith@acme.org 2021-03-01 True 2022-01-22 msmith account-admin


## Datadog Metrics and Dashboards


As always, we leveraged Datadog products to gain visibility and insight into our tooling. Clarity License Manager takes advantage of Datadog Metrics and Dashboards. We use metrics to track key information such as:


-


Number of services and accounts being monitored by CLM


-


Individual accounts CLM took action on


-


What actions were taken


We then use Datadog Dashboards to visualize the metrics we send from CLM to offer holistic insight into actions and trends. The dashboard also provides a high-level view into our current account recertification status, including calculations for the value of the licenses freed by CLM.


The Clarity License Manager Dashboard looks like this:


## Alternatives Considered


The primary alternative solution considered was purchasing a commercial offering . While many vendor products exist in this space, we decided to build for the following reasons:


-


Privileged access: These products typically require very privileged access in order to track activity as well as take action on accounts.


-


User privacy: These products track employee application use, lending concern regarding where the data resides and how it is protected.


-


Cost: Many products are licensed per account or employee, meaning as the company grows, so will licensing fees.


-


Lack of Customizability: Many products are utilizing an IdP’s SAML and OAuth logs in order to determine an employee’s last activity in an application. This data often does not provide the clearest picture of if an employee is actually using an application (for example, tracking Zoom activity by type of meeting rather than whether a meeting was hosted or joined in general).


## What’s Next


Now that we’ve surpassed our initial goal of expanding Clarity to also help us optimize spend and efficiency on third party apps, we want to keep honing in it’s functionality:


**Improved User Interaction** . The CLM Slackbot is currently notifying users of account inactivity. We would like to extend this to allow for user triggered deactivation if the user would prefer not to wait for the predefined notification threshold to hit and instead immediately deactivate the account. Additionally, a user needs to open a ticket in our service desk to request the account back. We would like this functionality to exist within the Slackbot natively.


**More Robust User Data** . Currently services in CLM are being monitored based on a user’s last login or last API usage, but we want to be more granular. For example, AWS is a large service for us and access level is based on Identity and Access Management (IAM) roles. Our goal is to import additional user data from AWS into CLM so we can define inactivity thresholds at a more precise level such as their IAM role as opposed to a one size fits all account level.


**Asynchronous Per-Application Runs** . Currently, each microservice within CLM runs across every SaaS application where an interface exists. As we add more SaaS applications into CLM, we will begin to hit bottlenecks with time and efficiency, and need to split the runs into asynchronous per-application runs.


The reliance on these applications and the value of being able to make data-driven decisions with them is now of critical importance, especially within IT teams. Datadog is built for cloud infrastructure monitoring with tools like metrics, alerting, and dashboards. IT teams can use Datadog to help manage and monitor tooling such as their SaaS applications and reduce their team’s workload.


Are you interested in helping us build these systems? We’re hiring!


-
-
-
