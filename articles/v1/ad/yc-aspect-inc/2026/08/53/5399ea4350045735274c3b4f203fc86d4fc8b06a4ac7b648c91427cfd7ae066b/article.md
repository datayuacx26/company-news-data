---
schema_version: "1.0.0"
document_id: "5399ea4350045735274c3b4f203fc86d4fc8b06a4ac7b648c91427cfd7ae066b"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/switching-guides/what-to-verify-before-switching-media-platforms"
published_at: "2026-08-08T00:00:00+00:00"
first_seen_at: "2026-08-09T21:00:56.600033+00:00"
fetched_at: "2026-08-09T21:00:58.529020+00:00"
content_hash: "sha256:18aa46847a705a37604afa725b581542f0c0dc8df069ab12e8eeb3d14843821b"
---

# What to Verify Before Switching Media Platforms

The safest way to switch media platforms is to treat the migration like a post workflow, not a file copy. You're moving the working memory of your team: clip names, rights notes, review history, folder logic, access rules, versions, proxies, comments, captions, transcripts, and the small bits of metadata that help a producer find the right shot six months later.


A vendor can usually show you a clean demo with a sample folder. The question is whether your real library survives the move in a form your editors, assistants, post supervisors, and production teams can actually use.


Migration area Ask the vendor Prove it in the pilot


Metadata Will custom fields, embedded metadata, rights notes, captions, transcripts, and technical metadata remain searchable and exportable? Search for known values and confirm they land in usable fields, not a generic notes dump.


Version history Will prior versions stay connected to the current asset with comments, approvals, timestamps, and owners intact? Open a real version stack and confirm reviewers can tell which notes and approvals belong to which cut.


Permissions How will internal roles, external reviewers, folder access, download rights, expired links, and view-only rules map? Log in as a producer, assistant, client, and reviewer to confirm each user sees only what they should.


Folder structure Will the full folder tree, empty template folders, original paths, long names, and duplicate names survive? Compare the migrated project against the source and confirm assistants can still find assets where they expect them.


## Start with one representative project


Don't begin with the whole library, and don't test with a tidy folder someone made for evaluation. Pick one real project that has the normal messiness of your workflow.


Use one real project with the normal workflow mess, not a sanitized sample folder. A good test project usually includes:


