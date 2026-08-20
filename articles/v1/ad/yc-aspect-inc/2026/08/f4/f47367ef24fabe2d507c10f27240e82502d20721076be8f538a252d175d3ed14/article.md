---
schema_version: "1.0.0"
document_id: "f47367ef24fabe2d507c10f27240e82502d20721076be8f538a252d175d3ed14"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/camera-workflows/how-to-match-red-multicam-frame-rates-for-slow-motion"
published_at: "2026-08-08T00:00:00+00:00"
first_seen_at: "2026-08-09T21:00:56.600033+00:00"
fetched_at: "2026-08-09T21:00:58.529020+00:00"
content_hash: "sha256:65d3173b9e50e781dba6b92dec77604c12435d8382b20873dbe2b339cb1c013a"
---

# How to Match RED Multicam Frame Rates for Slow Motion

The safest rule is simple: pick the timeline frame rate first, then decide which camera angles are meant to stay real-time and which are meant to become native slow motion. Don't start by trying to make every RED clip “match” at import. A 120 fps clip can be correct in more than one way depending on intent, because it can be a real-time 120 fps recording that you may slow down later, or it can be overcranked footage that should immediately play back at 23.976, 25, or 29.97.


You need to make that decision before you build the multicam. Multicam relies on duration and sync relationships, and slow motion changes duration. If one angle is playing real-time and another angle is already stretched 5x, those clips are no longer describing the same moment over the same amount of timeline time.


## The two frame rates that matter on RED


RED cameras separate the idea of capture from playback.


