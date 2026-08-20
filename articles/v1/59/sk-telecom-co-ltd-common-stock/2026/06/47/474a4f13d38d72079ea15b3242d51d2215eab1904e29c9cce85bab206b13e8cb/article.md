---
schema_version: "1.0.0"
document_id: "474a4f13d38d72079ea15b3242d51d2215eab1904e29c9cce85bab206b13e8cb"
company_key: "sk-telecom-co-ltd-common-stock"
company: "SK Telecom Co. Ltd."
source_id: "sk-telecom-co-ltd-common-stock-rss-aaa8518f0d7e"
canonical_url: "https://news.sktelecom.com/en/3089"
published_at: "2026-06-01T23:29:38+00:00"
first_seen_at: "2026-07-20T03:32:36.084520+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:03675599b67b5e7e87e5184eb5df7ad6afbf23faafb5e1312150254826e9898a"
---

# [Inside T] 3.8 Seconds from Live Broadcast to Shopping… The “Live-to-Cart” Technology That Captivated the World – SKT Enterprise Solution Development Team

- Facebook
- X
- Email
- Copy URL
- Print


In April 2026, SK Telecom’s “Live-to-Cart” became the only Korean entry to win a Product of the Year award at NAB (National Association of Broadcasters) Show in Las Vegas. NAB Show is the world’s largest broadcasting & media exhibition with a long history that began in 1923 and has continued for more than a century. “Live-to-Cart” is an AI-powered media commerce solution that enables viewers watching live broadcasts to instantly purchase products appearing on screen without having to search separately. SKT Newsroom met with Bae Ju-han and Kim Dong-won of SKT’s Enterprise Solution Development Team to hear about the meaning behind the award, the team’s achievement, and the background behind the technology’s development.


(From left) Kim Dong-won and Bae Ju-han of SKT’s Enterprise Solution Development Team posing for a commemorative photo after winning Product of the Year at NAB Show 2026.


## Proving Global Competitiveness with an AI Media Solution Ready for Commercial Deployment


**Q. Could you introduce the Enterprise Solution Development Team and what the team does?**


**A. Bae Ju-han:** The Enterprise Solution Development Team develops SKT’s AI and media technologies into solutions that can be immediately applied to real business operations for enterprise customers and SK Group affiliates.


**A. Kim Dong-won:** We work on applying core AI technologies such as vision-language models, speech-subtitle processing, and video analysis to media and commerce. We are responsible for integrating these technologies into an end-to-end pipeline optimized for the advertising domain and delivering solutions that can operate in live commercial environments with real traffic.


**Q. Last month at NAB Show 2026, SKT’s “Live-to-Cart” became the only Korean technology to receive a Product of the Year award. How did you feel about the win, and what significance does it hold?**


SKT Enterprise Solution Development Team posing with a participant at NAB Show 2026


SKT Enterprise Solution Development Team’s “Live-to-Cart” demonstration booth at NAB Show 2026


**A. Bae Ju-han:** What makes this award especially meaningful is that Product of the Year is not simply about technological novelty. It evaluates three dimensions comprehensively – innovation, technological excellence, and real-world applicability. We are very proud that global experts recognized our AI solution not just as a model run in a lab, but as a technology that operates in live media service environments and directly connects to business outcomes.


**A. Kim Dong-won:** This achievement was possible because SKT’s accumulated AI capabilities, SK Broadband’s B tv test environment, and commerce partnerships such as Naver Shopping all worked together as a full-stack ecosystem. This award belongs not to one team, but to the entire collaborative ecosystem behind it.


**Q. What do you think were the key factors that enabled SKT’s AI media technology to gain recognition in the global market?**


**A. Bae Ju-han:** The world has much higher expectations for Korean AI media commerce than one may think. This is because it is rare to have a market where high IPTV penetration, massive commerce platforms, and a K-content-driven PPL market all coexist within a single country.


Global operators all had a shared awareness that there wasn’t a commerce system that is integrated directly into live channels. No one had been able to demonstrate a full-stack implementation connecting telcos, IPTV providers, and commerce platforms simultaneously. However, with “Live-to-Cart,” SKT handles product extraction technology, SK Broadband operates the service, and Naver Commerce connects the purchasing links. This integrated process became our greatest competitive advantage and ultimately led to the Product of the Year award.


