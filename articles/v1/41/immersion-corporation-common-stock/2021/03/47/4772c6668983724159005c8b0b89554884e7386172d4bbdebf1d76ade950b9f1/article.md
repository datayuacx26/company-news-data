---
schema_version: "1.0.0"
document_id: "4772c6668983724159005c8b0b89554884e7386172d4bbdebf1d76ade950b9f1"
company_key: "immersion-corporation-common-stock"
company: "Immersion Corporation"
source_id: "immersion-corporation-common-stock-rss-474c38a6ce60"
canonical_url: "https://www.immersion.com/zh/experience-driven-haptics-a-vision-for-the-future-of-gaming-and-vr/"
published_at: "2021-03-08T23:27:11+00:00"
first_seen_at: "2026-07-20T23:19:25.565813+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:b0a33aba74efee2c89a0a72059b5f67f8ea3434330a366e733be15a05ef5e61d"
---

# Experience-Driven Haptics – A Vision for the Future of Gaming and VR

**“Content is King”** should be tattooed on everyone’s forehead.


by Chris Ullrich, CTO of Immersion


In a blog post, I wrote last October titled ‘[Beyond the PlayStation 5’](https://www.immersion.com/beyond-the-playstation-5/) , I tried to paint a picture of a rich and vibrant ecosystem of haptic experiences, with the new DualSense™ controller as the first in a series of haptic gaming innovations. Since then, the dramatic success of the PS5 and the rapid adoption of the new haptic gaming paradigm embodied in the DualSense controllers was surprising even to me. Given this trajectory, it’s now a good time to celebrate success and to remind ourselves of what it is going to take to truly make DualSense the first member of the new gaming paradigm.


As a long-time participant in the creation of haptic technologies, I always find it instructive to look for examples and lessons that can be used to point the way forward to a vibrant haptic ecosystem for gaming and VR.


---


**The Lessons of Dolby Atmos™**


The digitization of the sense of touch is at a critical juncture, not unlike where immersive digital audio was a decade ago. Dolby has been able to move the immersive audio market meaningfully in gaming, and the haptics community should take note of the key lessons that were learned during the development and deployment of Atmos:


**Content is King** – This should be tattooed on everyone’s forehead. In the domain of immersive audio, Dolby invests heavily in directly supporting content creators (e.g., studios) to ensure that the creation of immersive audio experiences is as easy as possible for content creators. This means the creation of reference tools and technologies that can be used by game toolmakers to enable Atmos in their products. Rich haptic experiences do not stand on their own. No user desires a ‘rich haptic experience’ – rather, users want immersive and engaging content experiences. Haptics can play a critical *supporting* *role* in an experience.


**Device Independence** – It is rare that a sustainable ecosystem can exist if content needs to be created in a device-dependent way. In the audio domain, end-user playback systems evolved from 2-channel stereo to 4-channel, 5.1, 7.1, 11.2, etc. The explosion of endpoints meant that content creators now needed to worry about remastering their experiences for each combination, develop custom sound engines, or author for the lowest common denominator. This is where technologically mature coding formats such as Dolby Atmos™ really shine – by flipping the paradigm and enabling audio designers to only design and encode their creative intent. The Atmos rendering system then handles the task of synthesizing the best possible audio signals for the specific end-user speaker configuration – everyone wins!


Reflecting on this precedent, gaming haptics still has a long way to go: the creation tool problem is unsolved, and existing haptic coding formats are largely device-specific.


**The Lessons of Core Haptics®**


In 2016, Apple introduced the iPhone 6s, which was the first device to feature Taptic Engine technology. Compared to other mobile phones, the investment that Apple made (and continues to make) in high-quality haptic feedback reinforces a core belief in the value of this modality for the iPhone UX.


In 2019, Apple released the[Core Haptics API framework for the iPhone](https://developer.apple.com/videos/play/wwdc2019/520/) . This framework embodied a key design principle – Design once, in a device-independent way. Apple realized that developers didn’t want to concern themselves with tuning haptic feedback across the different iPhone Taptic Engines (there are several) because this would be costly and the development for haptics is a very specialized design skill.


Apple solved this problem by introducing the AHAP (Apple Haptic and Audio Pattern) coded representation. This is a JSON-based, human-readable, effect description language that allows developers to express their creative desires. For example: ‘ramp up followed by 3 sharp pops’ is directly representable in AHAP. In this way, Apple has enabled a kind of device independence and future-proofed its haptic system for developers. Specifically, it has allowed haptic effects to be redeployed onto DualShock game controllers with no change.


**The Lessons of TouchSense®**


Of course, Immersion has direct experience trying to solve these problems for haptic gaming peripherals. Immersion’s TouchSense family of technologies is built on the basic premise that the design and playback of haptics need to be decoupled. Although developed much earlier, the approach of TouchSense did anticipate the key lessons of Atmos and Core Haptics: it does represent a technological offering that solves an ecosystem-wide need without creating a product-specific walled-garden.


The key lesson from TouchSense is that it is simply not possible for a technology stack to decouple design and playback without broad industry engagement and proactive sharing of best practices. The core know-how that is embodied in TouchSense needed to be seeded, through partner products, widely across the ecosystem: as content creation tools, as coding standards, and as playback SDKs and firmware. This know-how is the product of more than 25 years of single-minded focus and investment in bringing haptics to gaming and VR. Immersion was a key innovator in force-feedback joysticks, steering wheels, DualShock, DualSense, and nearly every other major haptic gaming product. This realization is one of the principal motivations behind Immersion’s recent investment in creating standards with[ISO/MPEG, ATSC, IEEE, and others](https://www.electronicdesign.com/technologies/analog/article/21136731/immersion-a-standard-for-hapticsenabled-media-files) and in the creation of an industry group,[Haptics Industry Forum](http://www.hapticsif.org/) .


---


#### **What is needed in Gaming/XR**


Gaming haptics now finds itself in the same place quadrophonic sound was in the 1970s: content creation depends upon specific playback devices. Most existing gaming content was previously created for DualShock or single-channel bi-manual XR controllers. New content created for the PS5 is awesome, but it requires specialized tools that specifically target DualSense hardware to make. As long as this is the landscape, the vision of a rich haptic ecosystem I painted in ‘Beyond the PS5’ will not come to pass. The path forward for the ecosystem is plain. However, given the lessons from Immersion TouchSense, Dolby Atmos, and Apple’s Core Haptics, there is a need for device-independent, experientially-focused content creation, coding, and playback technology.


**Content creation** : Haptics often suffers from a lack of tactile imagination. It’s hard to describe rich haptic experiences with consistent terminology, and there is no universal reference device/platform. This means that high-quality haptics need to be experienced first-hand by creatives to be understood and used as a source of inspiration. There is no consensus on what a reference device might be for haptic gaming. We need to empower the creative community with tools and reference hardware to enable the exploration of tactile experience in the context of their larger experiential goals. Perhaps more importantly, content creators should be allowed to focus on the tactile experience they want to create – not on the specific limitations to a particular piece of haptic hardware.


**Device-independent coded representation** : Game and XR developers need to focus on the haptic experience they want to create, not on tuning haptics for specific hardware idiosyncrasies. This creative intent is what should be encoded in haptic game assets, at least before platform deployment. This is a long-term, published goal of the[MPEG Haptics Ad-Hoc Group](https://www.mpegstandards.org/adhoc/) *.* To be successful, this group needs a vibrant commercial ecosystem of toolmakers and game engines to facilitate the evolution of cross-platform structures. With a device-independent coded representation, the market for experience-enhancing peripherals (mice, keyboards, headsets, chairs, etc.) will open up and be empowered with great content from the get-go.


#### What we should expect:


Once we have content creators developing new tactile experiences using a device-independent coded representation, we should expect more innovation in haptic gaming UX and a flourishing haptic hardware ecosystem. If we don’t invest in enabling these two needs, we should expect DualSense and high-quality haptics for gaming to follow the path of quadrophonic audio or to effectively stagnate the gaming ecosystem.


By enabling creatives to focus on the utility of the tactile medium to realize their vision, we should expect to see meaningful innovation in the role of haptics in gaming and VR. Haptic metaphors are normally focused on player immersion – explosions, terrain/surface texture, trigger, etc. With higher-order tooling, new tactile-inspired game-mechanics should emerge along with novel uses of haptics to increase user engagement.


By representing creative intent in a device-independent manner, we should expect to see a dramatic increase in the viability of novel gaming form factors and tactile modalities. Current multi-peripheral haptic systems are hamstrung because the haptic sensations must be derived from audio – which is a poor substitute for direct content creation. With explicit creative intent that is device-independent, it’s easy to imagine a rich ecosystem of peripherals, vests, keyboards, gaming chairs, mice, etc., that can meaningfully enrich the player experience.


#### Immersion’s Role


Immersion has been a key driver of haptic technologies in gaming since 1993 and, with the PS5, expects to continue to drive these technologies for the foreseeable future. We have a unique role in the gaming ecosystem. We do not develop or sell consumer-facing tools or hardware. Instead, we heavily invest in the innovation of core technologies to enable tools and hardware. In fact, Immersion is one of the only companies in the gaming ecosystem that can facilitate this vision beyond the PS5, primarily because our business model depends on enabling all parts of the ecosystem. Our key investments in the Haptics Industry Forum, MPEG, and other industry bodies are designed to facilitate making this vision a reality. We will continue building on all the great work by Dolby, Apple, Razer, Lofelt, and many others to realize this vision.


---
