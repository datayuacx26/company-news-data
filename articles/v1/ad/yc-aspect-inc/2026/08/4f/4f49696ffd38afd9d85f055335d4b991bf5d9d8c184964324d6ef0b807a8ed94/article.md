---
schema_version: "1.0.0"
document_id: "4f49696ffd38afd9d85f055335d4b991bf5d9d8c184964324d6ef0b807a8ed94"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/guide-to-closed-caption-and-subtitle-file-delivery"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-14T23:35:05.778838+00:00"
fetched_at: "2026-08-14T23:35:06.585712+00:00"
content_hash: "sha256:9bce466f0823a4b50e8c8b7623522be7146e323d4209f0eb522b8d607b50ef83"
---

# Guide to Closed Caption and Subtitle File Delivery

Caption delivery starts with one decision: deliver the timed text format the platform asked for, matched to the exact video file you're delivering. The final caption or subtitle file needs to line up with the final mezzanine, including runtime, frame rate, start timecode, logos, slates, acts, blacks, and any last-minute picture trims.


That sounds obvious until a show has six different masters, three vendors, a festival version, a domestic version, a textless version, a dubbed version, and a platform spec that uses the word “subtitle” when it really wants SDH captions. Most caption problems are delivery-spec problems.


The goal is to treat captions like any other finishing asset. Your team needs a source of truth, a format target, version control, and QC against picture.


## Captions, subtitles, SDH, and forced narratives aren't the same deliverable


Before choosing a file format, make sure everyone is talking about the same kind of timed text. People use the words loosely in post, but delivery specs often use them very specifically.


Timed text type Primary viewer need Usually includes Common delivery note


Closed captions Accessibility for deaf or hard of hearing viewers Dialogue, speaker IDs when needed, meaningful music, sound effects, off-screen speech Often subject to strict platform or broadcast caption rules


SDH subtitles Accessibility-style subtitles, often in subtitle workflows Dialogue plus relevant non-speech audio and speaker context May be requested instead of traditional closed captions by streamers


Full subtitles Translation or transcription for viewers who can hear the audio Spoken dialogue, sometimes translated on-screen text Not the same as SDH unless non-speech accessibility cues are included


Forced narratives Limited translation for foreign dialogue or necessary on-screen text Only the lines or text the primary-language viewer needs Should not be delivered as a replacement for full subtitles or captions


Dub subtitles Subtitle support for a dubbed audio version Text matched to the dub script or dub timing, depending on spec Confirm whether the platform wants dub-matched text or original-language subtitle translation


Closed captions are designed for viewers who are deaf or hard of hearing. They include spoken dialogue plus relevant non-speech audio, which can mean speaker IDs, music cues, sound effects, off-screen speech, tone of delivery, and plot-relevant audio that isn't visible on screen.


Subtitles usually translate or transcribe dialogue for viewers who can hear the program audio. A dialogue-only English subtitle file isn't the same thing as an English SDH or closed caption file. If a platform asks for English SDH and receives a clean dialogue subtitle file, it can fail QC even if every spoken word is correct.


Forced narratives are a smaller category. They're usually used for foreign-language dialogue or on-screen text that the viewer needs to understand when watching in the primary language. For example, an English-language film with a short Spanish conversation may need an English forced narrative track for that exchange. Forced narratives aren't a substitute for full subtitles or captions.


A typical timed text package may include several different assets:


- English closed captions or English SDH for accessibility
- Full subtitles for translated languages
- Forced narrative subtitles for foreign dialogue or important on-screen text
- Dub subtitles, if required by the platform or localization workflow
- Textless elements or reference notes for graphics that are handled outside the subtitle file


The key takeaway is simple: don't let “we've an SRT” become shorthand for “captions are done.” Ask what kind of timed text is required, what language it covers, and what user experience it's meant to support.


## Pick the format from the destination


Most video editing tools can export several caption formats. That doesn't mean all of them are acceptable for delivery. A format that works fine for review on YouTube may be wrong for a studio, broadcaster, streamer, or aggregator.


