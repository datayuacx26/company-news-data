---
schema_version: "1.0.0"
document_id: "b242485a01c899f5f4b2ce342fa1e1cebf1cf74442266ef6c2c83ec8591b0b11"
company_key: "yc-diversion"
company: "Diversion"
source_id: "yc-diversion-news-import-29544632d9e8"
canonical_url: "https://www.diversion.dev/blog/urc-vs-diversion"
published_at: null
first_seen_at: "2026-07-21T16:38:14.982062+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:6c8239aa07d9a685f2d0be4fb0bc321c4ddeefe92d456d498fcf706a50c45348"
---

# Lore / URC (Unreal Version Control) vs Diversion

***Update: Epic has released Unreal Revision Control as Lore.*** *When this article was first drafted, Epic’s version-control was still known under the URC name.*


*Should your team use Lore, Diversion, Perforce, Git LFS, or something else for production work in Unreal Engine, Unity and large-asset pipelines?*


*This post compares Lore and Diversion across the areas that matter most: hosting model, maturity, Unreal and Unity support, file locking, large binary performance, branching, reviews, automation, migration, security, and operational overhead.*


## *Why Epic renamed URC to Lore*


*Epic’s release changes the scope of the product. URC was known primarily as the revision-control system inside UEFN. Lore is the broader open-source version control project Epic is now positioning for games, entertainment, large binary assets, distributed teams, and future workflows.*


‍


Version control is having a moment in game development. For years the conversation was simple, if frustrating: Perforce for the studios who could afford the licenses and the admin headcount, Git LFS for everyone else willing to fight the file-size limits. That binary is breaking apart, and one of the most interesting forces pulling it open is Epic Games itself.


Epic's **Unreal Revision Control (URC)** started life inside Unreal Editor for Fortnite (UEFN), and the second-generation **URC2** brought a proper command-line interface, branching, and merge tooling to creators building Fortnite islands. The expectation across much of the Unreal community now is that Epic will open URC up beyond UEFN — likely as an open-source, self-hosted server that any studio can run on its own infrastructure.


If that happens, it's genuinely good news for the ecosystem. More choice, less lock-in, and a credible free option are all worth celebrating. But "free and self-hosted" and "the right fit for your team" are not the same statement. This post walks through how URC and[Diversion](https://www.diversion.dev/) compare today, where each one shines, and what a self-hosted URC would actually ask of your studio.


A note on scope: as of this writing, Epic has not formally announced a general-purpose or open-source URC server. The comparison below treats that release as the anticipated direction and evaluates URC on what it does today in UEFN plus the self-hosted model the community expects. We'll update as Epic shares more.


## What Lore is today - and how it relates to URC


Lore is the public open-source version of the technology previously known as Unreal Revision Control. URC remains the name many UEFN users know, but Lore is the broader open-source project Epic is now releasing.


URC is a snapshot-based revision control system built directly into the Unreal Editor for Fortnite. It captures the state of a project as discrete revisions, automatically locks assets when you edit them so two people don't clobber the same binary, and surfaces branch history and conflict resolution inside the editor. URC2 added a CLI —` urc2 repository status` and friends — along with create, sync, merge, clone, commit, and push operations for local and remote branches.


It's a thoughtfully designed system, and crucially it's *native* — built by the same company that builds the engine, with no third party in the loop. For UEFN creators it's the path of least resistance, and that counts for a lot.


It also has rough edges that the community has been vocal about: editor freezes during status updates, sync and check-in operations that stall, CLI authentication that historically required an open UEFN project, and "branch does not exist" errors after reverting. Some of this is the normal turbulence of a young product, and Epic iterates quickly. But it's the current reality, and a self-hosted server release would add a brand-new dimension Epic hasn't had to support before: *you* running the infrastructure.


## What "self-hosted and open-source" actually means


This is the part that's easy to cheer for and harder to live with. A self-hosted URC server would mean your studio is responsible for provisioning, scaling, securing, backing up, and maintaining the version-control backbone of your entire production. That's the same operational model as self-managed Perforce — the model a lot of teams are actively trying to leave.