**A. Kim Dong-won:** “Live-to-Cart” has three major differentiators from a technology perspective. First, its multimodal fusion architecture can simultaneously recognize visuals, subtitles, and on-screen text in live broadcasts for non-registered products to a level where it’s ready for commercial use.


Second, through FP8 QuantizationA technology that converts data into 8-bit floating point format to reduce AI model size and dramatically improve computational speed


and model optimization, we reduced GPU costs to support two to four channels per GPU, achieving the unit economics needed for multi-channel scaling.


Third, this was not just a proof of concept, but a validation of an actual commercial workflow running on live B tv channels that lead to real consumer purchases. I think these three elements together convinced global judges that this was a commercially viable AI solution.


**Q. Were there any memorable responses from or moments with the judges or industry attendees at NAB Show?**


**A. Bae Ju-han:** The question we heard most often was, “Is this really running on actual live channels and not just a proof of concept?” People were also highly interested in GPU costs and commerce platform matching. Ultimately, all the questions came down to commercial viability. Anyone can build an individual AI model these days. But global operators clearly understood that integrating those models into a single pipeline and creating an operational model with a viable unit economics is an entirely different challenge.


**A. Kim Dong-won:** Many people told us, “We have each of these technologies too, but it’s amazing that you integrated them and are actually running them on live channels.”


The most common response was that they had never seen a commercial service in live broadcasts capable of detecting products that were not pre-registered, matching them with purchasable items on a real commerce platform, and displaying them on TV screens within 3.8 seconds.


## Bridging the Gap Between TV and Commerce


Where did this technology begin before it earned global recognition? The starting point behind “Live-to-Cart” was a frustration you may have experienced at least once: seeing a product on TV and wanting to buy it, only for the scene to change before you could find out its name or price. That exact moment became the starting point for the Enterprise Solution Development Team.


**Q. What inspired the “Live-to-Cart” project?**


**A. Bae Ju-han:** Every day, TV viewers around the world see hundreds of products on screen, but when they want to buy one, they still have to pick up their phones and search for it. Whether it’s a jacket worn by an actor in a drama or a drink featured in a variety show, the moment when viewers feel the strongest desire of “I want to buy that” is completely disconnected from the actual purchasing process.


The same issue existed from the operator’s perspective. In IPTV VOD services, people can manually tag products and attach purchase links. However, live channels, which generate the highest traffic, were effectively producing zero commerce revenue. Our starting point was eliminating the disconnect between watching (live broadcast) and purchasing (commerce).


**A. Kim Dong-won:** No existing method could recognize non-registered products in raw live broadcasts in real time. We believed that solving this problem could transform live broadcasting into an entirely new commerce channel.
Fortunately, over the past two to three years, vision-language models reached a critical performance threshold, while quantization technologies such as FP8 made inference costs commercially viable. We realized that those two technological flows were finally converging at the right moment. Another decisive factor was that this was a challenge uniquely suited for SKT, which already had B tv as its own real-world test environment.


**Q. Could you explain how “Live-to-Cart” works?**


**A. Bae Ju-han:** The biggest characteristic of “Live-to-Cart” is its “zero-curation” structure, meaning products do not need to be manually registered or tagged in advance. Instead, AI identifies products appearing in live broadcasts in real time and converts them into purchase information.


**1. Ingest Stage:** The system receives live IPTV multicast streams, extracts keyframes every three to four seconds, and synchronizes them with EPG scheduling information and PPL advertising data. This stage provides contextual information about which program and scene the content belongs to.


**2. Multimodal Analysis Stage:** Each extracted keyframe is simultaneously analyzed by three AI modules.


> **– VLM (Vision-Language Model):** Detects products on screen and extracts visual features and keywords. Object recognition accuracy exceeds 80%.
> **– Caption Module:** Extracts product names and brand mentions from live subtitles.
> **– OCR Module:** Analyzes non-subtitle text displayed on screen, including price tags, brand logos, and CG overlays.


**3. Fusion and Scoring Stage:** The outputs from the three modules are deduplicated using cosine similarity, then scored based on spatial confidence between modalities and temporal relevance. Dark or blurry frames receive lower scores through image-quality filtering to eliminate noise.


**4. Delivery Stage:** Final candidate products are matched through the Naver Shopping API to generate purchasable product cards, which are displayed on the set-top box along with QR codes. Viewers scan the QR code and complete the purchase on mobile devices.


