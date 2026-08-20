---
schema_version: "1.0.0"
document_id: "6227b7671e3c4ec4a70c7ea849da4addc51415c51e7f099b2c3704c56de6f218"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/what-is-voip-phone"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:e8b64eb17704ff869aace719a835fee8b279b7053373f512d0d9b9ceaaccf39b"
---

# What is a VoIP phone (and how does it work)?

## What is a VoIP phone (and how does it work)?


A VoIP phone is any device that makes and receives calls over an internet connection instead of a traditional phone line.


That device can be a desk phone that looks like a landline, an app on your laptop, your mobile phone, or a browser tab. What makes it a VoIP phone is where the audio travels: *as data packets across the internet, rather than as an electrical signal down a copper pair.*


That's the basics, but it gets interesting when you look at the parts that decide whether your calls sound good, reach 911 correctly, and survive a security audit. That's what this covers.


## Key takeaways


- **A VoIP phone sends voice as data packets over an IP network** instead of using the circuit-switched public telephone network (PSTN).
- **VoIP uses two protocols, one for setup and one for audio.** SIP negotiates the call, and RTP carries the sound.
- **Call quality depends on three numbers:** Latency under 150 ms one-way, jitter under 30 ms, and packet loss under 1%.


## What is a VoIP phone?


A VoIP phone is a device or application that transmits voice as digital data over an internet connection, using Voice over Internet Protocol.


The call never touches the copper network unless it needs to reach someone who's still on it. Two VoIP endpoints talking to each other stay entirely on IP. When a VoIP caller dials a landline or a mobile number, a gateway hands the call off to the PSTN.


VoIP stands for Voice over Internet Protocol. You'll also see it written as IP telephony, internet telephony, or voice over IP. They all describe the same thing: Carrying voice traffic over packet-switched IP networks rather than the circuit-switched PSTN.


## Types of VoIP phones


VoIP phones can take several forms:


- **Hard phones:** Desk phones that look like traditional handsets but plug into ethernet or Wi-Fi instead of a phone jack. Common in call centers and offices.
- **Softphones:** Applications running on a laptop or desktop. Microsoft Teams, Zoom, and browser-based dialers all fall here.
- **Mobile VoIP:** Apps on a smartphone that place calls over cellular data or Wi-Fi rather than the carrier's voice network. WhatsApp and FaceTime work this way.
- **ATA adapters:** A small box that converts an analog phone into a VoIP endpoint, so existing handsets keep working.
- **Conference and room devices:** Speakerphones and video bars that register to a VoIP system the same way a desk phone does.


## VoIP phones vs. traditional phones


VoIP phones and traditional phones have quite a few differences for fulfilling a relatively similar purpose. Here are the main differences:


