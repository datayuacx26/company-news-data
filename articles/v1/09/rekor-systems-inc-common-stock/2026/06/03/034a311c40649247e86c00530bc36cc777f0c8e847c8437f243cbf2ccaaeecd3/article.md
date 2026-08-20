---
schema_version: "1.0.0"
document_id: "034a311c40649247e86c00530bc36cc777f0c8e847c8437f243cbf2ccaaeecd3"
company_key: "rekor-systems-inc-common-stock"
company: "Rekor Systems Inc."
source_id: "rekor-systems-inc-common-stock-news-import-6ba796de3125"
canonical_url: "https://www.rekor.ai/post/rekor-introduces-go-secure-video-proof-that-video-is-real"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-25T20:56:40.637818+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:e5ec945416c254985155e86d82feec6a349848cd0bac1bcc6a1102c993935889"
---

# Rekor Introduces Go-Secure.Video: Proof That Video Is Real

## In a World Where Video Can Be Edited, Clipped, Manipulated, AI-Generated, or Challenged After the Fact, Go-Secure.Video Answers the Only Question That Now Matters: Can a Video Be Proven to Be Authentic?


COLUMBIA, Md., June 30, 2026 (GLOBE NEWSWIRE) -- Rekor Systems, Inc. (NASDAQ: REKR), which builds trusted-data, privacy, and security solutions for real-world video and sensor networks, today introduced[Go-Secure.Video](https://www.globenewswire.com/Tracker?data=yA4U_pY46SD7AvQTO0LpcF_lNlcjJLpe7nUJnu5eiC9ZiDsqswFbr1BsLFbhAJkc-76hmwXojsEe-xnDBWiec6UgCo0vwCHa9p79MGiO_dk=) , a media authentication technology that answers a question courts, insurers, and law enforcement agencies are increasingly being forced to confront: Can this video be proven authentic?


Rekor's patent-pending technology cryptographically authenticates video content at the exact moment of capture, creating a tamper-evident, cryptographically verifiable record tied to the originating device, date/time/location, and bound into the bitstream; if anyone alters a single pixel, frame, or element after recording. Go-Secure.Video precisely identifies where and when the alteration occurred. The system does not analyze probability. It does not flag suspicious content. It delivers a binary, evidence-admissible determination: the video is authentic, or it has been tampered with. No estimates. No guesswork. Just irrefutable proof.


Go-Secure.Video can be used through the Go-Secure.Video mobile app, embedded into recording devices through a software development kit, or integrated directly into OEM platforms, public safety systems, insurance workflows, media systems, enterprise video environments, and other visual-recording ecosystems. The Go-Secure video app is currently available for evaluation on iOS via Apple's TestFlight platform, and Rekor is in discussions with several potential launch partner OEMs regarding broader device and platform integrations.


The scale of the problem is difficult to overstate. More than one billion surveillance and security cameras are operating worldwide, with an estimated 100 million network cameras shipped annually. Add to that more than one billion smartphones shipped each year, nearly all capable of high-quality video capture, plus body cameras, dash cameras, drones, professional cameras, and enterprise video systems, and the volume of visual media being produced globally is without precedent. A growing share of that footage will eventually need to be verified, challenged, preserved, or proven. The tools to do that reliably do not yet exist at scale. Go-Secure.Video was built to change that.


**Built to Answer a Question the Justice System Could No Longer Ignore**
Go-Secure.Video was founded in 2024 after multiple public safety customers asked Rekor a direct question: can video evidence captured by[Rekor Scout](https://www.globenewswire.com/Tracker?data=6vkKBbcaBcyh_RdM650PUmFNUqwFot5J__EVKilw31lXoNw77ha7o-IhkJbx0Mn67GedHvRlh5O6-F02o9m3nlmVCKtngMM4gObTwGz-Ru4=) ® be faked? Prosecutors and defense attorneys were using that footage in criminal proceedings and needed assurance that authenticity challenges could be answered with certainty, not probability.


Rekor responded by developing and filing patents for a chain-of-evidence framework for video and still images. The result is Go-Secure.Video, developed by Rekor Labs.


**How Go-Secure.Video Works**
Go-Secure.Video authenticates media in real time, not after the fact. The video is cryptographically signed at the point of capture/creation, directly on the recording, embedding a cryptographic V-Chain through signed SEI headers and hardware attestation (using Rekor Labs' patent-pending session-anchored, multi-layer, multimodal, change-hashing architecture). The resulting signature is permanent and independently verifiable by authorized parties without moving content outside the user's infrastructure.


The Validation Portal accepts any submitted video and returns detailed deterministic verdicts.


- Authentic and Complete — the content is unaltered and verified from the originating device.
- Partially Authentic (portions of the video are authentic and here are the differences):
- The video clip is a subset of a longer file. Here are the parameters: a clip is the usable unit in video; it can be authenticated thereafter at the frame level (GoSecure understands the completeness and content of the video).
- The video has been Edited/Clipped at the beginning - the beginning of the video has been altered (where and by how much).
- The video has been Edited/Clipped at the end - the end of the video has been altered from its original length (and where).
- The video has Been Edited/clipped internally – there is a segment that has been altered (where and by how much).
- Not a Go-Secure.Video (or completely altered) — the file was not captured through the Go-Secure.Video authentication framework.


**Evidence grade determination. No probabilistic scoring. No AI confidence intervals.**


**Built to Go Further.**


Go-Secure.Video operates at the clip level rather than the file level, maintaining internal context, Device and Situational Binding, and by embedding provenance within the bitstream itself at the hardware layer rather than applying it as a post-capture wrapper.


Existing AI-based video tampering detection tools are probabilistic and can operate on an extracted clip, but cannot provide the cryptographic certainty needed. Go-Secure.Video solves the problem they cannot: cryptographically prove authentication, completeness, and/or edit locality (down to the frame level) within a clip extracted from a broader file, such as a GoSecure-signed file.


**The Market Could Not Be More Urgent**
Deepfake technology is no longer the domain of sophisticated actors. It is widely accessible, and its impact on the credibility of surveillance video is documented and accelerating. Leading trade publications covering the physical security industry have reported extensively on the rapid expansion of AI-generated video and the growing threat it poses to evidence integrity across public safety, insurance, and commercial sectors.


When a law enforcement agency submits footage as evidence, when an insurer evaluates a claim, or when a court determines whether video is admissible, the question is always the same: can this be proven? Until Go-Secure.Video, there was no definitive answer. There is now.


**Leadership Statement**
*"Go-Secure.Video is what happens when you take a real problem from real customers and build all the way to a definitive solution. This is not a detection tool. It is a proof system. That distinction matters enormously in the markets we serve. We built Go-Secure.Video because we were asked to testify as an expert witness on video evidence and discovered there was no reliable way to prove authenticity at the clip and frame level. That gap was unacceptable. We closed it."*


*— Chris Kadoch, CTO, Rekor Systems*


**Target Markets**
Go-Secure.Video serves any organization or individual who requires proof of media authenticity. Primary markets include:


- Law enforcement agencies require chain-of-custody certification for video evidence in criminal proceedings.
- Insurance carriers evaluating the authenticity of video submitted in connection with claims.
- Courts and legal counsel require admissibility-grade certification of video and photo evidence.
- Commercial enterprises and security operators managing surveillance infrastructure where the authenticity of footage may be questioned.
- Camera manufacturers and platform providers seeking to embed authenticated capture capabilities into their hardware and software offerings, including video conferencing platforms.


**About Rekor Systems, Inc.**
Rekor Systems, Inc. (NASDAQ: REKR) builds trusted-data, privacy, and security solutions for real-world video and sensor networks. Rekor's AI-powered roadway intelligence platforms are deployed across the United States, delivering real-time data and actionable insights to transportation agencies, law enforcement, and commercial operators. For more information, visit[Rekor.ai](https://www.globenewswire.com/Tracker?data=6vkKBbcaBcyh_RdM650PUlXBm8hQwkitPfhrO3Sjw4HVJMSFjwuKsvwZ-X7zgYDUV85KTovo8hAA06pnTpv73w==) ; for Go-Secure.Video, visit[go-secure.video](https://www.globenewswire.com/Tracker?data=JXMFd9zaI3ZU2CnQ48LRvfsW_SvRLrymeo6Ti-sUYecVtq1nPPDwFeUPmLV2OH9g5a0Wa5Bvn8PPMQKHRJy8AGfqBSO0n109sMlROOqoCFU=) .


**Media & Investor Relations**
Charles Degliomini
Executive Vice President
Rekor Systems, Inc.
[cdegliomini@rekor.ai](https://www.globenewswire.com/Tracker?data=74dEBFm4Pg1C5XC9jFO3x2zSOuOjUO1HyTFuzmO1B63XyrzWglQlKQpbWqx7IwX9eXoaVTmjUtb-bVHpxE5ZBG18pLNS0ZjqhC9KWKHYLws=)


**Forward-Looking Statements**
*This press release and its links and attachments contains forward looking statements within the meaning of the Private Securities Litigation Reform Act of 1995 concerning Rekor Systems, Inc. that involve substantial risks and uncertainties, including particularly statements regarding our future results of operations and financial position, business strategy, prospective products and services, timing and likelihood of success, plans and objectives of management for future operations and future results of current and anticipated products and services. These statements involve uncertainties, such as known and unknown risks, and are dependent on other important factors that may cause our actual results, performance, or achievements to be materially different from the future results, performance or achievements we express or imply. For this purpose, any statements that are not statements of historical fact may be deemed to be forward-looking statements. In some cases, you can identify forward-looking statements by terms such as "may," "will," "should," "expect," "plan," "anticipate," "could," "intend," "target," "project," "contemplates," "believes," "estimates," "predicts," "potential," or "continue," or the negative of these terms or other similar expressions. These forward-looking statements speak only as of the date they are made and are subject to a number of risks, uncertainties and assumptions described under the sections in our Annual Report on Form 10-K for the year ended December 31, 2024 entitled "Risk Factors" and in our subsequent Quarterly Reports on Form 10-Q filed with the SEC. Given these risks and uncertainties, readers are cautioned not to place undue reliance on such forward-looking statements. Readers are urged to carefully review and consider the various disclosures made in this Press Release and in other documents we file from time to time with the SEC that disclose risks and uncertainties that may affect our business. The forward-looking statements in this Press Release do not reflect the potential impact of any divestiture, merger, acquisition, or other business combination that had not been completed as of the date of this filing. Because forward-looking statements are inherently subject to risks and uncertainties, some of which cannot be predicted or quantified and some of which are beyond our control, you should not rely on these forward-looking statements as predictions of future events. These forward-looking statements are qualified in their entirety by reference to the risks discussed in our SEC filings. This cautionary statement also applies to any forward-looking statements made during the conference call referenced herein. We do not undertake any obligation to publicly update any forward-looking statements, whether as a result of the receipt of new information, the occurrence of future events, or otherwise.*


‍
