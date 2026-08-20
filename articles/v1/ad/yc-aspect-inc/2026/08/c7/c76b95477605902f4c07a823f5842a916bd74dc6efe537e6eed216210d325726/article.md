---
schema_version: "1.0.0"
document_id: "c76b95477605902f4c07a823f5842a916bd74dc6efe537e6eed216210d325726"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/guide-to-broadcast-safe-levels-and-sdr-legalization"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-11T12:37:00.177103+00:00"
fetched_at: "2026-08-11T12:37:01.948724+00:00"
content_hash: "sha256:116beade6a55cae0fc22c5856e742c020a86ce316701e93e865d05254de38e94"
---

# Guide to Broadcast Safe Levels and SDR Legalization

If the master is going to broadcast, cable, OTT with a broadcast spec, DVD, or another QC’d delivery path, treat SDR legalization as part of finishing. Grade inside the target color space, watch the scopes while you work, use a soft roll where the image needs to keep texture, and leave a legalizer on the final output as a safety net. If the piece is only going to[normal web platforms](https://larryjordan.com/articles/broadcast-safe-keep-video-levels-legal/) , you may still want sane levels, but you're usually not trying to satisfy a strict broadcast legal range.


That decision matters because “broadcast safe” is a set of limits around luma, chroma, gamut, data levels, and sometimes screen layout. If you only slap on a hard clamp at the end, you can technically pass a meter while damaging highlights, changing saturated colors, or hiding a data-level mistake that will show up after encoding.


## Broadcast safe means signal limits


In SDR finishing, broadcast safe usually starts with Rec.709 video levels. For most HD SDR broadcast deliveries, the picture is expected to stay within legal video range rather than full-range computer RGB.


Common reference points are:


- On a waveform, legal black to legal white is roughly 0 to 100 IRE
- In 8-bit legal video range, luma is 16 to 235, with chroma typically 16 to 240
- In 10-bit legal video range, luma is 64 to 940, with chroma typically 64 to 960
- For SDR broadcast mastering, the color space is Rec.709, commonly monitored at gamma 2.4
- For chroma and gamut, saturated colors must remain within the allowed broadcast color volume, not just within a visible-looking RGB image


The takeaway is that “legal” isn't the same as “looks fine on my monitor.” A shot can look normal and still contain illegal excursions. The most common offenders are clipped skies, hot practicals, neon signage, LED walls, saturated graphics, lower thirds, and logos built in RGB at full-intensity colors.


A picture can look fine while the measured signal still exceeds legal limits. Also, broadcast[safe area](https://www.stvcommercial.tv/wp-content/uploads/2022/05/STVC_EditDelivery_Guide.pdf) is a different thing because safe area is about where text and important graphics sit in the frame so they aren't cropped on consumer displays or downstream presentation. Signal legalization is about electrical or digital level limits. A[delivery spec](https://www.bbcstudios.com/media/6914/contentdeliverybook.pdf) may care about both, but you solve them with different tools.


## The spec decides how strict you need to be


Don't assume every broadcaster uses the same tolerance. Some specs allow brief overshoots. Some reject anything outside a defined threshold. Some want 0 to 100 IRE. Some reference 64 to 940 in 10-bit files. Some specs include explicit chroma limits, line-up requirements,[bars and tone](https://www.bbcstudios.com/media/6914/contentdeliverybook.pdf) , audio loudness, slate, and text-safe rules.


For the finishing team, the useful move is to translate the delivery spec into working rules:


- Target color space and gamma, usually Rec.709 SDR for this workflow
- Required codec, wrapper, bit depth, frame rate, and field order
- Data levels, usually video/legal for broadcast masters unless otherwise specified
- Luma ceiling and floor, usually expressed in IRE or code values
- Chroma and gamut tolerance, often tested by automated QC
- Whether super-white or sub-black values must be clipped, retained, or avoided
- Audio loudness standard, such as[EBU R128](https://tech.ebu.ch/docs/r/r128.pdf) or an ATSC-style target, if the same delivery includes audio QC


Your team should know those items before the online. Blackmagic’s own Resolve documentation makes the same general point: final deliverables have to meet the client’s required signal levels, especially for broadcast, where luma and chroma boundaries can cause QC rejection.


This is also where editorial workflow matters. Dailies and offline media are built to move the production forward, not to prove final signal compliance. A proxy or offline edit can carry temporary LUTs, burned-in graphics, and non-final color. Legalization belongs in the conform, grade, online, and mastering path where the actual camera originals, graphics, transforms, and final deliverable settings are in play.


## Legal range vs full range is where a lot of mistakes start


Many level problems are range interpretation problems.


Full-range and legal-range interpretation can remap the same signal in different ways. Video/legal range maps black and white into a narrower part of the digital code range, while full range uses the full code range. If your team tags, interprets, monitors, or exports something incorrectly, the image may look washed out, crushed, or “illegal” even though the grade itself was fine.


Typical failure modes include:


- Your team drops a full-range graphic into a legal-range timeline and it stays too hot
- An application interprets a legal-range camera file as full range and the image appears lifted or compressed
- Your team exports a full-range intermediate as video range without realizing it
- Another tool re-encodes a file and changes or ignores range metadata
- Your team views Resolve’s scopes without understanding whether they're showing normalized video levels or underlying code values


Confirm the required data level for the codec and delivery, then keep that choice consistent through monitoring, export, and QC. In Resolve, the Deliver page often defaults to Auto for data levels, which can be correct for standard codecs, but Auto isn't a substitute for reading the delivery spec. For a broadcast master, the spec commonly expects Video levels. If a spec asks for Full levels, make that choice deliberate and document it.


One important Resolve-specific trap: don't enable options that retain super-white or sub-black data unless the delivery spec actually wants that. For a legal SDR broadcast file, retaining excursions above legal white or below legal black defeats the point of legalization.


## How legalization changes the image


A legalizer is a limiter for picture levels, and it looks at the outgoing signal and constrains values that exceed the permitted range. The simplest version clips anything above the ceiling or below the floor, while more sophisticated tools compress values as they approach the boundary, which is usually described as soft clipping, soft roll-off, or knee behavior.


For luma, legalization controls how bright and dark the signal can get. For chroma, it limits excessive saturation or colors that fall outside the allowed gamut. For RGB-derived graphics, a legalizer may need to deal with both at the same time.


The key point: legalization is destructive if it has to do too much. If the grade is sitting at 115 IRE and the legalizer is smashing it down to 100, the legalizer flattens the top end. If a logo is wildly saturated and the chroma limiter drags it into range, the brand color may shift. If a scene has saturated highlights, a legalizer can change hue in the brightest areas because luma and chroma are connected in the encoded signal.


A good legalizer pass should be boring, which means it should catch small overshoots.


## Hard clip vs soft roll


Hard clipping is a hard boundary, so anything above the limit becomes the limit and anything below the floor becomes the floor. On scopes, this appears as a flat line at the top or bottom of the waveform or parade.


Hard clipping flattens at the limit, while soft roll bends into it more gradually. Hard clipping is useful when you need absolute containment and the offending values are tiny, such as single-pixel overshoots from sharpening, titles, transitions, or compression math. It's also common as the last stage in a broadcast-safe tool because the output can't exceed the configured maximum.


The image cost shows up when the clipped area is visually meaningful:


- [Highlight texture disappears](https://larryjordan.com/articles/final-cut-pro-x-the-broadcast-safe-effect/) in clouds, skin shine, windows, and practical lights
- Saturated colors can flatten or shift hue
- Gradients can develop a harsh edge near peak white
- Specular detail can turn into a solid patch
- Noise and grain can look abruptly chopped at the boundary


Soft roll is different because instead of waiting until the signal crosses the limit and chopping it, a soft roll starts compressing values near the boundary. It bends the brightest values into range more gradually. In Resolve, this idea appears in Soft Clip controls in the Curves palette and in highlight roll-off decisions throughout the grade.


Soft roll is usually better for image areas you care about. It lets you keep the feeling of brightness without forcing every hot value into the same flat ceiling. The tradeoff is that it changes contrast near the top or bottom of the range, so the colorist should do it as part of the look.


A simple rule works well: use soft roll in the grade for real picture content, then use broadcast safe clipping as the guardrail.


Approach What it does Best use Main risk


Hard clip Forces anything above or below the limit to the exact boundary Tiny overshoots, single-pixel hits, titles, transitions, final output protection Can flatten highlights, crush shadows, chop grain, or create harsh edges


Soft roll Compresses values as they approach the boundary so they enter range gradually Highlights, shadows, skin shine, skies, practicals, and other visible picture content Changes contrast near the top or bottom of the image if pushed too far


Combined workflow Uses soft roll during the grade, then a hard safety limit at the output Most SDR broadcast finishing workflows Can hide upstream grading or range mistakes if the final legalizer is doing too much work


## Setting Resolve up for SDR broadcast levels


You can use Resolve for grading and mastering a broadcast file, but the setup has to match the delivery. The exact menu names vary a bit by version, so think in terms of these controls rather than memorizing one UI path.


Start with the project and timeline color management:


- Set the timeline/output color space to Rec.709 for SDR broadcast work
- Monitor in the expected gamma, commonly gamma 2.4 in a controlled room
- Match video monitoring/data levels to your hardware path and reference display setup
- Set Deliver page data levels to Video for most legal broadcast masters, unless the spec says otherwise
- Leave retain super-whites/sub-blacks off for normal legalized broadcast output


Those settings create the environment where the scopes and monitor mean what you think they mean. If the monitor path is wrong, you can legalize a signal that still looks wrong to the viewer. If the export data levels are wrong, a file can pass your eye test and fail downstream.


Then turn on the tools that show you problems while you work. Resolve includes video scopes on the Color page, and Resolve documentation references broadcast safe exception viewing. Use the waveform for luma, RGB parade for channel behavior, and vectorscope for saturation and hue. If your version exposes a broadcast safe exceptions overlay or gamut warning view, enable it while investigating problem shots rather than leaving it on all day. Warnings are most useful when you can correlate them with the scopes and the image.


For the actual legalizer, Resolve has had project-level broadcast safe controls such as “[Make Broadcast Safe](https://borisfx.com/blog/how-to-achieve-broadcast-safe-in-davinci-resolve/) ” or Broadcast Safe settings depending on version. Use the preset or threshold that matches the spec. If the client gives you a specific legal range, configure the legalizer to that range rather than picking the most aggressive option by habit.


A common Resolve finishing structure is:


- Use clip nodes for creative balance, contrast, secondaries, and shot matching
- Use timeline or group post-clip nodes for the show LUT, output transform, global trim, and soft clipping
- Use a final output safety stage with the broadcast safe/legalizer set to the delivery range


This order matters because if you legalize too early and then add contrast, sharpening, titles, or a color transform afterward, you can create new illegal values after the legalizer. Keep the final guardrail late in the chain.


The final broadcast-safe guardrail belongs late in the finishing chain.


## Where to solve common illegal-level problems


The best fix depends on what is causing the excursion.


For hot highlights in camera footage, start with highlight recovery, HDR wheels, curves, or Soft Clip. Bring the highlight into a nicer roll before the legalizer sees it. If the shot still has occasional single-pixel hits, the output safe tool can catch them.


For crushed blacks, decide whether the content actually needs to sit at legal black or whether you're losing detail. Raising the floor globally can make the scene milky. A toe adjustment or luma curve is usually better than a blunt lift.


For saturated graphics, fix the graphic if possible because computer-generated colors are a frequent source of chroma violations because pure RGB primaries can exceed broadcast-friendly saturation. If you can't revise the source, use a gamut limiter or saturation adjustment targeted to the offending hue. A final chroma clamp can protect the master, but it may change brand colors.


For mixed camera and graphics timelines, check transitions. Dissolves, glows, titles, sharpeners, and resizes can create overshoots even when the source clips are legal. That's one reason the final safety stage belongs after finishing effects.


For archival or stock footage, watch both range and gamut. Older masters may contain super-white values, sub-black setup issues, or encoded levels that aren't tagged cleanly. Fix interpretation first, then grade.


## Reading scopes without chasing ghosts


Scopes show whether the signal is inside the permitted container. A low-key night scene may live mostly in the lower third of the waveform and be perfectly correct. A white cyc may sit near the top and still need texture below peak white.


Use the waveform to find luma excursions. Use RGB parade to see if one channel is causing a color-specific problem. Use the vectorscope to spot saturation outside normal targets. When a QC report identifies a timecode, go to that frame and compare all three. A luma error and a gamut error can occur in the same frame, but the fix may not be the same.


Don't grade only to the scopes, though. Legal and good-looking are separate goals. The best SDR broadcast master is one where the creative grade already lives comfortably inside the required range, and the legalizer only catches edge cases.


## Audio is adjacent, but it isn't video legalization


Delivery specs often bundle video legality with audio loudness, so your team should keep the distinction clear. Video broadcast safe deals with luma, chroma, gamut, and data levels, while audio compliance deals with programme loudness, loudness range, and true peak. EBU R128, for example, uses programme loudness and[maximum true peak](https://www.itu.int/dms_pubrec/itu-r/rec/bs/R-REC-BS.1770-5-202311-I!!PDF-E.pdf) concepts rather than old peak-only mixing.


Don't let the shared word “levels” confuse the workflow. A legal video signal doesn't mean compliant audio, and a loudness-normalized mix doesn't mean legal picture. They're separate QC domains that happen to meet at delivery.


## The safest workflow is conservative


The strongest approach is to make the image legal before the legalizer has much work to do. Keep the grade in Rec.709 SDR, manage data levels intentionally, use soft roll for highlights and shadows that matter, tame saturated graphics at the source, then use Resolve’s broadcast safe control as the final output limiter.


That approach leaves you with a master that has a real chance of passing automated QC, and a picture that still looks like the grade you approved.


## FAQ


For SDR video, broadcast safe means the final signal stays within the delivery spec for luma, chroma, gamut, and data levels. In a typical Rec.709 HD broadcast workflow, legal luma is roughly 0 to 100 IRE, which corresponds to 16 to 235 in 8-bit video range or 64 to 940 in 10-bit video range. Chroma also has limits, so a shot can pass luma limits but still fail gamut or saturation checks.


No. Broadcast safe refers to signal legality, such as luma, chroma, gamut, and legal video range. Title safe and action safe refer to screen layout, meaning whether text, graphics, and important action are positioned far enough from the frame edges to survive different displays or presentation paths. A delivery spec may require both, but they're checked and fixed with different tools.


Use soft roll-off when the clipped area is visually important, such as clouds, skin highlights, windows, practical lights, or bright gradients. Soft roll compresses values gradually into range and usually preserves a more natural image. Hard clipping is better as a final safety boundary or for tiny excursions, but it can flatten highlights, chop shadows, and shift saturated colors if it's doing too much work.


The final broadcast safe legalizer should be late in the chain, after creative grading, output transforms, titles, resizes, sharpening, and finishing effects. If you legalize too early, later processing can create new illegal values. A common approach is to handle creative fixes and soft clipping in clip, group, or timeline nodes, then leave a final broadcast safe limiter as the last guardrail before output.


A file can look fine on a monitor while still containing illegal signal values. Common causes include super-white highlights, sub-black values, saturated RGB graphics, gamut excursions, incorrect full-range or video-range tagging, and legal values created or broken during transcoding. Automated QC tools measure the signal against the spec, not just the apparent look of the image.


QC notes are most useful when they land on the exact frame, with enough context to identify whether the issue is luma, chroma, gamut, a graphic, or an export setting. Aspect supports frame-accurate comments and annotations inside the review workflow.
