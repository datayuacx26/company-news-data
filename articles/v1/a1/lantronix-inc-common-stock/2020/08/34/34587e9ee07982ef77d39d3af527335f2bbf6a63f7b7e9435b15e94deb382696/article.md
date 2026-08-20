---
schema_version: "1.0.0"
document_id: "34587e9ee07982ef77d39d3af527335f2bbf6a63f7b7e9435b15e94deb382696"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/leverage-qualcomms-socs-voice-control-capabilities-system-modules-lantronix/"
published_at: "2020-08-06T18:54:42+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T22:26:40.232877+00:00"
content_hash: "sha256:e8cf767a6420e0e08119bbd80409ce861e36f2c400c72d68603f1c232ef810df"
---

# Leverage Qualcomm’s SOCs Voice Control Capabilities with System on Modules from Lantronix

August 6, 2020


- [General](https://www.lantronix.com/blog/category/general/)


### Leverage Qualcomm’s SOCs Voice Control Capabilities with System on Modules from Lantronix


Voice control is a crucial user interaction for today’s intelligent devices and peripherals. You can query voice assistants on your phone, on a smart home hub (see our past blog[here](https://www.lantronix.com/blog/smart-home-home-automation-made-easy/) ), in smart appliances and from an increasing number of products every day.


To work properly in terms of voice control, these highly capable intelligent edge solutions require security and low-power consumption. Most of all, these products need to be capable of identifying key words without missing parts of a phrase or query.


This blog describes how Lantronix can help you leverage the unique voice control capabilities in Qualcomm®’s SoCs to perform wake-up, keyword recognition and other actions based on phrases. In addition to voice control, this blog also explains how to enable a secure, low-power implementation while leveraging the power efficiencies available in Qualcomm’s heterogenous SOCs.


For Intelligent, battery-operated devices, Snapdragon Voice Activation (SVA) leverages the unique capabilities of Qualcomm’s advanced integrated processors. It enables devices to use the minimum possible power to listen only for the custom wake phrase set by the OEM or designated by the specific user, enabling both a secure and power-efficient solution.


Where systems integrate voice assistants or have other complex voice control-use cases, features such as “barge-in” and “natural language” are essential. This capability allows key phrases to overlap with system audio responses or merge together with a user’s command words. SDA845 and later products support the upgraded SVA version 3.0.


SDA845 and later products support the upgraded SVA version 3.0. Visit the[Open-Q™ 845 µSOM](https://www.lantronix.com/products/open-q-845-usom/) page to learn more.


**SVA 3.0 provides the following set of advanced features:**


- **First-stage detection**


- Occurs in one of the dedicated Qualcomm® Hexagon DSP cores: Either the WDSP or ADSP


- In the standalone use case, detection occurs in the WDSP
- During barge-in, detection occurs in the ADSP


- Based on Gaussian mixture models
- Focuses on maximizing detection


- **Second-stage detection**


- Contains two algorithms:


-


-


- Convolutional Neural Network (CNN) SVA (predefined keyword)
- Voice Print (user verification)


- Runs in the application processor and is only activated when first-stage detection is successful
- Focuses on minimizing false alarms


- Pre-defined keyword detection
- PDK + user verification
- User-defined keyword detection
- Support for multi-microphone inputs:


- WDSP 1-2 channel input
- ADSP 2-6 channel input


- SVA algorithms in the WDSP and ADSP are updated to:


- Support multichannel voice activity recognition
- Output the best channel index with the highest confidence rate to the application processor along with the detection result


- Far field voice activation via the ADSP with multi-mic processing for advanced noise suppression including Fluence beam-forming capabilities.


If enabled, the MAD codec hardware is always on and listening to mic activity. For platforms using codecs such as the WCD93xx, detection is done by the SPE in the codec. APSS wakes up only after the codec detects the keyword phrase and/or the user. The HAL in the APSS notifies the app of keyword detection.


**Low Power:** Only the first stage of detection, Mic Activity Detection (MAD), executes within the codec. LPASS is woken up to perform the remaining stages of detection.


If enabled, the codec is always on, listening to activity on the mic. LPASS runs the Listen Stream Manager (LSM), wakes up on request from the codec and detects the keyword phrase and the user. When the specified keyword phrase is identified, LPASS wakes up the APSS where the driver notifies an app predefined by the licensee.


**High Power:** All stages of detection, including MAD sound detection, execute within LPASS. A codec, if present, is not used for detection.


When enabled, all stages of Listen Detection are run on the LPASS, including the MAD (listening to activity on the mic). When mic activity at a particular level is detected, the LSM detection stages also run on LPASS. When the specified keyword phrase is identified, LPASS wakes up the APSS where the driver notifies an app predefined by the licensee. In this case, the codec is optional. Digital mics could be directly attached to the Qualcomm processor for cost reduction and board layout simplification.


**Sample Android** **™ Application:** Lantronix has developed a sample Android Application that uses Qualcomm-provided ListenEngine API’s. The customer can refer to this sample application for integrating the features into the main application. For more details, please contact the Lantronix Sales Team.


**Keyword models:** Lantronix’s team has experience in tuning keyword models. The Qualcomm keyword processing engine uses a phoneme graph-based model that can be refined with dedicated and targeted recordings for speaker, accent and noise robustness. Lantronix has assisted customers to refine their models for detection in difficult audio environments.


For more details on refining keyword models or other ways to optimize Voice Activation in your Open-Q-based system, please contact the Lantronix Sales Team at[\[email protected\]](https://www.lantronix.com/cdn-cgi/l/email-protection#afdccec3cadcefc3cec1dbddc0c1c6d781ccc0c2) .


Author:


Suzan Moidin is a Senior Member of Technical Staff at Lantronix. He has more than nine years of industry experience in embedded software product development, including Linux® and Android BSP development.