For a well-resourced studio with a dedicated DevOps function, that control is a feature: your data on your hardware, no per-seat SaaS bill, full freedom to modify the source. For a small or mid-size team, an indie studio, an education program, or a distributed group without an IT department, it's a standing cost in time, expertise, and risk. Storage growth, multi-site sync, disaster recovery, and uptime all become your problem.


Diversion's bet is the opposite one: that most teams want version control that scales like a managed cloud service and only reach for self-hosting when they specifically need it.


## How Diversion approaches the same problem


Diversion is cloud-native version control built for exactly the workloads that break Git LFS and strain Perforce — massive repositories, huge binaries, and mixed teams of engineers and artists. It's delivered as a managed service by default, but the choice of hosting stays with the studio: it can also run in your own private cloud or fully on-premises, deployed with Kubernetes or Docker. So if you *do* want infrastructure ownership, you can have it — without giving up the managed experience or rebuilding your workflow around a self-hosted-only tool.


The performance numbers Diversion publishes speak to the scale target: committing 400K files in around 30 seconds, cloning 5M files in under five minutes, and sustaining 100+ commits per minute without slowing users down. Branching and merging stay instant even on repos with millions of files, and there are no per-file size limits to design around. In production it runs on repositories spanning hundreds of terabytes with thousands of concurrent collaborators — the AAA-scale territory that's historically been Perforce's exclusive domain. URC, designed around UEFN-scale island projects, is unlikely to handle that kind of load today, and a freshly open-sourced self-hosted server would have to prove itself there before a large studio could bet a production on it.


