---
schema_version: "1.0.0"
document_id: "b6bb05903c7fd794d0bc06c12a6d4939ee27fbdfc60188517ae19bff70af2c37"
company_key: "yc-heroic-labs"
company: "Heroic Labs"
source_id: "yc-heroic-labs-news-import-3563bf1285ab"
canonical_url: "https://heroiclabs.com/blog/announcing-heroic-cloud-2/"
published_at: "2026-01-12T00:00:00+00:00"
first_seen_at: "2026-07-21T22:49:46.144164+00:00"
fetched_at: "2026-07-28T22:23:44.568277+00:00"
content_hash: "sha256:35f5b9a0b0e52c133359ca19fe2c91294729470c70788a1f26746a0f9af32ead"
---

# Heroic Cloud 2.0: Infrastructure for Multi-Game Studios

Today we’re proud to introduce **Heroic Cloud 2.0** , a complete reimagining of our cloud dashboard built around modern developer workflows and designed to transform how publishers and multi-game studios manage game infrastructure.


## Less time wrestling, more time building


We’ve watched talented developers spend countless hours wrestling with deployment pipelines, clunky UIs, and infrastructure stress instead of solving the complex technical challenges that drive their games forward.


Our community made it clear: you wanted more granular team controls, better automation capabilities, and enterprise-grade security features. Most importantly, you needed a system designed around your actual workflows, not generic cloud abstractions.


The redesigned Heroic Cloud 2.0 interface.


## Built for how you work


Here’s how Heroic Cloud 2.0 delivers on what you’ve been asking for:


**For publishers and AA studios: Organize by game title**


If you’re managing multiple game titles, infrastructure quickly becomes a tangled web of resources. Heroic Cloud 2.0 untangles this problem by organizing all your projects, resources, and permissions around individual game titles. Instead of navigating flat lists and mentally tracking which deployments belong to which game, everything related to a title lives in one place. Combined with the redesigned interface, spinning up new projects and managing existing ones is significantly more straightforward.


Organize all your infrastructure around individual game titles.


**For AAA and enterprise companies: Security and compliance built in**


This release adds Single Sign-On (SSO) and SAML support for major identity providers (such as Okta, Azure AD, and Google Workspace). As well, Directory Sync automatically syncs your existing employee directory, so onboarding and offboarding in your identity system immediately updates access across the platform. Security and compliance are baked into Heroic Cloud from day one.


Configure SSO and SAML for enterprise identity providers.


Heroic Cloud 2.0 is built to enterprise standards regardless of your studio size. The platform is *SOC 2 Type 2 certified* , and compliance reports are available upon request.


**Teams & permissions that scale**


As your studio expands across multiple titles, permission management becomes unnecessarily complex. Who can deploy what? Who can view which logs? Who can scale prod instances during a spike? Heroic Cloud 2.0 introduces Teams to solve this: define permission groups once, then assign users to the appropriate teams based on their role. Combined with an expanded catalog of granular permissions, you can now precisely control access without micromanaging individual accounts.


Define teams with granular permissions that scale with your studio.


Permissions operate at three levels: organization-wide, per game title, and per individual resource. This means your QA team can view production deployments without being able to push updates or scale instances. When a senior contractor needs elevated access during a launch spike, you can grant individual permissions without restructuring your entire team setup. As your studio grows, your permission overhead doesn’t.


Granular permissions at organization, game title, and resource levels.


**API-driven automation**


A new API turns manual dashboard tasks into automated workflows. Create service users that interact with Heroic Cloud programmatically, enabling GitHub-triggered builds, scheduled updates, and seamless integration with your existing DevOps tools.


**Secrets management, auto-deploys, and more for Nakama**


Sensitive environment variables and API keys are managed centrally, reducing security risks and deployment complexity.


Centralized secrets management for secure deployments.


You can also configure builders to automatically deploy fresh images to specific Nakama deployments, eliminating the manual step of pushing each new build to your target environments. Alongside API automation, you can now setup full end to end automated CI/CD deployment very easily.


## A foundation for what’s next


This release sets the stage for our broader platform vision. We have several more product updates scheduled that build on these improvements in the near future.


Expect advanced observability and debugging tools, exportable metrics and logs, the ability to schedule routine operations like database exports and deployment updates, and unified login across Heroic Cloud, Nakama, and Satori.


### Rollout timeline


Heroic Cloud 2.0 deploys as a staged rollout to existing customers. If you’re currently using the platform, expect a message from us about your upgrade timeline. New users will soon have access to these features.
