---
schema_version: "1.0.0"
document_id: "e567a45c61c501af0a0856112c0fab0a9e83122be870822eeebb8e24c0839c85"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/how-to-automate-dailies-delivery-with-watchfolders-and-presets"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-09T21:00:56.600033+00:00"
fetched_at: "2026-08-09T21:00:58.529020+00:00"
content_hash: "sha256:cf0ad70169af631b5e7228d655fc05d2f22a8157f7ef8ee472a8372f20a7e383"
---

# How to Automate Dailies Delivery with Watchfolders and Presets

Start by deciding what a dropped file is supposed to become. The watchfolder is only the trigger, and the real workflow is the preset, the folder structure around it, the naming convention, the error behavior, and the record of what happened after the job finishes.


For dailies, that distinction matters because one shoot day can produce several valid outputs from the same source media. Editorial may need edit-friendly proxies with synced audio and metadata. A director may need fast review files with burn-ins. A client may need a smaller, branded, watermarked version with limited access. If all of those targets share one generic “transcode” folder, the system will eventually make the wrong thing very quickly.


A good watchfolder setup makes the decision obvious before your team drops media. The person moving files shouldn't have to remember codec recipes, frame sizes, watermark rules, or where to upload the result at 1:00 a.m. They should only need to place the correct media package into the correct location.


Separate watchfolders can represent separate delivery intents.


## Build the workflow around delivery targets


Dailies aren't just compressed copies of camera files. They're the daily bridge between set, editorial, production, and review. They usually arrive after your team has backed up, checked, and organized camera originals and production audio. That means your automation should sit after data safety, not before it.


