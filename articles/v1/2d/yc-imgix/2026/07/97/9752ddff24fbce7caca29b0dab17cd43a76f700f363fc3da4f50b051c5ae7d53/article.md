---
schema_version: "1.0.0"
document_id: "9752ddff24fbce7caca29b0dab17cd43a76f700f363fc3da4f50b051c5ae7d53"
company_key: "yc-imgix"
company: "Imgix"
source_id: "yc-imgix-news-import-1efae3cf0bd9"
canonical_url: "https://www.imgix.com/blog/turn-any-audio-into-video-stabilize-any-clip-all-from-one-url"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T05:58:11.560261+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:83aee5f1c69438e01aacfb236e70df7aadce4555ed27a09e556357198c83d2a3"
---

# Turn Any Audio Into Video, Stabilize Any Clip, All From One URL

Our biggest video release yet. Captioned audiograms, real-time stabilization, chroma-key compositing, and a dozen more ways to shape your video, all from the URL.


Your video content has to work everywhere: vertical on TikTok, horizontal on YouTube, muted and autoplaying in a social feed, captioned for accessibility compliance, and stable even when it was shot handheld on a phone. Meeting all of that today usually means stitching together an editing tool, a captioning service, a stabilization plugin, and a separate audio workflow, each with its own learning curve and its own bill.


We get it. Every additional tool is another integration and another thing that breaks. This release folds fifteen new capabilities into the same URL-based video pipeline you already use, grouped into six areas. Here's a look at what's new.


‍


## **Turn Any Podcast Into a Captioned Video**


Point Imgix at any audio file and get back a fully captioned, waveform-animated, vertical video, ready for social in one API call.


One MP3 in, a captioned vertical clip out, with a Ken Burns drift on the cover art and a music bed that ducks under the voice.


