---
schema_version: "1.0.0"
document_id: "4989ffe16dff26583bf10616f6248a13b9353eb2b44f6e1b0e8fd09974adf105"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/codec-workflows/guide-to-dnxhr-profiles-for-avid-editing-pipelines"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T07:17:16.721422+00:00"
fetched_at: "2026-08-06T07:17:17.716201+00:00"
content_hash: "sha256:23965849e9816dfa51bd6905aef9891c0a1dc9e0cf673cc411af7b2705825eed"
---

# Guide to DNxHR Profiles for Avid Editing Pipelines

Pick the DNxHR profile from the job you're actually doing, not from the camera spec alone. For most Avid editorial pipelines, that means DNxHR LB for lightweight offline, DNxHR SQ when offline needs more visual confidence, DNxHR HQ or HQX for high-quality 4:2:2 finishing media, and DNxHR 444 only when you truly need 4:4:4 color fidelity through the handoff.


The expensive mistake is treating every DNxHR option as “better quality, bigger file.” That's technically true, but running a facility requires more. The right profile changes how much shared storage you need, how many streams can play in real time, whether assistants can move media remotely, whether color and VFX see the bit depth they expect, and whether Media Composer can simply rewrap media or has to do more work.


[DNxHR is designed](https://resources.avid.com/SupportFiles/attach/Media_Composer_Editing_Guide_2024.x.pdf) for high-resolution and resolution-independent workflows. DNxHD is still common for HD projects, but DNxHR is the family you reach for when your raster is UHD, DCI 4K, larger-than-HD, or when you want a consistent profile set across proxy, offline, and finishing media.


## The practical profile map


Here is the short version most teams need when building an Avid pipeline:


- DNxHR LB: low-bandwidth 8-bit 4:2:2 media for proxies and offline editorial.
- DNxHR SQ: standard-quality 8-bit 4:2:2 media for better-looking offline, light effects, and screenings where LB is too soft.
- DNxHR HQ: high-quality 8-bit 4:2:2 media for finishing only when 8-bit is acceptable.
- DNxHR HQX: high-quality 12-bit 4:2:2 media for 10-bit and 12-bit camera sources, grading, and most high-end 4:2:2 finishing.
- DNxHR 444: 12-bit 4:4:4 media for RGB, log, VFX pulls, graphics, and finishing where chroma fidelity matters.


Most teams cut with LB or SQ, then relink back to camera originals or finish with HQX/444 media. HQ is useful, but it's easy to overuse. If the source and delivery path are 10-bit, HQ isn't the “safe high-quality” choice because it's still an 8-bit profile. HQX is usually the safer finishing profile for 10-bit 4:2:2 workflows.


DNxHR profiles form a practical spectrum from lightweight editorial media to heavier finishing media. DNxHR profile Bit depth and chroma Best fit Use it when Be careful when


LB 8-bit 4:2:2 Lightweight offline and proxy media You need smooth playback, low storage use, remote-friendly media, or many hours online Reviews, reframes, temp effects, or focus checks need more detail


SQ 8-bit 4:2:2 Higher-quality offline and review media Proxies need to look better for screenings, multicam, temp effects, or client review Storage or bandwidth is tight and LB would be acceptable


HQ 8-bit 4:2:2 High-quality 8-bit media The source and delivery path are 8-bit, or the job does not require a 10-bit finish The source is 10-bit log, HDR, RAW-derived, or intended for serious grading


HQX 12-bit 4:2:2 High-quality 4:2:2 finishing media You need a practical finishing or graded return format for 10-bit or 12-bit 4:2:2 workflows The material truly needs RGB or 4:4:4 chroma fidelity


444 12-bit 4:4:4 RGB, graphics, VFX, and 4:4:4 finishing Keys, edges, gradients, graphics, VFX pulls, or the color pipeline benefit from 4:4:4 The source and delivery are 4:2:2 and HQX would preserve what the workflow needs


DNxHR 444 is bigger, heavier, and only pays for itself if the source, graphics, grade, VFX, or delivery path benefit from 4:4:4. If the camera source is 4:2:2 and the final delivery is 4:2:2, HQX is usually the more practical finishing codec.


## Match the profile to the workflow stage


Avid workflows usually separate three concerns: cutting performance, visual confidence, and finishing quality. Trying to satisfy all three with one media set often creates unnecessary storage and playback problems.


For offline editorial, the main job of the media is to cut smoothly, relink cleanly, and travel easily. DNxHR LB is built for that, and it gives assistants and editors enough image quality to make decisions without pushing every workstation and shared storage volume into finishing-mode bandwidth.


DNxHR SQ is a good middle ground when the offline needs to hold up in reviews. It's useful for documentary, reality, multicam, and agency review sessions where people will react badly to very soft proxies, and it's also a better proxy choice when editors are doing temp reframes, split screens, stabilizes, or basic effects that need more detail than LB gives them.


For finishing, your team should make the profile decision from source bit depth and chroma sampling. If the camera originals are 10-bit log or RAW debayered to 10-bit 4:2:2, DNxHR HQX is usually the practical finishing target. If graphics, VFX, or color need 4:4:4, use DNxHR 444 for those elements or for the finishing render path.


The important part is that the profile has a job. LB and SQ are there to make editorial fast. HQX and 444 are there to preserve detail through finishing. HQ sits between those worlds, but your team shouldn't choose it blindly just because it says “high quality.”


## How Media Composer treats DNxHR natively


DNxHR is an[Avid-native codec family](https://resources.avid.com/SupportFiles/attach/Avid_Supported_Video_File_Formats.pdf) , so Media Composer can work with it efficiently when your team creates and wraps the media in the expected way. In normal Avid-managed media workflows, DNxHR media lives as MXF media under the Avid MediaFiles structure. When the format and project settings match, Media Composer can often rewrap or fast import instead of performing a full transcode.


That native behavior matters for assistant editors because if you receive a DNxHR QuickTime, OP1a MXF, or third-party generated file, Media Composer may link to it, import it, consolidate it, or transcode it depending on the wrapper, codec, and media creation settings. The codec being DNxHR doesn't guarantee that dragging the file into an Avid media folder will work.


This is where a lot of preventable failures happen:


- Your team places a generic MXF directly into an Avid MediaFiles/MXF numbered folder, but it isn't[Avid OP-Atom media](https://resources.avid.com/SupportFiles/attach/HighRes_WorkflowsGuide.pdf) .
- Your team imports a DNxHR 444 file into a project whose media creation setting is DNxHR HQX or HQ, causing a mismatch prompt.
- A third-party transcode uses the right codec name but the wrong wrapper for Avid-managed media.
- A proxy transcode changes filename, timecode, source ID, or audio channel structure, making relink unreliable later.
- Your team sets a project to an HD raster, so high-resolution DNxHR transcode options aren't available in the way the team expects.


The takeaway is simple: use Media Composer, Avid-aware automation, or a tested dailies/transcode path to create Avid media. If your pipeline uses third-party tools, test the exact wrapper and folder behavior before the first shoot day. A file that plays in QuickTime or Resolve isn't necessarily valid Avid-managed media.


## Resolution and project raster affect your options


DNxHR supports high-resolution frame sizes, but Media Composer’s available transcode choices depend on project setup. Avid’s high-resolution workflow guidance calls out an important behavior:[in HD projects](https://resources.avid.com/SupportFiles/attach/High_Res_WorkflowsGuide.pdf) , the transcode menu is limited to HD formats. If you link high-resolution sources inside an HD project, you may not see the high-res or fractional proxy options you expected.


That matters for shows that cut in 1080 while finishing in UHD or 4K. A 1080p project can be convenient, especially for standard 16:9 UHD work, but assistants need to understand what happens during transcode. If you need high-res DNxHR media or specific proxy dimensions, you may need to temporarily use a larger-than-HD project raster, create the transcodes, then return to the editorial project setup.


Frame size decisions also affect relink and finishing. If you bake a resize into the transcode, that's different from using[FrameFlex metadata](https://resources.avid.com/SupportFiles/attach/Media_Composer_Editing_Guide_2024.x.pdf) to reframe source media. Baking a resize can be fine for editorial proxies, but it can create confusion later if the finishing team expects the sequence to reference the original raster and use source-side framing metadata.


A source-side crop can remain flexible, while a baked resize locks the crop into the media. A clean pattern is:


- Link to camera originals and confirm source metadata.
- Create proxies at the agreed editorial raster and DNxHR profile.
- Preserve source timecode, source file name, reel or tape metadata, and audio layout.
- Use FrameFlex intentionally for reframes instead of accidentally baking framing decisions into proxy media.
- Relink or conform against the original camera media, high-res DNxHR transcodes, or graded returns depending on the finishing plan.


Consistency matters more than one perfect setting because Avid can handle high-res and proxy workflows well, but only if your team plans the project raster, proxy dimensions, and relink metadata together.


## Storage and bandwidth planning by profile


DNxHR profiles scale with frame size and frame rate, which means UHD at 59.94 costs roughly twice as much bandwidth as UHD at 29.97. DCI 4K costs more than UHD, and multicam multiplies everything. So the right way to plan storage is to use Avid’s[DNxHR bandwidth tables](https://kb.avid.com/pkb/articles/en_US/Knowledge/Avid-DNx-naming-scheme-and-data-rates) for the exact raster and frame rate, then add headroom for real-world playback.


For quick planning, these rough ranges are useful for UHD 3840x2160 at common editorial frame rates:


- DNxHR LB: roughly 85 to 110 Mb/s around 23.976 to 29.97 fps.
- DNxHR SQ: roughly 200 to 260 Mb/s around 23.976 to 29.97 fps.
- DNxHR HQ: roughly 400 to 520 Mb/s around 23.976 to 29.97 fps.
- DNxHR HQX: roughly 680 to 850 Mb/s around 23.976 to 29.97 fps.
- DNxHR 444: roughly 820 Mb/s to just over 1 Gb/s around 23.976 to 29.97 fps.


Those are per stream, and once you add multicam, stacked video layers, grouped clips, renders, shared storage overhead, and multiple editors, the difference between LB and HQX becomes a facility-level decision, not just a quality preference.


Per-stream bitrate choices multiply quickly across multicam, layers, renders, shared storage, and editors. A simple way to sanity-check storage is to convert megabits per second to gigabytes per hour. Divide Mb/s by 8 to get MB/s, then multiply by 3.6 to get GB per hour. For example, a 100 Mb/s proxy stream is about 45 GB per hour. A 700 Mb/s finishing stream is about 315 GB per hour. Multiply that by camera hours, backup copies, renders, turnovers, and the number of versions you expect to keep online.


Bandwidth planning needs the same realism because a single workstation playing one stream of DNxHR LB is easy, while a shared project with three editors, two assistant stations, and multicam group playback is different. Avid NEXIS and similar shared storage systems are built for this kind of work, but the codec profile still determines how quickly you burn through available throughput.


For editorial, LB lets you keep more media online and remote-friendly. SQ costs more, but can reduce complaints about proxy quality. Your team should usually limit HQX and 444 to finishing, color returns, VFX pulls, online conforms, or specific sequences that need them.


## Choosing between LB and SQ for proxies


LB is the default proxy answer when the priority is performance and storage, and it's especially useful when editors are remote, laptops are involved, camera hours are high, or the show has many simultaneous users.


SQ is worth the extra size when offline image quality affects the work. Common examples include:


- Heavy reframing from UHD or 4K sources.
- Temp comps, split screens, stabilizes, and speed effects.
- Client or network review directly from editorial exports.
- Interviews or archive where fine texture and focus judgment matter.
- Multicam where LB compression makes matching angles harder.


If your team is unsure, test with a real scene. Use a noisy low-light shot, a detailed wide shot, skin tones, and a fast camera move. Generate LB and SQ at the intended proxy raster. Put them in an actual timeline with any normal LUTs, burn-ins, temp effects, and review exports. The better choice usually becomes obvious within ten minutes.


## Choosing between HQ, HQX, and 444 for finishing


For finishing, start with bit depth and chroma.


[DNxHR HQ is 8-bit 4:2:2](https://academysoftwarefoundation.github.io/EncodingGuidelines/EncodeDNXHD.html) , and it can be a good high-quality editorial or delivery-adjacent format when the source and delivery are both 8-bit, or when the job doesn't justify a 10-bit finishing path. But for modern log camera workflows, HDR work, or serious grading, it's often the wrong ceiling.


DNxHR HQX is the workhorse for high-quality 4:2:2 finishing because it supports higher bit depth. If you're rendering graded media back from Resolve to Media Composer for final assembly, HQX is often the practical choice for 10-bit workflows. Avid’s own Resolve roundtrip guidance recognizes DNxHR and DNxHD as supported interchange families, which is one reason they remain common in Avid finishing pipelines.


DNxHR 444 is for cases where 4:4:4 matters, which includes RGB graphics, some VFX plates, certain log finishing paths, and workflows where chroma subsampling would visibly damage edges, keys, gradients, or composited material. Use 444 when the images need it and the storage can support it.


A mixed finishing pipeline is normal, and camera-originated 4:2:2 material might finish as HQX, while graphics or VFX returns come back as 444. The key is to document which deliverables expect which profile so assistants don't accidentally transcode everything down to a lower media creation setting.


## Relink depends on metadata discipline


DNxHR profiles don't make relink safe by themselves because relink works when Media Composer can match the editorial media back to the original or alternate media using consistent identifiers.


Your proxy workflows should preserve the fields the conform will need:


Proxy workflows need preserved identifiers so editorial media can relink cleanly to originals or finishing media.


- Source file name or source path.
- Start timecode and duration.
- Reel, tape, or source ID strategy.
- Clip name conventions.
- Audio channel count and ordering.
- Frame rate and project rate interpretation.
- Camera metadata needed by color or VFX.


Avid has dedicated relink workflows for connecting transcoded clips back to original source media. Newer Media Composer versions also include improved proxy and dual-resolution linking features for environments that support them. Those tools are useful, but they don't rescue a pipeline where every transcode tool invents different names, strips metadata, or changes audio layouts.


For large teams, the practical move is to create one tested proxy recipe and one tested finishing recipe. Don't let every assistant, dailies vendor, and department make “equivalent” DNxHR files unless your team has tested the files inside the actual Avid project.


## Roundtripping with Resolve and other tools


[DaVinci Resolve supports](https://kb.avid.com/pkb/articles/en_US/Knowledge/Exchanging-Sequences-with-DaVinci-Resolve) DNxHR and DNxHD, which makes DNxHR a common bridge between Media Composer and color. A typical Avid-to-Resolve-to-Avid path might send an AAF with linked camera originals to Resolve, then render graded DNxHR HQX or 444 media back for final assembly in Media Composer.


Your return profile should reflect the grade and delivery path, and HQX is usually the practical return format for 10-bit 4:2:2 mastering. 444 is appropriate when the color pipeline, graphics, or VFX require it. Your team should generally not use LB and SQ for graded final returns unless the return is only for offline review.


The danger in roundtrips is silent mismatch because if Resolve renders DNxHR 444 but you set Media Composer media creation to HQX or HQ, you may see import prompts or behavior differences depending on whether you preserve the source resolution/profile or transcode. That isn't something to discover during final output, so test a short roundtrip with the actual profile, wrapper, project raster, handles, audio, and color management before the full grade starts.


## A sane default for most Avid pipelines


If you need a practical starting point, use this:


- Offline editorial: DNxHR LB at the agreed proxy raster.
- Higher-quality offline or review-heavy shows: DNxHR SQ.
- Standard 10-bit 4:2:2 finishing returns: DNxHR HQX.
- Graphics, VFX, RGB, or 4:4:4 finishing: DNxHR 444.
- Avoid DNxHR HQ for 10-bit finishing unless everyone understands the 8-bit limitation.


Then validate the exact raster, frame rate, wrapper, and relink behavior in Media Composer. DNxHR is very Avid-friendly when the pipeline is consistent, but most problems come from mixing project rasters, wrappers, metadata strategies, and profile assumptions without testing them as one workflow.


## FAQ


Use DNxHR LB when the priority is smooth playback, small files, remote editorial, or large camera-hour counts. Use DNxHR SQ when offline media needs to look better for reviews, temp effects, reframing, multicam, or focus judgment. Both are 8-bit 4:2:2 profiles, but SQ uses more bandwidth and storage than LB.


DNxHR HQ is 8-bit 4:2:2, so it's usually not the safest finishing choice for 10-bit log, HDR, or serious grading workflows. DNxHR HQX is generally the better 4:2:2 finishing profile because it supports higher bit depth. HQ can still be useful when the source and delivery path are 8-bit or when the job doesn't require a 10-bit finish.


Choose DNxHR 444 when the workflow needs 4:4:4 chroma fidelity, such as RGB graphics, VFX plates, keying, compositing, certain log finishing paths, or deliverables that benefit from full chroma resolution. If the camera source and delivery are 4:2:2, DNxHR HQX is usually more practical and storage-efficient.


Yes, DNxHR is an Avid-native codec family, but native behavior depends on the wrapper and how the media was created. Avid-managed media normally lives as OP-Atom MXF files inside the Avid MediaFiles folder structure. DNxHR QuickTime, OP1a MXF, or third-party generated files may need linking, consolidating, importing, or transcoding depending on the exact file and project setup.


Start with the DNxHR data rate for the exact raster, frame rate, and profile, then convert megabits per second to gigabytes per hour. Divide Mb/s by 8 to get MB/s, then multiply by 3.6 to estimate GB per hour. For example, 100 Mb/s is about 45 GB per hour, while 700 Mb/s is about 315 GB per hour. Multiply by camera hours, streams, multicam angles, renders, backups, and online storage needs.


For remote Avid editorial, the cleanest pattern is to create one tested DNxHR LB or SQ proxy set, then give editors access to the same media instead of distributing duplicate folders. Aspect lets teams mount one shared project space so editors and assistants work from the same proxy media, with fewer handoff delays and fewer relink surprises in a shared cloud filespace.
