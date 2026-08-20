---
schema_version: "1.0.0"
document_id: "03717aa170891e13573b516bd74f991081c34901b597c1e2a7779f8b423a32fa"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/camera-workflows/canon-log-2-vs-canon-log-3-which-curve-to-choose"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-17T20:24:08.329587+00:00"
fetched_at: "2026-08-17T20:24:09.115432+00:00"
content_hash: "sha256:ed6d3609e5b763cdd786d390a6c095911e02a162a4faa0b4c8da7ef9e148d80e"
---

# Canon Log 2 vs Canon Log 3: Which Curve to Choose

If you're choosing a Canon log curve for a normal production day, Canon Log 3 is the best starting point. It gives you a strong log workflow, better highlight handling than original Canon Log, cleaner shadows than Canon Log 2, and a faster path through editorial and grading.


Canon Log 2 is the right choice when the shot actually needs the extra dynamic range and the post pipeline is ready for it. That usually means controlled exposure, 10-bit or RAW capture, proper monitoring LUTs on set, and a colorist who knows the footage is Log 2 before they start matching cameras.


The mistake is treating Canon Log 2 as “better” because the number sounds higher or because it can hold more range. It's a more demanding curve, and if the scene, camera, codec, exposure, and finishing schedule don't benefit from it, Log 3 will usually get you to a better image with less pain.


## The short version for real jobs


