---
schema_version: "1.0.0"
document_id: "6617859cd766ab3f4562839e64bea78751d791f3aaec457af6861437255cf020"
company_key: "yc-recall-ai"
company: "Recall.ai"
source_id: "yc-recall-ai-news-import-5dc65e725852"
canonical_url: "https://www.recall.ai/blog/macos-screencapture-api"
published_at: "2026-04-22T00:00:00+00:00"
first_seen_at: "2026-07-22T11:03:36.206412+00:00"
fetched_at: "2026-07-28T22:15:44.247179+00:00"
content_hash: "sha256:ad699be3d820c17667071add9ab2d683737c1ad52238ea6c260a2ac7c3fe1cad"
---

# Exploring macOS screen capture APIs and recording approaches

Recording meetings has become the norm. Many developers need a way to pipe video and audio data from these meetings into their products without a bot joining a call.


Building a desktop recorder is one of the most effective ways to capture meeting data directly from a user’s machine. Feature-complete desktop recorders can also generate artifacts like transcripts and recordings.


In this blog, I’ll walk through the technical steps required to build a desktop meeting recorder, with a focus on the most critical piece: capturing audio and video data. I’ll cover the most common ways to record meetings on macOS, the technical challenges you’re likely to encounter, and what you need to build a feature-complete desktop recording app.


