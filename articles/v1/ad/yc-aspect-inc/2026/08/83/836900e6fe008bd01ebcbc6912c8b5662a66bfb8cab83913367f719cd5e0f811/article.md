---
schema_version: "1.0.0"
document_id: "836900e6fe008bd01ebcbc6912c8b5662a66bfb8cab83913367f719cd5e0f811"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/camera-workflows/how-to-configure-sony-mlut-outputs-for-on-set-monitoring"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-20T02:10:58.070865+00:00"
fetched_at: "2026-08-20T02:10:59.826472+00:00"
content_hash: "sha256:5913d1878e2afbbb540ab132cac0498360818826ebbf4c586e1dc65e513637c8"
---

# How to Configure Sony MLUT Outputs for On-Set Monitoring

The safest on-set setup is simple: record Log or RAW clean, apply the Sony MLUT only to the outputs people are using to judge the image, and make sure every monitor is looking at the same transform exactly once. When video village needs a normalized image, turn MLUT On for that SDI or HDMI feed. When editorial or dailies needs camera-original Log, keep Internal Rec MLUT Off. When a proxy is going straight to producers and they expect a viewing look, your team should set Proxy MLUT intentionally to On or Off instead of letting it inherit by accident.


That one rule solves most Sony monitoring problems, but the confusing part is that Sony cameras expose the controls differently across VENICE, VENICE 2, FX6, FX9, FR7, BRC-AM7, and newer Cinema Line bodies. The names move around, the outputs are grouped differently, and some cameras have model-specific limits. The workflow logic is the same: choose the base look, load any user LUTs, assign the MLUT to specific outputs, and verify the signal path at the monitor and recording side.


## What Sony means by MLUT


