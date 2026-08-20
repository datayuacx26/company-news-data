---
schema_version: "1.0.0"
document_id: "ce99c398a75abfc76c037553425c6a73125ebb5dfe8d26cfce97694b14fc39c7"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/camera-workflows/how-to-match-color-across-mixed-camera-systems-on-set"
published_at: "2026-08-08T00:00:00+00:00"
first_seen_at: "2026-08-09T21:00:56.600033+00:00"
fetched_at: "2026-08-09T21:00:58.529020+00:00"
content_hash: "sha256:d709ff611efc66a3fc56844b3413ed1b747841de8dad13c9bf89e88f03414fa2"
---

# How to Match Color Across Mixed-Camera Systems on Set

The most useful rule is this: pick one camera as the reference, keep every camera recording the cleanest native log or RAW format it can, and build the match as a documented offset into a shared monitoring and post pipeline. Don't try to make every camera “look the same” by baking creative looks into the files, because that can make set viewing feel better for an hour, then make editorial and color miserable for weeks.


Choose a reference camera, then bring the others into the same viewing path with controlled offsets. Mixed-camera color matching is really three separate jobs:


- Getting exposure and white balance into the same neighborhood on set
- Transforming different log curves and gamuts into a common space
- Creating per-camera offsets that correct repeatable differences without destroying the negative


If your team handles those three separately, the match becomes manageable. If your team blends them together, you end up chasing a moving target: one LUT doing exposure compensation, color science conversion, skin tone repair, and show look all at once.


## Start with the reference camera


Choose the reference camera before testing, and usually that means the A-camera, the hero cinema body, or the camera that will carry the most important coverage. In a studio, live, or PTZ-heavy environment, it may be the camera already approved by the producer, DP, or network engineering team.


The reference camera defines the target, but it doesn't mean you should force every other camera into the same internal settings. A Sony body shouldn't pretend to be ARRI LogC by using a random LUT in-camera. Don't paint a Canon PTZ into a brittle Rec. 709 look if your team is protecting the rest of the show for a grade. The better approach is to record each camera in its strongest native mode, then convert each source into a shared viewing or working space.


For most high-end or post-heavy work, that usually means recording native log and wide gamut:


- ARRI LogC3 or LogC4 with ARRI Wide Gamut
- Sony S-Log3 with S-Gamut3.Cine
- Canon Log 2 or Canon Log 3 with Cinema Gamut
- Panasonic V-Log with V-Gamut
- RED Log3G10 with REDWideGamutRGB
- Blackmagic Film or BRAW color science settings appropriate to the camera generation


