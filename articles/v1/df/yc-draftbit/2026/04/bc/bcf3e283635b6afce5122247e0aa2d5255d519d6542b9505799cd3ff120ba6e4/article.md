---
schema_version: "1.0.0"
document_id: "bcf3e283635b6afce5122247e0aa2d5255d519d6542b9505799cd3ff120ba6e4"
company_key: "yc-draftbit"
company: "Draftbit"
source_id: "yc-draftbit-news-import-aa5bd69b40d1"
canonical_url: "https://draftbit.com/whats-new/public-share-pages-prompt-polish-publish-from-staging-more/"
published_at: "2026-04-01T07:57:20.992+00:00"
first_seen_at: "2026-07-24T05:13:30.805139+00:00"
fetched_at: "2026-07-28T21:25:41.488077+00:00"
content_hash: "sha256:dbc10913b21534a23e658864e4d3f3c2c8c035b6288d0be6ef8d911333a01030"
---

# Public Share Pages, Publish From Staging + more!

Hey everyone! It's been a productive couple of weeks and we've got a lot to share. Across eight releases (v0.2.63 through v0.2.73), we've shipped a proper way to share your apps publicly, opened up publishing from staging, and landed a bunch of reliability improvements that make everyday building smoother.


**TL;DR:** You can now create a public share page for any app — no sign-in required for viewers. iOS and Android publishing now works from the staging environment. Sandbox resources scale by billing tier, syntax errors get caught before saving, and the editor and mobile builder got a round of polish.


## Public Share Pages


You can now create a public share page for any app in Draftbit. This gives your project a clean, standalone page where anyone can view and try it — no Draftbit account required. Great for sharing a prototype with a client, showing off a side project, or sending something to a friend for feedback.


Just click the Publish button in the top right of the builder and enable the Share option to generate a public link. Your share page will be generated and a public URL will be provided which you can send to others.


## Publish iOS and Android From Staging


Previously, publishing native apps was blocked entirely in the staging environment. That meant if you were testing publishing workflows or validating a build before going live, you had to switch over to production to do it. Not ideal.


Now you can publish iOS and Android apps directly from staging. You'll see a clear warning that you're publishing from a non-production environment, but the hard block is gone. We've also improved how app versions are tracked during publishing, so when multiple publishes start close together, iOS and Android version numbers don't get mixed up.


## Sandbox Resources Scale by Plan


Your sandbox now scales with your billing tier. Higher-tier plans get more CPU, memory, and disk capacity — which means faster builds, smoother preview sessions, and more room for larger projects. If you've been bumping into resource limits on a growing app, upgrading your plan now directly improves the underlying compute your sandbox has access to.


## Syntax Checking Before Saves


Draftbit now checks file syntax before saving code and JSON files. If there's a syntax error — a missing bracket, an extra comma — you'll see it immediately before the save goes through, rather than after a confusing build failure downstream. This is a small guardrail that prevents a surprisingly common class of issues.


We also fixed a false positive where valid **.js** files containing JSX were being incorrectly flagged by the syntax checker.


## Small But Notable


#### New model options


Added **gpt-5.4-mini** and **gpt-5.4-nano** as available AI model options. If you want a lighter, faster model for certain tasks, you've got more choices now.


#### QR code in viewer mode


The live preview QR code is now available in viewer mode too, so you can launch a device preview without needing to switch to the full editor.


#### Smarter live presence


The top bar no longer shows your own avatar in the live presence indicators, and duplicate viewer avatars are cleaned up. You'll only see other people who are actively viewing the same project.


#### Billing button improvements


The billing button in the top bar now shows plan and credit details for the app's organization specifically, so you won't get confused when working across multiple orgs. Upgrade credits are also granted more reliably after confirmed Stripe billing events.


#### AI thread and MCP reliability


Agent threads now recover more gracefully from sandbox crashes and orphaned runs. Unsupported persisted events are safely skipped instead of breaking thread loading. And we fixed crashes related to deleted or failed MCP integrations being retried from the config flow.


#### Sandbox and editor reliability


Fixed sandbox startup issues where Git checkout conflicts could block first-time repo initialization. Improved idle sandbox cleanup so background health checks no longer keep truly idle sandboxes alive. Concurrency handling around locks and file operations is more stable during active editing. And slow image downloads during caching now retry safely instead of crashing the sandbox.


---


That's everything from the past couple of weeks. We've got more fun stuff coming soon. Happy building!
