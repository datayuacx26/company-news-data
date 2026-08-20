---
schema_version: "1.0.0"
document_id: "95e7c4738a2243d910ccb51e8eabed34310ea768af863a31b0b9a3965b457150"
company_key: "yc-feather-2"
company: "Feather"
source_id: "yc-feather-2-news-import-072c928cc62c"
canonical_url: "https://www.featherhq.com/old-blog-2/what-is-telephony-pstn-sip-how-modern-voice-ai-connects-to-the-phone-network1"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-08-13T07:12:50.962488+00:00"
fetched_at: "2026-08-13T07:12:52.895889+00:00"
content_hash: "sha256:21bfda3a33ae5af7234d1718658f33ec8d010c4ae0de6e030a42c6263431fb50"
---

# What Is Telephony? PSTN, SIP & How Modern Voice AI Connects to the Phone Network1

The transition from circuit-switched PSTN infrastructure to modern AI-powered voice operations runs almost entirely through one protocol: **SIP** , the Session Initiation Protocol. If the PSTN is the highway system that moves voice calls around the world, SIP is the on-ramp that lets modern software-based systems enter and exit that highway.


Understanding SIP, and specifically understanding what a **SIP transfer** is and how it works, closes the gap between the abstract promise of AI voice agents and the concrete mechanics of how they actually handle your customers' calls.


### What Is SIP?


**SIP (Session Initiation Protocol)** is an application-layer signaling protocol defined in RFC 3261. It is used to initiate, maintain, and terminate real-time communication sessions, primarily voice and video calls, over IP networks. SIP handles the "control plane" of a call: it says "I want to call this number," "the called party is ringing," "the call is now connected," and "the call has ended."


SIP does not carry the actual voice audio. That job belongs to **RTP (Real-time Transport Protocol)** , which streams the media between endpoints once SIP has established the session. Think of SIP as the reservation system at a restaurant and RTP as the food delivery itself.


Key SIP concepts every telephony buyer should know:


-


**SIP trunk** : A virtual bundle of telephone lines delivered over an internet connection, replacing physical T1/PRI lines. A SIP trunk connects your business's telephony platform to a carrier's network, and from there to the PSTN. SIP trunks are sold in channels (each channel handles one simultaneous call).


-


**SIP endpoint** : Any device or software application that can send and receive SIP messages. This includes IP desk phones, softphone apps, contact center platforms, and AI voice agent infrastructure.


-


**SIP proxy / SIP server** : The intermediary infrastructure that routes SIP messages between endpoints, similar to the role a DNS server plays for web traffic.


-


**Codec** : The algorithm used to compress and decompress voice audio for transmission. Common codecs include G.711 (uncompressed, highest quality, standard on PSTN), G.729 (compressed, lower bandwidth), and OPUS (the codec increasingly preferred for AI voice applications because of its adaptability and low latency).


### What Is a SIP Transfer?


A **SIP transfer** is the process of moving an active call from one SIP endpoint to another. In a contact center context, this is how a call gets handed from an AI voice agent to a human agent, or from one department to another, without forcing the caller to hang up and dial a new number.


There are two primary types of SIP transfer:


1.


**Blind transfer (also called unattended transfer)** : The originating endpoint transfers the call to a new destination without first checking whether that destination is available. The caller may hear ringing or hold music. If the destination does not answer, the call typically fails. Blind transfers are simple and low-latency, but they offer no quality assurance.


2.


**Warm transfer (also called attended or consultative transfer)** : The originating endpoint first establishes a connection with the transfer destination, confirms the destination is ready and has relevant context about the call, and then bridges the caller through. The caller may hear hold music briefly while the handoff is coordinated. Warm transfers are more complex to implement in SIP terms (they require a third-party call control flow), but they are far superior from a customer experience standpoint.


This distinction matters enormously in AI voice agent deployments. A platform that can only execute blind SIP transfers puts callers at risk of dropped handoffs and forces human agents to re-ask questions the AI already answered. A platform that executes **warm transfers with full context attached** creates a seamless experience where the human agent already knows who they are talking to and why.


### SIP Transfers in the AI Voice Agent Context


When an AI voice agent handles a call, the SIP architecture typically looks like this:


-


