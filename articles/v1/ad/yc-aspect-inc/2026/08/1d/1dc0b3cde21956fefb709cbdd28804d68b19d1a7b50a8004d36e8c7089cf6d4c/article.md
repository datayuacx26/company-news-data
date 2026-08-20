---
schema_version: "1.0.0"
document_id: "1dc0b3cde21956fefb709cbdd28804d68b19d1a7b50a8004d36e8c7089cf6d4c"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/guide-to-folder-structures-for-multi-season-tv-shows"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-17T20:24:08.329587+00:00"
fetched_at: "2026-08-17T20:24:09.115432+00:00"
content_hash: "sha256:c1455185dcf122975b588ca8ae01b638a32396bcad2b73fa457646197bd24d04"
---

# Guide to Folder Structures for Multi-Season TV Shows

Build the tree around the series first, then season, then episode, then asset type. That's the decision that keeps a show from collapsing under its own weight in season three.


A single-episode folder structure can survive on vibes, but a multi-season show can't. You need something that works when episode 204 is still in offline, episode 206 is getting pickups, season one is being remastered for a platform delivery, and a producer asks for “that drone shot from last season” without knowing the shoot date, camera roll, or filename.


The structure should make the obvious path the correct path. If someone receives a drive, exports a temp mix, drops in VFX plates, or archives final masters, they should know where it goes without asking three people in Slack.


## The base hierarchy that scales


For most scripted, unscripted, docuseries, and serialized post workflows, start with this shape:


```text
SHOW_NAME/
00_SHOW_ADMIN/
01_SEASON_01/
00_SEASON_ADMIN/
01_EPISODES/
S01E01_EPISODE_TITLE/
S01E02_EPISODE_TITLE/
02_SEASON_ASSETS/
03_DELIVERIES/
04_ARCHIVE/
02_SEASON_02/
00_SEASON_ADMIN/
01_EPISODES/
02_SEASON_ASSETS/
03_DELIVERIES/
04_ARCHIVE/
90_SHARED_SERIES_ASSETS/
99_DOCUMENTATION/


```


Keep season and episode explicit, and keep shared material out of whichever episode happened to use it first.


Shared assets sit at the series level instead of being buried inside one episode. Inside each episode, use repeatable department or workflow folders:


```text
S01E01_EPISODE_TITLE/
00_EPISODE_ADMIN/
01_CAMERA_ORIGINALS/
02_AUDIO_ORIGINALS/
03_TRANSCODES_PROXIES/
04_PROJECTS/
05_EDITORIAL_EXPORTS/
06_VFX/
07_COLOR/
08_SOUND/
09_GRAPHICS/
10_DELIVERY/
11_ARCHIVE/


```


