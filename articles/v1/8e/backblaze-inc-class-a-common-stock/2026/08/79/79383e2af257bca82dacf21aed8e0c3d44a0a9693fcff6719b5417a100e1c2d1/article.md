---
schema_version: "1.0.0"
document_id: "79383e2af257bca82dacf21aed8e0c3d44a0a9693fcff6719b5417a100e1c2d1"
company_key: "backblaze-inc-class-a-common-stock"
company: "Backblaze Inc."
source_id: "backblaze-inc-class-a-common-stock-rss-a06767c1ff83"
canonical_url: "https://www.backblaze.com/blog/jamf-administrators-your-backup-deployment-just-got-simpler/"
published_at: "2026-08-19T13:28:18+00:00"
first_seen_at: "2026-08-19T14:39:46.357877+00:00"
fetched_at: "2026-08-19T14:39:48.452260+00:00"
content_hash: "sha256:d9a4c4f70e48cba85d7dffe6e2462b92e57f6941d21d410f9aafecf8424d3b9c"
---

# Jamf Administrators: Your Backup Deployment Just Got Simpler

If you’re running a Mac fleet, Jamf is often where everything starts. It handles provisioning, policies, app installs——the orchestration that keeps your fleet sane. But backup is the thing that doesn’t fit. Jamf gives you control over every Mac, but it doesn’t protect the data on them. Backblaze closes that gap without changing how your team works.


### Webinar: Building a Complete Mac Protection Strategy


Join Solution Engineers from Jamf and Backblaze for a practical discussion on building a complete Mac protection strategy.


## The device ownership problem


Either you know who owns every device upfront (rare), or you don’t (common). Most teams end up doing some mix: devices that came pre-assigned, devices still waiting for user mapping, devices that migrated between teams. You write a script to fix it, then another to catch the next variation. Three months later, you’re not sure if every device is actually backed up or just supposed to be.


## The new solution: Two ways to match devices to users. Pick the one that matches your reality.


This update solves the core friction: you don’t have to choose one deployment model anymore.


**Method 1: Fixed email (for controlled environments)** If you already know who owns each device at install time, for example, if you have clean HR data synced to Jamf, you can pass the user email directly during deployment. The installer uses it to set up the account automatically. No guessing, no drift.


**Method 2: Dynamic user detection (for real-world environments)** If you don’t have clean data upfront (e.g. when new devices arrive, get imaged, and wait for assignment) the installer waits until a user logs in. Once a user signs in, Backblaze can automatically associate the device with the appropriate user account based on the deployment configuration and identity information available on the device. This reduces the need for manual user assignment and helps prevent devices from being left unprotected.


Or mix them: some devices get email, others get dynamic detection. The system can now handle both in the same deployment.


## What this means for your workflow


You push the Backblaze installer through a Jamf policy, same as any other app. Set your preferred method (fixed or dynamic) once at the group level, then let it run. Devices show up in the Backblaze console under the right user, with the right backup scope, no extra steps.


When something does need adjustment—a device moved teams, a user credential changed—you handle it the same way you’d handle any other Jamf-managed app. Script it, reconfigure it, whatever your existing process is. Backup can now follow the same deployment and management workflows your team already uses for other Jamf-managed applications.


### Fewer things that can go wrong means less time managing edge cases


The friction point used to be this: you’d deploy backup, then spend the next week chasing down why a handful of devices aren’t appearing correctly. Someone’s account didn’t match. A device landed in the wrong group. Now you’re writing workarounds.


With two deployment methods that actually handle different scenarios instead of forcing everything into one model. The new deployment options reduce common onboarding issues that often require follow-up troubleshooting. ** Fewer edge cases means fewer scripts to maintain, fewer devices to manually fix, fewer things to check on at 3am.


### It still runs the same way once it’s installed


Nothing else about Backblaze changes. It backs up user data automatically, without caps or limits. Pricing stays flat per device. Restore works the same way. This update is purely about getting it deployed cleanly—the actual backup part just keeps working.


## How to start


Pick a small group of devices. Deploy through Jamf. Watch what happens for a week. You’ll see pretty quickly whether the user-matching is working and whether this fits your environment.


[How to install Backblaze silently with Jamf Pro for Mac](https://www.backblaze.com/computer-backup/docs/how-to-install-the-backblaze-client-silently-with-jamf-pro-mac)


[Learn more about Backblaze + Jamf](https://www.backblaze.com/cloud-backup/business/landing/ad/jamf)
