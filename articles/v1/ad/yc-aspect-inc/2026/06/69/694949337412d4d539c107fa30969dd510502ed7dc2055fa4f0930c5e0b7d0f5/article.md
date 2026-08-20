---
schema_version: "1.0.0"
document_id: "694949337412d4d539c107fa30969dd510502ed7dc2055fa4f0930c5e0b7d0f5"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/codec-workflows/managing-dnx-workflows-in-non-avid-environments-resolve-premiere"
published_at: "2026-06-27T00:00:00+00:00"
first_seen_at: "2026-07-24T08:04:34.272722+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:f57cf6d7dd587713e44810a904383d08d8de29642f5a91c048ce709bd3f9f8f8"
---

# How to Use DNxHR and DNxHD in Resolve and Premiere

## Choose the wrapper for the next owner


[DNxHD and DNxHR](https://kb.avid.com/pkb/sfc/servlet.shepherd/document/download/069VP00000a9H6ZYAU?operationContext=S1) are sometimes described as “Avid codecs,” but in a non-Avid workflow the wrapper can matter as much as the codec family. Premiere Pro and DaVinci Resolve can both work with DNx media, but a DNxHR HQX file inside a` .mov` is a different handoff from DNxHR HQX inside MXF OP1a, and both are different from[MXF OP-Atom media](https://academysoftwarefoundation.github.io/EncodingGuidelines/EncodeDNXHD.html) meant to live inside an Avid MediaFiles folder.


Choose the wrapper based on the next system that has to own the file:


- MOV works well when the files will be opened mainly in Premiere, Resolve, After Effects, or general desktop review workflows.
- MXF OP1a is useful for a self-contained professional interchange file, especially for delivery, broadcast-style exchange, or mixed NLE environments.
- MXF OP-Atom is appropriate when the file is intended to become Avid-native media inside Media Composer.
- OP-Atom should not be used just because “Avid equals MXF.” It is appropriate when Media Composer is the target, but awkward when Premiere or Resolve are the main editing systems.


That decision prevents relink, import, and “why is this file split into pieces?” problems later.


Wrapper choice can decide whether a DNx handoff arrives as one tidy file or as separated pieces.


## DNxHD vs DNxHR is a raster decision


DNxHD is the older HD family, designed around[fixed HD frame sizes](https://kb.avid.com/pkb/articles/en_US/Knowledge/Avid-DNx-naming-scheme-and-data-rates) , frame rates, and data rates. If you are working 720p or 1080p/1080i, DNxHD can be appropriate when the timeline and deliverable are HD.


DNxHR is the high-resolution family. Use it for UHD, 4K, larger-than-HD finishing, and mezzanine workflows where you do not want the codec choice tied to classic HD broadcast rasters.


The practical split looks like this:


- DNxHD 36 is a classic offline editorial choice for 1080p Avid-style proxy workflows.
- DNxHD 145/175/220 and their 10-bit variants are used for HD finishing or high-quality editorial workflows, depending on frame rate.
- DNxHR LB is the lightweight proxy/offline flavor.
- DNxHR SQ is an editorial mezzanine option when storage matters.
- DNxHR HQ is a higher-quality 8-bit finishing or interchange option.
- DNxHR HQX is the standard 10-bit choice for color and finishing.
- DNxHR 444 is for RGB/444 workflows where the rest of the pipeline supports and needs it.


If the project is UHD or 4K, stop trying to make DNxHD fit. Use DNxHR and choose the flavor based on whether you are cutting, grading, VFX-pulling, or delivering.


DNxHD is for HD rasters; DNxHR is the better fit when the project frame grows to UHD or 4K.


## How Media Composer, Resolve, and Premiere treat DNx


Media Composer has the most Avid-specific DNx integration because DNx is part of Avid’s native media model. Avid-managed MXF OP-Atom media, media databases, bin metadata, AAF exchange, and classic DNxHD offline workflows are what Media Composer was built around, so if the show is Avid editorial from day one, DNx in OP-Atom is the native path.


Resolve can read and write DNx media in transcode, conform, color, and finishing workflows. Current Resolve supported-format documentation lists DNxHD and DNxHR support across MOV, MXF OP1a, and MXF OP-Atom for common variants. Resolve also gives you render modes for round-tripping, including single-file exports for masters and individual-clip exports for relink or AAF workflows. It does not remove the need for workflow discipline, which means reel name, source timecode, levels, handles, and filename uniqueness need to be set deliberately.


Premiere Pro can read and write DNx in practical editorial workflows, and it can cut DNxHR or DNxHD media. Compared with Media Composer, differences tend to show up in media management rather than basic playback. Premiere does not treat Avid-style OP-Atom media as its native organizational model, and its relink/proxy behavior depends more heavily on file path, clip metadata, audio channel layout, and project state. It is flexible, but that flexibility means assistants need to impose the rules that Avid would normally enforce.


A simple way to think about the three apps:


- Media Composer fits workflows where DNx is the editorial system’s native managed media.
- Resolve fits workflows where DNx is the transcode, conform, color, and render format.
- Premiere fits workflows where DNx is a mezzanine or proxy format inside a broader Adobe workflow.


None of these tools is “wrong” for DNx. The trouble starts when a team uses Avid assumptions in Premiere, or Premiere assumptions in an Avid handoff.


## Native DNx setup in DaVinci Resolve


Resolve can be a practical place to create DNx media for a mixed workflow. It can read camera originals, generate DNx proxies or mezzanines, preserve source timecode, and render AAF-friendly media when Avid is involved.


For a Resolve-based DNx transcode, set the project up to match the real editorial or finishing spec before you render anything. Frame rate is the critical setting because if the Resolve project frame rate is wrong when media is imported, you can create problems that are annoying or impossible to fully unwind later.


In Resolve, the key setup choices are:


- Project frame rate and timeline frame rate before importing production media.
- Timeline resolution separately from render resolution.
- Reel name behavior in project settings or clip attributes, matched to the turnover plan.
- Source timecode preservation unless there is a documented reason to generate new timecode.
- Consistent clip naming, especially when rendering individual files.
- Individual source clips versus a single flattened timeline.
- MOV, MXF OP1a, or MXF OP-Atom based on the next application.
- DNxHD only for HD rasters and DNxHR for UHD/4K and higher.
- DNxHR HQX when the color pipeline needs 10-bit media.
- Data levels or video levels tested on a short representative render before committing a full batch.


Once those choices are locked, render a small sample and bring it into the actual target app. Do not validate a Resolve-to-Avid render only inside Resolve, and do not validate a Resolve-to-Premiere render only by opening it in a player.


For Avid-bound media, Resolve’s[Avid AAF-style render presets](https://kb.avid.com/pkb/articles/en_US/How_To/Exchanging-Sequences-with-DaVinci-Resolve) are useful because they can create MXF OP-Atom media that Media Composer understands as native-style media. The usual pattern is to render individual clips with handles, then bring the AAF and media into Media Composer. If the MXF files are going into an Avid MediaFiles folder, keep the folder structure clean and let Media Composer index the media databases, because random OP-Atom files scattered in a delivery folder are not the same thing as managed Avid media.


For Premiere-bound media, MOV or MXF OP1a is easier to manage than OP-Atom because the files are self-contained and can live with the rest of the project media. If the workflow is Resolve color back to Premiere, a single flattened DNxHR HQX export may be enough. If the workflow requires reconnecting graded shots to an edited sequence, individual clips with handles and stable names reduce relink risk.


## Native DNx setup in Premiere Pro


Premiere can use DNx in two main ways: as edit media/proxies, or as an export/interchange format. Those are different decisions.


For proxy workflows, Premiere’s proxy system expects proxy files to match the source clips in ways that matter for relinking. Frame rate, duration, timecode, and audio channel layout can all become pain points, which means a proxy that “looks right” in Finder or Explorer may still attach badly if the metadata does not line up.


When setting up DNx proxies in Premiere, use a preset that matches the editorial intent:


- DNxHD 36 or a similarly lightweight DNxHD flavor for HD offline editing when the project is truly HD.
- DNxHR LB or SQ for UHD/4K offline editing rather than forcing DNxHD.
- Proxy frame rate identical to source frame rate.
- Audio channel configuration consistent with the source when possible.
- Source start timecode preserved during proxy creation.
- A predictable proxy folder structure rather than proxies mixed beside camera originals with unclear suffixes.
- The proxy toggle in the Program Monitor so editors can see whether they are viewing proxies or full-res media.


After attaching proxies, spot-check a few clips at the head, middle, and tail of the source. Drift, wrong audio mapping, duplicate clip confusion, and accidental frame-rate interpretation changes are common signs that the proxy setup is not safe.


For DNx exports from Premiere, you usually choose between QuickTime/MOV DNx and MXF OP1a DNx. Use MOV for Adobe-to-Resolve, Adobe-to-VFX, or desktop interchange when the recipient asked for QuickTime files. Use MXF OP1a when the recipient expects MXF deliverables or wants a self-contained file with a broadcast-style wrapper.


In Premiere’s export settings, the practical choices are:


- QuickTime with an Avid DNxHD/DNxHR codec when the receiving app is Resolve, Adobe apps, or a post vendor that asked for MOV.
- MXF OP1a with DNxHD/DNxHR when the recipient expects MXF delivery or interchange.
- Frame size, frame rate, field order, and pixel aspect ratio matched to the sequence or delivery spec.
- DNxHR HQX for 10-bit finishing exports when the timeline and source quality justify it.
- Preview files excluded from final output unless the preview codec, raster, bit depth, and quality are intentionally set for that purpose.
- Full-resolution media reconnected before color-heavy exports, compositing pulls, or final finishing outputs.


For high-quality post tasks,[reconnect full-resolution media](https://helpx.adobe.com/in/premiere/desktop/organize-media/ingest-proxy-workflow/reconnect-full-resolution-media-to-proxies.html) rather than assuming proxies or offline placeholders will be bypassed correctly. This reduces the risk of a final export using the wrong media state.


## Picking the right DNx flavor


The right DNx flavor depends on what the file needs to support, because editing, review, color, VFX, and delivery do not need the same bitrate.


Common choices include:


- DNxHR LB or DNxHD 36 for offline editorial where performance and storage matter more than image quality.
- DNxHR SQ for higher-quality editorial media when you still need manageable file sizes.
- DNxHR HQ for high-quality 8-bit mezzanine workflows.
- DNxHR HQX for 10-bit camera sources, color finishing, and Resolve-to-Premiere or Resolve-to-Avid graded handoffs.
- DNxHR 444 only when the pipeline is actually RGB/444-aware and the receiving app, storage, and deliverable spec support it.


Avoid using a higher flavor just because it sounds safer. DNxHR 444 or HQX can increase storage and bandwidth requirements without improving the actual delivery. If nobody can explain why the job needs 444, it probably does not, while HQX is often the more practical high-quality finishing choice.


## MXF OP1a vs OP-Atom vs MOV


DNx workflows get messy when “MXF” is treated as a single thing.


MXF OP1a is a single self-contained file where video, audio, and metadata are wrapped together. It behaves like a normal file for handoff, import, copy, archive, and delivery.


MXF OP-Atom separates media essence into separate files, commonly used by Avid. In an Avid-native workflow, that is a feature. In a Premiere-first or Resolve-first workflow, it can feel like someone mailed you the contents of a machine room instead of a video file.


MOV is a QuickTime wrapper. DNx in MOV is used for interchange between creative apps, especially when a team wants files that behave like ProRes-style media but with DNx compression.


The destination usually determines the wrapper:


- Media Composer as native media: MXF OP-Atom, usually through an Avid-aware preset or AAF workflow.
- Media Composer as a file import/link: OP1a or OP-Atom, depending on what the Avid team requests.
- Resolve: MOV or MXF OP1a are usually straightforward.
- Premiere: MOV or MXF OP1a are usually safer than OP-Atom.
- Broadcast, QC, or delivery: the wrapper named in the spec, often MXF OP1a.
- VFX: the vendor’s requested format. Some vendors prefer image sequences, ProRes, or EXR, but DNxHR HQX MOV may be acceptable for editorial-quality pulls.


When the destination is unclear, OP1a is usually a better neutral MXF choice than OP-Atom. OP-Atom is excellent when it is intentional and annoying when it is accidental.


## The metadata that keeps relinks alive


DNx files still depend on metadata for a reliable conform.


Reliable DNx relinks depend on matching metadata, not just matching the codec. For round-trips between Premiere, Resolve, and Media Composer, the core identifiers are[source timecode, clip name](https://help.frameone.com/support/solutions/articles/69000800937-the-resolve-to-avid-round-trip) , reel/tape name, file name, and sometimes source file path. Different apps prioritize these differently. Media Composer has a long history with tape/reel-style metadata, while Resolve can derive reel names from embedded metadata or file paths. Premiere is often more path/name/project-item oriented unless you carefully manage metadata during turnover.


Relink problems include small metadata mistakes such as:


- Camera originals do not have useful reel names, and Resolve generates something inconsistent.
- Two clips from different cards share the same filename.
- Proxies were made with new start timecode instead of source timecode.
- Audio-only or merged clips do not reconnect the same way as simple video clips.
- Multicam clips hide the source metadata needed for a clean turnover.
- AAF or XML export references the wrong media because duplicate names exist in the project.
- Rendered graded clips come back with new names that no longer match the edit decision list.


The fix is direct: decide which metadata fields are authoritative at the start of the workflow, then test that those fields survive one full round-trip.


For Resolve-to-Avid workflows, reel name deserves special attention. If Media Composer expects tape/reel metadata and Resolve is not assigning it the way the Avid side expects, relink can fail even though the timecode looks right. For Premiere-to-Resolve workflows, duplicate file names are a classic trap, especially with camera cards that reset numbering. In that case, folder-derived reel names or unique transcode names can save the conform.


## Prove the path with a small round-trip


A useful DNx workflow test is a miniature version of the actual job. Take a few representative shots, run them through the planned path, and bring them back into the destination system before batching the full project.


Use material that reflects the real trouble spots: mixed frame rates, multicam, a speed change, a graphic, a clip with obvious audio channel mapping, a shot with deep shadows, and a shot with bright highlights. The goal is not just to see whether the file opens. It is to expose the kinds of problems that usually appear late, including wrong wrapper expectations, mismatched frame rate or raster, broken source timecode, weak reel/tape metadata, incorrect audio mapping, level shifts, proxy confusion, or files that only work on the sender’s newer software version.


If that small round-trip works in the receiving app, scale the workflow. If it does not, change the rule and test again. DNx is reliable when the wrapper, flavor, metadata, and destination app are treated as one workflow decision instead of four separate guesses.


## Performance expectations in non-Avid systems


DNx is an intraframe codec, which means each frame is compressed mostly independently. An NLE can move to a frame without decoding a long group of surrounding frames, which can reduce decode load compared with long-GOP acquisition codecs like H.264, H.265, XAVC variants, and some mirrorless camera formats. On a busy timeline, that structure can make scrubbing, trimming, and rendering more predictable.


That does not mean all DNx is light. DNxHR HQX and 444 at UHD or 4K can be very large, so storage bandwidth can become the bottleneck before CPU decode does, especially on shared storage, portable drives, or remote workflows.


Performance usually comes down to three areas:


- CPU/GPU decode load: DNx is usually easier than heavily compressed camera codecs.
- Storage bandwidth: high-quality DNxHR can demand fast disks and networks.
- Timeline complexity: effects, scaling, noise reduction, multicam, and color management can erase the performance gains of a good codec.


If editors complain that DNxHR HQX is slow, do not assume the codec is the decode problem. Check storage throughput and timeline effects first. A lightweight DNxHR LB proxy workflow may be the better editorial choice, with HQX reserved for finishing renders.


## Color levels and gamma surprises


DNx handoffs can expose level interpretation differences between apps. This often shows up as “washed out,” “crushed,” or “gamma shifted” exports after moving between Resolve and Premiere.


The hard part is that there is no universal one-click rule that survives every version, wrapper, platform, and display pipeline. Resolve, Premiere, QuickTime playback, operating system color management, and monitoring hardware can all influence what people think they are seeing.


The safe approach is to make[levels part of the workflow](https://mixinglight.com/color-grading-tutorials/revisiting-data-video-levels-resolve-export-to-premiere-import/) test before they turn into a last-minute debate.


Testing levels with scopes early helps prevent last-minute gamma and range surprises. For a Resolve-to-Premiere or Resolve-to-client DNx render, use a short section that includes:


- Normal skin tone exposure.
- Deep shadows near legal black.
- Bright highlights near legal white.
- Graphics or titles with known RGB values.
- A shot with a LUT or color transform applied.
- A frame that can be compared against scopes, not just a computer display.


Bring that test into the destination app and compare scopes against the source timeline. If the scopes match but a desktop player looks different, you probably have a player/display interpretation issue. If the scopes do not match, you have a real levels or transform problem to solve before rendering the full show.


In Resolve, pay attention to the render levels setting and whether the pipeline expects video/legal or full/data levels. In some Avid-bound or broadcast-style workflows, retaining sub-black and super-white values may matter. In web-first workflows, the expectations may be different, so decide deliberately and validate with scopes.


## Alpha, 444, and older-version traps


DNx is not just one capability level across every app version. Support for DNxHR 444, 12-bit variants, alpha handling, and certain[QuickTime DNx behaviors](https://community.adobe.com/questions-729/announcement-changes-to-quicktime-dnxhd-support-in-premiere-pro-22-3-1390056) has changed over time in major NLEs.


This matters when a facility is not on the same software version as the vendor sending files. A DNxHR 444 file created in a current Adobe or Resolve version may not behave correctly in an older application. A DNxHD QuickTime file from an older pipeline may not be encoded the same way a newer system expects. Alpha support can also vary depending on codec flavor, wrapper, and application.


Be conservative when compatibility matters more than theoretical quality:


- Use DNxHR HQX instead of 444 unless 444 is required.
- Avoid alpha in DNx unless the receiver has tested that exact wrapper and flavor.
- For graphics with transparency, consider whether ProRes 4444, image sequences, or another agreed format is safer.
- Ask vendors for application versions, not just app names.
- Test on the oldest system that needs to open the files.


If a workflow spans multiple companies, passing the test on the sender’s machine is insufficient. The recipient’s machine is the one that counts.


## Avid handoff from Resolve or Premiere


When Media Composer will receive the media, decide whether you are delivering linked media, native Avid media, or a sequence turnover. Those are different deliverables.


If the Avid team wants native media, Resolve is often the better tool for creating OP-Atom DNx media and an AAF. It can render individual clips, preserve timecode, include handles, and generate media that fits the Avid ingest model. The Avid side can then place media in the correct Avid MediaFiles structure and let Media Composer index it.


If the Avid team wants a simple file to link or import, MXF OP1a DNx may be easier. That is especially true for a flattened graded master, a textless element, a reference export, or a VAM-style deliverable.


Premiere can export AAFs, but effects translation, speed changes, nests, multicams, merged clips, and audio routing can complicate the handoff. If the online or color finish is in Resolve, a Premiere XML to Resolve is often cleaner for picture conform than trying to make Premiere behave like Avid.


For Avid-bound handoffs, the deliverable conversation should include:


- Whether the Avid editor expects OP-Atom media, OP1a files, or linked MOV files.
- Whether the sequence should be sent as AAF, XML via Resolve, EDL, or a flattened file.
- Required handles for graded or transcoded clips.
- Whether source timecode and reel/tape names are already correct.
- Whether audio is included, omitted, or handled in a separate turnover.
- Whether the Avid project is HD, UHD, progressive, interlaced, or thin raster.


Once those answers are known, the DNx settings become much less mysterious. The wrapper follows the handoff type.


## Premiere to Resolve with DNx in the middle


A common non-Avid workflow is offline in Premiere, color in Resolve, then either finish in Resolve or return a graded file to Premiere. DNx can be used in that workflow as proxies, intermediate renders, graded clips, or a final master.


For a clean Premiere-to-Resolve turnover, solve the sequence conform before focusing on DNx. Simplify the timeline, remove or bake unsupported effects as needed, export a reference movie, and send XML/AAF plus media. Resolve can then relink to camera originals, proxies, or mezzanine DNx media depending on the job.


DNx becomes useful in a few specific Premiere-to-Resolve cases:


- The camera originals are too slow or inconsistent for editorial, so the team cuts DNx proxies or mezzanines.
- The colorist wants a flattened DNxHR HQX preconform instead of rebuilding a complex timeline.
- VFX or graphics vendors need editorial-quality pulls that are easier than camera RAW.
- The final return to Premiere is a single graded DNxHR HQX MOV or MXF OP1a file.
- The project needs individual graded clips with handles for reconnection in Premiere.


If the plan is to return individual graded clips to Premiere, test that Premiere reconnects those files correctly before the grade is complete. Speed changes, nests, multicams, and merged audio can make this harder than it sounds. If the plan is to return a flattened master, DNxHR HQX is less dependent on relinking clip metadata, but you lose editability.


## Resolve to Premiere with rendered DNx


When Resolve is sending graded media back to Premiere, choose between a flattened render and a reconformable render.


A flattened render is one self-contained file of the final timeline, which makes it the least fragile option. Use it when the picture is locked, titles and graphics are either included or intentionally excluded, and Premiere only needs to add final audio, captions, or versioning.


Individual rendered clips are more flexible but more fragile. Use them when Premiere needs to keep the sequence editable, swap shots, version graphics, or maintain layered finishing. In that case, render with handles, preserve source timecode where possible, and make sure filenames are unique.


For Resolve returns to Premiere, the key delivery choices are:


- Single clip or individual source clips.
- MOV or MXF OP1a.
- DNxHR HQX for 10-bit graded returns.
- Render resolution matching the finishing timeline.
- Source timecode preservation for individual clips.
- Handle length long enough for trims and transitions.
- Data/video levels tested in Premiere.
- Audio included only if Premiere is meant to use that audio.


After import into Premiere, compare the returned DNx against the Resolve reference using both picture and scopes. Do not judge only from the Program Monitor, especially on unmanaged displays.


## When DNx is the wrong choice


DNx is a useful mezzanine and editorial codec, but it is not automatically the best choice for every non-Avid workflow.


You may want something else when:


- The whole facility is ProRes-based and nobody downstream wants DNx.
- The delivery spec explicitly requires ProRes, XDCAM, AVC-Intra, JPEG 2000, IMF, or another format.
- VFX needs EXR, DPX, or image sequences.
- The project requires alpha and the recipient has not tested DNx alpha support.
- Storage is tight and DNxHR HQX would create more bandwidth pain than value.
- The online finish is camera RAW-based and DNx would only be an unnecessary intermediate.


A codec workflow should follow the job requirements over brand preference. If DNx makes the handoff more stable or easier to relink, use it. If it adds translation steps for no reason, do not.


Wrapper Use when Main advantage Main risk


MOV Files stay mainly in Premiere, Resolve, After Effects, VFX pulls, or desktop review workflows Simple self-contained files that most creative apps handle easily Not ideal when the recipient expects MXF delivery or Avid-native media


MXF OP1a You need delivery, broadcast-style exchange, mixed NLE handoff, or a flattened master One self-contained MXF with video, audio, and metadata together Still needs careful matching for DNx flavor, audio layout, timecode, and levels


MXF OP-Atom Media Composer needs native-style Avid media, usually with an AAF workflow Fits Avid MediaFiles and Avid media database workflows Awkward as general interchange for Premiere or Resolve because media essence may be split across files


## FAQ


Use DNxHR for UHD, 4K, and larger-than-HD projects. DNxHD is designed around HD rasters such as 720p and 1080p. For 4K editorial proxies, DNxHR LB or SQ is usually appropriate. For finishing or color handoffs, DNxHR HQX is often the practical 10-bit choice.


The codec may be the same, but the wrapper changes how the file behaves in a workflow. DNx in MOV is common for Premiere, Resolve, After Effects, and general desktop interchange. MXF OP1a is a self-contained professional interchange or delivery wrapper. MXF OP-Atom is mainly for Avid-native Media Composer workflows and is less convenient as a general handoff format.


Yes. Premiere Pro can read and export DNxHD and DNxHR in common editorial workflows. The main limitation compared with Media Composer is not basic codec support, but media management. Premiere does not manage OP-Atom Avid media the same way Media Composer does, so relinking, proxies, audio mapping, and project organization need more deliberate setup.


For a high-quality graded return, DNxHR HQX is usually a good choice because it supports 10-bit workflows and is widely usable in finishing pipelines. MOV is often convenient for Premiere, while MXF OP1a may be better if the receiving workflow expects MXF. Always test levels in Premiere with scopes before rendering the full timeline.


This is usually a levels or color-management interpretation issue between applications, wrappers, players, or displays. Resolve, Premiere, QuickTime playback, operating system color management, and monitoring hardware may not all interpret the file the same way. The safest method is to test a short render, bring it into the destination app, and compare scopes rather than judging only by a desktop player.


Do not rely only on folder names, because DNx wrappers and flavors can look similar to non-technical reviewers. Aspect lets teams add custom metadata fields such as wrapper, codec flavor, delivery status, reel policy, approval state, or target NLE, then view and sort assets in a spreadsheet-like list using[custom metadata](https://aspect.inc/features/review-and-approve#metadata) .
