---
schema_version: "1.0.0"
document_id: "469c5af7772a2e485d30dea6a3160bc9222cf2c7c1b841af0e19df07cdb9907d"
company_key: "yc-dittofeed"
company: "Dittofeed"
source_id: "yc-dittofeed-news-import-09ec8ea46c9e"
canonical_url: "https://www.dittofeed.com/post/release-v0-23-0"
published_at: null
first_seen_at: "2026-07-27T01:53:28.118221+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:c96343f6bd136dd200ee241fa9d6769ade2b7d39c34cf1bd6791386a4a1d4dfa"
---

# Release v0.23.0

Dittofeed **v0.23.0** is our biggest release to date. If you’ve been waiting for a feature-rich open-source customer engagement platform, this release is for you. We’ve added a new Analysis page with graph and deliveries views. We’ve also completely overhauled Broadcasts with a more intuitive UX and a ton of new features.


The list of major improvements and additions goes on, so let’s take a deeper look.


To quickly get started messaging your users, try Dittofeed Cloud by visiting our[dashboard](https://app.dittofeed.com/dashboard) .


## **Release Highlights**


### **1. New: Analysis Page**


The new Analysis page combines a graph view of messaging data with a list view of message deliveries. It includes multiple ways to filter, group, and sort:


- Set standard or custom date ranges
- Filter and group by journey, broadcast, channel, provider, message status, and template
- Show graph metrics as percentages or absolute numbers
- Group deliveries by time sent, sender, receiver, and delivery status


Once you’ve dialed in an analysis view, you have the option to download deliveries as a CSV for importing data into 3rd-party analytics tools.


Analysis page


### **2. Improved: Broadcast UX**


The flow for creating and sending broadcasts has been completely overhauled to be more user-friendly, feature-rich, and performant.


There are five steps in the flow:


1. Recipients - Choose what users will receive your broadcast. Add users by subscription group (required), then narrow down the list by creating a new segment or using an existing one (optional).
2. Content - Create a new message template or choose an existing one as the content your broadcast will send to your specified recipients.
3. Configuration - Schedule your send in localized timezones, configure rate limiting, and choose how message send errors are handled by Dittofeed.
4. Deliveries - Once you’ve triggered a broadcast, your message deliveries are shown here.
5. Events - Once you’ve triggered a broadcast, easily view events related to the broadcast, such as rendering errors, for convenient debugging.


In addition to this new flow, a helpful deliveries table has been added to the bottom of the page, which can be expanded and collapsed as needed. We’ve also added the ability to pause and resume broadcasts after they’re triggered.


Broadcast UX


### **3. New: Random Cohort Nodes**


Journeys now have a Random Cohort node, which can be added at any point in your journey mapping to evaluate message performance.


Split users between two or more cohorts, and define what percentage of users to direct to each cohort.


Random cohorts


### **4. New: Includes Segment Condition**


The segmentation builder now has a new Includes condition. This condition allows the segment to check whether an array-based trait “includes” a value.


It’s particularly useful for checking whether a user is tagged (e.g., occupation, lifecycle stage, or feature usage tags).


"Includes" condition


### **5. New: Support for Keycloak, AWS Cognito, & GCP Oauth**


When using Dittofeed’s multi-tenant auth mode, an OIDC provider is required. Previously, only auth0 was supported.


In this release, we’ve added Keycloak, AWS Cognito, and GCP Oauth as supported providers.


### **6. New: Support for Signalwire SMS**


When using Dittofeed to automate SMS, an SMS provider is required. Previously, Dittofeed only supported Twilio SMS.


We now support Signalwire SMS as an additional provider!


SignalWire SMS provider


### **7. New: Absolute Date Segment Operator**


The segmentation builder now has a new Absolute Date operator. This operator allows the segment to check if a date-based trait associated with a user is before or after a specific date.


Absolute dates


### **8. New: Endpoint for Batch Sending Transactional Messages**


We’ve added a new API endpoint for batch sending transactional messages. This endpoint improves scalability and re-uses templates from your journeys and broadcasts.[Read the docs](https://docs.dittofeed.com/api-reference/auto/content/post-apiadmincontenttemplatesbatch-send) .


Batch sending transactional messages


### **9. Improved: Subscription Management UX**


Dittofeed now has an improved layout and UX for the subscription management page, which aides with GDPR compliance.


When a user unsubscribes from any email subscription group, Dittofeed will, by default, unsubscribe them from all subscription groups in the channel. This default aligns with requirements under GDPR.


### **10. New: Workspace User Management UI**


There is now an easy way to add workspace users to your workspace from the dashboard UI.


Dittofeed has no limit on the amount of additional workspace users. Add as many as you need without your costs increasing.


Workspace user management


### **11. New: Append Users to Manual Segments**


After creating a manual segment by uploading a CSV of users, you may want to add additional users. Previously, Dittofeed only allowed replacing users completely. Now you can append new users to manual segments that already exist.


Appending users to manual segments


### **12. New: Send Broadcasts with Personal Gmail Accounts**


Many smaller businesses don’t use company domain email addresses, and need a way to message their customers from their personal gmail accounts. You can now send broadcasts from Dittofeed using @[gmail.com](http://gmail.com/) email addresses.


### **13. New: Skip Journey Message Errors**


When a journey encounters a message error while attempting to message a user, you can now choose to have the journey skip the error to keep the user in the journey, rather than exit the user from the journey.


### **14. New: Localized Timezones in Events & Deliveries Tables**


Timestamps in the events and deliveries tables now include timezones localized to the user.


Localized time in events and deliveries tables


### **15. New: Downloading Events & Deliveries**


You can now download events and deliveries as CSVs from their respective search pages.


### **16. Improved: Events & Deliveries Search**


Large optimizations have been made to our events and deliveries search functionality.


### **17. Improved: Events Filtering**


We’ve drastically improved the performance of events filtering on the events search page.


### **18. Improved: Event Entry Journey Performance**


We’ve made substantial improvements to the performance of event entry journeys, which have cleared a bottleneck when routing large numbers of large events to individual journeys.


### **19. Improved: Workspace Compute Scheduler & Queue**


We’ve restructured our workspace compute scheduler and queue to better handle large numbers of workspaces and computed properties. This is principally an improvement to our cloud and embedded components.


## Wrap Up


That’s everything! We hope you enjoy all of the powerful new features and functionality available in v0.23.0.


To try dittofeed-ee for multi-tenancy or Dittofeed embedded components, emailsupport@dittofeed.com . To try out Dittofeed Cloud, you can start sending messages now in the[dashboard](https://app.dittofeed.com/dashboard) !


‍