**A. Kim Dong-won:** For example, if a jacket appears in a drama scene, the VLM extracts details such as material, color, and silhouette. The caption module identifies the brand name from dialogue, while OCR reads pricing information from on-screen graphics. When all three signals point to the same product, the system assigns the highest confidence score. It takes approximately 3.8 seconds from the moment a product appears on screen to the moment the product card is displayed, and the matching accuracy exceeds 75%.


**Q. Running three AI modules simultaneously in real time sounds technically challenging. What was the most difficult part of development?**


**A. Bae Ju-han:** There wasn’t necessarily a single technical challenge. The real challenge was ensuring that all three modules could operate simultaneously, in real time, and at commercially viable costs.


The first issue was the resources cost of multimodal simultaneous inference. Running VLM, captioning, and OCR models in parallel for every frame naturally requires more than one GPU per channel. Scaling that to 95 channels would make infrastructure costs commercially unviable. To solve this, we used FP8 quantization and model optimization to process two to four channels per GPU. That optimization decided whether this technology would remain a research project or become a commercially viable business.


**A. Kim Dong-won:** Another major challenge was integrating multimodal outputs. Determining confidence scores became extremely difficult when the three modules identified the same product differently, or only one module produced an incorrect detection. The threshold for deciding “At what score can we confidently say this is the correct product?” dramatically affected the results, so we conducted extensive validation using real broadcast data.


**Q. What kind of responses have you received from consumers? Are there any particularly memorable reactions?**


Demonstrating the technology to attendees at the “Live-to-Cart” booth


**A. Bae Ju-han:** During the live pilot broadcast on B tv in February 2026, 90% of viewers naturally discovered the feature just from on-screen prompts without any prior instructions.


That was the moment we truly realized the technology could change viewing behavior itself. Usually, new IPTV features require users to learn how to use them, which often prevents adoption. But “Live-to-Cart” appears precisely when viewers are already interested in a product on screen. As a result, more than half of viewers used the feature within the first five minutes of viewing, and one in three respondents said the feature would likely make them watch broadcasts longer.


**A. Kim Dong-won:** In terms of purchase intent, 60% of live broadcast viewers expressed interest in products recommended by the AI, and 69% said they would continue using the feature. In addition, 73% said they would be willing to purchase at least one product discovered on screen. What stood out most was that 65% of users said integration with Naver Shopping positively influenced their willingness to use the feature. This suggests that viewers value being connected to a familiar shopping platform they already use regularly, rather than being redirected to an unfamiliar standalone commerce platform.


## Beyond Korea, Toward a Global Standard for TV Commerce


What the pilot proved was not simply a set of numbers, but the possibility that technology could change viewer behavior. Now, the Enterprise Solution Development Team is looking beyond full deployment across B tv’s 95 channels and toward the global stage.


**Q. We understand that you are working with SK Broadband to expand across all 95 B tv channels and eventually aim to license the technology to global IPTV operators. How do you envision the future development of this technology?**


**A. Bae Ju-han:** There are three phases in the “Live-to-Cart” expansion roadmap. Phase one is expanding across 95 commercially applicable B tv channels. Currently, a single channel generates approximately $68,000 in annual revenue. Applying this technology across 95 channels would create a market worth approximately $6.5 million annually.


Phase two is expanding into domestic pay television and FAST (Free Ad-Supported Streaming TV) channels. Korea alone has nine platforms and 283 channel sources, representing an estimated annual market opportunity of approximately $79.2 million.


Phase three is licensing the technology to global IPTV, OTT, and FAST operators. More than 600 channel sources and over 5,000 service nodes worldwide are potential targets, representing an estimated annual market opportunity of approximately $339.8 million. Winning Product of the Year at NAB Show has also significantly expanded our opportunities to connect with global operators.


**A. Kim Dong-won:** We also plan to advance the technology itself in two directions. Currently, products are identified using visuals, subtitles, and text, but we plan to further improve accuracy by incorporating audio signals, cast style analysis, and viewer preference-based recommendations.


The second direction is redefining PPL advertising. Because “Live-to-Cart” can track the entire process of product exposure, clicks, and actual purchases, it can transform PPL into a measurable advertising channel. Our long-term goal is to create a structure where advertisers can verify return on investment, operators gain new revenue streams, and viewers can instantly purchase the products they want.


**Q. Lastly, what kind of project do you think “Live-to-Cart” will remain to each of you personally?**


