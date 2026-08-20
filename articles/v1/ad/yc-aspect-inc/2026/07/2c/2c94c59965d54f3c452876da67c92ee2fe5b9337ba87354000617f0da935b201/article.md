---
schema_version: "1.0.0"
document_id: "2c94c59965d54f3c452876da67c92ee2fe5b9337ba87354000617f0da935b201"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/how-to-verify-media-offloads-with-checksum-tools"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-30T08:11:15.735962+00:00"
fetched_at: "2026-07-30T08:11:16.568967+00:00"
content_hash: "sha256:f8c82ae0548f809b2c77564cff683d810c1bd0f0d02c35c534bf560bab58ff15"
---

# How to Verify Media Offloads with Checksum Tools

The offload rule is simple: don't erase or reuse camera media until your team has copied the media to at least two destinations and checksum-verified each destination against the source, with a log that production can read later. Verified means the bytes on the destination produce the same hash as the bytes on the original card or source drive.


That sounds obvious until the shoot is running late, the card reader is flaky, the DIT cart is packed, or a courier is waiting. A checksum workflow makes verification part of the normal offload process.


A checksum is a fingerprint a tool generates from file contents, and if one bit in the file changes, the checksum changes. During an offload, you generate checksums from the source files, copy the files to one or more destinations, then generate checksums from each destination and compare the results. If the hashes match, the copied files are byte-for-byte identical for practical production purposes. If they don't match, you stop and resolve the problem before that card goes back to camera.


Matching checksums indicate unchanged contents; a tiny file change produces a different checksum.


## Pick the checksum method before the shoot


