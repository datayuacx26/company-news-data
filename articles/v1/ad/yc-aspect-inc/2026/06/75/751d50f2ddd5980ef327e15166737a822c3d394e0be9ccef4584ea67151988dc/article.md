---
schema_version: "1.0.0"
document_id: "751d50f2ddd5980ef327e15166737a822c3d394e0be9ccef4584ea67151988dc"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/how-to-set-up-auto-save-and-version-control-for-editing-projects"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-24T08:04:34.272722+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:8efecb4454e0143cb215bda712ca6447210f72ebb1292879e9eb32ad8786b615"
---

# How to Set Up Auto-Save and Version Control for Editing Projects

Auto-save protects against crashes, while version control preserves earlier editorial decisions. In practice, you need both because they solve different problems.


In an edit, the usual loss isn't every frame of work. The more likely problem is losing the last known-good state before someone nested the wrong sequence, replaced synced audio, flattened a multicam, updated software, or “just cleaned up the bins.” A usable setup gives you three ways back: automatic recovery, named project versions, and archived milestone edits that another person can understand without calling you.


## Auto-save isn't version control


Treat auto-save as a short-term crash net. It should catch the last few minutes or hours of work when the NLE quits, the machine freezes, or a project file stops opening.


Treat version control as editorial memory because it should answer questions like:


- What was the edit before client notes?
- Which version went to color?
- Who made the stringout that sound is using?
- Can we get back to the timeline before the conform prep?
- Which project file was active on the day picture locked?


Those are different jobs, so don't make one system do both. Set auto-save for crash recovery, then create intentional manual versions at predictable moments.


Auto-save catches crashes; manual versions preserve deliberate checkpoints.


## Save locations should match the risk


The default “auto-save next to the project” behavior is convenient, but it isn't always enough. If the folder gets deleted, synced badly, or corrupted, your auto-saves may go with it. If auto-saves are on a slow network volume, you may get pauses or failed saves, while auto-saves that live only on the local system drive may be unavailable to another editor who needs to recover the work.


Separate working files, recovery files, and archive files. Here is a folder structure that makes those roles visible:


Keep working files, recovery backups, and archives in clearly separate places.


```text
ProjectName/
01_Project_Files/
Premiere/
Resolve/
Avid/
02_Auto_Saves/
Premiere_Auto_Save/
Resolve_Backups/
Avid_Attic_Copies/
03_Exports/
Review/
Turnover/
Masters/
04_Archive/
Daily_Project_Versions/
Milestone_Project_Versions/
Turnover_References/


```


The exact folder names matter less than the separation. Anyone opening the project should be able to tell which file is the active working file, which files are emergency recovery files, and which files are frozen historical versions.


For live work, keep the active project file on storage that can play the project reliably and is included in backups. When the NLE allows it, keep auto-save or backup copies on a different path from the active project. Avoid using a cloud-sync folder as the only live project location while the NLE is open, because sync conflicts and partial uploads can create confusing duplicates. Cache, renders, and auto-saves shouldn't all disappear into the same “mystery scratch” location with no cleanup policy.


If you're on shared storage, agree on the path before the first day of cutting. If you're on laptops or remote systems, make sure auto-saves are included in your backup plan, not just the main project file. The assistant editor, post supervisor, or technical director should know where the recovery files live.


## Premiere Pro: project auto-save, Productions, and Team Projects


