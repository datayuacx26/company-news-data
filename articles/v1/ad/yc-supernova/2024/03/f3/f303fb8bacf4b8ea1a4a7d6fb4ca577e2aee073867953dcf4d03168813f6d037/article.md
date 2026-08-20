---
schema_version: "1.0.0"
document_id: "f303fb8bacf4b8ea1a4a7d6fb4ca577e2aee073867953dcf4d03168813f6d037"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/behind-the-scenes-the-technical-evolution-of-supernova-2-0-from-flutter-to-react"
published_at: "2024-03-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:aeb0daa59469de55cb5a24854207ae58841c49a4ca6aa727cafce0597808489b"
---

# Behind the Scenes: The Technical Evolution of Supernova 2.0 from Flutter to React

Recently, we launched[Supernova 2.0](https://www.supernova.io/blog/introducing-supernova-2-0-the-future-of-product-development-starts-now) , our biggest and boldest update yet. The update introduced some exciting new features and an all-new look and feel. All of that was made possible by our engineering team and their decision to move Supernova from Flutter to React. we sat down with[Artem Ufimtcev](https://www.linkedin.com/in/artem-ufimtcev-29b497a9/) , our CTO;[Miro Koczka](https://www.linkedin.com/in/miro-koczka-2a50b599/) , Engineering Manager; and[Seva Krasnov](https://www.linkedin.com/in/vsevolod-krasnov/) , Product Engineer, for a behind-the-scenes glimpse into the transition and its implications for our users.


## Catalyst for change


Moving your whole product to a new tech stack is a challenging feat, but before we can get into the how and what changed, we first need to understand the why. Supernova was initially built over the years using Flutter, but as the product and company grew, a need for change started to develop.


**Can you share why the team decided to migrate away from Flutter?**


**Miro:** From our perspective, there were several reasons to move away from Flutter. Primarily, Google's support for web technologies wasn't strong enough. There were also challenges with hiring for Flutter, integration issues, and limitations in web functionalities, like not being able to select text on web pages.


**Seva:** Exactly. The support for Flutter Web was always lagging behind mobile, and the addition of desktop support diluted their efforts even more.


## Making the move to React


After deciding that the product needed to be moved from Flutter, the question then became: to where? That didn't last long, however, as it quickly became apparent React was the obvious choice.


**Was React the obvious choice to migrate to or were there other options on the table?**


**Miro:** We chose React mainly because of its scalability and the large developer community. React seemed like the only choice given the direction in which the market was heading.


An all new look and feel in React.


### Challenges along the way


Despite it being the obvious choice, moving to React still presented many challenges the team had to overcome to rebuild the product in record time.


**What were the main challenges you encountered during the move?**


**Miro:** The main challenge was balancing everything. We had to hire for a technology we hadn't used before, allow our Flutter engineers to learn React, and decide on the architecture and under-the-hood technology choices, all while supporting the existing Flutter project.


There was also a pressing time constraint. With a majority of engineering resources dedicated to rebuilding the product, the team needed to deliver it as fast as possible.


**Did you have to compromise on anything in order to get it delivered as fast as possible?**


**Artem:** We made compromises, but the decision-making framework was centered around whether we could quickly implement a feature after release. Some minor features were omitted for the sake of speeding up the release, but we're adding them back now.


## A faster and more responsive Supernova


Ten months of hard work culminated in the release of the new Supernova app, which boasted significant performance improvements, a testament to the power of React.


**A key area of focus with the move was performance; how did it differ now that you've finished the React app?**


**Artem** : Most of the performance gains that we're seeing right now are almost purely from the platform change to React. We're still planning on working to improve it with more and more optimizations.


The team reported a smoother development experience, with fewer complex bugs and a more stable platform for future enhancements. The React app's startup time was notably faster, enhancing the overall user experience.


Side-by-side comparison of loading times between Flutter and React apps.


**Seva** : Once, a few months ago, I had to jump back to the Flutter app. It just felt like an ‘uncanny valley.’ It felt weird, like the interactions, animations, and just the scrolling felt off, not like every other application. Most importantly, the startup time for the application has skyrocketed in the React app.


### Going for Supernova 2.0


With the unique set of challenges that were present, the team wanted to do more than a direct 1:1 rebuild. The main idea behind the rebuild was to enable Supernova to grow in ways that weren't possible before.


**How did the idea for making it a 2.0 version of Supernova come about?**


**Artem:** From the start, we aimed to end up with a product that was not only rewritten using a different technology but was significantly better. The new technology helped us introduce new features like multiplayer functionality and an updated architecture for the documentation editor.


Design systems are all about collaboration, and multiplayer editing has always been one of the ambitious plans the team wanted to implement for Supernova 2.0, but would be significantly more complicated using Flutter.


**Multiplayer documentation is the biggest new feature; what makes it so special?**


**Miro:** I think we've built one of the most complex editors in our space. Our editor combines the real-time capabilities of Google Docs with the block setup of Notion but goes further by allowing blocks to be fully configurable. It was built using[TipTap](https://tiptap.dev/) library for the core part of the editor and the multiplayer with[YJS](https://yjs.dev/) +[Liveblocks](https://liveblocks.io/) . This level of customization and real-time functionality is unique in the industry.


Supernova's multi-player documentation in action.


## Plans for the future of Supernova


While the immediate changes are all exciting, the real benefit of moving to React is what the team can do in the future.


**What excites you the most about the future, now that you're equipped with React?**


**Miro** : The shift to React significantly broadens our horizons, especially in documentation technology. Leveraging React's vast ecosystem, we've adopted libraries like TipTap, YJS, and Liveblocks, which saved us countless development hours. This advantage allows us to focus on innovation rather than reinventing the wheel, offering us the flexibility to build and improve with components and tools readily available in the React community. The transition not only streamlines our development process but also enhances our product's capabilities, setting the stage for future advancements and a richer user experience.


**Artem** : I'm excited about the possibility of making the feedback loop between writing and publishing documentation shorter. Additionally, expanding real-time collaboration features across the product is something we're looking forward to.


Apart from the documentation, we started to share some code between the back and front end, which wasn't possible before. This helps us keep things in sync and build a stable product. While that doesn't translate to something specific right now, it's a big opportunity for us in the future.


---


There are many exciting things still to come for Supernova in 2024 and beyond, which the team wasn't at liberty to talk about just yet. We hope you enjoyed this glimpse behind the scenes into the engine core of Supernova. For those of you who still haven't checked out[Supernova 2.0](https://www.supernova.io/blog/introducing-supernova-2-0-the-future-of-product-development-starts-now) , you can[get started on your design system journey for free](http://app.supernova.io/signup) . Join our[Slack](https://requests.supernova.io/) community and stay tuned to[X](https://twitter.com/supernova_io) for all the big updates coming in 2024.
