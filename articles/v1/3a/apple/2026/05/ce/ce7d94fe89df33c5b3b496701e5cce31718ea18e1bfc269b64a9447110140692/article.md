---
schema_version: "1.0.0"
document_id: "ce7d94fe89df33c5b3b496701e5cce31718ea18e1bfc269b64a9447110140692"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-b03a533acb22"
canonical_url: "https://www.apple.com/newsroom/2026/05/ai-meets-accessibility-in-this-years-swift-student-challenge/"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-21T07:16:21.842538+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:cbf959aadd74b25b9fbad24f48b371ae81a3a93ca1aa40a726140b28a9ba5811"
---

# AI meets accessibility in this year’s Swift Student Challenge

Text of this article


May 7, 2026


UPDATE


AI meets accessibility in this year’s :br(s): Swift Student Challenge


Receiving real-time feedback while giving a presentation. Escaping a flood zone in Accra. Playing the viola, without the physical instrument. Drawing on iPad without worry of tremors. These are just four of the solutions that this year’s Swift Student Challenge Distinguished Winners created with their winning app playgrounds.


The annual Swift Student Challenge invites students from across the globe to bring their ideas to life through original app playgrounds built with Apple’s easy-to-learn Swift coding language. This year’s 350 winning submissions represent 37 countries and regions, and showcase a wide range of technologies.


“The breadth of creativity we see in the Swift Student Challenge never ceases to amaze us,” says Susan Prescott, Apple’s vice president of Worldwide Developer Relations. “This year’s winners found remarkable ways to harness the power of Apple platforms, Swift, and AI tools to build app playgrounds that are as technically impressive as they are meaningful. We’re incredibly proud to support their journey and can’t wait to see what they create next.”


Fifty Distinguished Winners have been invited to attend the Worldwide Developers Conference (WWDC) at Apple Park in June, where they’ll take part in a curated three-day experience. Throughout the week, the students will have the opportunity to watch the Keynote live, learn from Apple experts and engineers, and participate in hands-on labs.


Many of this year’s winners took inspiration from their communities — or even from conversations at their kitchen tables — to engineer impressive apps with accessibility at their core. Below, Distinguished Winners Gayatri Goundadkar, Anton Baranov, Karen-Happuch Peprah Henneh, and Yoonjae Joung delve into their app playgrounds and the real-world problems they’re aiming to solve, demonstrating the power of app development to drive lasting change.


**Making Art More Accessible with Steady Hands**


Gayatri Goundadkar, 20, grew up drawing and painting with her grandmother in Pune, India. The two shared a passion for Warli painting, a centuries-old art form known for its use of basic geometric shapes. But as Goundadkar’s grandmother aged, her hands started shaking and she was unable to partake in her daily practice. That loss stayed with Goundadkar and inspired her to build Steady Hands, an app playground that uses Apple Pencil stabilization to support individuals with tremors in creating art.


“My main audience is older adults,” explains Goundadkar, a third-year computer science student at Maharashtra Institute of Technology World Peace University, where she’s involved in an app development program. “Especially in India, technology can feel intimidating for that generation, so I made every decision with that in mind. The interface had to feel calm, not clinical. I didn’t want anyone to open the app and feel lost or overwhelmed. I wanted them to feel like it was made for them.”


In order for the app to allow users to draw freely, Goundadkar had to understand tremors and how they affect interaction with the touchscreen on iPad. Inspired in part by Apple’s accessibility features such as Touch Accommodations, she got started by learning SwiftUI concepts, using Anthropic’s Claude to help unpack lessons on topics like how PencilKit handles stroke data. And to characterize a user’s tremor, she built a tool that analyzes raw motion data from iPad and Apple Pencil. It captures hand movements and applies signal processing techniques to identify the frequency and intensity of a user’s tremor.


“When a person draws, my app uses Apple’s PencilKit and Accelerate frameworks to analyze stroke data and recognize tremors. It detects what is intentional and what is not, and removes the tremor component,” she says. “Every drawing is then displayed in a personal 3D museum, because I wanted them to feel like artists, not patients. When users saw the stabilization working, they felt more confident.”


**Perfecting Presentations with Pitch Coach**


Anton Baranov, 22, was sitting at his family’s kitchen table in Frankfurt, Germany, when his mother, a linguistics and literature professor, made a comment that struck him.