This gives every episode the same internal map. Assistant editors can[duplicate the episode template](https://www.boomboxpost.com/blog/2018/3/12/a-killer-file-organization-system-that-saves-you-time-money-and-your-sanity) , and post supervisors can audit progress across episodes without relearning the layout. Online, mix, VFX, and delivery teams can find their lane without stepping into camera originals.


A good folder structure is simple, predictable, and boring. Boring is the point.


## Keep the series layer clean


The top-level show folder has three jobs: identify the show, separate seasons, and hold information that applies across the whole series.


Use the show name consistently. If the show has a rebooted title, alternate title, or similar title in a media library, include the year in the show folder name:


```text
SHOW_NAME_2026/


```


Media library tools commonly recommend a[series folder, then season](https://support.plex.tv/articles/naming-and-organizing-your-tv-show-files/) folder, then episode file naming, and they also rely on show name and year to avoid bad matches. Even if you aren't building a Plex, Emby, Jellyfin, or Kodi library, the same logic helps humans and automation. A show folder called` THE_OFFICE` is ambiguous, but` THE_OFFICE_US_2005` isn't.


The series level is also where shared assets belong. These are assets that aren't owned by one episode:


```text
90_SHARED_SERIES_ASSETS/
BRANDING/
FONTS/
LUTS/
MUSIC_PACKAGE/
OPENING_TITLES/
LOWER_THIRDS/
STOCK_RECURRING/
LEGAL_CLEARANCES/


```


If season two uses the same title sequence as season one, don't store it inside` S01E01` . If every editor needs the same music package, don't copy it into eight episode folders with eight different modification dates. Keep one canonical location, then reference or duplicate only when the workflow requires local copies.


## Season folders are production boundaries


A season is more than a batch of episodes, and it usually has its own schedule, vendors, delivery specs, network notes, music cue sheets, archival obligations, and sometimes a separate storage allocation. Treat it as a real boundary.


A season folder should contain the episodes plus the season-level materials needed to manage them:


```text
01_SEASON_01/
00_SEASON_ADMIN/
SCHEDULES/
DELIVERY_SPECS/
CONTACTS/
VENDOR_NOTES/
POST_CALENDAR/
01_EPISODES/
02_SEASON_ASSETS/
SEASON_SPECIFIC_GRAPHICS/
SEASON_SPECIFIC_MUSIC/
CAST_STILLS/
MARKETING_PULLS/
03_DELIVERIES/
NETWORK/
STREAMER/
INTERNATIONAL/
PROMO/
04_ARCHIVE/


```


The takeaway is that season-wide material should sit beside the episode folders. If a delivery spec applies to the whole season, store it once at the season level. If a lower third template changes in season two, store that in the season’s assets so nobody accidentally uses the season one version.


## Episode folders need fixed internal lanes


The episode folder is where the daily work happens, so it needs the most discipline. Every folder should describe a workflow state or department, not a person.


Good folder names tell you what the asset is or where it's in the pipeline:


```text
01_CAMERA_ORIGINALS/
02_AUDIO_ORIGINALS/
03_TRANSCODES_PROXIES/
04_PROJECTS/
05_EDITORIAL_EXPORTS/
06_VFX/
07_COLOR/
08_SOUND/
09_GRAPHICS/
10_DELIVERY/
11_ARCHIVE/


```


Weak folder names create long-term confusion:


```text
NEW/
OLD/
FROM_JASON/
EXPORTS_FINAL/
EXPORTS_FINAL_NEW/
MISC/
STUFF/


```


The first set can survive staff turnover, but the second set only works while everyone remembers the conversation that created it.


For camera media, preserve the original card structure. Assistant editor guidance from real post workflows consistently emphasizes copying the[entire camera card structure](https://www.avid.com/resource-center/assistant-video-editors-prep-media) to storage, not cherry-picking clips. Many camera formats depend on sidecar files, metadata folders, spanned clips, audio references, or folder relationships. If you flatten the card because the` .mov` files are the only thing that looks useful, you can create relink problems later.


A camera originals area might look like this:


```text
01_CAMERA_ORIGINALS/
2026-03-14/
A_CAM/
A001_0314AB/
CARD_STRUCTURE_HERE/
B_CAM/
B001_0314CD/
CARD_STRUCTURE_HERE/
2026-03-15/
A_CAM/
A002_0315AB/


```


Use shoot date, camera, and card or roll ID, and don't rename original camera files unless the workflow has a specific, tested reason to do so. Rename folders around the media, and keep the source media intact.


Keep original camera card relationships intact rather than flattening source media.


## Naming episodes without painting yourself into a corner


TV teams often use both production-style episode codes and library-style season/episode codes. The safest convention is to support both, but make one canonical.


A common internal TV code is` 101` , meaning season 1 episode 1. That works until you hit season 10, when` 1001` may not sort with` 101` unless everyone pads and parses correctly. The more explicit pattern is:


```text
S01E01_EPISODE_TITLE
S01E02_EPISODE_TITLE
S02E01_EPISODE_TITLE
S10E01_EPISODE_TITLE


```


Use two digits for season and two digits for episode unless your show can exceed 99 episodes in a season. If it can, use three episode digits from the start:


```text
S01E001_EPISODE_TITLE


```


Don't switch padding halfway through a series. Sorting matters. Automation matters. Human scanning matters.


For final media, screeners, and archival files, put the season and episode in the filename too, not only the folder path:


```text
SHOW_S01E01_EPISODE_TITLE_TEXTLESS_MASTER_v001.mov
SHOW_S01E01_EPISODE_TITLE_NETWORK_SCREENER_v003.mp4
SHOW_S01E01_EPISODE_TITLE_DIALOGUE_STEMS_v002.wav


```


That protects the file when it leaves the folder. Episodic delivery and archival specifications often require[season and episode identifiers](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360000384727-Picture-Archival-Assets-Folder-Structure-and-File-Naming-Convention) in the folder and file name for exactly this reason. A file called` FINAL_MASTER.mov` becomes meaningless the second it shows up in a transfer folder with seven other “final” files.


A file stays identifiable when its naming details travel with it.


## Clip, card, roll, and reel naming


For editorial and online, reel naming isn't cosmetic. It affects[conform, relink, turnover](https://masv.io/workflow/post-production) , and troubleshooting. The rule is simple: every source clip should trace back to one original card or roll, and that card or roll should have a unique ID across the whole show.


A solid card ID can include camera, roll number, and shoot date:


```text
A001_0314AB
B014_0422CD
C003_0501EF


```


Some productions prefer episode-aware roll names:


```text
S01E03_A001_0314
S01E03_B001_0314


```


That can help when episodes shoot separately, but it can be risky when multiple episodes shoot on the same day or block shooting mixes scenes from several episodes. If production shoots by block, date and card ID are more reliable than episode alone.


Use these fields consistently across folders, ALEs, bins, shot logs, and turnover documents:


```text
Show code: SHOW
Season/episode: S01E03
Shoot date: 2026-03-14
Camera: A_CAM
Card/roll: A001_0314AB
Scene/take: from script supervisor or sound report
Source filename: preserved from camera
Transcode name: derived, not invented


```


The key is to avoid duplicate reel names. Duplicate` A001` rolls across seasons are a conform trap. If season two starts over at` A001` , make sure the full reel identifier includes date, season, production block, or another unique component.


## Transcodes and proxies should mirror the source logic


Proxies should be easy to regenerate, which means their location and names should point back to the source media, not just the edit.


A proxy folder can mirror the camera originals structure:


```text
03_TRANSCODES_PROXIES/
2026-03-14/
A_CAM/
A001_0314AB/
SHOW_S01E03_A001_0314AB_C001_proxy.mov


```


If your NLE generates proxies internally, still document where they live and how they're named. If an assistant, DIT, or automated transcode station creates proxies, the naming convention should preserve enough source identity for relink.


Don't use one flat` PROXIES` folder for an entire season unless the show is tiny. It will sort poorly, hide duplicates, and make partial rebuilds painful.


## Shared assets, archival assets, and stock


Multi-season shows accumulate assets that aren't camera originals: archival footage, stock, stills, graphics, music, sound effects, legal docs, clearances, and marketing pulls. These need their own logic because they may serve many episodes.


For stock and archival,[avoid vague labels](https://blog.suitestudios.io/article/filesystem-organization-best-practices-brian-levin-pureplay-entertainment-suite-cloud-storage) . A folder called` STOCK_FOOTAGE` tells you almost nothing. Name the contents around subject, source, and acquisition order or date:


```text
90_SHARED_SERIES_ASSETS/
ARCHIVAL/
2026-02-10_GETTY_CHICAGO_AERIALS/
2026-02-12_FAMILY_PHOTOS_JOHNSON/
STOCK/
001_CHICAGO_AERIAL_DAY/
002_CHICAGO_AERIAL_NIGHT/
CLEARANCES/
ARCHIVAL_RIGHTS/
MUSIC_LICENSES/


```


Keep the source, license, and usage notes close to the asset. If the same archival shot appears in five episodes, you don't want five different copies with five different clearance PDFs. Keep a canonical asset and document usage per episode.


For masters, use the same principle: one canonical master per language, format, and version when possible. Your team should generate derivatives from that master, not treat them as new originals.


## Pickups, reshoots, inserts, and other exceptions


Exceptions are where folder structures usually break. The mistake is creating a top-level` PICKUPS` folder with no relationship to the episode. That helps on the day of ingest and hurts forever after.


Pickup and reshoot material should remain connected to the episode it supports. Store pickups and reshoots under the episode they serve, with a clear shoot date and type:


```text
S01E04_EPISODE_TITLE/
01_CAMERA_ORIGINALS/
2026-04-18_PICKUPS/
A_CAM/
A023_0418GH/
2026-04-25_RESHOOTS/
A_CAM/
A024_0425IJ/


```


If the pickup shoot covers multiple episodes, store the camera originals in a season-level production shoot folder and create episode references or documentation inside each affected episode:


```text
01_SEASON_01/
05_MULTI_EP_SHOOTS/
2026-04-18_PICKUPS_BLOCK_02/
CAMERA_ORIGINALS/
AUDIO_ORIGINALS/
SHOT_LOGS/


```


Then, inside each episode, include a note or alias strategy that points to the shared shoot. Avoid duplicating original camera media into every episode unless your storage policy requires it. If you do duplicate, document which copy is authoritative.


Specials, recaps, bonus scenes, web extras, and non-episodic segments need explicit treatment too. Many media systems use` Season 00` for specials, while delivery specs may require a segment identifier for non-episodic content. In a production filesystem, you can support both ideas:


```text
01_SEASON_01/
01_EPISODES/
06_SPECIALS_AND_BONUS/
S00E01_RECAP_SPECIAL/
SEG001_CAST_INTERVIEW/
SEG002_TRAILER_PULLS/


```


The rule is to name the exception by what it's and how it relates to the show. Never let exceptions become` MISC` .


## Delivery folders should separate audience and version


Delivery folders need a different shape than work-in-progress folders. They should answer three questions fast: who is this for,[what version is it](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360039293354-Best-Practices-IMF-Folder-Naming-Structure) , and when was it delivered?


For an episode:


```text
10_DELIVERY/
NETWORK/
2026-05-01/
SHOW_S01E01_NETWORK_MASTER_v001.mov
SHOW_S01E01_NETWORK_CAPTIONS_v001.scc
SHOW_S01E01_NETWORK_AUDIO_STEMS_v001/
STREAMER/
2026-05-03/
INTERNATIONAL/
2026-05-10/


```


For season-level delivery:


```text
03_DELIVERIES/
FULL_SEASON_SCREENERS/
MARKETING_ASSETS/
PLATFORM_ARCHIVE/
QC_REPORTS/


```


Don't mix review exports, internal rough cuts, network deliveries, and final archival masters in one` EXPORTS` folder. Editorial exports are working communication, while deliverables are contractual or archival. They deserve separate lanes.


Increment version numbers only when the asset changes. Dates are useful for folders and transfer records, but version numbers are better for tracking revisions of the same deliverable.


## Compatibility with media libraries and automation


Even if your main workflow lives in Avid, Premiere, Resolve, shared storage, or a MAM, your filesystem still influences automation. Watch folders, scripts, transcode tools, archive jobs, and media library scanners all prefer predictable paths.


Consumer media library docs aren't post-production manuals, but they reinforce useful compatibility rules:


```text
Series folder
Season folder
Episode file with SxxEyy in the name


```


They also warn against mixing major content types under the same path. Media library scanners can misclassify or ignore movies, shows, specials, behind-the-scenes, and bonus content when someone dumps them together. In post terms, that means your review library, archive viewer, or automated ingest process is more likely to work if the filesystem clearly separates episodic content from non-episodic content.


Avoid characters that cause problems across tools and operating systems, and keep names boring:


```text
Use:
SHOW_S01E01_EPISODE_TITLE_v001.mov


Avoid:
Show: Ep 1 / “Final?” v1.mov


```


Use underscores or hyphens, not slashes, colons, question marks, smart quotes,[or other reserved characters](https://jellyfin.org/docs/general/server/media/shows/) . Keep path lengths reasonable. Some delivery specs and transfer systems enforce character limits, and deeply nested folders can cause avoidable failures.


## Document the convention where the team actually works


A folder structure is only a system if the team knows it. Put the convention in a short document inside the project, not only in an onboarding deck nobody opens.


Create something like:


```text
99_DOCUMENTATION/
FOLDER_STRUCTURE_README.md
NAMING_CONVENTION.md
EXCEPTION_LOG.md
CHANGE_LOG.md


```


Include examples, not just rules, and show exactly how to name an episode folder, camera card folder, proxy file, VFX pull, mix stem, and final master. Include who owns changes to the convention. If assistants, editors, post supervisors, and vendors can all change folder names independently, the structure will drift.


Run the setup past the people who use it daily. Assistant editors often know where ambiguity will hurt first. Editors know how they search. Post supervisors know what delivery and archive will require. Technical directors know where automation will break. The best structure is the one the whole team can follow under deadline pressure.


## Backups and archive are part of the structure


Folder structure and backup strategy are connected. If nobody knows which folder contains the authoritative original, nobody knows what your team must back up first.


At minimum, define which folders are critical source assets, working assets, rebuildable assets, and deliverables:


Asset class Typical folders Backup priority Rebuildability


Critical source assets` 01_CAMERA_ORIGINALS` ,` 02_AUDIO_ORIGINALS` ,` ARCHIVAL` ,` CLEARANCES` Highest priority. Protect immediately with verified copies. Usually not rebuildable if lost.


Working assets` 04_PROJECTS` ,` 06_VFX` ,` 07_COLOR` ,` 08_SOUND` High priority. Back up frequently during active post. Partially rebuildable, but loss can cost days or weeks.


Rebuildable assets` 03_TRANSCODES_PROXIES` ,` RENDERS` ,` CACHE` Lower priority unless needed for active editorial speed. Usually rebuildable from originals and project files.


Deliverables` 10_DELIVERY` ,` 03_DELIVERIES` ,` 11_ARCHIVE` High priority after approval or delivery. Sometimes reproducible, but should be preserved as the record copy.


```text
Critical source assets:
01_CAMERA_ORIGINALS
02_AUDIO_ORIGINALS
ARCHIVAL
CLEARANCES


Working assets:
04_PROJECTS
06_VFX
07_COLOR
08_SOUND


Rebuildable assets:
03_TRANSCODES_PROXIES
RENDERS
CACHE


Deliverables:
10_DELIVERY
03_DELIVERIES
11_ARCHIVE


```


A 3-2-1 backup approach means three copies of the data, on two types of media, with one copy offsite. For media workflows, that usually means working storage, a local backup or nearline copy, and an offsite cloud or LTO copy. The exact implementation varies, but the folder structure should make it clear which assets your team protects immediately and which assets you can regenerate.


Camera originals, audio originals, project files, clearances, and final deliverables should never depend on one drive or one storage volume. Your team can usually rebuild proxies and caches, so they may not need the same priority.


## The structure should survive season five


A good multi-season folder hierarchy doesn't try to predict every weird thing the show will do. It gives weird things a safe place to go.


Use show, season, and episode as the spine. Preserve camera card structures. Put season and episode identifiers in folder names and filenames. Keep shared assets out of episode folders. Treat pickups and reshoots as named production events, not junk. Document the convention with examples and update it when reality changes.


If the structure is easy to explain, easy to duplicate, and hard to misuse, it will hold up when the show gets bigger, faster, and messier. That's the real test.


## FAQ


A scalable structure usually starts with the show folder, then season folders, then episode folders, then workflow or asset-type folders. For example: SHOW_NAME/01_SEASON_01/01_EPISODES/S01E01_EPISODE_TITLE/01_CAMERA_ORIGINALS. This keeps season and episode ownership clear while giving shared assets, deliveries, documentation, and archive materials their own locations.


No. Shared assets such as fonts, LUTs, title sequences, stock footage, music packages, recurring graphics, and legal clearances should live at the series or season level. If they're buried inside one episode, later teams may copy them unnecessarily, use outdated versions, or fail to find the canonical asset.


Pickups and reshoots should be named production events, not loose folders called MISC or PICKUPS. If they belong to one episode, place them under that episode by shoot date and type, such as 2026-04-18_PICKUPS. If they cover multiple episodes, store the originals in a season-level multi-episode shoot folder and document which episodes use the material.


In most workflows, no. Preserve the original camera card structure and original source filenames unless there's a tested workflow reason to rename them. Rename the folders around the media instead. Many camera formats rely on metadata, sidecar files, spanned clip relationships, and folder structure for reliable relink and conform.


Use an explicit season and episode pattern such as S01E01_EPISODE_TITLE. Keep the same padding throughout the series, such as two digits for season and two digits for episode, or three episode digits if needed. Final files should also include the show, season, episode, version, and deliverable type, for example SHOW_S01E01_NETWORK_MASTER_v001.mov.


The folder structure should give assets a stable home, but search depends on metadata as much as path names. Aspect lets teams add project-wide fields such as episode used, source, rights status, location, and asset type, making a long-running library easier to filter with custom metadata.