NAB Show 2026 Product of the Year trophy


**A. Bae Ju-han:** This was the project where I directly witnessed AI changing real viewer behavior. Watching demonstration participants naturally scan QR codes after seeing products on screen made us realize that the system we built was genuinely influencing people’s decision-making. Going forward, I want to continue developing technologies that can consistently generate data and revenue without relying solely on how much buzz a broadcast generates.


**A. Kim Dong-won:** For me, this project was “an experience of personally walking the final mile where technology becomes business.” AI models themselves have advanced tremendously over the past few years, but this project taught me that using those models to impact actual viewer action requires organically connecting everything from infrastructure and costs to operations and external integrations. Going forward, I want to continue building AI that is commercially viable.


The Enterprise Solution Development Team added that the most memorable moment came from a brief comment made by one of the NAB Show 2026 judges after the awards ceremony: “There were many AI-related submissions this year, but very few were AI systems actually being used by real viewers.”


“Live-to-Cart,” the technology that transformed live broadcasting into the world’s first real-time commerce channel, is now moving into full-scale deployment across B tv’s 95 channels. And for the Enterprise Solution Development Team, the journey toward the global stage is only just beginning.


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNWJDQkU5RWZESEI3aGxyTFRRYk1xYTY0a1lmRXZpK1ovWTYyZFZoUTRJSWltc09pdFhZYXBRSEtINllBR00xOGsvNWNiRXEySm9mNnZoWmNpS203OEFRPQ==


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNWZlVm93dFp2Ym0xRkFCRTRYVjJYaGdpSjlJVmsrdllvaEU0SEY2NkhSc2x4ampkbVplWjBkOHV0VS80YklDcnl3PT0=


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNWVGT1o3YmpzMFFLZWlxcTdPdTNlUnF2UHpDWkFTc21NQm5vU0lVRStHNG4=


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNVdsdUtPNGVVNmFFYW4rbUZxcjJGWDQ9


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNWFVZXFFdkVSdEl6bGJMNnVLVFZ5TVNZVzk2eVk0MWkremxVc29TcFdBL3M=


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNVcwd1hXZm85STU2eE1MenpCTjBRQm89


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNVNHcU5ZR3FORFlab3VzTldJVWR4OWM9


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNVpnMVNhN1dnTXoyZk1UcEhobEUzNFE9


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNVV0Ry9BRHdMc3B6d2ttRHZVZ0RZTnM9


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNWRnOWpYWWVZUkhSejVBQ3FrVHZBdGs9


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNWJDQkU5RWZESEI3aGxyTFRRYk1xYTY0a1lmRXZpK1ovWTYyZFZoUTRJSWlsK245OEQ1SVRrdmJRekZUQ0VlSHcxS04xTnRnNDVnSjZOaktYVFlWQm5VPQ==


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNWNqWVRnbHlzOVVyVFZscHlFWE8ySFJHSk53Sk92OTdERHo2ZEFPMlpPanE=


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3pvVjJIdUlYOVhSdFNZSHNYaTRnSXplOU51YXhPdHE0ZG9JSnBGU2tsaEw4eVVWcVlTbXVTNG1OMzQ2UTF0WllsOVlYSU5PMWg4VFNOTnRMZzFJVDZsSnF3akJSbDFndDhmWlhNUExoNXNDdmZUaGhNcXNrdEtad2NPZnNBaTJhVk93anR2QzVFcEs3dTZDRmlUR2dUNjBzeGsxZ1ZoZVMxaFo2UWkyNkllYTBEc3VEOUNCN2ZjdWpoS1BCenA5SzYvLzd0emxCQW5vYXEzZHBQYmdqNWZkaVNadGU3cDRHK1VEcVlpOUpZSW89


[HOME](https://news.sktelecom.com/en) >


[Insight](https://news.sktelecom.com/en/category/insight)


- [#AI](https://news.sktelecom.com/en/tag/ai)
- [#Enterprise Solution](https://news.sktelecom.com/en/tag/enterprise-solution)
- [#Inside T](https://news.sktelecom.com/en/tag/inside-t)
- [#Live to Cart](https://news.sktelecom.com/en/tag/live-to-cart)
- [#NAB](https://news.sktelecom.com/en/tag/nab)
- [#Zero Curation](https://news.sktelecom.com/en/tag/zero-curation)