[Sony uses MLUT](https://www.sony.com/electronics/support/articles/00253956) to mean Monitor LUT. It isn't a special creative format separate from a LUT, but a LUT being used for monitoring, usually while the camera records a Log or RAW image intended for post.


In Cine EI and related Log shooting modes, Sony cameras can apply a viewing transform such as s709, 709(800%), S-Log3, or a user-imported 3D LUT. On many bodies, you select these looks as Base Looks and then route them to outputs using LUT On/Off settings.


The important distinction is where the camera applies the LUT:


- Viewfinder or LCD
- SDI output
- HDMI output
- Streaming output
- Proxy recording
- Internal main recording


Those destinations are separate decisions on many Sony cameras. That's what lets a DP and DIT see a normalized image on set while post still receives clean Log or RAW camera originals.


Destination Usual MLUT state Why it matters


Viewfinder or LCD On, unless the operator wants Log Gives the operator a normalized image while the camera can still record clean.


SDI to video village On Lets producers, director, and crew judge a viewed image instead of flat Log.


SDI to DIT or external recorder Off for Log backup, On only for reference recording Prevents accidental baked-in LUTs on files meant to remain camera-original.


HDMI to onboard monitor On if the monitor LUT is off, Off if the monitor applies the LUT Avoids applying the same transform twice.


Proxy recording Deliberate On or Off Viewed proxies may help client review, while post-facing proxies usually need to match the Log workflow.


Internal main recording Off for most Log or RAW workflows Preserves the clean negative for dailies, editorial, and final color.


Don't treat “LUT loaded in camera” as the same thing as “LUT baked into the recording.” Loading and selecting a LUT defines the look that can be applied, and the output routing determines where it actually appears.


Loading an MLUT makes it available; routing decides where it appears.


## Start with the recording decision


Before routing anything, decide whether your team wants the LUT baked into recorded media. For most narrative, commercial, and high-end documentary workflows, the answer is no for the main camera original. The camera records S-Log3, X-OCN, RAW, XAVC, or another acquisition format without the monitoring LUT applied, then the look travels downstream as metadata, a LUT file, CDL values, a dailies recipe, or all of the above.


A typical clean-record setup looks like this:


- Internal Rec: MLUT Off
- SDI to video village: MLUT On
- SDI to DIT or recorder: depends on whether they need Log or viewed image
- HDMI to onboard monitor: MLUT On, unless the monitor is applying its own LUT
- Proxy: MLUT Off for post-facing proxies, MLUT On for quick client review proxies if your team has agreed
- Streaming: MLUT On for remote review unless the remote pipeline applies the LUT later


The main trap is enabling the LUT globally or enabling it for Internal Rec when you only meant to normalize a monitor. Some Sony menus make it easy to toggle LUTs quickly, especially through assignable buttons or web controls. That's useful on set, but it also means the LUT target matters. If the assignable LUT button target controls SDI and HDMI only, fine. If it controls recording or proxy outputs, that's a different risk.


For dailies, editorial, and finishing, clean recording gives the post team maximum range and avoids accidentally baking a temporary set look into the negative. If production wants a baked look, make that a deliberate camera workflow decision, not a monitoring shortcut.


## Load the MLUT correctly


Sony cameras generally support user 3D LUTs as .cube files, commonly 17-point or 33-point. Sony’s support material describes importing 3D LUT files created in tools such as Catalyst Browse, RAW Viewer, or DaVinci Resolve, with input color space and gamma matching S-Gamut3.Cine/S-Log3 or S-Gamut3/S-Log3. Many cameras allow[up to 16 user](https://www.youtube.com/watch?v=Tm4M-AXUiwU) LUTs in addition to built-in looks.


The exact card folder and menu path vary by camera, but the reliable approach is consistent:


- Format the card in the camera before copying LUTs.
- Use the camera-created folder structure rather than inventing one.
- Copy only the approved .cube files for that shoot.
- Register the LUTs into the camera’s user slots.
- Select the intended Base Look or MLUT in the camera menu.
- Confirm the selected LUT name on the camera status display or output status page.


On bodies like FX6 and FX9, you commonly handle user MLUTs through Base Look settings in Cine EI workflows. On VENICE and VENICE 2, monitor output controls and LUT indicators are part of the camera’s professional output routing system. On FR7 and BRC-AM7, the web interface and camera menu expose[Base Look/LUT controls](https://helpguide.sony.net/ilc/2240/v1/en/contents/TP1000669629.html) and per-output LUT On/Off settings.


The takeaway is that copying a LUT file to a card is only the first half. You still need to register it, assign it to a user slot or Base Look, and then route it to the destinations where it should appear.


## Match the LUT to the camera color pipeline


A good LUT used against the wrong input causes most monitoring mistakes.


If the LUT was built for S-Gamut3.Cine/S-Log3, set the camera to output that signal before the LUT is applied. If the camera is set to a different gamma, gamut, base look, HDR mode, or display assist path, the same LUT can look too saturated, too contrasty, washed out, or simply wrong.


The key settings to align are:


- Shooting mode: Cine EI, Cine EI Quick, Flexible ISO, or another Log-capable mode supported by that model
- Gamma and gamut: usually S-Log3 with S-Gamut3.Cine or S-Gamut3, depending on the LUT design
- Base Look: built-in s709, 709(800%), S-Log3, or a registered user LUT
- EI and exposure method: especially if the LUT includes exposure compensation
- White balance and tint: camera settings still affect the signal before the LUT
- HDR/SDR monitoring mode: don't mix HDR output expectations with SDR monitoring LUTs


Don't treat a LUT intended to normalize Log to Rec.709 as a creative grade for every possible output. If you're using the[Sony s709 look](https://sony-cinematography.com/articles/sony-s709-look-up-table-workflow-part-three-of-four/) , everyone should know that's the monitoring reference. When the colorist supplies a show LUT, confirm that it was built for the camera’s actual Log/gamut settings and not for a generic “Sony Log” assumption.


## Route MLUT to SDI without contaminating the recording


On Sony cameras with professional outputs, SDI routing is usually the center of the monitoring plan. VENICE and VENICE 2 bodies include multiple SDI outputs and status indicators for LUT state across SDI output groups, Monitor output, and HDMI. FR7-style controls expose LUT On/Off items for[SDI, HDMI/Stream, Proxy](https://helpguide.sony.net/ilc/2240/v1/en/contents/TP1000669679.html) , and Internal Rec. BRC-AM7 has similar ideas, including separate handling for[SDI1 and SDI2/HDMI/Stream](https://helpguide.sony.net/rc/brc-am7/v1/en/contents/cm_lut_on_off.html) .


A clean SDI setup usually separates destinations by purpose:


- Camera original recording: clean Log or RAW
- DIT feed: clean Log if the DIT is applying LUTs externally, MLUT On if the camera is the look source
- Video village: MLUT On
- Focus monitor: MLUT On or a high-contrast assist look, depending on the focus puller's preference
- Client stream: MLUT On, unless the streaming system applies a show LUT downstream


The routing decision isn't only “SDI On or Off.” On some cameras, SDI outputs are grouped. On others, SDI1 may be independent from SDI2/HDMI/Stream. On VENICE-style bodies, the camera display can show LUT indicators for SDI output pairs and monitor outputs. Use those indicators. They're there because the camera can be in a valid but confusing state where one output is LUTted and another is clean.


When sending a feed to an external recorder, be especially careful. A recorder connected to a LUTted SDI output may record the LUTted image, depending on how it's configured. Feed a recorder meant to capture a clean Log backup from a clean output. Label a recorder meant to capture a reference file for quick review clearly in the camera report and media workflow.


Separate output paths let a recorder stay clean while monitoring receives the viewing transform.


## Don't double-LUT the monitor


The reference monitor should receive either a Log signal and apply the LUT internally, or receive a LUTted signal from the camera and display it clean, and it shouldn't do both.


A monitor path should apply one LUT, not a camera LUT plus a monitor LUT. Double-LUT symptoms are easy to spot once you know them:


- Skin looks overly contrasty or crunchy.
- Saturation jumps in reds and blues.
- Shadows block up faster than expected.
- Highlights feel clipped even when exposure tools say they aren't.
- The same image looks acceptable in the viewfinder but wrong at video village.


When the camera output is MLUT On, turn off the monitor’s own 3D LUT and use a standard display mode, usually Rec.709 for SDR monitoring. When the camera output is clean Log, load the same show LUT into the monitor or DIT box and keep the camera output MLUT Off for that feed.


For a true reference path, set the monitor to the intended color space, EOTF, and signal range. In SDR workflows, that often means Rec.709 with an appropriate gamma such as 2.4 in a controlled viewing environment. In mixed onset environments, the DP may still choose a practical monitor brightness, but keep the setting consistent across A-camera, B-camera, DIT, and video village.


Also watch[legal versus full range](https://77snszqv.media.zestyio.com/Sony_CineAlta_WF_Guide_v1.2.pdf) and SDI level settings. A LUT can be correct while the transport range is wrong, which makes the image look lifted or crushed. When a monitor has scopes, compare the same chart or gray card through the clean and LUTted paths to confirm the transform. The MLUT should transform the image, not mysteriously remap levels outside the expected range.


## Use the camera’s indicators, not memory


On a busy set, “I’m pretty sure SDI 2 is clean” isn't good enough. Sony cameras often show LUT state in status screens, viewfinder indicators, or web control panels. VENICE and VENICE 2 manuals identify LUT indicators for SDI output groups, Monitor output, and HDMI. FR7 and BRC-AM7 menus explicitly show LUT On/Off state for SDI, HDMI/Stream, Proxy, and sometimes Internal Rec.


Fold verification into the setup while the camera is still on the cart:


- Select the active Base Look and confirm the displayed name.
- Toggle the intended SDI or HDMI LUT setting and watch the monitor change.
- Confirm Internal Rec remains MLUT Off if the main recording must stay clean.
- Record a short clip and inspect whether playback, proxy, and external recorder files match the expected state.
- Capture a camera report entry with LUT name, version, EI, gamma/gamut, and outputs.


This catches the one menu item that changed when someone switched shooting mode, loaded a scene file, updated firmware, or assigned a LUT button target.


## Account for model-specific constraints


Sony’s LUT behavior is broadly consistent, but not identical across bodies and firmware, which matters when a production mixes VENICE, FX9, FX6, FR7, and smaller cameras.


Some constraints to plan for:


- Certain LUT controls only appear in specific Log shooting modes such as Cine EI, Cine EI Quick, or Flexible ISO.
- Some cameras group outputs together, so HDMI and streaming may share a LUT state.
- Some cameras allow separate LUT routing for Internal Rec and Proxy, while others have fewer destinations.
- Firmware versions can add monitor output features, user 3D LUT support, de-squeeze options, CDL support, or SDI output changes.
- On some FX9 configurations, monitoring output behavior can be limited by recording format or picture cache settings.


This is why a prep-day test matters more than a generic menu recipe. Build the actual camera package, firmware, recording format, SDI format, monitor, wireless link, and external recorder chain. Then confirm which destinations the camera can LUT independently and which destinations it links.


When the camera can't provide the exact split you want, move the LUT downstream. For example, keep SDI clean out of the camera, then let a DIT box, LUT box, monitor, or video router create separate clean and viewed paths. That's often safer than forcing the camera into a compromised global LUT state.


## Manage MLUT versions like camera media


A show LUT is part of the color pipeline, and if there are multiple cameras, you need version control that survives crew changes, camera swaps, and rushed morning reloads.


A workable naming pattern includes the show, camera family, input transform, look name, version, and date. For example:


- SHOW_SG3C_SLog3_ShowLook_v03_2026-08-19.cube
- SHOW_VENICE2_SG3C_SLog3_DayExt_v02.cube
- SHOW_FX6_SG3C_SLog3_NightInt_v05.cube


Avoid names like final, latest, new, approved2, or DP_LUT. Camera displays may truncate long filenames, so keep the distinguishing version number early enough to see in the menu if possible.


For multiple cameras, define the slot map before shooting:


- User 1: primary show LUT
- User 2: low-con contrast version
- User 3: day exterior version
- User 4: night interior version
- User 5: technical Rec.709 transform
- User 16: reserved for tests only


Slot consistency matters, because if A-camera User 1 is the hero show LUT and B-camera User 1 is yesterday’s test LUT, someone will eventually select the wrong one.


Consistent LUT slot order across cameras reduces wrong-look selection errors. When a LUT changes, treat it like a controlled handoff. Send the new file to camera, DIT, dailies, post, and remote review at the same time. Include the version in camera reports and sound or script notes if the change happens mid-day. When possible, keep a hash or checksum of the approved .cube file in the color workflow notes so post can confirm they're using the same transform.


## Keep dailies and editorial aligned


Sony’s own workflow material frames on-set look management as part of the chain from production through dailies, editorial, and final grading. The camera MLUT is only one piece of that chain. If the look seen on set is supposed to carry into dailies, the dailies colorist needs the LUT file,[CDL values if used](https://partnerhelp.netflixstudios.com/hc/en-us/articles/4415931246995-Dailies-Best-Practices) , and the camera metadata needed to apply it correctly.


The useful handoff information is:


- LUT filename and version
- Camera model and firmware family
- Recording format
- Gamma and gamut
- Base ISO and EI
- White balance and tint
- Whether proxies were recorded with MLUT On or Off
- Which outputs were LUTted on set
- Any CDL or live grade values used beyond the LUT


This prevents the classic mismatch where the DP approves one image on set, dailies show another image the next morning, and editorial cuts with a third. When the MLUT is only a viewing aid and not the intended dailies look, state that clearly too.


## Common failure modes and fast fixes


Most MLUT problems fall into a few repeatable categories, and these are the ones worth training the camera team to recognize.


- The monitor looks wrong but the camera viewfinder looks right: the monitor may be applying its own LUT on top of the camera MLUT, or it may be in the wrong color mode.
- The external recorder captured the viewed image: the team fed it from a LUTted output, or the recorder applied a LUT during recording.
- The LUT option is missing: the camera may not be in Cine EI, Cine EI Quick, Flexible ISO, or another supported Log mode.
- The LUT file won't import: the .cube may be the wrong size, wrong format, in the wrong folder, or on media the camera didn't prepare.
- Proxy doesn't match dailies: Proxy MLUT may be On while dailies are applying the LUT again, or Proxy MLUT may be Off while everyone expected viewed proxies.


Trace the image path from recording format to LUT selection to output routing to monitor processing. The wrong setting will usually reveal itself.


## The setup that holds up on set


For most Sony Log workflows, the dependable configuration is clean camera originals, MLUT On only for monitoring outputs, monitor LUTs disabled when receiving a LUTted feed, and a documented LUT version shared across camera and post. Use Sony’s per-output LUT controls instead of global assumptions. Use the camera’s LUT indicators and status pages instead of memory. Keep Internal Rec and Proxy decisions explicit.


Once that's in place, the MLUT becomes what it's supposed to be: a reliable on-set viewing transform that helps everyone judge the image without taking control away from post.


## FAQ


Usually no. For most Log, RAW, X-OCN, and XAVC camera-original workflows, keep Internal Rec MLUT Off so the recorded image stays clean for post. Turn it on only if production has deliberately chosen to bake the viewing LUT into the recorded file.


Yes, on many Sony cameras you can route MLUT On to SDI, HDMI, monitor, stream, or proxy destinations while leaving Internal Rec MLUT Off. The exact output grouping depends on the camera model and firmware, so confirm the LUT state on the camera status page or output menu.


The monitor may be applying its own LUT on top of the camera's MLUT. Use only one transform in the path: either send clean Log to the monitor and apply the LUT there, or send a LUTted signal from the camera and turn off the monitor's LUT processing.


Many Sony Cinema Line and professional cameras support user 3D LUTs as.cube files, commonly 17-point or 33-point. The LUT should be built for the camera's actual input color space and gamma, such as S-Gamut3.Cine/S-Log3 or S-Gamut3/S-Log3, and must be imported through the camera's supported folder and registration process.


Use controlled LUT filenames, consistent user slot assignments, and camera reports that list the LUT name, version, gamma, gamut, EI, and which outputs were LUTted. When a LUT changes, distribute the same file to all cameras, DIT, dailies, post, and remote review teams so everyone is using the same transform.


Treat the approved LUT package like production media: store the.cube files, slot map, camera reports, and change notes in one shared location where the whole team can see the current version. Aspect can keep those files together while preserving change and revision history, so post can confirm which LUT version was active for a given shoot day.
