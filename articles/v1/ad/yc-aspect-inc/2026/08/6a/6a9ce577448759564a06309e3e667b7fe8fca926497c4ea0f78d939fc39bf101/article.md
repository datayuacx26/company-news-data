---
schema_version: "1.0.0"
document_id: "6a9ce577448759564a06309e3e667b7fe8fca926497c4ea0f78d939fc39bf101"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/how-to-configure-imf-delivery-packages-in-davinci-resolve"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-08T02:44:10.609871+00:00"
fetched_at: "2026-08-08T02:44:11.192490+00:00"
content_hash: "sha256:fbcd4127ddf2086d0860534df9a3ca6ffa437194049db59ee2cbacc9052c677b"
---

# How to Configure IMF Delivery Packages in DaVinci Resolve

Start with the[delivery spec](https://imfug.github.io/delivery-schema/documentation/specification.html) . Resolve’s IMF preset gets you into the right export mode, but the spec decides the timeline format, audio layout, color management, codec profile, naming, languages, subtitles, Dolby Vision metadata, and whether you're making a full package or a supplemental package.


That matters because an IMF isn't “a video file.” It's a package of XML and MXF files that describes how a distributor should assemble the program. Resolve can[create native IMF packages](https://www.youtube.com/watch?v=6xxkvfTLzTA) , but it will only create a useful one if the timeline, buses, metadata, and export settings already match the destination. Netflix, a studio archive, an international distributor, and an internal mastering handoff can all ask for IMF while meaning slightly different things.


## How an IMF package is built


An IMF package, often called an IMP, is a folder-based deliverable. The video, audio, subtitles, and metadata are separate assets, and the XML files describe what those assets are and how they play together.


An IMF package is a linked set of essence files and instruction documents, not a single video file. A typical IMF package includes these pieces:


- [CPL, or Composition Playlist](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360018026794-Best-Practices-Branded-Non-Branded-IMF-Deliveries) : the playback recipe for the title, version, reel/segment structure, edit timing, and references to the essence files.
- OPL, or Output Profile List: an optional instruction set for deriving an output from a composition, often used for controlled processing or version output workflows.
- Essence files: MXF-wrapped video, audio, subtitle, or data assets.
- PKL, or Packing List: an inventory of the assets in the package, usually with hashes used during validation.
- Asset Map: a map that tells the receiving system where the assets are inside the package.


The practical takeaway is that the CPL is usually what the receiving portal or studio system asks you to select. The CPL points at the actual essence files. If the CPL references a missing MXF, the package is broken even if the video file itself looks fine in a player.


This is also why renaming or moving individual files after export can create failures. Treat the IMF folder as a package, not as loose media. If you need to rename the delivery, rename the top-level folder only if your spec allows it, and leave the internal files alone unless the receiving party has given explicit naming instructions.


## Resolve’s IMF export is a mastering workflow


DaVinci Resolve Studio supports[native IMF encoding and decoding](https://documents.blackmagicdesign.com/SupportNotes/DaVinci_Resolve_15_New_Features_Guide.pdf) through the Deliver page. In current Resolve versions, IMF appears as a format option in the Render Settings panel. Blackmagic added native IMF export for SMPTE ST 2067-style tapeless deliverables, and the[supported codec documentation](https://documents.blackmagicdesign.com/SupportNotes/DaVinci_Resolve_19_Supported_Codec_List.pdf) lists native IMF/IMP workflows with JPEG2000 and related formats.


The important word is Studio. IMF mastering relies on features such as Kakadu JPEG2000 encoding/decoding and native IMF support that are part of Resolve Studio workflows. If you're on the free version, confirm support on the exact system before building your delivery schedule around it.


Resolve’s strength here is that it can render the IMF directly from the timeline through its normal image pipeline. You don't need to make a flattened ProRes master first unless your facility workflow requires one for QC, archive, or client approval. Rendering from the approved finishing timeline reduces one generation of risk, but it also means your timeline must be clean enough to serve as the master.


## Set the project and timeline to the delivery


For IMF, set the timeline to match the required delivery format. Don't rely on a mismatched timeline and hope the Deliver page fixes it. The delivery spec should drive the timeline resolution, frame rate, color space, audio configuration, subtitle tracks, and metadata.


The delivery spec should drive the project setup before export settings are chosen. Lock these settings early:


- Timeline resolution, such as 3840 x 2160, 4096 x 2160, or HD if required.
- Timeline frame rate, matching the final master and the studio request.
- Output color space and transfer function, such as Rec.709, P3-D65, Rec.2020, PQ/ST 2084, or HLG depending on the spec.
- Data levels, normally video/legal or full depending on the mastering pipeline and receiving requirement.
- Bit depth and monitoring format, especially for HDR and Dolby Vision work.
- Audio sample rate, usually 48 kHz unless otherwise specified.
- Reel or segment boundaries if the delivery requires segmented IMF output.


A common mistake is finishing in a convenience timeline and only changing export resolution at the end. That can work for a web file, but it's a bad habit for IMF. Scaling, retiming, subtitle positioning, Dolby Vision analysis, and QC all become harder to reason about when the finishing timeline doesn't match the deliverable.


If the production used proxies or optimized media, confirm Resolve will use camera originals or approved finishing media for export. Resolve generally renders from original media unless proxy usage is enabled for output, but you don't want Resolve to source an IMF accidentally from editorial proxies. For mastering, make the source path obvious: relink to the final graded media, disable proxy render usage unless intentionally required, and render a short test section through the same settings.


## Build the audio layout around buses


IMF audio isn't just “whatever is on the timeline.” The package needs clearly defined audio essence files and channel assignments. Resolve’s Fairlight bus structure is the safest place to make that explicit.


For a simple stereo-only package, this may be straightforward. For a streaming or studio master, you may be dealing with a 5.1 printmaster, stereo Lt/Rt or Lo/Ro, M&E, dialogue/music/effects stems, descriptive audio, commentary, or immersive formats. Resolve’s IMF export can use selected main buses, including workflows that support ST 2098 and Dolby immersive audio in supported versions.


A practical bus layout might include:


- Main 1: 5.1 full mix
- Main 2: stereo full mix
- Main 3: 5.1 M&E
- Main 4: stereo M&E
- Additional buses for dialogue, music, effects, descriptive audio, or commentary as required


The exact layout depends entirely on the delivery specification. Netflix-style deliveries, studio servicing masters, and localization vendors may all expect different language tags and channel orders. If the source request says the package language is English, the IMF metadata and MXF metadata need to agree. Some portals validate language from the XML and essence metadata, not from the upload form alone.


In Resolve, route tracks to the correct buses and verify the channel order with tone, stems, or known content. Don't assume that a timeline track labeled “5.1” is enough. The channel mapping in the bus and render settings needs to match the spec.


Audio tracks must be intentionally routed into the required output buses and channel order.


## Plan subtitles, captions, and timed text as package assets


IMF can carry subtitle and closed caption tracks as part of the package, but the receiving party decides what formats and languages are acceptable. Some specs want IMSC timed text, while others may ask for sidecar caption files separately from the IMF, depending on territory and platform.


Resolve can include subtitle and caption tracks in supported IMF workflows, but the timeline track needs to be prepared as a real deliverable asset. That means no placeholder language names, no editorial-only subtitles, and no timing your team hasn't checked against the final locked picture.


The areas that tend to fail are predictable:


- Subtitle track language doesn't match the delivery request.
- Caption frame rate or timecode doesn't match the master.
- Text extends beyond safe positioning or violates platform rules.
- Forced narrative subtitles aren't separate from full subtitles when the spec requires separation.
- The IMF package includes a subtitle track that the portal doesn't expect.


The fix is to decide responsibility before export. Decide which timed text assets belong inside the IMF and which are separate deliverables. Then your team should test import the package or run validation with those assets included.


## Configuring the IMF export in Resolve


On the Deliver page, use a custom render setting or a facility-approved render preset. Resolve 18.6 and later can import and export custom render presets as XML, which is useful if multiple rooms need to produce the same kind of package. A shared preset helps, but it doesn't replace reading the current spec.


In the Render Settings panel, the core IMF choices usually include these areas:


- Format: IMF
- Codec or encoding option: typically JPEG2000/Kakadu-based for many professional IMF deliveries
- IMF application/profile: selected to match the distributor or studio requirement
- Resolution and frame rate: same as the approved timeline and delivery spec
- Composition name and package name: following the required naming convention
- Audio source: selected main buses or timeline tracks, depending on the workflow
- Subtitle/caption inclusion: enabled only for approved tracks
- Segment list: single segment or defined segments/reels if required
- Metadata fields: language, title, issuer, content kind, annotation, and other required identifiers where available


The profile choice is where you need to slow down because IMF is a standard family, not one universal preset. Application 2, Application 2E, broadcast variants, Dolby Vision requirements, and studio-specific constraints can change the acceptable encoding, color metadata, audio layout, and CPL expectations. If the spec names an application profile, use that as the authority.


For HDR and Dolby Vision IMF, project setup matters even more. Dolby Vision export depends on having the correct Dolby Vision configuration, mastering display, trims, and metadata in the project or timeline. If you're exporting from an existing Dolby Vision grading timeline, confirm that the timeline settings, color management, Dolby Vision version, and mastering display match the approved master. If you're starting from a finished Dolby Vision master file, set the project up as a mastering project rather than treating it like a normal edit import.


## Segmenting the package


Resolve supports rendering IMF projects as a single segment or as[separate segments](https://www.steakunderwater.com/VFXPedia/__man/Resolve18-6/DaVinciResolve18_Manual_files/part3968.htm) . Segmentation can be useful when the delivery spec requires reel-like boundaries, when a system has file size constraints, or when the facility wants easier replacement of sections.


Common segmentation reasons include:


- Separating program content from logos, slates, or studio assets.
- Matching acts, reels, or predefined segment boundaries.
- Avoiding oversized files on legacy storage or transfer systems.
- Preparing for future replacements without re-encoding the entire program.


Decide segmentation before final QC, not during the final render. If you put a segment boundary on a bad frame, a transition, a subtitle event, or an audio edit, you can create unnecessary downstream issues. Put boundaries where the spec requires them, or where the edit can support them cleanly.


## Full packages versus supplemental packages


A full IMF package contains the assets needed for a complete composition. A supplemental package contains only changed assets and a new CPL that references both the original package and the new material. This is one of the main reasons IMF is useful for versioning.


A supplemental package adds only changed assets and a new playlist that references the base package. Package type Contains Best used when Main dependency risk


Full IMF package A complete CPL plus all required essence and package metadata for the composition Delivering a new master, first version, archive master, or package to a recipient that does not already have the base IMP Larger transfer and render, but it is self-contained when all referenced assets are present


Supplemental IMF package A new CPL plus only the changed or added assets Replacing shots, updating audio, adding subtitles, correcting QC issues, or creating territory versions from an accepted base package It depends on the original base package remaining available, unchanged, and compatible with the supplemental workflow


New full package instead of supplemental A complete re-export of the updated version The recipient does not accept supplementals, the base package is unavailable, or tool compatibility is uncertain More data to transfer, but fewer reconstruction dependencies for the receiving system


Use a supplemental package when the receiving party already has the original IMP and only a defined part has changed. Common examples are:


- Replacing a shot after VFX revision.
- Updating a textless/title section for a new territory.
- Swapping an audio mix or language version.
- Correcting a short QC issue without re-delivering the whole feature.
- Adding or replacing subtitle assets.


Resolve can reimport an existing IMF, allow replacement of sections, and export a supplemental package. The key constraint is compatibility: Resolve supplemental package workflows rely on the[Kakadu encoder/decoder path](https://www.steakunderwater.com/VFXPedia/__man/Resolve18-6/DaVinciResolve18_Manual_files/part3969.htm) and aren't compatible with packages created using easyDCP. If you're planning supplemental delivery, keep the original package, use the compatible decoder path, and avoid mixing tools unless the facility has tested the exact combination.


For versioning, archive the full base package, every supplemental package, and the validation reports together. A supplemental package without its base package isn't a complete master because it's a set of changes that depends on the base assets still being available and unchanged.


## Validating the package in Resolve


After export, bring the IMF back into Resolve as a package, not as loose MXF files. Importing the package lets you see whether Resolve can read the CPL and assemble the composition correctly. This is a practical first pass, not the final authority.


During the review, focus on the places where IMF packages commonly break:


- The CPL opens and references all required essence files.
- Runtime and start timecode match the approved master.
- Video plays at the correct resolution, frame rate, and color appearance.
- Audio buses appear in the expected order and channel layout.
- Subtitles and captions appear only where expected.
- Segment boundaries play without glitches.
- HDR or Dolby Vision metadata is present and interpreted as expected.


Then watch or spot-check the imported package against the approved reference. If you rendered a ProRes review master from the same timeline, use it for comparison, but remember that the IMF is the deliverable. The IMF import is the thing that needs to survive.


Also inspect the package folder at the file-system level, where you should see XML package files and MXF essence files, not a random assortment of exports. Don't edit the XML by hand unless you're in a controlled mastering workflow with validation afterward because manual XML edits can fix a label and break a hash.


## Validating against Netflix or studio specs


The receiving spec is the final test. Netflix Backlot, for example, runs automated[Photon validation](https://partnerhelp.netflixstudios.com/hc/en-us/articles/115000614752-Backlot-Delivery-Instructions-for-IMF) after you upload the contents of the IMP. The upload flow checks package components such as the Asset Map, Packing List, and CPL, then asks you to choose the CPL to deliver. If validation finds errors, the portal reports those errors before the delivery proceeds.


Studio and distributor validation systems vary, but they usually care about the same categories:


- Package completeness: required XML and essence files are present.
- Hash integrity: assets match the Packing List.
- CPL validity: composition references are correct and playable.
- Essence compliance: codec, bit depth, frame rate, resolution, and wrapping match the profile.
- Color metadata: primaries, transfer function, mastering metadata, and HDR signaling match the spec.
- Audio compliance: channel order, loudness, sample rate, language tags, and track labels are correct.
- Timed text compliance: format, language, timing, and required track separation are correct.
- Naming and metadata: title IDs, version labels, issuer fields, territory, and language match the request.


The fastest way to lose time is to treat validation as a final upload task, so run a local validation pass if your facility has a tool for it, then upload to the receiving portal only after the package imports cleanly and your team has checked the obvious metadata. If the portal reports an error, fix the source project or render settings and re-export. Avoid patching package internals unless your facility has approved that mastering procedure.


## Common failure modes and how to avoid them


Most IMF delivery problems are boring, which is good news. They usually come from mismatched assumptions.


The failures that show up repeatedly are:


- Timeline frame rate differs from the delivery request.
- Audio channel order is wrong, especially for 5.1, 7.1, M&E, or immersive layouts.
- Resolve uses proxies or temporary renders as source media.
- The project is missing Dolby Vision metadata, or the metadata is mismatched or generated from the wrong timeline state.
- Your team creates supplemental packages from an incompatible base package or decoder path.


The prevention strategy is simple: make the Resolve project reflect the delivery package you intend to create. Use the timeline as the master composition, the bus structure as the audio map, and the Deliver page as the packaging step. Don't leave structural decisions to the render queue.


## A practical handoff model


For a real production, the cleanest IMF workflow starts earlier than the final day of mastering. The DIT, assistant editors, online editor, colorist, sound team, post supervisor, and delivery vendor should all know what the final package needs to contain, but they don't all need to understand CPL XML. Your team should preserve the information the IMF needs later through the offline, conform, grade, audio mix, and timed text workflows.


For the final handoff, keep these items together with the package:


- The exported IMF folder.
- The Resolve project export you used to create it.
- Any facility render preset your team used for the delivery.
- The approved delivery specification.
- QC reports, validation reports, and portal error reports if any.
- Notes on full package versus supplemental package relationship.
- Checksums for transferred folders if your facility requires them.


That gives the next person enough context to reproduce the package, diagnose a rejection, or build a supplemental. IMF is designed for controlled versioning, but only if your team preserves the base package, metadata, and delivery notes.


Resolve Studio can make an IMF with the right supported formats, but the better question is whether you have configured your Resolve project as a valid master for the specific receiving system. If the timeline, audio buses, subtitles, metadata, codec profile, and validation target all agree, the IMF export is straightforward. If they don't, the render may complete perfectly and still fail delivery.


## FAQ


IMF mastering is a DaVinci Resolve Studio workflow. Native IMF export, Kakadu JPEG2000 support, and related mastering features are associated with Resolve Studio, so you should confirm support on the exact system before scheduling an IMF delivery.


An IMF package is the full folder-based deliverable that contains XML files and essence files. The CPL, or Composition Playlist, is one XML file inside the package that describes how the program should play and which video, audio, subtitle, or data assets are referenced.


Use a supplemental IMF when the recipient already has the original package and only specific assets have changed, such as a revised shot, updated audio mix, corrected subtitle file, or territory-specific title section. The supplemental package depends on the original base package, so both must be preserved.


An IMF can fail because validation checks more than picture playback. Common issues include missing referenced assets, hash mismatches, invalid CPL references, wrong audio channel order, incorrect language metadata, unsupported codec settings, missing HDR metadata, or subtitles that don't match the delivery spec.


You shouldn't rename, move, or edit internal IMF files after export unless the receiving party or a controlled mastering workflow specifically requires it. The XML files reference package assets by identifiers and paths, and the Packing List may include hashes, so manual changes can break validation.


Keep the full package, every supplemental, the Resolve project export, specs, QC notes, and validation reports in one controlled project location. Aspect gives the delivery team a shared cloud filespace so post supervisors, editors, and vendors aren't reconciling duplicate folders or wondering which package is current.
