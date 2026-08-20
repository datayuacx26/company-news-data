---
schema_version: "1.0.0"
document_id: "ecfd409cc8a68ddb97b475d68e1a358cdd6ba188c303bf3a68a4ef8af69f9e3f"
company_key: "yc-dittofeed"
company: "Dittofeed"
source_id: "yc-dittofeed-news-import-a053879274d2"
canonical_url: "https://www.dittofeed.com/post/release-v0-22-0"
published_at: "2025-03-24T00:00:00+00:00"
first_seen_at: "2026-07-25T01:44:36.904212+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:b58b976a352865ca2c35fdabf3b900662d45bb10f097e8ee16f7ba59b34f137b"
---

# Release v0.22.0

Hello everyone, **v0.22.0** is an important release for Dittofeed, our open-source customer engagement platform.


We’ve opened up dittofeed-ee, from a few select customers, to the broader public. This extension to Dittofeed includes support for multi-tenancy, with the ability to host multiple workspaces on the same Dittofeed instance.


We’ve also made major improvements to the UX of our users table along with various bug fixes and improvements.


If you haven’t already, you get started messaging your users with Dittofeed Cloud by visiting our[dashboard](https://app.dittofeed.com/dashboard) !


## Release Highlights


### 1. New: dittofeed-ee


dittofeed-ee is a closed source extension to Dittofeed that supports multi-tenancy. Multi-tenancy, via the` multi-tenant` auth mode, enables multiple workspaces to be hosted from the same instance of Dittofeed. It also supports authenticating dashboard members separately.


This feature has been in a closed beta with several of our long-time self-hosting users, and we’re releasing it publicly for the first time!


If you’re interested in trying it out, contact us atsupport@dittofeed.com and we’ll send you an access token!


See our docs for more information:


[https://docs.dittofeed.com/deployment/self-hosted/auth-modes/multi-tenant](https://docs.dittofeed.com/deployment/self-hosted/auth-modes/multi-tenant)


‍


### 2. Improved: Users Table UX


In this release we’ve made several substantial UX improvements to the users tables through the application. This includes:


##### • Improved performance


##### • Additional filtering, by subscription group


##### • Bug fixes for pagination


##### • Refresh and auto-refresh buttons


‍


Users Table UX


‍


### 3. Fix: Journey Builder


We made a fix to our journey builder which could prevent journey edits from being persisted under some configurations.


‍


### 4. Fix: Postmark Error Handling


We made improvements to our Postmark error handling, which could previously mark emails as successfully sent under some conditions. Specifically, when postmark responds with a 200 status, but presents an error code, we should mark an error, and not retry.


‍


## Wrap Up


That’s all folks! This release brings some exciting new features and improvements, with dittofeed-ee and our new users table.


If you want to try dittofeed-ee for multi-tenancy, shoot us an email atsupport@dittofeed.com . If you want to try out Dittofeed Cloud, you can start sending messages in the[dashboard](https://app.dittofeed.com/dashboard) !


‍


‍


‍
