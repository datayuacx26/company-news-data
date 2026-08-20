---
schema_version: "1.0.0"
document_id: "341fc26213858cb71cab36efbd128f230304a1c7e6ed6d28a2a2ccf760909d11"
company_key: "yc-bitmovin"
company: "Bitmovin"
source_id: "yc-bitmovin-news-import-596f48821b52"
canonical_url: "https://bitmovin.com/blog/apple-av1-support/"
published_at: "2026-04-18T20:35:21+00:00"
first_seen_at: "2026-07-27T08:05:57.697851+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:1976f837c1c9a16e4e8e105c021617e5775da2b20984aba90b774fe13dab8712"
---

# Everything you need to know about Apple AV1 Support

**This post was originally published in Sept 2023. It has been updated several times with the latest news and developments, most recently in April 2026 to reflect broader device adoption and ecosystem maturity.**


## TL;DR


- Apple entered the AV1 ecosystem with dedicated hardware decoding on iPhone 15 Pro and 15 Pro Max using the A17 Pro chip, marking Apple’s first official AV1 support. Since then, AV1 hardware decoding has expanded across Apple devices, including Macs with M3 and newer chips and iPads powered by M4 and later processors, significantly increasing ecosystem coverage.
- AV1 playback includes HDR10, Dolby Vision and FairPlay DRM support on these devices,enabling efficient, high-quality adaptive streaming via HLS without new signaling requirements. Safari and Apple platform media frameworks fully support AV1 playback on compatible hardware.
- Safari and Apple platform media frameworks now recognize AV1, with playback available on supported devices
- Apple extended AV1 hardware decode support to M3 Macs and M4-powered iPad Pro, expanding AV1 ecosystem coverage beyond phones
- Software AV1 decoding on older devices is not provided by Apple yet, though some AV1 features like AVIF image support exist in iOS/macOS; wider device support still lags hardware adoption


---


## Table of Contents