- Camera originals or high-res masters
- Proxies or mezzanine files
- Multiple cut versions
- Review comments or approval notes
- Custom metadata fields
- Rights or usage restrictions
- A nested[folder structure](https://partnerhelp.netflixstudios.com/hc/en-us/articles/31636764293907-Netflix-Footage-Ingest-Preparing-Your-Media-For-Upload)
- A mix of file types, including stills, audio, captions, graphics, and project files
- Internal users, external reviewers, and client access


Any platform should be able to upload files. Use the pilot to prove that the relationships between files survive:[which proxy belongs to which master](https://www.shotai.io/en/blog/editorial-conform-workflow-how-to-conform-edits-across-nle-systems) , which version replaced which edit, which comments apply to which asset, which folder rules matter, and which people should[still have access](https://www.niso.org/publications/rp-38-2021-cpm) .


Run this pilot early, while you still have negotiating room with the vendor. If the test exposes problems, you want time to change the migration plan, adjust field mapping, or decide that the platform isn't a fit.


## Metadata is where migrations break


Most media teams don't realize how much they rely on metadata until it disappears. Filename and upload date are the obvious fields, but the important fields are usually the ones your team created over time.


Ask the vendor how the new platform will import and store[each type of metadata](https://www.crossref.org/documentation/member-setup/working-with-a-service-provider/checklist-for-platform-migration/) , how your team will search it, and how your team can export it later. That includes both embedded file metadata and platform-level metadata.


The fields worth testing include:


- Clip name, reel name, scene, take, camera, shoot date, and timecode
- Custom tags, labels, campaign names, show names, and episode numbers
- Rights notes, territory restrictions, expiration dates, and talent usage limits
- Transcript text, speaker labels, captions, and subtitles
- Approval status, air status, legal status, or archive status
- Original path, source platform ID, and any[unique asset identifier](https://developers.google.com/actions/media/tools/Media_Actions_Quality_Checklists.pdf)
- Technical metadata like codec, resolution, frame rate, audio channel layout, and duration


Ask the vendor to show you exactly where the platform stores each field, how users will search it, and how your team can export it again.


Metadata should stay attached to the asset in structured, searchable form. For post teams, identifiers matter.[Reel names, timecode](https://partnerhelp.netflixstudios.com/hc/en-us/articles/4415931246995-Dailies-Best-Practices) , and source file names often connect offline edits, proxies, masters, and conform workflows. If the migration flattens, renames, or treats those values as generic tags, the platform may still look organized while becoming less useful for editorial.


If your current system has custom fields, ask whether the new platform supports the same field types. A dropdown field, free-text field, multi-select tag, date field, and rights window aren't interchangeable. Migrating everything into a notes field may technically preserve the text, but it can destroy filtering, reporting, and automation.


## Version history needs more than the latest file


A lot of platforms can migrate the current version of a file, but fewer can preserve the version stack in a way that still makes sense to a producer reviewing a spot, trailer, episode, or social cut.


Ask how the platform handles existing versions during migration. You want to know whether versions remain connected or become separate standalone files.


Test the cases your team actually uses:


- Rough cut, fine cut, locked cut, textless, final, and revised final
- Client review versions with comments attached
- Assets with the same filename uploaded at different times
- Versions that changed format, such as ProRes to H.264 review file
- Old versions that should remain viewable but not downloadable
- Approval states attached to specific versions, not just the asset as a whole


Version history often supports accountability. If a client approved V4, but the platform only shows the latest upload, your team loses context. If comments from V3 appear on V5 without clear separation, reviewers get confused. If renamed files break the version stack, assistants may spend days rebuilding history by hand.


A usable migration should preserve the relationship between versions, the timestamps, the uploader or owner where possible, and the review context around each version. If the vendor can't preserve all of that, make sure you know what will be lost and whether your team can live with it.


## Permissions rarely map perfectly


Permissions are one of the easiest areas to underestimate. On paper, both platforms may support admins, members, reviewers, folders, links, and external users, but in practice, the permission models may work very differently.


Ask the vendor to map your current permission structure into their model before migration, and don't wait until after the files arrive.


The mapping should cover:


- Internal admins, producers, editors, assistants, legal, marketing, and archive users
- External clients, agencies, vendors, freelancers, and reviewers
- Folder-level access
- File-level access
- Share links and their expiration dates
- Download permissions
- Watermarking or view-only restrictions
- Approval permissions
- Revoked users and inactive accounts


The hard part is preserving intent. If a client could only view a specific folder, they shouldn't suddenly see the whole project. If a freelance editor had upload rights but not admin rights, that distinction should survive. If legal had access to rights-restricted material but marketing didn't, the migration shouldn't flatten those roles into a single “team member” permission.


Be especially careful with inherited permissions. Some platforms apply access from a parent folder down through every subfolder. Others allow exceptions at lower levels. If your existing library depends on those exceptions, the migration can accidentally open or block sensitive material.


During the test migration, log in as a few real user types, and don't just inspect the admin view. Check what a producer sees, what an assistant sees, what a client sees, and what an external reviewer sees.


Check permissions from real user perspectives, not just the admin view.


## Folder structure is more than cosmetic


Folder structure carries operational meaning because it tells assistants where to drop new media, tells editors where to find proxies, tells producers which assets are approved, and tells archive teams how the project was delivered.


Ask whether the new platform preserves the full folder tree, including empty folders if your team uses them as templates. Also ask how it handles illegal characters, duplicate folder names, long paths, and naming collisions.


A migration can look successful while still damaging the folder logic. Common problems include:


- Flattened folder trees
- Renamed folders due to unsupported characters
- Duplicate folders merged together
- Empty template folders omitted
- Files moved into generic “Imported” folders
- Original paths lost
- Sort order changed in a way that disrupts assistant workflows


For post-production teams, the original path can be useful even after the move because it helps trace where an asset came from, rebuild broken references, and compare the migrated library against the source. If the new platform can't preserve the exact path as structure, ask whether it can preserve it as metadata.


This is also where you should test mixed media. A real project folder may include camera media, exports, thumbnails, cue sheets, PDFs, captions, audio stems, graphics, NLE project files, and delivery packages. Make sure the platform accepts the file types you actually store, not just the video files it can preview.


## Watch for proxy, master, and conform assumptions


A media platform migration isn't the same as moving an NLE timeline, but the same lesson applies: interchange formats and migration tools only carry the data their designers built them to carry. If a vendor says “we preserve everything,” ask what “everything” means in technical terms.


For editorial and finishing teams, the risky areas are usually relationships and references:


- Proxy-to-master relationships
- Source filename consistency
- Timecode and reel metadata
- Sidecar files such as captions, LUTs, XMLs, AAFs, EDLs, and CSVs
- Review files connected to high-res masters
- Watermarked screeners connected to clean exports
- Audio stems connected to picture versions


If your team ever conforms, relinks, rebuilds timelines, or references old edits, don't let the migration strip away the data that makes that possible. The platform doesn't need to be an NLE, but it shouldn't make your NLE workflows harder.


This is another reason to test with a real project, because a sample folder won't reveal whether the migrated library still supports the way your assistants prep, track, and hand off media.


## Ask how the migration will actually run


Once the pilot looks good, ask for the operational plan. A good migration plan should explain how data moves, how long it will take, who owns each task, and how your team and the vendor will validate the result.


You want practical answers to questions like:


- Will the vendor pull from the old platform, receive a drive, or require your team to upload?
- Can the migration run incrementally, or does it require a hard cutover?
- Who retries failed files, and how?
- How does the vendor or platform detect duplicates?
- What happens to files that the new platform can't preview?
- How will your team or the vendor use checksums or file counts to[confirm completeness](https://pomfort.com/article/how-to-establish-an-asc-mhl-workflow-with-silverstack-and-mediaverify/) ?
- What reports will you receive after each batch?
- Can users keep working during migration, or does the library need to freeze?


For large media libraries, time estimates need to include more than bandwidth. Upload speed, API limits, throttling, file count, small-file overhead, transcode or proxy generation, metadata import, and human review all affect the schedule.


A 200 TB library made of huge camera files behaves differently from a 20 TB library with hundreds of thousands of small assets, thumbnails, sidecars, and exports, and file count can be as painful as file size.


## Define the cutover in human terms


The technical migration may finish before the team is ready to work in the new platform. Plan the cutover around active productions, review cycles, delivery deadlines, and client approvals.


For many teams, the cleanest path is a phased move:


- Migrate and validate one completed project
- Migrate a small group of active projects
- Freeze changes in the old platform for a defined window
- Move the remaining[library in batches](https://www.mux.com/docs/guides/migrate-from-self-managed)
- Keep the old platform read-only for a short overlap period


The overlap period matters because it gives assistants and producers time to compare projects, catch missing metadata, and answer the inevitable “where did this go?” questions without blocking active work.


Don't let both systems stay editable for too long, though, because that creates split-brain problems where comments, versions, and approvals live in two places. If you need an overlap, make the rules clear: one system is the source of truth, the other is reference-only.


## Know what happens if you leave later


A platform switch is also the moment to ask about your next exit. That may feel premature, but it's much easier to negotiate and understand export terms before you sign than when you're already trying to leave.


Ask what your data looks like on export, including the structure and context around the media files.


Exit terms should produce an organized export, not loose disconnected files. The exit discussion should cover:


- Original media export
- Proxy and preview file export
- Folder structure export
- Metadata export format
- Version history export
- Comment and approval history export
- User and permission export
- Rights data export
- Captions, transcripts, and sidecar files
- Audit logs or activity history
- Costs for bulk export, storage retrieval, or support
- Timeline for account access after cancellation


If the answer is “you can download your files,” keep asking, because that may be fine for a simple storage bucket, but it isn't enough for a production media library. Your team needs to understand whether your team can use the exported data in another system or whether it becomes a pile of files plus a few CSVs that only make sense after weeks of cleanup.


Also ask whether the platform keeps any proprietary-only data. AI tags, transcripts, review annotations, approval states, and custom fields may be useful inside the system but difficult to move out. That doesn't automatically make the platform a bad choice, but you should know which parts of your workflow become platform-dependent.


## The practical decision


A media platform is ready for your library when it can prove four things with your real project: metadata shows up in usable fields, version history remains intelligible, permissions preserve intent, and folder structure comes out intact.


If any of those fail, the cost includes migration cleanup, slower editorial prep, weaker search, confused approvals, rights risk, and more manual reconstruction by the people who are already carrying too much of the workflow.


The best vendor conversations are specific. Bring one messy project. Ask them to migrate it. Open it as different users. Search for known metadata. Compare the version stack. Check the folder tree. Export the result. Then decide based on what survived.


## FAQ


Run a pilot migration with one real project that reflects your normal workflow. Use a project with versions, comments, custom metadata, rights notes, nested folders, mixed file types, and real user permissions. A clean sample folder won't reveal whether the new platform can preserve the relationships your team depends on. Aspect provides complimentary migration from any cloud or local platform and preserves metadata, folder structure, rights, and version history.


It depends on how the vendor maps it. Basic fields like filename and upload date are usually simple, but custom fields, rights data, transcripts, timecode, reel names, approval states, and technical metadata need to be tested. Ask where each field will land, whether it remains searchable, and how it can be exported later.


Version history preserves the context behind approvals, comments, revisions, and delivery decisions. If only the latest file migrates, producers and reviewers may lose the record of what changed, who approved which cut, and which comments applied to each version.


Not always. Platforms often use different permission models, even if they use similar labels such as admin, member, reviewer, or client. The important goal is to preserve intent, such as who can view, download, upload, approve, or access specific folders. Test permissions by logging in as different user types, not only as an admin.


Ask how your media, metadata, folder structure, version history, comments, permissions, rights data, captions, transcripts, and audit history can be exported. Also ask about bulk export costs, support fees, proprietary data, and how long you retain account access after cancellation.


Run a pilot on one real project that includes versions, permissions, metadata, folders, review notes, rights fields, and mixed media types. Aspect supports migration from cloud or local platforms and preserves metadata, folder structure, rights, and version history as part of its supported migration.
