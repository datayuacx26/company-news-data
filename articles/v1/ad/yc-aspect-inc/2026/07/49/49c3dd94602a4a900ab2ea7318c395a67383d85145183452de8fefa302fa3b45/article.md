---
schema_version: "1.0.0"
document_id: "49c3dd94602a4a900ab2ea7318c395a67383d85145183452de8fefa302fa3b45"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/switching-guides/how-to-migrate-off-dropbox-or-google-drive-to-a-media-platform"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T08:11:15.735962+00:00"
fetched_at: "2026-07-30T08:11:16.568967+00:00"
content_hash: "sha256:f086572ea7727eda02123ab80d72a8fc176799a0370743ba5caab09327a52bbc"
---

# How to Migrate Off Dropbox or Google Drive to a Media Platform

## Start by defining the real payload


Most teams outgrow Dropbox or Google Drive when the account becomes the production system: camera originals, Premiere projects, exports, approvals, client links, stale vendor access, half-finished rough cuts, archived campaign work, and mystery folders owned by people who left two years ago.


Copying everything from one cloud to another is how you spend money moving junk, break active links, lose context, and recreate the same mess in a more expensive place.


A good migration starts by defining what has to move as workflow, not just storage. For a media team, that usually means:


- Source media, including camera originals, audio, graphics, stills, project files, LUTs, fonts, and sidecars
- Working versions, including rough cuts, selects, transcodes, renders, and approval exports
- Finished deliverables, including masters, social cutdowns, captions, thumbnails, and delivery packages
- Review context, including comments, approvals, annotations, decision history, and version relationships
- Metadata, including folder names, naming conventions, EXIF, IPTC, shoot dates, campaign IDs, client names, usage rights, and custom tags
- Permissions, including internal groups, client access, vendor access, download rights, expiring links, and password-protected shares
- Active links, including review links, client delivery links, embedded links in project trackers, and links sitting in email threads


Those categories don't migrate equally: files and folders are the easy part, while workflow context is where most surprises happen.


The files are easy to move; the surrounding workflow context is harder to preserve. Google’s own Workspace migration materials describe tools for mass porting files and folders, filtering old or large files, and copying user and group permissions into Drive. Google Workspace Help also says its data import tool can copy files, folders, and associated permissions from Dropbox Business into a Workspace account, with a super admin handling setup. That's useful if you're moving into Google Drive.


But if you're moving from Dropbox or Google Drive into a media platform, you need to assume that some things will copy cleanly, some things will need translation, and your team will need to rebuild some things.


## Know what survives the move


The most common migration mistake is treating the source account as the system of record for everything. It usually isn't. Dropbox or Google Drive may contain the files, but the real state of a project may be spread across shared links, review tools, Slack threads, email approvals, NLE timelines, and someone’s notes.


Here is the practical way to think about each data type.


### How migration data usually translates


Data type Usually survives as-is? What to plan for in the destination


