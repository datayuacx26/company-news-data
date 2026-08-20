---
schema_version: "1.0.0"
document_id: "8b27c49f8dd29a7764d964724232f140a48c696a0681664ac2b2d84c9dd1723e"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/how-to-set-up-color-managed-dailies-that-match-the-on-set-look"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T23:50:08.498072+00:00"
fetched_at: "2026-08-10T23:50:10.327462+00:00"
content_hash: "sha256:0facadf0962707e4f54b847b9532088328ace75e55010e68b751c62b9aec6ba0"
---

# How to Set Up Color-Managed Dailies That Match the On-Set Look

The safest rule is simple: your dailies should use the same[color recipe](https://www.youtube.com/watch?v=dP46ohdKuOA) the DP used to judge the image on set, applied in the same order, with the same camera input assumptions, and rendered for the viewing environment editorial will actually use.


That sounds obvious until the show has an A camera in LogC4, a B camera in S-Log3, a crash cam in some flavor of Rec.709, a show LUT named “FINAL_v7_REALLY_FINAL.cube,” and three people asking why yesterday’s dailies look warmer than the monitor at video village, and an unclear pipeline causes most dailies color problems.


A color-managed dailies workflow is a repeatable chain:


- Identify the camera source color space and gamma.
- Normalize that source correctly.
- Apply the approved creative look.
- Apply shot-level CDL values when used.
- Transform to the dailies viewing target.
- Render proxies with metadata that can round-trip to editorial and finishing.


If any part of that chain is guessed, duplicated, skipped, or applied in the wrong order, editorial receives something that may be technically viewable but creatively misleading.


Dailies match the on-set look when the color chain is repeated in the same order.


## Decide how the look will travel before the first shoot day


The color plan belongs in the[workflow memo](https://partnerhelp.netflixstudios.com/hc/en-us/articles/4415931246995-Dailies-Best-Practices) , not in a Slack thread after the first camera card is uploaded. At minimum, your workflow memo should say whether the dailies colorist is applying a single show LUT, multiple scene or camera looks, per-shot CDLs, or a more formal ACES-based package such as AMF.


The most common options are:


- A single show LUT across all footage
- A show LUT plus per-shot CDL corrections
- Multiple looks selected by[camera metadata](https://partnerhelp.netflixstudios.com/hc/en-us/articles/50719037057555-Netflix-Dailies-OnSet-DIT-Guidance) or scene metadata
- An ACES pipeline using input transforms, a look transform, and an output transform
- Camera-native look metadata, such as ARRI Look Files, carried downstream


The right choice depends on how much on-set color work the production is actually doing. If the DP and DIT are using one approved viewing LUT all day and only making exposure decisions, a single show LUT may be enough. If the DIT is live grading every setup, your dailies workflow needs a reliable way to receive and apply those CDLs or look files. If the production uses multiple camera systems, a single LUT may still be part of the pipeline, but it can't be the entire pipeline unless it was designed for every source feeding it.


This is where many productions get into trouble. A show LUT is often built for a specific camera color space and display target. A LUT built for LogC to Rec.709 isn't automatically safe for S-Log3, Canon Log, V-Log, or a baked Rec.709 drone file. Feeding the wrong source into a LUT can shift saturation, contrast, skin tone, and highlight behavior in ways that look like creative intent but are really transform errors.


Different log sources need the correct input transform before a shared creative look.


## Separate technical transforms from creative intent


A clean dailies pipeline treats technical color management and creative look work as separate decisions.


The technical side answers: “What is this footage, and how do we display it correctly?” That includes camera color space, log curve, input transform, working space, and output display transform.


The creative side answers: “What did the DP and colorist intend the image to feel like?” That includes the show LUT, look file, LMT, CDL, saturation trims, contrast shaping, and other primary adjustments.


Keeping those separate makes the workflow easier to debug. If the image looks wrong, you can ask whether the camera input transform is wrong, whether the pipeline applied the look twice, whether the pipeline used the wrong output transform, or whether the CDL didn't come through.


A typical color-managed[order of operations](https://pomfort.com/article/order-of-operations-for-creating-looks-on-set/) looks like this:


- Interpret the source, including camera raw settings, log curve, gamut, ISO metadata, and white balance metadata
- Transform the input from camera space into the chosen working space, such as ACES or DaVinci Wide Gamut Intermediate
- Apply primary CDL adjustment, including slope, offset, power, and saturation when used
- Apply the creative show look, such as a LUT, LMT, ARRI Look File, or equivalent show transform
- Apply the output transform for Rec.709 Gamma 2.4, P3, HDR, or another agreed viewing target
- Encode the render with the editorial proxy codec, burn-ins, audio sync, and naming


Some workflows put the CDL before the show look, while others place it after a normalization transform but before a[display rendering transform](https://www.arri.com/resource/blob/361314/4809c14247c0704c619fc029a4064ba5/arri-how-to-create-an-arri-look-file-4-workflow-guideline-en-data.pdf) . What matters is that your team decides, tests, documents, and repeats the order. The same values in a different node order can produce a different image.


For dailies, it's usually better to keep shot-level color simple. Primary corrections and show LUTs are portable. Complex secondaries, power windows, tracking effects, and proprietary grading data are much harder to reproduce reliably across dailies systems, editorial software, VFX, and final color. If the on-set look requires advanced corrections, test the exact transport path before production depends on it.


## Build the pipeline around the camera sources you really have


The dailies system needs to know the origin of each image. Raw formats often carry enough metadata for the software to identify camera color science, but[non-raw files](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360002088888-Color-Managed-Workflow-in-Resolve-ACES) may not. ProRes, XAVC, H.264, and other video files can arrive with incomplete or misleading tags. If the dailies colorist guesses, the look match becomes luck.


For each camera type, your color plan should capture:


- Camera model and recording format
- Log curve and gamut
- Raw decode settings, if applicable
- Monitoring LUT or in-camera look used on set
- Whether the look is baked into any files
- Whether clip metadata carries the active look name
- Any required input transform or IDT
- Expected output target for dailies review


This is especially important on mixed-camera shows. An ARRI ALEXA 35 shooting LogC4/AWG4, an older ALEXA shooting LogC3, a Sony camera shooting S-Log3/S-Gamut3.Cine, and a DJI or GoPro source aren't interchangeable inputs. They may all end up as Rec.709 dailies, but they don't get there through the same transform.


Normalize each source correctly so the approved look behaves as expected. In an ACES workflow, that means using the correct input transform for each camera. In a Resolve color-managed workflow, that may mean assigning the correct input color space per clip. In a LUT-based workflow, that may mean using camera-specific technical conversion LUTs before a shared creative look, or building separate show LUTs for each camera family.


The worst version is when your team applies one camera-specific LUT to everything because it “looks close.” It may pass a quick review on a laptop, then fail when the editor cuts between cameras, the DP reviews on a calibrated monitor, or the conform returns to original camera files.


## Match the on-set monitor


When someone says dailies should match set, ask what “set” means. Was the DP looking at a calibrated Rec.709 monitor at video village? A DIT cart monitor? An HDR monitor? A wireless client monitor? A camera LCD? A still pulled from the DIT system?


The reference display matters because the final transform in the dailies chain is display-specific. A LogC-to-Rec.709 conversion isn't the same as a LogC-to-P3 or HDR transform. ARRI’s newer look workflows, for example, separate the creative part of a look from the display rendering transform, which allows the same creative intent to be viewed through different target transforms. That distinction is important because if the on-set monitor was SDR Rec.709 and editorial receives Rec.709 proxies, matching is straightforward. If set monitored HDR but editorial reviews SDR proxies, your team needs to define the SDR rendering, not assume the HDR look will magically translate.


The display transform has to match the target monitor for set and editorial to agree. For editorial dailies, Rec.709 Gamma 2.4 is still a common target for controlled viewing. Some teams use Rec.709-A or other variants for web and computer playback, but your team should make that choice consciously. A mismatch between Gamma 2.4, Gamma 2.2, Rec.709-A, and unmanaged QuickTime playback can make a correct render appear washed out or too contrasty.


The important questions are:


- What display target did the team use for approved on-set monitoring?
- What display target will editorial and creative review use?
- Are the monitors calibrated or at least known?
- Is the dailies viewer color-managed or browser/player-dependent?
- Are stills or reference frames available for comparison?


If editorial is judging dailies in a browser, on office displays, or on home monitors, exact matching isn't always possible. But your team should still export a file that's technically correct for its intended target. Don't compensate for a bad viewer by baking random corrections into the dailies because that creates a second mismatch downstream.


## Use reference stills as color evidence


Reference stills are one of the easiest ways to prevent look disputes. If the DP, DIT, or colorist approves a frame on set, your team should send that frame with the camera roll or dailies batch. It gives the dailies colorist a target and gives post a way to diagnose whether the mismatch came from the set monitor, the dailies transform, the viewer, or a changed look file.


Good reference material includes:


- Approved stills from the DIT system
- The look file, LUT, CDL, or AMF used for that still
- Camera source metadata for the matching clip
- Display target used when the still was approved
- Notes about exposure, white balance, or intended mood when relevant


The still documents the pipeline. If the still and rendered daily match inside the dailies software but not in the review platform, the issue may be playback color management. If the still doesn't match inside the dailies software, the issue is likely transform order, source interpretation, look versioning, or CDL application.


## Keep LUT versions boring and traceable


LUT chaos is a real production risk because the problem is that everyone has a different copy, with a similar filename, applied in a slightly different place.


A show LUT should have a clear version, owner, creation date, intended input, intended output, and usage note. For example, a LUT might be intended for LogC4/AWG4 input and Rec.709 Gamma 2.4 output. Another might be a creative log-to-log transform intended to sit inside an ACES or color-managed pipeline before the output transform. Those two files aren't interchangeable, even if they create a similar look.


Track these details for every look file:


- Look name
- Version
- Intended camera input color space
- Intended working space, if applicable
- Intended display output, if baked into the LUT
- Whether it's creative-only or includes technical conversion
- Approved date and approver
- Replacement policy when your team issues a new version


Creative-only looks are usually more flexible than display-baked LUTs because they can be paired with different output transforms. But many real-world productions still use .cube show LUTs, and that can work fine if your team understands the limitations. Knowing exactly what the file expects and where it belongs in the chain is the key.


## Preserve CDLs and metadata for downstream use


Dailies are part of the editorial and finishing bridge. Editorial may cut with proxies, but finishing needs to relink to original camera negative and often wants access to the same color decisions used during the shoot.


That means your dailies process should preserve the relationship between:


- Original camera filename
- Reel or roll name
- Timecode
- Sound roll and synced audio metadata
- LUT or look name
- CDL values
- Camera color space
- Framing metadata
- Clip names used by editorial
- Any ALE, EDL, XML, AMF, CDL, or other sidecar files


If this metadata is sloppy, the color may look fine on day one but fall apart during turnover. Assistant editors feel this pain first: proxies don't relink, source names don't match, color references can't be traced, and nobody knows which LUT dailies used on scene 42.


For color-managed dailies, metadata is part of the image pipeline. A rendered proxy without the color recipe is just a viewing copy. A proxy with traceable source metadata and color decisions can support editorial, VFX pulls, conform, and final grade.


Metadata travels with the daily so editorial and finishing can trace the intended look.


## Pick the right level of dailies color


Not every show needs per-shot graded dailies. More color control means more coordination, more metadata, and more places for failure. The best setup is the simplest one that preserves the DP’s intent.


For a straightforward single-camera or single-look production, use one approved show look and correct source interpretation, which is fast, consistent, and easy to maintain.


For shows where the DIT is making shot-level trims, use CDLs plus the show look, which gives the DP and DIT meaningful control without trapping the show in a proprietary grading setup.


For shows with multiple cameras, use camera-specific input transforms and either a shared creative look in a common working space or approved camera-specific looks. Don't rely on one camera LUT unless it was built and tested for all sources.


For productions using ACES, define each camera’s input transform, the look transform, and the output transform. If your team uses AMF, treat it as the transportable color recipe, which can help reduce the fragile handoff between set, dailies, editorial, VFX, and finishing.


For shows with heavy on-set grading, test the exact handoff path. If the grade uses secondaries, keys, tracking, or proprietary formats, confirm that dailies, editorial references, and finishing can reproduce or at least interpret the intent. Otherwise, simplify dailies color to portable primary corrections and save complex shaping for final grade.


## Where discrepancies usually come from


When dailies don't match the on-set look, the cause is usually one of a few repeat offenders.


Mismatch pattern Likely cause Fast isolation test Usual fix


One camera looks normal and another looks too saturated, flat, green, or contrasty Wrong input color space, gamma, or camera-specific transform Disable the creative look and view each source through only its input transform Assign the correct input transform per camera or build approved camera-specific paths


Image looks overly contrasty or clipped compared with set Normalization was applied twice, such as a LUT plus color management Bypass the technical conversion LUT or the color-managed input transform one at a time Use either the technical LUT or the color-management transform, not both


CDL values seem to push the image differently than the DIT reference CDL is applied in a different order relative to the show look or output transform Compare the same frame with CDL before and after the creative look Match the documented node order from set and keep it consistent for all dailies


Yesterday's dailies match but today's same setup does not LUT, look file, or CDL version changed without a controlled handoff Compare file names, hashes, approval dates, and sidecar metadata between days Lock look versions, archive replaced looks, and publish a clear replacement policy


Render is correct in the dailies system but wrong in review Viewer, browser, OS, or monitor is changing the display appearance Compare the same render on a calibrated reference monitor and in the review platform Keep the render technically correct for the target and document playback limitations


Common failure modes include:


- Your team assigned the wrong input color space to a camera source.
- The pipeline applied both a technical conversion LUT and a color management transform, doubling the normalization.
- The dailies system applied CDLs in the wrong order relative to the show look.
- The DIT updated the LUT, but dailies used an older version.
- Reviewers watched Rec.709 Gamma 2.4 renders in unmanaged players that changed the apparent contrast.


The fastest way to troubleshoot is to isolate the chain. View the source with only the input transform. Then add the CDL. Then add the show look. Then add the output transform. Compare against an approved reference still at each stage if you have one. If the image breaks after a specific node, you have found the neighborhood.


## Make editorial’s copy boring, consistent, and explainable


Editorial doesn't need mystery color. They need dailies that are synced, properly framed, consistent with set, and stable enough to cut against for weeks or months. The DP needs confidence that the creative review image hasn't drifted. The post supervisor needs a pipeline that can be repeated by another dailies colorist tomorrow. Finishing needs metadata that can be traced back to the OCN.


A strong dailies handoff includes the rendered proxies plus the color context behind them. That context can be simple: “A camera LogC4, show LUT v03, CDL per shot, Rec.709 Gamma 2.4 output,” or it can be more formal through[ACES and AMF](https://partnerhelp.netflixstudios.com/hc/en-us/articles/50323214402067-Achieving-Your-Look-Utilizing-AMF-in-an-ACES-Workflow) . Either way, the receiving team should be able to answer three questions without guessing:


- What source color space did dailies assume?
- What creative look did dailies apply?
- What display target did dailies render?


If those answers are clear, most dailies color problems become preventable. If they're missing, even a beautiful on-set look can arrive in editorial as an argument.


## FAQ


Yes, if that LUT was the approved viewing look and the dailies system is feeding it the same type of image in the same order. The LUT must be used with the correct camera input color space, any required CDL values, and the agreed output transform. A LUT built for one log curve or display target shouldn't be blindly applied to every source.


The most common causes are wrong source interpretation, a LUT applied to the wrong log curve, duplicated technical transforms, missing or misplaced CDL values, LUT version mismatches, or differences between the set monitor and the editorial viewing environment. Browser playback, unmanaged QuickTime viewing, and mismatched Rec.709 gamma assumptions can also make a technically correct file appear different.


Each camera source should be identified and normalized correctly before the creative look is applied. An ARRI LogC4 file, a Sony S-Log3 file, and a baked Rec.709 drone file shouldn't all receive the same camera-specific LUT unless that LUT was designed and tested for all of them. Use camera-specific input transforms, camera-specific technical LUTs, or a color-managed workflow such as ACES or Resolve color management.


CDLs are commonly applied after source interpretation or normalization and before the final display transform, but the exact order depends on the show’s chosen workflow. The important point is to document the order, test it, and repeat it consistently. The same CDL values can produce a different result if they're placed before or after a creative LUT.


Yes. Even when proxies have the look baked in, the production should preserve the source filename, reel name, timecode, camera color space, LUT or look version, CDL values, and sidecar files when available. This metadata helps editorial, VFX, conform, and final color trace what was done and relink accurately to the original camera media.


Treat the color recipe as part of the dailies package, not as a separate email attachment. Aspect gives the team one place to store camera media, reference stills, LUTs, CDLs, and notes together so the DIT, dailies colorist, assistant editor, and post supervisor are working from the same shared cloud filespace.
