---
schema_version: "1.0.0"
document_id: "f0a6ad7fcbe6ee0414544512df7aad93024550f183c850bf632005dc04dc6157"
company_key: "yc-bitmovin"
company: "Bitmovin"
source_id: "yc-bitmovin-news-import-596f48821b52"
canonical_url: "https://bitmovin.com/blog/scte-35-guide/"
published_at: "2026-06-19T09:51:00+00:00"
first_seen_at: "2026-07-27T08:05:57.697851+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:43f416f83d1e4bd3d16b121f6654c569e8ac0f1895947066e766dfe20ccee019"
---

# The Essential Guide to SCTE-35

## Everything you need to know about SCTE-35, the popular event signaling standard that powers dynamic ad insertion, digital program insertion, blackouts and more for TV, live streams and on-demand video.


**This post was last updated in 2025, It was updated in June 2026 with information about Bitmovin’s new VOD SCTE 35 support*


## TL;DR


- SCTE-35 is the industry standard binary cue format for event signaling used across broadcast, live streaming, and OTT.
- It’s the canonical metadata protocol that powers deterministic ad insertion and program control.
- Key splice commands include splice_insert (frame-accurate immediate/scheduled splices) and time_signal (scheduled cues plus rich segmentation descriptors).
- SCTE-35 cues ride in-band on their own PID in the transport mux, typically used for contribution/backhaul (satellite, fiber, SRT, Zixi) rather than direct consumer playback.
- Bitmovin’s Live Encoder parses SCTE-35 into HLS manifest markers and offers a Live Cue API for real-time insertion.


---


## Table of Contents


## What is SCTE?


The acronym SCTE is short for The Society of Cable and Telecommunications Engineers. SCTE is a non-profit professional organization that creates technical standards and educational resources for the advancement of cable telecommunications engineering and the wider video industry. When talking about it, you may hear people abbreviate SCTE with the shorthand slang “Scutty”.


SCTE was founded in 1969 as The Society of Cable Television Engineers, but changed its name in 1995 to reflect a broader scope as fiber optics and high-speed data applications began playing a bigger role in the cable tv industry and became the responsibility of its engineers. Currently there are over 19,000 individual SCTE members and nearly 300 technical standards in their catalog, including SCTE-35, which will be the focus of this post.


## What is SCTE-35?


SCTE-35 was first published in 2001 and is the core signaling standard for advertising and program control for content providers and content distributors. It was initially titled “Digital Program Insertion Cueing Message for Cable” but recent revisions have dropped “for Cable” as it has proven useful and versatile enough to be extended to OTT workflows and streaming applications. There have been several revisions and updates published to incorporate member feedback and adapt to advancements in the industry, most recently on Nov, 30 2023.


SCTE-35 signals are used to identify national and local ad breaks as well as program content like intro/outro credits, chapters, blackouts, and extensions when a live program like a sporting event runs long. Initially, these messages were embedded as cue tones that dedicated cable tv hardware or equipment could pick up and enable downstream systems to act on. For modern streaming applications, they are usually included within an MPEG-2 transport stream PID and then converted into metadata that is embedded in HLS and MPEG-DASH manifests.


Table with revisions and updates made to the specification over the years.
source:
SCTE-35 specification


## SCTE-35 markers and their applications for streaming video


While SCTE-35 markers are primarily used for ad insertion in OTT workflows, they can also signal many other events that allow an automation system to tailor the program output for compliance with local restrictions or to improve the viewing experience. Let’s take a look at some common use cases and benefits of using SCTE-35 markers.


### Use cases and benefits of SCTE-35


**Ad Insertion** – As mentioned above, inserting advertisements into a video stream is the main use case for SCTE-35 markers. They provide seamless splice points for national, local and individually targeted dynamic ad replacement. This allows for increased monetization opportunities for broadcasters and content providers by enabling segmenting of viewers into specific demographics and geographic locations. When ad content can be tailored for a particular audience, advertisers are willing to pay more, leading to higher revenue for content providers and distributors.


For ad supported streaming, clear SCTE-35 signaling is the foundation for both Server Side Ad Insertion (SSAI) and newer Server Guided Ad Insertion (SGAI) workflows. By exposing precise ad opportunities in live and VOD manifests, SCTE-35 gives downstream ad decision systems what they need to select, stitch and measure ads without breaking the viewing experience or the underlying stream.


Ad Break Example.
source: SCTE-35 specification


