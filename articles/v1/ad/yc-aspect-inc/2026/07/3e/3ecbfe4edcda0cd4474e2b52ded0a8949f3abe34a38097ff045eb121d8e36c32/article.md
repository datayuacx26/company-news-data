---
schema_version: "1.0.0"
document_id: "3ecbfe4edcda0cd4474e2b52ded0a8949f3abe34a38097ff045eb121d8e36c32"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/camera-workflows/how-to-jam-sync-timecode-on-multi-camera-red-komodo-shoots"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T08:11:15.735962+00:00"
fetched_at: "2026-07-30T08:11:16.568967+00:00"
content_hash: "sha256:af624d6b0a13be87ced5108e92fc8cc7fc4fcb8f3c92783d2b7031a4b596a246"
---

# How to Jam Sync Timecode on Multi-Camera RED KOMODO Shoots

The safest rule for multi-camera KOMODO timecode is simple: put a dedicated timecode box on every camera, leave it connected, and set each KOMODO to External timecode with the same project time base as the generator. Jam-and-disconnect can work, but it gives you more ways to drift, more ways to miss a battery swap, and more uncertainty for post.


That rule changes only when the physical build makes a box impossible, when you're doing a true camera array that also needs genlock, or when production accepts periodic re-jams as part of the day. For most documentary, commercial, multicam interview, reality, tabletop, and unit work, continuous external LTC into each camera is the cleanest handoff.


The safest multicamera setup gives every camera and recorder its own connected timecode box. Timecode won't make every sensor expose on the same instant. It gives every recorded frame a shared time reference so editorial can line up camera clips and external audio without relying on waveform sync. If you need frame-accurate sensor phase across cameras for plates, LED volumes, VFX arrays, stereo, or motion analysis, you're talking about genlock or a RED camera array workflow in addition to timecode.


## Timecode on KOMODO in practical terms


KOMODO records timecode metadata into the R3D. RED separates two concepts that often get blurred on set:


