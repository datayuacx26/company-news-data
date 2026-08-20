---
schema_version: "1.0.0"
document_id: "29365f8545a6f3ca5a6d959b270cbe5847044699c138a91b1ad0786c113bb555"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/camera-workflows/how-to-build-camera-test-packages-with-exposure-and-color-charts"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T19:04:16.000159+00:00"
fetched_at: "2026-07-31T19:04:17.104166+00:00"
content_hash: "sha256:9e0f79f5201c901189230e213e7dc2896259e1947bcb8000f0c44e51193eca4a"
---

# How to Build Camera Test Packages with Exposure and Color Charts

The most useful camera test package lets post answer the same few questions every time: how should this camera be exposed, how does it behave under the show’s lighting, how does it map into the color pipeline, and how does it compare with the other cameras on the job?


That means your package needs more than a ColorChecker held in front of the lens for five seconds. It needs a repeatable shooting method, documented camera settings, clean chart frames, lighting notes, post analysis, and a library structure that future productions can actually search.


For one camera on one shoot, the package can stay lean. For multiple brands, multiple sensors, or a show that will return for another season, your team should treat the package like a technical deliverable.


## Start with the decision the test is meant to support


A camera test can drift fast because one person wants to test skin, another wants to compare lenses, someone else wants a LUT, the colorist wants chart frames, editorial wants camera names and timecode to make sense, and the DIT wants to know whether the dailies transform is safe.


So define the decision first.


Common decisions include:


- Matching crash cams, drones, mirrorless bodies, action cameras, or phones to a cinema camera
- Establishing exposure guidance for a DP and camera team
- Building or validating a show LUT
- Testing an ACES, LogC, S-Log, V-Log, REDWideGamutRGB, DaVinci Wide Gamut, or camera-managed pipeline
- Confirming that dailies, editorial proxies, and finishing media carry the right color metadata


The chart package should serve that decision. If the question is “Can this B camera cut with Camera A in mixed office lighting?”, a perfectly isolated lab chart under one daylight source won't tell the whole story. If the question is “Can we build a technical input transform?”, then casual chart frames from the end of a lighting test aren't good enough.


