---
schema_version: "1.0.0"
document_id: "e4ce06edd7be6e2c770a3ab1d426de8e2d0c68932f8307a0d0682edb39c15327"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-69a1f8dc01ea"
canonical_url: "https://www.sourcebot.dev/changelog/v3"
published_at: "2025-03-31T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:52.577262+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:db9096ac1bad2db08a9dd6e2a81dc27ac24bfdde3cb85f111723a50fca44878e"
---

# Sourcebot v3 release

Sourcebot v3 introduces several major improvements to streamline onboarding, enable team rollouts, and enhance overall usability:


-


Parallelized repo indexing


-


Multi tenancy mode


-


Authentication


-


GUI connection management


-


Nav bar indicators


To learn how to upgrade from v2 to v3, checkout our[migration guide](https://docs.sourcebot.dev/self-hosting/upgrade/v2-to-v3-guide) .


### Parallelized repo indexing


We've improved Sourcebot's repo indexing capabilities to handle multiple repo index jobs in parallel, speeding up indexing significantly. We're using[BullMQ](https://github.com/taskforcesh/bullmq) to manage indexing jobs, with a Redis instance automatically spun up within the official docker image. Sourcebot can connect to an external Redis instance by changing the` REDIS_URL` environment variable.


### Multi tenancy mode


Sourcebot now supports multi-tenancy, configurable via the` SOURCEBOT_TENANCY_MODE` environment variable.


Multi tenancy mode allows your Sourcebot deployment to have multiple organizations, each with their own members and code host connections.


### Authentication


We've added built-in authentication mechanisms to gate your Sourcebot deployment, configurable via the` SOURCEBOT_AUTH_ENABLED` environment variable. This is supported in both multi and single tenancy modes. Admins can invite members and grant them access to their Sourcebot organization.


### GUI Connection Management


We've added the ability to manage your code host connections within the webapp. When authentication is enabled, this becomes the primary way to manage code host connections. If auth is disabled, the existing[declarative configuration file](https://docs.sourcebot.dev/self-hosting/features/declarative-config) mechanism is used.


### Nav bar indicators


We've added the ability to see the status of your repo indexing jobs at a glance from the nav bar. This allows you to see which repos are in progress, as well as any warnings or errors that my have occurred (when authentication is enabled).


If you have any feedback on the launch or want to suggest additional changes, please check out our[discord](https://discord.com/invite/6Fhp27x7Pb) or[github discussions](https://github.com/sourcebot-dev/sourcebot/discussions) page.
