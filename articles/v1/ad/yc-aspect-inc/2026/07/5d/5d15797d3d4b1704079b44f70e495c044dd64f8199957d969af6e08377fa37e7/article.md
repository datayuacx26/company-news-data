---
schema_version: "1.0.0"
document_id: "5d15797d3d4b1704079b44f70e495c044dd64f8199957d969af6e08377fa37e7"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/camera-workflows/arriraw-vs-prores-when-to-shoot-each-format-on-set"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T19:04:16.000159+00:00"
fetched_at: "2026-07-31T19:04:17.104166+00:00"
content_hash: "sha256:4fe06d728eeb52fa77dfebb85502c221d7ccc236ccf1e2eff0129caa760ddc25"
---

# ARRIRAW vs ProRes: When to Shoot Each Format on Set

If the project can afford the data, the post schedule, and the extra handling, shoot ARRIRAW when the negative needs to stay as flexible as possible. If the job needs speed, smaller media, simpler editorial access, or a lighter dailies pipeline, shoot ProRes. That's the practical decision point. ARRIRAW and ProRes are both professional choices, and the right format is the one that protects the creative work without creating a data problem the production can't actually support.


Decision factor ARRIRAW tends to fit when ProRes tends to fit when


Storage and offload The production has enough camera media, verified backup capacity, and time for larger files The day depends on smaller files, faster copies, simpler shuttles, or limited travel storage


Grade flexibility Exposure, white balance, HDR shaping, or final look decisions may change significantly in post The look is controlled on set and the grade is expected to be moderate


VFX and plates The work involves keying, cleanup, CG integration, LED wall plates, or heavy image manipulation VFX is limited, the footage is clean, and ProRes 4444 XQ or 4444 is enough for the shot work


Editorial speed Editorial can wait for a heavier dailies or proxy process Editorial needs fast access, direct ProRes handling, or lightweight remote review files


Pipeline risk The post team has tested the exact camera, wrapper, color pipeline, software versions, and conform path The team benefits from broad NLE, finishing, transcode, and review compatibility


Archive and future use Long-term reprocessing, future HDR trims, remasters, or VFX revisions are likely The deliverables are immediate, well-defined, and do not require maximum camera-negative flexibility


A useful on-set rule is this: choose ARRIRAW for high-end finishing, heavy VFX, uncertain exposure or color decisions, demanding HDR grades, reframing, extraction, or long-tail archival value. Choose ProRes when the look is controlled, the grade is expected to be normal rather than extreme, editorial needs to move immediately, and the production benefits more from throughput than from raw sensor access.


The trap is making this decision only from image quality. Image quality matters, but the format choice also changes magazine counts, offload time, dailies render time, editorial compatibility, VFX handoff, conform complexity, cloud upload cost, and how quickly production can know whether yesterday’s work is safe.


## What ARRIRAW and ProRes actually are


ARRIRAW is ARRI’s raw camera negative. ARRI describes it as uncompressed, unencrypted sensor data that preserves the camera’s natural color response and exposure latitude. On ALEV3 cameras it's[12-bit log ARRIRAW data](https://www.arri.com/en/learn-help/learn-help-camera-system/pre-postproduction/file-formats-data-handling/arriraw-faq) , while on ALEXA 35, it's 13-bit log ARRIRAW data. Your post team still has to process it into a viewable RGB image through debayering and color processing.


That processing step is the point. With ARRIRAW, you keep more of the decisions open for post. The post team can revisit the debayer, color processing, white balance interpretation, exposure handling, and color pipeline later in a controlled finishing environment. Like film negative, it's meant to preserve the source rather than be the fastest working file.


ProRes from an ARRI camera is different because it's an in-camera processed recording format. The camera debayers and encodes the image into an Apple ProRes codec, commonly in LogC or LogC4 depending on camera generation and settings. It's still very high quality, especially in ProRes 4444 XQ and ProRes 4444, but it's no longer raw sensor data.