Apple made waves across the video encoding and streaming communities when they announced the iPhone 15 Pro and 15 Pro Max would have a dedicated AV1 hardware decoder, making them the first Apple devices with official AV1 codec support. We’ve compiled all the details from their announcement, the HLS interest group, and product release notes to bring you everything you need to know about Apple AV1 codec support. If you’re looking for more information about[AV1](https://bitmovin.com/av1/) playback on Android, Smart TVs and set-top boxes, you can find more information at[https://bitmovin.com/av1-playback-support/](https://bitmovin.com/av1-playback-support/) . Otherwise, keep reading to learn more!


##


## **Since iPhone 15 Pro: AV1 becomes standard on Apple devices**


On September 12, 2023, Apple officially introduced AV1 support with the A17 Pro chip, which included a dedicated AV1 hardware decoder. This marked the first Apple devices capable of native AV1 playback.


Since the iPhone 15 Pro, AV1 hardware decoding has become a standard feature across newer Apple devices, including iPhones, Macs, and iPads powered by Apple Silicon. This has significantly expanded AV1 coverage within the Apple ecosystem and made it a viable target for large-scale streaming deployments.


This shift marked a major milestone for the streaming industry, as Apple had been one of the last major platforms without AV1 support.


Block diagram of Apple’s A17 Pro chip, highlighting dedicated AV1 decoder – Image source: Apple iPhone 15 Pro announcement


> “We also included a dedicated AV1 decoder, enabling more efficient and high-quality video experiences for streaming services.”
>
>
> Sribalan Santhanam – VP, Apple Silicon Engineering Group


## More details about HDR, DRM, HLS and Safari support for AV1


With the introduction of AV1 on iPhone 15 Pro, Apple confirmed support for hardware-accelerated AV1 playback across its media stack. On supported devices, this includes not only Standard Dynamic Range (SDR) content but also High Dynamic Range formats such as HDR10, as well as content protected by FairPlay Streaming DRM-capabilities that rely on secure and efficient hardware decoding.


Playback is supported through Apple’s native frameworks, including AVPlayer and AVSampleBufferDisplayLayer, as well as in Safari on devices with AV1 hardware support. What initially appeared behind experimental flags has since become part of the standard playback pipeline on modern Apple platforms.


HLS playback of AV1 works without requiring new signaling mechanisms, relying on standard attributes such as CODECS and VIDEO-RANGE. The SCORE attribute can be used to prioritize AV1 renditions, but streams encoded with AVC and/or HEVC should still be included to ensure compatibility with older devices and features such as AirPlay.


Safari fully supports AV1 playback on compatible devices and can seamlessly select between multiple codecs when provided. While progressive playback remains possible, adaptive streaming via HLS continues to be the preferred approach for delivering optimal quality of experience and bandwidth efficiency.


The codecs parameter string remains important for signaling detailed information such as profile, level, bit depth, and dynamic range. Providing a complete and accurate codec string allows the player to make optimal playback decisions and fall back gracefully when necessary.


Apple has also continued to improve developer tooling in Safari, including media playback diagnostics and debugging features, which are particularly useful when working with modern codecs such as AV1.


html snippet for multi-codec progressive video with AV1, HEVC and VP9 – Image source: webkit.org blog


New Media stats overlay feature available in Safari 17.0 – Image source: webkit.org blog


## **Since M3: AV1 support expands across Mac devices**


Since the introduction of the M3 generation in late 2023, Apple has included AV1 hardware decoding across its desktop-class processors. This applies to the M3, M3 Pro, and M3 Max chips, and continues with newer Apple Silicon generations, meaning that modern MacBooks, iMacs, and other Mac devices broadly support AV1 video playback.


While Apple Silicon does not currently provide AV1 encoding capabilities, hardware decoding is the key requirement for streaming use cases. As a result, the growing install base of M3 and newer devices represents a significant expansion of the addressable audience for AV1 playback on Apple platforms.


Apple’s new M3 family of processors with AV1 decoding support (Source: Apple)


## **Since M4: AV1 support extends to iPad**


Since the introduction of the M4 chip in 2024, Apple has brought AV1 hardware decoding to iPad devices, starting with the iPad Pro. The M4 media engine supports a wide range of codecs, including H.264, HEVC, ProRes, and AV1, further aligning iPad capabilities with Apple’s broader media ecosystem.


With this, AV1 support now spans across iPhone, Mac, and iPad product lines, making it a viable target for large-scale streaming deployments across the Apple ecosystem.


## Apple AV1 Dolby Vision Support


Apple has introduced support for Dolby Vision in combination with AV1, enabling high-end HDR streaming workflows on supported devices. This includes support for Dolby Vision Profile 10, which is Dolby’s 10-bit AV1-compatible profile.


In addition, Apple supports multiple Dolby Vision profiles:


- Profile 10 (native Dolby Vision)
- Profile 10.1 (HDR10-compatible)
- Profile 10.4 (HLG-compatible)


For profiles 10.1 and 10.4, the SUPPLEMENTAL-CODECS attribute must be used together with the appropriate VIDEO-RANGE. Specifically, Profile 10.1 uses ‘db1p’ with PQ, while Profile 10.4 uses ‘db4h’ with HLG.


An example codec configuration is:
CODECS=”av01.0.13M.10.0.112″,SUPPLEMENTAL-CODECS=”dav1.10.09/db4h”,VIDEO-RANGE=HLG


With Dolby Vision support now integrated into AV1 playback on Apple devices, the codec is positioned not only as a bandwidth-efficient option but also as a premium-quality delivery format for HDR content.


## AV1 Software Decoding Support?


Unlike the rollout of HEVC, where Apple introduced both hardware and software decoding to expand compatibility across older devices, AV1 support on Apple platforms remains limited to hardware decoding.


As of 2026, Apple has not introduced a system-wide AV1 software decoder for video playback. This means that AV1 playback is only supported on devices with dedicated hardware decoding capabilities, such as newer iPhones, Macs, and iPads.


While some AV1-related features exist at the platform level, such as AVIF image support, full AV1 video playback is not available on older devices without hardware support. As a result, streaming services must continue to include fallback codecs like AVC or HEVC to ensure broad device compatibility.


Screenshot comparing H.264, VP9 and AV1 video codec quality for low bandwidth streams. Source: Meta Engineering Blog


## Ready to take advantage of AV1 Encoding?


With AV1 now supported across modern Apple devices, the codec has reached a level of maturity that makes it a practical choice for production streaming workflows.


Bitmovin provides full support for AV1 encoding, including Per-Title and 3-pass optimizations, enabling efficient bitrate ladders and high-quality output. AV1 can be used seamlessly across both DASH and HLS workflows, including support for Widevine and FairPlay DRM, allowing premium content delivery across all major platforms.


With native support for AV1 in HLS on Apple devices, including fMP4-based workflows, streaming services can now deliver high-quality, bandwidth-efficient video to a significantly larger audience.


AV1 encoding is available directly in the Bitmovin Dashboard, making it easy to get started without complex setup. It is also included in the free trial, providing a straightforward way to explore the quality and efficiency benefits AV1 can deliver.


Bitmovin Dashboard Encoding Configuration with new AV1 video codec support


[Click here](https://dashboard.bitmovin.com/signup) to start your free trial today!


---


## FAQs


### What devices currently support AV1 on Apple platforms?


Apple devices with hardware AV1 decoding include iPhone 15 Pro and newer models, Macs with M3 and later Apple Silicon generations (including M5-based systems), and iPad Pro models powered by M4 and newer chips. These devices can natively decode AV1 streams with HDR and DRM support.


### Does Safari support AV1 playback?


Yes. Safari supports AV1 playback on devices with hardware decoding capabilities. This enables adaptive streaming via HLS as well as progressive playback where applicable.


### Is AV1 supported through Apple’s native playback frameworks?


Yes. AV1 playback is supported through Apple’s native frameworks, including AVPlayer and AVSampleBufferDisplayLayer, on supported devices. This integrates seamlessly with HLS and Safari playback.


### Will older iPhones and Macs get AV1 support through software decoding?


No. As of 2026, Apple has not introduced a system-wide AV1 software decoder. Devices without hardware support cannot natively play AV1 video streams.


### Does AV1 support advanced features like HDR and DRM on Apple?


Yes, on supported hardware, AV1 streams can include HDR10 and FairPlay DRM, enabling premium, adaptive streaming experiences with robust content protection.


### How does Apple’s AV1 adoption impact streaming workflows?


With AV1 now supported across modern Apple devices, streaming services can prioritize AV1 renditions for a growing share of users, achieving improved compression efficiency and higher video quality. However, fallback codecs such as AVC and HEVC are still required to ensure full device compatibility.


---


## Related links


- Read the latest info about our AV1 playback support and device testing[here](https://bitmovin.com/av1-playback-support/) .
- Learn how using[Bitmovin’s Per-Title Encoding together with AV1](https://bitmovin.com/av1-4k-video-sd-bitrates/) can let you stream 4K video at bitrates that had been limited to Standard Definition with older codecs.
- Check out our[AV1 hub](https://bitmovin.com/av1/) and download our datasheet to learn all about the codec’s development, performance and how it can lower your CDN costs.