**Program boundary markers** – Another common use case is to signal a variety of program boundaries. This includes the start and end of programs, chapters, ad breaks and unexpected interruptions or extensions. Many of these become particularly useful in Live-to-VOD scenarios. Ad break start/end markers can be used as edit points in a post-production workflow to automate the removal of ads for viewers with ad-free subscriptions. A program end marker can be used to trigger the next episode in a series for binge viewing sessions if so desired. All of these markers open new possibilities for improving the user experience and keeping your audience happy and engaged.


**Blackouts and alternate content** – Another less common, but important use case is to signal blackouts, when a piece of content should be replaced or omitted from a broadcast. This often applies to regional blackouts for sporting events. Respecting blackout restrictions is crucial for avoiding fines and loss of access to future events. Using SCTE-35 allows your automation system to take control and ensure you are compliant.


Workflow example with Program Boundaries and Blackouts.
source: SCTE-35 specification


## Types of SCTE-35 markers


SCTE-35 markers are delivered in-band, meaning they are embedded or interleaved with the audio and video signals. There are five different command types defined in the specification. The first 3 are legacy commands: splice_null(), splice_schedule() and splice_insert(), but splice_insert() is still used quite often. The bandwidth_reservation() command may be needed in some satellite transmissions, but the most commonly used command with modern workflows is time_signal(). Let’s take a closer look at the 2 most important command types, splice_insert and time_signal.


### splice_insert commands


Splice_insert commands are used to mark splice events, when some new piece of content like an ad should be inserted in place of the program or a switch from an ad break back into the main program. Presentation time stamps are used to note the exact timing of the splice, enabling seamless, frame accurate switching.


### time_signal commands


Time_signal commands can also be used to insert new content at a splice point, but together with segmentation descriptors, they can handle other use cases like the program boundary markers mentioned above. This enables the segmenting and labeling of content sections for use by downstream systems.


Structure of a time_signal command.
source: SCTE-35 specification


## Using SCTE-35 markers in streaming workflows


### MPEG-2 transport streams


In MPEG-2 transport streams, SCTE markers are carried in-band on their own PID within the transport stream mux. These streams are usually used as contribution or backhaul feeds and in most cases are not directly played by the consumer. They may be delivered over dedicated satellite or fiber paths or via the public internet through the use of streaming protocols like SRT or proprietary solutions like Zixi.


### HLS


The Bitmovin Live Encoder supports a range of different HLS tags that are written when SCTE-35 triggers are parsed from the MPEG-TS input stream. Multiple marker types can be enabled for each HLS manifest. Which marker types to use depends on the consumer of the HLS manifest. An example consumer would be a Server Side Ad Insertion (SSAI) service. They usually state in their documentation which HLS tags they support for signaling SCTE-35 triggers.


- ` EXT_X_CUE_OUT_IN` : Ad markers will be inserted using` #EXT-X-CUE-OUT` and` #EXT-X-CUE-IN` tags.