“She said her students are really talented, but sometimes when they present something, they just freeze. They lose their words. They’re slouching. They can’t share their ideas,” says Baranov, a computer science student at the University of Applied Sciences Mittelhessen in Germany. It was in that moment that[pitch coach](https://apps.apple.com/us/app/pitch-coach-speak-confident/id6759990855) — an app Baranov describes as “an Apple Intelligence-powered wingman for *Shark Tank* pitches” — was born.


Baranov, who got into programming at 16, used Swift for the first time last August, and in February, he built pitch coach. He brought an early version to his mom’s students and discovered a specific pain point: Students know where they fall flat, but they only realize their mistakes after the fact. “A student told me, ‘I want to be able to catch myself in the act,’” Baranov recalls. “That’s exactly how the real-time feedback and AirPods posture tracking became the core of the app.”


To guide users through overcoming presentation anxiety, Baranov leveraged Apple’s Foundation Models framework to generate personalized, context-aware feedback and summaries after each session, alerting the user to filler words such as “like” or “um.” He also used Claude Agent in Xcode 26 to translate the app into 20 languages, and consulted with friends and colleagues to help identify filler words in other languages.


Baranov released pitch coach on the App Store in early March, and since then, it has amassed more than 6,000 organic downloads. Most of the app’s users employ it for presentation practice, but Baranov mentions some use cases that have made him laugh: practicing rap performances and stand-up comedy routines. “Users define the app, so if they like it for this purpose, they use it for this purpose,” he says.


**Finding Safe Flood Zone Evacuation Paths with Asuo**


Karen-Happuch Peprah Henneh only learned Swift this year. After completing undergraduate degrees in computer science and information technology in her home country of Ghana, Henneh focused on animation since coding opportunities were slim. She learned Figma and HTML5 on the side, and is now working on her master’s in interaction design at the California College of the Arts.


She designed her winning app playground, Asuo, for flood-prone communities. ( *Asuo* means “flowing water” in Twi, a language that’s widely spoken in Ghana.) Asuo provides safe real-time routing to individuals in flood zones, and it’s rooted in lived experience — the fatal floods that hit Accra in 2015, causing a ripple effect of disaster.


“That experience really stayed with me because the whole country was in mourning,” Henneh says. “I decided that if I ever had a chance, it’s going to be the first thing that I would want to work on: Build an app that can calculate rain intensity and uses a pathfinding algorithm informed by historic flood data.”


To create Asuo, Henneh had to not only synthesize all this data, but also ensure that it worked for everyone. “Accessibility was a core consideration from the start, not an afterthought,” she says. “I believe that during a crisis, no one should be left behind because of a disability or limitation.”


The app’s interactive elements have VoiceOver labels and hints, so that users who are blind or have low vision can navigate every screen, and Henneh also built a custom voice alert system using AVSpeechSynthesizer, which users can toggle on with a speaker button.


After designing Asuo’s interface in Figma, Henneh turned to Claude for help in designing the rain simulator on her app’s launch screen, along with implementing the A* pathfinding algorithm. “Because I’m a designer, I don’t really dive into the very technical parts,” she shares. “I rely on AI agents for assistance with those. Something that would have taken me months to do was able to be done in three or four days.”


Through her nonprofit,[Radiance Girl Africa](https://www.radiancegirl.com/) , Henneh has led discussions and workshops at several schools, including the University of Education in Ghana and UniMAC, with the aim of empowering young women to succeed in tech and the arts. “The digital divide is very glaring,” Henneh says. “Many of these people didn’t have access to computers growing up. There are a lot of problems that technology is able to solve, but if people from where I’m from are not the ones designing it, it’s a bit difficult to catch up and learn it. I design for the people in marginalized communities.”


**Democratizing Music Education with LeViola**


When Yoonjae Joung, 21, was packing for his exchange program at New York University, the computer science student couldn’t fit his viola in his suitcase. But after attending a concert at the New York Philharmonic, he began to miss his instrument. That’s when he was inspired to create LeViola, an app playground designed to make learning and playing the viola more accessible.


Though Joung has been coding for a long time — as a teenager in Seoul, South Korea, he made a timer to control electronics in the classroom, and he recently developed an AI companion device for the elderly living alone — he’s new to Swift. “When I came up with the idea of using my hands to play the instrument, and using the camera overlay to help users navigate their own bow pose, I didn’t know where to start,” he says. To familiarize himself with the coding language, Joung used Claude as well as OpenAI’s Codex and Google’s Gemini. He then experimented with Create ML to train his own model before integrating it into his app using Core ML.


While building LeViola, Joung aimed to make the most of Apple’s frameworks for on-device machine learning. “I used them to analyze the joint of the left hand to determine which notes are pressed,” he explains. “To differentiate between strings and a realistic playing experience, I decided to track the angle of the right arm.”


Joung is well aware of the barriers to entry when it comes to learning an instrument. Most instruments are bulky, and lessons can be expensive. “I engage with technology as a tool to connect people,” he says. “This app is only the beginning. I can make this for other instruments, too. People without instruments can now engage in classical music. I want more people to have the opportunity to learn an instrument and enjoy orchestra, and iPhone makes it all possible.”


And though Joung is focusing on LeViola for the time being, he already has in mind another app that fuses his passions for art and technology. “I want to make digital platforms which can connect people in the real world,” he says.


*Apple is proud to champion the next generation of developers, creators, and entrepreneurs through its annual Swift Student Challenge program. Thousands of participants from all over the world have built successful careers, founded businesses, and created organizations focused on democratizing technology and using it to build a better future. Learn more at*[developer.apple.com/swift-student-challenge](https://developer.apple.com/swift-student-challenge/) *.*


Press Contacts


Nehel Cochinwala


Apple


n_cochinwala@apple.com


Apple Media Helpline


media.help@apple.com


Copy text