The fastest way to create confusion is to let every loader, DIT, assistant editor, and post vendor use a different[verification setting](https://www.youtube.com/watch?v=aLLa-M6wbDI) . Decide the checksum type during[technical prep](https://partnerhelp.netflixstudios.com/hc/en-us/articles/4415931246995-Dailies-Best-Practices) and write it into the workflow memo.


For most media offloads, the common choices are:


- xxHash or[xxHash64](https://support.limecraft.com/support/solutions/articles/48001282999-setting-up-file-verification-in-limecraft-edge) : Very fast, especially useful for large camera originals, image sequences, and multi-terabyte shoot days.
- MD5: Slower than xxHash, but widely supported across post tools, archive systems, and manifest formats.
- SHA1: Also widely available and stronger than MD5 from a cryptographic standpoint, but usually slower and less common than MD5 in legacy media offload workflows.


The key detail: in post production, checksums are usually about detecting accidental corruption, not defending against a malicious attacker creating a deliberate collision. For that purpose, xxHash, MD5, and SHA1 can all be valid choices when implemented consistently. If a studio, streamer, archive vendor, or lab specifies a hash type, use that. Compatibility beats preference.


Method Best fit Relative speed Compatibility Notes


xxHash or xxHash64 Fast on-set and near-set verification of large camera originals, image sequences, and multi-terabyte shoot days Usually fastest Good when tools explicitly support it, but less universal than MD5 Choose the exact variant and keep it consistent across manifests and receiving verification


MD5 General media offloads where vendors, labs, archive systems, or older tools expect MD5 manifests Moderate Very broad Strong default for accidental corruption detection when handoff compatibility matters


SHA1 Pipelines already standardized on SHA1 or environments that require it Usually slower than xxHash, with performance varying by tool and hardware Broad, but less common than MD5 in legacy media offload workflows Use it when the workflow specifies it, not just because it is available


If you control the whole pipeline, xxHash is often the practical choice for speed. That matters when you're verifying thousands of ARRIRAW, CinemaDNG, VFX plate, or still sequence files. MD5 remains a safe default when you need maximum tool compatibility or when a receiving department expects MD5 manifests. SHA1 is useful when your tooling already standardizes on it, but don't choose it casually if downstream teams expect MD5 or ASC MHL files with a specific algorithm.


The most important decision is “which algorithm your team will generate, compare, log, and expect every handoff point to understand?”


## Copying isn't verification


A[normal file copy](https://www.arri.com/en/learn-help/learn-help-camera-system/pre-postproduction/data-transfer) confirms that the operating system believes it wrote files to a destination, but it doesn't prove that every destination file matches the original content, and it also doesn't give production a useful record when someone asks, “Was Card A023 verified before it was reformatted?”


Simple copy tools can fail in boring ways:


- A copy operation skips hidden files or metadata because someone dragged a folder incorrectly.
- A camera card is partially copied after a reader disconnects and remounts.
- A destination drive reports success, but a file differs due to storage or cable errors.
- A human copies only the visible media folder and misses sidecar metadata, audio, LUTs, or camera database files.
- Folder naming drifts between destinations, making later comparison harder.


These aren't exotic failures because they're exactly the kind of failures that appear when media teams are tired and storage is moving fast.


Professional offload tools reduce that risk by preserving folder structure, writing to multiple destinations, generating and comparing checksums, and producing reports. If you're writing your own scripts, you need to reproduce those same guarantees deliberately.


## Build the offload around the camera card, not individual clips


Treat each card, mag, shuttle drive, or recorder volume as the unit of work. The job isn't “copy these clips.” The job is “preserve this source volume exactly enough that post can reconstruct and verify it later.”


Offload the whole card structure, not just a handful of loose clips. That usually means copying the full card folder structure into a clearly named container folder, such as:


```text
PROJECT_DAY03/
A_CAM/
A023_20260730/
<entire card contents>
B_CAM/
B014_20260730/
<entire card contents>


```


Avoid flattening camera folders or pulling only media files unless the camera manufacturer, lab, and post supervisor have explicitly approved that. Many camera formats depend on sidecar files, clip databases, audio relationships, metadata folders, or naming conventions. Even when the NLE can import a raw media file directly, the full card structure is often what makes later conform, relink, color, VFX, and archive work sane.


A good offload job records a few pieces of identity around the copy:


- Project or production name
- Shoot date or media date
- Camera or recorder ID
- Card or mag ID
- Source volume name
- Destination volume names
- Checksum algorithm
- Start and finish time
- Data wrangler name
- Verification result


This metadata turns a checksum log into a production record. Without it, you may have a technically correct hash file that nobody can interpret two weeks later.


## Generate hashes from the source at the point of offload


Generate the source hash while the original camera media is still mounted and available. That's your reference. If you only hash the destination after copying, you haven't proven it matches the card. You've only made a fingerprint of whatever was copied to the drive.


Generate the source manifest while the card is mounted, then verify each destination against it. The workflow should look like this:


```text
source card -> generate source hashes
source card -> copy to destination A and destination B
destination A -> generate destination hashes and compare to source
destination B -> generate destination hashes and compare to source
write report -> only then release card for reuse


```


Some offload tools do this internally and present a single “verified” result. In scripted workflows, it's useful to make the source manifest explicit.


A simple manifest line usually contains a checksum and a relative file path:


```text
9b1f4c7a7a5f2b31  A023/PRIVATE/CLIP/C0001.MXF
31fb7a2c14d98009  A023/PRIVATE/CLIP/C0002.MXF


```


Relative paths matter because if the manifest contains absolute paths like` /Volumes/CARD_A023/...` , it becomes harder to use after the card is unmounted or after the folder moves to a server. Store paths relative to the card root or the card container folder.


## Use manifests that post can keep


A checksum result printed to a terminal is better than nothing, but it isn't enough for a production record. Save manifests and logs alongside the copied media and in a separate operations folder.


Common options include:


- Plain text hash manifests, such as MD5, SHA1, or xxHash list files.
- CSV logs with source, destination, file size, checksum, timestamp, and status columns.
- [ASC MHL manifests](https://cdn.theasc.com/ASCMHL_Specification_v1.0.pdf) when your toolchain supports them.
- PDF or HTML reports from dedicated offload applications.
- JSON logs for automated ingest systems or pipeline tools.


The takeaway is that verification needs to survive the data wrangler’s laptop. A receiving assistant editor or post supervisor should be able to open the record and understand which card was copied, where it went, which algorithm was used, and whether each destination passed.


ASC MHL is especially useful in professional handoffs because it's designed for media hash lists and can travel with the media. Tools that support MHL can verify generations of files later, detect renamed files in some workflows, and provide a standardized record between set and post. If your production already uses MHL, don't replace it with an ad hoc text file unless everyone downstream agrees.


## Verification against the source is different from destination-to-destination comparison


There are two comparisons people often confuse.


The first is source-to-destination verification, which proves that Destination A matches the original card, and Destination B matches the original card. This is the core offload requirement.


The second is destination-to-destination comparison, which proves that two copies match each other. It's useful, but by itself it doesn't prove either copy matches the card. If the original copy operation skipped a file and your team made both destinations from that incomplete copy, destination-to-destination verification can still pass.


When possible, verify every destination directly against the source before the card is cleared. If you must clone Destination B from Destination A because of time or hardware limits, make that visible in the log. Don't let the report imply both copies were independently verified against the camera card if they weren't.


## A practical shell pattern for scripted offloads


Dedicated offload software is the right choice for many productions, especially when multiple cards, multiple destinations, reports, and data wrangler handoffs are involved. Still, technical directors and post teams often need scriptable verification for labs, near-set ingest, shared storage, or automated backups.


The exact commands vary by operating system and installed tools, but the pattern is consistent:


```text
#!/usr/bin/env bash
set -euo pipefail


SRC="/Volumes/A023"
DEST1="/Volumes/RAID_01/PROJECT/DAY03/A_CAM/A023"
DEST2="/Volumes/RAID_02/PROJECT/DAY03/A_CAM/A023"
LOGDIR="/Volumes/RAID_01/PROJECT/DAY03/_OFFLOAD_LOGS/A023"
ALG="md5"


mkdir -p "$DEST1" "$DEST2" "$LOGDIR"


echo "Offload started: $(date)" | tee "$LOGDIR/offload.log"
echo "Source: $SRC" | tee -a "$LOGDIR/offload.log"
echo "Destination 1: $DEST1" | tee -a "$LOGDIR/offload.log"
echo "Destination 2: $DEST2" | tee -a "$LOGDIR/offload.log"
echo "Algorithm: $ALG" | tee -a "$LOGDIR/offload.log"


rsync -a --protect-args "$SRC"/ "$DEST1"/
rsync -a --protect-args "$SRC"/ "$DEST2"/


cd "$SRC"
find . -type f -print0 | sort -z | xargs -0 md5sum > "$LOGDIR/source.md5"


cd "$DEST1"
md5sum -c "$LOGDIR/source.md5" > "$LOGDIR/dest1_verify.txt"


cd "$DEST2"
md5sum -c "$LOGDIR/source.md5" > "$LOGDIR/dest2_verify.txt"


echo "Offload finished: $(date)" | tee -a "$LOGDIR/offload.log"
echo "Review verification files for any FAILED entries." | tee -a "$LOGDIR/offload.log"


```


This example is intentionally plain. It copies the full source folder to two destinations, generates a source manifest using relative paths, and checks each destination against that manifest.


In a real production script, you would add more guardrails:


- Refuse to run if the destination folder already exists unless the data wrangler confirms the resume behavior.
- Record file counts and total byte counts for source and destinations.
- Capture tool versions, host name, OS version, and logged-in user.
- Fail the job if any verification output contains` FAILED` .
- Save logs to both destinations, not just one.
- Write a summary file that a non-technical production person can read.


Your automation should make the verification state unambiguous.


## macOS and Windows command differences


If your team mixes platforms, test the commands before the first shoot day. Hash tools often use different names and output formats.


On Linux,` md5sum` ,` sha1sum` , and often` xxhsum` are straightforward. On macOS, built-in MD5 and SHA commands may be named differently, such as` md5` and` shasum` , and their default output may not match GNU-style` md5sum -c` verification. Many teams install GNU coreutils or a dedicated xxHash utility to keep scripts consistent.


On Windows, PowerShell can generate hashes with` Get-FileHash` , including MD5 and SHA1 depending on policy and version. For xxHash, you'll typically rely on a third-party command-line utility or an offload application.


These differences matter because verification isn't just the hash value, and it's also the manifest format. If one machine writes manifests in a format another machine can't check, you've created avoidable friction.


For cross-platform workflows, define these details:


- The hash algorithm name and variant, such as MD5, SHA1, xxHash64, or xxHash3.
- The exact tool or application used to generate the manifest.
- Whether paths are relative to the card root or destination folder.
- How spaces, special characters, and non-ASCII filenames are handled.
- Where your team stores the logs on each destination.


Once those decisions are stable, automation becomes much easier to support.


## Speed is part of safety


Verification takes time, and time pressure is where bad decisions happen. The safest workflow is the one that can keep up with camera turnover without requiring data wranglers to bypass it.


Checksum speed depends on the algorithm, CPU, storage speed, reader speed, file count, and whether the tool is reading source and destination efficiently. For large media files, storage throughput may dominate. For image sequences or folders with thousands of files, per-file overhead can become a major factor.


xxHash is attractive because it's designed for speed. On many real media jobs it can verify much faster than MD5 or SHA1, which can be the difference between keeping cards moving and building a dangerous backlog. MD5 may still be the right choice when downstream compatibility matters more than ingest speed.


Don't judge speed from one small test folder. Test with representative media:


- A full camera card from each camera type.
- The largest expected card size.
- Any image sequence formats.
- Multichannel production audio.
- The actual card readers, hubs, cables, and destination drives.
- The same number of simultaneous copies expected on set.


The goal is to learn whether your workflow can clear media safely under real conditions. If verification can't keep up, solve that with more readers, faster storage, different hashing, staged handoffs, or dedicated data wranglers. Don't solve it by skipping verification.


## Network and cloud transfers need their own proof


A common misconception is that network protocols, storage systems, or hardware checksum offload features already “handle checksums,” so file-level verification is redundant. They don't replace media verification.


Network checksum offload is about packet-level checksums handled by network hardware and drivers. It helps move packets efficiently and detect certain transmission errors at the network layer. It doesn't give your post supervisor a file-level manifest proving that` A023/C0007.MXF` on the shuttle drive matches the original card.


Cloud storage and managed transfer platforms may provide integrity checks internally. That's useful, but it's still not the same as a production-controlled source manifest unless the workflow exposes comparable file hashes and logs. If your team uploads camera originals to cloud storage, keep the same thinking: generate or preserve a manifest from the original media, verify uploaded objects against expected hashes when the platform allows it, and log the result.


For remote editorial teams, folder structure matters as much as the hash result. If files arrive verified but reorganized, assistants may lose time relinking or reconstructing camera rolls. Verification protects bytes, but it doesn't fix messy media management.


## How to handle failures


A checksum mismatch isn't a nuisance to click past. It means the destination file isn't identical to the source manifest. The correct response is to preserve evidence, isolate the failed copy, and retry from the original source if it's still available.


A checksum mismatch means stop, protect the source card, and investigate the failed copy. Typical failure modes include:


- One file reports a checksum mismatch.
- A file exists on the source but is missing on a destination.
- A destination has extra files that aren't in the source manifest.
- The source card disconnects during hashing or copy.
- Verification can't complete because a destination drive unmounts.


Don't reformat the card after a failed verification. Re-run the copy to a clean destination folder or remove and recopy the failed file only if your procedure allows partial repair and logs it clearly. If the source itself can't be read consistently, escalate immediately to the DIT, camera department, post supervisor, or data recovery plan. A failing card is a production event, not a private data wrangler problem.


Keep failed logs because they're useful. A clean final report is good, but the history of a failed cable, replaced reader, or retried copy can explain later questions and help production identify weak hardware.


## Logging should be boring and complete


The best offload logs are boring, consistent, and easy to audit.


A useful summary might read:


```text
Project: RIVER_UNIT
Shoot Day: Day 03
Card: A023
Camera: A Cam
Source Volume: A023
Destinations:
RAID_01/PROJECT/DAY03/A_CAM/A023
RAID_02/PROJECT/DAY03/A_CAM/A023
Algorithm: xxHash64
Files: 487
Bytes: 512,884,301,824
Started: 2026-07-30 18:42:11
Finished: 2026-07-30 19:16:44
Destination 1: PASS
Destination 2: PASS
Operator: J. Lee
Notes: Verified against original card before release.


```


Put that summary next to the detailed manifest and verification output. Production usually needs the summary, while post or engineering may need the detailed file list.


Store logs in predictable places. A common pattern is to put a` _TRANSFER_LOGS` ,` _OFFLOAD_REPORTS` , or` _MHL` folder at the shoot day level, then duplicate relevant reports onto each shuttle or backup destination. If the delivery drive is separated from the main storage, the verification evidence should travel with it.


## Card release needs an explicit status


The most dangerous moment in the process is when someone asks, “Can I clear this card?” so make that answer procedural.


Your team shouldn't release a card just because a copy job completed. Release it only when the offload record says the required destinations passed verification against the source. If the production requires three copies, then the card waits for three verified copies. If the workflow memo says one on-set RAID and one shuttle drive before reformat, that's the rule.


This is where naming and status language helps. Use states like:


- ` IN_PROGRESS`
- ` COPY_COMPLETE_VERIFYING`
- ` VERIFIED_OK`
- ` FAILED_HOLD_CARD`
- ` RELEASED_TO_CAMERA`


These states can live in a spreadsheet, a shared production tracker, an offload app, or a simple daily report. What matters is that nobody has to infer card status from a folder existing on a drive.


## Make the receiving side verify too


Offload verification proves that the first copies matched the card. It doesn't prove that later handoffs, courier drives, cloud downloads, or shared storage ingest preserved everything.[Receiving teams should verify](https://pomfort.com/article/how-to-establish-an-asc-mhl-workflow-with-silverstack-and-mediaverify/) against the supplied manifests when media arrives.


For an assistant editor or post facility, the receive workflow should include:


- Confirm the expected card IDs and folder names arrived.
- Verify received files against the provided hash manifests or MHL records.
- Compare card IDs against camera reports, sound reports, and dailies paperwork.
- Report missing, extra, or failed files immediately.
- Preserve the original folder structure during ingest.


It extends the chain of custody. Every major handoff is an opportunity for accidental change, so every major handoff deserves verification.


## The practical recommendation


If you're setting this up from scratch, use[dedicated offload software](https://docs.hedge.video/offshoot/features/verification) when people on set need speed, reporting, multiple destinations, and clear data wrangler controls. Configure it to generate checksums during transfer, verify every required destination against the source, and export logs or MHL records that travel with the media.


If you're scripting the workflow, keep it boring: copy the full card structure, generate a source manifest immediately, compare each destination against that manifest, fail loudly on mismatches, and write human-readable logs. Choose xxHash when speed is the main constraint and downstream tools support it. Choose MD5 when compatibility and vendor expectations matter most. Use SHA1 when your environment has already standardized on it.


The practical value is the habit: every offload produces verified copies and a record. When that habit is in place, card reuse becomes a controlled production decision instead of a leap of faith.


## FAQ


Only after the required number of destinations have been copied and checksum-verified against the original source media. For many productions, that means at least two verified copies plus a readable log showing the card ID, destinations, checksum method, verification result, time, and operator.


Use the checksum type required by the studio, post facility, archive vendor, or lab. If you control the workflow, xxHash is often best for speed, especially with large camera originals or image sequences. MD5 remains widely compatible with post tools and manifests. SHA1 can be useful if your pipeline already standardizes on it, but compatibility with receiving teams matters more than preference.


No. A normal copy only indicates that the operating system attempted to write files to the destination. It doesn't prove every destination file matches the original card byte-for-byte, and it usually doesn't create a production record. A proper offload includes checksum generation, comparison against the source, and saved logs or reports.


Destination-to-destination comparison can be useful, but it isn't a substitute for source-to-destination verification. If both destinations were made from an incomplete or corrupted copy, they may match each other while still not matching the original card. The safest workflow verifies each required destination directly against the source before the card is released.


Don't reformat or reuse the source card. Preserve the failed logs, isolate the questionable copy, and retry from the original source to a clean destination or follow the production’s approved repair procedure. If the source card has read errors or disconnects repeatedly, escalate to the DIT, camera department, post supervisor, or data recovery plan immediately.


Treat checksum reports as production metadata, not just terminal output. Store manifests with the media, then track fields like card ID, shoot day, algorithm, verification status, operator, and destination inside the asset system. Aspect supports spreadsheet-like asset views and custom fields, which makes it easier for post teams to sort, filter, and audit offload records using custom metadata.