- Time of Day, usually called TOD, records clock time as the timecode value for each clip.
- [Edgecode records elapsed time](https://docs.red.com/955-0219/955-0219_V2.0+Rev-A+RED+PS,+KOMODO-X+Operation+Guide/Content/5_How_To/TCGen/KX_Time%20Code.htm) for media and resets when you insert new media.


For multi-camera syncing, you normally care about TOD, because every camera and the audio recorder can share the same running clock. Edgecode can still exist in the file, but it isn't the shared day clock you want editorial to use when grouping cameras.


KOMODO can use several timecode sources depending on firmware and model generation, including Internal Time of Day, Internal Manual, External, and PTP. For a normal timecode box workflow, use External. For a one-time jam where the camera continues on its own internal clock after you disconnect the box, use Internal TOD and jam it from a source. The catch is drift: once the camera is free-running, its internal clock and every other device’s internal clock can slowly separate.


RED’s own docs note that when[using Internal TOD](https://docs.red.com/955-0196/955-0196_V1.7%20Rev-B%20RED%20PS,%20KOMODO%20Operation%20Guide%20HTML/Content/4_Menus/03_AudioTC/TCSource.htm) , the time at which the jam occurs affects overall drift across a 24-hour period because of non-drop-frame timecode behavior. That doesn't mean the camera is unusable without a box attached, but if the job depends on painless post sync after a long day, “we jammed it this morning” is weaker than “every camera stayed locked to its own generator all day.”


## Pick the sync method based on the job


There are three common ways to run KOMODO timecode on set. They all work, but they don't carry the same operational risk.


Sync method Best fit Camera connection Main risk Post impact


Continuous external LTC, one box per camera Normal multicam interviews, documentary, commercial, reality, tabletop, and unit work A dedicated generator stays connected to each KOMODO Low, as long as boxes stay powered and cables stay seated Most reliable grouping by shared TOD timecode


Jam-and-disconnect Tight rigs, gimbals, vehicle mounts, short shoots, or limited generator count A master generator is connected only long enough to jam the camera Medium to high, because drift, power loss, and missed re-jams can break sync confidence Usable when re-jam intervals and sync notes are well managed


Array workflow with timecode plus genlock/control VFX arrays, stereo, LED volume work, plates, motion analysis, or sensor phase critical setups Timecode plus genlock, EXT access, matched settings, and often networked camera control Higher setup complexity and more strict matching requirements Needed when cameras must expose in sync, not just line up in editorial


The most reliable multicam setup is one generator per camera, connected for the full shoot. The audio recorder gets its own generator or acts as the master source, depending on the sound team’s workflow. At call time, your team jams all boxes to the same clock and matches them to the production frame rate. Each KOMODO receives continuous LTC through its timecode input. If a camera power-cycles, the generator is still there. If a take runs long, the clock is still there. If someone swaps camera batteries, the timecode source hasn't vanished.


The lighter setup is one master generator used to jam each KOMODO, then disconnected. This keeps the camera build cleaner, but now the cameras are free-running. You need scheduled re-jams, and you need someone responsible for re-jamming after camera power loss, body swaps, firmware resets, or long breaks. This can be fine for short shoots or low camera counts. It isn't the first choice for a 12-hour multicam day where editorial expects everything to group automatically.


The array setup is different. If you need cameras to behave as an array, RED’s support guidance calls for matching firmware, matching project time base, matching recording frame rate, access to the 9-pin EXT port through the appropriate module, and network infrastructure such as gigabit Ethernet for camera array operation. This is beyond “make the clips line up in Premiere.” Timecode is part of the system, but genlock and camera control requirements drive the rig.


## Choose generators for the whole workflow, not just the camera port


Most modern timecode boxes can feed a KOMODO if you've the correct cable and output settings. Tentacle, Deity, Ambient, Saramonic, Denecke, UltraSync-style systems, and recorder-based workflows can all be viable. The differences matter less in marketing terms and more in boring production terms.


Useful selection criteria include:


- Frame rate support for the project rates you actually shoot, including 23.976, 24, 25, 29.97, 30, and drop-frame or non-drop-frame needs.
- Stable holdover accuracy across a full shoot day.
- A clear display or app that shows frame rate, jam status, battery, and running code.
- Locking or low-profile cable options that survive camera handling.
- Battery life longer than your longest realistic day, with margin.
- Compatibility with the audio recorder, not just the camera.
- A workflow for multiple units where the sound mixer, DIT, and ACs can all identify which box belongs to which camera.


For KOMODO specifically, the cable often decides whether the setup is pleasant or fragile. Without an expander, timecode typically enters through the RED 9-pin EXT-style timecode cable path. With a KOMODO expander module or third-party expansion box, you may expose a BNC timecode input, which can be easier to rig with standard sync cabling. Tentacle’s own support material, for example, points users to a[Tentacle-to-RED 9-pin timecode cable](https://support.tentaclesync.com/hc/en-us/articles/360019373897-RED-Komodo-RED-V-Raptor-Tentacle-Sync) for the camera body and Tentacle-to-BNC cables when using an expander module.


Cable paths matter: the same generator may need a different connector route depending on the camera build. Don't assume that a 3.5 mm timecode output cable is interchangeable across brands. Some boxes use different wiring, output levels, or connector conventions. A cable that works into one camera’s mic input as audio LTC may not be the correct cable for KOMODO’s dedicated timecode path.


## Match the camera and generator settings


Before you care about jamming, make sure the cameras agree about the project. In RED terminology, the project time base matters. The recording frame rate matters too, especially if you're using genlock or array workflows. For normal sync, mismatched time bases are still one of the fastest ways to create confusing metadata and yellow or warning states on the camera.


Set the shared production values first:


- Same project time base on every KOMODO.
- Same recording frame rate where you intend the cameras to sync directly.
- Same timecode frame rate on every generator and audio recorder.
- Same drop-frame or non-drop-frame convention where applicable.
- Same firmware version when cameras are part of a genlocked or array setup.


On KOMODO, the core menu path is straightforward: go to Audio / TC, set Timecode Source to External, and set the timecode display mode to TOD if you want to monitor the shared time-of-day clock on the camera. The exact menu labels vary a bit across firmware and KOMODO generations, but the working idea is the same: the camera should apply external timecode to recordings, and the displayed timecode should be the shared TOD value you expect editorial to see.


On the generator, match the frame rate to the project time base. If the box has output level settings, use the manufacturer-recommended LTC output for camera timecode input. Some workflows describe this as “line out” or an LTC output mode. The KOMODO needs to see a valid timecode signal, not a mic-level scratch feed or an incorrectly attenuated audio signal.


## A clean wired setup for multiple KOMODOs


For a two or three camera shoot, the wired external workflow is usually quick:


Jam all timecode boxes from the master clock. In many crews, the sound mixer’s recorder or master generator is the source of truth. In others, your team jams all boxes together from a phone app or one designated master box. The key is that every box starts with the same timecode value, frame rate, and drop-frame setting.


Mount one generator on each camera. Keep the box accessible enough to see battery and status, and keep the cable strain-relieved. Small timecode boxes are easy to hide until someone grabs the camera, yanks a cable, and the camera spends the next two hours recording internal code.


Feed each KOMODO through the correct timecode cable. That might be a 3.5 mm TRS-to-RED 9-pin cable from a small generator, a BNC-to-RED cable from a larger generator, or a BNC cable into an expander module. Use the cable designed for that signal path.


Set each KOMODO to External timecode. Confirm the displayed TOD matches the generator. You don't need to eyeball every digit for minutes, but the hour, minute, second, and frame behavior should be obviously identical and running.


Watch the KOMODO status indicator. RED documents the[timecode icon states](https://docs.red.com/955-0196/955-0196_V1.7%20Rev-B%20RED%20PS,%20KOMODO%20Operation%20Guide%20HTML/Content/3_Components/Status_Bar.htm) by color: gray means no timecode generator signal detected, green means the source is connected and jammed, red means connected but not jammed, white means jammed and not connected, and yellow means cross-jammed at a different project time base. In a continuous external setup, green is what you want to see.


Roll a short test clip on every camera and the audio recorder. Slate it, call out the camera IDs, and let it run long enough to capture a visible clap or verbal sync point. The first test clip is the cheapest place to catch a wrong frame rate, bad cable, swapped box, or camera left on internal timecode.


A short start-of-day slate test catches sync mistakes before the real takes begin. Once the test clip is good, leave the boxes alone. The best sync workflow is the one nobody has to keep touching.


## When wireless sync is worth it


“Wireless timecode” can mean a few different things, so it's worth being precise. Many small boxes use wireless or Bluetooth for setup, monitoring, or jamming boxes to each other, but the camera still receives LTC through a[short physical cable](https://www.youtube.com/watch?v=9ic_XI4dwNA) . That's still a wired camera connection, and it's usually what you want on KOMODO.


A fully wireless setup is useful when you've many cameras spread across a location, cameras on rigs that can't easily come back to sound, or a crew that needs to monitor timecode status centrally. The benefit is faster box management, not magic immunity from bad camera settings. Each camera still needs the right cable, the right timecode source setting, and the right frame rate.


Wireless box-to-box jamming can save time on large multicam days because you can jam or verify the fleet without physically daisy-chaining every unit. But don't treat a wireless app status screen as the only source of truth. The camera’s own status matters more. A generator can be healthy while the camera is on Internal, connected through the wrong cable, or cross-jammed to a different project time base.


For small controlled shoots, a simple wired jam from the audio bag to each box at call time may be faster and less error-prone than adding another RF layer. For reality or multicam commercial work with many bodies, wireless monitoring can be useful when an assistant catches a dead sync box before lunch.


## Jam-and-disconnect without making post hate you


Sometimes you can't leave a box on the camera. Maybe the camera is going into a tight vehicle mount, a gimbal build has no space, or production has fewer generators than bodies. In that case, treat jam-and-disconnect as a managed exception.


Set the KOMODO timecode source for an internal TOD jam workflow rather than expecting External to behave as if the source is still present. Jam the camera from a known good generator, confirm the displayed TOD, then disconnect. The camera status may indicate that it's jammed but not connected, which is expected for a disconnected jammed source. Log the jam time.


Re-jam at predictable moments: after lunch, after any camera power issue, after body swaps, after firmware or settings resets, and before important long takes. If cameras are split across units, each unit needs the same discipline or a designated master source.


The practical question is “how much drift can this production tolerate before post has to hand-fix groups?” For quick pickups, the answer may be generous. For a full-day interview with three KOMODOs and separate audio, the tolerance is usually low because the whole point of timecode is to avoid spending assistant editor time repairing preventable sync problems.


## Timecode and genlock aren't substitutes


A common sync misunderstanding on multicam RED shoots is assuming that matching timecode means the cameras are genlocked. It doesn't. Timecode labels frames, while genlock synchronizes the timing of image capture.


For most editorial grouping, timecode is enough. If Camera A and Camera B both record 10:13:22:08, the NLE can align those clips around that shared code. The sensors may not have exposed at the exact same phase, but for interviews, dialogue, and normal multicam cutting, that's usually fine.


For sensor sync, RED’s camera array guidance matters. Cameras need the same firmware version, the same project time base, and the same recording frame rate. Access to genlock and timecode requires hardware that exposes the 9-pin EXT port, such as the[KOMODO Expander Module](https://support.red.com/hc/en-us/articles/1500002465841-KOMODO-Setting-up-a-Camera-Array) or appropriate camera array accessories. For more than two cameras in an array workflow, RED describes networked setup requirements involving gigabit Ethernet infrastructure.


So the decision is:


- Use external timecode for editorial sync across cameras and audio.
- Add genlock when exposure timing between sensors matters.
- Use camera array tooling when you need synchronized control across multiple KOMODO bodies.


If the post supervisor asks for “sync,” clarify which one they mean. It can save a lot of unnecessary rigging or, worse, reveal that the job needed genlock only after the cameras are already wrapped.


## Verifying drift during a long day


Don't wait until the end of the shoot to find out whether the cameras stayed together. Your team should weave verification into the day instead.


At the start of the day, record a sync test across all cameras and the production audio recorder. Have someone call the camera letters and show a clap or slate that all cameras can see if possible. If the cameras aren't all pointed at the same thing, at least record simultaneous audio or a visible timecode display.


During the day, compare camera display time against the mounted generator when cameras come back to base. If you set a KOMODO to External and the icon is green, the camera is seeing and using the source. If it's white, the camera may be jammed but no longer connected. If it's yellow, investigate project time base mismatch immediately. If it's gray, the camera isn't seeing the signal at all.


After long breaks, battery changes, crashes, or rebuilds, record another short test. This is especially important after a camera comes off a gimbal, goes into a car mount, or your team strips and rebuilds accessories. Those are the moments when a timecode cable gets left in a pouch.


If you're using jam-and-disconnect, compare all cameras against the master generator at set intervals. You don't need a full lab measurement. You need to know whether the cameras are still close enough that post can trust the metadata. If you see drift accumulating, re-jam and tell the script supervisor or DIT where the clean boundary is.


Jam-and-disconnect workflows need periodic comparison because free-running camera clocks can drift.


## Common KOMODO sync failures and how to read them


Most failures come from source settings, frame-rate mismatch, cabling, or assuming the camera is jammed when it's only displaying internal code.


These are the patterns worth recognizing on set:


- Gray timecode icon: the camera doesn't detect a timecode generator signal. Check the box power, cable, connector path, and whether you're plugged into the correct timecode input.
- Green timecode icon: the source is connected and jammed. This is the desired state for continuous external timecode.
- Red timecode icon: the source is connected but not jammed. Re-jam and confirm the generator settings.
- White timecode icon: the camera is jammed but the source isn't connected. This may be fine for jam-and-disconnect, but it isn't the desired state if you intended continuous external timecode.
- Yellow timecode icon: the camera is cross-jammed at a different project time base. Check the KOMODO project time base and the generator frame rate before rolling more footage.


If the Audio / TC menus are unavailable or behave strangely, check whether the recording frame rate and project time base differ. Some RED documentation notes that the camera can disable certain audio and external timecode controls when those values don't match, and that can also affect audio recording.


If nothing makes sense, go back to camera fundamentals: confirm firmware, reboot, reseat cables, remove questionable accessories, and test with a known-good media card and power source. RED’s[general troubleshooting guidance](https://docs.red.com/955-0196/955-0196_V1.7%20Rev-B%20RED%20PS,%20KOMODO%20Operation%20Guide%20HTML/Content/6_Troubleshoot/GeneralTroubleshoot.htm) starts in the same place because intermittent accessory or cable issues can look like camera menu problems.


## How this shows up in post


When the workflow is right, post should receive R3D clips with usable TOD timecode and an audio recorder with matching code. In Premiere Pro, RED recommends importing R3D media through[Media Browser](https://support.red.com/hc/en-us/articles/360059576833-KOMODO-Recommended-R3D-Workflow-for-Adobe-Premiere-Pro) so spanned 4 GB R3D portions come in as unified clips. In Final Cut Pro, RED workflows commonly involve importing R3D, syncing audio and video, optionally creating optimized or proxy media, and round-tripping via XML when needed.


The exact NLE is less important than preserving the camera metadata. If assistants import partial R3D folders incorrectly, rename or separate clip components carelessly, or sync from proxies that don't carry expected metadata, a good on-set timecode workflow can still become messy. Keep the full card structure intact during offload and import, and make camera IDs and roll names obvious. If your team re-jammed, swapped cameras, or noticed sync incidents, pass those notes forward.


A practical handoff note might say: “We jammed A/B/C KOMODOs and mixer at 07:12 to 23.976 NDF. Boxes stayed connected all day except C camera on gimbal from 14:10 to 15:05; we re-jammed C at 15:08.” That one sentence can save an assistant editor from guessing why one section behaves differently.


## The setup that avoids the most pain


For a normal multi-camera KOMODO shoot, use matched generators, one per camera, and leave them connected. Set every camera and generator to the same project time base and timecode frame rate. Set KOMODO timecode source to External and display TOD. Confirm the camera shows connected and jammed, record a quick sync test, and re-check after rebuilds or long breaks.


Use jam-and-disconnect only when the rig demands it, and then make re-jamming somebody’s explicit job. Add genlock only when the production needs synchronized sensor timing, not just editorial sync. Most sync disasters are preventable before the first real take, and the camera gives you enough status feedback to catch them if someone actually looks.


## FAQ


Yes, that is the safest workflow for most multi-camera KOMODO shoots. Put one dedicated timecode generator on each camera, set the KOMODO timecode source to External, and leave the box connected. This reduces drift risk and protects you during battery swaps, long takes, and camera power cycles.


You can, but it should be treated as a managed exception. After disconnection, the camera is free-running on its internal clock, so drift can accumulate over time. If you use jam-and-disconnect, log the jam time and re-jam after lunch, power loss, camera rebuilds, body swaps, firmware resets, or before important long takes.


For normal editorial sync, use Time of Day timecode rather than Edgecode. TOD gives every camera and the audio recorder a shared running clock, which makes it easier for post to group clips by timecode. Edgecode is media-based elapsed code and is not the shared production clock most multicam workflows need.


No. Timecode labels frames with a shared time reference, but it does not synchronize sensor phase. For interviews, reality, documentary, and most multicam editorial work, timecode is usually enough. If the production needs frame-accurate sensor timing for VFX, stereo, LED volume work, or camera arrays, you need genlock or a RED camera array workflow in addition to timecode.


Green means the camera sees a connected and jammed timecode source, which is the desired state for continuous external timecode. Gray means no timecode generator signal is detected. Red means a source is connected but not jammed. White means the camera is jammed but the source is not connected. Yellow means the camera is cross-jammed at a different project time base, so you should check the KOMODO project time base and generator frame rate.


Treat sync events as searchable production metadata, not just comments buried in a chat thread. In Aspect, a team can add fields for camera ID, jam time, timecode status, sound roll, gimbal exceptions, or drift notes, then filter and sort those values across the project with custom metadata.
