---
schema_version: "1.0.0"
document_id: "609a9f8074feed2d79bb71b1e265cbd42fce52269a698b02497e469dbc6d723a"
company_key: "yc-bitmovin"
company: "Bitmovin"
source_id: "yc-bitmovin-news-import-596f48821b52"
canonical_url: "https://bitmovin.com/blog/adaptive-streaming/"
published_at: "2026-06-05T11:49:00+00:00"
first_seen_at: "2026-07-27T08:05:57.697851+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:1cfc0d9c603d2b0e556c95109fa93ecb09f52fccc27c1826dfa2e38d399cae7f"
---

# Adaptive Bitrate Streaming (ABR): What is it & How Does it Work? [2026 Update]

This article was originally published in September 2023 and has been updated.


## TL;DR


- Adaptive Bitrate Streaming (ABR) dynamically adjusts video quality in real time based on a viewer’s available bandwidth, device capabilities, and playback conditions to minimize buffering and playback failures
- ABR works by encoding the same content at multiple bitrates and resolutions, segmenting it into small chunks, and allowing the player to switch between renditions during playback
- The video player is the decision-making layer in ABR, continuously monitoring network conditions and selecting the optimal segment from the manifest (e.g., HLS or DASH)
- ABR significantly improves QoE by balancing video quality, startup time, rebuffering, and stream stability across heterogeneous devices and networks
- Industry-standard protocols like HLS and MPEG-DASH operationalize ABR at scale, enabling efficient delivery of high-quality streaming video across browsers, mobile devices, and connected TVs


---


## Table of Contents


## What is Adaptive Bitrate (ABR) Streaming?


Adaptive Bitrate Streaming (ABR) is a technology used in modern video streaming applications that can dynamically adapt in real-time, in order to provide the best quality for each individual viewer. ABR streams take into account network conditions, screen size and device capabilities and can adjust on the fly to changes in available bandwidth and device resources.


Video content is prepared for ABR streaming by encoding different video files or renditions of the same content at different bitrates and video resolution. The video file is split into smaller chunks so the streaming client or video player can select the best video quality depending on its current network connection. This may change several times throughout a viewing session, but by implementing abr streaming, providers can ensure smooth playback for all viewers whether they are mobile users or viewing on 4K televisions at home.


## What is Progressive Video Streaming?


Progressive streaming or Progressive video streaming is a form of streaming that involves using a single video file to deliver content over the internet. Viewers can progressively download video content and begin streaming before the entire file is downloaded. It’s a simple, straightforward approach to deliver video that works with most web browsers, but has some drawbacks. Because it is limited to a single file, the same resolution and bitrate will be delivered to every viewer and device, regardless of its capabilities or available bandwidth.


## What might happen if you don’t use Adaptive Bitrate Streaming?


Progressing streaming using a single file or single video bitrate to deliver video has some limitations when compared to adaptive bitrate streaming. If you choose to prioritize higher quality video by encoding with a higher video bitrate, viewers on slower connections may experience buffering and other performance issues. If you play it safe and encode a low bitrate stream, you will reduce streaming quality for everyone, which will be especially noticeable on larger screens. Either way, using progressive streaming limits your ability to deliver the best video quality to everyone, while adaptive bitrate streaming ensures your video content can be efficiently delivered with the highest possible quality for every viewer.


## How Does Adaptive Bitrate Streaming Work?


There are several steps involved with adaptive bitrate streaming. First, content is prepared for video streaming by encoding a direct camera feed or transcoding a master source file into multiple video streams with different bitrates, resolutions, and frame rates. Files prepared for adaptive streaming are also usually split into multiple video segments or chunks. Next, a manifest file is created that contains all the information adaptive streaming clients or video players need to know about the different versions.


Once video playback begins, the player will monitor things like buffer level and the download speed of video segments, and if the network connection slows down, it will switch to a lower bitrate stream. Once conditions improve, it will seamlessly switch back to a higher quality version. That’s the “adaptive” part of adaptive video streaming, the ability to adjust video quality on the fly depending on changing conditions, ensuring the best possible viewing experience.


## Preparation: Video Encoding and Transcoding


