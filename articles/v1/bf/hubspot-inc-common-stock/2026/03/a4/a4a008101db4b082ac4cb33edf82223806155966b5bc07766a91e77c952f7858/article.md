---
schema_version: "1.0.0"
document_id: "a4a008101db4b082ac4cb33edf82223806155966b5bc07766a91e77c952f7858"
company_key: "hubspot-inc-common-stock"
company: "HubSpot Inc."
source_id: "hubspot-inc-common-stock-rss-7ea402701b4d"
canonical_url: "https://product.hubspot.com/blog/hubspot-incident-report-february-25-2026"
published_at: "2026-03-20T17:48:30+00:00"
first_seen_at: "2026-07-20T03:32:21.515438+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:e8ef09d286fd0ebb0a1735cd82db5d0c9c929293aad55c2c9c70ff86f83fd85c"
---

# HubSpot Incident Report: February 25, 2026

On February 25, 2026, HubSpot experienced a service disruption affecting user access to specific types of workflows within our user interface. All backend workflow automations continued to execute normally without interruption - no automations were delayed, missed, or lost. We have completed a thorough analysis of this incident and are implementing targeted safeguards to prevent this class of failure from recurring.


### What Happened


Our engineering team deployed a configuration update to our permissions framework, intended to expand access to contact, company, order, and project-based workflows for certain account tiers. This required creating new, object-type-specific permission scopes to replace a broader shared scope.


The new permission scopes were created and validated successfully in our staging environment. However, the corresponding user role assignments for these scopes were not promoted to production before the configuration went live. Without those role assignments, our access-control system could not confirm that users held the required permissions and defaulted to a restrictive access level.


### Technical Impact Details


Because the new scopes were active in production without corresponding role assignments, the access-control system returned a restrictive permission level instead of the expected user-level access.


- **Incorrect Permission Response** : The access endpoint continued returning successful HTTP 200 responses, but with the wrong permission level. This meant standard error-rate monitoring did not flag the issue, and the deployment's automated health checks passed normally.


- **UI Lockout** : The frontend interpreted the restrictive permission level as the user lacking access, and stopped rendering workflow links for the affected object types. Users saw errors when attempting to open contact, company, order, or project workflows.


### Customer Impact


Between approximately 3:43 PM EST and 4:20 PM EST, customers experienced the following disruptions:


- **Loss of UI Access** : Users were unable to click into, view, edit, or manage contact, company, order, and project-based workflows.


- **Unaffected Services** : Deal-based, ticket-based, and data-driven workflows remained fully accessible throughout the incident.


- **No Backend Impact** : Workflow execution was entirely unaffected. All automations continued to run and process data as expected. No customer data was lost during this incident.


### Timeline of Events


- **3:10 PM EST** : **** Deployment of the permission configuration update begins its initial rollout.


- **3:43 PM EST** : Deployment completes full rollout to production. UI access to contact, company, order, and project workflows begins failing across all customers.


- **3:59 PM EST** : Internal incident is officially declared after a surge in customer support reports.


- **4:16 PM EST** : Engineering teams initiate a full rollback of the configuration deployment.


- **4:20 PM EST** : Rollback takes effect. UI access to all workflow types restored.


### Our Response During the Incident


Upon identifying the scope of the UI lockout, our engineers traced the issue to the recent permissions configuration deployment. The team initiated a full automated rollback, reverting the system to its previous configuration state and restoring UI access for all affected workflow types.


### What We're Doing to Improve


Pre-deployment scope validation


: We are adding automated checks to our deployment pipeline that will block any permission-scope configuration change from going live until the corresponding user role assignments are confirmed active in production. This directly addresses the root cause - the configuration should not have been deployable in its incomplete state.


Accelerated automated test alerting


: Our automated test suite detected failures during the deployment's canary window, but the alerting threshold was configured to wait 60 minutes before paging engineers. We are reducing this threshold to trigger within minutes of consecutive failures. Additionally, we are correlating test failures with active deployments so that failures during a rollout window are automatically escalated.


Real-time permission correctness monitoring


: The access endpoint returned successful HTTP 200 responses with incorrect data, which made this invisible to standard error-rate monitoring. We are building new metrics that track the ratio of user-level access grants versus restrictive responses, broken down by workflow object type. If any object type shows a sudden shift toward restricted access, our on-call engineers will be paged within minutes.


### Our Commitment to Reliability


Workflows are central to how our customers run their businesses. Losing the ability to view and manage them - even briefly, and even while execution continues uninterrupted - is disruptive. We are applying the core lesson from this incident - our monitoring must verify correctness of responses, not just availability, across our critical customer-facing systems.


Thank you for your patience and continued trust in HubSpot.