The[recording frame rate](https://docs.red.com/955-0225/955-0225_V2.0+Rev-A+RED+PS,+V-RAPTOR+%5BX%5D+8K+VV+Operation+Guide/Content/4_Menus/b_ProjSet/RecFrameRate.htm) , sometimes called the capture frame rate or sensor frame rate, is how many frames the camera records per second. The project time base is the rate at which that footage is intended to play back. If the recording frame rate is higher than the project time base, the camera is overcranking and the clip plays as slow motion. If the recording frame rate is lower than the project time base, it plays as fast motion.


So a clip recorded at 96 fps with a 24 fps project time base is usually a 24 fps clip containing 96 captured frames per second of action, played back at one quarter speed. That's native slow motion.


The important RED-side settings are:


- Project time base: the intended playback rate, often 23.976 for cinema-style work, 25 for PAL regions, or 29.97 for NTSC/broadcast-style work.
- Recording frame rate: the capture rate, such as 48, 60, 96, 120, or higher depending on the camera, sensor format, REDCODE, and other settings.
- Sensor format and resolution: these affect the maximum frame rate available.
- REDCODE compression: crews often use[higher compression ratios](https://support.red.com/hc/en-us/articles/48287484445331-RED-Camera-Launch-Sequence-DSMC2-RANGER) when a higher frame rate is required or storage is limited.
- Audio and recording mode: off-speed and special recording modes can affect whether usable production audio is captured.


The mistake is treating “120 fps” as the edit rate. For the edit, the useful question is: should this clip occupy real time or slow-motion time?


The same high-frame-rate clip can occupy normal event time or expand into slow-motion time.


## Choose the timeline rate, not the fastest camera rate


In most jobs, set the multicam timeline to the delivery or edit base rate, not the highest capture rate on set. If the show is 23.976, build the timeline at 23.976. If the show is 25, build it at 25. If the broadcast deliverable is 29.97, build there unless post has a specific reason to cut elsewhere.


This keeps titles, turnovers, timecode references, exports, review files, and audio workflows predictable. It also keeps the assistant editor from having to explain why the timeline is 59.94 when the whole show is being finished at 23.976.


A typical RED multicam slow-motion setup might look like this:


- A camera: 23.976 project time base, 23.976 recording frame rate, normal sync sound.
- B camera: 23.976 project time base, 48 recording frame rate, 50 percent slow motion.
- C camera: 23.976 project time base, 96 recording frame rate, 25 percent slow motion.
- Multicam for dialogue or continuous performance: real-time angles only, unless your team retimes the slow cameras back to real-time for sync.
- Slow-motion selects: use overcranked angles as inserts, cutaways, or dedicated slow-motion sections.


That last distinction matters because a 96 fps angle playing at 23.976 isn't a normal synced multicam angle for a two-minute performance. It's a slow-motion version of that performance and will run roughly four times longer than the real-time cameras.


## Native slow motion versus retimed slow motion


There are two common ways slow motion enters the edit, and they behave differently.


The camera creates native slow motion because the RED records more frames than the project time base, and the file metadata tells the NLE to play those frames at the project rate. Motion is smooth because every playback frame comes from a real captured frame, up to the limits of the chosen speed. A 96 fps recording played at 23.976 gives you about 25 percent speed. A 48 fps recording played at 23.976 gives you about 50 percent speed.


You create retimed slow motion in the NLE. A clip that plays real-time in the edit is slowed down after the fact. If there are enough source frames, the NLE can use those real frames. If not, it has to repeat frames, blend frames, or synthesize motion using optical flow or another interpolation method.


For RED multicam, the cleanest workflow is to label the intent early:


- Real-time coverage: keep duration matched to actual event time.
- Native slow-motion coverage: allow the clip to play longer than real time using the RED project time base.
- Flexible high-frame-rate coverage: keep it real-time for sync, then slow down selected edits later.
- Problem footage: clips where the camera project time base doesn't match the intended final time base.


The practical consequence is straightforward: native slow-motion clips are great for beauty shots, action details, sports, music-video inserts, product shots, and emotional cutaways. They aren't automatically great as synced multicam angles for continuous live action unless you first convert them to real-time speed for the multicam.


## Building a multicam when angles have different frame rates


A multicam clip wants all angles to represent the same clock time. If camera A records a clap at 01:00:00:00 and the subject walks across frame for ten seconds, every real-time angle should cover that same ten seconds. If camera B is already playing native slow motion, that same walk might last forty seconds in the browser. Syncing those two as if they're equivalent will create confusion immediately.


There are three practical patterns that work.


Multicam pattern Best use case How to treat high-frame-rate RED clips Main risk


Real-time multicam with slow motion later Continuous sync work such as concerts, interviews, unscripted scenes, live events, and sports coverage Convert or retime overcranked clips to real-time duration before building the multicam, then slow selected shots later If a native slow-motion clip stays slow inside the multicam, it will not match the real-time angles


Normal-speed multicam plus separate slow-motion selects Scenes where the editor needs reliable angle switching, but also wants beauty shots, inserts, or cutaways Keep real-time cameras in the multicam and organize overcranked clips separately at native slow-motion playback Slow-motion material may be harder to find if bins and naming do not clearly label intent


Dedicated slow-motion multicam Multiple cameras intentionally covering the same action at the same slow-motion playback relationship Use angles with matching overcrank relationships, or manually retime them so the same action has matching duration Mixing 48 fps and 96 fps slow-motion angles without adjustment makes the same action run for different lengths


The first pattern is a real-time multicam with optional slow motion later. Use all angles at real-time duration. For high-frame-rate RED clips that import as native slow motion, speed them up to match real-time before building the multicam. For example, a 96-over-23.976 clip needs to run at about 400 percent to match real time. Cut the multicam normally, then apply slow motion to selected edits after flattening, committing, or otherwise separating the chosen angle from the multicam structure.


This is the best pattern for concerts, interviews, unscripted scenes, live events, sports coverage with continuous sync, and anything where the editor needs normal angle switching.


The second pattern is a normal-speed multicam plus separate slow-motion selects. Build the multicam from synced real-time cameras only. Keep overcranked RED clips in a separate bin or stringout at their native slow-motion playback. The editor cuts the multicam for the scene, then uses slow-motion clips as inserts where sync continuity is less important.


This is often the cleanest assistant-editor workflow because it avoids forcing slow-motion material into a structure that doesn't need it.


The third pattern is a dedicated slow-motion multicam. This only works cleanly when you intend the relevant cameras to play back at the same slow-motion rate, or when you manually conform them into matching slow-motion durations. For example, two RED cameras both recording 96 fps against a 23.976 project time base can form a useful slow-motion multicam because they share the same playback relationship. But mixing one 48 fps slow angle and one 96 fps slow angle in the same slow-motion multicam means the same action will play at different durations unless you retime one of them.


The takeaway: multicam sync is about shared time. Slow motion deliberately changes time, so decide which side wins for each group of clips.


Multicam sync depends on shared real time; native slow motion changes the clip’s duration.


## Interpreting RED footage correctly in the NLE


Most major NLEs can work with R3D files directly, including Premiere Pro, DaVinci Resolve, Final Cut Pro, and Avid workflows depending on the exact setup. The frame-rate metadata usually comes through, but assistants still need to inspect clips rather than assuming the bin column tells the whole story.


For RED media, look for both the capture/recording rate and the playback/project rate. A clip may have been captured at 120 fps but play back at 23.976. In the edit, that clip behaves like a 23.976 slow-motion clip, not like a 120 fps sequence.


In Premiere Pro, import RED camera cards through the[Media Browser](https://support.red.com/hc/en-us/articles/360059576833-KOMODO-Recommended-R3D-Workflow-for-Adobe-Premiere-Pro) rather than dragging individual file chunks from the filesystem. R3D clips can be split into 4 GB portions during recording, and proper card-level import helps the application index them as unified clips.


In Resolve, set project and[Camera RAW preferences](https://www.youtube.com/watch?v=a3i3JPU1iLs) before bringing in large batches when possible, then confirm how Resolve decodes and interprets the RED clips. Resolve is commonly used for R3D workflows and gives you strong access to RED RAW settings, but timeline frame rate decisions can become harder to change once the project is underway.


In Final Cut Pro, RED RAW settings applied to[original R3D clips](https://www.apple.com/final-cut-pro/docs/RED_Workflows_with_Final_Cut_Pro_X.pdf) can update reference clips such as synced clips, compound clips, and multicam clips. That's useful, but it also means your team should be deliberate about when they adjust RAW settings versus when they create editorial structures. Avoid creating unnecessary optimized or proxy media at import if the workflow depends on later RAW adjustments.


Across NLEs, the practical inspection points are:


- Does the clip duration match real time or slow-motion time?
- Does the playback frame rate match the show timeline?
- Does the metadata show a higher recording frame rate than project time base?
- Does the clip include usable audio?
- Are proxies, transcodes, and camera originals using the same starting timecode?
- Are high-frame-rate clips labeled by intent, not just by fps?


If a 120 fps clip is supposed to be slow and it plays in real time, conform or interpret it to the timeline rate. If a 120 fps clip is supposed to sync with real-time cameras and it plays slow, apply a speed change for the multicam version.


## Keeping audio sync when frame rates differ


Audio is where frame-rate mistakes become expensive. Picture can look “close enough” for a while, but audio drift exposes a bad interpretation quickly.


Start with this assumption: production sync sound belongs to real-time recording, not to overcranked slow motion. RED support guidance for missing audio points directly at[varispeed and special recording modes](https://support.red.com/hc/en-us/articles/217961428-REDCINE-X-PRO-Troubleshooting) as things to disable when your team expects audio. In other words, if the sensor frame rate differs from the project time base, don't assume the camera audio will be present or useful, so confirm it.


Stable sync audio belongs with real-time footage, while slow-motion picture is cut visually against it. For dialogue, performance, and event coverage, use a stable real-time audio source:


- External recorder running at the production sample rate.
- Camera audio from a real-time camera.
- Timecode sync or jammed timecode across cameras and audio devices.
- Slate or waveform sync as a fallback.
- Clear notes identifying which RED angles are off-speed.


When you use the high-frame-rate RED angle as native slow motion, its audio usually shouldn't drive sync. Use the real-time audio bed and cut the slow-motion picture against it creatively. If you need a slow-motion shot to line up with a specific beat, clap, impact, or lyric, align the visual event to the real-time audio in the timeline. Don't try to make the whole slow-motion clip maintain sync over its full duration.


Be careful with frame-rate override tools. REDCINE-X PRO has frame-rate override features, but[changing the frame rate](https://docs.red.com/955-0004_v42/REV-A/HTML/955-0004_V42%20Rev-A%20%20%20RED%20PS,%20REDCINE-X%20PRO%20Operation%20Guide/Content/5_AdjustClips/FrameRateOverride.htm) can remove audio from the clip. That isn't a small side effect if your editorial team relies on scratch tracks or embedded camera audio. If you need to change playback interpretation, preserve the original media and keep any audio sync work traceable.


A particularly dangerous case is footage recorded with the[wrong project time base](https://docs.red.com/955-0004/REDCINE-XProOperationGuide/Content/10_Troubleshoot/WrongFPS.htm) . For example, if a camera recorded something with a 23.98 project time base but the final needs to be 29.97, a direct export that simply changes the rate can speed up both picture and audio and create sync problems. Your team needs proper standards conversion for that kind of conversion, not a casual reinterpret.


## Handling mixed 23.976, 25, 29.97, 50, 59.94, and 120


Mixed-rate jobs are common now: RED A-cam at 23.976, drone at 59.94, action camera at 119.88, broadcast source at 29.97, archive at 25. The decision tree is still the same, but you need to separate “different acquisition standard” from “intentional slow motion.”


There are a few common cases:


- 47.952 or 48 captured for 23.976 or 24 playback: usually half-speed native slow motion.
- 59.94 captured for 23.976 playback: often 40 percent speed if you conform it, or real-time with frame sampling if you leave it as normal 59.94.
- 50 captured for 25 playback: usually half-speed native slow motion.
- 119.88 captured for 23.976 playback: usually 20 percent speed if you conform it.
- 29.97 source in a 23.976 show: usually a standards/conversion issue, not automatically slow motion.
- 25 source in a 23.976 show: usually a standards/conversion and audio pitch/sync issue, not automatically slow motion.


Don't conform everything to the timeline rate just because the timeline is 23.976. Conforming changes duration, which is exactly what you want for native slow motion, and exactly what you don't want for normal-speed documentary, broadcast, or archive sources.


For non-RED sources, variable frame rate can also cause trouble, especially with phones, screen recordings, and some action cameras. RED R3D isn't usually the source of variable-frame-rate weirdness, but a multicam project may contain those files alongside RED. Normalize those sources before relying on waveform or timecode sync.


## Proxies and transcodes need the same timing story


Proxy workflow can hide frame-rate problems until conform. If the proxy is interpreted one way and the R3D original is interpreted another way, the edit may relink with different durations, different start frames, or different timecode behavior.


Proxy and original media need matching timing so relink doesn't reveal drift or mismatched duration. For RED multicam slow motion, your proxy specs should carry the same timing as the source used for cutting. If the native R3D plays as 23.976 slow motion, the proxy should also play as 23.976 slow motion with matching duration and timecode. If you create a real-time version of a high-frame-rate clip for multicam sync, name and track it as a derived real-time version so nobody mistakes it for the native slow-motion source.


Keep these versions clearly separated:


- Camera original R3D.
- Native slow-motion proxy or transcode.
- Real-time retimed proxy or transcode for multicam.
- Flattened or committed retimes.
- Color turnover media or XML/AAF references.


R3D workflows also use metadata and sidecar files for non-destructive adjustments. That's a strength of the format, but it means your team needs to know whether a change lives in metadata, a proxy, a rendered transcode, an NLE speed effect, or a source interpretation override.


## Failure modes that show up in the edit


Most frame-rate problems are visible if you know what to look for. They usually show up before finishing, but only if your assistants test the actual multicam workflow instead of only checking that files import.


Common failure modes include:


- A high-frame-rate angle drifts out of sync because it's playing native slow motion inside a real-time multicam.
- A slow-motion clip plays real-time because the NLE interpreted the capture frame rate instead of the intended playback rate.
- A clip becomes longer after relink because the proxy and R3D were conformed differently.
- Embedded camera audio disappears after a frame-rate override.
- A 23.976 to 29.97 conversion speeds up picture and audio instead of doing a proper frame-rate conversion.


Document intent and make the NLE match that intent.


## A practical handoff note for the post team


For every RED roll or camera card, your editorial team should be able to answer four questions: what was the project time base, what was the recording frame rate, should the clip play real-time or slow, and where does sync audio come from?


That can be as simple as bin columns and clip naming. For example:


- ` A001_CamA_23.976_RT`
- ` B014_CamB_96over23.976_NATIVE_SLOMO`
- ` C022_CamC_120_REALTIME_FOR_MULTICAM`
- ` D003_59.94_SOURCE_CONVERT_TO_23.976`


The shared understanding matters more than the exact naming style. Editors shouldn't have to guess whether a 120 fps shot is supposed to be a slow-motion insert or a normal-speed angle with extra frames available for later.


The right RED multicam frame-rate workflow is a set of deliberate choices: timeline rate based on delivery, RED playback based on project time base, multicam sync based on real-time duration, and slow motion applied only where it belongs. Once those choices are explicit, mixed RED frame rates become manageable instead of mysterious.


## FAQ


Usually no. Set the timeline to the editorial or delivery frame rate, such as 23.976, 25, or 29.97. A 120 fps RED clip is normally a high capture rate, not the edit rate. Decide whether that clip should play as native slow motion or be retimed to real-time before adding it to a multicam.


A native slow-motion clip has a longer playback duration than the real-time event it recorded. For example, 96 fps captured for 23.976 playback runs about four times longer than real time. If you place that angle in a real-time multicam without retiming it, it may line up at the slate but drift immediately because it's no longer representing the same clock time as the other cameras.


Recording frame rate is how many frames the camera captures per second. Project time base is the rate at which those frames are intended to play back. If a RED camera records 96 fps with a 23.976 project time base, the clip is intended to play back as smooth slow motion at roughly 25 percent speed.


Don't assume it will be available or usable. Off-speed and varispeed recording can affect embedded camera audio, and some modes may not record audio as expected. For sync sound, use an external recorder, a real-time camera angle, timecode sync, or a slate and waveform reference. Treat overcranked picture as a visual source unless production confirms otherwise.


Proxies should match the timing intent of the source used in editorial. If the R3D plays as native 23.976 slow motion, the proxy should have the same slow-motion duration and timecode relationship. If you make a separate real-time version for multicam sync, label it clearly as a derived retimed version so it isn't confused with the native slow-motion source.


Don't rely on fps alone. Use naming, bin columns, and shared metadata fields for intent, such as native slow motion, real-time for multicam, or standards conversion needed. Aspect can help teams carry those intent labels with custom metadata so assistants, editors, and post supervisors are reading the same timing story.
