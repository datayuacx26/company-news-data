---
schema_version: "1.0.0"
document_id: "044a56209fb29f3251b7876516e070bf653818d35a135c73c581f2357b1322e2"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/codec-workflows/prores-vs-prores-raw-which-camera-workflow-to-use"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-17T06:50:01.197449+00:00"
fetched_at: "2026-08-17T06:50:01.796019+00:00"
content_hash: "sha256:92a46221fd9ac675b32165aba816a0bd3d75cc48500d56091e2b4134719c95f9"
---

# ProRes vs ProRes RAW: Which Camera Workflow to Use

When the image is already close on set and the schedule depends on fast edit turnaround, regular ProRes is usually the better workflow. When the grade needs real latitude for white balance,[ISO, exposure offset](https://support.apple.com/guide/final-cut-pro/adjust-prores-raw-camera-settings-ver3eb60032c/mac) , HDR highlight handling, or mixed lighting, ProRes RAW may be worth considering, but only after you confirm the camera, recorder, NLE, storage, and finishing path can actually carry it.


That's the real decision, because ProRes RAW isn't “better ProRes” for every job; it changes[where image processing happens](https://www.apple.com/final-cut-pro/docs/Apple_ProRes_RAW_White_Paper.pdf) . Regular ProRes gives post a high-quality, already processed video file, while ProRes RAW preserves sensor data and delays more of the camera interpretation until post. That gives color more control, but it also moves responsibility into the workflow: debayering, RAW controls, metadata interpretation, software support, transcoding, and larger working sets.


Regular ProRes is already interpreted into a video image; ProRes RAW keeps more sensor information available for later adjustment. Factor Regular ProRes ProRes RAW


Image state Already processed video pixels RAW sensor data preserved for post processing


Camera decisions White balance, ISO behavior, debayering, sharpening, noise reduction, and log or display transform are mostly committed More camera interpretation can be adjusted later in supported software


Grading latitude Strong latitude when recorded well, especially in 10-bit 4:2:2 or 4:4:4:4 Greater control over ISO, exposure offset, color temperature, and sensor-level interpretation when supported


Editorial compatibility Broadly supported across NLEs, finishing systems, dailies tools, and review workflows More dependent on specific software, metadata handling, and RAW control support


Performance Designed for efficient real-time editing and multistream playback More decode and processing work because RAW data must be interpreted into viewable pixels


Storage pressure Predictable data rates and simpler media management Often creates more workflow overhead through originals, proxies, dailies, transcodes, and archive needs


Best fit Fast turnaround, multicam, documentary, corporate, live capture, and predictable post pipelines HDR work, difficult lighting, hero shots, select VFX plates, and Apple-centered RAW workflows


## The difference that matters in post


Regular ProRes stores conventional video pixels. The camera has already converted sensor data into an image, usually using the camera’s color science, white balance, ISO behavior, noise reduction, sharpening, debayering, and log or display transform. You can still grade it heavily if it's a good 10-bit 4:2:2 or 4:4:4:4 recording, but the camera has baked in some choices.


ProRes RAW stores RAW sensor data using ProRes compression technology. Instead of committing fully to the camera’s processed video output, it keeps the underlying sensor information available for RAW processing later. In Final Cut Pro, for example, ProRes RAW can expose controls such as ISO, exposure offset, and color temperature in the inspector. For supported iPhone ProRes RAW media, Apple’s tools can expose additional camera settings and processing options.


The simple version:


- ProRes is a high-quality editing and finishing codec for already processed video.
- ProRes RAW is a capture format for RAW sensor data.
- ProRes is easier to hand between departments and applications.
- ProRes RAW gives more image latitude, but requires compatible tools and a more deliberate post path.
- You can't[turn a normal ProRes](https://support.apple.com/en-us/102124) recording into ProRes RAW later.


That last point is the one that catches productions. You have to capture ProRes RAW from a compatible camera or camera-plus-recorder setup. If the camera only recorded[ProRes 422 HQ](https://support.apple.com/en-us/102207) internally, the production has already missed the RAW decision.


## When regular ProRes is the right camera workflow


Regular ProRes is still the safer default for a lot of professional work because it's predictable. Editors know how it behaves, and DITs and assistants know how to back it up, transcode it, proxy it, and relink it. Post supervisors can estimate storage and turnaround without building a science project around every camera body.


ProRes fits projects that need:


- Fast dailies and same-day edits
- Multicam cutting with many streams
- A broad choice of NLEs and finishing systems
- Simple archive and turnover
- A grade that starts from a well-exposed log image rather than relying on RAW-side correction


For many shows, ProRes 422 HQ or ProRes 4444 is already more than enough. Apple designed the ProRes family for real-time, multistream editing performance and high image quality at lower storage rates than uncompressed media. That matters when editorial has to cut all day without rendering every move.


The main ProRes decision is which flavor to record. The right answer depends on camera output, bit depth, chroma requirements, graphics/alpha needs, and finishing target. ProRes 422 HQ is often a strong acquisition choice for high-quality 4:2:2 workflows. ProRes 4444 or 4444 XQ makes sense when the pipeline needs 4:4:4:4 quality, alpha channels, or very high-end log preservation. ProRes LT and Proxy are more common in offline editing or lower-data-rate recording contexts, not usually the first choice for premium camera masters unless the project constraints demand it.


The benefit is fewer weird handoff moments. A ProRes file is usually easier to view, sync, string out, transcode, and send to another department. That's why regular ProRes remains a strong choice for documentaries, corporate work, episodic editorial, multicam shoots, live capture, branded content, and lower-friction commercial post.


## When ProRes RAW is worth the overhead


ProRes RAW earns its keep when the image really benefits from post-side RAW control. The project needs a realistic use case for those options and a schedule that can support them.


ProRes RAW is most useful when you expect one or more of these conditions:


- High dynamic range delivery where highlight handling matters
- Mixed lighting or uncontrolled environments where white balance may need deeper correction
- Exposure conditions that are hard to nail consistently
- VFX pulls or beauty work that benefit from cleaner sensor-level interpretation
- A color workflow centered on Final Cut Pro, Motion, Compressor, or another confirmed ProRes RAW-aware tool


RAW gives post more room to make camera interpretation decisions later. If a scene is badly clipped, noisy, underlit, or shot with the wrong monitoring assumptions, RAW won't make it painless. But when exposure and color are close enough, RAW can give the colorist more latitude before the image falls apart.


This is especially relevant for HDR. Apple positions ProRes RAW as well suited to HDR content creation because it carries more of the sensor information into post. In practice, that helps when you're managing highlight rolloff, bright practicals, skies, reflective surfaces, and high-contrast interiors where a baked log recording may leave less flexibility.


## Camera and recorder support isn't automatic


Don't assume that a camera that records ProRes also records ProRes RAW. These are separate capabilities. Some cameras record regular ProRes internally. Some output RAW over HDMI or SDI to a supported external recorder. Some record ProRes RAW internally. Some support another RAW format instead, such as BRAW, ARRIRAW, REDCODE RAW, or Nikon N-RAW.


Common ProRes RAW capture paths include:


- Supported cameras outputting RAW to[Atomos recorders](https://support.apple.com/en-us/101557)
- Cameras that record ProRes RAW directly, including models such as Nikon Z 8 and Nikon Z 9
- DJI Zenmuse X7 recording ProRes RAW directly
- Supported iPhone models recording ProRes RAW through Apple’s camera workflow with the required external storage setup
- Specific third-party cameras and devices that Apple has licensed and certified for ProRes RAW workflows


You still need to check the exact model, firmware version, resolution, frame rate, recorder, media, and app support. ProRes RAW compatibility is full of “yes, but only in this mode” details. A camera may output RAW at one resolution but not another, while a recorder may support one frame rate but not another. The camera or recorder may need a firmware update, and a camera may support ProRes RAW over HDMI but not internally.


The production mistake is treating “supports ProRes RAW” as one complete spec. Treat it as a chain.


ProRes RAW compatibility depends on the whole capture and post chain, not just the camera setting. The chain includes:


- Camera model and firmware
- RAW output mode
- Recorder model and firmware, if external
- Cable type and signal reliability
- Recording media speed
- Resolution, frame rate, and sensor crop mode
- Audio and timecode handling
- NLE import support
- RAW control support in the finishing application
- Transcode or proxy plan
- Archive plan for the original camera files


If one part of that chain fails, the format choice can become a post problem instead of an image-quality advantage.


## The edit system may not want your RAW files


Apple designed ProRes RAW to bring ProRes-like performance to RAW media, but it's still not the same editing burden as regular ProRes. RAW playback requires processing at decode time. The system has to interpret sensor data into viewable pixels, apply[RAW-to-log conversion](https://support.apple.com/en-tm/guide/final-cut-pro/ver5d55de8fd/mac) or a camera LUT, and then handle whatever grade, effects, scaling, or retiming an editor adds.


Final Cut Pro has the cleanest Apple-native path. It can process ProRes RAW automatically on import by applying RAW-to-log conversion and built-in camera LUT settings based on metadata. If that metadata is missing, for example because transcoding or mishandling stripped it, your team may need to view or change those settings manually.


That metadata behavior matters for assistants. A clip that looks correct in one system and strange in another may not be “bad footage.” It may be a RAW interpretation issue. The application may be using a different conversion, missing a camera LUT, reading the camera metadata differently, or defaulting to a transform no one intended.


For editorial, this creates a decision: cut the ProRes RAW originals, or create dailies and proxies. Cutting originals can work on strong hardware, especially for shorter projects or simpler timelines. But if the job has lots of footage, remote editors, multicam, heavy effects, or tight daily turnaround, an offline workflow is usually saner.


Dailies still matter. A proper dailies process gives editorial color-corrected, framed, synced, usable files while preserving the original camera negative for finish. For RAW acquisition, that process also becomes the place where the show establishes its first interpretation of the image. Your team shouldn't treat the dailies color pipeline as an afterthought.


## Storage and performance tradeoffs


ProRes RAW is compressed, but RAW workflows still tend to create more data pressure than a simple ProRes editing path. The camera originals may be larger than the ProRes alternative, and the workflow often creates additional files: dailies, editorial proxies, review exports, intermediate transcodes, VFX pulls, and final graded masters.


The storage problem is how many departments need access, how quickly they need it, and whether the network or drives can sustain playback. A single editor on a fast local RAID has a very different experience than five remote editors pulling media across shared storage.


RAW overhead can show up as shared storage pressure, access delays, and playback bottlenecks across departments. Expect pressure in these areas:


- Camera card offload time
- Cloud upload and download windows
- Proxy generation time
- Relink reliability
- Archive cost


RAW is easiest to justify when the production has already planned for this. If the post team is discovering the storage impact after the first shoot day, the workflow is already in trouble.


The safer compromise is often: capture ProRes RAW only where it matters, capture regular ProRes everywhere else, and keep a consistent dailies format for editorial. Not every angle in a multicam interview needs RAW. Not every B-roll shot needs RAW. Not every social cutdown needs HDR-grade latitude. Selective RAW capture can preserve the image benefits without drowning the whole post pipeline.


## Color control and metadata behavior


The main creative reason to choose ProRes RAW is the ability to adjust camera interpretation in post. In Apple’s RAW controls, that can include ISO, exposure offset, and color temperature. Those controls are different from pushing a normal color correction node on baked video. They happen as part of RAW processing, before the image is fully converted into the working color space.


RAW controls happen before the image is fully interpreted, while normal grading works on an already processed frame. That's useful, but it also means the show needs consistent color management. Decide early how your team should view RAW media, which log conversion to use, whether to apply built-in camera LUTs, and what editorial dailies should look like. Otherwise, editorial, VFX, client review, and color may all be looking at different versions of the same shot.


Common failure modes include:


- RAW clips importing with the wrong camera LUT or no LUT
- Metadata missing after a transcode
- Editorial proxies not matching the RAW interpretation used in color
- Assistants relinking to files with different names, reel metadata, or timecode
- A finishing system that can't access the same RAW controls used earlier


Your team can solve these problems, but only if you catch them during workflow testing. A five-minute camera test through ingest, dailies, edit, relink, color, and export is more useful than a long theoretical debate about codecs.


## ProRes RAW and DaVinci Resolve complications


Many finishing teams live in DaVinci Resolve. That's one reason ProRes RAW needs a specific conversation before production starts. Resolve support for RAW formats isn't universal across every RAW codec, and teams often choose camera-native RAW formats partly because they fit the finishing tool better.


For example, some Nikon workflows may choose N-RAW because it works cleanly in Resolve, while teams may prefer ProRes RAW for an Apple-centered workflow. Panasonic Lumix users may face a similar decision between ProRes RAW and BRAW depending on recorder, camera support, and post toolchain.


The format should follow the finishing plan. If your team has confirmed color in Final Cut Pro or another ProRes RAW-aware workflow, ProRes RAW may be straightforward. If the grade is in Resolve and the team can't bring ProRes RAW through natively in the way they need, you may be adding a transcode step that removes some of the reason you shot RAW in the first place.


That doesn't mean “never shoot[ProRes RAW for Resolve-bound jobs](https://www.youtube.com/watch?v=pY66xg2lD70) ,” but it does mean you need to test the exact path. Know whether you're transcoding to ProRes 4444, another mezzanine format, image sequences, or a different RAW option. Know which controls survive and which your transcode bakes in.


## A sane way to choose on a real job


Start from the finish, not the camera menu. Ask where your team will grade the master, what the delivery requires, and how much image flexibility the grade actually needs. Then work backward to acquisition.


Regular ProRes fits when speed, compatibility, and predictable editing matter more than RAW latitude. That includes most fast-turnaround jobs, many multicam shoots, most corporate and documentary projects, and productions where the camera team can expose and white balance consistently.


ProRes RAW fits when the image needs post-side camera control and your team has built the pipeline for it. That includes HDR-focused work, difficult lighting, premium small-camera acquisition, select VFX plates, and jobs where the finishing team specifically wants ProRes RAW controls.


A mixed workflow is often the most production-friendly choice. It can mean recording ProRes RAW for hero material, difficult scenes, or shots your team will grade most aggressively. It can mean recording ProRes 422 HQ or ProRes 4444 for everything that benefits more from easy editing and handoff. Then normalize the editing experience with dailies or proxies so assistants and editors aren't constantly switching behavior clip by clip.


The codec decision is about deciding where risk belongs. Regular ProRes puts more responsibility on the camera team to bake a strong image and gives post a stable, efficient file. ProRes RAW gives post more control, but asks the production to manage compatibility, metadata, performance, and storage with more care.


If your team will use the extra latitude, ProRes RAW can be the right call. If it will mostly sit unused while assistants fight transcodes and relinks, regular ProRes is the better workflow.


## FAQ


Not automatically. ProRes RAW gives post more control over sensor interpretation, including settings such as ISO, exposure offset, and white balance in supported workflows. Regular ProRes is usually easier to edit, share, relink, archive, and finish. Choose ProRes RAW when the extra image latitude will actually be used and the whole pipeline supports it.


No. ProRes RAW must be captured from a supported camera or camera and recorder setup at the time of shooting. A normal ProRes 422, ProRes 422 HQ, or ProRes 4444 file is already processed video and can't be turned back into sensor RAW data.


ProRes RAW support depends on the exact camera, firmware, resolution, frame rate, recorder, media, and app. Common workflows include supported cameras outputting RAW to Atomos recorders, cameras such as the Nikon Z 8 and Z 9 that can record ProRes RAW internally, DJI Zenmuse X7 workflows, and supported iPhone ProRes RAW capture paths with the required external storage setup. Always confirm the specific mode before production.


It depends on the tested workflow. Many Resolve-based finishing teams prefer RAW formats that Resolve supports directly, such as BRAW, ARRIRAW, REDCODE RAW, or Nikon N-RAW, depending on the camera. If a ProRes RAW project is headed to Resolve, test whether you need to transcode to ProRes 4444, image sequences, or another format, and confirm which RAW controls will be baked in during that process.


Not always in a simple one-to-one comparison, because ProRes RAW is compressed and has different data rates depending on resolution and quality setting. The bigger issue is total workflow storage. RAW jobs often generate additional dailies, proxies, VFX pulls, review files, intermediates, and online conform media, so the overall storage and transfer burden can be higher than a straightforward ProRes editorial workflow.


Treat the RAW interpretation as production metadata, not tribal knowledge. Store notes for camera LUTs, RAW-to-log settings, exposure offsets, and dailies looks with the media so assistants, editors, and color can track the intended version in Aspect through custom metadata.
