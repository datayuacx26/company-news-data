---
schema_version: "1.0.0"
document_id: "6aeffc9a2e28d165481e21e9960454b46b2f5362e639e427944c473747a02eb3"
company_key: "immersion-corporation-common-stock"
company: "Immersion Corporation"
source_id: "immersion-corporation-common-stock-rss-474c38a6ce60"
canonical_url: "https://www.immersion.com/zh/the-haptic-stack-design-layer/"
published_at: "2020-07-14T23:14:33+00:00"
first_seen_at: "2026-07-20T23:19:25.565813+00:00"
fetched_at: "2026-07-28T22:26:46.047008+00:00"
content_hash: "sha256:606f501c7cee9627b7cbf41b0682abcc6a99e6ef7ef187702b9e5f3e5edff67e"
---

# The Haptic Stack – Design Layer

By Chris Ullrich, CTO at Immersion


Over the last few months, I introduced the haptic stack – a conceptual framework that we use at Immersion to think about the key technology components of an engaging haptic experience. As a quick refresher, the stack consists of three key layers: design, software (see[this post](https://www.immersion.com/the-haptic-stack-software-layer/) ), and hardware (see[this post](https://www.immersion.com/the-haptic-stack-hardware-layer/) ). These layers work together in a highly interdependent way to create a tactile experience that is in harmony with the overall UX of a product or experience. Product designers need to think carefully about the trade-offs in all three layers when designing a product to deliver a valuable and delightful end-user experience.


This month we’re going to delve into the top layer – the design layer. The design layer brings software and hardware together into a coherent end-user experience. This is arguably the most important layer from an overall experience perspective, but what’s possible here can be constrained by choices made at the software and hardware layers.


## What is the Design Layer?


The design layer is where everything comes together to create a complete haptic experience. It requires a deep knowledge of various design approaches, haptic effects, and interaction designs that have proven successful. At Immersion, we normally also include tooling and reference systems at this layer as well because these tangible artifacts embody best-in-class design approaches to haptics.


In this blog post, I review the design framework and principles that Immersion uses when it develops haptic experiences, in all markets. I also talk about the enabling tools that can be used to create these effects.


## Principles and Know-How


At Immersion, we have evolved a design framework that captures every haptic use case that we’ve encountered (across automotive, gaming, mobile, and other markets). This framework shown below helps us think about the role of haptics in a specific product or experience.


These elements are not all-inclusive, but do cover the vast majority of uses; they help us define what the primary goal of a haptic experience should be. With a clear understanding of the goals of haptics in an experience, it becomes much easier to make the right design decisions (as well as software and hardware decisions). We discuss each of these design goals below.


### Attention Shift


An attention shift haptic effect is intentionally incongruous with other elements of the experience (such as audio/video). Attention shifting effects are bold and unmistakable in their character and design. This is similar to choosing a pop of color as an ‘accent’ for a house painting project. Adding a pattern or texture can also help users understand what the goal or meaning of the attention shift is.


### Delightful Moments


This type of haptic experience has the goal of generating a smile or positive effect with users and is often associated with some type of surprise. Think of an emoji or sticker with haptics that makes the recipient laugh or feel more connected with the sender. Typically, these effects are tightly coupled with graphics and audio to provide just the right *pop* to an overall experience.


### Information Flow


Haptics are commonly used to support user interactions with an interface on a touchscreen. In this case, haptics convey information to the user about the state of the UI. This can include confirmation of a button press, end-of-list, or other UI state change. This principle can also be helpful in VR experiences to help users understand what they are selecting or pointing at. Typically, these effects are sharp and precise but can also be skeuomorphic (an imitation of a real action, like a button click).


### Sensory Immersion


In gaming and VR experiences, adding tactile effects that result from user interactions with the virtual world is a known way to dramatically increase a sense of immersion. This technique has been used in commercial gaming platforms for more than two decades and is still one of the most effective uses of haptics. It requires close synchronization with audio and video and often employs dynamic haptic effects – ones that respond to a user’s gesture.


### Hedonics


Hedonics is the generation of an emotional response in users. Haptic feedback is capable of facilitating both positive and negative emotional responses and the design of effects is a key mechanism for this. For example, effects that have smooth envelopes and are well synchronized with the overall experience are more likely to create positive affect, whereas effects with sharp envelopes are more likely to generate a negative affect.


### Illusion


In this case, the goal is to convince the user that the touch experience is completely realistic. The iPhone 6s is a great example of this: Apple was able to replace a mechanical switch (the Home button) on the phone with a haptic sensation and most users didn’t notice. Creating realistic mechanical switches, surface textures and other real-world tactile sensations is extremely challenging and requires the highest level of haptic engineering; but when successful, it can be utterly convincing to users.


## Haptic Language and Effect Design


A common question asked about haptic effect design is whether there is some kind of haptic language that can communicate information to users. Associating meaning with haptic effects is challenging because there is no grammar or other symbolic construct associated with them. There are very few haptic sensations that are commonly agreed upon, so it is very hard to get users, in general, to be able to extract meaning from a haptic sensation. In addition, the meaning of a haptic sensation is normally very context-dependent. (A great example is in the excellent[Archaeologies of Touch](https://www.amazon.com/dp/B079SJSXDJ) by David Parisi – who describes researchers in the 1950s creating a kind of Morse code with haptic vibrations.) Direct symbolic haptic effects require a significant amount of learning on the part of users. This can be acceptable for professional use cases, but for consumer devices, the level of learning required is usually too great a barrier to be useful.


To try and solve this problem, Immersion proposed a situational-dependent haptic effect coding system for wearables that would provide a much more accessible haptic language based on learning, in-context of a progressively refined set of basic haptic effects.


Other examples include the distinctive effects on the Apple Watch and the ability to associate haptic effects with a ringtone on many mobile devices.


In general, consumer products should have a very small set of haptic effects that convey information, and the meaning of each effect should be as self-evident (and learnable) as possible. More complex effect languages require both learning and context to be effective.


## Creation Tools


At Immersion, we occasionally create our own tools in order to enable our designers to make full use of the haptic capabilities of a product or platform. But good tools require a large investment and are normally very hard to monetize. Instead, our experienced designers have found it extremely effective to use existing audio toolchains.


For mobile and gaming haptics, audio workflows such as ProTools™ or Wwise™ are very well suited for creating effects. Many effects in these experiences are closely tied to audio effects, and haptics can be thought of as an additional track in the mix– similar to an LFE (low-frequency effect) track. With these tools, it is very efficient to enable live monitoring of the haptic tracks, as this greatly increases tuning and iteration during design.


Some of the most impactful haptic experiences reflect or integrate user motion or gesture into the effect itself. These are often considered haptic illusions and a normal audio workflow can be insufficient if only the rasterized signal is available at playback time. In tools such as Wwise, which also have a runtime engine, it is possible to modify the haptic effects continuously during playback by adjusting one or more parameters. This can be effective in convincing users that the tactile dimension of the experience is responding directly to them.


At Immersion, our goal is to enable rich haptic experiences wherever possible. To that end, we are working diligently to promote haptic effect encoding standards that can help content creators to both use standard tools and to address a wide variety of haptic-capable devices. We believe that, together with design competence, this will enable a huge variety of compelling and valuable haptic experiences in many markets.


## Summary


Haptic design is still in its infancy with exciting growth potential ahead. Throughout Immersion’s history, our designers have had to struggle with low-level and limited design tools, often late in a product development cycle. With the emergence of high-quality haptics in the iPhone, certain Android devices, and the upcoming PS5 controller, we see a bright future for haptic experience design and an opportunity for savvy practitioners to make real, tangible differences in the delight that users have in their product experiences.


Related articles:


[The Haptic Stack](https://www.immersion.com/the-haptic-stack/)


[The Software Layer](https://www.immersion.com/the-haptic-stack-software-layer/)


[The Hardware Layer](https://www.immersion.com/the-haptic-stack-hardware-layer/)