> If you want to skip the complexity of[building a meeting recorder,](https://us-west-2.recall.ai/) Recall.ai’s Desktop Recording SDK helps you bring one into production on macOS and Windows in days.


## Technical components of meeting capture


Building a reliable desktop meeting recorder on macOS involves several steps. It starts with capturing meeting audio and video data. This is handled by the data capture layer, which we’ll focus on in this blog.


A meeting consists of audio and video data, and the capture layer ensures the data is recorded. Audio and video quality directly impact the usability of recordings, transcripts, and downstream analysis. Poor audio capture can lead to lost information and misattributed transcripts, making it unclear who said what.


At a high level, the capture layer is responsible for:


- **Video:** The visual content of the meeting, including participants’ videos and screens shared.
- **System audio:** Sound coming from your computer like people talking in the meeting or the videos played.
- **Microphone audio:** Sound picked up by your microphone, including your voice and any surrounding noise. This must be handled separately from system audio.


> If you want more than just meeting data, you can use a[calendar API](https://www.recall.ai/product/calendar-integration-api) as an additional capture layer.


## How to capture a meeting on macOS


[Demo Video](https://youtu.be/f7ZLrP0s194)


In this section, I’ll cover how to capture a meeting on macOS using Electron’s desktopCapturer API, AVFoundation, and ScreenCaptureKit. I’ll examine the solutions based on the key features required for a reliable recording experience.


### 1. Electron App desktopCapturer


Electron provides a built-in way to capture audio and video on macOS and Windows via the[desktopCapturer API.](https://www.electronjs.org/docs/latest/api/desktop-capturer) You can capture system audio on macOS, while microphone input must typically be captured separately and combined manually. desktopCapturer does not include built-in echo cancellation, and requires additional post-processing to avoid duplicated audio.


### 2. AVFoundation


[AVFoundation](https://developer.apple.com/av-foundation/) is Apple’s native media framework for working with audio and video on macOS 10.7 and later. It can access input devices like microphones and capture screen content via` AVCaptureScreenInput` , but it does not capture system audio or work on Windows.


### 3. ScreenCaptureKit


[ScreenCaptureKit](https://developer.apple.com/documentation/screencapturekit/) is Apple’s native screen-capture framework for macOS. It captures video and system audio and is the preferred API for newer versions of macOS (12.3+). While earlier versions of ScreenCaptureKit do not support microphone input, this capability was added in macOS 15+. ScreenCaptureKit does not work on Windows.


> [FFmpeg](https://ffmpeg.org/) is commonly used when working with audio and video data but it is not included in this blog because it is not a capture tool. Electron’s DesktopCapturer, AVFoundation, and ScreenCaptureKit are capture tools that handle recording meeting video and audio data.


## Limitations of native macOS and Electron APIs


To better understand the strengths and limitations of these approaches, I built three different meeting recorders using Electron’s DesktopCapturer, AV Foundation and ScreenCaptureKit. While my solutions captured both meeting video and audio, I quickly ran into a few limitations.


### Automatic meeting detection


Electron’s desktopCapturer API, AVFoundation, and ScreenCaptureKit do not have a built-in meeting detection feature. They cannot automatically determine when a meeting starts or ends. If you build on these APIs and do not implement meeting detection, your end users must manually start and stop recordings.


### Video recording


When it comes to video recording, these tools default to capturing the entire screen rather than just the meeting window. As a result, other applications and background activity are recorded as well.


A usable recording should capture only the meeting window and intentionally shared content. You will need to build tab isolation to avoid users unintentionally sharing personal content when switching between tabs or windows.


### Audio recording


AVFoundation supports microphone capture but does not support system audio, which must be captured using APIs like ScreenCaptureKit. Electron’s desktopCapturer API and ScreenCaptureKit can record both system and microphone audio, but you must manually combine the audio streams for audio synchronization for all three solutions.


Combining audio streams can introduce echo if not handled correctly. None of these tools provide built-in echo cancellation, so this must be implemented separately. These tools also lack system audio isolation meaning they record all audio coming from your computer including notification sounds rather than just the meeting audio. Additionally, they can’t detect mute state and will continue recording even when users mute themselves in a meeting.


### Diarized transcription


None of the solutions tested generate transcripts, adding to the list of things the developers would need to build themselves. Even if you capture audio clearly, for transcripts to be usable, they must be diarized, with clear speaker attribution to identify who said what. Speaker labels are needed to generate diarized transcripts, but they are not provided by the solutions I tested.


Here is a side-by-side comparison of the solutions I tested, including the key features and edge cases to consider when building a production-ready meeting recorder:


Capabilities Electron’s desktopCapturer API AVFoundation ScreenCaptureKit[Recall.ai’s Desktop Recording SDK](https://docs.recall.ai/docs/desktop-sdk)


Automatically starts and stops recordings when meetings start and end No No No Yes


Captures the audio from the user’s microphone during the meeting Yes (macOS 13+) Yes Yes Yes


Participant speech and media played during the meeting are recorded Yes (macOS 13+) No Yes Yes


Eliminates echo caused by routing microphone audio into system audio No No No Yes


Window selection removes the need to manually specify recording dimensions Yes Limited Yes Yes


Captures the shared screen directly, ensuring clear and readable content No No No Yes


Ensures only the intended tab is shared and recorded No No No Yes


Generates diarized transcripts automatically No No No Yes


Gets speaker names No No No Yes


Optimized for low battery usage, so recording won’t drain the user’s device No No No Yes


Works when user’s disk is full No No No Yes


Works when user’s connection drops No No No Yes


Works on macOS and Windows Yes No No Yes


As shown above, Electron’s desktopCapturer API, AVFoundation, and ScreenCaptureKit do not address many important edge cases. If your goal is to simply capture raw video, even if it is cluttered and requires manual setup, or audio quality isn’t important, these tools may be sufficient.


However, if you need high-quality and clear video recordings that only show the intended shared content, audio that is synced and echo-free, or transcripts with perfect speaker diarization, you’ll need to explore other options.


## A better approach using SDKs


Electron’s desktopCapturer API, AVFoundation, and ScreenCaptureKit provide basic recording. But high-quality recordings require handling edge cases and managing everything beyond the capture layer.[Recall.ai’s Desktop Recording SDK](https://docs.recall.ai/docs/desktop-sdk) provides an integrated solution that takes care of this complexity.


> For clarity, I will be referring to Recall.ai’s Desktop Recording SDK as the Desktop Recording SDK.


The Desktop Recording SDK handles everything from the capture layer to producing outputs like transcripts with[perfect speaker diarization.](https://www.recall.ai/blog/speaker-diarization) But even at the capture layer, it optimizes for performance and reliability.


Users often switch between tabs, windows or apps during a meeting. The Desktop Recording SDK handles tab isolation, ensuring that only the relevant meeting window is recorded. When users switch tabs during a meeting, it continues to record only the meeting content, preventing accidental exposure of unrelated or private information.


During screen sharing, the Desktop Recording SDK records the shared window directly rather than recording it through the call, resulting in a higher recording quality.


To get clear audio, the Desktop Recording SDK eliminates the echo caused by microphone audio being routed into system audio, eliminating duplicate sound. When users are muted in a meeting, it will not record them. This prevents accidental recording of private conversations.


Additionally, the Desktop Recording SDK precisely detects when meetings start and end. Many desktop recording apps on the market rely on microphone activity to infer when meetings start, but this approach is unreliable, as it does not account for waiting rooms or scenarios where the microphone is used outside of an active meeting.


> If you want to get started quickly, check out our[Desktop Recording SDK tutorial](https://www.recall.ai/blog/how-to-build-a-desktop-recording-app)


[How to build a desktop recording app (Like Granola)](https://www.youtube.com/watch?v=79BRcupZH_8)


To generate perfectly diarized transcripts, you need speaker names. Recall.ai’s Desktop Recording SDK automatically captures the speaker names, essential for understanding who said what, enabling reliable summarization and action item tracking.


Building a product that deals with the numerous edge cases and complex functionality that works reliably across different devices can drain battery life. It also requires significant engineering effort to handle the long tail of device-specific issues. These are some of the many challenges we’ve already solved with the Desktop Recording SDK.


> Recall.ai’s Desktop Recording SDK is built to handle complex edge cases and work across meeting platforms on both Windows and macOS.


## Conclusion


Building a production-ready desktop meeting recorder on macOS requires more than just recording audio and video content. In this post, we explored several ways to record meeting audio and video on macOS. Common limitations of some of the solutions we explored include lack of automatic meeting detection, no tab isolation, and echo from combined audio sources. Solving these common limitations usually takes months of engineering time.


Recall.ai’s Desktop Recording SDK deals with all of these limitations. It provides reliable automatic meeting detection, generates clear video and audio recordings, and delivers perfectly diarized transcripts. The Desktop Recording SDK manages all the edge cases so you can focus on building your product, and not the underlying infrastructure.


You can[build a production-ready](https://us-west-2.recall.ai/) desktop app in a day using Recall.ai or you can[learn more](https://www.recall.ai/book-a-demo) about the product.