The caller dials a phone number.


-


The PSTN routes the call to a SIP trunk associated with the AI platform.


-


The AI platform's SIP endpoint answers the call and begins the conversation.


-


The AI qualifies the caller, answers questions, or completes a workflow.


-


At a decision point (for example, the caller qualifies as a hot lead or needs a specialist), the AI initiates a SIP transfer to a human agent or another system.


The quality of that transfer step is one of the clearest differentiators between AI voice platforms. A warm transfer with context attached requires the platform to not just execute the SIP mechanics correctly, but to also pass a structured summary of the conversation to the receiving agent in real time. This is a software-level capability layered on top of the SIP infrastructure.


### VoIP vs. PSTN vs. SIP Trunking: Clearing Up the Terminology


These three terms are frequently conflated, and the confusion causes real problems when evaluating telephony platforms.


-


**VoIP (Voice over Internet Protocol)** is the broad category of any voice communication that travels over IP networks rather than dedicated circuit-switched lines. VoIP encompasses SIP, WebRTC, proprietary protocols like Cisco SCCP, and more. All SIP calls are VoIP, but not all VoIP calls use SIP.


-


**PSTN** is the global telephone network infrastructure described in the previous section. It is the destination that most calls ultimately reach, even if they start their journey over VoIP.


-


**SIP trunking** is specifically the service of connecting a business's on-premise or cloud telephony system to the PSTN using SIP over the internet, instead of physical telephone lines. SIP trunking providers include carriers like Twilio, Bandwidth, Vonage (now part of Ericsson), and Lumen.


For an AI voice platform, the typical connectivity chain is: **caller on PSTN > SIP trunk provider > AI platform's SIP endpoint > AI conversation logic > SIP transfer back to human agent or CRM workflow** .


### How AI Voice Platforms Differ in Their Telephony Architecture


Not all AI voice platforms connect to the phone network the same way. The architectural choices made here have significant implications for call quality, compliance, and operational reliability.


**Developer-first platforms like Vapi** give engineering teams direct access to the underlying telephony stack. You can bring your own SIP trunk, configure your own codec preferences, and route calls however your infrastructure requires. This is powerful and flexible, but it requires significant engineering investment to get right. The telephony configuration is your responsibility.


**Mid-market platforms like Retell AI** sit between the developer toolkit and the fully managed service. They abstract some of the telephony complexity but still expect buyers to make meaningful technical decisions about call routing and infrastructure.


**High-volume outbound platforms like Bland AI** are optimized for dialing at scale. Their telephony architecture is tuned for throughput on outbound campaigns. Features like warm transfer and inbound scheduling workflows have historically been gated behind enterprise tiers.


**Business-ready platforms like Feather AI** are built for operations teams that need a working, compliant calling operation without assembling the telephony stack themselves. The SIP connectivity, codec configuration, carrier redundancy, and call quality monitoring are handled at the platform level. The buyer's responsibility is configuring the agent's behavior and workflows, not managing SIP trunks.


### Codecs, Latency, and Call Quality in AI Voice Deployments


Any discussion of telephony for AI voice agents has to address latency. Conversational AI requires very low end-to-end latency to feel natural. If the gap between a caller finishing a sentence and the AI responding is longer than about 600 to 800 milliseconds, the interaction starts to feel broken.


The codec choice directly affects latency. G.711, the standard PSTN codec, introduces minimal encoding delay but requires more bandwidth. G.729 compresses audio more aggressively, reducing bandwidth at the cost of slightly higher encoding latency and marginally lower voice quality. OPUS, widely used in WebRTC and modern AI voice stacks, is adaptive and particularly well-suited to the variable network conditions of internet-delivered calls.


Beyond the codec, latency in an AI voice call accumulates across several stages: the SIP signaling round-trip, the media streaming path, the speech-to-text (STT) processing, the LLM inference for response generation, and the text-to-speech (TTS) rendering. A platform optimized for production-grade voice AI needs to minimize latency at every stage, not just the telephony layer.


This is why evaluating a voice AI platform purely on its AI capabilities, without examining its telephony architecture, leads to poor purchasing decisions. The best language model in the world will feel clunky if the underlying SIP configuration adds 300 milliseconds of unnecessary signaling delay.
