---
schema_version: "1.0.0"
document_id: "25c955854aa77f4d2f14c378a2ae0684f5d83403dd169fa960675e95da4f6879"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/how-to-set-up-aces-in-davinci-resolve-for-multi-camera-projects"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-24T08:04:34.272722+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:4ee8dc8322319af65637e4aadff3ceacf7cf76c5a244dbb6c42ed69a62f40637"
---

# How to Set Up ACES in DaVinci Resolve for Multi-Camera Projects

ACES can help on multi-camera jobs when different cameras need to feed into one managed grading pipeline, but it only works if every source is identified correctly before matching starts. Resolve won't reliably infer that one angle is ARRI LogC3, another is Sony S-Log3/S-Gamut3.Cine, another is Blackmagic Film Gen 5, and another is a baked Rec.709 drone file. You have to tell Resolve what each source is.


The core setup rule is simple: set the project to ACEScct, choose the ACES version and output transform intentionally, then assign the correct IDT per camera source. Don't use one global input transform for a mixed-camera project unless every source was shot in the same camera color space.


Mixed cameras need matching input transforms, not one shared guess.


## The basic ACES path in Resolve


In an ACES-managed Resolve project, the image moves through three stages.


Source color space and camera encoding enter through an IDT, or Input Device Transform. Resolve converts that material into the ACES working environment, where you grade the image. Resolve then converts the result to your monitor or delivery target through an ODT, or Output Device Transform.


The two settings that most often create visible problems are:


