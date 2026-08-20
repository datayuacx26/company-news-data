---
schema_version: "1.0.0"
document_id: "cb87fef3e4558276d7ce1ce31916cf7282696318a3e210892189e7cfbd9ff553"
company_key: "yc-mentra"
company: "Mentra"
source_id: "yc-mentra-atom-1275414bf67e"
canonical_url: "https://mentraglass.myshopify.com/blogs/blog/augmentedchords-goes-1-on-hacker-news-sheet-music-app-build-on-mentraos"
published_at: "2025-05-06T03:56:00+00:00"
first_seen_at: "2026-07-27T03:44:49.649440+00:00"
fetched_at: "2026-08-20T00:50:23.357001+00:00"
content_hash: "sha256:2e6fef1fa77652d9845f1d7cc47d5a4bd8e645fcf09428eed1ce336554b2e277"
---

# AugmentedChords Goes #1 on Hacker News - Sheet Music App build on MentraOS

Kevin Lin came to our first MentraOS hackathon and built a smart glasses app that overlays sheet music on his vision while he plays piano. Then he made a video post about it and went #1 on Hacker News on May 3. Kevin is a legend in the MentraOS community and has placed at every Mentra hackathon.


Here’s the Hacker News post:[https://news.ycombinator.com/item?id=43906442](https://news.ycombinator.com/item?id=43906442)


Here’s Kevin’s words, verbatim:


*Hi everyone, my name is Kevin Lin, and this is a Show HN for my sheet music smart glasses project. My video was on the front page on Friday: https://news.ycombinator.com/item?id=43876243, but dang said we should do a Show HN as well, so here goes!*


*I’ve wanted to put sheet music into smart glasses for a long time, but the perfect opportunity to execute came in mid-February, when Mentra (YC W25) tweeted about a smart glasses hackathon they were hosting - winners would get to take home a pair. I went, had a blast making a bunch of music-related apps with my teammate, and we won, so I got to take them home, refine the project, and make a pretty cool video about it (https://www.youtube.com/watch?v=j36u2i7PKKE).*


*The glasses are Even Realities G1s. They look normal, but they have two microphones, a screen in each lens, and can be even made with a prescription. Every person I’ve met who tried them on was surprised at how good the display is, and the video recordings of them unfortunately don’t do them justice.*


*The software runs on AugmentOS, which is Mentra’s smart glasses operating system that works on various 3rd-party smart glasses, including the G1s. All I had to do to make an app was write and run a typescript file using the AugmentOS SDK. This gives you the voice transcription and raw audio as input, and text or bitmaps available as output to the screens, everything else is completely abstracted away. Your glasses communicate with an AugmentOS app, and then the app communicates with your typescript service.*


*The only hard part was creating a Python script to turn sheet music (MusicXML format) into small, optimized bitmaps to display on the screens. To start, the existing landscape of music-related Python libraries is pretty poorly documented and I ran into multiple never-before-seen error messages. Downscaling to the small size of the glasses screens also meant that stems and staff lines were disappearing, so I thought to use morphological dilation to emphasize those without making the notes unintelligible. The final pipeline was MusicXML -> music21 library to render chunks of bars to png -> dilate with opencv- > downscale -> convert to bitmap with Pillow -> optimize bitmaps with imagemagick. This is far from the best code I’ve ever written, but the LLMs attempt at this whole task was abysmal and my years of Python experience really got to shine here. The code is on GitHub: https://github.com/kevinlinxc/AugmentedChords.*


*Putting it together, my typescript service serves these bitmaps locally when requested. I put together a UI where I can navigate menus and sheet music with voice commands (e.g. show catalog, next, select, start, exit, pause) and then I connected foot pedals to my laptop. Because of bitmap sending latency (~3s right now, but future glasses will do better), using foot pedals to turn the bars while playing wasn’t viable, so I instead had one of my pedals toggle autoscrolling, and the other two pedals sped up/temporarily paused the scrolling.*


*After lots of adjustments, I was able to play a full song using just the glasses! It took many takes and there was definitely lots of room for improvement. For example: - Bitmap sending is pretty slow, which is why using the foot pedals to turn bars wasn’t viable. - The resolution is pretty small, I would love to put more bars in at once so I can flip less frequently. - Since foot pedals aren’t portable, it would be cool to have a mode where the audio dictates when the sheet music changes. I tried implementing that with FFT but it was often wrong and more effort is needed. Head tilt controls would be cool too, because full manual control is a hard requirement for practicing.*


*All of these pain points are being targeted by Mentra and other companies competing in the space, and so I’m super excited to see the next generation! Also, feel free to ask me anything!*


Awesome work building cool apps on MentraOS, Kevin!
