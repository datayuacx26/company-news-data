---
schema_version: "1.0.0"
document_id: "4d3c4b0b7dd1b58bb7366ac117ac48f69839ada215c06f42bcca8827587191bc"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-audit-logging-for-improved-security"
published_at: "2025-06-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:e5fb9726fde2d8385ef17a55abd82d484020051d93e263ba2bc60d0414850b41"
---

# Now available: Audit logging for improved security

We're excited to announce the launch of Audit Logging in the[Depot web app](https://depot.dev/sign-up) , a critical step forward for security and operational accountability. Audit logging gives you a clear, timestamped record of key actions that lets you track important changes and identify exactly who made them. This feature empowers you to detect unexpected behavior, ensure compliance, and debug faster.


[Your builds don't have to suck. Follow our docs to turn 20-minute nightmares into 2-minute wins. Read the docs →](https://depot.dev/docs)


## What is audit logging?


Audit logging is the automatic recording of events and actions taking place in an application. It captures critical context including


- **What** action was taken
- **Who** performed the action
- **When** it occurred
- **Which** resources it affected


This is essentially a paper trail of notable activity in the application. It can provide a detailed history of actions that can help to identify notable changes to resources in your organization as well as monitor who's making those changes.


audit logging in depot web app


In Depot, we've started by capturing events for key actions in the application relating to your organization and projects. This includes, but is not limited, to the setup of new organizations or projects, changes in who has access to your organization, changes to API and access tokens, as well as configuration updates with various integrations.


## Why audit logging is useful


When multiple people are managing shared resources, it's easy to lose track of changes. Audit logs give your team a clear, time-stamped record of what happened, when, and who made the change. This visibility is essential for debugging issues, staying secure, and maintaining operational clarity.


For example, audit logs can reveal:


- A cache resets that explains sudden[cache](https://depot.dev/blog/introducing-depot-cache) misses during builds
- Deleted access tokens causing authentication failures
- Removed users losing access to the Depot web app


Instead of guessing what went wrong, you'll be able to quickly pinpoint the action, who performed it, and the exact impact. Audit logging not only makes troubleshooting faster but also provides the accountability needed for compliance, security, and smooth collaboration.


## Integration with WorkOS


Our audit logging implementation is powered by an integration with[WorkOS](https://workos.com/) to store and view these audit log events. WorkOS provides a customizable and exportable audit logging solution that enables us to send structured audit log event data to be viewed and managed in their Admin portal. Each action has a versioned schema associated with it, which allows for standardization of the included details for each action across events. For example, we can configure the actor, who is responsible for performing the action, for all events to always include the additional metadata of email and role so that there is an additional level of clarity about who is performing these actions.


WorkOS Schema for the` project.created` action with actor metadata:


```text
{
action  :   'project.created'  ,
actor  : {  metadata  : {  role  :   'string'  ,   email  :   'string'  }},
targets  : [{type:   'project'  }],
}
```


audit logging actor details


This helps ensure that we're providing data in a consistent format for events to make it easy to search, filter, and analyze your organization's audit logs.


These audit logs are viewable via an admin portal in WorkOS for a default retention of 30 days. The admin portal is available from the Organization Settings page in the Depot web app for those with the feature enabled.


how to access audit logging in depot web app


Additionally, WorkOS supports LogStreams, which allows you to forward audit logs to your own Security Incident and Event Management (SIEM) system or any other log storage provider you already use, enabling seamless integration into your existing observability or compliance workflows.


## Audit log availability


This feature will be available for customers on our business plan. If you're interested in utilizing this feature, reach out and we can get it set up for you.


[This feature is currently available for customers on our business plan. If you're interested, reach out. We can set you up. Talk to a human →](https://cal.com/team/depot/engineer-talk)


## Take control of your security: Try audit logging today


If you're managing projects and access in Depot, audit logging is your new best friend for staying on top of changes. This feature is available for customers on the Business Plan. Reach out to enable it and start gaining visibility into your organization's actions.


## Related posts


- [Making EC2 boot time 8x faster](https://depot.dev/blog/faster-ec2-boot-time)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Now available: Gocache v2 for improved Golang build performance](https://depot.dev/blog/now-available-gocache-v2-faster-improved-golang-build-performance)
- [Now available: Depot ephemeral registries](https://depot.dev/blog/depot-ephemeral-registry)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


Iris Scholten


Staff Engineer at Depot
