---
schema_version: "1.0.0"
document_id: "03525aa3b399929efa4a6c8890c0c655422d96a66260df97f98551fd68555356"
company_key: "yc-giga"
company: "Giga"
source_id: "yc-giga-news-import-92619addbaaf"
canonical_url: "https://giga.ai/news/synthetic-voice-detection"
published_at: "2026-08-13T12:00:00+00:00"
first_seen_at: "2026-08-14T07:15:52.332828+00:00"
fetched_at: "2026-08-14T07:15:53.105764+00:00"
content_hash: "sha256:2486829e7d407376407d08493040b977c63cdff7b393b569c6061c718dbd1c0a"
---

# Real-Time Synthetic Voice Detection Comes to Giga

Today we are launching real-time synthetic voice detection on the Giga platform. Every call handled by a Giga voice agent can now be screened, as the conversation happens, for signs that the speaker on the other end is not a person but a machine.


## Why this matters now


Voice used to be the strongest trust signal a business had. If someone called in and sounded like a customer, they almost certainly were one. That assumption has quietly stopped being true, because modern text-to-speech systems can clone a convincing voice from just a few seconds of reference audio1 , and they are cheap, fast, and available to anyone.


The consequences are no longer hypothetical. Criminals used a cloned executive voice to authorize a fraudulent transfer of $243,000 as far back as 20192 , and in 2024 a finance employee in Hong Kong wired roughly $25 million after a video call staffed entirely by deepfaked colleagues3 . Deloitte’s Center for Financial Services projects that generative AI could push fraud losses in the United States to $40 billion by 20274 .


Fig. 01 · The documented cost


Sourced


Three amounts on one money ruler; every major step is ten times the last. The 2019 and 2024 amounts are single documented payouts. The 2027 amount is a projected national annual total, a different kind of number, marked as such. Sources: The Wall Street Journal (2019) · CNN (2024) · Deloitte Center for Financial Services (2024)


Contact centers face a newer, quieter version of the same attack. Instead of one dramatic heist, bad actors can point AI voice agents at customer support lines and run them as persistent callers: placing many routine calls, probing the refund policy, accepting escalating politely, and retrying across interactions until a rare policy edge case pays out. Any single call looks routine, but taken together this is industrialized fraud running at a scale no human caller could sustain. We built this product because our customers, including some of the largest consumer brands in the world, asked us to help them stop it.


Fig. 02 · A week of support calls


Illustration · not customer data


Every dot is a call. In aggregate the week looks routine; isolate one synthetic caller and the campaign appears, a few unremarkable calls at a time.


## Why synthetic voice detection is hard


The obvious approach sounds simple enough: train a classifier that separates real speech from generated speech. The research community has been working on exactly this problem for a decade through the ASVspoof challenge series5 , and its results explain why the problem resists easy solutions.


The first obstacle is generalization. Detectors that score near-perfectly against the synthesizers they were trained on degrade sharply when they meet voices from new, unseen models. Müller et al. found that error rates of published detectors increased severalfold when evaluated on in-the-wild audio instead of curated benchmarks6 . New voice models ship every month, so a detector that memorizes the artifacts of today’s synthesizers is obsolete on arrival. It has to learn what makes speech human rather than what makes one particular model sound fake.


Fig. 03 · Benchmark vs the wild


Sourced


Equal error rate of published detectors: the rate at which real and synthetic speech are confused, so lower is better and 50% is a coin flip. Near zero on the benchmark they trained against; 30 to 60% on found, in-the-wild audio. Source: Müller et al., Does Audio Deepfake Detection Generalize?, Interspeech 2022


The phone network itself works against detection. Much of the research literature is built on clean, high-sample-rate audio, while real calls are narrowband, compressed by lossy codecs, and degraded by packet loss and background noise. The ASVspoof 2021 evaluation showed that these transmission effects can substantially weaken common detection systems7 , since many of the spectral artifacts that give away synthetic speech in the lab live in exactly the frequencies the phone network throws away.


