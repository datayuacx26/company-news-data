---
schema_version: "1.0.0"
document_id: "d56529fe5ccd194d35c7dfcc5acf0656fbd05883887639e5bb51024a88a51ef1"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/ai-assisted-blackberry-dynamics-sdk-app-migration-toolkit"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T02:00:55.928958+00:00"
fetched_at: "2026-08-07T02:00:56.798928+00:00"
content_hash: "sha256:fcc4320ddda107b8ded73748ec2bdfb376fa72cdd9c4b372600cd9a0b8140d94"
---

# Introducing the AI-Assisted SDK App Migration Toolkit for BlackBerry Dynamics

# Introducing the AI-Assisted SDK App Migration Toolkit for BlackBerry Dynamics


The new toolkit automates the migration of existing native iOS and Android apps into the BlackBerry Dynamics secure container in hours — not months.


Aug 6, 2026


·


Blog


·


Baldeep Dogra


Every enterprise we talk to has the same gap: a portfolio of mobile apps their business runs on, or needs to run on, and a security mandate those apps weren't built for resulting in a standard approach; ‘rebuild them inside a secure framework’ which then costs months of engineering per app, per platform. So, the apps stay outside the perimeter, and the risk stays on the books.


Today we're closing that gap. The new AI-assisted SDK toolkit for BlackBerry


®


Dynamics


™


automates the migration of existing native iOS


®


and Android


™


apps into the Dynamics secure container. No rebuild. No re-platforming. Your apps, yes, the ones you've already invested in, gain app-level cryptographic security in days instead of months.


## Why App-Level Security?


Most mobile security strategies protect the device: if the phone is trusted, the apps on it are trusted. That model breaks the moment a device is lost, compromised, or simply unmanaged such as in a BYOD deployment.


The BlackBerry Dynamics container works differently. Each app individually encrypts its own data, its own storage, and its own connections back to your systems. Protection travels with the app, not the device. If the device is compromised, the data inside the container stays cryptographically sealed. It's the difference between locking the building and locking every safe inside it.


That's why BlackBerry Dynamics has long been the choice of governments, defense agencies, banks, and healthcare systems. The barrier was never the security; it was the engineering effort to get an existing app inside the container. That's the barrier this toolkit removes.


## What the Toolkit Does


-


**Automated migration:** The toolkit's AI engine analyzes your existing native iOS or Android codebase and automates the SDK integration work; the mapping, rewiring, and configuration that engineers previously did by hand over weeks.


-


**Full container security:** Migrated apps run inside the BlackBerry Dynamics container with app-level encryption, secure storage, and secure connectivity; the same protections trusted by the most heavily regulated organizations in the world.


-


**No rebuild:** You keep your codebase, your build pipeline, and your roadmap. The toolkit adapts what exists rather than demanding a rewrite.


-


**Faster for partners:** Development partners and ISVs can bring their apps into the BlackBerry ecosystem dramatically faster; which means a broader catalog of secured apps for every customer.


## What This Means for Your Business


The business case is immediate. Apps that have waited in the containerization backlog for quarters can be secured this quarter, shrinking mobile risk at a pace that used to be impossible. And because migration is now measured in days rather than months, the economics change entirely; securing an app no longer competes with building new features for the same engineering budget. Compliance gets simpler too: app-level encryption gives auditors a clean, provable answer to data-at-rest and data-in-transit questions on mobile, per app and independent of device posture. Best of all, this isn't a point fix. The toolkit is part of the same platform advancing post-quantum cryptography and sovereign endpoint control across BlackBerry UEM and BlackBerry Dynamics; security that stays ahead of the threat curve, not behind it.


## Under the Hood


Developers would be keen to understand the migration workflow; this is what it looks like.


-


**Analyze:** The toolkit statically analyzes your app; entry points, storage calls, network layers, IPC surfaces, and produces a migration map of every integration point the BlackBerry Dynamics SDK needs to touch.


-


**Transform:** The AI engine applies the integration: swapping storage and networking calls for their secure BlackBerry Dynamics equivalents, wiring up authentication and container lifecycle handling, and updating build configuration for both platforms.


-


**Review:** You get a full report of every change, flagged edge cases that need human review, and a diff you can inspect before anything ships. The toolkit assists; your engineers stay in control.


-


**Validate and ship:** Build, run your test suite, and validate in the container. Apps deploy through BlackBerry UEM like any BlackBerry Dynamics app, with full policy and lifecycle management.


The output is a native BlackBerry Dynamics integration; full SDK API access, not a binary wrapper. That distinction matters: you can extend the integration, call BlackBerry Dynamics services directly (secure storage, secure communication, shared services), and stay aligned with the SDK release train, including the platform's progress toward FIPS 140-3 and post-quantum accreditation.


We're publishing the supported frameworks and current limitations openly in the developer docs; including the cases where the AI flags work for human hands. We'd rather you know exactly what to expect on app one.


Get updates about the latest in-depth knowledge for secure communications.