Whether you are doing ABR live streaming or preparing video files for on-demand viewing, you’ll need to create different bit rates to serve different devices and network conditions. The process of converting a raw camera feed or uncompressed source is called[video encoding](https://bitmovin.com/video-encoding-guide/) . Converting from an already compressed file, sometimes known as a mezzanine file, is known as transcoding. Both involve preparing the source for adaptive bitrate streaming by converting it into multiple formats and splitting it into video segments or chunks. The lengths vary from application to project but are often between two and ten seconds each.


Codec, target bitrates, resolution and frame rate are all things that need to be considered during this phase. H.264 is the most commonly used and widely supported codec, but newer codecs like[H.265](https://bitmovin.com/developer-network/lesson-video-1-9-high-efficiency-video-coding-hevc/) ,[VP9](https://bitmovin.com/vp9-codec-status-quo/) and[AV1](https://bitmovin.com/av1/) can provide higher quality while using lower bitrates than H.264. It’s also important to consider your viewing audience and application when deciding on your encoding settings. If your streaming service delivers video primarily to Smart TVs and set-top boxes, then you probably have different priorities than a service that only involves mobile streaming. For example, mobile users are more likely to have slower and more challenging network conditions than someone viewing on a home broadband connection, while someone viewing on a 4K television will want content that takes full advantage of their larger screen when streaming video.


## What Video Stream Bitrate Should I Choose?


The best bit rate depends largely on your content and your target audience, but with adaptive streaming, you don’t have to choose just one. By choosing a range of different bitrates and resolutions, you can ensure your video platform is providing the best quality of experience for your entire audience. This range of bitrate and resolution combinations is commonly referred to as an abr encoding ladder.


## Encoding Ladders for Adaptive Bitrate Streaming


The video assets you’ve prepared for ABR streaming make up your encoding ladder. On the top of the ladder, a high-bitrate, high-quality stream is output to viewers on fast connections, using the latest technology and hardware. At the bottom of the ladder, the same footage can be seen as a lower resolution, low bitrate stream for users with smaller screens or a poor connection speed. Having more “rungs” on your ladder or more versions of the same content can mean switching between them is less noticeable to users, but this comes at the cost of higher storage requirements.


In the early days of adaptive streaming, one-size-fits all ABR ladders were often used, many based on Apple’s default recommendation. Today, the more modern and efficient approach involves using a content-aware encoding technology like Bitmovin’s[Per-Title Encoding](https://bitmovin.com/encoding-service/per-title-encoding/) , which analyzes the content and complexity of each video file, and determines the ideal ABR ladder. This ensures you’re providing the best quality for viewers without wasting any video data or storage capacity.


## Which Adaptive Streaming Protocol Should I Use?


A streaming protocol provides a means of displaying audio or video via the internet. The two most commonly used streaming protocols are HLS (HTTP live streaming) and MPEG DASH (Dynamic Adaptive Streaming over HTTP). HLS is more associated with Apple’s streaming technology ecosystem while DASH is more thought of as a universal and open solution, but both are widely used and support the use of multiple codecs, encryption and DRM.


CMAF (Common Media Application Format) is a relatively new open standard[container format](https://bitmovin.com/container-formats-fun-1/) that is compatible with both HLS and DASH. This has simplified workflows, allowing any streaming service to deliver both HLS and DASH streams, without having to perform duplicate encodings.


Microsoft Smooth Streaming is another streaming protocol that had seen some wide use in the early days of adaptive streaming. But as the popularity of HLS and DASH grew, content providers and streaming services transitioned away from smooth streaming and Microsoft began phasing out support in 2021.


## Manifest File Creation


Manifest files provide all the information a client needs for video playback and dynamic adaptive streaming and are used for both live streaming and video-on-demand (VOD). They contain information about what codecs were used, their bitrates, resolutions, frame rates and where the individual video files are stored on the streaming server. Different streaming protocols use their own manifest file format. HLS uses the .m3u8 playlist and[MPEG DASH](https://bitmovin.com/dynamic-adaptive-streaming-http-mpeg-dash/) uses the .mpd (Media Presentation Description) format.


Most encoding and transcoding products and services can automatically generate your manifest files for you, based on your adaptive bitrate ladder and other settings. They may also provide more advanced users the ability to fine-tune their manifest or create multiple manifests used to target different devices, based on their video playback capabilities. There are also some standalone packaging solutions that can create ABR streaming manifests from previously encoded video files.


## Dynamic Playback to Maximize Video Quality


Video players typically start streaming video at low bitrate and then request higher and lower quality video chunks if the network conditions change, though some platforms will allow you to specify which quality is chosen first. The ABR streaming algorithm determines which version to download next.


There are several variations, but the ABR algorithms video players use typically consider the speed of download and buffer capacity when deciding which video segment to select. This is an area of adaptive bitrate streaming that has gotten more attention recently with video streaming services looking to find a competitive edge by[customizing their abr algorithm](https://community.bitmovin.com/t/configuring-your-abr-in-bitmovin-player/782) to provide the best possible quality of experience.Another aspect of ABR streaming getting more attention recently is the environmental impact of streaming video.


New research from Bitmovin and the[GAIA project](https://go.bitmovin.com/gaia-report) is exploring ways to reduce the carbon footprint of streaming by providing ways for services and end users to adjust the ABR algorithm in their video players to lower power consumption. The image below shows a prototype interface for our Eco Mode, provides extra detail and allows the user to adjust the player properties to use less power.


## The Benefits Of Adaptive Bitrate Streaming


Adaptive bitrate streaming offers several benefits for both content providers and their users by making content available to a wider array of devices and improving the viewing experience regardless of network conditions. Whether viewing at home or on a mobile device, content is prepared in a way that makes the best impression on every screen and is able to seamlessly adapt to variable bandwidth.


Costs are reduced by optimizing the ABR ladder to minimize unnecessary video data transfer, while at the same time improving the quality of experience with less buffering and an uninterrupted viewing session. This also improves viewer engagement and retention, reducing churn for subscription services and increasing impressions for advertising-based services.


ABR streaming is also highly scalable and is well suited for both live and VOD content, making it the best choice to support popular events and viral videos. ABR streaming is essential for delivering the highest quality content to the widest audience with a diverse assortment of video playback devices and network connections. Implementing adaptive bitrate streaming ensures your content is presented seamlessly and in its best form, creating the best experience for your viewers and outcome for your streaming business.


---


## FAQ


### Does Adaptive Bitrate Streaming Improve Video Quality?


Video quality is largely determined by the encoding settings used, but ABR streaming allows you to encode in multiple bitrates and resolutions to ensure the best quality of experience for all of your viewers.


### Does Netflix use Adaptive Bitrate Streaming?


Yes! Netflix, YouTube and all other modern streaming platforms take advantage of ABR streaming for the best combination of quality of experience for viewers and cost efficiency.


### What is Multi bitrate vs Adaptive Bitrate Streaming?


Multi bitrate streaming usually refers to a situation where the viewer can explicitly select which version or quality of the video they want to watch, where Adaptive Bitrate Streaming handles the quality selection automatically based on their device and network connection.


### What is the difference between progressive and adaptive streaming?


Adaptive Streaming involves creating multiple versions of your content to best serve viewers on all types of devices and network connections. Progressive streaming involves the use of a single video file and ultimately means sacrificing quality for some viewers and making your content inaccessible to others.


---