- The[ACES Input Transform](https://www.steakunderwater.com/VFXPedia/__man/Resolve18-6/DaVinciResolve18_Manual_files/part311.htm) tells Resolve what the source is.
- The ACES Output Transform tells Resolve what display or deliverable you're targeting.


If either one is wrong, the picture can still look plausible enough to delay diagnosis. A wrong IDT can look like a creative contrast choice. A wrong ODT can look like a monitor issue. A double transform can look like bad exposure. Build the project so those questions are settled before anyone starts making creative decisions.


## Project color management


Start in Project Settings, then go to Color Management. For grading work that benefits from log-like controls, choose[ACEScct as the color science](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360002088888-Color-Managed-Workflow-in-Resolve-ACES) . ACEScc is also available, but ACEScct has a toe in the shadows that many colorists find easier to grade with than the fully logarithmic ACEScc response. If a studio, platform, or show LUT pipeline specifically requires ACEScc, follow that spec. Otherwise, ACEScct is the more practical default for most Resolve grading sessions.


Use the ACES version approved for the job. If there's no required version and your Resolve version offers ACES 1.3, that's a reasonable choice because it includes the ACES Reference Gamut Compression option. If you use ACES 1.3 and the workflow allows it, turn on[Apply ACES Reference Gamut Compress](https://www.arri.com/resource/blob/293888/ef75c2c754b967c0e9c722b7ae203279/arri-color-workflows-for-mixing-arri-logc3-and-logc4-incl-sample-projects-workflow-guideline-data.pdf) before the grade starts and leave it on. Don't toggle it halfway through a job because it changes how saturated out-of-gamut colors are handled before creative corrections are applied.


For a mixed-camera project, the project-level setup usually starts with ACEScct, the approved ACES version, and an ODT that matches the calibrated monitoring target. Leave the project-level ACES Input Transform unset unless the entire project uses one camera color space. When using ACES 1.3, enable Apply ACES Reference Gamut Compress unless the show spec says otherwise.


Once this is set, save the project and communicate the settings to anyone else touching the grade. Color management is a project decision, not a private preference on a shared show.


## Per-source IDTs for mixed-camera projects


The project-level ACES Input Transform is useful on a single-camera project. On a multi-camera project, it's the wrong place to solve source interpretation unless every camera used the same gamma and gamut.


If your A-cam is ARRI LogC4, B-cam is Sony S-Log3, C-cam is Canon Log 2, drone is D-Log, and screen recordings are Rec.709, a global ARRI LogC4 IDT will force every non-ARRI clip through the wrong transform. The result is incorrect before grading begins.


Assign IDTs per source instead. In Resolve, you can[right-click clips in the Media Pool](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360002088888-Color-Managed-Workflow-in-Resolve-ACES) or timeline and assign an ACES Input Transform at the clip level. The exact menu location can vary slightly by Resolve version, but the workflow principle is the same: the clip gets its own input transform, overriding the project default.


Organize sources before assigning transforms so you can work in batches. Useful bins include A-Cam ARRI LogC4, B-Cam Sony S-Log3, C-Cam Canon Log 2, Drone D-Log, Stills, Graphics, and Rec709 Archive. Confirm the recording format, gamma, gamut, and camera generation from camera reports, metadata, or the DP/DIT, then select matching clips and assign the correct ACES IDT together.


After assignment, open representative clips from each camera and compare them on scopes before grouping or matching. A short project note listing the IDT choices can save time later when another colorist, assistant, online editor, or conform artist needs to understand the setup.


## Camera generations matter


Camera names aren't enough because “ARRI LogC,” “Blackmagic Film,” and “Sony Log” aren't specific enough to identify the correct transform.


A few common places people get burned:


- ARRI ALEXA LogC3 and ARRI LogC4 are different and need different transforms.
- Blackmagic Design Film Gen 4 and Gen 5 shouldn't be treated as interchangeable.
- Sony S-Log2 and S-Log3 are different, and Sony gamuts matter too.
- Canon Log, Canon Log 2, and Canon Log 3 are different encodings.
- Panasonic V-Log/V-Gamut shouldn't be treated like generic log.
- DJI, GoPro, phone HDR, and drone “flat” profiles may not have complete ACES support.


If the camera report only says “shot log,” that isn't enough information to assign an IDT confidently. Ask for the camera model, recording mode, gamma, gamut, firmware generation if relevant, and whether any LUT was burned in.


## RAW sources


Resolve can often debayer RAW sources into the ACES pipeline using camera metadata and supported camera SDKs. That's convenient, but you should still inspect the Camera RAW settings and confirm what Resolve is using.


For RAW clips, pay attention to:


- Decode using camera metadata versus project settings.
- Color science or generation settings, especially for Blackmagic RAW.
- ISO, white balance, tint, and exposure metadata.
- Whether the clip has been transcoded to a non-RAW mezzanine file before it shows up in the Resolve project.
- Whether editorial proxies were made with a baked LUT that isn't present in the camera originals.


RAW still leaves room for mistakes. Resolve may have accurate source information available, but only if the conform is pointing at the original media and not a flattened or LUT-baked intermediate.


## Baked Rec.709 and graphics


Not everything in a multi-camera timeline is scene-referred camera footage. You may have archival clips, screen recordings, graphics, subtitles, logos, phone videos, social media exports, and editorial temp renders. Those sources don't belong under a log camera IDT.


If a clip is already display-referred Rec.709, assign an appropriate Rec.709 input transform or keep it in a separate display-referred handling path, depending on the project design. Don't pretend it's log. A baked Rec.709 file pushed through an S-Log3 IDT will usually come out low contrast, unusually saturated, or wrong in a way that wastes matching time.


Already finished Rec.709 material should not be treated like camera log. Graphics need separate handling. If a lower-third was designed in sRGB on a computer display, it may not behave like camera footage under ACES. Decide whether graphics are being delivered as display-referred elements, scene-linear EXRs, or something else, then keep that rule consistent. For SDR broadcast-style work, a graphics white level around 100 nits is a common practical target, but follow the show’s finishing spec when one exists.


## Multicam clips


Resolve’s multicam tools are useful for editorial, but they can hide the source-level differences that matter to color. If you build a multicam clip from several camera formats and then only look at the multicam container, it becomes easier to miss that the angles need different IDTs.


The least ambiguous approach is to assign source IDTs before creating multicam clips, so each angle enters the multicam structure already interpreted correctly. If the multicam edit already exists, inspect the underlying source clips and make sure the transforms are attached to the actual camera media, not just adjusted visually on top of the multicam timeline.


For finishing, teams often flatten or decompose multicam edits back to original clips once picture is locked. This makes the color timeline easier to audit, easier to group by camera, and easier to conform if something relinks badly. If you keep multicam containers live through color, build a small test timeline with one shot from each angle and confirm that IDT changes are behaving as expected before grading the whole show.


## Choosing the ODT


The ODT should match your reference display and monitoring environment. Treat it as the display transform that tells Resolve how to show ACES-managed image data on your target display.


The output transform should match the display you are actually judging. For common finishing scenarios, the ODT decision usually looks like this:


- For SDR broadcast or standard video monitoring, use Rec.709 for a calibrated Rec.709 display, commonly around 100 nits with BT.1886 or the facility’s specified gamma.
- For HDR PQ mastering, use[P3-D65 ST.2084](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360002088888-Color-Managed-Workflow-in-Resolve-ACES) with the nit level that matches the mastering display, such as 1000 nits if that's the approved monitor target.
- For web graphics review on computer displays, sRGB may be useful for review, but don't treat that as a broadcast Rec.709 master unless the delivery spec says so.
- For archival ACES outputs, use no display ODT only when you're intentionally exporting ACES data for another managed stage, not for normal client viewing.


Set the ODT to the display being used for approved monitoring, then create separate output passes for different deliverables. Don't grade on an HDR ODT and render an SDR master without a proper SDR trim or transform pass.


## The viewer and the reference monitor


Resolve’s GUI viewer is useful for interface work and quick checks. A calibrated video output path is the correct reference for finishing decisions. On macOS especially, display profiles and viewer color settings can affect what you see in the interface. This means some ACES workflows can look different in the GUI viewer than through SDI monitoring, depending on system settings and Resolve preferences.


For serious finishing, trust a calibrated external display fed through a proper video I/O path. If the GUI viewer and the reference monitor disagree, don't “fix” the grade to make the GUI viewer feel better until you understand the display path. That's how teams accidentally compensate for the wrong thing.


## Where Premiere Pro and Media Composer fit


Premiere Pro, DaVinci Resolve, and Media Composer can all be part of a professional multi-camera workflow, but each has a different role when the job is an ACES-managed finish.


Premiere Pro is often used for editorial on projects with mixed codecs, proxies, graphics, and quick-turn review exports. It has color management features and can carry viewing LUTs and basic transforms for editorial review. For an ACES grade, though, Premiere is commonly treated as the offline or finishing edit environment, with the locked edit sent to Resolve, Baselight, or another dedicated color system. The risk is that an editor’s viewing LUT or display management gets mistaken for the final color pipeline.


Media Composer is built for longform editorial, shared storage workflows, turnovers, and AAF-based post. It can support editorial color adapters, LUTs, and reference looks, but it isn't typically where the final ACES color-managed grade is performed. In an ACES job, Media Composer’s main responsibility is to preserve the edit, metadata, source timecode, and clean turnover.


DaVinci Resolve is the most direct place of the three to set up and finish an ACES grade because color management, per-clip input transforms, RAW controls, scopes, grouping, trimming, and delivery all live in one environment. Edits can still happen elsewhere. If Resolve is your color finishing system, it should be the source of truth for ACES transforms and final monitoring.


The practical division of labor is simple: let editorial systems help people edit efficiently, but don't let editorial viewing transforms become undocumented color science.


## Turnover information that protects the ACES setup


A reliable ACES setup starts before the colorist opens the project. If editorial turns over a timeline with duplicate filenames, missing camera reports, baked LUTs, and undocumented transcodes, the color-managed grade becomes detective work.


For multi-camera ACES jobs, production and editorial should provide the information that affects color interpretation:


- Camera model and sensor mode for each source.
- Recording gamma and gamut, not just codec.
- Camera generation or color science generation where relevant.
- Whether LUTs were used only for monitoring or baked into files.
- Original camera filenames with unique names across cards.
- Camera reports, DIT reports, or at least a source map by shoot day and camera.
- Reference exports from editorial with the temp look included.
- Notes for any archival, stock, phone, screen recording, or drone material.


Use this information to build your bins and IDT assignments before matching cameras. If something is missing, flag it early instead of grading around uncertainty.


## Unsupported sources


Some sources don't have an[official ACES IDT](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360002088888-Color-Managed-Workflow-in-Resolve-ACES) available in Resolve. A camera may be too new, too obscure, too consumer-oriented, or recorded in a profile that ACES doesn't officially support in your Resolve version. The wrong approach is to pick a random IDT that “looks close” and keep moving without telling anyone.


Unsupported sources need deliberate handling instead of a random transform. There are a few defensible ways to handle unsupported sources:


- If the manufacturer provides a documented ACES IDT or technical transform, use that and document it.
- If Resolve has a Color Space Transform option for the camera gamma and gamut, you can use a node-level transform into the ACES working space, but keep it as the first technical operation and label it clearly.
- If the source is display-referred Rec.709, treat it as Rec.709 instead of forcing it through a log transform.
- If the source is unknown, contact the DP, DIT, camera department, production company, or vendor before guessing.
- If you must match by eye, isolate those clips, document that they're unmanaged or custom-managed, and avoid making them the basis for show-wide look decisions.


Separate official ACES-managed sources from best-effort sources. A mixed-camera grade can handle some best-effort material if it's documented. It becomes difficult to manage when every transform is treated as equally valid.


## LUTs inside ACES


Most camera LUTs are designed for a specific input and output, often log camera space to Rec.709. If you drop that kind of LUT into the middle of an ACEScct node tree, you may be feeding it the wrong color space. The LUT may still create contrast and color, but not the contrast and color it was designed to create.


Use LUTs only when you know their expected input and output. A technical LUT should happen in the correct technical position, either before ACES conversion, as part of a documented custom input path, or outside the ACES-managed pipeline for editorial reference only. Creative show LUTs should be built or adapted for the working color space they're used in.


This matters when matching cameras. If one camera has a manufacturer LUT applied and another uses a proper ACES IDT, you're no longer comparing cameras through the same pipeline. You're comparing two pipelines.


## Matching cameras after the transforms


Once every source has the right IDT, camera matching has a cleaner starting point. Start with exposure, contrast, white balance, and saturation, not a heavy look. Use scopes and known references in the frame whenever possible: gray cards, charts, skin, wardrobe, product colors, practical lights, or repeated set pieces.


Group grades can help, but group by camera and source condition, not just scene. An ALEXA Mini in LogC3 and an ALEXA 35 in LogC4 shouldn't automatically share the same pre-clip correction. A Sony FX6 and Sony VENICE might both be S-Log3/S-Gamut3.Cine, but they may still need different matching trims because sensors, lenses, exposure, and compression differ.


A practical node structure keeps technical and creative work separated:


- Source or camera balancing near the start.
- Shot matching after the source balance.
- Creative look at the group or timeline level.
- Output-specific trims near the end, especially when making SDR and HDR versions.


Keep that separation intact as the grade gets busier. If you solve a bad IDT with a creative node, somebody may eventually copy that grade to the wrong shot and spread the problem.


Before grading the whole show, it's worth building a short sequence with one representative shot from every camera and source type: each main camera, drone, phone, archival, graphics, VFX, and any oddball codec. Assign the intended IDTs, view through the intended ODT, and make basic exposure and balance adjustments. A correctly managed log camera should appear in a reasonable place before the grade, while a Rec.709 clip shouldn't collapse when it enters the project. Saturated lights shouldn't clip or shift in a way that looks unrelated to the photographed scene, and skin shouldn't require extreme corrections just to become plausible.


If one source needs extreme corrections immediately, don't start matching around it. Confirm the IDT, camera metadata, LUT history, and whether the clip is actually an original or a baked transcode. In multi-camera ACES work, catching that problem early costs less than rebuilding late.


## Common ACES failure modes in multi-camera timelines


ACES problems in mixed-source timelines often come from setup mistakes rather than creative grading choices.


The failures you're likely to see are:


- Every camera is using the project-level IDT, even though the timeline is mixed-source.
- A baked Rec.709 clip is being treated as log.
- A log clip has both an ACES IDT and a camera LUT applied, creating a double transform.
- ARRI LogC3 and LogC4 clips are being treated as the same thing.
- RAW settings changed after grading started, shifting exposure or color temperature.
- The ODT is set for HDR while the team is judging on an SDR monitor.
- The GUI viewer is being trusted over the calibrated video output.
- A VFX EXR is tagged as a camera log file instead of ACEScg or ACES2065-1.
- Editorial proxies with baked looks are being compared directly to camera originals without understanding the LUT path.
- Someone changed the ACES version or Reference Gamut Compression setting mid-project.


When something looks wrong, diagnose the transform path before adding more corrections. A bad transform can rarely be fixed cleanly with more secondaries.


## VFX and EXR sources


VFX pulls need clear color instructions. If the show is ACES-managed, VFX vendors should know whether plates are being supplied as camera originals,[ACES2065-1 EXRs](https://sharktacos.github.io/OpenColorIO-configs/docs/ResolvePull.html) , ACEScg EXRs, or something else. Those labels determine how shots come back into Resolve.


For returned EXRs, assign the input transform that matches the file’s actual color space. ACEScg is common for CG and compositing. ACES2065-1 is common for interchange. Don't assign an ARRI or Sony camera IDT to an ACEScg render just because the original plate came from that camera. Once VFX has rendered into ACEScg, the file is no longer camera log footage.


When in doubt, ask the VFX vendor for the exact working and delivery color space. “It’s ACES” isn't enough because you need to know which ACES color space.


## Output and delivery passes


ACES makes it easier to target multiple outputs because the output transform is separated from the scene-referred grade. Separate review remains necessary. An SDR Rec.709 master, HDR PQ master, web sRGB export, and archival ACES file are different deliverables, so they shouldn't all be treated as one render with a filename change.


For each output, set the ODT or export path intentionally, review on an appropriate display, and render from a clearly labeled project version or timeline. If you need both HDR and SDR, don't assume the SDR version will be approved just because the HDR grade is approved. Make an SDR trim pass.


Also watch data levels on export. A correct ACES transform can still be undermined by a full/video range mismatch, codec tag issue, or platform-specific interpretation. Render a short test file, re-import it, and compare it against the timeline through the same monitoring path before committing to the full delivery set.


Source type Typical Resolve ACES handling Confirm before grading


Camera log footage with official ACES support Assign the matching clip-level ACES IDT, such as ARRI LogC3, ARRI LogC4, Sony S-Log3/S-Gamut3.Cine, Canon Log 2, or Blackmagic Film Gen 5 Camera model, gamma, gamut, color science generation, and whether a LUT was baked in


RAW camera originals Use Resolve Camera RAW settings and supported metadata to bring the source into the ACES pipeline Decode settings, ISO, white balance, tint, color science generation, and whether the conform points to originals


Baked Rec.709 video, archive, or screen recordings Assign an appropriate Rec.709 input treatment or keep it on a documented display-referred path Whether the file is truly display-referred, whether it contains a baked look, and expected legal or full range


Graphics and titles Treat according to how they were created, such as sRGB display graphics or scene-linear elements Design color space, intended white level, alpha handling, and final delivery spec


VFX EXR returns Assign the transform that matches the EXR color space, commonly ACEScg or ACES2065-1 Vendor delivery color space, whether the file is CG, comp, or plate-based, and any embedded metadata


Unsupported camera or unknown source Use a documented manufacturer transform, a clearly labeled Color Space Transform, or isolate as custom-managed material Source origin, gamma, gamut, LUT history, and approval from the DP, DIT, vendor, or post supervisor


## FAQ


ACEScct is usually the best default for grading in Resolve because its shadow response feels more natural for color correction than ACEScc. Use ACEScc only if a studio, platform, or show pipeline specifically requires it. Choose the latest ACES version approved for the job, and avoid changing the ACES version after grading has started.


Only use a project-level ACES Input Transform if every clip in the project was shot in the same camera color space and encoding. For mixed-camera timelines, assign the ACES Input Transform per clip or per batch of clips in the Media Pool. Each camera source needs the IDT that matches its actual gamma, gamut, and camera generation.


Do not treat baked Rec.709 footage as log camera material. Assign an appropriate Rec.709 input transform or manage it through a separate display-referred path, depending on the workflow. This applies to archival footage, screen recordings, social media exports, and some drone or phone clips that are already display-referred.


Choose the ODT based on the display you are actually using for approved monitoring. For SDR grading, that usually means a Rec.709 output for a calibrated Rec.709 monitor. For HDR mastering, use the correct PQ or HDR ODT that matches the mastering display, such as P3-D65 ST.2084 at the target nit level. Do not grade under one ODT and assume another deliverable will automatically look correct.


First check whether the manufacturer provides a documented ACES IDT or technical transform. If not, use a clearly labeled Color Space Transform only when the source gamma and gamut are known. If the footage is already Rec.709, treat it as Rec.709. If the source is unknown, contact the DP, DIT, camera department, or vendor before guessing. Unsupported sources should be documented so they are not mistaken for fully managed ACES material.


Build a source map that records camera model, gamma, gamut, camera generation, LUT status, and assigned IDT for each batch of media. Aspect can store those fields alongside the assets so assistants and colorists can filter and sort sources with[custom metadata](https://aspect.inc/features/review-and-approve#metadata) .