Fig. 04 · What the phone line keeps


Schematic


A schematic speech spectrum. Narrowband telephony carries roughly 300 Hz to 3.4 kHz; everything outside that band, where many synthesis artifacts live, never reaches the detector.


Timing adds a further constraint. A forensic model that analyzes a complete recording after the fact is useful for audits, but it cannot stop a live fraud attempt. In a contact center, detection has to run on streaming audio and produce a confident signal within the first moments of a conversation, while the caller is still mid-sentence, without adding any latency to the call.


Watermarking, finally, covers only part of the threat. Some modern voice models embed watermarks in their output, and proactive schemes such as localized audio watermarking are a promising research direction8 . A watermark, however, only helps when the vendor that generated the audio cooperates. Open-source models, fine-tuned clones, and audio that has been re-recorded or re-encoded all fall outside that protection, so a real defense has to catch synthetic speech that was never watermarked at all.


## What we are launching


Giga’s synthetic voice detection runs natively inside our voice platform, with no recording to upload and no separate pipeline to build.


During a live call, the system continuously analyzes the caller’s audio stream and produces a calibrated synthetic-speech signal in real time. It is built for the conditions contact centers actually operate in: narrowband telephony audio, noisy lines, and callers who speak for only a few seconds at a time. Because it evaluates properties of speech production rather than the fingerprint of any single synthesizer, it is designed to hold up against voice models it has never seen.


What happens with that signal is up to each team. A flagged call can be routed to step-up verification, handed to a human specialist, restricted from sensitive actions like refunds or account changes, or simply logged for fraud analytics. Detection results flow into the same infrastructure that already power Giga voice agents, so fraud and CX teams see the fraud detection results.


Our approach draws on the same lines of research that have defined the field, including end-to-end raw-waveform models9 and graph attention architectures10 alongside self-supervised speech representations11 , which have consistently set the strongest results in the ASVspoof evaluations. We combine these techniques with training pipelines built specifically for telephony conditions and adversarial robustness, and we continuously refresh them as new synthesis models appear.


Real-time synthetic voice detection is available now for Giga customers. If your support line is seeing repeat-caller fraud, policy abuse, or suspected AI callers, reach out to your Giga team or[contact us](https://giga.ai/contact) to see it on your own traffic.


## References


1. Wang, C. et al. (2023). *Neural Codec Language Models are Zero-Shot Text to Speech Synthesizers* (VALL-E). arXiv:2301.02111.
2. Stupp, C. (2019). *Fraudsters Used AI to Mimic CEO’s Voice in Unusual Cybercrime Case.* The Wall Street Journal.
3. Chen, H. & Magramo, K. (2024). *Finance worker pays out $25 million after video call with deepfake ‘chief financial officer’.* CNN.
4. Deloitte Center for Financial Services (2024). *Generative AI is expected to magnify the risk of deepfakes and other fraud in banking.*
5. Wang, X. et al. (2024). *ASVspoof 5: Crowdsourced Speech Data, Deepfakes, and Adversarial Attacks at Scale.* ASVspoof Workshop.
6. Müller, N. et al. (2022). *Does Audio Deepfake Detection Generalize?* Interspeech 2022.
7. Yamagishi, J. et al. (2021). *ASVspoof 2021: Accelerating Progress in Spoofed and Deepfake Speech Detection.* ASVspoof Workshop.
8. San Roman, R. et al. (2024). *Proactive Detection of Voice Cloning with Localized Watermarking* (AudioSeal). ICML 2024.
9. Tak, H. et al. (2021). *End-to-End Anti-Spoofing with RawNet2.* ICASSP 2021.
10. Jung, J. et al. (2022). *AASIST: Audio Anti-Spoofing Using Integrated Spectro-Temporal Graph Attention Networks.* ICASSP 2022.
11. Tak, H. et al. (2022). *Automatic Speaker Verification Spoofing and Deepfake Detection Using wav2vec 2.0 and Data Augmentation.* Odyssey 2022.
