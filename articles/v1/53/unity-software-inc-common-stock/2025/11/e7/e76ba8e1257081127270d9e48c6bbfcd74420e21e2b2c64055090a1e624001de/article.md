---
schema_version: "1.0.0"
document_id: "e76ba8e1257081127270d9e48c6bbfcd74420e21e2b2c64055090a1e624001de"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/how-sega-and-rovio-built-sonic-rumble-for-speed-scale-and-stability"
published_at: "2025-11-05T00:00:00+00:00"
first_seen_at: "2026-08-03T18:06:18.177744+00:00"
fetched_at: "2026-08-05T03:48:44.436222+00:00"
content_hash: "sha256:e08748bebb51aaed128e180a098126f6d7d3e099f60da2bb745d2533ff9b4a39"
---

# How SEGA and Rovio built Sonic Rumble for speed, scale, and stability

In 2023, SEGA acquired Rovio, the creator of the global mobile phenomenon *Angry Birds* . Since then, the two teams have combined SEGA’s legacy of crafting high-quality, visually striking games with Rovio’s deep expertise in mobile gaming to bring the Sonic universe to mobile in a fresh new way. The result is *Sonic Rumble* – a fast-paced, arcade royale where up to 32 players race through chaotic, toy-themed stages in Dr. Eggman’s diabolical Toy World. As they compete to be the last toy standing, players can collect and customize iconic Sonic characters with a wide range of skins and emotes.


