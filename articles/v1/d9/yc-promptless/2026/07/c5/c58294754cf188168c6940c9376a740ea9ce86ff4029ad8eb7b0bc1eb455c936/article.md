---
schema_version: "1.0.0"
document_id: "c58294754cf188168c6940c9376a740ea9ce86ff4029ad8eb7b0bc1eb455c936"
company_key: "yc-promptless"
company: "Promptless"
source_id: "yc-promptless-news-import-48c94b307195"
canonical_url: "https://promptless.ai/blog/product-updates/integration-health-alerts/"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-08-07T08:03:50.270603+00:00"
fetched_at: "2026-08-07T08:03:52.181710+00:00"
content_hash: "sha256:fa85f83c457da671755468de884e997b8abaf10a1096d6d516eb4c3e4ab7ff64"
---

# Promptless Now Alerts You When an Integration Has a Problem

# Promptless Now Alerts You When an Integration Has a Problem


[← Back to Blog](https://promptless.ai/blog)


Promptless now alerts you when a connected integration develops a problem. A broken connection no longer fails silently.


## The problem


Section titled “The problem”


When a Promptless integration breaks, documentation suggestions stop generating for any doc collection that depends on it. Before this change, there was no outbound signal. A teammate might notice the docs had not updated. Or you might discover the problem when you checked the dashboard after days of silence. The gap between failure and discovery could stretch to a week. Recovery work grows with every day the connection stays broken.


Integrations break for ordinary reasons, like an expired access token or a revoked Slack permission. A third-party provider can also have an outage. These problems are common and easy to fix once you notice them.


## What changed


Section titled “What changed”


Promptless now monitors the health of each connected integration. When a connection develops a problem, Promptless sends a notification to your organization’s configured escalation channels. Escalation channels can be Slack, Microsoft Teams, email, or any combination of these. The message names the specific integration. It also lists the doc collections that can no longer update.


Promptless uses two alert states.


**Needs reconnect** fires immediately when Promptless detects that an integration needs action, such as expired credentials or a revoked permission. You get one notification when Promptless first detects the problem.


**Temporary outage** fires only if the problem lasts more than 24 hours. Short outages usually resolve on their own. Promptless does not send an alert for those. If a temporary outage turns into a reconnect requirement, Promptless sends a follow-up alert.


In both cases you get one notification per problem. Promptless does not send a new notification every day the problem continues. Each message includes a reconnect link. You can act on it without opening the Integrations page first.


*We regularly share actionable insights grounded in research, experiments, and real-world product learnings. Subscribe to get future posts in your inbox.*


## Who benefits most


Section titled “Who benefits most”


This feature helps teams most when the person who configured an integration isn’t checking the dashboard. That describes most teams. A developer sets up the GitHub connection during onboarding. The auth token expires two months later. Nothing in the developer’s normal workflow reveals the problem. This alert sends that signal instead of waiting for someone to notice.


This feature also helps teams with documentation freshness requirements. A broken integration creates an undetected documentation gap. Each unresolved day adds more undocumented code changes. Catching the failure in hours instead of days limits the drift.


## Setup


Section titled “Setup”


Notifications go to your organization’s escalation channels. Configure escalation channels in Organization Settings, under Notification channels. If you haven’t configured one, this is the only required step.


Once you configure escalation channels, integration health monitoring runs automatically. There’s no per-integration opt-in.


The Integrations page also shows a health badge for each connected service at any time. The alert notifies you automatically when something breaks. The Integrations page shows the current status when you check it yourself.


## More from the blog


- [Fix skill slop before it makes your AI workforce worse](https://promptless.ai/blog/product-updates/introducing-promptless-for-agent-instructions) Product Updates


- [Grant Promptless Access to Files in Private and Shared Teams Channels](https://promptless.ai/blog/product-updates/teams-private-channel-file-access) Product Updates


- [Google Drive as a Context Source for Documentation Suggestions](https://promptless.ai/blog/product-updates/google-drive-context-source) Product Updates


[← Back to Blog](https://promptless.ai/blog)