- [Audio as a source](https://docs.imgix.com/en-US/apis/video/audio#audio-as-a-source) : Any MP3, WAV, or audio file becomes a valid video input, no video wrapper required.
- [Captioned](https://docs.imgix.com/en-US/apis/video/subtitles/video-generate-subtitles?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_video_generate_subtitles)[audiograms](https://docs.imgix.com/en-US/apis/video/audio-waveforms?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_audio_waveforms) : Auto-generated captions, translated into 100+ languages,[burned in](https://docs.imgix.com/en-US/apis/video/subtitles/video-caption-style?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_video_caption_style) alongside an animated waveform.
- [Ken Burns motion](https://docs.imgix.com/en-US/apis/video/fill/fill-image-motion?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_fill_image_motion) : A moving background, panning and zooming across cover art or a photo, instead of a static image.
- [Background music](https://docs.imgix.com/en-US/apis/video/audio/audio-mark) : Layer in a soundtrack that ducks automatically under narration.


No editor and no separate captioning tool required.


‍


## **Real-Time Stabilization for Shaky Footage**


Handheld and action-cam footage often needs stabilizing before it's publish-ready, and that has traditionally meant a round trip through a separate editing tool. Imgix now stabilizes shaky video directly from the URL, using the same hardware-accelerated motion analysis that already powers our smart preview tooling.


One parameter, and shaky handheld video is publish-ready.


The same handheld clip, before and after. video-stabilize crops in to hold a steady frame across the shake.


[See the stabilization docs →](https://docs.imgix.com/en-US/apis/video/stabilization)


## **Assemble Multiple Clips Into One Finished Video**


Every published video wants the same bookends: a branded intro up front, a call-to-action card at the end, sometimes a sponsor clip in between. Editing those into every upload, and matching resolution and frame rate each time, is exactly the repetitive timeline work an API should handle.


One assembled listing video: intro, main, and outro stitched with transitions, motion, and an auto-timed label, all from a single URL.


- [Intros and outros](https://docs.imgix.com/en-US/apis/video/assembly/video-assembly) : Store one intro and one outro in your bucket and append them to thousands of videos from the URL. Change the file once and every video updates.
- [Image slideshows](https://docs.imgix.com/en-US/apis/video/assembly/video-assembly-dur) : Turn a list of stills into a video, with per-slide durations, Ken Burns motion, and dissolves between them. No video file required.
- [GPU-rendered transitions](https://docs.imgix.com/en-US/apis/video/assembly/video-assembly-transition) : A full catalog of fades, wipes, slides, and shape effects, plus true 3D cube and doorway moves, at every cut.
- [Auto-timed scene labels](https://docs.imgix.com/en-US/apis/video/assembly/video-assembly-labelhttps://docs.imgix.com/en-US/apis/video/assembly/video-assembly-label) : Burn a caption over any segment, timed automatically to when it is on screen.


Captions, watermarks, and thumbnails all run on the assembled video, exactly as if you had uploaded the finished cut.


[See the assembly docs →](https://docs.imgix.com/en-US/apis/video/assembly)


‍


## **Full Creative Control Over Every Frame**


Compositing decisions, where a crop lands, how an overlay enters the frame, what sits behind a transparent subject, usually require a dedicated video editor.


Six compositing controls on the same base clip: chroma-key, directional crop, rotation, custom subtitles, text-overlay transitions, and a branded underlay.


- [Chroma-key](https://docs.imgix.com/en-US/apis/video/video-pip/video-mark-chromakey?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_video_mark_chromakey) : Remove a green screen from a picture-in-picture overlay.
- [Custom background](https://docs.imgix.com/en-US/apis/video/fill/fill-image?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_fill_image) : Replace letterbox bars with your own branded image when reframing video.
- [Directional crop](https://docs.imgix.com/en-US/apis/video/size/crop?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_crop) : Control exactly where a crop lands, not just the center.
- [Rotation](https://docs.imgix.com/en-US/apis/video/rotation/rot?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_rot) and[flip](https://docs.imgix.com/en-US/apis/video/rotation/flip?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_flip) : Mirror or rotate footage with one parameter.
- [Bring-your-own subtitle](https://docs.imgix.com/en-US/apis/video/subtitles/video-subtitle-src?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_video_subtitle_src) : Burn in your own[caption file](https://docs.imgix.com/en-US/apis/video/subtitles/video-caption-embed?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_video_caption_embed) instead of relying on auto-generated captions.
- [Independent transitions](https://docs.imgix.com/en-US/apis/video/text-overlays/video-txt-transition-in?utm_source=blog&utm_medium=blog&utm_campaign=2026_jul_video_api_launch&utm_content=feature_video_txt_transition_in) : Different entrance and exit animations, and durations, for text and overlays.


Every one of these composes with the video transforms you're already using.


‍


## **Cinematic Effects and On-Brand Playback Cues**


Muted, autoplaying social clips lose the sense of where a viewer is in the video, and stock footage often looks flat next to produced content.


A burned-in progress bar keeps muted autoplay oriented; the video-vfx set adds vignette, film grain, and chromatic aberration.


- [Burned-in progress bar](https://docs.imgix.com/en-US/apis/video/progress-bar) : Keeps viewers oriented even with no player controls visible.
- [Video VFX](https://docs.imgix.com/en-US/apis/video/vfx) : Five stylized effects: film grain, bloom, halation, vignette, and chromatic aberration, for a produced, cinematic look.


Combine any of these with your existing color grading for a fully custom look.


‍


## **Extract Broadcast-Ready Audio From Any Video**


Pulling a clean audio track out of a video, for a podcast feed, a transcript, or a standalone clip, usually means a separate audio tool. Imgix now extracts audio directly from any video, running it through the same loudness normalization, denoising, and EQ your video already gets. The output is broadcast-ready, not a raw rip. (2 credits per 5 minutes of output.)


Pair it with your existing trim and speed controls to pull exactly the segment you need.


Any video in, a broadcast-ready audio track out as MP3, WAV, or M4A.


[See the audio-only output docs →](https://docs.imgix.com/en-US/apis/video/audio)


## **Trim by Percentage, Not Just Seconds**


A single trim URL applied across a library of clips with different lengths doesn't hold up when every clip runs a different duration. Clip start, end, and duration now accept a percentage in addition to seconds, so one URL trims consistently across assets of any length.


A small change with an outsized effect on any templated trim workflow.


video-clip-start=50% lands at a different timestamp for each clip, so one URL trims consistently across any length.


[See the clip docs →](https://docs.imgix.com/en-US/apis/video/clip)


‍


Every feature in this release runs through the same Imgix Video pipeline as everything else, so there's nothing new to integrate.


Already an Imgix customer?[Check the docs](https://docs.imgix.com/en-US/apis/video/overview) or reach out to your customer success manager if you want help getting started.


New here?[Start a free trial](https://dashboard.imgix.com/signup)


‍