ARRIRAW stays closer to the sensor data, while ProRes is processed in camera into a more editing-friendly file. That distinction matters more than the wrapper. Modern ARRI cameras may record both MXF/ARRIRAW and MXF/ProRes. Older workflows may involve QuickTime-wrapped ProRes. ARRI moved newer[ProRes recording into MXF](https://www.arri.com/en/learn-help/learn-help-camera-system/pre-postproduction/file-formats-data-handling/prores) because MXF is an open standard and gives better access to metadata. But MXF/ProRes is still ProRes, not raw.


It's also worth separating ProRes from ProRes RAW.[ProRes RAW is Apple’s raw codec family](https://support.apple.com/en-us/102124) , supported in specific products and workflows. Internal ARRI ProRes recording, as commonly discussed on ALEXA workflows, is conventional ProRes, not ProRes RAW. If someone says “we’re shooting ProRes RAW on ARRI,” the team should confirm the exact camera, recorder, codec, and finishing software. That's a different workflow conversation.


## Storage is often the real deciding factor


ARRIRAW produces much larger camera originals than ProRes. The exact number depends on the camera, sensor mode, resolution, frame rate, and recording option, but the operational pattern is consistent: ARRIRAW burns through media faster, takes longer to offload, needs more shuttle storage, creates larger backups, and puts more pressure on nearline, LTO, cloud transfer, and finishing storage.


ARRIRAW usually creates a much larger media footprint than ProRes. ProRes is compressed and designed for high-performance post workflows. Apple describes the ProRes family as built for[real-time, multistream editing performance](https://www.arri.com/resource/blob/31914/9e205c6174d911712c76cf3258e0d95a/2022-04-apple-prores-white-paper-data.pdf) with reduced storage rates. ProRes is intraframe and variable bit rate, so every frame is self-contained and simpler for edit systems to decode than many long-GOP codecs. That's one reason it became so common in editorial and finishing.


For a production team, the format decision touches several practical resources:


- Camera media count and reload rhythm
- Checksum verification time
- Number and size of backup drives
- Dailies render time
- Storage required for conform and grade


Plenty of productions handle ARRIRAW every day, but your team must budget ARRIRAW as a workflow, not just select it as a camera setting. If the DIT cart, loaders, storage, network, and post facility are sized for ProRes, switching to ARRIRAW can break the day even if the camera can record it.


On ALEXA 35 and newer ARRI workflows, there are additional format options and efficiencies depending on camera generation, including ARRIRAW-related footprint reduction and newer ARRI formats like ARRICORE. ARRICORE isn't ARRIRAW and not ProRes, but it exists because productions increasingly want high image quality with a smaller footprint. If the show is on a camera that supports those choices, your team should include them in the workflow test.


## Where the grade starts to show the difference


In a normal grade with well-exposed footage, good lighting, and a clear show LUT, ProRes can look excellent. ARRI ProRes has been used at high levels for a long time because the camera’s color science and LogC encoding hold up extremely well. If the DP is exposing carefully and the finishing target is SDR or a controlled HDR pass without aggressive rescue work, ProRes 4444 XQ or ProRes 4444 can be more than enough.


The differences become more visible when the grade asks the image to stretch.


Aggressive grading can reveal where one recording format holds detail better than another. ARRIRAW gives the colorist more room in situations like these:


- Major white balance changes after the shoot
- Underexposed scenes that need shadow recovery
- Mixed lighting where color separation matters
- Heavy HDR highlight shaping
- Debayer choices for final resolution and texture


ProRes can still grade beautifully, but it's already processed into a video image. In 422 variants, you also have less chroma information than in 4444 variants. That may not matter much for straightforward editorial and finishing, but it can show up in hard keys, deep saturation, thin color edges, and heavy secondary corrections.


This is where the ProRes flavor matters. ProRes 422 Proxy is an editorial proxy format, not a camera master for serious finishing. ProRes 422 HQ is robust for many productions, especially broadcast, branded, documentary, and fast-turnaround work. ProRes 4444 and 4444 XQ are better choices when ProRes is the acquisition master and the project still expects a serious grade, compositing, or premium delivery.


ProRes can grade, but the practical question is “How much do we expect to change the image later, and how painful would it be if we can't?”


## Dailies and editorial throughput


Dailies are where the format choice becomes visible to the whole production. ARRI describes dailies as[color corrected and properly framed versions](https://www.arri.com/en/learn-help/learn-help-camera-system/pre-postproduction/editorial-workflow) of the clips shot each day, created after multiple backups of the original camera negative. They're the bridge from set to post, and they let editorial start while the show is still shooting.


With ARRIRAW, dailies creation requires debayering and color processing. That means more compute, more time, and more attention to color management. The dailies team needs the correct camera color pipeline, look files, CDL/LUT decisions, framing, audio sync, timecode, naming, and metadata handling. None of that's unusual, but it's real work.


With ProRes, the path can be faster because the camera has already produced a high-quality RGB or YCbCr image in a post-friendly codec. Depending on the editorial system and storage, editorial can use ProRes directly, or your team can transcode it into lighter editorial proxies. This is the origin of ARRI’s “direct-to-edit” history with ALEXA and ProRes. It doesn't mean every show should edit original camera files, but it does mean the source format is friendlier to fast editorial access.


A production that needs same-day edits, agency review, sports/doc turnaround, remote editorial upload, or a lightweight assistant editor pipeline will usually benefit from ProRes. ARRIRAW can still work, but the dailies system must be built for it.


For editorial, the important decisions are usually these:


- Will editorial cut camera originals, mezzanine files, or proxies?
- Will your team make dailies on set, near set, or at a lab?
- Is the upload path sized for OCN, proxies, or both?
- Will the dailies software, the NLE, or both handle sound sync?
- Is the show LUT/look metadata traveling consistently into editorial?
- Will the conform return to ARRIRAW, ProRes masters, or rendered graded media?


Those answers shape the format choice more than theory. A short commercial with a finishing house ready for ARRIRAW may be easier than a documentary traveling with a tiny crew and weak internet, even if the documentary has more total footage.


## VFX pulls, plates, and metadata


For VFX-heavy work, ARRIRAW is usually the safer acquisition choice. Green screen, blue screen, set extensions, beauty work, CG integration, high-end cleanup, and heavy plate manipulation all benefit from maximum source data. ARRIRAW gives the VFX and DI teams more control over debayering, color transforms, exposure interpretation, and noise handling.


VFX teams deliver many shots from ProRes 4444 or 4444 XQ sources successfully, especially when the work is limited and the photography is clean. But if the project is expecting hundreds of shots, complicated keys, underexposed plates, LED wall interaction, or extensive roto and comp, the extra safety of ARRIRAW is usually worth the storage.


Metadata matters here too, and ARRI notes that modern ALEXA cameras store per-frame and per-shot metadata in recorded formats such as MXF/ARRIRAW and MXF/ProRes. The camera can also capture active look files as metadata. That's helpful, but it isn't a replacement for a real VFX data package.


Recorded media carries technical metadata that matters for post, VFX, and conform. VFX teams usually need more than the codec:


- Camera reports and clip metadata
- Lens metadata when available
- Frame lines and extraction notes
- Lens grids
- HDRI and witness camera material
- Color pipeline notes
- Show LUTs, CDLs, and viewing transforms
- Plate handles and retime information
- Any in-camera texture, sharpening, or noise reduction settings


ARRIRAW protects the image, but VFX still fails when the surrounding data is messy. ProRes can work when the image is good and the metadata package is complete. The format helps, but the handoff process still carries the shot.


## Compatibility means more than opening the file


ProRes has the broadest practical compatibility. Final Cut Pro, Premiere Pro, Media Composer, Resolve, finishing systems, review systems, transcoders, and many asset platforms understand ProRes in some form. Apple’s own documentation lists support for all ProRes versions in Final Cut Pro. That broad support is one reason ProRes is still a safe choice for fast-moving productions.


ARRIRAW support is also common in professional post, but it's more version-sensitive. You need support for the exact camera generation, wrapper, resolution, color science, and metadata. ALEXA 35 introduced[LogC4 and the REVEAL color pipeline](https://www.arri.com/resource/blob/277396/5a14858a5279638c1e0756d67e94a7d7/2022-05-alexa-35-workflow-post-guide-launch-data.pdf) , so old assumptions from LogC3 workflows may not hold. If the post pipeline hasn't been updated, ARRIRAW may import but display incorrectly, lose metadata behavior, or require extra conversion steps.


The same applies to MXF. ARRI’s newer cameras use MXF for both ARRIRAW and ProRes, and ARRI states that the camera stores metadata in the file header. That's good for professional workflows, but it means older tools built around QuickTime ProRes habits may need testing. “It supports ProRes” isn't specific enough. “It supports this ARRI camera’s MXF/ProRes, this resolution, this frame rate, this LogC version, and this metadata path” is the useful version.


The practical compatibility test should include the actual route the footage will take:


- Offload and checksum tool
- Dailies software
- Sound sync path
- Editorial NLE
- Assistant editor relink workflow
- VFX pull tool
- Color grading system
- Archive and restore process
- Any remote review or cloud proxy workflow


[Run a short camera test](https://www.youtube.com/watch?v=MU6lI4zHXB4) through that full path, not just into one application. Most format problems appear at handoffs: dailies to editorial, editorial to conform, conform to grade, grade to VFX, or archive to restore.


## Delivery doesn't automatically require ARRIRAW


Delivery specs often influence acquisition, but they don't always dictate it directly. A streamer, broadcaster, studio, or distributor may specify approved cameras, minimum raster, HDR requirements, archival deliverables, or original camera format requirements. If the spec says ARRIRAW or raw acquisition, you've your decision. If it allows ProRes, then the production still has to judge whether ProRes is enough for the creative and post plan.


For theatrical finishing, HDR mastering, premium episodic, and long-term library value, ARRIRAW is attractive because the negative remains maximally reprocessable. Future remasters, alternate trims, new HDR passes, or VFX revisions benefit from having the raw source.


For corporate, documentary, live-adjacent, social, branded, internal, event, and fast-turnaround broadcast work, ProRes often makes more sense. The audience won't see the difference if the lighting, exposure, lensing, and color management are good. They might notice missed deadlines, delayed edits, dropped review cycles, or a production that ran out of storage halfway through the day.


Delivery is the final output, while acquisition is the risk-management decision that shapes how safely the production reaches it.


## Common ways the choice goes wrong


Most ARRIRAW versus ProRes problems come from choosing a format without the workflow to support it rather than from choosing the “wrong” format in theory.


The common failure patterns are predictable:


- The team selects ARRIRAW, but the production doesn't add enough recording media.
- The team estimates offload time from file copy speed instead of checksum-verified backup time.
- Editorial assumes direct playback of OCN on hardware that can't handle it.
- The team chooses ProRes 422 for a VFX-heavy job that really needed 4444 XQ or ARRIRAW.
- ALEXA 35 LogC4 material enters a LogC3-based pipeline.


ARRI’s data handling guidance is blunt on one point: avoid simple file copy methods for original camera data. Use[checksum-verified backups](https://www.arri.com/en/learn-help/learn-help-camera-system/pre-postproduction/data-transfer) . That applies whether the camera is recording ARRIRAW or ProRes. Smaller ProRes files are easier to move, but they aren't less important.


## A practical decision model for set


In prep, the team should start with the hardest part of the job, not the average shot. One greenscreen day, one night exterior, one heavy HDR scene, or one sequence with unknown final look can justify ARRIRAW even if most of the show could survive on ProRes. Conversely, one prestige scene doesn't automatically justify ARRIRAW for a six-week documentary if the storage, travel, and editorial schedule make it unmanageable.


ARRIRAW is usually the better call when the project has these conditions:


- High-end narrative, commercial, or premium episodic finishing
- Heavy VFX, keying, cleanup, or CG integration
- Aggressive HDR grade or uncertain final look
- Significant reframing, extraction, or stabilization
- Enough media, storage, backup, and dailies capacity


ProRes is usually the better call when the project has these conditions:


- Fast editorial turnaround
- Limited storage or travel footprint
- Remote collaboration with large upload constraints
- Controlled lighting and exposure
- Minimal VFX


For many jobs, the best answer is mixed. Shoot ARRIRAW for VFX plates, hero beauty, night work, LED wall plates, product macro, or scenes with major grade uncertainty. Shoot ProRes 4444 XQ, 4444, or 422 HQ for the rest, depending on the finishing target. Mixed-format workflows need clear camera reports and editorial communication, but they can save huge amounts of storage without giving up raw where it matters.


Camera, post, editorial, VFX, and production should make the format choice together. The DP may own the image intent, but the assistant editor, post supervisor, DIT, and colorist know whether the chosen format can move through the pipeline without breaking schedule or budget.


The simplest good decision is still the first one: ARRIRAW when flexibility and image headroom are worth the data. ProRes when speed, compatibility, and operational efficiency matter more. Both are professional, but the mistake is treating either one as automatic.


## FAQ


No. ARRIRAW preserves the most flexibility because it keeps the image as raw camera data, but it also increases storage, offload time, dailies processing, and post complexity. ProRes can be the better choice when the look is controlled, the grade isn't expected to be extreme, editorial needs fast access, or the production can't support a raw workflow.


For serious finishing, ProRes 4444 XQ or ProRes 4444 are usually safer choices than 422 variants because they preserve more color information. ProRes 422 HQ can still be appropriate for many broadcast, documentary, branded, and fast-turnaround jobs. ProRes 422 Proxy should generally be treated as an editorial proxy format, not as a high-quality acquisition master.


ARRIRAW must be debayered and processed into a viewable image before editorial dailies can be made. That adds compute time and requires correct color management, look files, framing, metadata handling, and audio sync. ProRes is already processed in camera into a post-friendly video file, so it can often move into dailies and editorial faster.


Yes, especially when the footage is clean, well exposed, and recorded in a high-quality ProRes format such as 4444 or 4444 XQ. However, ARRIRAW is usually preferred for heavy keying, compositing, underexposed plates, CG integration, beauty work, LED wall work, and shots that need substantial manipulation because it gives VFX and DI teams more control over debayering, color, exposure interpretation, and noise handling.


The test should follow the real path from camera to delivery: verified offload, backup, dailies, sound sync, editorial import, relink, VFX pulls, conform, grade, review, archive, and restore. It should use the exact camera model, recording format, wrapper, resolution, frame rate, color science, LUTs, and metadata workflow planned for production. A file opening in one application isn't enough proof that the whole pipeline is safe.


Mixed-format shows need consistent clip naming, camera reports, color pipeline notes, and metadata that identifies which scenes are raw, which are ProRes, and which files are editorial proxies or finishing masters. Aspect lets teams track those attributes in searchable project-level fields, so assistants and post supervisors can filter by format, LUT status, VFX status, or delivery stage using custom metadata.