The game is now free-to-play on iOS, Android, Google Play Games on PC, and Steam. We spoke with Hiromitsu Suto, programming lead of *Sonic Rumble* , to learn how the team tackled[cross-platform](https://unity.com/solutions/multiplatform) optimization, high-speed gameplay at scale, and the challenges of supporting live service content across multiple devices.


This content is hosted by a third party provider that does not allow video views without acceptance of Targeting Cookies. Please set your cookie preferences for Targeting Cookies to yes if you wish to view videos from these providers.


**What is the main post-launch goal for the team?**


**Hiromitsu Suto** : Our main goal following release is to deliver a smooth, global rollout and ensure the game lives up to the expectations of our passionate Sonic community. The team is working around the clock to make that happen.


We’re also committed to keeping the experience fresh and exciting. Players can look forward to a steady flow of new characters, game modes, and themed levels – along with some surprising crossovers. Whether you’re a lifelong Sonic fan or just jumping in for the first time, we want *Sonic Rumble* to feel like a thrilling, ever-evolving arcade playground.


In-Editor screenshot showing all 32 characters and their footprint effects being rendered simultaneously


**What was the biggest technical challenge in development?**


**HS:** Supporting multiple platforms globally posed one of our most significant technical challenges. Achieving smooth performance across both mobile and PC required extensive optimization and rigorous testing.


On mobile, we prioritized lightweight performance to support a broad spectrum of hardware, including low-end devices. On PC, our focus shifted toward high-fidelity visuals and low-latency responsiveness. Balancing these needs required iterative testing and fine-tuning across platforms.


We conducted thorough cross-device verification, including performance profiling, rendering consistency checks, and stress testing. Performance bottlenecks were identified – such as during scene transitions or network-heavy operations – and we implemented targeted optimizations to address them.


This helped us maintain 30 fps on devices considered low-spec in the Japanese market and ensured stable operation on devices with 3 GB of memory.


In-Editor screenshot showing stage collisions and high-speed gimmick splines being rendered


**What performance-related issues did the team encounter, if any?**


**HS:** Bringing Sonic-style speed and gameplay to a 32-player environment – with enemies, hazards, and interactive elements – required extensive performance optimization.


To ensure smooth gameplay on mobile, we implemented a wide range of techniques. Beyond adjusting visual quality to reduce rendering load, we used distance-based culling not just for graphics, but also to lighten the load on animations, physics, and online systems. This allowed us to preserve a rich gameplay experience without sacrificing performance.


Since players are constantly moving at high speeds, we also developed a predictive system that estimates their current position while accounting for latency – striking a careful balance between network load and onscreen accuracy.


In-Editor screenshot showing stage collisions and high-speed gimmick splines being rendered, along with Rings and items


**How was the Universal Render Pipeline (URP) instrumental in helping the team scale assets?**


**HS:** During development, we encountered issues with the size of our large stage assets. After testing both static batching and[URP](https://unity.com/features/srp/universal-render-pipeline) ’s SRP Batcher – and seeing no difference in performance – we decided to discontinue static batching. This decision significantly reduced asset size and memory usage, which was especially important for mobile scalability.


**What was VFX Graph used for?**


**HS:**[VFX Graph](https://unity.com/features/visual-effect-graph) allowed us to prototype Ring effects rapidly, letting us visualize different Ring burst patterns and timing behaviors without writing custom shaders. However, some mid-range mobile devices couldn’t handle the GPU load during high-Ring count scenes, leading us to seek a more performant solution.


In the end, we transitioned to manual indirect rendering for better performance across the board, but VFX Graph remained a valuable part of our early development toolkit.


In-Editor screenshot showing a VFX Graph setup for prototyping


**How did Shader Graph help the team’s technical artists?**


**HS:** Since[Shader Graph](https://unity.com/features/shader-graph) allowed our technical artists to build and edit shaders visually, we avoided hand-coded shader conflicts during Unity version upgrades. This reduced upgrade-related regressions and helped us maintain shader consistency across platforms with minimal engineering overhead.


In-Editor screenshot of a shader graph created by the artist


**How did Probuilder improve team collaboration?**


**HS:** Our planners used ProBuilder to quickly prototype and test stage layouts without needing direct involvement from artists or programmers. This streamlined collaboration across disciplines and made iteration cycles significantly faster.


In-Editor screenshot showing level design work using ProBuilder


**How was the Unity Profiler helpful in supporting optimization efforts?**


**HS:** Since we regularly update elements like stages, UI, and effects to improve the player experience, optimization happened alongside ongoing development. Using the[Unity Profiler](https://unity.com/features/profiling) , we quickly identified performance spikes in real-time builds, pinpointed issues, reduced memory allocations, and optimized canvas structures. Over time, this led to steady and measurable improvements in performance.


**What other player engagement strategies is the team preparing?**


**HS:** Following the global launch, we are implementing seasonal events to spotlight iconic Sonic characters and introduce exciting collaborations with other popular IPs.


We also plan to reintroduce select events and skins from the pre-launch phase in a balanced and thoughtful way, giving more players access to them without undermining early adopters.


Finally, we’re excited to launch a Creator Program that will empower content creators and foster community-driven content. By supporting creators around the world, we aim to build a vibrant ecosystem around *Sonic Rumble* that goes beyond the game itself.


Through all of these initiatives, our goal is to deliver an experience that Sonic fans – new and old – can enjoy for years to come.


In-Editor screenshot showing a large number of rings, gimmicks, and characters being rendered


**Is there anything the team would have done differently to support post-launch content?**


**HS:** It’s less about maximizing content and more about lessons learned. One thing we do regret is relying so heavily on physics systems early on. While they added dynamic interactions, they also increased processing load in ways that made later optimization more difficult. If we could go back, we might approach that balance differently.


**What advice would you give a team looking to scale content effectively?**


**HS:** To manage a high volume of content, it’s critical to build a robust and parallel production pipeline – one where planners, designers, and programmers can work simultaneously and iterate quickly from idea to completion.


Establishing and maintaining this kind of workflow early on is key to long-term scalability. In today’s gaming landscape, where players expect regular updates and fresh experiences, it also takes real creative drive. A team that’s passionate about delivering unique content will always find ways to keep the experience engaging.


As *Sonic Rumble* gears up for its global launch, the team continues to iterate, optimize, and expand. With a strong foundation, cross-platform reach, and a roadmap full of events and creator-driven content, the game promises to be more than just an arcade royale – it’s a celebration of speed, style, and the Sonic fandom.


***To read more about projects made with Unity, visit the[Resources page](https://unity.com/resources) .***