- **Pricing:** Cloud-based VoIP calls are cheaper than traditional phone landline calls. Plus, most customers and businesses already have internet installed, so while VoIP demands additional usage fees, you won’t be purchasing additional services.
- **Features:** VoIP providers offer several advanced features you won’t find with traditional phones, like automated attendants, call parking, call flipping, real-time analytics, business integrations, and more.
- **Device flexibility:** Traditional phones are often limited to a specific physical location, while VoIP phones can be your desktop, desk phone, or on-the-go mobile device.
- **Emergency services:** VoIP phone numbers are location-dependent, so dispatchers don’t always know where to send emergency services unless the caller provides the details during a call. This makes traditional phones a more reliable system for emergency calls.
- **Call quality and reliability:** VoIP calls rely on a strong internet connection, which used to be less accessible years ago. However, you can now find reliable bandwidth in most locations, making[VoIP phones just as reliable and high quality](https://www.twilio.com/blog/voip-call-quality) as traditional phones.
- **Scalability:** Scaling VoIP phone systems is simple—you can purchase additional lines and licenses and enable your employees’ personal devices. However, scaling traditional phones is a bit more complex—you’ll need to purchase more phone sets and wired phone lines to add more users to your system.


## How do VoIP phones work?


At a high level, a VoIP phone converts your voice into data, sends it across the internet, and converts it back to audio on the other end. The detail that most explanations skip is that this happens over two separate protocols doing two separate jobs.


### Signaling and media are different things


**SIP (Session Initiation Protocol) handles signaling.** It's the part that finds the other party, rings their phone, negotiates what audio format both sides can speak, and tears the call down when someone hangs up. SIP carries no audio at all.


**RTP (Real-time Transport Protocol) carries the media.** Once SIP has negotiated the session, the actual voice packets flow over RTP, usually directly between the two endpoints.


This split matters in practice. When a call connects but nobody can hear anything, the problem is almost always RTP being blocked by a firewall or mangled by NAT while SIP passes through fine. Knowing which protocol failed tells you where to look.


Here's the full sequence:


1. **Registration:** Your phone tells the VoIP provider where it is, and authenticates.
2. **Invitation:** You dial. Your phone sends a SIP INVITE toward the destination.
3. **Negotiation:** Both sides exchange the codecs and network addresses they can use.
4. **Media flow:** The call connects. Audio is sampled, encoded, packetized, and sent over RTP, typically in 20 ms chunks.
5. **Decoding:** The receiving device buffers the packets, reorders anything that arrived out of sequence, and plays the audio.
6. **Teardown:** Someone hangs up, a SIP BYE goes out, and the session ends.


### Codecs and what they cost you


The codec decides the tradeoff between bandwidth and sound quality.


- **G.711:** Uncompressed, around 64 kbps of payload before overhead. Best quality of the legacy codecs, and the default on most PSTN interconnects.
- **G.722:** Wideband, or HD voice. Same bandwidth as G.711, noticeably richer audio, though both endpoints have to support it.
- **Opus:** Adaptive and modern. Adjusts its bitrate to network conditions in real time. The standard for WebRTC and browser-based calling.
- **G.729:** Heavily compressed at around 8 kbps of payload. Useful on constrained links, at the cost of audible quality loss.


Real-world bandwidth runs higher than the payload number because of IP, UDP, and RTP headers. Budget roughly 85 to 100 kbps per concurrent G.711 call in each direction.


## What determines VoIP call quality


VoIP call quality comes down to three network measurements. Get them inside these ranges and calls sound clean. Miss them and no amount of provider-switching helps.


- **Latency:** The one-way delay between speaking and being heard. The[ITU-T G.114 recommendation](https://www.itu.int/rec/T-REC-G.114) puts the ceiling for good quality at 150 ms one-way. Between 150 and 300 ms, people start talking over each other. Past 300 ms, the conversation stops feeling like a conversation.
- **Jitter:** The variation in packet arrival times. Under 30 ms is the working target. Jitter buffers smooth out small variations by holding packets briefly before playback, which trades a little latency for stability.
- **Packet loss:** Voice tolerates less than 1% loss before it becomes audible as clipping or dropouts. Unlike a file transfer, there's no time to retransmit a lost voice packet, so it's gone.
- **MOS (Mean Opinion Score):** A 1 to 5 quality rating. Above 4.0 is considered good. G.711 under clean conditions tops out around 4.4, which is the practical ceiling for standard voice.


Bandwidth is rarely the constraint. A single call needs under 100 kbps, which any broadband connection handles. The problems come from a network that's fast on average and inconsistent under load, which is why QoS rules that prioritize voice traffic tend to fix more calls than a bigger pipe does.


If you're running voice at scale, instrument it.[Twilio Voice Insights](https://www.twilio.com/en-us/voice/insights) surfaces these metrics per call, so you can tell the difference between a bad provider and a bad Wi-Fi access point.


## Should you use a VoIP phone system?


VoIP phone systems offer[significant advantages](https://www.twilio.com/blog/reasons-to-have-voip-call-center) over traditional phone landline systems, but these aren't right for everyone. Here are a few benefits and drawbacks you should consider before investing in your business communications system:


### Benefits of VoIP phones


- **Call routing:** Easily route your calls to a VoIP phone, regardless of where you've plugged in your physical network. This allows you to take calls anywhere in the world, as long as you have a reliable internet connection.
- **Portable numbers:** Since VoIP numbers are virtual, you can take them wherever you go. That means you keep the same number anywhere in the world.
- **Lower costs:** Because VoIP doesn't require investments in traditional private branch exchange or PBX systems and landlines, you can switch and save significantly on your phone bill. Plain old telephone services (POTS) cost your business each month for each line, while[VoIP saves you money](https://www.twilio.com/client/pricing) with[pay-as-you-go pricing](https://www.twilio.com/voice/pricing/us) . This allows you to easily scale up or down in response to demand.
- **Clearer quality:** Thanks to the evolution of the internet, finding a high-quality connection is simple and easy. This leads to clear calls with no noticeable latency, lag, or dropouts. However, if you don't have a quality connection, VoIP can become a con (more on that later).
- **Multichannel functionality:** SIP trunking enables you to provision[SMS and MMS](https://www.twilio.com/learn/messaging/what-are-sms-and-mms) functionality from your phone numbers, allowing you to send texts, images, and videos—creating a more seamless conversation.
- **Security and privacy:** VoIP technology can eliminate security and privacy threats with encryptions and identity management advancements.


Plus, with[Twilio Voice Insights](https://www.twilio.com/voice/insights) , you can analyze your call performance to troubleshoot customer issues faster and deliver better communication experiences.


While VoIP has a number of benefits, it's not all sunshine and rainbows. There are a few downsides to VoIP that you should keep in mind.


### Drawbacks of VoIP phones


- **Quality dependent on internet connection:** VoIP is only as reliable as your internet connection. While VoIP doesn't require too much bandwidth, you'll still need low latency on your network for lag-free conversations.
- **Limitations for emergency services:** The 911 system is built with traditional phones in mind. So when you dial 911, first responders will be alerted to the fixed location of your landline. However, with the portability of VoIP calling, calling 911 will only transmit the address you provided at the time of setup—not your current location. Of course, you could provide your location details when making the 911 call, but that's not always a possibility in an emergency.
- **Power dependency:** If you lose power and have no backup solution, your VoIP phones won't work. However, landlines don't require local electricity to make calls—they rely on power sent through the landline from the power companies. These companies usually have backup generators in case of a broad power outage, allowing you to keep making calls.


## What to look for in a VoIP provider


Not every VoIP provider offers the right set of features and functionality for your business. Some VoIP providers simply offer additional phone numbers and call-forwarding features, while others enable SMS, team messaging, file sharing, and call management features.


Here are a few things you’ll want to consider when looking for the right VoIP provider:


- **Price:** Think about whether you can afford the VoIP provider's different tiers. Some charge for more advanced features, while others provide free international calling to select destinations. Examine your budget and determine what you can afford now and at scale.
- **Scalability:** Consider how big you plan to grow your team and customer base. Some systems cater to small teams, while others support big businesses with hundreds or thousands of employees.
- **Features:** Think about what features you’ll need to support your team and provide top-notch experiences for your customers. This could be[IVR systems](https://www.twilio.com/blog/ivr-what-it-is-and-how-it-benefits-contact-center) , multilevel automated attendants, call parking, video conferencing, and (possibly, most important) APIs,[SDKs](https://www.twilio.com/voice-sdk) , and integrations.
- **Channel support:** Consider what channels you plan on using for your business. If you’re just looking for voice systems, you can go for cheaper options. However, if you want to add SMS, video conferencing, team messaging, WhatsApp, Facebook Messenger, and live streaming, you’ll need a more advanced VoIP provider.


## Build a VoIP system with Twilio


Consumers and businesses alike are dropping old landline technology in favor of wireless and VoIP-enabled phones, and it's not hard to see why. Businesses that adopt this technology now will have success for years to come and get access to all the VoIP advantages we shared.


With[flexible voice pricing options](https://www.twilio.com/voice) , there's no reason to delay making the switch. Want to get started?[Check out all our no-code voice apps](https://www.twilio.com/code-exchange?q=&f=voice&f=serverless) that you can build in less than 5 minutes. Plus, take a look at how you can[build a VoIP system with Twilio, 3CX, and Python](https://www.twilio.com/blog/build-voip-system-twilio-3cx-python) .


## Frequently asked questions


### **Q: What does VoIP stand for and what does it mean?**


A: VoIP stands for Voice Over Internet Protocol. It's a technology that lets you make and receive phone calls using the internet instead of traditional phone lines. Think of it as turning your voice into data that travels over the web.


### **Q: Do VoIP phones work during power outages?**


A: No, VoIP phones need electricity and internet to function. Unlike traditional landlines that get power through the phone line itself, VoIP systems stop working when you lose power unless you have a backup power source like a UPS battery.


### **Q: Can I keep my existing phone number with VoIP?**


A: Yes, most VoIP providers allow you to port (transfer) your existing phone number to their service. This process typically takes 1-3 business days and lets you keep the same number customers already know.


### **Q: How much internet speed do I need for VoIP calls?**


A: VoIP requires minimal bandwidth—about 64-100 kbps per call for good quality. A typical broadband connection easily handles multiple simultaneous VoIP calls without affecting other internet activities.


### **Q: Are VoIP calls as clear as regular phone calls?**


A: With a good internet connection, VoIP calls often sound clearer than traditional phones. However, call quality depends entirely on your internet speed and stability—poor connections result in choppy, delayed audio.


### **Q: Can I use my existing desk phones with VoIP?**


A: Sometimes. You can use traditional phones with an adapter (ATA device) or upgrade to IP phones designed specifically for VoIP systems. Many businesses prefer IP phones for better features and integration.


### Q. Are VoIP calls secure?


VoIP calls are secure when encryption is enabled. SIP over TLS protects the signaling, and SRTP encrypts the audio itself. Without both, call metadata and audio are readable on the network. Toll fraud is the other common risk, mitigated with rate limits and anomaly alerting.


##
