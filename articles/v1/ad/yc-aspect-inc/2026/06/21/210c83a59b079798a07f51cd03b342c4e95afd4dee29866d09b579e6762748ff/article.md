---
schema_version: "1.0.0"
document_id: "210c83a59b079798a07f51cd03b342c4e95afd4dee29866d09b579e6762748ff"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/camera-workflows/braw-multi-cam-sync-in-davinci-resolve"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-24T08:04:34.272722+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:ac00ec4a1e9ed97f4eb06afec29f025d77d63fc17ac159bebbd735953bf10d79"
---

# How to Sync BRAW Multicam Footage in DaVinci Resolve

When a BRAW multicam shoot has real timecode, use that as the sync source. Without it, waveform sync should only be used when every camera recorded[usable scratch audio](https://www.steakunderwater.com/VFXPedia/__man/Resolve18-6/DaVinciResolve18_Manual_files/part700.htm) . That choice determines whether Resolve can build the group from source metadata or whether an assistant editor must inspect and repair angles by hand.


BRAW fits this workflow because DaVinci Resolve reads Blackmagic RAW directly, exposes camera metadata, and lets you carry[RAW controls into the grade](https://documents.blackmagicdesign.com/UserManuals/DaVinci-Resolve-18-Colorist-Guide.pdf?_v=1679295612000) . The parts that still need planning are sync discipline, mixed frame rates, camera naming, audio routing, and metadata consistency between the assistant editor, editor, and colorist.


## Decide how the group will sync


Resolve gives you several ways to build a multicam clip, but for real camera workflows the practical choices are usually timecode, waveform, or manual sync using a slate or visual event.


Use timecode when cameras and audio recorders were jammed or otherwise recorded matching timecode. In this mode, Resolve isn't guessing because it reads source timecode and lines up matching moments. On a timecode-equipped multicam shoot, this should be the default plan.


Reliable timecode sync starts with devices sharing the same clock. Use waveform sync when there's no reliable shared timecode, but every camera recorded recognizable production sound. This can work for interviews, podcasts, small concerts, and unscripted setups where all cameras can hear the same clap, voice, or room audio. It becomes unreliable when cameras are far apart, one camera is isolated, a camera only recorded silence, or one source has distorted scratch audio.


Use manual sync when neither timecode nor waveform can be trusted. A clapper, flash, hand clap, camera flash, or other clear transient gives you a known point. Manual sync takes more inspection than automatic sync, but it makes the correction explicit. If the auto-sync result is wrong, don't keep rebuilding the same broken multicam and hoping Resolve changes its mind. Open the multicam, move the bad angle, and make the correction visible.


Choose the sync method separately for each shooting pattern in the project. A two-camera sit-down interview might sync cleanly by waveform, while a live event with locked cameras across a room should use timecode. A hybrid shoot may need both: timecode for the main cameras, waveform or slate correction for crash cams and pickup units.


## Organize BRAW media before building multicam clips


Do the organization in the Media page or Media Pool before you create multicam clips. Resolve’s Media page is built for importing, organizing, syncing, and editing metadata, and it's easier to fix camera names and reel names before those values are used as multicam angle names.


For a BRAW multicam shoot, create bins that match the shooting structure rather than the card dump structure. A clean bin layout might look like this:


- Day 01 / Scene or segment / A Cam
- Day 01 / Scene or segment / B Cam
- Day 01 / Scene or segment / C Cam
- Day 01 / Audio
- Day 01 / Multicam Builds
- Day 01 / Sync Fixes or Problem Clips


That structure gives the editor a way to find the material by production logic while still preserving the original folder tree somewhere outside Resolve.


Set or confirm the clip metadata that Resolve may use for grouping and angle display. The useful fields vary by job, but these are the ones that tend to matter most:


- [Camera number or camera ID](https://www.steakunderwater.com/VFXPedia/__man/Resolve18-6/DaVinciResolve18_Manual_files/part700.htm)
- Camera angle
- Reel name
- Clip name
- Start timecode
- Date created or shoot date
- Audio channel names
- Scene, shot, and take if the production used them


Don't assume every Blackmagic camera writes metadata exactly the same way, especially if the shoot mixed camera families such as Pocket Cinema Camera, URSA, PYXIS, Cinema Camera 6K, Studio Camera, or HyperDeck ISO recordings. Resolve can read BRAW and Blackmagic metadata, but your job is to normalize the human-facing fields so the multicam group is understandable.


## Timecode sync


When all BRAW cameras[share timecode](https://www.blackmagicdesign.com/products/blackmagicpyxis) , create the multicam clip from the selected camera clips and sync by timecode. In the Media Pool, select the clips for one scene, song, act, or continuous recording block. Right-click and create a new multicam clip using selected clips, then[set the multicam frame rate](https://larryjordan.com/articles/get-started-multicam-editing-in-davinci-resolve/) intentionally and choose a timecode-based angle sync method.


These choices usually keep a timecode-based build manageable:


- Build separate multicam clips for continuous production blocks. A whole-day multicam is usually too broad.
- Use the project or delivery frame rate as the multicam frame rate unless there's a specific reason not to.
- Name angles by camera metadata if it's clean, or by bin/order if metadata is inconsistent.
- Keep production audio as its own angle or audio source when double-system sound exists.
- Don't rely on file names alone if multiple cameras reset naming during the day.


After Resolve creates the multicam, open it in timeline form and inspect the start of each angle. Don't just look at the first frame and call it done. Scrub to a clap, a line of dialogue, a drum hit, or a visible mouth movement and confirm audio and picture agree, then jump later in the same clip and check again. If the cameras weren't truly locked, drift may show up after ten, twenty, or sixty minutes.


Timecode sync can fail in a few predictable ways. The most common is matching timecode that isn't actually matching timecode: two cameras can both start at 01:00:00:00 without ever being jammed together. Resolve will line them up because the numbers match, but the content will be wrong. Another common failure is time-of-day timecode with one camera set to the wrong time zone or date, which may place clips hours apart or stack them in a way that makes no editorial sense.


If the timecode result is suspicious, don't immediately blame Resolve. Check whether the source timecode is real, whether all devices were jammed, whether any camera was rebooted without re-jamming, and whether external audio used the same timecode standard.


## Waveform sync


Waveform sync is the practical fallback when the cameras weren't timecode locked. In Resolve, you can create a multicam clip from selected clips and choose a sound or waveform-based sync option, or you can align clips manually in a timeline using audio waveform alignment and then build from there.


Waveform sync works best when each source has a clear version of the same sound. You don't need polished audio on every camera, but you do need enough shared information for Resolve to match patterns. A camera mic pointed at the room is often enough, while a camera with disabled audio isn't.


Waveform sync is strongest when the source media has these traits:


- Each camera recorded audio for the whole take or at least for the sync point.
- Scratch audio isn't clipped beyond recognition.
- The external recorder and camera mics captured the same event.
- There are clear transients, such as claps, consonants, drum hits, or slate marks.
- Long takes don't drift because of unstable device clocks.


When those conditions are present, waveform sync can produce usable alignment without manual marking. When they aren't, use waveform sync as a starting point and expect manual repairs.


The risk with waveform sync is that it can be almost right. A camera may be one or two frames off, especially with echoey rooms, distant cameras, Bluetooth monitoring delays, or heavily compressed scratch audio. For dialogue, check mouth shapes against consonants. For music, check drum hits, guitar strums, and hand claps. Live events often provide useful checks when a speaker hits a podium mic or the audience claps.


If one angle is consistently offset, open the multicam clip as a timeline, select that angle’s clip, and slip or move it into sync. If it starts in sync and drifts later, that's a different problem because you may be dealing with mismatched sample rates, a camera clock issue, dropped frames, or nonstandard recording. In that case, split the angle into sections and correct by segment, or conform the problem source outside the multicam build.


## Mixed frame rates inside a multicam group


Mixed frame rates are where otherwise clean multicam workflows get complicated. Resolve lets you create multicam clips with a chosen frame rate, but that choice matters because the multicam clip has its own frame rate, the project has a timeline frame rate, and each BRAW source has its recorded frame rate. Those values aren't interchangeable.


Mixed frame rates can force uneven frame rhythms into one multicam timeline. Use the delivery or edit timeline frame rate as the controlling frame rate unless the job has a specific reason to do otherwise. For a 23.976 final piece, build the main editorial structure around 23.976. For 29.97 broadcast, keep the multicam aligned to that world. Don't let one off-speed camera accidentally define the whole edit.


The following patterns are common in mixed-rate projects:


- Same-speed cameras should live together in the main multicam group.
- True slow-motion or off-speed clips should usually be treated as inserts, not live-switch multicam angles.
- A 59.94 camera intended for real-time playback in a 29.97 timeline can be interpreted or retimed intentionally, but don't leave that decision implicit.
- A 23.976 A-cam and a 29.97 B-cam can be grouped, but motion cadence and frame mapping need editorial approval.
- Audio should stay tied to the real-time production clock even when picture is interpreted for artistic slow motion.


After you build a mixed-rate multicam, watch moving subjects rather than checking sync only on a still frame. Pans, claps, lip sync, and fast gestures reveal cadence problems more clearly than a static talking head. If an angle looks like it's stepping, blending, or mapping inconsistently on action, decide whether to change clip attributes, use optical flow or frame sampling, or keep that source out of the live multicam switch.


There's no universal correct setting for every mixed-frame-rate multicam because the intent changes the answer. A 60 fps camera might be a real-time sports angle, a half-speed beauty shot, or a safety camera that production never expected to use for sync audio. The safe claim is simple: set the multicam frame rate deliberately, document off-speed sources, and inspect motion plus sync after creation.


## Switching angles on the Cut page and the Edit page


Resolve gives you two main editorial experiences for multicam: the Cut page and the Edit page. Neither is the right one for every job.


The Cut page is useful when the priority is rapid assembly. Its[Sync Bin](https://www.blackmagicdesign.com/products/hyperdeckstudio/multicam) and multicam-style multiview tools are designed for seeing matching angles and building an assembly. This is especially relevant for ISO and live-style workflows, where multiple camera recordings share timecode and the editor wants to cut while looking at a multiview. If you're working with properly synced BRAW or HyperDeck-style ISO recordings, the Cut page can feel closer to live switching.


The Edit page is more traditional and more detailed because it gives you the familiar multicam viewer, timeline tools, trimming behavior, audio control, clip inspection, and finishing-adjacent control that many editors expect. If you need to repair sync, manage complex audio,[flatten multicam edits](https://larryjordan.com/articles/get-started-multicam-editing-in-davinci-resolve/) , trim heavily, or hand a timeline to color and sound, the Edit page is usually where detailed cleanup happens.


The split is mostly about speed versus control:


- Use the Cut page when timecode is solid and the job needs a rapid assembly, live-event stringout, or producer selects.
- Use the Edit page when waveform sync needs inspection or repair.
- Use the Edit page when audio routing matters.
- Use the Edit page when the multicam will be flattened, conformed, or sent to color and sound.


That split uses the Cut page for quick work and keeps the Edit page available when the job needs deeper timeline control.


If you start in the Cut page, still open the resulting edit in the Edit page before turnover. Check that the angle switches are actual multicam edits, that audio is coming from the intended source, and that no camera proxy or partial sync media has accidentally become the only referenced source.


## Audio strategy matters more than the angle switch


Multicam editing often looks like a picture problem, but the audio decision affects whether the timeline is usable for editorial and turnover. Decide early whether the edit should use camera audio, external production audio, or audio embedded in one specific camera.


For BRAW shoots, scratch audio is commonly used for sync while external WAV files carry the real mix or isolated channels. In that case, the multicam clip should preserve the production audio in a way the editor can actually use. Don't bury the only good lav track three levels inside a multicam clip and expect the sound team to guess where it went.


Common approaches are:


- Use external production audio as the main audio source for the multicam.
- Keep camera scratch audio available but muted or disabled after sync.
- Use A-cam audio only when that camera received the board feed or production mix.
- Keep isolated microphones outside the multicam if the sound workflow needs direct access.
- Use consistent track naming so dialogue, mix, boom, lavs, and scratch tracks are obvious.


Pick one approach per show or segment and make it consistent. Editors can work around routing choices when the behavior is predictable, but they lose time when every multicam clip routes audio differently.


When checking the multicam, switch picture angles while listening to audio. If the audio changes every time you switch cameras and that isn't intended, fix the multicam audio behavior before the editor starts cutting. For most dialogue edits, picture switching shouldn't randomly switch the active microphone.


In dialogue multicam edits, picture angles can change while production audio stays continuous.


## Metadata differences across Blackmagic cameras


Resolve reads Blackmagic RAW and Blackmagic camera metadata directly, but multicam jobs still expose metadata differences. Different models, firmware versions, operator habits, and recording modes can produce different clip naming patterns, camera IDs, reel values, LUT metadata, resolution, color science settings, and audio channel layouts.


The biggest editorial issue is angle identity. If Resolve names angles from metadata and three cameras all identify as “Camera 1” or use generic clip names, the multicam viewer becomes confusing. Fix the naming before creating the multicam clip.


Clear angle identity makes multicam switching easier before the edit begins. Useful angle names are boring and clear:


- A Cam Wide
- B Cam Tight
- C Cam Side
- D Cam Gimbal
- E Cam Overhead
- Program ISO
- Production Audio


Those names help because editorial decisions usually follow shot function. “PYXIS 6K” is less useful than “B Cam Tight” unless the show’s workflow specifically tracks camera bodies.


BRAW metadata also affects color work. ISO, white balance, tint, color science, gamma, and camera LUT choices may vary between cameras. Resolve lets you adjust RAW settings at the project or clip level, so decide whether the show will use camera metadata as shot, project-level RAW defaults, or clip-level normalization. For multicam, this matters because editors and producers may judge angles before the grade. If A Cam is log-like and B Cam has a show LUT appearance, people may start making creative decisions based on inconsistent monitoring.


A practical approach is to normalize viewing without destroying RAW flexibility. Apply consistent color management or viewing LUT behavior for editorial, but keep the BRAW controls available for the grade. If the cameras were set differently on the day, document that in notes or metadata rather than hiding it.


## Proxies, live sync, and partial media


BRAW multicam can be heavy, especially with several 6K or 12K angles. Resolve supports Blackmagic RAW, but real-time multicam playback depends on storage speed, GPU, CPU, debayer settings, resolution, and timeline complexity. If the editor can't play all angles, the sync may be fine but the cutting experience will feel broken.


Resolve’s proxy workflow can help. Blackmagic Proxy Generator can create H.264, H.265, or ProRes proxies in watch folders, and Resolve can link proxies back to camera originals. Some Blackmagic camera workflows can also create[small H.264 proxies](https://www.blackmagicdesign.com/au/products/davinciresolve/collaboration) alongside originals and sync those files into a Resolve bin through Blackmagic Cloud workflows. That can be useful for fast-turnaround multicam, but it adds a new risk: the editor may start cutting while only proxies have uploaded or only part of the camera-original media has reached the project.


For fast-turnaround jobs, media status needs to be visible to everyone touching the timeline:


- Label bins for camera originals, proxies, and cloud-synced files.
- Avoid renaming files outside the agreed workflow after proxies are linked.
- Confirm that multicam clips relink to BRAW originals when finishing requires RAW.
- Watch for clips that are still recording, still uploading, or missing high-res media.
- Keep proxy audio behavior consistent with original audio behavior.


The editor can start early, but the post supervisor still needs to know which timelines are safe for finishing and which are still based on incomplete media.


## Repairing a bad multicam without starting over


When a multicam clip is wrong, you usually don't need to rebuild the entire show. Open the multicam clip in timeline view and treat it like a source timeline, where you can move an angle, slip a clip, replace a segment, mute scratch audio, rename tracks, or add missing audio.


Common failure modes are easy to recognize once you know the patterns:


- One camera is offset by the same number of frames for the whole take.
- One camera starts in sync but drifts over time.
- One clip segment from a spanned recording is missing or duplicated.
- Waveform sync matched the wrong applause, clap, or repeated phrase.
- Timecode sync placed clips hours apart because one device had a wrong clock.
- Angle names are duplicated or meaningless.
- Audio switches with picture when it should remain fixed.
- A slow-motion source is playing as real time, or a real-time source is treated as slow motion.


The fix depends on the pattern. Constant offset is usually a simple move or slip, while drift needs a deeper look at sample rate, device clock, or frame interpretation. Wrong match points need manual realignment. Bad angle names should be fixed in the multicam and, if possible, in the source metadata so the mistake doesn't repeat.


After repairing, cut a short test section from the multicam into a normal timeline and switch through all angles. This is safer than discovering the repair failed after an editor has made a large number of cuts.


## Premiere Pro, Resolve, and Media Composer handle this differently


If your team moves between NLEs, it helps to understand the tradeoffs instead of treating every app like a skin over the same workflow.


DaVinci Resolve is most straightforward when the camera originals are BRAW and the finishing path stays in Resolve. You get native BRAW support, RAW controls, color management, multicam editing, audio tools, and finishing in one project. The Cut page can be efficient for synced multi-source work, while the Edit page gives a more traditional multicam environment. The tradeoff is that Resolve multicam can become confusing if metadata and audio routing aren't cleaned up early, and large BRAW groups still need storage and playback planning.


Premiere Pro is familiar to many editorial teams and has a flexible multicam workflow. It's comfortable for editors who like sequence-based organization, manual sync, and broad plugin or interchange workflows. Premiere can be a good hub when the finishing path is split across multiple tools. The tradeoff with BRAW is that you need to be deliberate about plugin support, color interpretation, proxies, and handoff if the grade is happening elsewhere.


Media Composer is well suited to structured offline editorial environments, especially where assistant editors, shared storage, bins, turnovers, and longform discipline matter. Its multicam tools are mature, and its media management culture is built for large teams. The tradeoff is that BRAW camera-original workflows usually require transcoding or a managed offline path rather than the same direct RAW-first experience Resolve gives you.


The decision depends on where the job needs control. If the show is BRAW-heavy and finishing in Resolve, building the multicam in Resolve reduces translation. If the editorial department is already standardized on Premiere or Media Composer, Resolve may still be the best dailies, transcode, or color tool while multicam editorial happens elsewhere.


## Putting the Resolve workflow together


A stable BRAW multicam workflow in Resolve starts with the correct project and timeline frame rate, organized camera-original media, and clear production audio. Import the BRAW camera originals and audio into bins that reflect the shoot structure. Before building multicam clips, make sure source timecode, camera IDs, reel names, and audio channel names are understandable to the people who will edit and finish the show. If camera metadata is inconsistent, fix display names and angle names early.


Build multicam clips around production blocks rather than the whole shooting day. A scene, segment, song, act, or continuous recording block is usually a better unit than a massive all-day group. Sync by timecode when the cameras were jammed, and use waveform sync only when all sources have usable scratch audio. Set the multicam frame rate intentionally, usually to match the edit or delivery timeline, and name angles by camera role rather than file name.


Once the multicam exists, open it as a timeline and inspect real sync points at the beginning and later in the take. Lips, claps, impacts, and music transients are more useful than a static frame. Confirm that the intended audio source stays active while picture angles change. If the group includes mixed frame rates, watch motion and cadence as well as sync.


The Cut page is a good place to move quickly when timecode is clean and the job needs a fast assembly. The Edit page is better for detailed trimming, sync repair, audio decisions, and turnover prep. If proxies or cloud-synced files are part of the workflow, make sure the multicam can relink to camera originals and that the finishing team knows whether the current timeline is still proxy-based.


For finishing, inspect the flattened or conformed timeline in context. Each angle should still point to the intended BRAW source, RAW settings should remain available where expected, color management should be consistent, and temporary sync audio shouldn't become the final production track by accident. A clean multicam then becomes a reliable source for color, sound, and delivery.


Sync method Best used when Main risk Sync check


Timecode Cameras and audio recorders were jammed or share reliable source timecode Matching numbers may not mean devices were actually jammed Check source timecode, jam history, frame rate, and sync near the start and later in the take


Waveform Every camera recorded usable scratch audio from the same event Resolve can land a source one or two frames off, or match the wrong repeated sound Check lips, claps, drum hits, and any distant or distorted camera audio


Manual slate or visual sync Timecode is unreliable and scratch audio is missing or unusable Slower setup and more human error Use a clear clap, flash, slate, or impact, then document any repaired angle


Hybrid Main cameras have timecode but crash cams, pickups, or isolated sources do not One method may be trusted too broadly across unlike sources Build by production block and inspect exceptions separately


## FAQ


Use timecode if the cameras and audio recorder were properly jammed or recorded matching source timecode. It is the most reliable method because Resolve can align clips by recorded time values instead of guessing. Use waveform sync when there is no trustworthy shared timecode, but every camera captured usable scratch audio of the same event. If neither timecode nor audio is reliable, sync manually with a slate, clap, flash, or other visible and audible sync point.


Yes. DaVinci Resolve supports Blackmagic RAW natively, so BRAW clips can be imported, organized, synced, edited, and graded without transcoding first. Resolve also exposes BRAW camera metadata and RAW controls, which helps preserve flexibility for color correction after the multicam edit is complete.


Set the multicam frame rate deliberately, usually to match the edit or delivery timeline. Same-speed cameras are easiest to manage in the main multicam group. Off-speed or true slow-motion clips are often better treated as inserts rather than live-switch angles. If you include mixed frame rates, check motion cadence, lip sync, claps, pans, and fast gestures after building the group because sync can appear correct on a still frame while motion looks wrong.


The Cut page is useful for fast multicam assemblies, especially when timecode is clean and the project resembles an ISO or live-switch workflow. The Edit page is better for detailed trimming, audio routing, sync repair, flattening, and turnover prep. Many workflows use both: start quickly on the Cut page, then inspect and refine the multicam edit on the Edit page before finishing.


Resolve may use clip metadata, camera metadata, bin order, or file names when creating angle names. If multiple Blackmagic cameras share generic camera IDs, reset clip names, or inconsistent reel names, the multicam viewer can show confusing or duplicated labels. The best fix is to normalize camera IDs, reel names, and angle names before creating multicam clips, using clear role-based names such as A Cam Wide, B Cam Tight, C Cam Side, or Production Audio.


Treat multicam status as metadata, not tribal knowledge. A post team can use custom fields for sync status, camera role, proxy status, missing originals, color notes, and turnover readiness, then sort and filter assets like a spreadsheet. Aspect supports custom metadata fields that make those production states visible across the team in[metadata columns](https://aspect.inc/features/review-and-approve#metadata) .
