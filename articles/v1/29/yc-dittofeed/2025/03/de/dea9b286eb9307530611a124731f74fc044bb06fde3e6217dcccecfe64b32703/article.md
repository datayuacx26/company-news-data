---
schema_version: "1.0.0"
document_id: "dea9b286eb9307530611a124731f74fc044bb06fde3e6217dcccecfe64b32703"
company_key: "yc-dittofeed"
company: "Dittofeed"
source_id: "yc-dittofeed-news-import-a053879274d2"
canonical_url: "https://www.dittofeed.com/post/release-v0-21-0"
published_at: "2025-03-10T00:00:00+00:00"
first_seen_at: "2026-07-25T01:44:36.904212+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:11bfc354373f406377d58a9e51186bbaee165f497b58b8def276759dd3d7b3f8"
---

# Release v0.21.0

We’re excited to announce version **v0.21.0** of Dittofeed, the open-source customer engagement platform. In this release, we’re introducing powerful new features—most notably dynamic groups—along with enhanced delivery tools and significant backend performance improvements.


If you haven’t already, we invite you to try out Dittofeed Cloud by visiting our[dashboard](https://app.dittofeed.com/dashboard) !


## Release Highlights


### 1. New: Groups


**Dynamic Group Composition** ‍


Groups now represent collections of users (who are themselves represented as users) that can have their own traits and perform actions.


**Fully Dynamic Group IDs** ‍


Unlike segments, group IDs are fully dynamic. For example, you can assign a user to a group like` "group-123"` without pre-defining any resources.


‍ **Easy Group Management via API**


Users can be assigned to groups by submitting a Group event using the Group endpoint. For example:


` POST /api/public/apps/group
{
"userId": "user-123",
"groupId": "group-456"
}
‍` ‍


Groups can also have traits assigned and can perform actions (e.g., triggering events) just like individual users:


` POST /api/public/apps/group
{
"userId": "user-123",
"groupId": "group-456",
"traits": {
"name": "Max"
}
}


POST /api/public/apps/identify
{
"userId": "group-456",
"traits": {
"name": "Max"
}
}


POST /api/public/apps/track
{
"userId": "group-456",
"event": "PURCHASE",
"traits": {
"itemId": "123"
}
}
‍
`


**Flexible Group Retrieval**


Retrieve groups for a user via the` /groups/user-groups` endpoint or list all users in a group using the` /groups/users` endpoint.


For detailed examples and additional context on groups, please refer to our[Groups documentation](https://docs.dittofeed.com/resources/groups#groups) .


### 2. New: Enhanced Deliveries Table


New Omnichannel Message Deliveries Table


‍


**Date Filtering** ‍


Filter your delivery data by date to focus on specific timeframes.
‍ **‍**


**Revamped User Experience:**


A completely redesigned interface makes it easier to navigate and analyze your delivery records.


‍


### 3. Improvements: Backend & Performance Optimizations


**Direct ClickHouse Read/Write for Assignments:**


Computed properties (user properties and segments) are now read directly from ClickHouse instead of being replicated to and read from Postgres.


This yields huge performance improvements during initial segment calculations and large broadcasts by eliminating the postgres replication step in our compute properties pipeline. These improvements impact both the total compute time, and memory footprint.


Depending on your workload, this could accelerate your compute property intervals, and broadcast deliveries by several minutes.
‍


**Optimized Multi-Tenant Compute Properties:**


This change significantly reduces the cost and latency associated with compute property intervals when running Dittofeed in multi-tenant mode.


Compute property operations in multi-tenant mode are scheduled in batches with concurrency limits using a semaphore, thereby reducing workflow task volume and limiting ClickHouse query concurrency.
‍


**Replaced Prisma with Drizzle:**


We’ve switched our ORM from Prisma to Drizzle, which improves overall performance and reduces memory footprint.


‍


### 4. Fixes and Additional Features


**Mailchimp Webhook Processing:**


Fixed an issue where Mailchimp webhook processing would fail when multiple workspaces submitted data through the same Mandrill/Mailchimp account.
‍


**Workspace Pause in Multi-Tenant Mode:**


Workspaces can now be paused in multi-tenant mode, offering you greater operational control.
‍


**Bugfixes for Performed & Group Segments:**


Various fixes have been applied to ensure that logical operators (And, OR) in performed and group segments work reliably.
‍


**Image Block in Low-Code Email Editor:**


A new image block has been added to the low-code email editor, simplifying the process of incorporating visuals into your emails.


‍


## Wrap Up


This release brings significant new features and performance enhancements that empower you to manage customer engagement more efficiently. We’re continuously evolving Dittofeed to be more robust, intuitive, and high-performing.


Experience these updates now by trying out Dittofeed Cloud at[https://app.dittofeed.com/dashboard](https://app.dittofeed.com/dashboard) .


Thank you to all our contributors and community members for your continued support!


‍
