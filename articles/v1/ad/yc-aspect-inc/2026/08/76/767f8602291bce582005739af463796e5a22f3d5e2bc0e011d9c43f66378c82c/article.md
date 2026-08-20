---
schema_version: "1.0.0"
document_id: "767f8602291bce582005739af463796e5a22f3d5e2bc0e011d9c43f66378c82c"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/codec-workflows/guide-to-dnxhr-quality-tiers-lb-sq-hq-hqx-and-444"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T04:20:56.920356+00:00"
fetched_at: "2026-08-05T04:21:00.040924+00:00"
content_hash: "sha256:4e3186f3ae2ad8c113c5ec2184493cc0e23540d588fe55ced5c0552fcd6fa5eb"
---

# Guide to DNxHR Quality Tiers: LB, SQ, HQ, HQX, and 444

If you need a fast answer, pick the lowest DNxHR tier that preserves the decisions still ahead of you. Use LB when the edit is the job. Use SQ when editorial needs better-looking media but you aren't finishing from it. Use HQ for strong 4:2:2 online media. Use HQX when 10-bit color, HDR, or heavy grading matters. Use 444 only when the workflow actually needs 4:4:4 or RGB, usually VFX, titles, graphics, or high-end finishing.


That sounds obvious, but most DNxHR mistakes come from treating the tiers like a generic “good, better, best” slider. They're really workflow choices. The right tier depends on who will touch the file, how many generations it may survive, what storage can play in real time, and whether the mezzanine is temporary or becomes the finishing source.


DNxHR tiers make more sense when they're matched to workflow stages.


## DNxHR is a mezzanine choice, not a delivery choice


