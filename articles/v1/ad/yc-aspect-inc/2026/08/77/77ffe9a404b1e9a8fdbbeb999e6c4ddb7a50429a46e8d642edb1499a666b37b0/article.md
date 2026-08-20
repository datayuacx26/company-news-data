---
schema_version: "1.0.0"
document_id: "77ffe9a404b1e9a8fdbbeb999e6c4ddb7a50429a46e8d642edb1499a666b37b0"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/camera-workflows/how-to-configure-canon-xf-avc-for-broadcast-delivery"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T05:48:31.739108+00:00"
fetched_at: "2026-08-18T05:48:33.149548+00:00"
content_hash: "sha256:d1caf6bc32c4c23cc3632835450cbd54fc8342bb11e2f62eb8f7f3d843016ad7"
---

# How to Configure Canon XF-AVC for Broadcast Delivery

Start by configuring XF-AVC for the broadcaster’s acquisition requirements, not for the final delivery file.


That sounds obvious, but it's the place where a lot of Canon workflows get messy. XF-AVC is usually your camera master format, while the delivery file may be XDCAM HD422, AVC-Intra, ProRes, DNxHD, IMF,[AS-11](https://amwa-tv.github.io/AS-11_X9/AMWA_AS_11_X9.html) , or another network-specific MXF package. Your camera settings need to preserve enough quality, metadata, timecode, and audio structure to survive edit, grade, mix, QC, and transcode into that delivery format.


If the spec says “camera originals must be 10-bit 4:2:2,” shoot XF-AVC in a 10-bit 4:2:2 mode. If it says 29.97i delivery, don't casually shoot 23.98p unless the post path and cadence are already approved. If it says four discrete audio channels, set that up in camera and confirm the NLE sees them as separate channels. The safest XF-AVC setup is the one that matches the show’s post and delivery assumptions from the first card.


## XF-AVC is an acquisition choice, not a universal delivery answer