Premiere Pro uses project files, usually` .prproj` , and its auto-save behavior is easy to understand. By default,[Premiere auto-saves every five minutes](https://helpx.adobe.com/premiere/desktop/get-started/preferences-and-settings/auto-save-preferences.html) and keeps the last twenty project versions. Those auto-saves are stored in a Premiere Auto-Save folder unless your workflow or scratch settings route them elsewhere.


For a normal local Premiere project, open Preferences, then Auto Save. Enable automatic saving, set the interval based on the pace of the job, and set the maximum number of project versions high enough to cover more than one bad session. Confirm where the Premiere Auto-Save folder is being created. After changing the setting, force a save, make a small harmless edit, wait through the interval, and make sure a new auto-save file appears where expected, because a setting isn't real until you've seen the file appear in the right place.


A practical Premiere interval for most editorial work is 5 minutes with at least 20 versions. For client-supervised sessions, reality shows, multicam edits, or anything with lots of small timeline moves, use 3 to 5 minutes and more retained versions. For projects where saving causes a noticeable pause, you may need 10 minutes, but compensate with more manual saves.


Premiere has a few workflow-specific traps:


- Auto-save can preserve a broken state if you keep working after the mistake.
- If “save current project” behavior is enabled in your version, auto-save may overwrite the active file as well as create backups, which is useful for crash recovery but not a substitute for manual versioning.
- A project file inside a cloud-synced folder can be duplicated, conflicted, or partially synced if multiple machines touch it.
- Productions and[Team Projects](https://helpx.adobe.com/premiere/desktop/get-started/preferences-and-settings/best-practices.html) have their own collaboration logic, so “I saved” doesn't always mean “everyone else has my current work.”


For Productions, the advantage is that the job is[broken into multiple project files](https://www.editorsguild.com/Portals/0/Images/Training/Premiere-Pro-Productions-Workflow-Guide.pdf) . That can reduce the effect of one corrupted file, but it also means you need a clear policy for which projects are active, which are read-only references, and where auto-saves go. If one bin-like project file goes bad, you may be able to recover just that piece instead of the entire show.


For Team Projects, remember that Publish and Update aren't the same thing as auto-save. Auto-save may protect your local state, but collaborators don't see your work until you publish it. Build a habit of publishing after coherent chunks of work, not every few seconds and not only at the end of the day.


A simple Premiere manual versioning pattern looks like this:


```text
ProjectName_EDIT_20260220_v001_JD.prproj
ProjectName_EDIT_20260220_v002_JD.prproj
ProjectName_EDIT_20260221_v003_AM.prproj
ProjectName_LOCKED_20260225_v010_JD.prproj
ProjectName_COLOR_TURNOVER_20260226_v011_JD.prproj


```


Use the same pattern for sequence names inside the project, because the project filename alone won't help if someone duplicates the wrong timeline.


## DaVinci Resolve: Live Save, backups, and exported projects


Resolve thinks differently because projects live inside a Project Library, not as ordinary standalone project files in the same way Premiere does. That makes the backup conversation more important. If you only enable Live Save, Resolve keeps saving your current state as you work, which is convenient but can also immediately save mistakes.


Resolve gives you three relevant tools:[Live Save, Project Backups](https://beginnersapproach.com/davinci-resolve-autosave-livesave/) , and Timeline Backups. You configure them in Preferences under the User settings, in Project Save and Load.


A practical Resolve setup uses Live Save for everyday crash protection, Project Backups for recoverable snapshots, and Timeline Backups when the edit is timeline-heavy or multiple timelines are being tested. Set the backup location to a known folder outside the active database or library path when possible. Export` .drp` project files at milestones, especially before turnovers, grade work, conform prep, or software updates.


Live Save is useful for not losing the last action, while Project Backups are what you reach for when Live Save preserved the thing you wish it hadn't preserved.


For interval settings, use Live Save for continuous saving, project backups every 5 to 10 minutes during active editorial, hourly backups for longer retention, and daily backups for deeper history if your version of Resolve exposes those options. The exact controls can vary by Resolve version, but the principle is stable: short interval for recovery, longer retention for rollback.


Resolve failure modes usually come from a different place than Premiere. Watch for these:


- Project Library or database corruption.
- Backups stored on a drive that isn't mounted.
- Disk permission problems that prevent saves.
- Confusion between the active project and an exported` .drp` .
- Collaborative projects where one person assumes another person’s local backup is enough.


The safest Resolve milestone habit is to export a` .drp` at every major editorial state and store it in your archive folder. For bigger jobs, also back up the Project Library itself according to your facility policy. A` .drp` is easy to move, label, and restore, while the Project Library backup protects the larger environment.


For timeline naming, don't leave everything as “Timeline 1 copy copy.” Use names that explain the state:


```text
EP03_MainCut_20260220_v004_JD
EP03_NetworkNotes_20260222_v006_AM
EP03_PictureLock_20260225_v010_JD
EP03_ColorTurnover_20260226_v011_JD


```


Resolve makes it easy to duplicate timelines, so people overdo it. Duplicate when the edit reaches a meaningful state, not after every tiny trim.


## Media Composer: bins, the Avid Attic, and shared project discipline


Media Composer is bin-based. Instead of one giant project file containing everything, your project is made of bins, sequences, and supporting project data. Auto-save protects bins, and the Avid Attic stores previous versions of bins.


Configure auto-save in Media Composer’s Bin settings. Depending on the version, you'll see controls for auto-save timing, inactivity behavior, forced auto-save timing, and how many attic versions are retained. Facilities often standardize these settings, so check before changing them on a shared system.


For most Media Composer editorial rooms, use a short auto-save interval and enough attic depth to survive a bad afternoon, not just a crash. The Avid Attic is useful, but it isn't the same as a consciously named sequence version. It tells you a bin existed at a time, but it doesn't tell you which edit was sent to the director.


Avid’s versioning should happen mostly at the sequence and bin level. A common pattern is to duplicate the active sequence at the start of the day, then duplicate again before major restructuring, turnovers, or client sessions. Put old versions in a clearly named archive bin. If your team prefers a clean room, keep only the current active sequence in the active working bin. When multiple people touch the same project, add editor initials and dates to sequence names.


That gives you human-readable editorial history while the Attic keeps doing its recovery work in the background.


Media Composer’s shared-project model also changes the risk profile. Bin locking prevents two people from unknowingly writing to the same bin at the same time, but it doesn't prevent bad creative choices, accidental deletes inside an unlocked bin, or confusion caused by duplicate sequences with vague names.


A clean Avid naming pattern might be:


```text
SHOW101_SC03_Cut_20260220_v003_JD
SHOW101_SC03_DirectorNotes_20260221_v004_AM
SHOW101_SC03_Locked_20260224_v008_JD


```


If you need to recover from the Attic, copy the recovered bin out, rename it clearly, open it alongside the current project, and pull only the needed sequence or item back into the working bins. Don't blindly replace a current bin unless you're sure nobody else has added useful work since the attic copy was created.


## How the three big NLEs differ


Premiere Pro, DaVinci Resolve, and Media Composer all have workable protection systems, but they protect different objects.


NLE What it mainly protects Built-in safety tool Best manual versioning habit Main watchout


Premiere Pro` .prproj` files, Productions project files, and Team Project local states Auto Save, Productions, Team Projects publish and update Save dated` .prproj` versions and keep sequence names in sync with project versions Auto-save can preserve mistakes, and cloud-sync or collaboration confusion can create conflicting project states


DaVinci Resolve Projects inside a Project Library plus individual timelines Live Save, Project Backups, Timeline Backups Export` .drp` files at milestones and duplicate timelines only at meaningful cut states Live Save immediately saves bad decisions, and the Project Library location may be less obvious to editors


Media Composer Bins, sequences, and shared project data Bin auto-save, Avid Attic, bin locking Duplicate sequences before major changes and store older cuts in clearly named archive bins Attic recovery restores bin versions, so replacing current bins blindly can overwrite useful newer work


Premiere is straightforward for single-editor project files. You can see the` .prproj` , duplicate it, rename it, and archive it. Productions improve large-project organization, and Team Projects add collaboration features, but you need clear Publish and Update habits. Premiere’s risk is that project files can become messy, while casual duplication can create a swamp of “final_final” files if nobody enforces naming.


Resolve saves continuously when Live Save is enabled and can manage multiple timelines, but the Project Library model can hide where the project really lives. Live Save is a comfort until you realize it also saves mistakes immediately. A Resolve setup should combine Live Save, scheduled backups, and exported` .drp` milestones.


Media Composer’s bins, locks, and Attic are designed around shared editorial work. Recovery can feel less obvious to newer editors because you may be restoring a bin, not a single “project file.” The upside is that one damaged bin doesn't always mean the entire show is damaged.


If you're choosing a workflow, choose based on how the team works:


- Single editor, short-form, lots of client revisions: Premiere or Resolve with consistent manual versioning works well.
- Color-heavy workflow or finishing inside the same app: Resolve’s timeline backups and` .drp` exports are valuable.
- Multi-editor long-form with assistants and shared storage: Media Composer’s bin model is well suited to that work.
- Premiere long-form with multiple editors: use Productions or Team Projects intentionally, not as an afterthought.


The tool matters, but the habit matters more. Auto-save can't rescue a team that can't identify the correct locked edit.


## Manual versioning rules that hold up


Manual versions should be created at moments where the project’s meaning changes. Don't make a new version every time you save, but don't wait until the end of the week either.


These moments deserve named versions:


- Start of each edit day.
- End of each edit day.
- Before importing a large batch of new media.
- Before syncing, grouping, or replacing production audio.
- Before flattening multicam, nesting, rendering-and-replacing, or baking effects.
- Before applying client, director, network, or agency notes.
- Before[turnover to color](https://www.cined.com/the-correct-way-to-hand-your-video-project-over-to-a-colorist/) , sound, VFX, captions, or online.
- Before updating the NLE, plugins, GPU drivers, shared storage client, or operating system.
- At picture lock, even if you know notes are coming.


This gives you useful rollback points without creating hundreds of files that don't mark a meaningful editorial state.


For daily saves, make the version even if you think nothing major happened. Editorial memory is fuzzy, and “nothing major” often becomes “the last version before we tried that thing.”


For milestone saves, freeze the version and stop editing inside it. If notes continue, duplicate forward into the next version. A picture lock version should remain the picture lock version, which means that if it changes, it needs a new version name.


## Naming that doesn't collapse under pressure


Good names sort correctly, explain themselves, and survive being copied to another drive.


Use this format for project files:


```text
Project_EpisodeOrSpot_Department_YYYYMMDD_v###_Initials.ext


```


For example:


```text
RIVER_S104_EDIT_20260220_v012_JD.prproj
RIVER_S104_EDIT_20260220_v012_JD.drp
RIVER_S104_AVID_20260220_v012_JD.avb


```


Use this format for sequences or timelines:


```text
Project_EpisodeOrSpot_CutName_YYYYMMDD_v###_Initials


```


For example:


```text
RIVER_S104_MainCut_20260220_v012_JD
RIVER_S104_Teaser_NetworkNotes_20260221_v003_AM
RIVER_S104_PictureLock_20260225_v018_JD


```


Keep these naming rules consistent:


- Use` YYYYMMDD` , not` 2-20-26` , so[files sort correctly](https://www.nemovideo.com/blog/version-control-video-edits-teams) .
- Use three-digit versions like` v001` ,` v002` ,` v003` .
- Use initials when more than one person may create versions.
- Use milestone words only when they're true, such as` PictureLock` ,` ColorTurnover` , or` SoundTurnover` .
- Avoid` final` ,` final2` ,` real_final` , and` new_new` .
- Don't rename old versions after delivery unless your team has agreed to an archive policy.


Pretty filenames are secondary. The goal is that a tired editor can find the right version at 11:47 p.m. while a producer is waiting.


## Version the exports too


Project versions and export versions need to talk to each other. If` v012` of the project generated the client review export, the export should carry that version number or reference it in a notes document.


A project file and its export should share an obvious version relationship. A clean review export folder might look like this:


```text
03_Exports/
Review/
RIVER_S104_v012_ClientReview_20260220.mp4
RIVER_S104_v013_ClientNotes_20260221.mp4
RIVER_S104_v018_PictureLock_20260225.mp4
Turnover/
RIVER_S104_v018_ColorTurnover_Reference_20260226.mp4
RIVER_S104_v018_SoundTurnover_Reference_20260226.mp4


```


This prevents the classic problem where the client is commenting on` Review_v5.mp4` , the editor is working in` v017` , and color received an XML from something else entirely.


For turnovers, include the version in the project file, timeline name, reference export, XML/AAF/EDL, and any notes document. Picture lock should mean everyone can identify the same timeline, not just that someone said “locked” in a meeting.


## Recovery from a corrupted project file


When a project stops opening, resist the urge to keep double-clicking it and hoping. First preserve the evidence. Duplicate the broken project file or bin and move the copy into a recovery folder. Then work from copies of backups, not the only remaining original.


Protect the corrupted original and test recovery on copies. Start with the most recent auto-save or backup copy. If it opens, immediately save it as a new active project version with a recovery name. If the latest backup fails, step backward through older backups until one opens. Once you've a project open, compare the most recent active sequence or timeline against the latest export, notes, or reference file before doing broader repair work.


Relink media only after you know the project structure is usable. If the project opens but crashes on a specific timeline, import or copy bins into a clean project one at a time. If one bin, timeline, effect, title, plugin, or linked asset triggers the crash, isolate it and rebuild only that piece. This keeps you from turning a recoverable corruption issue into a bigger mystery.


Premiere recovery usually means opening a` .prproj` from the Auto-Save folder, then using Save As to create a new main project file. If the project opens but behaves badly, create a clean project and import sequences or bins from the damaged one. If a specific sequence crashes, duplicate earlier versions of that sequence and narrow down the bad object.


Resolve recovery usually means restoring from Project Backups or Timeline Backups, then exporting a fresh` .drp` once you've a stable version. If the Project Library itself is suspect, don't keep working inside it casually. Create or restore into a clean library according to your facility’s workflow.


Media Composer recovery often means going to the Avid Attic, finding the relevant bin version, copying it out, renaming it, and opening it in the project. Pull the needed sequence or clips into a current bin rather than replacing current work blindly.


After recovery, don't just continue working with “Recovered Project 3.” Rename it into the normal version sequence so the team knows it's now the active state:


```text
RIVER_S104_EDIT_20260221_v014_RECOVERED_JD.prproj


```


Then, once everyone agrees it's good, continue forward:


```text
RIVER_S104_EDIT_20260221_v015_JD.prproj


```


That preserves the recovery event without making the rest of the job live forever in panic filenames.


## The tradeoff with aggressive auto-save


Shorter intervals reduce the amount of work you can lose, but they aren't free. Auto-save can interrupt playback, stall large projects, or create dozens of files that nobody understands. Longer intervals are smoother, but they increase the amount of work you can lose.


Set intervals by session type:


- 3 to 5 minutes for supervised sessions, fast notes, reality, multicam, and unstable systems.
- 5 to 10 minutes for normal offline editorial.
- 10 to 15 minutes only when saves are disruptive and you've strong manual version habits.
- More retained versions for long days, shared projects, and inexperienced teams.
- Fewer retained versions only when storage is tightly managed and other backups are strong.


If you notice auto-save causing pauses, don't simply turn it off. Move the project to faster storage, reduce project bloat, split the job into smaller projects or bins, increase retained manual versions, or schedule saves during natural breaks.


## Media and project files need different protection


Project files are small compared with camera media, so versioning project files aggressively is cheap. Duplicating camera originals isn't.


Use a different protection model for media:


- Back up camera originals to at least two locations before cards are formatted.
- Use[checksum-verified transfers](https://blockreeldao.com/blog/media-management-101-checksums-folder-rules-and-backup-strategies) when possible.
- Keep camera originals read-only for editorial.
- Version project files, timelines, bins, XMLs, AAFs, EDLs, and notes.
- Don't duplicate entire media folders just because an edit changed.


This distinction saves storage and prevents confusion. The project version should point back to stable media, not create a new media universe every time someone makes notes.


## Collaboration rules should be boring and explicit


Most versioning failures are people problems wearing technical clothes. Two editors both think they made the current version, while an assistant exports from the wrong timeline or a producer asks for the “latest” and gets yesterday’s review file. A colorist can receive an XML from one edit and a reference movie from another, which means the system has failed even if every file technically saved.


Write the working rules in plain language and put them somewhere obvious. The rules should cover:


- Who is allowed to create official version numbers.
- Whether versions increment by project file, sequence, export, or all three.
- Where active projects live.
- Where archived versions live.
- How editor initials are used.
- When old versions can be deleted, if ever.
- How turnovers are labeled.
- How remote editors publish, upload, or hand off work.


Once those rules exist, enforce them gently but consistently. The first time someone sends` newest_edit_USE_THIS_ONE` , rename it properly and explain why.


## Prove the backup path early


The only proof your setup works is opening a backup, and the best time to do that's early in the project. Create a harmless edit, wait for auto-save, then open the auto-save or backup copy. Make sure the expected timeline is there, media links behave as expected, and the file can be saved as a new version.


In Resolve, restore or open a project backup and export a test` .drp` . In Media Composer, recover a test bin from the Attic and bring a sequence back into the project. You don't need to overcomplicate this because ten minutes of testing can save a day of panic later.


## A simple working standard


If your team has no policy yet, start with a modest standard and make it consistent. Auto-save every 5 minutes, retain at least 20 auto-save versions, and keep more for long-form or shared jobs. Save active projects on reliable working storage, and store auto-saves or backups in a known recovery folder.


Create one manual version at the start of the day and one at the end of the day. Create milestone versions before notes, turnovers, software updates, and picture lock. Use` YYYYMMDD_v###_Initials` in every project, sequence, timeline, and review export name. Avoid “final” unless the job is truly delivered, and even then include the version number.


That gives you crash recovery, daily rollback points, milestone archives, and names that another person can follow. You can make the system fancier later, but the core idea stays the same: automatic saves for crashes, manual versions for decisions, and names clear enough that someone else can recover the job without you in the room.


## FAQ


For most editing work, set auto-save to every 5 minutes and retain at least 20 versions. Use 3 to 5 minutes for supervised sessions, multicam, reality, or unstable systems. Use 10 minutes only if saving causes noticeable pauses and you are disciplined about manual versions.


No. Auto-save is short-term crash protection. It helps recover recent work after a crash, freeze, or failed project file. Version control is intentional editorial history. It preserves important states such as pre-notes cuts, picture lock, turnovers, and end-of-day versions.


Store backups in a known recovery folder, ideally separate from the active project file when the software allows it. Avoid relying on a cloud-sync folder as the only live project location. On shared jobs, make sure assistants, editors, or the post supervisor know exactly where recovery files are stored.


Use names that sort correctly and identify the project, date, version, and editor. A reliable format is Project_EpisodeOrSpot_Department_YYYYMMDD_v###_Initials.ext, such as RIVER_S104_EDIT_20260220_v012_JD.prproj. Avoid names like final, final2, or use_this_one.


First, duplicate the broken file or bin and preserve it in a recovery folder. Then open the newest auto-save or backup copy. If that fails, step backward through older backups until one opens. Once you find a stable version, save it as a new properly named project version before continuing work.


Use matching version numbers across the project file, timeline, review export, and notes. A review system should also keep comments, annotations, approvals, and revision history attached to the exact upload being reviewed. Aspect supports timestamped comments, version stacking, and change history inside the same[review workflow](https://aspect.inc/features/review-and-approve#change-history) .