Canon Log 2 is Canon’s maximum dynamic range curve. Canon developed it for cameras such as the EOS C300 Mark II and C700 generation to manage[roughly 15 stops](http://downloads.canon.com/nw/learn/white-papers/cinema-eos/White_Paper_Deep-Dive-HDR-Part2.pdf) of dynamic range and fit into cinema-style workflows. It gives more room for highlight and shadow information, especially when the sensor and recording format can support it.


Canon Log 3 is the more forgiving production curve. It keeps much of the highlight benefit of log, extends beyond original Canon Log, and is designed to be easier to grade. Canon’s own materials describe Log 3 as having[narrower shadow tones](https://cam.start.canon/cs/C018/guide2/html/CP-02_Settings_0030.html) than Log 2, which is part of why it generally shows[less noise in the shadows](https://www.canon-europe.com/pro/stories/enhance-filmmaking-with-canon-log/) and turns around faster.


That gives you a useful rule:


- Use Canon Log 3 for interviews, doc, branded, corporate, social, event, multicam, fast-turnaround editorial, and most Rec.709 delivery work.
- Use Canon Log 2 for high-contrast narrative, commercial, HDR, VFX-heavy plates, controlled lighting, and shots where protecting the full sensor range matters more than speed.
- Either curve is a bad choice if nobody downstream is prepared to transform log footage correctly. Log footage is supposed to look flat out of camera and needs post-processing.


The important point is that Log 2 is a capture decision and a post decision at the same time. You don't get its benefit just by selecting it in camera. You get the benefit when the whole workflow preserves, transforms, and grades it correctly.


Decision area Canon Log 2 Canon Log 3


Main advantage Maximum dynamic range when the sensor, exposure, and recording format support it Easier, cleaner log workflow for most productions


Shadow behavior More shadow gradation, but thin exposures can reveal noise quickly Narrower shadow tones, usually less visible shadow noise


Grading demand Best with a colorist, correct monitoring LUTs, and tested transforms Faster to normalize, match, and move through editorial


Best fits HDR, high contrast narrative, commercials, VFX plates, controlled lighting Interviews, doc, corporate, event, multicam, SDR Rec.709


Main risk if misused More noise reduction, harder matching, inconsistent dailies Less total latitude than Log 2 when the scene needs full sensor range


## What the curves are doing


A log curve redistributes tonal values so the camera can record a wider range between clipping white and crushed black. Instead of using code values in a way that looks contrasty on a standard display, the curve compresses scene brightness into a flatter image that can survive grading.


That's why Canon Log footage looks dull, low contrast, and desaturated before it's transformed. That look is the working state.


Canon has three related log curves in common circulation:


- Canon Log: the original curve, designed around earlier Cinema EOS cameras and an 800% dynamic range target.
- Canon Log 2: the maximum dynamic range curve, developed for newer Cinema EOS sensors and Cineon-style workflows.
- Canon Log 3: the easier grading curve, sitting between original Canon Log and Log 2 in workflow difficulty.


For this decision, the useful distinction is how the curve spends tonal precision across shadows, mids, and highlights.


A simple comparison of how two log curves can allocate shadow detail differently. Canon Log 2 allocates more room to shadow gradation and maximum range. Canon’s custom picture guidance describes Log 2 as having wide shadow tones with rich shadow gradations, and being close to film characteristics. That makes it attractive when you want to shape exposure in the grade and preserve tonal detail across a wide scene.


Canon Log 3 compresses the shadows more. Canon describes it as having narrower shadow tones, with images closer to BT.709 behavior while extending highlights beyond original Canon Log. In production terms, that means less shadow flexibility than Log 2, but also less visible shadow noise and less grading work.


## Dynamic range is only useful if the image survives the workflow


The headline advantage of Canon Log 2 is dynamic range. On cameras built to support it well, Log 2 is the curve you choose when you want the most latitude the camera can offer.


That doesn't mean every Log 2 file has more usable dynamic range than every Log 3 file. The final result depends on the sensor, ISO, exposure, bit depth, codec, noise floor, and grade. If you underexpose Log 2 and then lift the shadows, you may reveal noise faster than you reveal useful detail.


Log 2 is most valuable in scenes like these:


A high-contrast room with a bright window illustrates where maximum dynamic range matters.


- Bright exterior windows in an interior scene where the window detail matters.
- Day exterior work with clouds, white wardrobe, reflective surfaces, or sky detail that needs to hold.
- HDR finishing where extra highlight information will actually be used.
- VFX plates where retaining scene information helps pulls, keys, relighting, or integration.
- Narrative or commercial work with time for shot-by-shot grading.


In those cases, Log 2 gives the colorist more room to decide where contrast should land. It's especially useful when the production is already exposing with false color, waveform, or a calibrated monitor LUT rather than guessing from the flat image.


Log 3 is stronger when the job needs consistency more than maximum latitude. If the camera is moving quickly through mixed lighting, if assistant editors need proxies that look sane, or if the grade needs to move fast, Log 3 usually wins. It protects highlights well enough for most deliveries and keeps the shadows from becoming the whole conversation.


## Noise is the tradeoff people feel in post


The most common complaint with Canon Log 2 is noisy shadows. That isn't because Log 2 “adds noise.” A log curve doesn't create sensor noise, but it changes how tonal information is encoded and how aggressively you're likely to lift, stretch, and reshape the image later.


Log 2 preserves more shadow gradation, which is great when the shadows are properly exposed and the camera has enough signal. But if the shot is thin, those wide shadow tones can expose the noise floor during normalization and grading.


Log 3 is often cleaner because it's less ambitious in the shadows. Canon’s own comparison frames this as a workflow advantage: Log 3 has narrower shadow tones and is faster to grade. For editors and post supervisors, that usually means fewer shots needing noise reduction, fewer awkward shadow matches, and fewer surprises when a LUT or color managed transform is applied.


A good way to think about it:


- Log 2 gives you more shadow information to work with, including more of the problems.
- Log 3 gives you less shadow freedom, but the image usually behaves better faster.
- If the exposure isn't disciplined, Log 2 can punish you harder than Log 3.
- If the crew lights and exposes the scene properly, Log 2 can give the colorist more subtle control.


This is why “we’ll shoot Log 2 to be safe” is often backwards. Log 2 is safer for highlight-heavy scenes with a controlled pipeline. Log 3 is safer for everyday production uncertainty.


## Camera support isn't the same across the Canon lineup


Canon Log 3 is widely available across recent Canon EOS and Cinema EOS cameras. Canon Log 2 is more selective. Your team shouldn't standardize a show around one curve until you confirm the exact camera bodies, firmware versions, recording modes, and RAW or compressed formats.


Canon’s Cinema EOS line has long supported log workflows, but the available curves vary by model and generation.[Canon Log 2 appeared](https://www.usa.canon.com/content/dam/canon-assets/white-papers/pro/white-paper-canon-log-gamma-curves.pdf) with cameras such as the EOS C300 Mark II and is also associated with higher-end Cinema EOS bodies like the C700 and C700 FF. Later cameras such as the C300 Mark III support both Canon Log 2 and Canon Log 3, along with formats such as Cinema RAW Light and XF-AVC.


On hybrid EOS bodies, Log 3 is often the realistic common denominator. For example, the[EOS R5 received Canon Log 3](https://www.usa.canon.com/support/canon-product-advisories/firmware-notice-eos-r5-firmware-version-1-3-0) through firmware, allowing users to select Canon Log or Canon Log 3 in the Canon Log settings. Newer bodies such as the EOS R5 Mark II and EOS R1 include custom picture guidance covering Canon Log 2 and Canon Log 3, but support still depends on the body and mode.


For mixed-camera jobs, the support questions that matter are specific:


Mixed camera packages may not share the same log curve options.


- Does every camera on the job support the chosen gamma curve?
- Is the selected curve available in the recording mode you're using?
- Is the footage recorded in 10-bit or RAW, rather than a fragile 8-bit format?
- Are the cameras using the same color space, such as Canon Cinema Gamut or BT.709?
- Are monitoring LUTs available for the chosen gamma and gamut combination?


If one body can shoot Log 2 and another can only shoot Log 3, forcing everything into Log 2 isn't an option. In that case, Log 3 may be the better production standard, even if the A-camera could technically capture more range.


## Color space matters as much as the log curve


“Canon Log 3” by itself isn't a complete color management description. You also need to know the gamut.


A clip might be Canon Log 3 with Canon Cinema Gamut, or Canon Log 3 with a Rec.709-related color space, depending on the camera and settings. Those aren't interchangeable in post. If Resolve, a LUT, or a color space transform is told the wrong input gamut, the image may look oversaturated, undersaturated, hue-shifted, or just subtly wrong.


For post teams, your handoff should name both parts:


- Gamma: Canon Log 2 or Canon Log 3.
- Gamut: Canon Cinema Gamut, BT.709, or the exact camera setting used.
- Recording format: RAW, Cinema RAW Light, XF-AVC, MP4, or other.
- Data levels if known, though most Resolve workflows should leave levels on Auto unless there's a known issue.
- Monitoring LUT used on set, if any.


That last point matters because a production LUT can easily become confused with a technical transform. A viewing LUT on set helps the crew judge a normalized image. It doesn't mean the recorded file is Rec.709, and it doesn't mean editorial should bake that LUT into dailies unless the workflow calls for it.


Canon cameras can apply LUTs to[video outputs such as SDI](https://support.usa.canon.com/kb/s/article/ART174711) , monitor, viewfinder, or HDMI depending on the model. That's useful for judging exposure and contrast while still recording log. Your team should document the monitoring path and the recorded path separately.


## How Canon Log 2 and Log 3 behave in Resolve


DaVinci Resolve can handle Canon Log 2 and Canon Log 3 cleanly, but only if you use the correct input transform. The two common approaches are Resolve Color Management or node-based Color Space Transform.


Log footage needs the correct transform before it becomes a normal viewing image. In a Resolve Color Managed project, you assign the input color space and gamma for the clips, then let Resolve transform them into the timeline and output color spaces. This is clean for larger projects because it keeps normalization consistent across timelines, renders, and trims.


In a node-based workflow, you leave the project in DaVinci YRGB and use a[Color Space Transform node](https://www.youtube.com/watch?v=VdbPhOSX2pI) or a technical LUT to convert from Canon Log 2 or Canon Log 3 into the working or display space. Many colorists prefer this because it gives clear node-level control over where the transform happens.


Both approaches can work, but the failure mode is mismatched metadata or assumptions. Resolve doesn't always identify every Canon log clip automatically, especially from some mirrorless bodies and compressed formats. If the image looks wrong in a color managed project but correct through a manual CST, the clip may not be tagged with the input space you think it's using.


For Canon Log 3 footage, common input descriptions might include Canon Log 3 gamma with Canon Cinema Gamut. For Canon Log 2, the input should be Canon Log 2 with the correct gamut. If the clip was shot in a BT.709 color space with a log gamma, treating it as Cinema Gamut would be incorrect.


The symptoms of a wrong transform are familiar:


- Skin tones lean strange even after white balance correction.
- Saturation feels too intense or too weak after normalization.
- Highlights roll off abruptly or look gray and compressed.
- Shadows lift with odd color contamination.
- A Canon technical LUT looks better than the managed transform, or the reverse, because the inputs don't match.


When that happens, your first step isn't building a creative grade to fight the problem. Confirm the camera setting, clip metadata, and Resolve input assignment. Most “Canon color science” complaints in post are really transform mismatches.


## LUTs, log-to-log transforms, and dailies


Canon provides LUTs for converting Canon Log, Canon Log 2, and Canon Log 3 footage into standard viewing spaces, and also offers[log-to-log options](https://color.ssw.imaging-saas.canon/color/en/pc/log/) intended to preserve log data for later grading. That distinction matters.


A log-to-Rec.709 LUT is usually for viewing, editorial, client review, or a simple finishing path. A log-to-log LUT is for changing the tonal and color encoding while keeping the material in a grading-friendly log state.


For assistant editors and post supervisors, your team should make the dailies decision before footage arrives:


- Editorial proxies can have a viewing LUT baked in if offline review needs a normal-looking image.
- Your team should leave camera originals untouched and clearly labeled.
- Your colorist should know whether dailies were made from Canon Log 2 or Canon Log 3.
- If the show uses a show LUT, your team should document it separately from the technical input transform.
- If the finish is HDR, your team shouldn't bake SDR assumptions into anything that will be used for color decisions.


The takeaway is simple: A LUT expects a specific input gamma, input gamut, range, and target. Applying a Canon Log 3 LUT to Canon Log 2 footage may produce an image, but not a trustworthy one.


## When Canon Log 2 is worth it


Canon Log 2 is worth the extra grading complexity when the project can actually spend the extra latitude.


That usually means the A-camera has a sensor and recording format that support the range, the crew can expose carefully, and post has time for a proper grade. It also helps when the final image benefits from nuanced highlight and shadow shaping, not just a quick Rec.709 conversion.


Good Log 2 candidates include:


- Cinema EOS productions recording Cinema RAW Light or robust 10-bit XF-AVC.
- HDR commercials, documentary specials, and narrative work with high-contrast scenes.
- Controlled interviews with bright windows or practicals that need to hold detail.
- Green screen, screen replacement, and VFX plates where retained tonal information matters.
- Projects with a dedicated colorist and a tested Resolve, ACES, or color managed pipeline.


Log 2 is less attractive when the project is underexposed, rushed, heavily mixed with non-Log-2 cameras, or finishing through template LUTs. In those cases, the theoretical latitude can turn into more noise reduction, more matching time, and more inconsistent dailies.


If the question is “Can we make Log 2 look better?” the answer is yes. If the question is “Will Log 2 make this job easier?” the answer is often no.


## When Canon Log 3 is the better production choice


Canon Log 3 is the right choice for most teams because it balances latitude with reliability. It gives enough log flexibility for exposure correction and creative grading, while keeping the image closer to a predictable post path.


It's especially useful when:


- Multiple Canon bodies need to match.
- The final delivery is SDR Rec.709.
- The production has limited on-set monitoring.
- The footage will pass through assistant editors, producers, clients, and review exports before final color.
- The camera is a hybrid EOS body where Log 3 is better supported than Log 2.


For many productions, the goal is to capture a robust image that survives the whole pipeline. Log 3 is often the curve that does that with the fewest surprises.


## A sane default for Canon shows


For a team Canon workflow, Canon Log 3 should be the default unless someone can explain why a specific scene, camera, or deliverable needs Canon Log 2.


That explanation should include the camera body, recording format, gamut, monitoring plan, and Resolve transform. If those details are known and tested, Log 2 can be excellent. If they aren't, Log 3 is usually the smarter call.


The best Canon log choice is the curve that gives the production enough image information and gives post a clear, repeatable path to the finish.


## FAQ


Not automatically. Canon Log 2 can preserve more dynamic range on cameras and recording formats that support it well, but it's more demanding to expose and grade. Canon Log 3 is usually the better default for everyday production because it's cleaner in the shadows, easier to normalize, and faster to match across shots.


Canon Log 2 doesn't add sensor noise, but it preserves and exposes more shadow information. When footage is underexposed or heavily lifted in the grade, the noise floor becomes more visible. Canon Log 3 uses a less aggressive shadow encoding, so it often appears cleaner and requires less noise reduction.


For most SDR Rec.709 jobs, Canon Log 3 is the safer choice. It provides useful highlight protection and grading flexibility without the extra complexity of Canon Log 2. Canon Log 2 is more appropriate when the scene has extreme contrast, the exposure is controlled, and the grade can take advantage of the added latitude.


Sometimes, but you shouldn't rely on automatic detection. Resolve may not correctly tag every Canon clip, especially from some mirrorless cameras or compressed recording modes. Confirm the input gamma and gamut manually, such as Canon Log 3 with Canon Cinema Gamut or Canon Log 2 with the correct gamut, before building the grade.


Yes, but each group of clips needs the correct input transform. In Resolve Color Management, assign the proper input color space and gamma per clip or camera group. In a node workflow, use the correct Color Space Transform or technical LUT for each source. Don't apply a Canon Log 3 LUT to Canon Log 2 footage unless that's specifically what the LUT was designed for.


Create review media with the intended viewing transform while keeping the camera originals untouched for color. Aspect supports automatic generated proxies and previews, which lets reviewers see usable media without every stakeholder downloading heavy Log 2, Log 3, RAW, or XF-AVC files.
