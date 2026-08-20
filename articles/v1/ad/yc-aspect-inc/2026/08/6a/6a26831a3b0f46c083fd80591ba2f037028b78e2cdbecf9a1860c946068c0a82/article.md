---
schema_version: "1.0.0"
document_id: "6a26831a3b0f46c083fd80591ba2f037028b78e2cdbecf9a1860c946068c0a82"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/vfx-plate-naming-conventions-for-shot-and-frame-delivery"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T15:38:22.849241+00:00"
fetched_at: "2026-08-12T15:38:24.146114+00:00"
content_hash: "sha256:00084fc2a368b9745fddae914ff50527bcfb1f78e69c5bc0643f2038df860bf2"
---

# VFX Plate Naming Conventions for Shot and Frame Delivery

The naming decision that matters most is where the[stable shot identity](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360057627473-VFX-Shot-and-Version-Naming-Recommendations) ends and the delivery-specific information begins. Treat the shot name as the permanent ID everyone can point to, then append plate numbers, version numbers, frame numbers, and file extensions as separate fields. If you mix those jobs together, every re-pull, retime, vendor handoff, or conform update becomes harder to track.


Keep the shot identity stable, then attach delivery-specific fields as separate pieces. A good VFX plate name needs to be unique, parseable, and durable across editorial, VFX, finishing, storage, and review. The name should tell a person and a system which show, episode, sequence or scene, shot, plate, version, and frame they're looking at without needing to open the file. The rest of the context belongs in the pull list, tracking system, turnover notes, EDL/XML/AAF, or metadata.


A typical plate sequence name might look like this:


```text
AGM_104_TCC_067_0010_PL01_v001.1001.exr
AGM_104_TCC_067_0010_PL01_v001.1002.exr
AGM_104_TCC_067_0010_PL01_v001.1003.exr


```


One valid structure shows the important separation:


```text
show_episode_sequence_scene_shot_plate_version.frame.extension


```


For a feature, you may not need an episode field, while for a show that doesn't use sequence codes, scene may carry the grouping. For a series, episode is usually not optional if multiple episodes are active at the same time. Make sure the same identifier can't mean two different things in two different places.


## Build the shot name around the unit VFX tracks


Editorial often thinks in clips, source timecode, timeline events, and temp names, while VFX tracks shots. That distinction is where many naming problems start.


A VFX shot is the work unit that gets bid, assigned, reviewed, versioned, approved, delivered, and conformed. It may come from one source clip or several, and it may include handles that never appear in the edit. It may be split later, and it may be omitted from the final edit and still need to remain traceable. Because of that, the shot ID shouldn't be a source clip name and shouldn't be only an editorial event number.


A sane shot ID usually combines a few stable fields:


- show or project code
- episode code for series work
- sequence code or scene number
- shot number
- optional department or vendor-facing suffix only when agreed in advance


For example:


```text
AGM_104_TCC_067_0010


```


Here,` AGM` is the show code,` 104` is the episode,` TCC` is the sequence,` 067` is the scene, and` 0010` is the shot. Some productions would drop either sequence or scene, depending on how the show is organized:


```text
AGM_104_TCC_0010
AGM_067_0010
SHOW01_SC067_0010


```


The rule is that the shot number must be unique within its parent grouping, and the full shot name must be unique across the entire project. If two departments can generate the same full shot name independently, the convention isn't safe enough.