The most reliable pattern is to create[separate watched locations](https://www.youtube.com/watch?v=NHo2s_UEcdI) for each delivery intent. For example, a small production might use folders like this:


```text
/Watchfolders
/Editorial_Proxy
/Director_Review
/Client_Review
/Audio_Only
/Failed
/Completed
/Logs


```


Each watched folder maps to one preset or delivery recipe. The folder name should describe the output. “A001_Camera” is useful inside the media package, but it isn't enough to tell the automation whether the result should be ProRes Proxy, H.264 review, DNxHD, WAV, or something else.


A larger team may split by show, episode, shoot day, or unit:


```text
/ShowName
/S01E03
/Day_014
/Editorial_Proxy
/Director_Review
/Client_Review


```


That structure gives post supervisors and assistants a quick way to answer the operational questions that come up every morning: what the system processed, what failed, what was sent, and which version editorial received?


## Choose presets by destination


Presets are where the workflow becomes specific. A preset should define the transcode settings,[audio treatment, overlays, metadata output](https://kb.pomfort.com/silverstack/hands-on/creating-dailies/transcoding-workflow/) , destination, and any naming rules. Some systems separate[transcoding presets from delivery presets](https://help.ayon.app/articles/9181441-batch-delivery-configuration) , which is useful. The transcode preset says “make this kind of file,” while the delivery preset says “put it here, name it this way, and send or register it with this destination.”


Delivery target Typical output Preset should prioritize Common mistake to avoid


Editorial ProRes Proxy, DNxHD, DNxHR, or another edit-friendly proxy Source timecode, reel or clip metadata, synced audio, stable frame rate handling, relink-safe naming Making a file that plays well but does not relink cleanly for conform


Director review H.264, H.265, or platform-ready review file Fast playback, timecode and clip burn-in, viewing LUT, simple delivery path Overbuilding the file like an editorial proxy when speed matters more


Client review Compressed review file with approved branding or watermark Access control, watermarking, approved identifiers, limited metadata exposure Exposing internal clip names, notes, or burn-ins that were meant only for production


Overview stringout Combined day, scene, or selected-take sequence Easy scanning, clear separators, useful burn-ins, small file size Treating the stringout as an editorial source for cutting or conform


Audio or sync package WAV, ALE, CSV, XML, or sidecar metadata Channel mapping, sync references, clip IDs, NLE compatibility Delivering picture files without the metadata or audio structure editorial needs


For dailies, the common preset families are usually easy to separate:


- Editorial dailies: edit-friendly codec, consistent frame rate, source timecode, reel or clip metadata, synced production audio, and handles only if the editorial workflow expects them.
- Director review: smaller file, fast playback, visible clip name and timecode burn-in, basic color transform or look, and delivery to a review platform or shared location.
- Client review: compressed deliverable, watermark or branding if required, limited metadata exposure, and a destination controlled by production or post.
- Quick overview strings: combined clips or scene/day compilations for fast scanning, not for editorial conform.
- Audio replacement or sync packages: WAV, ALE, CSV, or sidecar metadata depending on the downstream system.


The takeaway is that “low-res” isn't a spec because editorial low-res and client low-res can be completely different files with different risk profiles. Editorial cares about relinking, timecode, audio channels, and metadata integrity. Client review cares about access, playback, approvals, and sometimes confidentiality.


When you name presets, include the target, codec, frame size, frame rate behavior, and version. A name like` Editorial_ProResProxy_1080p_SourceTC_v03` is boring, but boring is good. A name like` Dailies_New` will become a trap after the next prep day.


## Decide whether the watchfolder is local, NAS, or cloud


The right location depends on where your team uploads or copies the media and who needs the result. A local watchfolder is simple and fast when a DIT, dailies technician, or assistant editor is sitting next to the workstation doing the encoding. A NAS watchfolder makes more sense when multiple post team members or systems need access. A[cloud watchfolder](https://docs.hybrik.com/tutorials/watchfolders/) is useful when remote ingest, automatic upload, or distributed review is part of the job.


The tradeoffs are practical:


- Local storage: easiest to troubleshoot, lowest network dependency, but tied to one machine.
- Shared NAS: good for a facility or editorial room, but requires permissions, mount stability, and enough throughput for simultaneous reads and writes.
- Cloud storage: strong for remote teams and automatic distribution, but depends on upload speed, polling behavior, cloud costs, and transfer reliability.
- Hybrid storage: common for productions that process locally, then upload completed outputs or metadata automatically.


Watchfolder automation can run in any of these models, but each has a different failure mode. Local jobs fail when the workstation sleeps, runs out of disk, or loses access to a license. NAS jobs fail when permissions change, mounts disconnect, or another process moves files too soon. Cloud jobs fail when uploads are incomplete, credentials expire, or polling sees an object before it's fully available.


The location should match the weakest part of the day. If set internet is unreliable, don't make cloud upload the first required action before editorial can start. If the assistant team is remote, don't strand completed dailies on a local machine with no transfer automation.


## Make file arrival boring and predictable


A watchfolder shouldn't process a file the instant its filename appears. Large media files can take minutes or hours to copy. If the automation grabs a growing file, you get corrupt outputs, partial transcodes, or strange failures that are hard to reproduce.


A dependable watchfolder watches for stability before starting a job. That can mean scanning at intervals, checking whether[file size has stopped changing](https://steinberg.help/wavelab_pro/v10/en/wavelab/topics/batch_processing/watch_folder_settings_dialog_r.html) , waiting for a marker file, or only processing completed folders after a transfer tool says the copy is finished.


A stable-file check keeps incomplete media out of the transcode queue. Good arrival rules usually include these behaviors:


- Have the automation poll the folder at a predictable interval so it doesn't constantly hammer storage.
- Ignore temporary, hidden, or partial transfer files.
- Wait until file size and modified time remain stable for a set period.
- Process folders as packages when audio, video, LUTs, and metadata must stay together.
- Move or mark items once the automation has claimed them.


This is one of the places where simple automation becomes dependable enough for dailies. The goal is to be reliable enough that the night assistant editor doesn't have to babysit every copy.


If your workflow depends on sidecar files, camera reports, ALEs, CDLs, LUTs, or production audio, think carefully about whether a single media file is enough to trigger the job. Often the better trigger is a completed shoot-day folder, not an individual clip.


## Keep original camera media out of the danger zone


Dailies automation shouldn't casually manipulate original camera files or original audio files. Studio guidance often separates responsibilities for backing up, archiving, and clearing camera cards because the consequences are serious. If nobody knows whether the dailies team, loader, DIT, or post team is responsible for archiving OCF and OAF, automation can make confusion worse.


A safe pattern is to keep sources, work-in-progress, outputs, and transfer areas separate:


```text
/Source_ReadOnly
/Watch_Incoming
/Processing
/Output
/Completed
/Failed
/Archive_Manifest


```


The automation reads from a controlled input and writes somewhere else. It shouldn't delete or overwrite source media. If your system needs to move files after processing, move the copied input package, not the only copy of the original.


Automation should read from protected media and write results elsewhere. For camera card workflows, automation should begin after your team has[verified offload and backup](https://partnerhelp.netflixstudios.com/hc/en-us/articles/4415931246995-Dailies-Best-Practices) . That can be RAID, LTO, cloud object storage, or another approved storage plan, depending on the production. The important thing is that your team doesn't treat the watchfolder as the archive just because it contains media.


## Design outputs for editorial relink


Editorial dailies have different requirements from review dailies. They need to survive the path from ingest to offline edit to turnover and conform. That means the preset has to preserve[the identifiers editorial will rely on](https://www.arri.com/en/learn-help/learn-help-camera-system/pre-postproduction/editorial-workflow) later.


For an editorial proxy preset, pay attention to these fields and behaviors:


- Source timecode and duration.
- Reel, tape, camera roll, or clip name strategy.
- Filename consistency with camera originals or the agreed editorial convention.
- Audio channel mapping, including mix tracks and isolated microphones.
- Frame rate handling, especially for off-speed or mixed-rate material.
- Color transform or viewing LUT, without baking decisions that should remain flexible.
- Sidecar metadata such as ALE, CSV, EDL, XML, or project bins when required.


This is where editorial should have input during prep. Editors and assistant editors know what their NLE and turnover process need. A preset that looks fine in a review player can still be a bad editorial file if the preset flattens the audio channels incorrectly or the reel name doesn't support conform.


After the first test batch, open the files in the actual editorial system. Don't only play them in Finder, Resolve, VLC, or a browser. Confirm that the NLE sees the expected timecode, audio layout, metadata, and duration. That verification belongs near preset creation, not at the end of the first shoot week.


## Use burn-ins intentionally


Burn-ins are useful until they aren't. For director and production review, visible clip name, source timecode, camera, scene, take, and date can save a lot of back-and-forth. For client review, the same burn-in might expose information you don't want outside the core team. For editorial, burn-ins may be helpful on picture but shouldn't replace real metadata.


A practical burn-in strategy separates audiences:


- Internal review: clip name, source timecode, camera roll, scene/take, shoot day, and look name if useful.
- Director review: source timecode and clip or scene/take, usually with minimal clutter.
- Client review: watermark, project branding, and only the identifiers production approves.
- Editorial proxy: burn-in only if editorial wants it, with metadata preserved in the file or sidecar.


The preset should make those choices automatic because if your team is manually toggling burn-ins per batch, the wrong version will eventually go to the wrong audience.


## Route completed jobs without relying on memory


The transcode is only half the delivery. Once the file exists, it has to be delivered where the next person expects it. That might be a shared storage folder, a review platform, a cloud bucket, a media asset manager, an editorial NAS, or a transfer portal.


For each preset, define the completed output route as part of the delivery recipe:


```text
Editorial_Proxy -> /Editorial/Ingest/ShowName/Day_014
Director_Review -> /Review/Director/ShowName/Day_014
Client_Review -> /Review/Client/ShowName/Day_014


```


If your workflow includes uploads, the automation should record whether upload completed, not just whether the local encode completed. A successful transcode sitting in a local output folder isn't a successful delivery if the director is waiting in another city.


For remote productions, watchfolder systems are often paired with automatic transfer tools. Your team can drop media into a portal, transfer tools can copy it into cloud or NAS storage, the automation can transcode it, and the system can deliver it onward without a person downloading and re-uploading every batch. That can remove a lot of manual handoff, but only if the handoff points are logged clearly.


## Treat failures as expected events


Every dailies system will hit bad files, unsupported codecs, missing audio, full disks, disconnected storage, expired credentials, and human mistakes. The difference between a usable system and a stressful one is whether failures are visible and recoverable.


Useful failure categories include:


- File still copying or never reached a stable state.
- Unsupported, corrupt, or unexpectedly wrapped media.
- Missing sidecar metadata, LUT, CDL, or production audio.
- Preset mismatch, such as a codec the automation tool doesn't support.
- Destination unavailable because of network, permissions, or credentials.
- Insufficient local, NAS, or cloud storage.
- Duplicate filename or already-processed package.
- Application crash, worker timeout, or license issue.


Don't dump all failures into one mystery folder. Configure the automation to[move failed jobs](https://odailies.com/docs/uploading/) into a location that preserves the original package and writes a readable error report next to it. The assistant editor should be able to tell whether the fix is “retry after copy finishes,” “relink missing audio,” “use a different preset,” or “call engineering.”


For retries, avoid automatic infinite loops. A corrupt file that retries forever can block the queue and hide newer work. Configure the system to retry a small number of times for transient issues, then quarantine the job and alert a human.


## Log like someone will ask tomorrow


Dailies questions often arrive after everyone is tired: Did scene 27B go to editorial? Which preset made the client file? Was the director sent the version with the updated LUT? Did the upload finish before the notes call? Logging answers those questions without archaeology.


A useful job log records the operational facts:


A useful job log gathers the operational facts in one record.


- Source path and source filename.
- File size, duration, codec, resolution, frame rate, and timecode start when available.
- Preset name and preset version.
- Output filename and destination path.
- Job start time, finish time, and processing duration.
- Worker machine or service that ran the job.
- Transfer or upload status.
- Error message and retry count if the job failed.
- User or process that submitted the media, if known.


A CSV, JSON file, database table, or system dashboard can all work. What matters is consistency. If the team can filter by shoot day, preset, status, and destination, most daily troubleshooting becomes much faster.


Preset version is especially important. If you change audio mapping or burn-in layout on day five, the log should show which files the system made before and after the change. Otherwise, you may not know which dailies your team needs to regenerate.


## Keep humans in the loop where judgment matters


Automation is best at repeatable routing and encoding. It isn't a replacement for dailies supervision, color judgment, sync checks, or editorial communication. A watchfolder can detect new media, run a preset, upload outputs, and write logs. It can't decide whether a LUT is creatively right, whether a take should be held back, or whether a metadata convention changed because editorial requested it.


A practical automated dailies workflow still has human review at the right points. The assistant editor or dailies technician should see enough status to catch missing clips, obvious sync problems, incorrect burn-ins, or failed uploads before the files become someone else’s problem.


The best version of this workflow feels uneventful. Media shows up in a known place, and the system waits until it's safe to process. The correct preset runs for the intended audience, and outputs route to the right destination. Failures are isolated with readable reasons, and completed jobs are logged well enough that post can answer questions without digging through drives.


That's the real value of watchfolder automation for dailies. It makes the technical decisions repeatable, visible, and much harder to mess up under pressure.


## FAQ


They should usually be organized by delivery target. A folder named for the intended output, such as Editorial_Proxy, Director_Review, or Client_Review, makes it clear which preset should run. Organizing only by camera roll or source type can create confusion because the same camera media may need multiple different outputs.


A reliable watchfolder should wait until files are stable before starting a job. Common methods include polling at intervals, checking that file size and modified time have stopped changing, ignoring temporary transfer files, or requiring a marker file that confirms the copy is complete. For complex dailies packages, it's often safer to trigger processing from a completed folder rather than from a single media file.


An editorial dailies preset should preserve source timecode, duration, reel or clip metadata, filename conventions, audio channel mapping, frame rate behavior, and any required sidecar metadata such as ALE, CSV, XML, or bins. The output should be tested inside the actual editorial system to confirm that relinking, audio layout, metadata, and duration behave as expected.


Review dailies are made for viewing, notes, approvals, and communication. They often use smaller compressed files, burn-ins, watermarks, or platform delivery. Editorial proxies are made for editing and later turnover. They need reliable timecode, metadata, audio mapping, naming conventions, and relink behavior. A file that works well for review may not be suitable for editorial.


A useful dailies log should record the source path, source filename, media properties, preset name and version, output filename, destination, start and finish time, processing duration, worker machine, upload status, retry count, and any error message. Preset version is especially important because it shows which files were created before and after changes to audio mapping, burn-ins, LUTs, or delivery settings.


Use a separate client review preset and destination, then restrict access at the folder, collection, or link level. Aspect supports view, download, comment, and edit controls, plus link expiry, so client dailies can be shared with granular permissions.