On the Unreal side specifically, Diversion is widely adopted by the UE devs community and is also recognized by Epic as a version control solution for Unreal Engine (see[Unreal official documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/diversion-version-control-in-unreal-engine) ). Its free Unreal plugin is[among the highest-rated on Fab](https://www.fab.com/listings/59d591f2-4026-4e8f-9128-9c8ef1ff9bab) , installs in a click with no server setup, shows live branch and sync status in-editor, reloads updated assets without a restart, and warns about conflicts before you open or save a file. Horde is supported and Unreal Game Sync (UGS) support is rolling out, which matters for studios standardizing on Epic's broader toolchain.


## Feature-by-feature


Capability Lore (formerly Unreal Revision Control / URC) Diversion


Hosting model Self-hosted server (expected); UEFN-cloud today Managed cloud by default; private cloud / on-prem via Kubernetes or Docker


Scale UEFN-scale projects; unproven at AAA scale 100s of TBs, thousands of collaborators in production


Ops burden You run, scale, secure, and back up the server Fully managed; self-host only if you choose to


Engine support Unreal / UEFN Unreal, Unity, Godot, any engine or none


Desktop app & web UI Editor-integrated Native desktop app (Win/macOS) + full web UI with blame/annotate


Code & asset review Not a focus Built-in review and approval for code and art


CI/CD & webhooks Not established Webhooks, pre-commit hooks, CI/CD (Jenkins, TeamCity), REST API


AI features None Diversion Trajectory captures the reasoning behind AI-generated changes


GitHub interop None Real-time bi-directional GitHub mirroring with failover


Migration N/A Automated Perforce/Git import, side-by-side no-downtime migration


Security/compliance Your responsibility when self-hosted SOC 2, GDPR, encryption at rest/in transit, SSO, multi-region backups


Cost Free (plus your infra + ops time) Free for indie (5 users) & education (10 users); Pro $25/user/mo, Studio $35


A few of those rows deserve a closer look.


**Beyond the engine.** URC is Unreal's world. Diversion treats Unreal as a first-class citizen but also ships a Unity 6 plugin with in-editor status and clash indicators, supports Godot, and works with any engine or none. For studios with mixed pipelines — or a tools team living in regular code — one system covering everything is a real operational simplification.


**Review and automation.** Diversion includes built-in review and approval workflows that handle both code and creative assets in one place, plus webhooks, pre-commit hooks, CI/CD integrations, and a modern REST API for custom tooling. URC, by contrast, is focused on the core check-in/lock/merge loop rather than the surrounding collaboration and automation surface.


**The AI angle.** This is where the two are solving genuinely different problems. Diversion Trajectory is designed for AI-assisted development — it captures the *why* behind agent-generated code changes (starting with a Claude Code plugin), linking the reasoning to the commit it produced so teams can debug, onboard, and do "design archaeology" later. As more of the codebase gets written with agents in the loop, that history of intent becomes hard to replicate after the fact.


**Git and Perforce Sync.** Migration doesn't have to be a cliff edge. Diversion can sync bi-directionally with both Git and Perforce, so studios can move at their own pace rather than cutting over in a single risky weekend. Keep your existing Perforce depot or Git repo as the source of truth while a team or a single project runs on Diversion, let changes flow both ways, and shift more of the studio across as confidence grows. History, branches, and workflows stay intact throughout, and there's no downtime — when you're ready to fully commit, the switch is a formality rather than a leap. It's a gradual, reversible path off the tools you've outgrown, which is exactly what URC's self-hosted server doesn't offer.


## Where URC will be the right call


None of this means URC is the weaker choice for everyone. If your work lives entirely in UEFN, native tooling from Epic is the natural fit. And if you value being able to read and modify the source of your version-control server, a free open-source URC is a compelling proposition — and a healthy competitive pressure on the whole market. It's worth noting, though, that the other reasons teams reach for self-hosting — infrastructure ownership and strict data-residency requirements — aren't exclusive to URC: Diversion can run in your private cloud or on-premises via Kubernetes or Docker, so you can keep full control of your data without trading away scale, breadth, or the managed option.


The honest framing is that URC and Diversion optimize for different things. URC, in its expected form, optimizes for control and zero licensing cost at the price of operational ownership. Diversion optimizes for scale, breadth, and low operational overhead, with self-hosting available when you actually need it rather than required by default.


## The bottom line


Epic opening up URC would be a win for the ecosystem, and we'll be first in line to see what they ship. But "self-hosted server" is a meaningful commitment, not a free lunch — it trades a SaaS bill for an ops bill, and that math works very differently for a 200-person studio with a DevOps team than for an indie group of five or a university lab.


Diversion was built for the teams in the middle and at the edges of that spectrum: studios that want Perforce-grade scale and native Unreal, Unity, and multi-engine support, with code review, webhooks, CI/CD, GitHub mirroring, and AI-aware history on top — and without standing up a server to get it. Whichever direction Epic goes, that's the gap worth measuring any new option against.


‍


## FAQ: Lore, URC, and Diversion


### What is Lore version control?


Lore is Epic Games’ open-source version control system for teams working with large codebases, binary assets, and game-development projects. It was previously known as Unreal Revision Control, or URC.


### Is Lore the same as Unreal Revision Control?


Lore is the broader open-source version of the technology previously known as Unreal Revision Control. Many people still search for URC, especially because URC was the name used inside UEFN, but Epic is now branding the project as Lore.


### Is Lore available today?


Yes. Epic has released Lore publicly as an open-source project. However, Lore is still early and pre-1.0, so teams should evaluate its maturity carefully before adopting it for production pipelines.


### How is Lore different from Diversion?


Lore is an open-source version control system released by Epic. Diversion is a managed, production-ready version control platform built for large game teams, Unreal projects, binary assets, branching, reviews, automation, and enterprise workflows. The biggest difference is that Diversion focuses on giving studios a complete production platform rather than only the underlying version control technology.


### Is Lore a Perforce replacement?


Lore may become a serious Perforce alternative for some Unreal and game-development teams, especially teams that want an open-source system backed by Epic. But replacing Perforce in production usually requires more than storage and sync: teams also need migration tooling, permissions, reviews, integrations, hosting, support, CI workflows, and operational reliability.


### Should Unreal Engine teams use Lore or Diversion?


Teams that want to experiment with Epic’s open-source version control technology should look closely at Lore. Teams that want a production-ready platform with hosting, collaboration tools, reviews, support, enterprise workflows, and a lower-operational-risk path away from Perforce should evaluate Diversion.


*Curious how it feels in practice?*[Diversion is free for indie teams of up to five and for education](https://www.diversion.dev/pricing) *, the*[Unreal plugin](https://www.diversion.dev/version-control-for-unreal-engine) *installs from Fab in a click, and the*[Unity plugin](https://assetstore.unity.com/packages/tools/version-control/diversion-connect-for-unity-353134) *is on the Asset Store.*
