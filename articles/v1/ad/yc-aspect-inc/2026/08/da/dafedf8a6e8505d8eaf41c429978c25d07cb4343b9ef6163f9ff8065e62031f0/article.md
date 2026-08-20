---
schema_version: "1.0.0"
document_id: "dafedf8a6e8505d8eaf41c429978c25d07cb4343b9ef6163f9ff8065e62031f0"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/camera-workflows/how-to-set-up-redwidegamutrgb-and-log3g10-in-resolve"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-10T10:23:18.554820+00:00"
fetched_at: "2026-08-10T10:23:20.449973+00:00"
content_hash: "sha256:39dc8f4cde81896773f60d34bd00d5842da22ccd4920101035b5dfa72661ecba"
---

# How to Set Up REDWideGamutRGB and Log3G10 in Resolve

If you're grading RED R3D in Resolve, the safest starting point is simple: decode the camera originals as REDWideGamutRGB and Log3G10, grade in that log state, then apply one output transform to the delivery color space. One input interpretation, one output transform. Most RED color problems in Resolve come from breaking that rule.


That means you should be clear about where the transform is happening. It can happen in Resolve Color Management, in a Color Space Transform node, in an IPP2 output LUT, or through RED RAW metadata. Pick one path and keep the rest out of the way, because if the image suddenly looks too contrasty, highlights get crunchy, skin shifts, or a monitoring LUT looks “twice as strong,” you're probably transforming the same image more than once.


## Why REDWideGamutRGB and Log3G10 are the normal starting point


