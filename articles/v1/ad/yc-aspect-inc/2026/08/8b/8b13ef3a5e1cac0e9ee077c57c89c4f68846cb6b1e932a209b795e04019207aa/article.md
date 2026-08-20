---
schema_version: "1.0.0"
document_id: "8b13ef3a5e1cac0e9ee077c57c89c4f68846cb6b1e932a209b795e04019207aa"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/camera-workflows/how-to-set-up-panasonic-ssd-recording-and-media-management"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T20:39:23.289636+00:00"
fetched_at: "2026-08-13T20:39:26.270289+00:00"
content_hash: "sha256:56fe5a0d9a3fb5daff329c21320461656673e4874208368709c5d07c470cf417"
---

# How to Set Up Panasonic SSD Recording and Media Management

First decide whether the camera, format, and shooting style actually belong on USB-SSD, P2, SD/CFexpress, or an external RAW recorder.


Panasonic spans a few very different recording worlds. A LUMIX GH6, GH7, G9II, or S5IIX-style setup may record directly to a USB-C SSD for high-bitrate internal codecs. A VariCam 35 or VariCam LT workflow is more likely to involve expressP2, P2, microP2, an SD proxy slot, or a dedicated RAW recorder. VariCam PURE uses[Codex Capture Drive media](https://pro-av.panasonic.net/en/cinema_camera_varicam_eva/workflows/varicam_pure/) for V-RAW, not a generic USB-C SSD. Treating all of those as “SSD recording” creates confusion on set and in post.


Different Panasonic recording media paths are separate ecosystems, not interchangeable versions of the same workflow. Panasonic workflow Typical recording media Best fit Offload and post implication


LUMIX USB-SSD, such as GH6, GH7, G9II, or S5IIX where supported USB-C SSD within the camera’s supported limits High-bitrate internal modes that the camera explicitly allows to SSD Treat the SSD as camera original media. Test the exact drive, cable, firmware, power setup, and recording mode.


VariCam internal recording expressP2, P2, microP2, plus possible SD proxy media depending on model and setup Professional master, sub-recorder, dailies, and proxy workflows Preserve the full P2 structure. Plan for Panasonic drivers, P2-aware viewing, and metadata-safe offload.


VariCam PURE V-RAW Codex Capture Drive with Codex V-RAW recorder Uncompressed 4K RAW workflows up to the supported frame rates Use Codex handling, offload, preview, and dailies tools. Do not treat the media like a generic USB SSD.


For most production teams, the rule is simple: use USB-SSD recording when the camera explicitly supports it for the format you need, when the drive is on the camera vendor’s confirmed list or your team has tested it at your exact settings, and when your data workflow can treat removable drives as camera originals. If any of those are uncertain, build the day around the camera’s native media and use SSDs for verified offload instead.


## Match the media to the Panasonic camera family


Panasonic’s current LUMIX USB-SSD workflow is meant to solve a specific problem: some codecs and frame sizes generate more data than SD cards can reasonably support, or aren't recordable to SD at all. Panasonic notes, for example, that GH7 can record[5.7K 30p ProRes RAW HQ](https://www.panasonic.com/uk/consumer/cameras-camcorders/lumix-g-compact-system-cameras-learn/article/efficient-workflow.html) in-camera, but not to SD card. On cameras that support USB-SSD recording, the SSD becomes the recording destination for those heavier modes.


The important compatibility constraints are concrete. Panasonic’s GH6 support notes say external SSD recording requires[firmware version 2.2 or later](https://help.na.panasonic.com/answers/how-to-connect-to-and-use-an-external-ssd-dc-gh6/) , uses SuperSpeed USB 10Gbps, and supports USB Type-C SSDs up to 2 TB. Panasonic also recommends using SSDs whose operation has been confirmed by Panasonic, while making clear that this isn't a guarantee for every condition or every unit.


That wording matters because “USB-C SSD” isn't a single performance class. A tiny portable drive, an NVMe blade in an enclosure, and a bus-powered rugged SSD may all mount on a computer, but behave differently on a camera powered from a small battery, hanging off a rig, recording long takes in heat.


For LUMIX USB-SSD recording, treat these as minimum expectations:


- Camera firmware supports USB-SSD recording for that body
- SSD capacity is within the camera’s supported limit, commonly up to 2 TB on documented models such as GH6
- Drive and cable support the required USB generation, such as USB 3.2 Gen 2 / 10Gbps where specified
- The specific recording mode is allowed to record to SSD
- You have formatted the drive in the camera before production use
- Your team has tested the exact drive, cable, power setup, and record mode in a long-form test


The last line is the one that saves shoots because a drive that records a 20-second test clip may still fail during a long interview, high frame rate take, or hot exterior day.


## Don't mix up USB-SSD with P2 and Codex workflows


On the professional cinema side, Panasonic workflows are built around different media assumptions. VariCam 35 records to expressP2, P2, and microP2 media depending on configuration. Its main recorder supports formats including AVC-Intra 4K, AVC-Intra 2K, AVC-Intra 100/200, and multiple ProRes options. Your team can use the sub recorder for lower-resolution or proxy-style media, depending on the setup.


VariCam 35 also supports[simultaneous recording of a master and dailies](https://pro-av.panasonic.net/en/cinema_camera_varicam_eva/workflows/varicam_35/) , plus in-camera color grading where CDL and 3D LUT information can be stored with main and sub recordings. That media management feature means editorial can receive lighter material quickly while the finishing path keeps the camera originals intact.


VariCam PURE is different again because V-RAW recording uses a Codex V-RAW 2.0 Recorder and Codex Capture Drive media. Panasonic describes this as uncompressed 4K RAW up to 120 fps, with offload, preview, and dailies handled through Codex workflow tools. If the show is shooting V-RAW, plan for Codex media handling from the start. Don't assume the DIT can pull a commodity USB SSD and copy it like a LUMIX drive.


P2 workflows also have computer-side requirements. Panasonic notes that a PC needs the included[P2 driver to recognize](https://pro-av.panasonic.net/en/support/notes/p2.html) , copy, and transfer P2 files, and P2 Viewer Plus or compatible editing software may be required for preview. That affects offload stations, editorial machines, and any “just plug it in” expectations.


## Set the camera mode before formatting and testing media


Select the recording mode before media testing. The drive that works for 4K LongG may not be appropriate for ProRes, RAW, high frame rate, or open gate recording.


On VariCam, settings such as frequency, main pixel, main codec, and high speed govern Panasonic’s system operation. Changing frequency can require a restart. VariCam LT similarly separates resolution, codec, frame rate, SDI RAW output, VFR, and proxy behavior. In other words, the camera isn't simply “in 4K.” It's in a full system mode that controls which media paths and recording options are valid.


For LUMIX USB-SSD work, the same production logic applies even if the menus look simpler. Decide these settings first:


- Container and codec, such as MOV, ProRes, ProRes RAW, or another supported mode
- Resolution and sensor area, such as UHD, DCI, open gate, or cropped modes
- Frame rate and whether the mode is normal, high frame rate, or S&Q
- Bit depth and chroma sampling where selectable
- Audio requirements
- Monitoring LUT versus baked-in look
- Whether cards, proxies, or external recorders are part of the same workflow


Some Panasonic models disable features when USB-SSD mode is enabled. G9II documentation notes that when[USB-SSD is set to on](https://eww.pavc.panasonic.co.jp/dscoi/DC-G9M2/html/DC-G9M2_DVQP3010_eng/0106.html) , recording to cards and playback of card media are unavailable, double card slot functions are unavailable, and certain USB, firmware update, and copy functions are unavailable. It also notes power requirements for some higher-frame-rate video modes, including use of an AC adaptor and DC coupler for specified conditions.


USB-SSD changes the camera’s operating state, which means camera assistants, DITs, and post supervisors need to know what becomes unavailable when SSD recording is enabled.


## Format in the camera and keep the media identity stable


Format camera-original media in the camera that will record it. That applies whether the “media” is a LUMIX USB-SSD, a P2 card, or a Codex Capture Drive. Computer formatting can create the wrong file system, allocation behavior, partition layout, or hidden file clutter, and it may mount fine on a laptop and still behave badly in-camera.


A sane formatting workflow starts with a controlled naming system. The physical label, camera reel, volume name, and offload folder should agree as closely as possible. If the camera doesn't allow custom volume names, use your own drive labels and offload folder names to preserve the identity.


A practical reel naming pattern might include:


- Camera letter, such as A, B, or C
- Shooting day, such as D01
- Media index, such as 001, 002, 003
- Optional project code for multi-show environments


For example, a physical SSD label might read` A_D01_003` . The offload folder should carry the same identity. If the camera writes a different internal volume name, record that in the camera report or media log. The goal is that nobody has to guess whether “Untitled,” “NO NAME,” or “SSD_1” is the same drive as the one in the shot log.


A stable media identity should follow the SSD from camera use through offload. Before the shoot, format each SSD in-camera, record a test clip in the final production mode, power-cycle the camera, and play the clip back from the camera. Then offload that test clip through the same ingest station that your team will use on set. This catches three categories of problems early: the camera can't sustain the mode, the computer can't read the media as expected, or the folder structure isn't what editorial expects.


## Choose recording modes around the post path


The right Panasonic recording mode is the one that preserves the creative intent while keeping the rest of the pipeline honest.


For color-managed work, V-Log/V-Gamut is usually about keeping latitude for post rather than making the on-set image look pretty. Panasonic describes V-Log as having characteristics similar to a film scan log curve, designed to preserve exposure information for grading. That means your team needs to plan monitoring separately from recording. A LUT on a monitor or proxy can help the director, DP, and editor see intent without forcing that look into the master.


VariCam workflows make this explicit. You can select V-Log, V-709, or graded views for sub-recorder, EVF, LCD, and external monitor output, depending on configuration. Panasonic’s LUT guidance also cautions that your team generally shouldn't bake a LUT into the master because it restricts adjustment in post. Applying a LUT to monitoring or proxy files is often useful. Burning a LUT into originals is a finishing decision.


For editorial speed, simultaneous master and sub/proxy recording can be more valuable than raw bandwidth. VariCam 35 can record a master and dailies at the same time. VariCam LT has related master and proxy capabilities depending on configuration. If the show needs fast dailies, make sure your team tests file names, timecode, clip metadata, and LUT handling before the first shoot day. Matching proxy and master names determines how painless conform will be later.


For LUMIX SSD shoots, where the SSD may hold the only high-quality original, decide whether editorial will cut from those originals, transcodes, or proxies. If editors are remote, don't wait until wrap to discover that a day’s worth of ProRes RAW or high-bitrate MOV files need to be transcoded before anyone can view them.


## Rig the SSD like camera media


The most common USB-SSD failures are boring: bad cable, stressed connector, unsecured drive, accidental unplug, overheated drive, or power behavior that your team never tested.


[Mount a production SSD](https://www.youtube.com/watch?v=ZDke0P34ArA) so the cable can't wiggle at the camera port. Short certified cables are usually safer than long flexible ones, but only if they don't put side-load on the USB-C connector. Cage clamps, SSD mounts, and right-angle adapters can help, but every extra adapter is another failure point. Test the final physical build, not just the drive model.


A securely mounted SSD and restrained cable reduce the risk of connector movement during recording. Build the rig around these failure modes:


- Cable bumped during handheld or gimbal work
- SSD mount blocking heat dissipation
- Camera battery swap interrupting a recording state
- Camera assistant removing a drive before recording has fully stopped
- Same-looking empty and exposed drives mixing in a pouch


The access light matters because on Codex and other professional recorders, media status indicators are part of safe handling. On USB-SSD workflows, the camera screen and drive activity indicators are your equivalent. Don't unplug because the tally light stopped. Wait until the camera has fully ended the clip and returned control to the camera operator.


## Manage multiple SSDs with a reel-based rotation


Once SSDs become camera originals, they need the same discipline as camera cards. The DIT or media manager should know which drives are blank, which are exposed, which are copying, which are verified, and which are cleared for reuse. Don't let the camera team and data team invent separate status systems.


A simple color or case-position system works well:


- Blank and formatted media stays in one marked area
- Exposed media goes into a separate protected area
- Media being copied stays physically attached to the ingest station
- The media manager seals or flags verified media after all destinations pass verification
- Media approved for reuse returns only after the media manager signs it back


Avoid erasing or reformatting on camera based only on “I copied it.” Reuse should require verified copies to the agreed number of destinations, plus a readable spot check. If the production requires camera originals to remain untouched until editorial confirms, then your team may need to treat SSDs like mags and hold them for the day or the entire shoot.


Capacity planning should account for more than total runtime. Panasonic’s 2 TB guidance on GH6-style USB-SSD workflows means you may need more physical drives than expected for all-day ProRes or RAW recording. Smaller drives can also reduce loss exposure. Losing or damaging one 2 TB drive is worse than one 1 TB drive if your team splits the day cleanly across reels.


## Preserve the folder structure during offload


Copy the entire camera-created volume structure during offload, not just the visible movie files. Panasonic P2 media is especially dependent on its file and metadata structure, and P2-aware tools or drivers may be required. LUMIX SSD folders are more familiar to most NLEs, but the same rule applies: preserve what the camera wrote.


A verified offload process should create at least two independent copies before your team reuses media. Many productions use three: primary shuttle, backup shuttle, and nearline or shared storage, and verify at copy time.


Verified offload means the camera original is copied to separate destinations and checked before reuse.[Use checksum-based copying](https://partnerhelp.netflixstudios.com/hc/en-us/articles/4415931246995-Dailies-Best-Practices) rather than drag-and-drop. The copy tool should calculate a source hash and destination hash, then report a pass or fail. After that, spot check playback from the destination, not from the original SSD still attached to the camera or reader. If you're dealing with P2, make sure the offload station has the Panasonic driver or compatible software needed to read and preview the files correctly.


The media log should capture:


- Reel or drive ID
- Camera letter
- Shoot day
- First and last clip, if practical
- Recording format
- Timecode notes or resets
- LUT or color pipeline notes
- Offload destinations
- Verification result
- Person responsible for the copy


The log needs to be unambiguous when editorial asks, “Where is A camera, day 2, card 7?”


## Handle split clips and long takes deliberately


Some Panasonic recording formats can split long recordings into multiple files or clips while the camera presents them as one continuous take. VariCam documentation notes that P2 recordings may continue automatically as another clip after certain duration thresholds, and that clips may appear as separate clips in nonlinear editing software or on a computer. AJ-PX270 documentation also includes file split options such as one file, split, and short split depending on recording format and media.


This isn't necessarily a problem, but it's a conform risk if someone copies only part of the media or renames files manually. Long interviews, concerts, lectures, and live events are where this shows up most often.


For long-form recording days, warn editorial that split files may be normal. Offload whole volumes. Avoid manual renaming inside camera folders. If your team transcodes, confirm that the transcode tool joins or interprets spanned clips correctly, and keep the originals intact until picture lock and final conform are safe.


## The setup that usually works


For a LUMIX USB-SSD shoot, the clean setup is a current firmware body, a Panasonic-confirmed or production-tested USB-C SSD within the supported capacity limit, a short reliable cable, in-camera formatting, and a record test at the exact delivery mode. Use USB-SSD when the format needs it or because the workflow benefits from direct-to-drive capture.


For VariCam, separate the master, proxy, LUT, and RAW decisions early. If you record internally, plan around P2 or expressP2 behavior and required computer drivers. If you record V-RAW, plan around Codex media and Codex offload. Use simultaneous master and dailies when it helps editorial, but test naming and conform before production.


For every Panasonic workflow, the media rule is the same: the recording format decides the media, the media decides the offload plan, and the offload plan decides when anything can be erased. If your team makes those three decisions in that order, SSD recording becomes a production advantage instead of a data management gamble.


## FAQ


No. Use USB-SSD recording only on Panasonic cameras that explicitly support it, with firmware that enables the feature, and with a drive that meets the camera’s capacity and USB speed requirements. For example, GH6 support notes specify firmware 2.2 or later, SuperSpeed USB 10Gbps, and SSDs up to 2 TB. Panasonic also recommends using SSDs whose operation has been confirmed, but that still doesn't replace testing the exact drive, cable, power setup, and recording mode before production.


Format camera-original media in the camera that will record it. Computer formatting can create file system, partition, or hidden-file issues that may not be obvious until recording. After formatting, record a test clip in the final production mode, power-cycle the camera, play the clip back in-camera, and offload it through the same ingest station that will be used on set.


No. LUMIX USB-SSD recording, P2 or expressP2 recording, and Codex-based VariCam RAW recording are different workflows. LUMIX bodies such as GH6, GH7, G9II, and S5IIX-style setups may support direct USB-SSD recording for certain high-bitrate modes. VariCam systems more commonly use P2, expressP2, microP2, proxy media, or external RAW systems. VariCam PURE V-RAW uses Codex Capture Drive media, not generic USB-C SSDs.


The selected codec, resolution, frame rate, RAW or ProRes mode, bit depth, and proxy behavior determine whether a drive and camera configuration are valid. A drive that works for a lower-bitrate 4K mode may fail or be unsupported for ProRes, RAW, high frame rate, or open gate recording. Testing should always be done at the exact settings planned for the shoot.


Treat SSDs as camera originals, not accessories. Use a clear reel or drive ID system that connects the physical label, camera report, offload folder, and media log. Keep blank, exposed, copying, verified, and reusable media physically separated. Don't erase or reformat a drive until the agreed number of checksum-verified copies has been made and the media manager has released it for reuse.


Preserve the full camera folder structure, but don't rely on folder names alone for the production record. Add fields for camera letter, shoot day, reel ID, recording format, LUT intent, copy destination, and verification status so assistants and post supervisors can find the right media later with custom metadata.