Teams often allocate shot numbers in[increments of 10](https://www.youtube.com/watch?v=MtFYQx3Jq5E) :


```text
0010
0020
0030


```


That spacing gives editorial and VFX room to insert shots later without renaming the whole sequence. If your team inserts a new shot between` 0010` and` 0020` , it can become` 0015` . That's much cheaper than renaming every downstream comp, review file, database record, and delivery folder after a cut change.


Avoid shot IDs like these:


```text
VFX_1
ShotA
DroneFix
Scene67Wide
FinalExplosion


```


They may be readable during the first turnover, but they don't scale. They collide easily, sort poorly, and usually require someone to remember context that the filename should have encoded.


## Keep plate names attached to the shot, not the edit event


A plate is an input to VFX for a specific shot. It might be the main background plate, a clean plate, an element, a witness plate, a split-screen side, or a re-pulled source range. The plate name should extend the shot name rather than replace it.


A common pattern is:


```text
shotName_PL##_v###.frame.ext


```


Example:


```text
AGM_104_TCC_067_0010_PL01_v001.1001.exr


```


In that structure,` PL01` identifies the plate number, and` v001` identifies the plate version. The[plate number increments](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360055781274-VFX-Plate-Naming-Best-Practices) when a shot has multiple plates, while the version increments when that plate is re-pulled or replaced.


That distinction matters. If` PL01` is the main background plate and editorial later requests a longer handle range from the same source, it should usually stay` PL01` and version up:


```text
AGM_104_TCC_067_0010_PL01_v001.1001-1080.exr
AGM_104_TCC_067_0010_PL01_v002.1001-1096.exr


```


If your team adds a new foreground element, it should become a new plate number:


```text
AGM_104_TCC_067_0010_PL02_v001.1001.exr


```


Don't use the version number to mean “new element” and don't use the plate number to mean “new pull of the same element.” That kind of ambiguity is painful because it looks organized while breaking the logic that tracking systems and humans rely on.


For simple shows,` PL01` ,` PL02` , and` PL03` may be enough. More complex shows sometimes add descriptive plate labels, but they should still preserve the sortable plate number:


```text
AGM_104_TCC_067_0010_PL01_BG_v001.1001.exr
AGM_104_TCC_067_0010_PL02_FG_v001.1001.exr
AGM_104_TCC_067_0010_PL03_CLEAN_v001.1001.exr


```


Use labels only if the receiving teams agree on the vocabulary.` BG` ,` main` ,` beauty` ,` src` , and` plate` can mean different things at different facilities.` PL01` is boring, but boring is good when files need to round-trip through multiple systems.


## Frame numbers are part of the sequence contract


For image sequences, the[frame number is how](https://movielabs.com/prodtech/sdw/vfx/ETC_ImageSequenceNaming_v1.0-063020_FINAL.pdf) systems reconstruct motion from individual files. If the frame field is inconsistent, missing, or not padded correctly, ingestion and review tools can misread the sequence or treat frames as separate stills.


Frame numbers let separate image files assemble back into a continuous moving sequence. A common file pattern is:


```text
basename.frame.extension


```


Example:


```text
AGM_104_TCC_067_0010_PL01_v001.1001.exr


```


Use a consistent frame padding length across the show. Four digits is common for frame ranges starting at` 1001` , while longer sequences or pipelines that require it may use six or more digits:


```text
1001
1002
1003


```


or:


```text
001001
001002
001003


```


Your team should make the padding decision at the show level, not per vendor and not per export. Mixed padding causes annoying failures because some tools sort lexically, not numerically. Without padding, frame` 100` can sort before frame` 99` , and frame` 10` can appear near frame` 1` .


Your team also needs to agree on the frame start. Many VFX pipelines start plates at frame` 1001` because it leaves room for handles, pre-roll, and internal offsets while avoiding frame zero and negative frame numbers. Some facilities prefer source frame numbers, timeline frame numbers, or frame` 0001` , but the safest convention is the one that every receiving system expects and that the[turnover documentation states explicitly](https://ves-on-set-data.org/data/On-Set_VFX_Data_Collection_and_Usage_Guide_v1.0.0.pdf) .


The key is that frame numbering must remain stable for a given plate version. If` PL01_v001` starts at` 1001` , don't silently re-export the same version starting at` 0001` . That creates a version collision even if the filenames are technically different, and the comp may line up in one system and slip in another.


For every turnover, your pull list or tracking record should carry the frame relationship clearly:


- editorial in and out
- source timecode in and out
- handle length
- plate frame start and end
- cut frame start and end within the delivered range
- retime or speed change notes
- source clip or camera roll reference


The filename needs to match that information.


## Match editorial names to VFX names with a crosswalk


Editorial usually creates names for speed, while VFX teams create names for production tracking. Trying to make one name serve both jobs often creates a mess.


Instead, maintain a crosswalk between editorial and VFX identity. That can live in a pull list, tracking database, turnover spreadsheet, or asset management system. The important part is that each VFX shot ID maps to the editorial context that created it.


Editorial clip context and VFX tracking identity can stay separate while linked by a maintained mapping. A useful mapping includes:


- VFX shot name
- editorial sequence name or cut version
- timeline event number
- source clip name
- source file or camera roll
- source timecode in and out
- record timecode in and out
- handles requested
- plate name delivered
- notes on retimes, stabilizations, repos, flops, resizes, or temp comps


This mapping is what lets an assistant editor say, “the temp named` Explosion_v3` in the offline is now` AGM_104_TCC_067_0010` in VFX tracking.” It also lets a vendor say, “we delivered` AGM_104_TCC_067_0010_comp_VENDOR_v008` , which corresponds to the editorial event currently sitting at 01:12:14:08.”


Don't rely on bin names, marker comments, or email threads as the only bridge because those are useful, but they aren't stable enough to be the system of record. The crosswalk should survive turnovers, vendor changes, cut changes, assistant editor changes, and archive restores.


If editorial must use readable temp names, keep them as aliases:


```text
Editorial temp: Sc67_drone_cleanup
VFX shot: AGM_104_TCC_067_0010
Plate: AGM_104_TCC_067_0010_PL01_v001


```


The alias can change, but the VFX shot ID shouldn't, unless your team formally splits, merges, or renumbers the shot by the show’s agreed process.


## Version the thing that changed


Version numbers are only useful if everyone agrees what they apply to. A plate version, comp version, review version, and submission folder version are related, but they aren't the same thing.


A plate version changes when the input plate changes:


```text
AGM_104_TCC_067_0010_PL01_v001.1001.exr
AGM_104_TCC_067_0010_PL01_v002.1001.exr


```


A comp version changes when the vendor output changes:


```text
AGM_104_TCC_067_0010_comp_ABC_v001.mov
AGM_104_TCC_067_0010_comp_ABC_v002.mov


```


A[review submission folder](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360057627253-VFX-Media-Review-Delivery-Specifications) may version up when your team sends multiple batches on the same day or when you need to resend a package:


```text
2026-08-12_VENDOR_review_01
2026-08-12_VENDOR_review_02


```


Keep those namespaces separate. If a vendor receives` PL01_v002` , their next comp shouldn't automatically become` v002` unless it's truly their second comp version. The input version and output version answer different questions.


Namespace Version applies to Increment when Do not use it to mean Example


Plate version The source plate or element delivered to VFX The same plate is re-pulled, extended, replaced, or corrected A new foreground, clean plate, or different element` AGM_104_TCC_067_0010_PL01_v002.1001.exr`


Comp version The rendered VFX output for review or delivery The vendor or internal team submits a new comp render The version of the input plate` AGM_104_TCC_067_0010_comp_ABC_v005.mov`


Review package version The transfer or submission batch A package is resent, supplemented, or split into another batch The creative version of every item inside the package` 2026-08-12_VENDOR_review_02`


Tracking status The production state of the shot The shot moves through stages such as turnover, WIP, approved, final, or omitted A file version on disk` approved` ,` finaled` ,` superseded`


This also helps finishing. If a final comp has a problem, the team needs to know whether the issue came from the delivered comp, the underlying plate, the conform, or the review package. Reusing one generic` v003` across all of those layers makes the detective work slower.


## Use delimiters and padding that machines can parse


A naming convention must be parseable by software without guessing. That means fixed delimiters, predictable field order, and padding where numbers need to sort.


Underscores are common because they survive most systems and are readable:


```text
AGM_104_TCC_067_0010_PL01_v001.1001.exr


```


Dots are usually reserved for the frame number and extension:


```text
basename.frame.ext


```


Hyphens can work, but mixing hyphens and underscores casually makes parsing harder.[Avoid spaces](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360000611467-VFX-Best-Practices) . Don't use slashes, colons, question marks, and other filesystem-sensitive characters in file names. Standardize case too. If` pl01` ,` PL01` , and` Pl01` all appear on the same show, someone will eventually deliver a duplicate that behaves differently depending on storage and operating system.


A show naming spec should define the boring details:


- delimiter between fields
- delimiter before frame number
- frame padding length
- shot number padding length
- plate number padding length
- version padding length
- allowed characters
- case convention
- whether the convention uses sequence, scene, or both
- whether vendor codes appear in output names
- whether descriptive plate labels are allowed


These rules feel small until a review system ingests 400,000 EXRs and groups them into the wrong sequences.


## Avoid rename collisions across departments


Rename collisions usually happen when each department has a locally reasonable naming system that becomes unsafe once files meet.


Locally safe names can collide when departments deliver files into the same shared destination. Common collision patterns include:


- two episodes using the same scene and shot number without an episode field
- two vendors delivering` SH010_comp_v001.mov`
- editorial replacing a temp file with a final using the same name
- image sequences from different shots[sharing the same basename](https://partnerhelp.netflixstudios.com/hc/en-us/articles/51477329357715-VFX-Media-Submission-Tool-Library-User-Guide) inside one directory
- case-only differences such as` PL01` and` pl01`


Reserve unique namespaces, and for series work, include episode. Vendor outputs should include a vendor or facility code when multiple vendors may touch the same shot, while plates, comps, mattes, references, and editorial proxies shouldn't share the exact same basename unless the file type and folder structure make the distinction unambiguous.


Don't allow silent overwrites in shared destinations. If your team resends a package, version the package or version the asset. If a shot is superseded, keep the older version available until the show’s retention policy says otherwise. Storage is cheaper than rediscovering why a vendor’s comp no longer lines up.


Folders can help, but they shouldn't carry information that the filename needs in order to remain unique. A file named` PL01_v001.1001.exr` is only safe while it stays in the perfect folder. A file named` AGM_104_TCC_067_0010_PL01_v001.1001.exr` remains identifiable after someone copies it to a desktop, transfer folder, archive bucket, or vendor ingest queue.


## Decide how split shots and merged shots get named


Cut changes are where naming conventions prove themselves. When your team extends, trims, splits, merges, or moves a shot, the team needs a rule for whether the VFX shot ID survives.


For a simple trim, keep the same shot ID and update the cut range in tracking. If the source plate needs more handles or a different range, version the plate.


For a split, create new shot IDs that preserve the relationship where possible:


```text
AGM_104_TCC_067_0010
AGM_104_TCC_067_0015


```


or:


```text
AGM_104_TCC_067_0010A
AGM_104_TCC_067_0010B


```


The first option usually sorts better and keeps the numeric spacing model intact. The second can be useful when the split is temporary or when the show wants the relationship to the original shot visible. Pick one approach early.


For a merge, avoid creating a vague new name like` combined_shot` . Either nominate one surviving shot ID and retire the other with notes, or create a new shot ID and mark both old IDs as superseded. The tracking system should show the lineage because the filename alone can't carry all of that history.


## Keep the convention small enough to follow


The best naming convention is the one people will actually use at 1 a.m. during a delivery crunch. If it has too many optional fields, aliases, abbreviations, and exceptions, it will fail through partial compliance.


Start with the required identity fields:


```text
show_episode_sequence_or_scene_shot


```


Then add the delivery-specific fields:


```text
plate_version.frame.ext
comp_vendor_version.ext
matte_version.frame.ext


```


Then document the exceptions. Don't make every possible future need part of every filename. Lens, color space, source camera, resolution, handle count, pull date, and task description are often important, but they usually belong in metadata, tracking, folder structure, or delivery notes unless your pipeline has a specific reason to parse them from names.


A plate filename should answer:


```text
Which shot is this?
Which plate for that shot is this?
Which version of that plate is this?
Which frame is this?
What file type is this?


```


If it does that consistently, the rest of the workflow has a stable base.


## A solid starting convention


For many shows, this is a workable starting point:


```text
SHOW_EP_SEQ_SHOT_PL##_v###.####.ext


```


Example:


```text
AGM_104_TCC_0010_PL01_v001.1001.exr


```


If the show uses scenes instead of sequences:


```text
SHOW_EP_SCENE_SHOT_PL##_v###.####.ext


```


Example:


```text
AGM_104_067_0010_PL01_v001.1001.exr


```


If the show needs both sequence and scene:


```text
SHOW_EP_SEQ_SCENE_SHOT_PL##_v###.####.ext


```


Example:


```text
AGM_104_TCC_067_0010_PL01_v001.1001.exr


```


For comp outputs, use the same shot identity and a separate output descriptor:


```text
AGM_104_TCC_067_0010_comp_VENDOR_v005.mov
AGM_104_TCC_067_0010_comp_VENDOR_v005.1001.exr


```


That keeps editorial, VFX, finishing, and review talking about the same shot while still separating inputs from outputs.


The practical result is fewer re-pulls, fewer overwritten plates, fewer “which version is this?” messages, and less manual detective work during conform. A naming convention is production infrastructure. Keep the shot ID stable, version the thing that changed, make frame numbers predictable, and maintain a crosswalk between editorial and VFX tracking. Downstream work will be easier when your team makes those decisions early and follows them consistently.


## FAQ


The VFX shot name is the stable identity for the work unit, such as show, episode, sequence, scene, and shot number. The plate name extends that shot identity with delivery-specific fields such as plate number, plate version, frame number, and file extension. For example, AGM_104_TCC_067_0010 is the shot, while AGM_104_TCC_067_0010_PL01_v001.1001.exr is a specific frame of a specific plate version for that shot.


Many VFX pipelines start image sequences at frame 1001 because it avoids frame zero, leaves room for handles or offsets, and is widely supported by review and compositing tools. It isn't a universal requirement. Some workflows use frame 0001, source frame numbers, or timeline frame numbers. The important rule is to choose one convention for the show, document it, and keep the frame start stable for each plate version.


Plate numbers identify different inputs for the same shot, while version numbers identify updated deliveries of the same input. If PL01 is the main background plate and it's re-pulled with longer handles, it should usually remain PL01 and version up from v001 to v002. If a new foreground element or clean plate is added, it should receive a new plate number such as PL02. Keeping those meanings separate prevents confusion in tracking, compositing, review, and conform.


Editorial clip names should be mapped to VFX shot names through a crosswalk such as a pull list, turnover spreadsheet, tracking database, or asset management record. That mapping should include the VFX shot ID, editorial sequence or cut version, event number, source clip, source timecode, record timecode, handles, delivered plate name, and notes about retimes, repos, flops, stabilizations, or temp comps. Editorial aliases can change, but the VFX shot ID should remain stable.


Avoid spaces and filesystem-sensitive characters such as slashes, colons, question marks, asterisks, quotation marks, angle brackets, and pipes. Use a consistent delimiter such as underscores between fields, and usually reserve dots for the frame number and extension, as in basename.1001.exr. Case should also be standardized because PL01, pl01, and Pl01 may behave differently across storage systems, operating systems, and pipeline tools.


The crosswalk should live somewhere more durable than marker comments or email threads, with fields for VFX shot ID, editorial event, source timecode, plate version, handles, and notes. Aspect can support this by keeping searchable custom fields alongside the media and review records as custom metadata.