Here are the[common formats](https://videocentral.amazon.com/support/delivery-experience/timed-text) you'll run into in delivery conversations:


Format Extension Common use Main limitation


SRT .srt Web platforms, screeners, simple sidecar subtitles Minimal styling, limited positioning, no rich metadata


SCC .scc North American broadcast-style closed caption workflows, CEA-608 heritage Legacy constraints, limited character set and formatting


STL .stl EBU subtitle delivery, common in some broadcast and international workflows Region/spec dependent, can be easy to misconfigure


TTML / IMSC / SMPTE-TT .ttml, .xml, sometimes .dfxp Streamers, IMF-adjacent workflows, modern platform delivery More complex, strict validation, spec-specific profiles


SRT is the most forgiving and the easiest to inspect in a text editor. It contains cue numbers, time ranges, and text, and that simplicity is why it's everywhere, but also why it's often not enough for professional delivery. If the platform needs positioning, styling, speaker treatment, language metadata, frame rate declarations, or profile-specific validation, SRT may not carry what QC expects.


SCC is common when a spec is rooted in traditional closed caption delivery. It's associated with 608-style caption data and is still requested by some video services. It can be the correct answer when a spec asks for it, but it isn't a universal master format. You need to be careful with encoding, frame rate, drop-frame behavior, and caption placement.


STL usually means EBU STL in delivery discussions. It shows up often in broadcast and international subtitle workflows. It can carry more structure than SRT, but the exact requirements vary by client and territory. Don't assume one house’s STL export settings will pass another broadcaster’s ingest.


[TTML is a family](https://www.w3.org/TR/ttml-imsc1.3/) , not one single delivery behavior. Specs may refer to TTML, SMPTE-TT, IMSC, or a branded platform profile. This is where many modern streamer requirements live because TTML-based formats can support global languages, positioning, styling, and stricter validation. The tradeoff is that a technically valid TTML file may still fail if it doesn't match the exact profile requested.


If the delivery spec names a format, use that format. If it names a profile, use that profile. If the request is vague, ask before exporting. “Can we send SRT?” is much cheaper to resolve before localization than after a QC rejection.


The destination spec determines which timed-text format fits.


## Sidecar delivery is usually safer than embedding


For professional delivery, sidecar timed text files are usually the cleanest path. A[sidecar file](https://www.youtube.com/watch?v=efeteVKqPUo) travels next to the video master as a separate asset, and it can be replaced, versioned, language-tagged, validated, and re-delivered without re-encoding picture.


Embedded captions live inside the video file. That can be useful for certain broadcast masters or archive workflows, but it's risky if the destination doesn't want embedded tracks. Some services explicitly reject video submissions that contain[embedded closed caption tracks](https://contentguide.universalmusic.com/closed-captioning-help/) and require captions as separate SCC or TTML files. Others may accept embedded captions only for specific containers or distribution paths.


Burned-in subtitles are different again. They're rendered into the picture and can't be turned off. Use them only when the spec or creative intent calls for open captions, burned-in foreign dialogue, festival screeners, or review copies. Burned-in text is almost never a substitute for accessibility captions because the platform can't expose it as a selectable caption track.


The delivery choice usually falls into these buckets:


- Sidecar captions or subtitles for most streamers, platforms, aggregators, and localization workflows
- Embedded captions when a spec explicitly requests them in the video container
- Burned-in text for open-caption versions, festival copies, or creative on-screen translation
- Both sidecar and burned-in reference only when the recipient asks for both and understands which is authoritative


The safe default is sidecar delivery against the exact master. If someone asks for embedded captions, confirm the container, codec, caption standard, and whether the sidecar is still required.


A sidecar caption file stays separate from the master but travels with it.


## Timing has to match the delivered master


Caption sync issues often come from using the wrong reference file. A vendor may time captions to an H.264 review export that starts at 00:00:00:00, while the final mezzanine starts at 01:00:00:00 and includes two seconds of black at head. Or editorial may trim a logo after captions were approved. Or a distributor may require bars, tone, slate, or act breaks that weren't in the caption reference.


Your team should conform every timed text asset to the length and timing of the accompanying video. Some platform specs allow a small tolerance, such as subtitles conforming[within half a second](https://partnerhelp.netflixstudios.com/hc/en-us/articles/7357416307603-Localization-Accessibility-and-Dubbing-Branded-Delivery-Specifications) , but you shouldn't use that as working slack. Half a second late is very visible in dialogue, and a few frames can matter on fast exchanges, comedy timing, or music performance.


Your team should verify timing in the context where the file will be delivered:


- Confirm the video file used for caption timing is the same version as the delivery master
- Confirm runtime, start timecode, and frame rate
- Confirm captions don't start before first frame of program or extend past end of file
- Confirm any slate, logo, recap, teaser, credits, or post-credit scene is handled correctly
- Confirm mastering didn't shift act breaks, commercial blacks, and texted sections


The common failure mode is that the whole file is offset by a consistent amount, or that sync is good until a version change. If the first caption is right and the last caption is wrong, look for a missing shot, different bumper, conformed frame rate issue, or timebase conversion problem.


A small version difference can make captions drift after starting in sync. For long-form work, spot-checking only the first minute isn't enough. Verify the head, several points through the middle, dense dialogue scenes, music or montage sections, credits, and the tail. If there were late picture changes, check around every changed area.


## Text rules are part of delivery


Caption and subtitle text has to be readable under real playback conditions. That means line length, duration, line breaks, speaker labels, punctuation, and placement all affect whether the asset passes QC and whether viewers can use it.


Different platforms publish different style rules, but most specs are trying to control the same things:


- Maximum characters per line
- Maximum lines per subtitle or caption event
- Minimum and maximum duration per event
- Reading speed, often measured as[characters per second](https://www.paramount.com/sites/g/files/dxjhpe356/files/2024-07/Global_Content_Delivery_Guide_V2.2.pdf)
- Line break rules for grammar and readability
- Treatment of speaker IDs, off-screen dialogue, and overlapping speakers
- Treatment of music, lyrics, and sound effects
- Placement to avoid lower thirds, burned-in text, credits, and other plot-relevant graphics
- Language-specific punctuation, casing, and character support


SRT is especially easy to over-trust here because it will let you write almost anything. A player may display long lines poorly, wrap text in unexpected places, or cover lower thirds. A platform may accept the upload but produce bad viewer results. More structured formats can define positioning and styling, but they can also fail validation if those details are outside spec.


For closed captions and SDH, include meaningful[non-speech information](https://partnerhub.warnermediagroup.com/ingest-specifications/component-delivery/subtitles-cc) without turning the file into an audio commentary track. “\[door opens\]” may matter if it explains why a character reacts, while “\[soft synth pad continues under dialogue\]” probably doesn't, unless the music cue is story-relevant. Use judgment, but follow the receiving platform’s house style if it has one.


Speaker identification is another area where files drift between acceptable and messy. If the speaker is visually obvious, many styles don't require a label. If the speaker is off screen, on a phone, behind a door, or one of several overlapping voices, a label can be necessary. Consistency matters more than personal preference.


On-screen text needs a deliberate decision. If narrative burned-in text is in a language the viewer doesn't understand, subtitle specs often require translation. If on-screen text overlaps with dialogue, the most plot-relevant message usually wins. Don't let automated caption generation decide this for you.


## QC captions against picture, audio, and the delivery spec


Caption QC is a three-way comparison between the timed text file, the final picture/audio, and the delivery requirements. A caption file can be beautifully written and still be the wrong asset type, wrong language, wrong timebase, or wrong profile.


Caption QC compares the timed-text file, final picture and audio, and delivery requirements together. A useful caption QC pass looks at several categories:


- Technical validity: file opens, parses, validates, and matches the requested extension and format
- Asset identity: language, territory, version, episode number, cut name, and file naming match the delivery request
- Timing: captions appear and disappear in[sync with speech](https://partnerhelp.netflixstudios.com/hc/en-us/articles/360051554394-Timed-Text-Style-Guide-Subtitle-Timing-Guidelines) and meaningful audio
- Completeness: all required dialogue, SDH information, forced narrative text, credits, and plot-relevant on-screen text are covered
- Readability: line length, reading speed, line breaks, and event duration feel usable
- Placement: captions avoid burned-in text, lower thirds, credits, and important visual information where the format supports positioning
- Text accuracy: spelling, names, lyrics, terminology, censored words, and punctuation match the approved program and spec
- Playback behavior: the file displays correctly in a player or validation tool that's relevant to the destination


QC the actual deliverable by exporting the sidecar file, importing or loading it against the final master, and watching it as the viewer or ingest system will see it. If the platform provides a validator, use it. If a post house or caption vendor provides a conformance report, keep it with the delivery package.


Automated transcription and auto-caption tools can help create a first pass, especially for rough cuts, screeners, and internal review. Your team shouldn't treat them as final delivery assets without human editing and timing review. Automatic captions often miss names, technical terms, accents, overlapping dialogue, lyrics, speaker changes, and meaningful sound cues. They also don't know your delivery spec.


## Version control prevents most late-stage caption pain


Captions usually arrive late because they depend on locked picture, final audio, approved titles, and sometimes legal or network notes. That makes version control critical. If your team is casual about filenames and references, captions become a guessing game.


Use filenames that make the asset identity obvious. Include project, episode or reel, language, timed text type, version, frame rate if useful, and date or revision. Keep the naming consistent with the delivery spec when one exists.


For example, your team should be able to tell the difference between:


- English SDH captions for the final domestic master
- English dialogue subtitles for a festival screener
- Spanish full subtitles for the international texted master
- English forced narrative subtitles for foreign dialogue only
- Revised captions conformed after a picture trim


Store the reference video used for caption timing with the caption delivery materials, or at least preserve its exact filename, runtime, frame rate, and checksum if your workflow uses checksums. If a vendor timed against “final_v7_review.mp4” but delivery is “feature_domestic_final_final_revised.mov,” your team needs to prove those files are timing-identical.


When picture changes after captions are underway, communicate the change like a conform note. “Trimmed 12 frames at 00:34:10:08” is useful, while “Small change in act three” isn't. Your team or vendor can conform captions quickly when the change data is precise, but they get expensive and error-prone when someone has to rediscover the edit by eye.


## A clean handoff makes caption delivery boring


The best caption deliveries are uneventful. The post supervisor knows the required formats early, and editorial and finishing know which master is authoritative. The caption vendor receives a stable reference with accurate timing information, and QC reviews the exported sidecar files against the final picture. Nobody is trying to turn a web SRT into a platform-specific TTML file at midnight.


When you set up the workflow, define these details as soon as you have delivery specs:


- Required timed text types, such as CC, SDH, full subtitles, and forced narratives
- Required languages and territories
- Required file formats and profiles
- Whether delivery is sidecar, embedded, burned-in, or some combination
- Master video runtime, frame rate, and start timecode
- Naming rules and version identifiers
- Who approves text accuracy, accessibility treatment, and final sync
- Which file is the timing source of truth


That upfront clarity is what keeps captions from becoming a delivery scramble. The format choice matters, but the larger workflow matters more. A correct SCC, STL, SRT, or TTML file is only correct relative to a specific destination and a specific video master.


If you remember one rule, make it this: captions are finished when your team has exported, validated, synced, and viewed the right timed text asset against the exact picture you're delivering.


## FAQ


Closed captions and SDH are accessibility tracks for viewers who are deaf or hard of hearing, so they include dialogue plus meaningful non-speech audio such as speaker IDs, sound effects, music cues, and off-screen speech. Subtitles usually provide dialogue translation or transcription for viewers who can hear the audio. Forced narratives are limited subtitle events for foreign-language dialogue, signs, or on-screen text that must be understood when watching in the primary language.


Use the method required by the delivery spec. For many professional deliveries, sidecar files are preferred because they can be versioned, replaced, language-tagged, validated, and redelivered without re-encoding the picture. Embedded captions are appropriate only when the destination explicitly requests them in a supported container and caption standard. Burned-in subtitles should be used only for open-caption versions, screeners, festival copies, or creative translation that's meant to be permanently visible.


SRT is simple and widely supported, but it carries limited styling, positioning, metadata, and validation information. A review platform may display it acceptably, while a broadcaster, streamer, or aggregator may require SCC, STL, TTML, IMSC, or a specific platform profile. SRT can also hide readability problems such as long lines, poor wrapping, missing speaker treatment, or missing SDH information.


Load the exported caption or subtitle file against the exact video master being delivered, not just the NLE timeline or a review proxy. Confirm runtime, frame rate, start timecode, drop-frame or non-drop-frame behavior, head and tail timing, act breaks, logos, credits, and any late trims. Spot-check the beginning, middle, dense dialogue scenes, music sections, credits, tail, and every area affected by picture changes.


Common rejection reasons include the wrong format or profile, wrong language or asset type, mismatch to the delivered master, incorrect frame rate or timecode basis, captions extending past the program end, missing SDH cues, missing forced narratives, excessive line length, poor reading speed, invalid characters, bad positioning, and filenames or metadata that don't match the delivery request.


Caption notes should be tied to the picture frame where the issue appears, not described only in email or a spreadsheet. Aspect supports frame-accurate comments, so a sync issue, typo, bad line break, or missing SDH cue can be reviewed at the exact point in playback.