Netflix’s multicam guidance is aligned with this idea: capture in the camera’s[native wide-gamut color space](https://partnerhelp.netflixstudios.com/hc/en-us/articles/1500000256962-Best-Practices-Multicam-Production-at-Netflix) and log encoding, avoid baking in looks, and use camera shading or painting carefully so the recorded files remain flexible. That's the key distinction. A technical offset that brings cameras closer together is useful. A baked-in taste decision that limits the grade is a trap.


## Match the scene, not just the menus


Matching camera menus is a starting point, not a match, because two cameras set to 5600K can still see the same light differently. Two lenses from different families can shift contrast, flare, warmth, and saturation. Even neutral-density filters can introduce color shifts, especially when cameras are using different strengths or different brands.


Before touching LUTs, reduce the obvious variables:


- Use the same lens family where possible
- Use the same ND brand and type where possible
- Disable auto white balance, auto ISO, dynamic contrast, and scene detection
- Set shutter angle or shutter speed consistently
- Use the same monitoring target for exposure, not just the same f-stop
- Custom white balance under the actual light, or deliberately set the same Kelvin and tint when that's the show standard


This is where a lot of “color mismatch” is actually exposure mismatch. If the B-camera is half a stop under and using a greener ND, the colorist can fix it, but every intercut will need more work. The waveform will tell you faster than your eyes whether the cameras are in the same luminance neighborhood.


Use false color, waveform, or spot readings to place shared references consistently: gray card, white card, face, highlight edge, black fabric, or whatever matters for the scene. The exact IRE targets depend on the camera, log curve, LUT, and monitoring transform, so don't blindly apply one universal number. What matters is that the reference camera establishes the placement and you bring the other cameras into a repeatable relationship with it.


## Shoot charts when the light changes


A color chart isn't magic, but it's still one of the cheapest ways to save a mixed-camera job. Shoot the chart with every camera under the same light that will illuminate the scene. Fill enough of the frame for the chart patches to be usable, keep it out of glare, and expose it the way you expose the scene.


A chart is most useful when every camera sees it under the same light as the scene. Common chart options include:


- DSC Labs OneShot or similar video-oriented charts
- Calibrite ColorChecker Video or Passport Video
- Datacolor SpyderCheckr
- Gray card and white card when a full chart is too slow


The chart gives post a neutral reference for hue, saturation, and white balance, and it also gives the DIT or shader something concrete to compare on scopes. But charts don't solve everything. Panasonic’s camera matching documentation makes an important point: accurate matching isn't possible from a[color chart alone](https://eww.pass.panasonic.co.jp/pro-av/support/content/guide/EN/Color_Match_CAM_ver.1_0_0.pdf) if the brightness characteristics are different. Gamma, pedestal, flare level, and matrix behavior all affect the result.


In practical terms, shoot charts for each meaningful lighting setup, not once at call time and then never again. Daylight window scene, tungsten practicals, LED volume, sodium streetlight, concert lighting, and green spill are different matching problems. If the light changes, the camera relationship can change too.


For fast-moving documentary or reality work, you may not get a perfect chart every setup. In that case, capture quick references whenever you can: a gray card near talent, a white shirt under the key, a Macbeth chart at the top of an interview, or a slate held in the actual lighting. Imperfect reference is still better than no reference.


## Use scopes before trusting the monitor


A monitor is necessary, but it isn't enough because your eye adapts quickly, especially on a busy set with changing ambient light. Scopes give you a stable way to compare cameras.


The waveform is usually the first place to look because it shows whether the exposure relationship is similar: blacks, midtones, faces, windows, practicals, and clipped highlights. If one camera has brighter mids and crushed blacks, it will feel different even if the color patches are close.


The RGB parade comes next because it shows channel imbalance and white point shifts. If a neutral gray patch has the green channel elevated on one camera, you have a tint problem. If the red channel rolls off differently in highlights, you may be seeing camera color science or highlight handling, not a simple white balance error.


The vectorscope helps with hue and saturation relationships, and it's especially useful for checking skin tone direction, chart patches, and oversaturation after transforms. But don't use it alone. A camera can have a pleasant-looking vectorscope trace and still cut badly because the contrast curve is different.


In a practical scope pass, your team should compare the reference camera and each secondary camera in the same display pipeline. That last part matters. Don't compare one camera in log, another through a manufacturer Rec. 709 LUT, and another through a show LUT. Put them through equivalent transforms first, then judge the offset.


## Separate technical transforms from creative looks


The cleanest mixed-camera pipeline has layers. Each layer has one job.


Keep transforms, camera offsets, creative looks, and output handling as separate layers. Pipeline layer Main job Typical on-set or post tool Risk if it is mixed with another job


Source transform Convert each camera log curve and gamut into the agreed working or viewing space ACES IDT, Resolve Color Management input setting, Baselight input transform, manufacturer technical LUT Wrong input transform, double transform, or a camera displayed through a different path


Camera offset Make each non-reference camera sit closer to the reference camera without changing the overall show look CDL, node tree, camera-match LUT, matrix or paint controls when appropriate Exposure, tint, or hue repairs get baked into the creative look and become hard to revise


Show look Apply the approved creative style to all cameras after they have been normalized and matched Show LUT, look node, LUT box, dailies look The look becomes a hidden camera correction, so it only works for one body or one setup


Output transform Convert the managed image to the monitor or delivery color space Rec. 709, P3, HDR, SDR, or display-specific transform The image is judged in the wrong display space, or the output transform is applied twice


A typical on-set and post-friendly stack might be:


- Source transform: camera log and gamut into the working or monitoring space
- Camera offset: per-camera correction to match the reference camera
- Show LUT or creative look: the intended viewing style
- Output transform: conversion to the monitor or deliverable color space


In a color-managed workflow, ACES, Resolve Color Management, Baselight color management, or another defined pipeline may handle the source transform. In a LUT-based workflow, manufacturer technical LUTs or custom transforms may handle it. Either can work, but the team needs to know which system is in control.


ACES is useful because it's designed to bring different camera sources into a[common color framework](https://cdn.theasc.com/ACES-Project-Quick-Start-Guide-DP-101519-01.pdf) , and it can simplify mixed-camera matching and help carry the monitored appearance through production and post. But it isn't a “make every camera identical” button. Camera IDTs, sensor behavior, lens color, exposure, and lighting still matter.


The same warning applies to[manufacturer LUTs](https://www.youtube.com/watch?v=71ebKrT2XlY) . A Canon Log to Rec. 709 LUT converts Canon’s log image into a display image. A Sony S-Log3 to Rec. 709 LUT converts Sony’s log image into a display image. Those two LUTs don't necessarily make Sony look like Canon or Canon look like ARRI. They may preserve each manufacturer’s color biases, which is often good for normal single-camera work but not sufficient for close intercut matching.


Camera-matching LUTs and tools can help when they're built for the specific source and target. Canon’s[Camera Color Matching Application](https://www.usa.canon.com/pro/applications/camera-color-matching-app) , for example, is designed to match supported Canon PTZ cameras to a primary Canon or third-party camera by capturing a color chart from both and generating a 3D LUT. That kind of tool is useful in controlled multicam environments, especially when PTZ cameras need to sit beside cinema or broadcast cameras without a full grading session for every angle.


The takeaway is simple: know whether a LUT is a technical transform, a camera match, a creative look, or an output transform. If one file is doing all four jobs, label it clearly and test it hard.


## Build per-camera offsets during prep


The best time to build the match is camera prep, not after lunch on the first shoot day, so put the cameras side by side, light a test scene, shoot charts, include skin tones, include saturated colors, and include highlight stress. Then build a simple offset for each non-reference camera.


A good camera offset usually corrects:


- Exposure placement
- White balance or tint bias
- Contrast curve differences
- Saturation level
- Specific hue skews, especially skin, reds, greens, and blues
- Highlight rolloff differences when they can be softened without damage


Keep the offset conservative and make the B-camera cut smoothly while preserving room for the final grade.


For LUT-based monitoring, the DIT or colorist can create a per-camera correction LUT that sits before the show LUT. For camera shading workflows, an engineer may use matrix, gamma, pedestal, flare, and paint controls to bring cameras closer before recording or switching. That's normal in broadcast and studio environments, but be careful about what your team bakes into the recorded file. If the record path captures painted Rec. 709, that's a different post workflow than recording untouched log.


Document every offset. Your camera report or color workflow notes should identify:


- Camera body and serial number
- Recording format, resolution, codec, bit depth, and chroma sampling
- Log curve and gamut
- White balance and tint
- ISO or EI
- Lens and filtration
- Monitoring LUTs and offset LUTs
- Whether any paint, matrix, pedestal, or gamma changes were baked into the recording


This metadata tells editorial, dailies, VFX, and final color what they're looking at.


## Handle mixed ARRI generations intentionally


Mixed ARRI jobs are common now because ALEXA 35 and ALEXA 265 use ARRI’s newer REVEAL color science and LogC4/AWG4, while earlier cameras such as ALEXA Mini LF output LogC3/AWG3 over SDI. ARRI’s own guidance treats this as a real workflow decision.


On set, you need to decide how your team will view[LogC3 and LogC4 signals](https://www.arri.com/resource/blob/359512/89ebcfa98c2523a8f8deaf0e2d73a1c6/arri-mixing-arri-logc3-and-logc4-on-set-guideline-data.pdf) together. In post, you need to decide whether the project will remain mixed-source with proper transforms, or whether your team will debayer or convert legacy ARRIRAW footage into the newer REVEAL/LogC4/AWG4 path where supported. ARRI notes that[Resolve Studio 18.5 and later](https://www.arri.com/resource/blob/293888/ef75c2c754b967c0e9c722b7ae203279/arri-color-workflows-for-mixing-arri-logc3-and-logc4-incl-sample-projects-workflow-guideline-data.pdf) can debayer ARRIRAW from LogC3-based ARRI cameras into REVEAL color science, and ARRI’s Reference Tool can also be part of that conversion workflow.


Not every ARRI project needs to become LogC4, but LogC3 and LogC4 are different encodings and gamuts. Treat them as different sources that need explicit transforms. If the on-set monitor path assumes they're the same, the match you approve on set may not survive dailies or final color.


## Verify on a calibrated reference monitor while you still have the cameras


Scopes catch measurable problems. A calibrated monitor catches perceptual ones. You need both.


Use a calibrated monitor and scopes together to catch both perceptual and measurable mismatches. At least one proper reference monitor on set is worth fighting for on mixed-camera jobs. Netflix’s multicam guidance calls out the value of a professional UHD reference monitor for image QC because it gives production more confidence in what your team is capturing. That doesn't mean every village monitor needs to be a grading display, but someone responsible for image decisions should have access to a[calibrated, known display](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360025502033-What-is-Color-Management) in controlled viewing conditions.


Use that monitor to compare real intercuts, not just charts, and put A-camera and B-camera coverage side by side or toggle between them through the same LUT path. Look at faces, wardrobe, product colors, practical lights, windows, and shadows. A chart may say the cameras are close, while a face reveals that one camera pushes magenta in the cheeks or makes LED-lit shadows go cyan.


Also verify the match after your team or the dailies vendor generates dailies. ARRI’s workflow guidance points out that log images look flat and desaturated on a regular video monitor until the pipeline transforms and tone-maps them into the target color space. Dailies are where many pipeline mistakes first become visible: wrong input color space, wrong LUT order, double transform, missing offset, or a Rec. 709 file treated as log.


When the dailies match the on-set reference, editorial can cut without being distracted by color jumps. When they don't, fix the pipeline immediately. Don't let ten shoot days accumulate with a known transform error.


## Common failure modes that create ugly intercuts


Most mixed-camera problems come from a short list of avoidable mistakes. These are the ones worth watching for during prep and the first shoot day:


- One camera records log while another records baked Rec. 709
- White balance is set by Kelvin only, with tint ignored
- Different ND filters create different color casts
- One camera clips a color channel earlier than the reference camera
- Dailies apply the wrong input transform or apply a LUT twice


That last point isn't directly about color, but it matters to the workflow, so your team should back up original camera negative with checksum verification, not drag it through Finder or Explorer and erase it. If your only clean chart take or camera test is corrupt or missing, the color pipeline loses its anchor.


## The decision that makes the match easier


If you have time and control, the strongest workflow is to record each camera in native log or RAW, transform every source into a shared managed color pipeline, create conservative per-camera offsets from proper tests and charts, monitor through calibrated displays, and keep the creative look separate.


If you're in a live, PTZ, or broadcast-style environment, you may need more camera painting and generated match LUTs because the switched output has to look right immediately. That's fine, but decide what your team is baking into the recording and make sure post knows.


If you're in a small crew or documentary situation, focus on the basics that give the biggest return: lock exposure behavior, lock white balance, shoot quick charts when lighting changes, use scopes when available, and avoid mixing log and baked profiles unless there's no alternative.


Different sensors, lenses, filters, and processing pipelines will never become the same camera, but their differences can be predictable, documented, and small enough that the edit feels intentional instead of patched together.


## FAQ


Choose a reference camera, expose and white balance all cameras consistently under the actual scene lighting, shoot color charts, and build conservative per-camera correction offsets. Then view every camera through the same monitoring pipeline. The match should separate source transforms, camera offsets, creative looks, and output transforms instead of relying on one LUT to do everything.


Not necessarily. A manufacturer Rec. 709 LUT usually converts that camera’s log image into a display-ready image, but it may preserve the manufacturer’s own color bias. A Sony S-Log3 to Rec. 709 LUT and a Canon Log to Rec. 709 LUT don't automatically make Sony and Canon images match. Camera matching usually requires additional per-camera offsets or a managed color workflow.


Shoot a chart for every meaningful lighting setup when possible. A chart captured at call time isn't enough if the production later moves from daylight to tungsten practicals, LED walls, concert lighting, or mixed sodium streetlight. In documentary or reality work, even quick references such as a gray card, white card, or chart at the top of an interview can help post match cameras more accurately.


ACES and other color-managed workflows help by converting different camera log curves and gamuts into a common framework, but they don't make every camera identical. Sensor response, lenses, filtration, exposure, highlight handling, and lighting still affect the image. Color management is the foundation, while per-camera offsets and visual verification are still needed for close intercut matching.


Scopes show measurable differences in exposure, channel balance, and saturation, but they don't replace perceptual judgment. A calibrated reference monitor lets the team evaluate how the cameras actually cut together through the intended viewing transform. It's especially important for checking skin tone, wardrobe, product colors, shadows, practical lights, and dailies accuracy.


Store the chart clips, reference stills, CDLs, LUTs, and camera notes in one project location instead of passing revised folders around. Aspect gives editorial, dailies, VFX, and color the same shared cloud filespace, so a B-camera offset update doesn't become four conflicting files.