Files Mostly Validate file counts, total bytes, representative paths, modified dates, and large-file handling. Throughput can be limited by[source limits, destination limits](https://rcloneview.com/support/blog/cloud-to-cloud-migration-rcloneview) , APIs, local disk, and retry behavior.


Folder structure Mostly Preserve hierarchy where it supports NLE relinking, but do not assume folder names carry enough context. Convert important folder meanings, such as client approved or final, into metadata, collections, or project status.


File versions Sometimes Decide whether older versions should become separate files, true stacked versions, or archive-only history. Dropbox version history varies by plan, but that history may not map natively into another platform.


Review comments and approvals Rarely without a supported path Export or preserve approval records when they matter. Comments in Dropbox Replay, Google Drive, or another review system should not be assumed to become native frame-accurate comments elsewhere unless the migration path explicitly supports it.


Metadata Partially Embedded EXIF and IPTC may be re-extracted, but custom labels, folder conventions, campaign IDs, usage rights, spreadsheet trackers, and informal naming rules need mapping into destination fields.


Permissions Sometimes for internal users and groups Some supported migrations can copy associated permissions, such as Google's Dropbox Business to Drive import path. External collaborators, public links, inherited access, ownership changes, expiry dates, and download rights still need a deliberate access design.


Active links Usually no Dropbox and Google Drive URLs point to source objects. Plan a replacement window for client portals, project boards, delivery trackers, email templates, and review threads.


Files usually survive, assuming you've enough bandwidth, enough local staging capacity if you're using desktop sync, and a destination that supports the file sizes you actually have. Google caps each user at 750 GB uploaded and copied to Drive within 24 hours, and Drive can upload and synchronize files up to 5 TB. That limit is specific to Drive, but it's a good reminder that migration throughput isn't just “how fast is our internet.” It's also governed by source limits, destination limits, API limits, local disk, retry behavior, and whether the tool preserves timestamps and folder structure.


Folders usually survive, but the meaning of folders may not. “Client approved,” “Final final,” “_old,” and “Don't delete” are metadata disguised as folder names. If those distinctions matter, preserve them intentionally as metadata or collections in the destination, not only as folder paths.


File versions are mixed. Dropbox sets account recovery and version history windows by plan, including 30-day, 180-day, and 1-year history depending on tier. That doesn't mean every historical version will arrive as editable version stacks in a new media platform. In most migrations, you should decide whether prior versions need to become separate files, true stacked versions, or archive-only history.


Review comments rarely survive as native comments unless the source and destination have a supported migration path for them. Dropbox Replay has review and approval features for rich media, according to Dropbox’s own product materials, but your team shouldn't assume comments inside one review system will become frame-accurate comments inside another. Same for Google Drive comments on files. If approvals matter legally or operationally, export the record or preserve screenshots/PDFs where needed.


Metadata partially survives. The destination platform can often extract EXIF and IPTC embedded in images and media files again. A Backblaze media migration article published February 19, 2025 describes how MAM systems inspect files,[extract inherent metadata](https://www.backblaze.com/blog/workflow-playbook-migrating-your-media-assets-to-a-mam/) , note file location, and often create previews or proxies. But your team needs to map custom labels, folder-based conventions, spreadsheet trackers, and “everyone knows what this means” naming rules. They don't magically become structured fields.


Metadata needs to be deliberately attached to assets, not merely implied by folder structure. Your team needs to[rebuild permissions more often](https://transend.com/githubtowpexpress/doc-getting_started-file_migration_methodologies/) than people expect. A migration tool may copy user and group permissions in some source-destination combinations, such as Google’s Dropbox-to-Drive import path. But external collaborators, public links, expiring links, inherited folder permissions, shared drive policies, and ownership changes are easy places for access to drift. Drive[access can drift](https://yetone.pro/blog/moving-from-google-drive-to-dam/) when files move between owners or into Shared Drives with different access policies. Treat permissions as a design task, not an export setting.


[Active links generally](https://deliciousbrains.com/wp-offload-media/doc/how-to-change-storage-provider/) don't survive. A Dropbox or Google Drive URL points to a Dropbox or Google Drive object. When your team uploads the file to a new platform, it gets a new identity. If links are embedded in client portals, project boards, scripts, shot lists, invoices, or email templates, plan for a link replacement window.


The takeaway is simple: copy operations move files. Migrations move systems of work.


## Separate working media from finished work and archive


If you try to move every byte at once, active teams will either stop working or keep creating new deltas in the old platform while the migration runs. Both are painful.


Split the account into three practical states: active, reference, and archive.


Before moving data, split the account into active work, reference material, and archive. Active work is anything the team may open, edit, render from, review, approve, or deliver in the next few weeks, so your team should move this first or give it a controlled bridge plan. Active work includes more than footage. It includes project files, linked media, fonts, proxies, audio stems, graphics, review exports, and current permissions.


Reference work isn't being edited every day, but the team still searches it, pulls clips from it, reuses graphics, or answers client questions from it. This is often where a media platform starts paying off, because search and metadata matter more than raw sync.


Archive is everything you want to retain but don't need in hot production storage. That may include completed campaigns, superseded exports, unused transcodes, old cache folders, duplicated delivery packages, or client work outside your retention window.


The archive decision is where you save the most money and migration time. General file sync accounts often accumulate duplicate exports and local-sync artifacts because storage felt infinite until it didn't. Don't reward that behavior by lifting it wholesale into the next system.


Useful archive candidates usually share a few traits:


- No one has opened or modified the folder recently
- The project has been delivered and accepted
- The folder contains duplicate exports that exist elsewhere as masters
- The files are cache, render, or preview artifacts that can be regenerated
- The work belongs to a client or campaign with a defined retention policy
- The folder has unclear ownership and no active business owner
- The media is only needed for legal hold, compliance, or deep retrieval


Archive doesn't mean delete, but it does mean avoiding putting cold material into the same operational lane as active production. If the destination platform supports archive storage with preserved previews, metadata, and search, use that. If it doesn't, keep archive in lower-cost object storage or another controlled retention system and migrate only the index and retrieval process.


This is also where Dropbox or Google Drive may still be the better fit for some content. If a team mostly stores office documents, lightweight creative briefs, PDFs, spreadsheets, and occasional review exports, a general file sync platform may be fine. Google Drive is built around real-time collaboration, custom permissions such as edit/comment/view, expiry dates, Shared Drives, and integration with third-party apps. Dropbox leans on sync, sharing, password-protected links, team folders, and admin controls. Those are good capabilities for general business files. The switch becomes urgent when media workflow, not document storage, is the bottleneck.


## Build an inventory you can act on


The inventory needs to be useful enough to decide what moves, what waits, what archives, and who gets access after cutover.


Start with the top-level structure, then sample deeper because you're looking for patterns, not trying to hand-classify every file on day one.[Export file listings](https://cloudstorage.app/cloud-storage-migration-checklist) where possible, including path, owner, size, modified date, created date, extension, and sharing state. If you can't export sharing details cleanly, capture enough to identify the riskiest areas: public links, external domains, folders with many collaborators, and folders owned by departed users.


A media migration inventory should expose these groups of facts:


- Largest folders by total size
- Highest file-count folders
- Recently modified projects
- Folders shared externally
- Public or link-accessible assets
- Files owned by former employees or freelancers
- Duplicate delivery folders
- Known project roots for current jobs
- File types that need special handling, such as RAW formats, project files, fonts, captions, and sidecars
- Folders that contain generated media, cache, previews, renders, or transcodes


The goal is to turn “we've 92 TB in Drive” into something like “18 TB active, 27 TB reference, 41 TB archive, 6 TB junk or duplicate pending owner review.” That's the point where migration becomes manageable.


Don't rely only on modified date. A finished master from three years ago might be business-critical. A render cache from yesterday might be disposable. Use modified date as a signal, not a rule.


Also watch for file relationships. Video projects are full of linked assets. If you move a Premiere project without the folder structure it expects, the editor may spend the first day after cutover relinking media. If your destination preserves folder hierarchy and can present a mounted file space to editors, you can avoid a lot of that pain. If it reorganizes everything into asset records without a familiar path structure, plan for relinking and retraining.


## Decide the new permission model instead of copying the old one


Permissions are where old messes become new security problems.


Dropbox and Google Drive make it easy for people to share work in the moment. That's part of their appeal. Over time, though, media accounts collect client links, freelancer access, agency partner folders, “anyone with the link” shares, and internal exceptions that no one remembers approving.


A migration is your cleanest chance to reset that model.


Migration is a chance to replace accumulated one-off sharing with a cleaner permission model. The destination should have a simple access design that matches how production actually works. Most teams need some version of these access layers:


- Workspace-level admins who can manage users, billing, security, and global settings
- Producers and post leads who can create projects and manage project access
- Editors and artists who need edit access to working files
- Reviewers who need comment access but not file management rights
- Clients who need view, comment, or download access to selected assets
- Vendors who need time-limited access to specific folders or collections
- Archive users who can search and retrieve, but not casually modify old work


Once those roles exist, map people into groups instead of recreating one-off folder permissions everywhere. If your source has many direct user grants, treat them as evidence of how people worked, not as policy to preserve forever.


External collaborator access deserves its own pass. Don't simply invite every outside email address you find in Dropbox or Drive. Some are old clients. Some are freelancers who no longer work with you. Some are personal accounts used during a rush. Some are vendor aliases. Ask the project owner who should still have access, what they should be able to do, and when that access should expire.


For active external links, create a replacement plan. If a client is reviewing a rough cut this week, don't break the old link without sending the new one. If a delivery link is embedded in a launch tracker, update the tracker. If a link is in a public or semi-public document, coordinate the change.


## A realistic DIY migration path


A DIY migration can work if the account is small enough, the team has technical ownership, and the destination has solid import tooling. Your team will usually stage the work as copy, validate, cut over, and clean up.


The first move should be a pilot project, not the whole library. Choose a project that has enough complexity to be meaningful: camera originals, project files, exports, review notes, external collaborators, and a known owner who can validate it quickly. Avoid choosing the cleanest project in the account. That only proves easy projects are easy.


For the pilot, capture the source state in a way you can compare later. You want file count, total size, representative paths, owner, sharing status, and a few known files that editors can open. Then migrate using the method your destination supports:[cloud-to-cloud transfer](https://docs.saltbox.dev/reference/guides/cloudtocloud/) , API import, desktop sync to local staging, or assisted upload.


Each method has tradeoffs:


- Cloud-to-cloud transfer avoids pulling everything through a local workstation, but may expose fewer controls for weird file states and review context
- API migration can preserve more structure when officially supported, but it depends on source and destination compatibility
- Desktop sync is understandable and easy to supervise, but it can be slow, fragile, and constrained by local disk
- Local staging gives you a clean copy and possible checksum validation, but requires storage, time, and operational discipline


After your team moves the pilot into the destination, validate it with the people who actually use the project. A storage admin can confirm counts and sizes. Only an editor can confirm that the timeline opens without relinking chaos. Only a producer can confirm the right client sees the right rough cut. Only an ops lead can confirm the access model isn't too loose.


During validation, compare a small set of concrete signals:


- Source and destination file counts for the migrated scope
- Total bytes transferred
- Presence of expected folder paths
- Modified dates where they matter
- Ability to open project files and linked media
- Preview and proxy generation for representative media
- Playback behavior for large video files
- Preservation or recreation of version relationships
- Internal group access
- External reviewer access
- Download restrictions, passwords, and expiry where required


The point is to catch the types of failures that will repeat at scale.


Once the pilot is clean, migrate in batches by project state. Active projects first, reference libraries second, archive last. For very large libraries, avoid mixing active cutover and cold archive transfer in the same operational window. They've different urgency and different tolerance for delay.


Dropbox’s guidance for migrating server data into Dropbox recommends prioritizing active files before archived files, moving in batches, scheduling overnight or weekend moves, and asking the team to avoid changing files during transition. That advice is written for moves into Dropbox, but the operational principle applies broadly: reduce churn while copying, and don't let active work mutate under your migration.


## Switch at project boundaries whenever possible


A clean cutover says, “new work starts in the new platform, old work finishes where it already is unless there's a reason to move it.”


Project-boundary switching keeps active work moving, and it also reduces the number of files that can change in two places at once.


For current jobs, choose one of three patterns.


The first pattern is finish-in-place. If a project delivers next week and the source platform isn't actively hurting it, leave it where it's. Move the final package, metadata, and archive copy after delivery. This is boring and often correct.


The second pattern is controlled mid-project cutover. Use this when the current platform is blocking work: editors can't access large files efficiently, review is fragmented, sync conflicts are common, or external access is risky. Freeze changes for a short window, copy the whole project root, validate in the destination, then announce the new working location. Keep the old location read-only or clearly marked as retired.


The third pattern is dual-running with a narrow bridge. Use this only when you can't stop work and can't move everything at once. Define which system is authoritative for each thing. For example, editing happens in the new platform, but old client links stay alive until the next review round. Or your team uploads new footage to the new platform, while old archival selects remain in Drive until pulled. Dual-running without clear ownership creates duplicates and confusion fast.


During the switch, naming matters. Add obvious markers to retired source folders, such as “READ ONLY - migrated to media platform on July 12.” If your admin controls allow it, remove edit rights from old project folders after cutover. If people can keep editing the old copy, someone eventually will.


## Rebuild review and delivery intentionally


Media teams often discover during migration that storage was only part of the problem. They had glued review and delivery together with whatever links people could create fastest.


A media platform should let you rebuild that flow more cleanly: upload or stream working media, generate previews or proxies, collect frame-accurate comments, manage approvals, and share selected assets without exposing the whole folder tree.


If you're moving into Aspect, the platform supports generated previews and proxies, frame-accurate comments and annotations, version stacking, custom metadata, and collection sharing with permissions such as view, download, comment, and edit.


The operational benefit is that the migration can become more than a storage relocation. You can convert messy review folders into version stacks, turn spreadsheet-only tracking into custom metadata, and give clients collection-based access without breaking the underlying production folder hierarchy.


Don't over-convert everything on day one. Start with active projects and the review patterns that create the most pain. It's fine if older finished work arrives as files plus searchable metadata while new work uses the full review flow.


## Where assisted migration changes the workload


The useful part of assisted migration is that it removes a pile of coordination work from your team: transfer planning, batching, retry handling, validation strategy, permission mapping, and deciding how the new system should represent old structures.


For Aspect migrations, Aspect offers complimentary migration support for enterprise customers and can preserve metadata, rights, and version history coming from Dropbox or Google Drive. Aspect’s migration team can move up to 8 TB per team per day from Dropbox or Google Drive to Aspect.


That distinction matters. DIY timing depends on your source limits, local network, transfer method, number of files, file sizes, API behavior, and how many times you need to retry failed batches. A library with millions of small files may behave very differently from a library with fewer large camera originals. A cloud-to-cloud migration may behave differently from a local desktop sync. Don't promise stakeholders a timeline based on a vendor-assisted throughput number unless that vendor is actually running the transfer.


Assisted migration is most valuable when the source account has one or more of these conditions:


- Tens or hundreds of terabytes of media
- Many external collaborators
- Unclear ownership across old projects
- Active productions that can't pause for long
- Complex folder structures tied to NLE project files
- A need to preserve metadata, rights, or version history
- A large archive that your team needs to search but doesn't want to keep in hot storage
- Limited internal IT time for transfer monitoring and remediation


Even with assistance, your team still owns the business decisions: what is active, what is archive, who should have access, what your team can delete, and which client workflows must not break. The migration team can move and structure the system, but they can't know whether “Final_v12_USE_THIS” is actually the approved master unless someone on your side says so.


## The decision point


You're ready to move off Dropbox or Google Drive when you need a media workflow that storage alone isn't solving.


If your team mostly collaborates on documents, shares occasional files, and benefits from general productivity integrations, Dropbox or Google Drive may still be the right center of gravity. Their official materials describe sharing, admin, sync, and migration capabilities for general business content.


If your team is fighting large media access, duplicated downloads, scattered review notes, weak version context, unclear usage rights, and external collaborator sprawl, treat the migration as a workflow redesign. Define the payload, separate active work from archive, map what survives, rebuild permissions, pilot with a real project, switch at project boundaries, and use assisted migration when the scale or risk justifies it.


A good migration leaves the team with cleaner access and a platform that matches how media work actually happens.


## FAQ


Usually no. A media migration should separate active work, reference material, archive, duplicates, and disposable generated files before transfer. Moving everything can increase cost, extend the migration window, and recreate the same permission and folder problems in the new system. Active projects and searchable reference libraries usually deserve the most attention. Cold archive may belong in lower cost storage or a controlled retention system instead of hot production storage.


In most cases, no. A Dropbox or Google Drive link points to an object in that platform. When the asset is copied into a new media platform, it normally receives a new identity and a new link. Plan a replacement window for client review links, delivery links, project tracker links, embedded portal links, and links sitting in email threads. For active projects, keep old links available until the new review or delivery link has been shared and confirmed.


Sometimes, but do not assume a clean one-to-one transfer. Google Workspace Help, retrieved July 28, 2026, says Google’s data import tool can copy files, folders, and associated permissions from Dropbox Business into Google Workspace when configured by a super admin. That is useful for Dropbox to Drive migrations. For moves into a media platform, permissions often need to be redesigned, especially external collaborators, expiring links, inherited folder permissions, public links, and ownership changes. Treat the old permissions as evidence of how people worked, not necessarily as the policy to preserve.


If the team mostly works with office documents, spreadsheets, PDFs, lightweight creative briefs, and occasional file sharing, a general file sync platform may still be the better center of gravity. Google Drive’s product page, retrieved July 28, 2026, emphasizes real-time collaboration, Shared Drives, edit/comment/view permissions, expiry dates, and third-party integrations. Dropbox plan materials emphasize sync, sharing, password-protected links, team folders, and admin controls. A media platform becomes more compelling when the bottleneck is large media access, review context, version relationships, rights metadata, external collaborator sprawl, or archive search.


Do not estimate only from internet speed. Migration time depends on source limits, destination limits, API behavior, file count, average file size, local staging storage, retry behavior, checksum or validation requirements, and how much work must pause during cutover. Google Workspace Help, retrieved July 28, 2026, states that each user can upload and copy 750 GB to Drive within 24 hours and that Drive can upload and synchronize files up to 5 TB. Those are Drive-specific limits, but they show why throughput is governed by platform rules as well as bandwidth. A pilot migration with a real project is the safest way to create a usable estimate.


Treat review history as workflow context, not just file data. If prior approvals matter, export or preserve the record before migration, then rebuild active reviews in the destination so comments and versions are attached to the right media. Aspect supports frame-accurate comments, annotations, replies, notifications, version stacking, and change history as part of the team’s review workflow.