Canon’s XF-AVC is[based on MPEG-4 AVC/H.264](https://global.canon/en/news/2015/apr08e.html) and is used across Cinema EOS and XF cameras in MXF-based workflows. Depending on the camera, it can record UHD/4K, 2K, and HD in intra-frame or Long GOP modes, with options for 10-bit 4:2:2 and, on some models, higher-end RGB 4:4:4 recording.


For broadcast work, the important distinction is intra-frame versus Long GOP.


Intra-frame treats frames independently, while Long GOP links neighboring frames into groups. Decision area XF-AVC Intra XF-AVC Long GOP


Compression behavior Each frame is compressed independently Frames are compressed across a group of pictures


Media and storage Larger files and shorter card runtimes Smaller files and longer card runtimes


Edit and grade performance Easier to scrub, decode, multicam, grade, and conform More CPU intensive and more likely to need proxies or transcodes


Typical fit UHD finishing, color-heavy work, VFX, high-end documentary, and tight online schedules News, factual, events, HD broadcast, and storage-limited field workflows


Main workflow risk Higher storage load, longer backups, and more media planning Playback, relink, cadence, or broadcaster acceptance problems if the exact mode is untested


Best choice when Post responsiveness and finishing margin matter most The broadcaster and post path explicitly accept the exact recording mode


Intra-frame XF-AVC compresses each frame independently and is heavier on media and storage, but easier to decode, scrub, grade, and conform. Long GOP XF-AVC uses groups of pictures, which is more storage-efficient but more computationally demanding in post.[Canon’s own Cinema EOS materials](https://downloads.canon.com/nw/camera/products/cinema-eos/c700/white-papers/eosc700-whitepaper-extended-recording.pdf) position intra XF-AVC for 4K/UHD capture and 2K/HD capture, while Long GOP XF-AVC is described around 2K/HD broadcast television applications and lower-data-rate proxy or offline use.


Long GOP can be acceptable, but you shouldn't choose it only because it saves card space. Use it when the broadcaster, post team, and finishing path accept it, and when turnaround or storage constraints matter more than edit-system simplicity.


Common XF-AVC decision points include:


- Capture resolution may be 4096x2160, 3840x2160, 2048x1080, or 1920x1080, depending on camera and sensor mode
- Frame rate and system frequency may be 23.98, 24.00, 25, 29.97, 50, 59.94, progressive or interlaced where supported
- Broadcast-oriented modes are often YCbCr 4:2:2 10-bit, with some RGB 4:4:4 10-bit or 12-bit options on higher-end models
- Compression structure can be Intra-frame for higher-end finishing and heavier post, or Long GOP for efficient broadcast and offline-friendly workflows
- Media is wrapped as MXF inside Canon’s folder structure
- Audio and timecode are embedded production data that must stay consistent through ingest and conform


[Canon’s C700 documentation](https://support.usa.canon.com/kb/s/article/ART174735) , for example, lists XF-AVC configurations such as 4K/UHD YCbCr 4:2:2 10-bit intra-frame at high bitrates including 810 Mbps and 410 Mbps, plus 2K/HD RGB 4:4:4 12-bit intra-frame options around 440 Mbps. Other Canon bodies expose different menus and limits, but the logic is the same: pick the mode that satisfies the spec while leaving enough margin for post.


## Match the camera menu to the delivery reality


For most broadcast jobs, start with the delivery spec and work backward.


Broadcast delivery requirements should drive the XF-AVC camera setup. If final delivery is HD SDR, you may not need to shoot 4K unless reframing, archive value, VFX, or production policy require it. If final delivery is UHD HDR, don't shoot an HD Long GOP mode and expect post to rescue the master. If the network requires 10-bit 4:2:2 source, avoid 8-bit recording modes even if the picture looks fine on set.


Your team should lock the camera configuration around a few non-negotiables:


- Set the system frequency to 59.94 Hz, 50 Hz, or true 24.00 Hz, depending on region and delivery
- Match the project frame rate to the actual edit and delivery cadence
- Choose progressive or interlaced scanning based on the spec and production style
- Set the capture resolution, not just the delivery resolution
- Choose XF-AVC Intra or XF-AVC Long GOP
- Confirm bit depth and chroma, usually 10-bit 4:2:2 for serious broadcast acquisition
- Set gamma and color space to Canon Log 2, Canon Log 3, Wide DR, BT.709, PQ, or HLG, depending on finishing plan
- Confirm audio channel count, sample rate, and track assignment
- Set timecode to time-of-day, record-run, or jam-synced external code


The takeaway is that “XF-AVC” alone isn't a useful enough spec. A post supervisor needs to know the full recording mode: codec family, intra or Long GOP, bitrate class, frame size, frame rate, bit depth, chroma sampling, gamma, color space, and audio layout.


On cameras such as the C700, the camera assistant selects the main recording format from the Rec/Media Setup menu, where the camera assistant can choose XF-AVC or ProRes for CFast recording. Other Canon Cinema EOS cameras use different menu layouts, but your production should still document the exact recording preset before principal photography. If the show has multiple camera bodies, verify that each one is actually capable of the same XF-AVC mode. Similar model names don't guarantee identical bitrate, frame-rate, or media options.


## When to choose Intra and when to choose Long GOP


Use XF-AVC Intra when the footage will go through a heavier post path.


That includes color-intensive work, VFX pulls, multicam finishing, high-end documentary finishing, UHD master delivery, or a schedule where edit responsiveness matters. Intra-frame files are larger, but the predictability is worth it. Systems don't need to reconstruct frames from surrounding frames every time an editor scrubs or a colorist jumps around the timeline.


Use XF-AVC Long GOP when storage, card runtime, or field turnaround is the main constraint and the acceptance path is clear.


Long GOP can be a good fit for news, factual, events, and HD broadcast workflows where the post path is designed for it, and it may also be used for lower-data-rate proxy or offline-oriented recording on some Canon workflows. The main risk is later discovering that the edit system, shared storage, remote workflow, or broadcaster’s technical rules are less happy with the exact Long GOP variant than expected.


Typical failure modes show up quickly:


- Choppy playback during multicam editing
- Slow waveform generation and thumbnailing
- Inconsistent performance between Mac and Windows workstations
- Relink problems when proxies, transcodes, and camera originals don't share clean metadata
- QC rejection because the acquisition mode didn't meet bit depth, chroma, or cadence requirements


If you choose Long GOP, make that a deliberate workflow decision. Test a real card through ingest, edit, grade, audio turnover, export, and QC before the first shoot day.


## Preserve the Canon card structure during ingest


Canon XF-AVC media isn't just a pile of video essence files. Canon documents the card folder path with a[root-level CONTENTS folder](https://support.usa.canon.com/kb/s/article/ART165712) and a CLIPS folder such as CLIPS001. Users can drill down into the card to find the video files, but for professional ingest, copying only the visible MXF files is a bad habit.


Copy the whole card volume structure into a named camera-original folder. Keep the folder names stable. Don't rename individual MXF files unless the workflow has a tested relink and metadata strategy. If you need human-readable organization, name the parent folder with production, shoot date, camera, card, and roll.


Copying the full card structure keeps related folders and media together for ingest. A simple card folder name might look like this:


- SHOW_EP103_A001_2026-04-18
- SHOW_EP103_B004_2026-04-18
- SHOW_INT_CAMC_CARD017


Inside that folder, preserve the Canon structure exactly as it came from the card. This protects sidecar data, clip relationships, spanned clips, metadata, and future relink, and it also makes life easier if the project moves between Premiere, Resolve, Avid, a color facility, a finishing vendor, and archive storage.


The ingest log should capture the details that matter later: camera body, firmware if relevant, recording mode, frame rate, gamma, color space, LUT used for monitoring, audio channel map, timecode mode, and whether the camera or post team generated proxies.


## Ingesting XF-AVC in Premiere Pro


Modern Premiere workflows can import Canon MXF-based media natively in most common cases, while older workflows historically depended on[Canon XF plugins](https://support.usa.canon.com/kb/s/article/ART138053) for some NLE versions. The key is to use Premiere’s media workflow in a way that preserves the relationship between the project, camera originals, and any proxies.


For most broadcast jobs, bring the preserved card folders into a managed media location first, then import into Premiere. Avoid cutting directly from camera cards, download folders, or ad hoc desktop copies.


Premiere’s ingest and proxy tools are useful when XF-AVC performance is a concern. You can copy media on import, create proxies, or transcode through Media Encoder depending on the facility workflow. For assistant editors, the important thing is consistency: proxy file names, timecode, audio channel layout, and frame rate must line up with the full-resolution XF-AVC.


Premiere can relink offline clips through the Link Media dialog when files are moved, renamed, or deleted outside the project. That's helpful, but it isn't a substitute for clean media management. Relink becomes fragile when someone copied only individual MXFs, camera roll naming is inconsistent, or your team created proxies without matching timecode and audio channel structure.


Premiere also supports reconnecting full-resolution media to proxies. That matters because your team should do high-quality tasks against the camera originals or approved finishing media, not accidental proxy media. If the offline edit used lightweight proxies, reconnect full-resolution XF-AVC before color, online, final graphics review, and delivery encoding.


In Premiere, pay attention to:


- Confirm clip interpretation for frame rate and field order
- Set audio channel mapping before editing starts
- Handle color management and LUTs intentionally for Canon Log material
- Use XMP metadata fields that identify camera roll, scene, take, and notes
- Check proxy attachment status before export or turnover
- Match sequence settings to the actual delivery cadence


Don't assume the export preset fixes upstream mistakes. Premiere’s export panel gives access to format, codec, frame size, frame rate, field order, bitrate, color-related options, and other encoding settings, but the master will only be as clean as the timeline and source interpretation behind it.


## Ingesting XF-AVC in DaVinci Resolve


Resolve’s supported codec lists include Canon XF-AVC, XF-AVC Intra, and XF-HEVC in MXF Op1A and related MXF workflows. As always, support can vary by Resolve version, operating system, hardware, and Studio versus free licensing for some formats, so your facility should test the exact camera mode.


In Resolve, the safest approach is the same: import from preserved card copies, not loose files. Use the Media page to inspect metadata, timecode, resolution, codec, frame rate, and audio channels before building bins or timelines. If your team is conforming the project from Premiere, confirm that reel name behavior is compatible with the XML, EDL, or AAF strategy your team is using.


Resolve gives post teams several ways to make XF-AVC easier to work with:


- Generate proxy media for editing performance
- Use optimized media where appropriate inside a Resolve-managed workflow
- Transcode camera originals to a finishing-friendly intermediate
- Organize clips with metadata and smart bins
- Build deliver-page presets for recurring broadcast outputs


Proxy media and optimized media aren't the same operationally. Proxy files are portable alternate media that your team can manage outside Resolve. Optimized media is more internal to Resolve’s performance workflow. For a facility handoff, portable proxies or proper transcodes are usually easier to reason about than workstation-local optimized media.


For color-managed Resolve projects, tag and transform Canon Log 2, Canon Log 3, Wide DR, BT.709, HDR PQ, or HLG sources intentionally. A surprising number of QC issues start as a color-management mismatch that nobody noticed until the file hit a waveform monitor.


## Metadata is part of the deliverable chain


[XF-AVC metadata](https://support.usa.canon.com/kb/s/article/ART174791) helps editorial, conform, archive, and QC understand what the clip is and where it came from.


XF-AVC metadata travels with the clip as useful production properties. Premiere exposes clip metadata through its Metadata panel, supports XMP metadata workflows, and can display metadata overlays. Resolve supports metadata presets, importing and using metadata, renaming clips with metadata, searching by metadata, and smart bins. Those features reduce ambiguity when there are hundreds of clips, multiple cameras, duplicate file names, and proxy or transcode layers.


At minimum, preserve and track:


- Original file name
- Camera roll or card ID
- Camera letter or unit
- Clip start and end timecode
- Shoot date
- Frame rate
- Resolution
- Codec and recording mode
- Gamma and color space
- LUT used for viewing during the edit, if any
- Scene, slate, take, and notes when available
- Audio channel assignments


If production sound is separate, timecode consistency becomes even more important.[Jam-sync cameras and audio recorders](https://www.youtube.com/watch?v=NYIImgKQWHc) when the workflow depends on it, and document whether timecode is time-of-day, record-run, or externally generated. Editorial can fix many things, but it shouldn't have to guess whether a camera’s clock drifted or whether a clip was interpreted at the wrong rate.


## How XF-AVC fits into broadcast QC


Broadcast QC primarily cares whether the finished master conforms to the spec, and XF-AVC affects QC because it determines what quality and metadata you bring into finishing.


The most common QC-sensitive areas are:


- Avoid accidental frame rate and cadence problems, including 23.98-to-29.97 conversions, duplicate frames, or mixed-cadence timelines unless approved
- Confirm correct field dominance for interlaced deliveries
- Manage color space and levels intentionally, with legal luma and chroma levels where required for SDR or HDR
- Avoid unnecessary 8-bit or 4:2:0 bottlenecks if the spec expects a higher-quality bit depth and chroma path
- Match audio layout to the delivery document, including channel order, stereo pairs, M&E, 5.1, silence, tone, and loudness
- Align timecode for program start, bars, slate, first frame of action, and captions
- Follow the broadcaster’s sidecar or embedded requirements for captions and subtitles
- Match the required profile for the delivery MXF, MOV, IMF, or AS-11 package


This is why it's dangerous to treat camera setup and delivery encoding as separate worlds. If you shoot the wrong cadence or lose channel assignments during ingest, the delivery export becomes damage control.


## Export from the timeline the broadcaster actually asked for


Your team should make most broadcast deliveries from a locked, conformed, full-resolution timeline, not from the offline edit sequence. Build that timeline at the delivery frame rate, resolution, scan type, and color space. If the camera originals were XF-AVC but the delivery spec calls for another codec, export to that required codec.


Premiere and Media Encoder expose detailed encoding controls, but smart rendering only applies to supported combinations of source format, preview format, wrapper, and export codec. Don't assume that an XF-AVC source timeline will pass through untouched, because if the delivery codec is different, you're transcoding. Set render quality, scaling, color, field order, audio, and bitrate with the spec in front of you.


Facilities often use Resolve’s Deliver page for recurring broadcast presets, especially in facilities that finish and QC inside Resolve. The same rule applies: make the deliver preset match the broadcaster, not the camera. XF-AVC may be the source format, the conform source, or an archive source, while the deliverable may be a completely different profile.


A clean Canon XF-AVC broadcast workflow usually looks like this:


- Shoot the approved XF-AVC mode with matching frame rate, bit depth, chroma, gamma, audio, and timecode
- Copy and verify the full Canon card structure
- Import into Premiere or Resolve without breaking folder structure or clip metadata
- Generate proxies or transcodes only when the naming, timecode, and audio plan is tested
- Edit and conform against correctly interpreted media
- Grade with intentional Canon color management
- Reconnect full-resolution media before finishing
- Export the required broadcaster delivery format
- Run technical QC against the final file, not just the camera originals


The camera setting is only the first move, and a reliable workflow is the one where every later system can still understand what was shot, how it should be interpreted, and what the final broadcaster expects.


## FAQ


Usually, XF-AVC is treated as a camera acquisition format rather than the final delivery format. Many broadcasters require delivery as XDCAM HD422, AVC-Intra, ProRes, DNxHD, IMF, AS-11, or a specific MXF profile. The safe approach is to shoot XF-AVC in a mode that meets the acquisition requirements, then conform and export the final master to the broadcaster’s required delivery specification.


Choose XF-AVC Intra when the footage will go through heavier post, such as grading, VFX, multicam editing, UHD finishing, or tight online workflows. Choose XF-AVC Long GOP when storage efficiency, card runtime, and field turnaround matter, and when the broadcaster and post pipeline have already approved that exact recording mode. Long GOP can work well, but it should be tested through ingest, edit, conform, export, and QC before production.


Yes. Copy the full card structure, including the Canon folders, rather than only copying individual MXF files. The folder structure can contain information needed for clip relationships, spanned clips, metadata, and reliable relinking. A good practice is to place the untouched card copy inside a clearly named parent folder that identifies the show, date, camera, card, or roll.


Modern versions of Premiere Pro and DaVinci Resolve can usually work with common Canon XF-AVC MXF media natively, but support depends on the exact camera mode, software version, operating system, hardware, and licensing. Facilities should test the specific XF-AVC resolution, frame rate, bit depth, chroma sampling, and compression type before the shoot. If native performance is poor, proxies or transcodes may be a better operational choice.


At minimum, track the original file name, camera roll or card ID, camera letter, clip timecode, shoot date, frame rate, resolution, codec mode, gamma, color space, LUT used for editorial viewing, scene, take, notes, and audio channel assignments. This metadata helps editorial, conform, color, audio turnover, archive, and QC teams understand the source media and avoid relink or interpretation problems.


Capture that information during ingest and keep it searchable alongside the media, especially when multiple camera bodies, proxies, and delivery transcodes are involved. Aspect lets teams define project-specific custom metadata, so fields like camera ID, card roll, codec mode, color space, and audio map can travel with the assets.