- ` EXT_OATCLS_SCTE35` : Ad markers will be inserted using` #EXT-OATCLS-SCTE35` tags. They contain the base64 encoded raw bytes of the original SCTE-35 trigger.
- ` EXT_X_SPLICEPOINT_SCTE35` : Ad markers will be inserted using` #EXT-X-SPLICEPOINT-SCTE35` tags. They contain the base64 encoded raw bytes of the original SCTE-35 trigger.
- ` EXT_X_SCTE35` : Ad markers will be inserted using` #EXT-X-SCTE35` tags. They contain the base64 encoded raw bytes of the original SCTE-35 trigger.
- ` EXT_X_DATERANGE` : Ad markers will be inserted using` #EXT-X-DATERANGE` tags as specified in the[HLS specification](https://datatracker.ietf.org/doc/html/draft-pantos-hls-rfc8216bis#section-4.4.5.1) . They contain the ID, start timestamp, and hex-encoded raw bytes of the original SCTE-35 trigger.


*Example HLS manifest with Cue Out, duration and Cue In tags:*


```text
#EXTINF:4.0,
2021-07/video/hls/360/seg_18188.ts
#EXT-X-CUE-OUT:120.000
…
#EXTINF:4.0,
2021-07/video/hls/360/seg18218.ts
#EXT-X-CUE-IN


```


*Example HLS manifest using EXT-OATCLS-SCTE35 tag with base64 encoded marker* :


```text
#EXTINF:4.0,
2021-07/video/hls/360/seg_18190.ts
#EXT-OATCLS-
SCTE35:/DBcAAAAAAAAAP/wBQb//ciI8QBGAh1DVUVJXQk9EX+fAQ5FUDAxODAzODQwMDY2NiEEZAIZQ1VFSV0JPRF/3wABLit7AQVDMTQ2NDABAQEKQ1VFSQCAMTUwKnPhdcU=


```


*Note: You can copy the base64 encoded marker above, (beginning with the first / after* SCTE35: *) and paste it into this[payload parser](https://comcast.github.io/scte35-js/) to see the full message structure.*


### MPEG-DASH


In MPEG-DASH streams, SCTE-35 defined breaks and segments are added as new periods to the .mpd file.


```text
<MPD>
<Period start="PT0S" id="1">
<!-- Content Period -->
</Period>


<Period start="PT32S" id="2">
<!-- Ad Break Period -->
<EventStream timescale="90000"
schemeIdUri="urn:scte:scte35:2014:xml+bin">
<Event duration="2520000" id="1">
<Signal xmlns="urn:scte:scte35:2013:xml">
<Binary>      /DAlAAAAAAAAAP/wFAUAAAAEf+/+kybGyP4BSvaQAAEBAQAArky/3g==
<Binary>
</Signal>
</Event>
</EventStream>
</Period>


<Period start="PT60S" id="3">
<!-- Content Period -->


```


With SCTE messages embedded in the stream, various forms of automation can be triggered, whether it’s server or client-side ad insertion, content switching, interactive elements in the application or post-production processing.


## Bitmovin Live Encoder SCTE Support


### SCTE message pass-through and processing


Bitmovin supports the parsing of SCTE-35 triggers from MPEG-TS input streams for Live Encodings, the triggers are shown below as splice decisions. The triggers are then mapped to HLS manifest tags.


#### Splice Decisions


Certain SCTE-35 triggers signal that an advertisement or break (to from the original content starts or ends. The following table describes how the Bitmovin Live Encoder treats SCTE-35 trigger types and SCTE-35 Segmentation Descriptor types as splice decision points, and the compatibility of those types with the different command types, Spice Insert and Time Signal.


✓= Supported


✖ = Not currently supported


**Segmentation UPID Type (Start/End)** **Descriptor Type Name** **SPLICE_INSERT** **TIME_SIGNAL**


**–** ✓ ✖


**0x10, 0x11** PROGRAM ✓ ✖


**0x20, 0x21** CHAPTER ✓ ✖


**0x22, 0x23** BREAK ✓ ✓


**0x30, 0x31** PROVIDER_ADVERTISEMENT ✓ ✓


**0x32, 0x33** DISTRIBUTOR_ADVERTISEMENT ✓ ✓


**0x34, 0x35** PROVIDER_PLACEMENT_OPPORTUNITY ✓ ✓


**0x36, 0x37** DISTRIBUTOR_PLACEMENT_OPPORTUNITY ✓ ✓


**0x40, 0x41** UNSCHEDULED_EVENT ✓ ✖


**0x42, 0x43** ALTERNATE_CONTENT_OPPORTUNITY ✓ ✖


**0x44, 0x45** PROVIDER_AD_BLOCK ✓ ✖


**0x46, 0x47** DISTRIBUTOR_AD_BLOCK ✓ ✖


**0x50, 0x51** NETWORK ✓ ✖


### Live cue point insertion API


In addition to the SCTE-35 pass-through mode, Bitmovin customers can insert new ad break cue points in real-time, using live controls in the user dashboard or via API. These can be inserted independently of existing SCTE-35 markers in the input stream and may be useful for live events when the time between ads is variable depending on breaks in the action. This allows streamers that don’t have SCTE-35 markers embedded in their source to take advantage of the same downstream ad insertion systems for increased monetization.


*API Call:*


```text
POST /encoding/encodings/{encoding_id}/live/scte-35-cue
{
"cueDuration": 60, // duration in seconds between cue tags (ad break length)
}


```


The #EXT-X-CUE-OUT tag will be inserted into the HLS playlist, signaling the start and duration of a placement opportunity to the DAI provider. Based on the cueDuration and the segment length, the #EXT-X-CUE-IN tag will be inserted after the configured duration and the ad opportunity will end, continuing the live stream.


*HLS manifest with Cue Out, duration and Cue In tags* *inserted via the API call above:*


```text
#EXTINF:4.0,
2021-07-09-13-18-34/video/hls/360_500/segment_18188.ts
#EXT-X-CUE-OUT:60.000
#EXTINF:4.0,
2021-07-09-13-18-34/video/hls/360_500/segment_18189.ts
...
#EXTINF:4.0,
2021-07-09-13-18-34/video/hls/360_500/segment_18203.ts
#EXT-X-CUE-IN
#EXTINF:4.0,
2021-07-09-13-18-34/video/hls/360_500/segment_18204.ts


```


## **Bitmovin VOD Encoder SCTE Support**


### SCTE message insertion for VOD workflows


As of Encoder version 2.221.0, Bitmovin users can configure and add SCTE-35 triggers to their videos. Base 64 encoded binary representations of each SCTE marker are attached at the desired time, expressed in seconds. The Bitmovin encoder will adjust segment boundaries and PTS alignment as needed to ensure a precise match between the trigger and segment boundary.


### VOD cue point insertion API


Bitmovin customers can insert new SCTE-35 triggers, via the API. This functionality inserts a key frame at the specified time and adds the Base64 encoded metadata to the container and manifest.


To utilize this functionality, a keyframe needs to be inserted at the right time and a SCTE-35 trigger to be added via API Call – https://developer.bitmovin.com/encoding/reference/postencodingencodingsscte35triggersbyencodingid:


### **VOD cue point insertion API**


*To utilize this functionality, a keyframe needs to be inserted at the right time and a SCTE-35 trigger to be added via API Call –*[https://developer.bitmovin.com/encoding/reference/postencodingencodingsscte35triggersbyencodingid](https://developer.bitmovin.com/encoding/reference/postencodingencodingsscte35triggersbyencodingid) **


Currently this VOD SCTE-35 support works with HLS outputs only. See the[release notes](https://developer.bitmovin.com/encoding/docs/encoder-22000-22490#22210-beta) for more details.


Want to get started using SCTE-35 in your streaming workflow?[Get in touch](https://bitmovin.com/contact-bitmovin) to let us know how we can help.


---


## FAQs


### What is SCTE-35 and why does it matter?


SCTE-35 is the core industry standard for ad insertion and program control across broadcast and streaming workflows, now fully adopted in OTT environments. SCTE-35 identifies ad breaks, program boundaries (intro/outro, chapters), blackouts, and live event extensions.


### What are the most important SCTE-35 command types I need to support?


Operationally focus on splice_insert (immediate or scheduled splice with precise PTS/duration) and time_signal (scheduled event often paired with segmentation descriptors). Other legacy commands exist (e.g., splice_null, splice_schedule, bandwidth_reservation) but are less relevant for modern OTT/SSAI workflows.


### Where are SCTE-35 markers actually placed in a stream?


SCTE-35 markers live in-band within the transport stream for MPEG-TS and are exposed as manifest-level cues/tags in HLS and MPEG-DASH for playback and ad insertion.


### How do SCTE-35 cues get delivered to HLS and DASH consumers?


In HLS, SCTE-35 is mapped into manifest-level tags like #EXT-X-CUE-OUT/#EXT-X-CUE-IN, EXT-OATCLS-SCTE35, EXT-X-SCTE35, EXT-X-SPLICEPOINT_SCTE35, or EXT-X-DATERANGE (base64 raw bytes or hex/ID/time metadata). In DASH, cues are expressed as EventStreams/Periods in the MPD carrying binary SCTE-35 payloads. For MPEG-TS contribution feeds, the cues ride their own PID.


### How do segmentation descriptors and UPIDs affect ad decisioning?


Segmentation descriptors (segmentation_upid, segmentation_type, segmentation_event_id, segmentation_duration) provide IDs and intent (e.g., ad pod, chapter, blackout) so ADS/SSAI systems can map cues to campaigns, enforce blackouts, and generate consistent reporting.


### How does Bitmovin’s Live Encoder handle SCTE-35?


Bitmovin Live Encoder parses SCTE-35 from MPEG-TS input and maps triggers to HLS manifest tags. It supports pass-through and processing modes and maps segmentation descriptor types (program, chapter, break, provider/distributor advertisement/placement opportunity, etc.) to splice decisions according to compatibility with splice_insert and time_signal.


### Does SCTE-35 work for both live and VOD?


Yes, though historically live-centric, modern streaming workflows support SCTE-35 in VOD manifests or during packaging for SSAI and targeted ad logic.


## Resources


[Tutorial: Bitmovin Live Encoding with SCTE-35, HLS and SSAI](https://developer.bitmovin.com/encoding/docs/live-encoding-with-hls-scte-35-and-ssai)


[Guide: Bitmovin Live Encoding and AWS MediaTailor for SSAI](https://developer.bitmovin.com/encoding/docs/live-encoding-with-aws-mediatailor)


[Guide: Bitmovin Live Encoding with Broadpeak.io for SSAI](https://developer.bitmovin.com/encoding/docs/live-encoding-with-broadpeak-io)


[SCTE website](https://www.scte.org/)


[SCTE-35 specification](https://www.scte.org/standards/library/catalog/scte-35-digital-program-insertion-cueing-message/)


[SCTE-35 payload parser](https://comcast.github.io/scte35-js/)


[Bitmovin Live Encoding data sheet](https://go.bitmovin.com/hubfs/One%20Sheets/bitmovin-one-sheet-live-event-encoder-final.pdf)