RED’s current IPP2 workflow is built around REDWideGamutRGB, usually shortened to RWG, and Log3G10.[REDWideGamutRGB is the camera color space](https://docs.red.com/955-0187/PDF/915-0187%20Rev-C%20%20%20RED%20OPS%2C%20White%20Paper%20on%20REDWideGamutRGB%20and%20Log3G10.pdf) . It's designed to contain the colors RED cameras can record without clipping them prematurely. Log3G10 is the log encoding used to carry that tonal range in a grading-friendly way.


The value is consistency. Older RED workflows often involved camera-specific combinations like REDgamma and DRAGONcolor variants. IPP2 gives you a common working state across current and past RED cameras: RWG for color, Log3G10 for tone. Once the footage is in that state, your grade and output transform can be managed in a predictable way.


Think of the[IPP2 path as three stages](https://docs.red.com/915-0190/915-0190%20Rev-D%20%20%20RED%20OPS%2C%20IPP2%20Image%20Pipeline%20Stages.pdf) :


A clean three-stage IPP2 path keeps RAW development, grading, and display output separate.


- Primary RAW development into REDWideGamutRGB and Log3G10
- Creative grading while the image is still in that log/wide-gamut state
- Output transform to Rec.709, P3, Rec.2020, HDR, or whatever the delivery requires


Resolve can do all three, but it will also let you do the same stage in multiple places. That flexibility is useful when you know what is happening and dangerous when a project inherits LUTs, RAW metadata, XML round trips, or mixed camera settings from somewhere else.


## Start by deciding who owns color management


Before touching clip settings, decide whether the project will be managed at the project level or at the node level.


For most teams, there are two clean Resolve approaches:


- Resolve Color Management: Resolve handles input transforms and output transforms based on project settings and clip input color spaces.
- DaVinci YRGB with manual transforms: You decode RED RAW as RWG/Log3G10, grade manually, and use a Color Space Transform node or approved output LUT near the end of the node tree.


Both can produce correct results, but the bad version is the hybrid where Resolve Color Management is transforming footage,[RED RAW settings](https://support.red.com/hc/en-us/articles/360057729654-KOMODO-Recommended-R3D-Workflow-for-Blackmagic-Design-s-DaVinci-Resolve) are also applying an IPP2 output transform, and a Rec.709 LUT is added on top because the image still “looks flat” in one viewer.


For editorial turnovers and assistant-driven conforms, Resolve Color Management can be easier to standardize. For colorists who want explicit control, manual CST nodes are often easier to audit because the transform is visible in the node graph. Everyone on the job needs to know which path your team is using.


Workflow path Input handling Where the output transform happens Best fit Main risk


Resolve Color Management Tag RED clips as REDWideGamutRGB / Log3G10, or let Resolve assign them correctly Project output color space Assistant-driven conforms, mixed-camera timelines, standardized facilities Adding extra CST nodes or Rec.709 LUTs on top of project color management


DaVinci YRGB with manual CST Decode RED RAW as REDWideGamutRGB / Log3G10 in Camera RAW settings A visible CST node, timeline node, or approved output LUT near the end of the grade Colorist-controlled grades where every transform needs to be visible Hidden transforms in group nodes, timeline nodes, compound clips, or LUTs


RED RAW or IPP2 metadata-driven viewing Camera or RMD metadata may carry look settings or monitoring choices Sometimes through an IPP2 output look or LUT, depending on setup Matching a production reference or preserving a DP-approved monitoring intent Treating a viewing transform as if it were the final color pipeline


## Set the RED RAW decode to RWG and Log3G10


In Resolve, RED RAW settings live under Camera RAW in Project Settings, and you can also adjust individual clips from the Camera RAW controls when needed. For a clean IPP2-style grading setup, set the RED RAW profile so new R3D clips decode into REDWideGamutRGB and Log3G10.


A typical project-level RED RAW setup looks like this:


- Camera RAW profile: RED
- Decode using: Project, if you want one consistent decode across the job
- Color science: IPP2, where available
- Color space: REDWideGamutRGB
- Gamma curve: Log3G10
- ISO, color temperature, tint: Camera metadata unless the color team wants project-level defaults
- Decode quality: Set for playback during editorial or grading
- Force debayer to highest quality: Enabled for final render when performance allows


The key settings are color space and gamma curve. RED’s own Resolve guidance recommends working with the RAW footage in a logarithmic gamma space because it keeps flexibility and gives colorists a familiar starting point. RWG/Log3G10 is that starting point.


REDWideGamutRGB and Log3G10 are source interpretation settings attached to the RAW clip. Decode quality is a performance setting, not a look decision, so if a system can't play 6K or 8K R3D smoothly, lowering decode quality for work sessions is normal. For final output, use the highest practical debayer quality so you aren't baking a lower-quality decode into the master.


If you change Camera RAW project settings after media is already imported, verify that existing clips are actually following project settings. Some clips may use Clip instead of Project, especially in conformed timelines or projects that have passed through multiple hands. That doesn't mean the media is broken, but clip-level RAW settings may be overriding the project.


## Respect camera metadata, but don't let it surprise you


Because R3D is RAW, many image decisions are metadata-driven and non-destructive. Camera color space, gamma, ISO, white balance, tint, and look information can travel with the clip. RED also uses[RMD sidecar files](https://www.red.com/red-101/rmd-non-destructive-editing) to store non-destructive adjustments without changing the original R3D.


That's useful, but it can confuse a conform if nobody knows which metadata should be honored.


Common metadata sources include:


- In-camera look settings
- REDCINE-X PRO changes written to RMD
- Clip-level RAW changes made in Resolve
- Camera RAW project defaults inside the Resolve project
- LUTs or output transforms added for editorial viewing


The cleanest finishing workflow is to treat the R3D as the source of truth for capture metadata, then consciously decide which metadata should affect the grade. White balance and ISO metadata are often good starting points. Your team shouldn't accidentally treat a baked-looking editorial LUT or viewing transform as the final color pipeline.


If the DP created a show LUT or IPP2 monitoring look on set, ask whether it's a creative reference or a required transform because those are different things. A creative LUT may belong before the final output transform, assuming it was designed for RWG/Log3G10. An output LUT that converts RWG/Log3G10 to Rec.709 shouldn't be stacked on top of another Rec.709 output transform.


## Using Resolve Color Management


If you're using Resolve Color Management, set the project settings to describe both the source and the destination. Set the source for RED clips to REDWideGamutRGB and Log3G10. Set the destination to match your monitoring and delivery target.


For a normal SDR broadcast or web master, that often means[Rec.709 Gamma 2.4](https://docs.red.com/915-0201/REV-A/PDF/915-0201%20Rev-A%20%20%20RED%20OPS,%20IPP2%20Output%20Transforms.pdf) , though web-only teams sometimes choose Gamma 2.2 depending on their display assumptions. For theatrical or HDR, the output decision changes, but the idea doesn't: RWG/Log3G10 in, delivery color space out.


In a color-managed setup, make sure RED clips are tagged correctly. If Resolve automatically recognizes them correctly, great. If not, assign the input color space manually to REDWideGamutRGB / Log3G10. Then avoid adding a separate CST from RWG/Log3G10 to Rec.709 on every clip, because Resolve is already handling the conversion.


A color-managed project is especially helpful when you have mixed cameras. You can tag RED as RWG/Log3G10, ARRI as LogC/Wide Gamut, Sony as S-Gamut/S-Log, and so on, then work into a common timeline space and output to the target. But the same warning applies: don't add camera-to-Rec.709 LUTs just because the footage started as log.


Correct input tagging lets mixed camera sources enter one shared managed path before delivery. You should see a normal display image on a properly configured viewer or external monitor without needing an extra viewing LUT on each RED clip. If the image is still log-flat, either the clip isn't tagged correctly, the output color space isn't set, or the viewer path isn't what you think it's.


## Using manual CST nodes


Manual transforms are straightforward if you keep the node tree disciplined: decode the R3D as RWG/Log3G10, do your balancing and creative work while the image is still in that state, and then use one[output transform near the end](https://www.youtube.com/watch?v=a3i3JPU1iLs) .


A simple node structure might be:


- RAW decode: REDWideGamutRGB / Log3G10 in Camera RAW settings
- Early nodes: exposure balance, white balance trim, noise reduction if needed
- Middle nodes: contrast shaping, secondaries, look work
- Late node: Color Space Transform from REDWideGamutRGB / Log3G10 to delivery space
- Final trim nodes: legalizing, small display-referred adjustments, if required


For an SDR Rec.709 delivery, the CST output would usually be Rec.709 with the appropriate gamma, commonly Gamma 2.4 for broadcast-style monitoring. Use tone mapping and gamut mapping intentionally. These settings affect highlight rolloff and how the transform brings saturated camera colors into the smaller output gamut.


The advantage of this method is visibility. If a producer asks why the highlights look different from the offline, you can point to the exact output transform and tone mapping choice. If a shot looks double transformed, you can disable the CST and immediately see whether another transform is already active somewhere else.


The disadvantage is that every timeline, group, shared node, and compound clip needs discipline. A CST hidden in a group post-clip node plus another CST on the timeline can create the same double-transform problem as bad project color management.


## Choosing the output transform


The output transform is the technical mapping from RWG/Log3G10 into the space where the master will be viewed. RED’s IPP2 output stage exists to map wide-gamut camera data into an output color space with tone mapping, highlight rolloff, and gamut mapping.


Common output targets include:


- Rec.709 Gamma 2.4 for SDR broadcast-style mastering
- Rec.709 Gamma 2.2 for some computer-display-oriented work
- P3 variants for cinema or certain display pipelines
- Rec.2020 or HDR transforms for HDR delivery
- A log intermediate if you're handing the grade to another finishing system


Don't choose Rec.709 just because the footage looks flat. Choose it because the deliverable or monitoring path is Rec.709. If the job needs HDR and SDR, you may need separate output transforms or separate trims. A single Rec.709 transform baked into the grade won't preserve an HDR path.


Also be careful with LUTs. In IPP2, your team should generally classify LUTs as either creative LUTs or[output transform LUTs](https://docs.reddigitalcinema.com/915-0202/REV-A/PDF/915-0202%20Rev-A%20%20%20RED%20OPS%2C%203D%20Cube%20LUTs%20and%20IPP2.pdf) . A creative LUT changes the look while staying within RWG/Log3G10 assumptions, while an output LUT converts the image to a display space. If you apply an output LUT and then also use Resolve Color Management or a CST to output Rec.709, you have converted twice.


A single output transform preserves the scene; stacked transforms can crush highlights and shadows.


## How double transforms usually happen


Double transforms are common because every department needs to see a pleasing image. The camera team may monitor with an IPP2 look. Editorial may cut with a Rec.709 LUT. The assistant editor may bake proxies with a viewing LUT. The colorist may receive a Resolve timeline with project color management enabled. None of those choices is automatically wrong, but stacking them is.


Typical failure modes look like this:


- Highlights clip or roll off too aggressively even though the RAW has detail
- Saturated colors shift, especially reds, LEDs, wardrobe, or neon
- A LUT that should be subtle looks extreme
- Switching a RED clip between Project and Clip decode changes more than expected
- The viewer looks right, but exports are darker or more contrasty


When you see these symptoms, work backward. Disable timeline LUTs, group nodes, output LUTs, CST nodes, and project color management one at a time until the RED image returns to a flat RWG/Log3G10 state. Then rebuild the intended path with only one output transform.


A healthy RWG/Log3G10 image will look flat before display conversion, which is expected, but it shouldn't already look like finished Rec.709 unless an output transform is active somewhere.


## Matching project settings to camera originals


For production teams, the best time to prevent RED color confusion is before your team makes the first proxy or editorial transcode. Ask for the camera report, confirm the RED camera model and color pipeline, and preserve the R3D folder structure during ingest. R3D clips can be split into multiple file parts during recording, so sloppy file handling can create relink and conform problems later.


For Resolve finishing, your conform should[reconnect to the original R3D files](https://www.apple.com/final-cut-pro/docs/RED_Workflows_with_Final_Cut_Pro_X.pdf) , not to editorial proxies that already have transforms baked in. Proxies are fine for cutting, but document them: did the team make them flat RWG/Log3G10, or did the team make them with a Rec.709 viewing transform? If the editor cut against Rec.709 proxies, your team should send the colorist a reference export so they can compare intent without inheriting a hidden transform.


Project-level consistency matters most when multiple assistants are importing media over time. If the Camera RAW profile is set to RWG/Log3G10 before import, new R3D media should enter the project in the expected state. If people import first and configure later, spend a moment checking whether older clips follow project decode settings or have clip-level overrides.


The same applies to shots adjusted in REDCINE-X PRO. RMD sidecars can carry non-destructive look changes. Decide whether those are production-approved looks, temporary viewing adjustments, or values your team should reset for grading.


## Export settings and monitoring are part of the color pipeline


The final render should match the output transform and monitoring target. If you're mastering SDR Rec.709 Gamma 2.4, monitor that way and render with tags and codec settings that make sense for the delivery spec. If you're making a mezzanine for another facility, don't casually bake a Rec.709 transform into a file that they expect to be log.


Resolve format support for RED is mature, and recent versions include RED SDK support and GPU-accelerated RED workflows depending on hardware and Resolve edition. Performance still depends on GPU, storage, decode quality, resolution, and whether you're using Studio-only features. If playback is the issue, lower decode quality during work, but don't change color management just to make playback easier.


For final output from R3D, force the debayer quality as high as practical and make sure the output transform is exactly the one your team intends for the master. If a timeline contains both RED and non-RED cameras, verify that Resolve is interpreting each source type correctly before judging the final output. A perfect RED transform won't fix a Sony or ARRI clip with the wrong input-space tag.


## A default for most RED finishing jobs


For a standard SDR grade from RED camera originals, a clean default is:


- Conform back to original R3D media
- Set RED RAW decode to IPP2, REDWideGamutRGB, Log3G10
- Use camera metadata for ISO and white balance unless the colorist chooses otherwise
- Grade in RWG/Log3G10 or in a managed timeline space
- Apply one output transform to Rec.709 Gamma 2.4 for SDR monitoring and delivery
- Keep editorial LUTs and viewing LUTs out of the final transform path unless your team intentionally makes them part of the look


That setup preserves RED RAW flexibility while keeping the color pipeline easy to explain. If the image is wrong, there are only a few places to inspect: RAW decode, input assignment, creative LUTs, output transform, and export tags.


The main habit is to separate input interpretation from output display. REDWideGamutRGB and Log3G10 describe what the camera data is after RAW development. Rec.709, P3, or HDR describes where the finished image is going. Keep those roles separate, transform once, and the Resolve pipeline is easier to audit.


## FAQ


Yes, for most modern RED IPP2 workflows, decoding R3D media as REDWideGamutRGB and Log3G10 is the safest starting point. REDWideGamutRGB preserves the camera color gamut, while Log3G10 keeps the tonal range in a grading-friendly log state. From there, apply a single output transform to the required delivery space, such as Rec.709, P3, or HDR.


Set RED RAW decode options in Project Settings under Camera RAW, using the RED profile. Choose IPP2 where available, then set the color space to REDWideGamutRGB and the gamma curve to Log3G10. If individual clips have clip-level RAW settings, they may override the project settings, so check any conformed or inherited timelines carefully.


Yes. In a Resolve Color Management workflow, RED clips should be assigned or detected as REDWideGamutRGB and Log3G10 at the input stage. Resolve then maps them through the selected timeline and output color spaces. If Resolve is already doing the output conversion, avoid adding a separate RWG/Log3G10 to Rec.709 CST or output LUT on the same clips.


A common cause is a double transform. This can happen when an IPP2 output LUT, a Color Space Transform node, project-level color management, timeline LUTs, or baked proxy looks are stacked together. The result can be crushed shadows, clipped or crunchy highlights, dense skin tones, and color shifts. Disable transforms one at a time until the image returns to a flat log state, then rebuild the intended path with only one output transform.


Not always. Some IPP2 LUTs are output transform LUTs designed to convert REDWideGamutRGB and Log3G10 into a display space such as Rec.709. Creative LUTs are intended to shape the look while staying within the expected working color space. Applying an output LUT and then also using Resolve Color Management or a CST to Rec.709 will usually transform the image twice.


Treat proxies as editorial viewing media, not as the color source, and document whether they were made flat or with a Rec.709 viewing transform. Aspect can keep the camera originals, editorial exports, and generated viewing media together, including proxies and previews.
