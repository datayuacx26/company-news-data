---
schema_version: "1.0.0"
document_id: "f18970dd0e26e478e5a2aed0c8db4d417f9d2cd04d371350fe91cbeed7fcb79e"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/how-to-fix-retimed-and-speed-ramped-clips-during-conform"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-12T02:47:02.788653+00:00"
fetched_at: "2026-08-12T02:47:05.631497+00:00"
content_hash: "sha256:4c023f5334e400cd5becdab116ac01a5df5ea8787cd1e396c9a63b36cde9f788"
---

# How to Fix Retimed and Speed-Ramped Clips During Conform

The safest conform rule is simple: translate constant speed changes,[bake complex retimes](https://partnerhelp.netflixstudios.com/hc/en-us/articles/21541276559763-Netflix-Conform-Pulls-User-Guide) .


A 50% slow-down, 200% speed-up, reverse shot, or freeze frame often survives an AAF/XML handoff well enough to rebuild in finishing. A sculpted speed ramp with keyframes, optical flow, mixed frame interpolation, nested transforms, and a resize riding on top is a different animal. It's a creative effect, and every application describes that effect differently.


During conform, your job is to get the final timeline matching the approved offline, on the correct source media, without burning days on avoidable mismatches. Retimes are one of the fastest places to lose that time.


## Retimes sit between editorial and finishing


Conforming is the process of[rebuilding the locked edit](https://learn.foundry.com/nuke/Content/timeline_environment/conforming/conforming_sequences.html) against the high-resolution source media, usually original camera negative or finishing-quality transcodes. The edit system defines the sequence: clip order, timeline timecode, source timecode, clip names, reel names, and track structure. Resolve or another finishing system uses that information to relink the edit to the proper media.


That works cleanly when the editorial proxy and the camera original agree on the basics:


- Source timecode
- Clip name or reel name
- Timeline in and out
- Source in and out
- Frame rate interpretation
- Unique filenames or reliable reel metadata


Retimes add another layer, which means the conform no longer asks, “Which source frames appear in this timeline range?” It asks, “Which source frames appear, in what order, at what rate, using which interpolation method, and with which keyframed changes over time?”


A normal conform selects frames in order; a retime changes frame spacing and sometimes frame order. That distinction matters because a simple cut can be wrong by one frame and still be easy to diagnose. A speed ramp can look right at the first frame, drift in the middle, and land wrong at the end. If the shot also has optical flow or a resize, the mismatch may not be obvious until review.


## How AAF and XML communicate speed changes


AAF and XML are[instruction files](https://mixinglight.com/color-grading-tutorials/conform-issues-database-introduction/) , and they don't carry the finished picture unless media is embedded or separately rendered. They describe the timeline so another application can rebuild it.


For retimed clips, that description can include some combination of:


- A constant speed percentage
- A reverse direction flag or negative speed value
- A source range and timeline duration
- Time remap keyframes
- Freeze-frame or frame-hold instructions
- Motion effect metadata
- Frame blending or interpolation hints, depending on the system


AAF/XML can describe retimes, but every NLE has its own model for them.


The same retime data can be interpreted as different timing behavior in different systems. Premiere Pro, Avid Media Composer, Final Cut Pro, and Resolve all support speed changes, but they don't always store the same math, curve behavior, frame sampling, or interpolation choices. Even when an XML contains time remap keyframes, the receiving system may interpret the curve differently. One system’s ease-in/ease-out ramp may become a linear ramp somewhere else. A frame hold may become a zero-speed segment, a duplicate still, or a trimmed freeze. A nested speed effect may flatten unpredictably or not translate at all.


[EDLs are even more limited](https://www.arri.com/en/learn-help/learn-help-camera-system/pre-postproduction/conforming) , and they're useful for simple cut lists and some basic transitions, but they aren't a good carrier for modern variable speed work. If a timeline has meaningful retimes, treat an EDL as a reference or fallback, not the primary description of the effect.


## Why Resolve may not match the offline


Resolve has conform and native retiming tools, including constant and variable speed changes, freeze frames, retime curves, and frame interpolation options such as optical flow and Speed Warp in Studio. But Resolve can only rebuild what it receives and understands.


Common retime failures show up in a few repeatable ways:


- The clip relinks correctly but plays at the wrong speed.
- The first and last frames match, but the ramp timing drifts through the middle.
- A freeze frame becomes a very short still or disappears.
- A nested or compound retime imports as a normal-speed clip.
- Duplicate[clip names or reel conflicts](https://mixinglight.com/color-grading-tutorials/fixing-clip-conflicts-frame-rate-issues-davinci-resolve/) cause the retimed section to link to the wrong take.


The last two are especially painful because they masquerade as retime problems. If editorial cut with proxies and the OCN doesn't share source timecode, reel name, or clip name behavior, Resolve may place the wrong source frames before you even evaluate the retime. Once that happens, adjusting speed won't fix the shot, so you have to fix the conform relationship first.


A useful diagnostic is to separate the problem into two questions: is Resolve using the correct source frames, and is it playing those frames with the correct timing? If the source frames are wrong, solve relinking and reel metadata first. If the source frames are right but the motion is wrong, solve the retime.


## Choose translation or baking shot by shot


You don't need to render every retime out of editorial. Baking everything can create unnecessary media, reduce grading flexibility, and hide source metadata. But trying to translate every speed effect can burn more time than it saves.


Use the creative and technical complexity of the retime to decide.


Retime situation Best handoff Main risk if translated Finishing approach


Constant speed change, such as 50 percent or 200 percent AAF/XML instruction Minor rounding or frame sampling differences Relink to correct source, apply or verify speed, compare head and tail


Simple reverse shot AAF/XML instruction or manual rebuild Source in point may not represent the visible first frame Find the first visible offline frame, then rebuild backward from that frame


Basic freeze frame AAF/XML instruction or short baked section Hold frame may shift, shorten, or disappear Identify the held source frame and create an explicit hold for the correct duration


Keyframed speed ramp Usually bake, unless finishing is rebuilding creatively Curve shape may change between NLEs Match to offline using visual anchors, or use a baked reference as the timing target


Optical flow, Speed Warp, fluid motion, or blended-frame retime Usually bake Motion estimation artifacts and interpolation will not match exactly Use the approved bake, or rebuild in Resolve and get creative approval


Retime inside a nest, compound clip, multicam, or stacked effect Usually bake or flatten with clear notes Nested effect may import as normal speed or with missing layers Render with handles, or provide a flattened reference plus source notes


Hero shot that needs grading latitude and exact timing Hybrid handoff Bake may limit image quality, rebuild may miss timing Bake a reference, then rebuild from OCN in finishing and compare frame by frame


Retimes your team can usually translate:


- Constant speed changes such as 50%, 75%, 125%, or 200%
- Simple reverse shots
- Basic freeze frames without additional effects
- Overcranked slow motion where the camera original supports the intended playback
- Retimes that can be rebuilt quickly and visually matched in Resolve


Your team can usually check these visually and rebuild them without much ambiguity.


Retimes your team should usually bake:


- Variable speed ramps with keyframed acceleration
- Shots using optical flow, Speed Warp, fluid motion, or another motion estimation method
- Retimes inside nests, compound clips, multicam clips, or stacked effects
- Retimes combined with stabilization, reframing, warps, split screens, or graphics
- Any effect where the offline render is the creative source of truth


Prevent a mathematically different speed ramp from becoming a surprise in the grading room. If you want the finishing artist to improve the retime using Resolve’s tools, send enough information to rebuild it. If the exact editorial timing is already approved, render that section deliberately.


## Keep proxy and original timing aligned


A lot of retime conform trouble starts earlier than the AAF/XML export. Offline workflows depend on[proxies and camera originals](https://www.arri.com/en/learn-help/learn-help-camera-system/pre-postproduction/editorial-workflow) matching in the fields used for relinking. That means your proxy workflow needs to preserve timecode, clip name, reel name, and frame rate interpretation in a predictable way.


This is especially important with offspeed footage. A camera may record 50 fps, 60 fps, 100 fps, or 120 fps for a 23.976, 24, or 25 fps project. Editorial may cut the clip already slowed down, or may speed it back up for sync, or may apply an additional retime on top. If the proxy and OCN disagree on how that media is interpreted, the conform can use the wrong frames even when the visible edit looked fine offline.


The finishing team should know how the footage was treated:


- Did the camera shoot it offspeed?
- Did dailies interpret it to the project frame rate?
- Did editorial cut it at normal speed and then slow it in the NLE?
- Did someone slow it in the proxy file itself before editorial?
- Did editorial add a motion effect on top of an already offspeed source?


Those are different workflows, and they can all be valid, but they don't conform the same way. The cleanest version is usually one where dailies preserve source timecode and naming, editorial cuts proxies that match the OCN, and any creative retime is visible as a timeline effect rather than hidden inside a newly exported file with no usable metadata.


## Bring an offline reference into Resolve


A locked offline reference is the fastest way to catch retime problems in Resolve. Import the AAF/XML, relink the media, then load the approved reference as an[offline reference clip](https://documents.blackmagicdesign.com/UserManuals/DaVinci_Resolve_10_Reference_Manual.pdf?_v=1399618800000) , and use it to compare the conformed timeline against the approved offline.


Your team should check retimes at meaningful points inside the shot:


- First visible frame of the retimed segment
- Last visible frame before the next cut
- Action beats such as footfalls, head turns, impacts, flashes, or camera bumps
- Music hits or sync moments
- The start and end of[each speed ramp](https://www.youtube.com/watch?v=C561OeNewME)
- Any freeze-frame boundary


If the first and last frames match but the middle is off, you probably have a ramp-curve mismatch. If the middle timing feels right but the cut exits on the wrong frame, the source range or speed duration may be off. If the whole shot looks like the wrong take, stop retiming and fix the conform.


When head and tail frames match but the middle drifts, the ramp curve likely translated differently. Resolve’s comparison tools make this less subjective. Use the reference as a visual target, then park on specific frames and compare source timecode, clip identity, and motion position. Your goal is to know whether the mismatch is a relink issue, a source range issue, or a retime math issue.


## Repair constant speed changes first


Constant speed problems are usually the fastest to fix manually. Once you link the clip to the correct source media, adjust the clip speed so the source frames line up with the offline.


For a constant retime, you need three facts:


- Timeline duration of the shot
- Correct source start frame
- Correct source end frame or intended speed percentage


If editorial reports the speed percentage and the shot is visually correct at the head, apply that speed and check the tail. If the tail doesn't match, the source start may be wrong or the NLE may have rounded frame sampling differently. If editorial reports the source in/out, fit the source range to the timeline duration and verify against the reference.


Reverse shots need extra care because the correct “start” of the timeline clip is often the later source frame. When a reverse imports incorrectly, find the first visible frame in the offline, locate that same frame in the source, and rebuild the clip backward from there. Don't assume the XML’s source in point is the visible head frame after reversal.


Treat freeze frames as frame-accurate still decisions rather than generic speed effects. Identify the held source frame, create the hold for the correct timeline duration, and compare both sides of the hold against the offline. If a freeze is part of a larger ramp, consider splitting it into its own segment so the hold frame remains explicit.


## Rebuild speed ramps around visual anchors


Variable speed ramps are where manual conform becomes more craft than data entry. If the imported ramp is close but not exact, use the offline reference to find anchor points.


Good anchors are visible events that occur on exact frames:


- A hand touching a surface
- A flash frame
- A face turning to camera
- A beat where motion stops or changes direction


Place speed points around those events and adjust the retime curve until the conformed shot hits the same frames at the same timeline positions as the reference, and then check the motion between anchors. A ramp can technically hit every marker and still feel wrong if the acceleration curve differs, so you need both frame accuracy and motion feel.


Resolve’s retime curve is helpful here because it lets you shape the speed change rather than only setting segment percentages, but remember that you need to match the approved offline. If Premiere, Avid, or Final Cut used a different interpolation curve, copying the visible result matters more than copying the nominal values.


When a ramp uses optical flow or advanced motion estimation, expect visual differences. Resolve’s Optical Flow or Speed Warp may produce cleaner motion than the offline, but it may not match artifacts, warping, or blended frames that were approved. That's a creative decision. If the offline artifacts are part of the approved edit, use the baked render. If your team allows finishing to improve the motion, rebuild it in Resolve and get approval on the new render.


## Render retimed sections when accuracy matters more than flexibility


Baking a retime means rendering the effect into a new media file and cutting that rendered file into the finishing timeline. This is the right move when the retime is an approved visual effect or when the interchange can't represent it reliably.


Baking turns an adjustable retime into rendered picture, ideally with handles for finishing. The render shouldn't create a new conform problem because a bad bake can be worse than a bad XML if it arrives with no handles, no timecode, and a filename like “final_final_speedshot.mov.”


Your team should deliver baked retimes with:


- A high-quality codec appropriate for finishing, such as ProRes, DNxHR, or EXR when needed
- Handles, commonly 12 to 24 frames or the show’s agreed handle length
- Matching resolution, frame rate, color pipeline, and scaling intent
- Source timecode preserved when possible
- Clear filenames that include sequence, shot, version, and retime status
- Notes describing what was baked, such as speed ramp, freeze frame, optical flow, stabilization, or resize
- A reference export showing the baked section in context


If the retimed shot needs grading latitude, avoid baking into a low-quality editorial codec. If the source is log or RAW and finishing can render the retime from camera originals in Resolve, that may be better than asking editorial to bake from proxies. But if the creative speed effect only exists correctly in editorial, editorial should render it from the highest-quality source it can access and your team should document the compromise.


There's also a hybrid approach: editorial bakes a reference retime, while finishing rebuilds the effect from OCN and compares against that bake. This gives the colorist access to better source media while preserving the approved timing target. It takes more time, but for hero shots it's often worth it.


## Communicate retimes as editorial decisions


Mark retimes clearly in the sequence and include notes with the turnover.


A useful retime note is specific without becoming a novel:


- Clip name and timeline timecode
- Type of retime: constant, ramp, reverse, freeze, optical flow
- Speed values if known
- Whether editorial baked the effect or finishing should rebuild it
- Handle length on baked renders
- Any stacked effects that affect the result, such as resize or stabilization


This is also where assistant editors can save the conform. If editorial created a retime inside a nest, multicam, compound clip, or adjustment-layer stack, say so. If editorial rendered and replaced a section, say what source was used. If the XML should ignore the original live effect because the baked file is authoritative, make that clear.


Resolve can conform and retime, but it isn't a mind reader. The more clearly editorial separates edit decisions from baked creative effects, the easier it's for finishing to preserve the edit.


## The real fix is deciding earlier


Retimed clips fail during conform because the workflow treated them like ordinary cuts, and they combine source-frame selection, time mapping, interpolation, and often other visual effects.


For simple speed changes, keep the metadata clean, export AAF/XML, relink in Resolve, and verify against the offline reference. For complex speed ramps, optical flow, nested retimes, and approved timing effects, render with handles and clear metadata. For anything in between, decide based on how exact the motion needs to be and how much flexibility finishing needs.


That decision is the fix. The manual tools matter, but the real win is knowing which retimes should travel as instructions and which should travel as picture.


## FAQ


Retimed clips fail because NLEs don't all describe speed effects the same way. AAF and XML can carry some speed information, but curve behavior, frame sampling, interpolation, freezes, nested effects, and optical flow settings may be interpreted differently in Resolve or another finishing system.


Constant speed changes, simple reverse shots, basic freeze frames, and overcranked slow motion are usually good candidates for translation, as long as the proxy and original media share reliable timecode, reel, clip name, and frame rate metadata. They should still be checked against the approved offline reference.


A speed ramp should usually be baked when its exact timing is already approved, when it uses keyframed acceleration, optical flow, Speed Warp, stabilization, reframing, graphics, nests, compound clips, or stacked effects. Baking is also safer when matching the offline precisely matters more than retaining full grading or retiming flexibility.


First confirm that Resolve is using the correct source media and source frames. If the shot is the wrong take, has the wrong source timecode, or starts from an unexpected part of the clip, fix reel, clip name, timecode, or relinking issues before adjusting speed. If the source frames are correct but the motion timing is wrong, then treat it as a retime problem.


Editorial should provide high-quality renders in an agreed finishing codec, with handles, clear filenames, matching frame rate and resolution, useful timecode where possible, and notes describing the baked effect. The delivery should also identify whether the baked render is the creative source of truth or only a reference for rebuilding from camera originals.


Use structured notes on the media, such as retime type, speed values, handles, optical flow, stabilization, resize, and whether the bake is authoritative. Aspect lets teams add project-specific fields to assets with custom metadata, so retime status travels with the media instead of living only in an email.