ACES guidance is blunt on this point: chart capture for transform generation is a precise procedure, and[a poorly generated transform](https://docs.acescentral.com/system-components/input-transforms/capture-guide/) can be worse than no transform. That doesn't mean every production needs a formal IDT session, but it does mean you should separate quick creative camera tests from calibration-style chart capture.


## Choose charts based on the job, not brand loyalty


X-Rite and DSC Labs charts solve different problems, and many productions use both.


Compact color charts and larger video charts answer different test questions. Chart or reference Best use Main strength Watch out for


18 percent gray card Exposure placement and white balance reference Fast, simple, easy to repeat across cameras Does not describe color rendition or highlight behavior


X-Rite ColorChecker Classic or Passport General color reference, quick comparisons, travel kits Widely recognized and easy to shoot on set Small patches can be hard to sample if the chart is too small in frame


X-Rite ColorChecker Video Video-oriented color and skin reference Familiar to DITs, editors, and colorists in fast workflows Still needs controlled lighting and proper transform handling


DSC Labs ChromaDuMonde, OneShot, or CamAlign Scope-based camera matching and engineering review Designed for vectorscope, grayscale, and camera alignment work Larger, more expensive, and less casual to deploy


Grayscale or step wedge chart Tonal response, clipping, shadow separation, latitude checks Shows how exposure maps through the camera and transform Must be evenly lit and exposed carefully to be meaningful


Focus or resolution chart Sharpness, lens behavior, debayer, detail, or compression testing Separates optical and processing issues from color issues Can distract from exposure and color goals if the test is not scoped


[Gray ball, chrome ball](https://caveacademy.com/wiki/onset-production/data-acquisition/data-acquisition-training/the-grey-the-chrome-and-the-macbeth-chart/) , and color chart VFX lighting reference and CG look development Helps reconstruct lighting direction, intensity, and color Needs VFX supervision and consistent placement to be useful


The familiar X-Rite ColorChecker Classic, ColorChecker Passport, and ColorChecker Video charts are compact, affordable, and widely understood by DITs, colorists, editors, and finishing teams. They're useful for white balance reference, relative exposure checks, and rough camera-to-camera comparison. They're also easy to travel with and quick to shoot at the head of a setup.


DSC Labs charts are more production-oriented for video workflows. Charts like ChromaDuMonde, OneShot, CamAlign, and grayscale or dynamic range charts are designed to be read on scopes and used in engineering-style camera matching. They tend to be larger, more expensive, and more useful when you want to see vectorscope placement, skin tone behavior, grayscale tracking, and exposure response with less ambiguity.


A practical test package usually includes a few chart types:


- 18 percent gray card for exposure and white balance reference
- Color rendition chart such as X-Rite ColorChecker Classic, Passport, or Video
- DSC Labs color chart when scope-based camera matching is part of the workflow
- Grayscale or step wedge chart for tonal response and clipping behavior
- Focus or resolution chart if sharpness, lens behavior, or debayer/detail settings are part of the decision
- Gray ball, chrome ball, and color chart if VFX, lighting reconstruction, or CG look development is involved


The takeaway is simple: a ColorChecker alone isn't a full camera test, but it's a useful reference target. For exposure latitude, you need tonal steps. For camera matching, you want charts that read clearly on scopes. For VFX, you need lighting references. For actual profiling, you need[chart layout data, reference values](https://rawtherapee.com/mirror/dcamprof/camera-profiling.html) , controlled lighting, and enough care that the measured patches mean something.


Also, don't assume that a chart can “fix” color by itself.[Color rendition charts aren't magic](https://www.colour-science.org/posts/the-colorchecker-considered-mostly-harmless/) calibration devices unless you've the right reference data and a controlled process. Use them to anchor decisions, compare cameras, and document conditions. Be careful with one-click correction if you don't understand what transform it's building.


## Build a controlled setup that production can repeat


The goal is repeatability, and you want a package where your team tested Camera A, Camera B, and Camera C under the same conditions, with settings that reflect the real shoot.


For a controlled chart setup, keep these variables stable:


- Light source and color temperature
- Light level at the chart
- Camera distance and framing
- Lens focal length or field of view
- Aperture, shutter, ISO or EI, frame rate, and recording format
- White balance method and Kelvin value
- Monitoring transform and any LUT used on set
- Timecode, slate naming, and camera ID
- Chart angle, chart flatness, and glare control


The boring parts matter because uneven lighting, a chart that isn't flat, specular reflections, wrong focus plane, in-camera processing, or a chart that's too small in frame can ruin a test. Those failures are easy to miss on set because the image “looks fine” on a monitor. They show up later as noisy patch readings, inconsistent grayscale, or false conclusions about a camera.


[Light the chart evenly](https://www.imatest.com/solutions/photography-lab-setup/) . Avoid reflections. Keep the chart perpendicular to the lens unless you're intentionally testing angle or flare. Use a lens and distance that avoid distortion across the chart. Fill enough of the frame that you can sample patches cleanly, but leave enough border to confirm the chart isn't cropped or distorted.


An even, front-on setup keeps chart captures repeatable. For mixed-camera testing, standardize motion settings first. Frame rate and shutter angle affect motion blur in ways that post can't cleanly repair. Manual Kelvin white balance is usually safer than auto white balance, especially in multicam interviews, events, or documentary work where angles need to cut together. If you let one camera drift in auto white balance while the others are locked, the chart frames won't save the edit.


## Capture the camera settings like metadata depends on it


The chart package is only useful if post knows what it's looking at, and a chart frame without camera settings is a nice picture of a chart.


For each camera body and setup, record the core technical state:


- Camera make, model, serial number, and firmware
- Sensor mode, resolution, crop mode, and aspect ratio
- Codec, bit depth, chroma subsampling, compression ratio, and RAW settings
- Log or gamma curve, color gamut, and camera color science version if applicable
- ISO, EI, gain mode, or dual-base setting
- Shutter angle or shutter speed
- Frame rate and project base rate
- White balance Kelvin and tint or CC value
- Lens make, focal length, T-stop, filtration, and any speed booster or adapter
- Internal noise reduction, sharpening, detail, highlight recovery, or look settings
- Monitoring LUT, show LUT, CDL, or viewing transform
- Audio sample rate and timecode mode when relevant to editorial sync


This belongs in the camera report and in the test package notes, and your slates should also show it where possible. The[workflow memo](https://partnerhelp.netflixstudios.com/hc/en-us/articles/4415931246995-Dailies-Best-Practices) for a production should already capture camera choice, resolution, framing, frame rate, data management, color management, and dailies deliverables. Your camera test package should feed that memo.


When editorial will receive proxies, your team should include how chart shots should transform into dailies. Dailies are technical, color-corrected, properly framed versions of the day’s clips that bridge set and post. If the chart package establishes the wrong transform, that error can propagate through dailies, offline editorial, review files, and finishing references.


## Shoot exposure brackets instead of a single “correct” frame


One properly exposed chart frame is useful, but a bracketed series is much more useful.


Exposure brackets show how each camera behaves above and below normal. Exposure brackets show where the camera places middle gray, how it rolls into highlights, how shadows behave, and how much correction is comfortable before the image breaks. This is especially important when comparing cameras with different log curves, RAW controls, highlight handling, or noise behavior.


A simple bracket pattern might include:


- Normal exposure based on the chosen meter method
- One stop under and one stop over
- Two stops under and two stops over
- Additional half-stop or third-stop increments near expected clipping points
- A gray card frame at each key exposure when transform work is involved
- A skin tone reference at normal, under, and over exposure if talent is available


The exact bracket range depends on the camera and the decision. For cinema cameras, you may want a wide range to evaluate highlight retention and shadow noise. For small cameras or phones, you may care more about when sharpening, tone mapping, and noise reduction become obvious. For HDR delivery, you need to know what happens in bright practicals, windows, and speculars, not only chart whites.


When shooting brackets, don't change multiple variables at once. For an exposure test, adjust exposure in a controlled way, with notes that document whether you changed aperture, shutter, ISO/EI, ND, or light level. Changing aperture also changes lens performance and depth of field. Changing ISO/EI may change noise behavior or highlight allocation depending on the camera. Changing shutter changes motion. For chart-only exposure response, light level or aperture changes are usually easier to interpret, but your method should match the real production question.


## Include real-world lighting after the clean chart pass


A clean chart pass is the baseline, but it isn't the whole test.


After the controlled chart frames, shoot the situations the production will actually face. That might be sodium-vapor street spill, LED volume light, mixed office fluorescents, tungsten practicals, firelight, stage color, car interiors, overcast day exterior, or high-contrast windows.


For each lighting condition, capture:


- Gray card
- Color chart
- Skin tone reference when possible
- Neutral wardrobe or fabric
- Important production colors, such as hero wardrobe, product colors, set paint, screens, or brand colors
- A few seconds of motion if the test affects editorial matching


This is where camera differences become obvious. Some cameras match well under daylight and separate badly under narrow-spectrum LEDs. Some hold skin beautifully but push saturated blues or reds. Some small cameras look acceptable until mixed lighting triggers aggressive noise reduction or local tone mapping.


Don't overbuild this into a full shoot day unless the production needs it because the value comes from controlled comparison. Same subject, same lighting, same framing, same exposure method, same transform path.


## Keep the slate and naming boring


Camera tests often fail in post because the media is technically fine but impossible to interpret. “A001_C003” isn't enough when there are eight cameras and twenty brackets.


Use visible slates and folder names that describe the test. Good labels include camera ID, chart type, lighting condition, exposure offset, color pipeline, and date.


A useful naming pattern might look like this:


```text
CAM_A_ALEXA35_DAY5600_COLORCHECKER_NORMAL
CAM_A_ALEXA35_DAY5600_COLORCHECKER_PLUS1
CAM_B_FX6_DAY5600_COLORCHECKER_NORMAL
CAM_B_FX6_DAY5600_COLORCHECKER_PLUS1
CAM_C_BMPCC6K_TUNG3200_DSC_ONESHOT_MINUS1


```


The exact syntax matters less than consistency. If the camera department, DIT, assistant editor, and colorist can all decode it without a phone call, it works.


For multicam tests, assign permanent camera IDs early. Camera A shouldn't become Camera C halfway through the test because someone liked a different body more. Editorial and post supervisors care about continuity of identifiers across camera reports, folder names, proxies, timelines, and color notes.


## Analyze the footage in the same pipeline the show will use


Don't judge log chart frames by eye in a random viewer. Bring the footage into the color pipeline that the production expects to use.


Chart footage should be reviewed through the intended color pipeline. If the show is using ACES, test through the intended input transforms and output transform. If the show is using a manufacturer pipeline, test the manufacturer transform and show LUT. If the colorist will work in DaVinci Wide Gamut or another managed space, test that path. When dailies will use CDLs plus a show LUT, your analysis should include that dailies path.


Your analysis should answer practical questions:


- Does middle gray fall where expected after the transform?
- Do neutral patches stay neutral, or is there a tint bias?
- Does skin sit where the DP and colorist expect after the base transform?
- How far can each camera be pushed before noise, banding, clipping, or hue shifts become unacceptable?
- Which settings should the team ban because they create avoidable post problems?


[Use scopes, not only monitors](https://leaderphabrix.com/pdfs/guides/Leader%20Scopes%20-%20On-Set%20Production.pdf) : waveform helps evaluate exposure placement, clipping, and grayscale separation, RGB parade shows channel imbalance, vectorscope helps compare hue and saturation behavior, and[false color can be useful](https://www.youtube.com/watch?v=rPyOZvaTUzQ) if it's aligned with the camera’s log curve or the working color space.


For exposure charts or step wedges, look for where patches merge at the top and bottom. If a step chart uses small increments, the waveform can reveal where the camera stops separating tones. Visual inspection still matters because noise texture, compression, sharpening, and color breakup may be more objectionable than the numeric clipping point.


When matching cameras, avoid the trap of forcing every patch to line up perfectly. Different cameras have different spectral sensitivities, matrices, and tone mapping. A correction that makes chart patches match may make skin or real objects look worse. Use charts to guide the match, then confirm on faces, wardrobe, production design, and practical lighting.


## Turn findings into production guidance


Your test package should produce decisions.


A useful summary for production might say:


- Camera B should be rated one-third stop lower than Camera A for the show look.
- Camera C is acceptable for daylight inserts but not mixed LED interiors.
- The show LUT is safe for dailies, but the dailies team should apply only primary CDLs.
- Your team shouldn't use auto white balance, internal sharpening above a certain setting, or high-compression proxy recording.
- Editorial should flag the drone camera in editorial bins because it needs a separate transform.


Keep this language direct because editors and post supervisors need to know which camera is which, which transform your team used, and whether the material is expected to cut.


Also include caveats, and your notes should say whether the test used one lens, one firmware version, one lighting source, and one recording format. If production later swaps to a different sensor mode or bakes in a different LUT, the reference may no longer apply.


## Package the reference library so future teams can use it


A camera test is expensive, so don't let it disappear after the prep week.


Create a reference library that keeps original media, proxies, stills, notes, and analysis together. The structure should be simple enough that an assistant editor can ingest it and a colorist can find the original chart frame two years later.


A practical library structure could include:


```text
Camera_Test_Package/
00_ReadMe/
01_Workflow_Notes/
02_Original_Camera_Media/
03_Transcodes_Proxies/
04_Reference_Stills/
05_LUTs_CDLs_Transforms/
06_Scope_Exports_Reports/
07_Colorist_Notes/
08_Editorial_Notes/


```


The readme isn't optional, and it should explain the shoot date, crew, cameras, chart types, lighting setup, pipeline assumptions, and known limitations. Include chart serial numbers or target versions when available, especially for formal profiling work. Some profiling workflows depend on chart layout files and measured reference values. Targets can vary in manufacturing and can fade or get damaged, so document which physical chart your team used.


For each camera, store:


- Original camera files
- Camera reports and lens notes
- Stills exported through the neutral technical transform
- Stills exported through the show look, if one exists
- CDL and LUT versions your team used during analysis
- Resolve project, Baselight scene, or other grading project when allowed
- Written conclusions and exposure recommendations


Don't store only graded JPEGs because future teams may need to reprocess the test through a new ACES version, a new manufacturer transform, or a different HDR output. Keep the OCN or original test clips with enough metadata to rebuild the analysis.


## Treat chart condition and environment as part of the data


Charts are physical objects, and they get fingerprints, scratches, dust, warped backing, sun fading, and coffee mist. A damaged chart can make a camera look wrong.


Store charts in protective cases. Avoid touching patches. Keep them out of direct sun except when shooting. Replace charts that are visibly faded or damaged. For measurement-heavy work, charts with measured reference data or a separate chart measurement may be appropriate.


The same applies to the viewing environment. If analysis happens on an uncalibrated display in a bright room, conclusions about subtle color differences are shaky. You can still learn useful things from scopes and relative comparisons, but be honest about the limits. A reference-grade review environment isn't always available in prep, but your team shouldn't let a chaotic one become the basis for show-wide color decisions.


## Common ways camera test packages go wrong


Most bad packages fail for ordinary reasons.


The usual problems are easy to recognize:


- The chart is under mixed or uneven light.
- The camera is focused behind the chart.
- One camera used a different shutter, frame rate, or white balance.
- A monitoring LUT was mistaken for a recorded look, or the reverse.
- The final notes say “Camera B is greener” without saying under which light or transform.


The fix is stricter repeatability and better notes. If the test conditions are clear, post can interpret imperfect footage. If the test conditions are unknown, even beautiful chart frames become guesswork.


## Make the package small enough to finish


There's a temptation to test everything: every ISO, every lens, every LUT, every codec, every lighting condition. That usually creates a giant pile of footage nobody has time to analyze.


A useful camera test package is scoped around decisions. For a typical cross-brand production, the core package can be compact:


- Controlled daylight chart pass for each camera
- Controlled tungsten chart pass for each camera
- Exposure brackets for hero and secondary cameras
- Skin tone reference under key lighting conditions
- Real-world mixed-light scene
- Show LUT or color pipeline validation
- Written recommendations for exposure, matching, dailies, and editorial labeling


For VFX productions, add gray ball, chrome ball, color chart, HDRI or lighting reference as required by the VFX supervisor. For heavy HDR finishing, add highlight torture tests and display-referred review. For productions that rely on phones, drones, or action cams, add tests for internal processing, stabilization, sharpening, and compression artifacts.


The package is done when it answers the production question and gives post enough reference to rebuild the answer later.


## The handoff that actually helps post


When the package moves from camera prep to post, include context with the media. The assistant editor should know which clips are technical references and which are creative tests. The colorist should know which chart frames are valid for analysis and which were casual on-set references. The post supervisor should know what decisions came out of the test.


A strong handoff includes original test media, proxies if needed, camera reports, workflow notes, LUTs, CDLs, transform assumptions, stills, scope grabs, and a short written recommendation. Keep the recommendations tied to camera IDs and lighting conditions.


The best result is boring in the right way: editorial gets consistent labels, dailies get the intended color path, the colorist gets trustworthy references, and production gets clear exposure guidance. When pickups or season two arrive, the team can open the library and understand exactly how the original cameras were tested.


That's the real value of exposure and color chart packages. They're a shared technical memory for the whole production.


## FAQ


A practical camera test package usually includes an 18 percent gray card, a color rendition chart such as an X-Rite ColorChecker, a DSC Labs chart for scope-based matching, a grayscale or step wedge chart for tonal response, and a focus or resolution chart if sharpness or lens behavior is part of the test. VFX-heavy productions may also need gray balls, chrome balls, color charts, and HDRI or lighting reference.


Shoot a normal exposure first, then capture controlled underexposed and overexposed versions, commonly at one-stop and two-stop intervals. If clipping behavior is important, add half-stop or third-stop increments near the limits. Change only one exposure variable at a time and document whether the bracket was made with aperture, light level, ISO or EI, ND, or another method.


A chart frame can look very different depending on the transform, LUT, color management system, or display path. Testing in the intended pipeline shows whether middle gray, neutrals, skin tone, saturation, clipping, and camera matching behave as expected in the same environment that dailies, editorial references, and finishing will use.


Record camera make, model, firmware, sensor mode, resolution, codec, bit depth, log or gamma curve, color gamut, ISO or EI, shutter, frame rate, white balance, lens, filtration, internal processing settings, monitoring LUT, show LUT, CDL, and timecode mode when relevant. Without this context, chart footage can be difficult or impossible to interpret later.


Store original camera media, proxies, reference stills, workflow notes, camera reports, LUTs, CDLs, transform assumptions, scope exports, colorist notes, editorial notes, and a clear readme in one structured package. The readme should describe the shoot date, cameras, charts, lighting setup, pipeline assumptions, conclusions, and known limitations so future teams can understand and reuse the test.


Treat camera ID, chart type, lighting condition, exposure offset, transform, and decision notes as searchable metadata instead of burying them only in filenames. Aspect lets teams create project-specific fields so those attributes can be tracked, filtered, and sorted as custom metadata.
