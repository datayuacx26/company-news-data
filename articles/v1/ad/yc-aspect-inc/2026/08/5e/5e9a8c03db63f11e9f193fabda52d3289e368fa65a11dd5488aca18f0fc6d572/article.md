---
schema_version: "1.0.0"
document_id: "5e9a8c03db63f11e9f193fabda52d3289e368fa65a11dd5488aca18f0fc6d572"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/technical-solutions/how-to-automate-lto-tape-catalogs-with-open-source-tools"
published_at: "2026-08-01T00:00:00+00:00"
first_seen_at: "2026-08-01T20:45:11.974820+00:00"
fetched_at: "2026-08-01T20:45:13.262319+00:00"
content_hash: "sha256:450ba28d749b1df994d596f9f3055022874a566d5a7b053e4aab5aa1c96296c2"
---

# How to Automate LTO Tape Catalogs with Open-Source Tools

For searchable LTO without buying a full archive platform, use LTFS as the tape format,[capture the tape index](https://github.com/amiaopensource/ltfs/blob/master/ltfs/doc/README) every time you write or eject, and ingest that index into a small database that lives off-tape. That's the core pattern, and everything else is implementation detail.


The mistake is treating the tape itself as the catalog. LTFS makes an LTO cartridge look like a filesystem, which is great when the tape is mounted, but most of the time your tapes are offline, in a shelf, case, library slot, or vault. A catalog has to answer the question before the cartridge is loaded:


The physical tape can stay offline while its catalog remains searchable elsewhere. “Which tape has` A003_C014_0812G7.mov` , and where is it?”


For production and post teams, your team should create the catalog as part of the write. If the catalog depends on somebody remembering to scan a tape after the rush, it will drift. Your team should use boring automation: write, verify, export index, ingest index, search.


## The basic architecture


A practical open-source LTO catalog workflow has four layers:


- LTFS formatting and mounting for the tape filesystem
- A write tool or script that copies media and verifies it
- An exported tape index, usually XML, JSON, CSV, MHL, or a filesystem scan
- A searchable database that stores tape IDs, paths, sizes, hashes, dates, and project metadata


SQLite is enough for a single workstation or small archive station. PostgreSQL is better if multiple people need to search from the network, if you want a web UI, or if your catalog will grow across years of productions.


The key design choice is whether the database is the source of truth, or whether exported tape indexes are the source of truth and the database is a searchable copy. For most post teams, keep the exported index files as the durable record and treat the database as rebuildable. That way, if the database is corrupted or migrated badly, you can re-ingest the index files without mounting every tape.


That pattern is also closer to how mature tape systems think: inventory tells you which physical media exist, while the media catalog tells you what content is on them.


## Pick the right tool level


There are several open-source or open-ish approaches, and they fit different teams.


Tool level Good fit Catalog behavior to rely on Caveats


Standard LTFS tools Teams that want a transparent, scriptable base for formatting, mounting, copying, and ejecting tapes LTFS exposes the tape as a filesystem, then your scripts export or scan the index for ingest LTFS alone is not a searchable offline catalog, so you must build verification, index export, and database ingest


[LTOpers](https://github.com/amiaopensource/ltopers/blob/main/README.md) Post teams that want LTFS-specific helpers without buying a full archive platform Can export an XML schema on eject and update the schema when a tape is amended Put the index directory somewhere backed up and shared, not in one user profile


[ltfs-tools](https://github.com/ctsilva/ltfs-tools) Python-friendly archive stations that want hashing, MHL output, index backup, and offline catalog experiments Can combine transfer verification, manifests, LTFS index backup, and catalog functions Test carefully before production use, especially with camera originals


[TapeHoard](https://github.com/adamlamers/tapehoard/blob/master/README.md) Mixed offline archives with LTO, shuttle drives, USB disks, and S3-compatible storage Index-first search across multiple offline storage types Useful as a broad catalog, but not a replacement for a controlled LTFS write and verify workflow


Bareos, Bacula, or Proxmox Backup Server Tape libraries, autochangers, and backup-style restore operations Tracks volumes, slots, inventories, and restore catalogs Strong for backup operations, but not designed around post-production asset metadata


For the most transparent LTFS workflow, build around the standard LTFS tools. On Linux and macOS, LTFS tooling gives you formatting, mounting, unmounting, and access to the tape filesystem.[OpenLTFS and vendor LTFS packages](https://github.com/harrypm/LTO/wiki/OpenLTFS) provide the familiar` mkltfs` and` ltfs` commands. This is the right base when you want scripts you can understand and rebuild.


For media-archive-specific helpers, look at LTOpers. It includes commands for formatting, mounting, writing, schema ingestion, and searching LTO metadata. One useful behavior is that when a tape is ejected, it can export the tape’s LTFS index as an XML` .schema` file to a configured index directory. If a schema file already exists for that tape, it updates it with new data. That's exactly the behavior you want for amended tapes.


For a newer Python toolkit, ltfs-tools is interesting because it targets cross-platform LTFS archive workflows with source hashing, transfer verification, MHL output, automatic LTFS index backup, and an offline catalog system. It also describes a CatalogFS style approach, where catalogs can be mounted as a virtual filesystem. Treat this kind of project carefully in production, because some open-source LTO tools are experimental and say so plainly. Test them on non-critical tapes before trusting them with camera originals.


For a broader offline-media index, TapeHoard takes an index-first approach across offline HDDs, USB drives, S3-compatible storage, and LTO. That can be useful if your archive is mixed, which is common in real post houses. Not everything old is on LTO, and some shows are on shuttle drives, some on nearline NAS, some on cloud cold storage, and the catalog still needs to find the shot.


For an actual tape library and backup-suite behavior, Bareos, Bacula, or Proxmox Backup Server may make sense. They aren't post-production archive tools in the Canister or YoYotta sense, but they're backup systems, and they understand tape inventories, tape catalogs, slots, volumes, and restores. Bareos, for example, needs volumes in its catalog and[slot information for autochangers](https://github.com/bareos/bareos/blob/bareos-25/docs/manuals/source/TasksAndConcepts/AutochangerSupport.rst) . Proxmox separates[known tapes from media catalog](https://pbs.proxmox.com/docs-2/tape-backup.html) content and uses the catalog to locate restore data. That model is useful even if you build something smaller.


## What to capture at write time


At minimum, capture enough data to find and trust the file later. A catalog that only stores filenames will fail the first time two cameras use the same clip name.


For each file your team writes to tape, store these fields:


- Tape ID or barcode
- LTFS volume UUID, if available
- Project or production code
- Archive job ID
- Full path on tape
- Original source path
- Filename
- File size
- Modified time from source
- Write date
- Checksum, ideally from the verified source copy
- Checksum algorithm
- Archivist or workstation
- Tape generation, such as LTO-7, LTO-8, or LTO-9
- Notes for spanning, excludes, or partial writes


Those fields let you answer the normal restore questions: which cartridge, which path, whether the file is likely the exact object you expect, and which write event created it.


For media teams, add production-friendly metadata where you can. Show code, episode, shoot date, camera, card, roll, scene, vendor, and asset type are more useful than generic folder paths. Sensible file naming helps a lot here, because dates in ISO format, camera IDs such as` A_CAM` or` B_CAM` , roll IDs, and codec markers make both the filesystem and the catalog more searchable.


Don't rename camera originals to make the catalog nicer. Preserve the original folder structure and names, then add metadata beside them in the database.


## A simple open-source implementation


Your team can build a small but reliable setup with LTFS, Python, SQLite or PostgreSQL, and a scheduled or event-driven ingest script.


The write station needs a predictable directory layout:


```text
/archive_station/
incoming/
manifests/
tape_indexes/
logs/
mhl/
catalog.db


```


The tape write script should do the work in a repeatable order:


```text
mount LTFS tape
read tape identity
copy selected source folders
generate or import checksums
verify written files
export LTFS index
write MHL or manifest
ingest index into database
unmount or eject tape


```


Index export and database ingest belong inside the same workflow as the write.


Index export and database ingest belong inside the same automated write workflow. When using LTOpers, configure the LTO index directory so ejected tapes export their XML schema files into a known location. The default behavior described by the project is useful: if no path is set, it uses a documents folder, and if an index already exists for that tape, it updates the schema with new data. In a production setup, don't leave that directory buried in one user’s home folder. Put it somewhere backed up and readable by your catalog ingest process.


Example path conventions:


```text
/tape_catalog/indexes/LTO123.schema
/tape_catalog/indexes/LTO124.schema
/tape_catalog/manifests/LTO123_2026-08-01.mhl
/tape_catalog/logs/LTO123_2026-08-01_write.log


```


After every write, ingest the latest index into the database. The ingest script should upsert records, not blindly append duplicates. Your ingest script can uniquely identify a file record by a combination of tape ID, full path, size, and checksum. If you allow amended tapes, you also need to track index generation or ingest time.


A minimal database can look like this:


```text
CREATE TABLE tapes (
id INTEGER PRIMARY KEY,
tape_label TEXT UNIQUE NOT NULL,
barcode TEXT,
volume_uuid TEXT,
generation TEXT,
status TEXT,
first_seen_at TEXT,
last_indexed_at TEXT,
location TEXT
);


CREATE TABLE files (
id INTEGER PRIMARY KEY,
tape_label TEXT NOT NULL,
path TEXT NOT NULL,
filename TEXT NOT NULL,
size_bytes INTEGER,
modified_at TEXT,
checksum TEXT,
checksum_algorithm TEXT,
project_code TEXT,
archive_job_id TEXT,
indexed_at TEXT,
deleted_from_latest_index INTEGER DEFAULT 0
);


CREATE INDEX idx_files_filename ON files(filename);
CREATE INDEX idx_files_path ON files(path);
CREATE INDEX idx_files_checksum ON files(checksum);
CREATE INDEX idx_files_project ON files(project_code);
CREATE INDEX idx_files_tape ON files(tape_label);


```


SQLite full-text search can work for filenames and paths. PostgreSQL gives you better multi-user access and stronger search options. Either is fine as long as the schema is boring and exportable.


## Handling amended tapes


LTFS makes it possible to add files to a tape later, but your catalog logic has to understand that a tape index changes over time.


Amended LTFS tapes create new index generations that the catalog must track cleanly. There are two sane models.


The simpler model stores only the current state of the tape. Each time the tape is mounted, written, or ejected, export the index and replace that tape’s file list in the database. This is easiest for day-to-day retrieval, because search results show what should currently exist on the tape.


The safer model stores every index generation. Each exported index gets a generation number, timestamp, or hash, and the database marks which files appear in the latest generation, but keeps previous records for audit. This helps when a tape was mounted, amended, or repaired and you need to know what changed.


For production archives, the safer model is worth the small extra effort because it lets you answer awkward questions later, like “Was this folder on the tape when we shipped the archive?” or “Did this file disappear from the index, or was it never written?”


When your ingest script re-ingests an index, compare it with the previous generation for the same tape. The diff should identify:


- New files added to the tape
- Files still present and unchanged
- Paths that no longer appear in the latest index
- Files with the same path but different size or checksum
- Metadata changes, such as modified time or LTFS generation


Don't ignore a path disappearing from an LTFS index, because it could be a normal result of an amended index, a tooling issue, a mount problem, or archivist error. Keep the old record, mark it as not present in latest index, and flag it for review instead of deleting it from history.


## Searching across tapes


Search should match how people ask for media. Editors rarely know the full archive path. They ask for a camera roll, a scene, a date, a filename fragment, an audio day, or “the VFX plates from episode 104.”


At minimum, support these search patterns:


- Exact filename
- Partial filename
- Folder path contains
- Project or show code
- Shoot date or archive date
- Camera, card, or roll ID
- Checksum
- Tape label or barcode


A command-line search tool is enough for technical teams:


```text
lto-search --filename A003_C014
lto-search --project SHOW104 --camera A_CAM --date 2026-07-18
lto-search --checksum 9f2c...


```


The output should be retrieval-oriented, not database-oriented:


```text
Found 2 matches


Tape: LTO123
Location: Vault Shelf B4
Path: /SHOW104/OCF/2026-07-18/A_CAM/A003/A003_C014_0812G7.mov
Size: 87.4 GB
Checksum: xxh64:9f2c...
Indexed: 2026-08-01 19:42


Tape: LTO128
Location: Offsite Box 12
Path: /SHOW104/CONFORM_PULLS/A003_C014_0812G7.mov
Size: 87.4 GB
Checksum: xxh64:9f2c...
Indexed: 2026-08-04 10:15


```


That tells a coordinator or assistant editor what to pull, where it's, and whether there are multiple copies. If you've both camera originals and later pulls on LTO, search results should make the asset type clear so nobody restores a transcode when they need OCF.


A searchable catalog can point one requested media item to multiple cartridges and storage locations. A lightweight web UI can help non-technical users, but don't start there. First make the catalog data trustworthy. A pretty search page over stale or incomplete indexes is worse than a plain CLI over solid records.


## Verification belongs in the same workflow


Cataloging isn't verification, because a catalog can prove that your workflow indexed a path, but it doesn't prove your workflow wrote the file correctly unless you store and validate fixity.


The practical approach is to generate checksums before or during transfer, verify after write, and store the result with the catalog record. MD5 is still common in media workflows because many MHL and camera offload tools support it, while xxHash64 is much faster and useful for large volumes. Some teams use both: MD5 for interchange compatibility, xxHash64 for speed.


MHL output is especially useful in post because it's a familiar sidecar format for media integrity. When your archive tool can emit MHL, keep it beside the tape index and ingest the checksum values into the catalog database.


Store failed verification attempts too. If a write job fails halfway through, the catalog shouldn't pretend nothing happened. It should record an archive job with failed status, log path, tape label, and the point of failure. That prevents someone from reusing a questionable tape without context.


## Common LTFS failure modes that affect catalogs


Tape catalog automation depends on successful mounts, writes, reads, and ejects, and LTFS problems can create stale catalogs.


A few failure modes deserve special handling in scripts and logs:


- LTFS changes the tape to read-only because the drive reports too many write errors
- The tape formats successfully but fails to mount
- A tape library slot inventory doesn't match the expected barcode
- The tape was ejected before the index export completed
- The catalog database ingested an older index after a newer one


When[LTFS drops a tape to read-only](https://docs.hedge.video/canister/troubleshooting) , it's usually reporting an underlying tape or hardware issue, not just a software preference. Possible causes include bad media, a dirty or worn drive head, outdated firmware, or environmental problems such as heat. Your automation should stop the archive job, mark the tape status as suspect, and avoid ingesting a “successful” catalog update unless the write and index export completed cleanly.


Mount failures also need careful logging. LTFS error messages about failing to read labels, partition labels, or the ANSI label can point to drive, HBA, firmware, media, or device-path issues. From a catalog perspective, the rule is simple: when the tape doesn't mount cleanly, don't update the tape’s current file list. Keep the last known good index and mark the attempted mount event separately.


## Library and barcode handling


If you use a single desktop drive, you can type or scan the tape label. If you use an autoloader or tape library, you need to separate physical slot inventory from content catalog.


The system should track:


- Barcode label on the cartridge
- LTFS volume name or serial
- Library slot
- Drive serial or device path
- Tape status, such as blank, active, full, suspect, retired, or vaulted
- Physical location when outside the library


Backup tools such as Bareos and Proxmox model this distinction explicitly because they've to know which volume is in which slot before they can load media. A post-oriented catalog should do the same, even if it starts as a small SQLite database.


Don't rely only on human-readable tape names like` SHOW_ARCHIVE_01` . Use barcode-style labels with unique IDs, and store friendly project names as metadata. Productions wrap, drives get replaced, and shelves get reorganized, but unique tape IDs survive all of that.


## Make the catalog rebuildable


Your team should back up the catalog database, but you should also keep it rebuildable from files stored outside the database.


Keep these artifacts for each tape:


- The exported LTFS index
- The write manifest or MHL
- The archive job log
- The tape barcode and location record
- A copy of the ingest script version or tool version used


Store one copy on your archive station or server, one copy in normal backup, and consider writing a copy of the manifest back to the tape itself in a dedicated metadata folder. The tape should remain understandable even if the original workstation is gone.


Your rebuild process should be able to scan the` tape_indexes` folder, ingest every index, and recreate the searchable catalog. Test that process before you need it, because discovering during a restore that the only searchable catalog was a laptop database that left with a freelancer is too late.


## A practical recommendation for small post teams


For a small production company, dailies lab, or editorial department, start with this stack:


- LTFS or OpenLTFS for formatting and mounting
- LTOpers or a Python LTFS workflow for writing and index export
- MHL or checksum manifests for fixity
- SQLite for a single archive station, PostgreSQL for shared search
- A simple CLI or internal web page for search
- A backed-up folder of exported tape indexes as the durable record


Keep the first version intentionally plain, and don't start by building a complete MAM. Start by making every tape answerable offline: what is on it, when your team wrote it, whether it verified, where it lives, and how to retrieve a file without mounting random cartridges.


Once that works, add nicer metadata, user permissions, a web UI, and integration with your production tracking or asset system. The foundation is still the same: capture the catalog at write time, update it when tapes are amended, store it somewhere searchable, and keep enough verification data to trust the restore.


## FAQ


LTFS contains an index on the tape, but it isn't enough by itself for offline search. Once the cartridge is on a shelf or in a vault, you need an off-tape catalog that stores the tape ID, paths, file sizes, checksums, dates, and project metadata so you can find media before loading a cartridge.


SQLite is usually enough for one archive workstation or a small single-operator setup. PostgreSQL is a better choice when multiple users need network access, when you want a web search interface, or when the catalog will grow across many productions and years.


The catalog should ingest a new tape index after every amendment and compare it with the previous known index. A simple setup can replace the current file list for that tape, but a safer setup keeps each index generation, marks which files are present in the latest version, and flags files that disappeared or changed.


At minimum, capture the tape label or barcode, LTFS volume UUID if available, full tape path, source path, filename, file size, modified time, write date, checksum, checksum algorithm, project code, archive job ID, operator, workstation, tape generation, and any notes about partial writes or spanning. For media workflows, show, episode, shoot date, camera, roll, card, scene, vendor, and asset type are also useful.


The automation shouldn't update the tape’s current catalog as if the job succeeded. It should keep the last known good index, record the failed job with logs and status, and mark the tape as suspect if the drive reports serious errors such as read-only fallback, label read failures, or repeated write errors.


Keep the LTFS index files and MHLs in your rebuildable archive records, but mirror the human-facing restore context somewhere the team already searches. Aspect can help by attaching tape label, project, episode, roll, restore status, and other fields as custom metadata.