DNxHR is Avid’s high-resolution member of the DNx family. Where DNxHD was built around fixed HD frame sizes, DNxHR was introduced for[high-resolution and proxy workflows](https://resources.avid.com/SupportFiles/attach/WhatsNew_MediaComposer_v8.3.pdf) , including UHD, 4K, and other non-HD rasters. It's meant for editing, interchange, rendering, VFX pulls, and finishing pipelines where predictable playback and image quality matter more than tiny files.


It isn't the same job as H.264, HEVC, or AV1. Those codecs are usually delivery or camera acquisition codecs. They can look great, but they aren't always pleasant for editorial systems,[shared storage](https://kb.avid.com/pkb/articles/en_US/white_paper/NEXIS-Bandwidth-for-DNxHR-Material) , frame-accurate interchange, or multi-generation post work. DNxHR trades file size for consistency. That trade is often worth it when several people and systems need to handle the same media without decoding headaches.


The[key DNxHR tiers](https://kb.avid.com/pkb/sfc/servlet.shepherd/document/download/069VP00000a9H6ZYAU?operationContext=S1) are:


- LB: Low Bitrate
- SQ: Standard Quality
- HQ: High Quality
- HQX: High Quality Extended
- 444: Best Quality for 4:4:4 and RGB-style workflows


Avid’s naming and[bandwidth specs](https://kb.avid.com/pkb/articles/en_US/Knowledge/DNxHR-Codec-Bandwidth-Specifications) define different data rates per tier, frame size, and frame rate. So “DNxHR HQ” isn't one bitrate everywhere. UHD 23.976, UHD 59.94, DCI 4K, and 2K all have different bandwidths. As frame size and frame rate climb, the storage cost climbs with them.


## The practical difference between the tiers


The tiers separate three things that matter in post: compression level, color precision, and chroma sampling. Those aren't interchangeable.


A higher bitrate can reduce compression artifacts, but it doesn't magically create missing color information from a 4:2:0 camera original. A 10-bit tier can preserve smoother gradients through grading, but it doesn't make an 8-bit source truly 10-bit. A 4:4:4 tier can protect RGB graphics and key edges, but it's often wasted on standard 4:2:2 editorial footage.


Here is the practical read of each tier.


DNxHR tier Relative visual quality and data rate Color precision and sampling Best fit Avoid using it as


LB Lowest quality and lightest files; built for easy playback Typically 8-bit 4:2:2 Offline editorial, proxies, multicam, remote cutting Final evaluation, grading, VFX, archive masters


SQ Better-looking editorial media with moderate storage cost Typically 8-bit 4:2:2 Higher-quality offline, nearline editorial, internal review, mixed-source editorial mezzanine Default finishing master when color, graphics, or archive quality matters


HQ High-quality files with much higher bandwidth than SQ Typically 8-bit 4:2:2 SDR online, strong 4:2:2 editorial masters, high-quality renders Log, HDR, or heavy grading workflows that need 10-bit precision


HQX High-quality 10-bit files with heavier storage and bandwidth needs[10-bit 4:2:2](https://academysoftwarefoundation.github.io/EncodingGuidelines/EncodeDNXHD.html) Grading, HDR, log sources, safer online and finishing workflows Routine offline proxies that will[relink to camera originals](https://resources.avid.com/SupportFiles/attach/HighRes_WorkflowsGuide.pdf)


444 Highest tier and largest files, especially at UHD and 4K 4:4:4 or RGB-oriented workflows VFX plates, titles, graphics, high-end finishing, true 4:4:4 masters Ordinary 4:2:0 or 4:2:2 editorial footage with no RGB or 4:4:4 requirement


## DNxHR LB: proxies and offline cutting


LB is the low-bitrate tier. Use it when the goal is responsive editing, not final image evaluation.


Good LB uses include:


- Offline editorial for high-shoot-ratio projects
- Remote assistant workflows where upload size matters
- Multicam cutting where storage bandwidth is the bottleneck
- Rough assemblies, story edits, producer review cuts, and selects
- Temporary proxies that will relink to camera originals or higher-quality mezzanine media


LB is supposed to be light, and that's the point. If you're cutting a feature, doc series, reality show, or multicam concert with many streams, LB can make the difference between a working timeline and a day of dropped frames.


The failure mode is using LB beyond its job. Don't make color, noise reduction, keying, beauty, or finishing decisions from LB media unless everyone understands that it's only a proxy view. Low-bitrate compression can hide noise, smear texture, soften edges, and mislead people about whether a shot is actually usable.


For LB workflows, relink discipline matters. Keep source file names, tape/reel/source IDs, timecode, frame rate, and raster metadata clean. If the proxy can't reliably reconnect to the source, the storage you saved won't matter.


Proxy savings only matter when the proxy relinks cleanly to the source.


## DNxHR SQ: standard editorial media with a quality cushion


SQ is the middle-ground tier. It's useful when editorial media needs to look respectable, play reliably, and not consume the storage of HQ or HQX.


Good SQ uses include:


- Offline or “nearline” editorial where proxies need to look better than LB
- Long-form cutting where storage still matters
- Internal review exports or editorial turnovers where high-end finishing isn't happening from the file
- Projects with moderate grading needs and no serious VFX extraction
- Facilities standardizing on one editorial mezzanine for mixed camera sources


SQ is often the tier people wanted when they chose HQ out of habit. If your team will conform back to camera originals, SQ may be enough for editorial. It gives assistants and editors a nicer image than LB without pushing shared storage as hard as HQ.


The risk with SQ is assuming “standard” means “safe for final.” Sometimes it's, but often it isn't the best choice for a master-quality intermediate. If the file will become the source for color, online, graphics, final text, or archive, you need to think harder. SQ can be a good editorial codec, but it isn't automatically a finishing codec.


## DNxHR HQ: high-quality 4:2:2 online media


HQ is the practical high-quality tier for many 4:2:2 workflows. It's a strong choice when your team may use the mezzanine for online editorial, high-quality renders, and finishing prep, but the project doesn't need HQX or 444.


Good HQ uses include:


- Online editorial in SDR 4:2:2 workflows
- High-quality transcodes from 8-bit or 10-bit sources when the finishing plan doesn't require 10-bit preservation
- Master-quality intermediates for projects with moderate grading
- Broadcast-style pipelines where 4:2:2 quality is expected
- Graphics-light shows where VFX pulls aren't the main concern


HQ can be a sensible mezzanine when the camera originals are highly compressed and the team wants a smoother post format. It can also be the right render codec for sequences that need to move between systems without becoming a heavily compressed delivery file.


The thing to watch is bit depth. If you're working with log, HDR, wide-gamut material, or anything headed into a serious grade, HQ may not be the right ceiling. You may want HQX to preserve 10-bit 4:2:2 precision through the pipeline.


## DNxHR HQX: 10-bit 4:2:2 for grading, HDR, and safer finishing


HQX is the “extended” high-quality tier. In normal post language, this is the tier you reach for when 10-bit 4:2:2 matters.


Good HQX uses include:


- UHD or 4K finishing where your team may grade the mezzanine
- HDR workflows
- Log camera sources where banding risk matters
- Online media that may survive multiple renders
- VFX pulls when the source and pipeline are 4:2:2 rather than RGB or 4:4:4


HQX is frequently the safest “don’t make me regret this later” DNxHR choice for serious finishing, especially if the source is 10-bit 4:2:2 and the final doesn't require 4:4:4. It costs more storage than SQ and often similar or more than HQ depending on the exact raster and frame rate, but it protects color precision in a way HQ doesn't.


The most common overspec is using HQX for every offline transcode because someone heard “10-bit is better.” Yes, it's better for the right job. But if the media is only for cutting and will relink to source for finish, HQX can punish storage, sync, upload, and remote review workflows for no visible benefit during editorial.


## DNxHR 444: for RGB, 4:4:4, VFX, and high-end finishing


444 is the top DNxHR tier. It's designed for 4:4:4 and RGB-oriented workflows, not ordinary editing convenience.


Good 444 uses include:


- VFX plates where edges, keys, and color channels need maximum preservation
- Graphics, titles, animation, and design renders
- Film-style DI pipelines that expect 4:4:4 or RGB intermediates
- High-end finishing where the mezzanine is a true master source
- Round-trips from compositing tools when the project’s color pipeline justifies it


444 is expensive, and at UHD and 4K frame sizes, high-quality DNxHR tiers can move into very high bandwidth territory, and 444 is the tier most likely to expose weak storage, slow networks, and underplanned transfers. If the source is 4:2:0 or 4:2:2 camera material and there's no RGB/4:4:4 finishing requirement, 444 often creates larger files without creating more real picture information.


Use 444 when there's a reason tied to the pipeline: compositing, keying, RGB graphics, high-end color, or a delivery spec. Don't use it just because it's the highest option in the menu.


444 is best reserved for pipelines that need clean channels and edges.


## Data rate is where the decision becomes real


DNxHR tiers are easy to discuss abstractly until someone tries to play eight streams from shared storage. Avid publishes bandwidth specifications because the tier name alone isn't enough. Data rate changes with raster and frame rate.


As a rough planning pattern, for UHD or 4K at common 23.976 or 24 fps rates:


- LB sits in proxy-friendly territory.
- SQ is several times heavier than LB.
- HQ and HQX are much heavier again.
- 444 can be roughly double the high-quality 4:2:2 tiers.


At higher frame rates, expect the bandwidth to scale up. UHD 59.94 will be much more demanding than UHD 23.976. Multicam multiplies the problem again. A single stream that plays fine from local SSD may not be fine when eight editors are pulling multiple streams from shared storage while assistants are transcoding in the background.


For planning, estimate both storage capacity and sustained throughput. Capacity tells you whether the show fits. Throughput tells you whether the show plays.


Capacity is about fitting media; throughput is about playing it smoothly. A simple storage conversation should include:


- Runtime hours, including handles and alternates
- Frame size and frame rate
- Number of simultaneous streams in editorial
- Number of editors, assistants, and finishing systems accessing media
- Whether media lives on local drives, NAS, SAN, or cloud-mounted storage
- Whether the DNxHR files are temporary proxies, online media, or archive masters


The takeaway is simple: don't pick HQX or 444 until you know the storage path can actually sustain the work. “It fits on the drive” isn't the same as “it plays in the room.”


## Match the tier to the project stage


DNxHR decisions get easier when you separate the stage of the project from the final quality ambition.


For offline editorial, LB is usually the first candidate. If the show has a lot of visually subtle material and producers need nicer review images, SQ can be a good upgrade. HQX for offline only makes sense when offline and online are effectively the same process, or when reconform risk is higher than storage cost.


For online editorial, HQ and HQX are the serious candidates. HQ works for high-quality SDR 4:2:2 workflows where 8-bit precision is acceptable. HQX is better when the media is 10-bit, log, HDR, or expected to handle heavier color work.


For VFX, the answer depends on what the VFX team needs to receive and return. If the job is basic cleanup from 4:2:2 editorial media, HQX may be enough. If the job involves keying, CG integration, graphics, plates, or RGB compositing, 444 may be appropriate. Your VFX team should decide through the pull spec.


For archive, avoid assuming the biggest DNxHR file is the best archive. Tie archive to the long-term source of truth. That may be camera originals plus project files, a graded master, a textless master, a final mezzanine, or all of the above. HQX is often a reasonable high-quality archive mezzanine for 4:2:2 masters. 444 makes sense when the master is genuinely 4:4:4/RGB or the delivery contract asks for it.


## DNxHR versus DNxHD in Avid projects


Avid workflows still make a practical distinction between DNxHD and DNxHR. DNxHD is for HD frame sizes, while DNxHR is the high-resolution family built to support larger and more flexible rasters.


That matters when you're working in Media Composer. In HD projects, transcode menus can be limited to HD formats, and high-resolution sources may transcode to HD rather than offering the high-res or proxy-raster options you expected. As a common workaround, you can[temporarily change the project raster](https://kb.avid.com/pkb/articles/en_US/how_to/Media-Composer-Software-High-Resolution-Workflow-Tips) to a larger-than-HD format, create the needed high-resolution or proxy transcodes, and then switch the project back.


That affects how assistants build a clean offline/online path. If the project raster, proxy raster, and relink plan aren't aligned, you can end up with media that plays but doesn't conform the way you expected.


Also be careful with resize choices. Baking a resize into a transcode isn't the same as preserving source size metadata and using a reframing tool such as FrameFlex. If editorial needs access to the source raster for reframing, stabilization, or online conform, don't casually bake away that information during proxy creation.


## Wrapper and interchange choices still matter


DNxHR is the codec, and your team may wrap the file as[MXF or MOV](https://lowepost.com/courses/blog/the-difference-between-dnxhr-and-prores-codecs-r4/) depending on the workflow. In Avid-native environments, MXF is often the cleanest path. For interchange with tools like Resolve, Premiere, After Effects, finishing systems, or VFX vendors, MOV may appear more often.


The codec tier doesn't solve wrapper, metadata, or color management issues by itself. An HQX file with ambiguous range, wrong color tags, or missing source timecode can still create a bad handoff.


For turnovers, include the details people actually need:


- DNxHR tier
- Wrapper, such as MXF or MOV
- Raster and frame rate
- Color space and transfer function when relevant
- Legal or full range expectations
- Audio layout and sample rate
- Source timecode and reel/source metadata
- Handles for VFX or finishing pulls


Those details are especially important when DNxHR files are moving between editorial, color, VFX, and archive systems. The tier only tells part of the story.


## Common under-spec mistakes


Under-speccing usually happens when a proxy decision becomes a finishing decision.


The most common problems are:


- Teams use LB media for judging focus, noise, texture, or beauty work
- Teams use HQ where 10-bit HQX should have protected gradients, log images, or HDR work
- Teams send 4:2:2 media to VFX when the shot needed 4:4:4 or RGB preservation
- Teams create proxy transcodes without reliable timecode, source IDs, or relink metadata
- Teams bake resizes into editorial media when online needs the original raster


The fix is to label the purpose of the DNxHR media when your team creates it. “DNxHR SQ editorial proxy, relink to camera original for finish” is a very different object from “DNxHR SQ final mezzanine.” Same tier, different risk.


## Common over-spec mistakes


Over-speccing is just as real, and it burns storage, slows copies, makes remote workflows painful, and can reduce the number of streams editorial can play.


Watch for these patterns:


- Teams make HQX proxies for a show that will conform from camera originals
- Teams export 444 from 4:2:0 or 4:2:2 sources with no VFX or RGB requirement
- Teams use HQ or HQX for every review file when H.264 would have been the delivery format
- Teams apply one universal mezzanine tier to every department, even though editorial, VFX, color, and archive have different needs
- Teams create high-frame-rate UHD media at premium tiers without checking shared storage bandwidth


Choose tiers by stage. Editorial can be LB or SQ. Online can be HQ or HQX. VFX can request HQX or 444 per shot. Archive can preserve the actual master requirement.


## A practical rule for choosing


If you're building a DNxHR workflow from scratch, start with this decision path.


Use LB when the file is only for cutting and your team will relink it later. Use SQ when editors and producers need a better-looking offline image but your team will finish elsewhere. Use HQ when you need robust 4:2:2 online media and 8-bit precision is acceptable. Use HQX when you need 10-bit 4:2:2 for grading, HDR, log sources, or safer finishing. Use 444 when the pipeline is truly 4:4:4 or RGB.


Then test the actual path with real footage. Don't test with a five-second clip on one workstation. Test a representative scene with the expected frame rate, audio, effects, multicam load, shared storage path, and relink or turnover step. If that test fails, the tier may not be the problem. It may be storage bandwidth, project raster, wrapper choice, metadata, or color management.


DNxHR is useful because it gives you predictable options across a wide range of post workflows. The point is to pick the tier that preserves what still needs to be decided while staying light enough for the team to work.


## FAQ


Use DNxHR LB when the files are only proxies for cutting and will relink to camera originals or higher-quality media later. Use DNxHR SQ if editors, producers, or directors need a cleaner-looking offline image but the project will still finish from another source.


HQX is better when 10-bit 4:2:2 precision matters, such as log footage, HDR, heavy grading, gradients, skies, beauty work, or finishing from the mezzanine. HQ can be enough for strong SDR 4:2:2 online workflows where 8-bit precision is acceptable and the project doesn't need the extra color headroom.


Use DNxHR 444 when the workflow actually needs 4:4:4 or RGB preservation, such as VFX plates, keying, titles, animation, graphics, high-end compositing, or premium finishing. If the source is ordinary 4:2:0 or 4:2:2 camera footage and there's no RGB or 4:4:4 requirement, HQX is usually the more practical high-quality choice.


It can preserve the image more safely through later processing, but it doesn't create missing information. An 8-bit source doesn't become true 10-bit, and a 4:2:0 source doesn't gain real 4:4:4 color detail. Higher DNxHR tiers are most useful when they prevent additional damage during editing, grading, VFX, or rendering.


DNxHR data rate changes by tier, frame size, and frame rate. UHD or 4K at 59.94 fps needs far more bandwidth than 23.976 fps, and multicam multiplies the load again. Plan for both total capacity and sustained throughput, because media can fit on a drive but still fail to play smoothly over shared storage.


Keep the DNxHR media in one shared workspace and let editors mount the project files directly instead of copying drives. Aspect lets the team cut from the same media, which helps prevent duplicate LB, SQ, or HQX transcodes from drifting out of sync in a shared cloud filespace.
